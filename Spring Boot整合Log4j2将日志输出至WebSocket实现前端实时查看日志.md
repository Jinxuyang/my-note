## 需求

需要将后台产生的日志试试通过WebSocket发送，在前端建立连接后可实时查看当前系统产生的日志

## 思路

先进行一番搜索看看有没有现成的轮子

看到一篇博客与我的想法接近，但是他使用的是Logback

https://cloud.tencent.com/developer/article/1096792

在Log4j2的官方文档中给出了不同场景下各个日志框架的性能区别https://logging.apache.org/log4j/2.x/performance.html

秉持能用好的用好的能用快的用快的的观念，我选择Log4j2



对于这个问题我有两个思路

1. 将日志文件写到文件中，监控文件变化，变化时，读取一行交给WebSocket发送
2. 实现一个Appender直接将日志交给WebSocket



总感觉第一种方法不太优雅，需要指定配置文件所在的地址

这里详细说一下第二种方法

## 步骤

### 引入Log4j2依赖

   `spring-boot-starter`包下有一个`spring-boot-starter-logging`内包含了Logback相关的依赖，需要将其排除

![image-20211122122228342](https://raw.githubusercontent.com/Jinxuyang/my-note/master/assets/image-20211122122228342.png)

再引入依赖

![image-20211122122322354](https://raw.githubusercontent.com/Jinxuyang/my-note/master/assets/image-20211122122322354.png)

### 引入WebSocket依赖

​	![image-20211122122357493](https://raw.githubusercontent.com/Jinxuyang/my-note/master/assets/image-20211122122357493.png)

​	什么是WebSocket，WebSocket怎么用等等一系列问题可以看http://www.mydlq.club/article/86/

​	写的非常好，非常全面

​	Spring Boot支持使用 STOMP，我们这里也使用的是STOMP，关于STOMP上面的博客也有提到。

### 配置WebSocket

新建一个WebSocketConfig类用来配置WebSocket的基本信息

```java
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    /**
     * 配置Broker，表明可以在topic域上可以向客户端发送消息
     * 当客户端向服务端发起请求是需要/app前缀
     */
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic");
        config.setApplicationDestinationPrefixes("/app");
    }

   /**
     * 配置WebSocket连接的端点
     */
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/websocket")
                .withSockJS();
    }
}
```

​	到此WebSocket配置完毕

### 实现一个WebSocketAppender将日志记录到WebSocket

如何自己实现appender可以看

https://logging.apache.org/log4j/2.x/manual/extending.html 

https://logging.apache.org/log4j/2.x/manual/plugins.html

官网的两节内容

也可以搜索关键字”Log4j2 插件“进行学习



为什么要自己实现是因为Log4j2本身提供的appender都没有契合这个需求的（貌似）

#### appender实现

这里直接给我我的实现

```java
@Plugin(name = "WebSocketAppender", category = Core.CATEGORY_NAME, elementType = Appender.ELEMENT_TYPE, printObject = true)
public class WebSocketAppender extends AbstractAppender {

    // 一个阻塞队列
    private LoggerQueue loggerQueue  = LoggerQueue.getInstance();

    protected WebSocketAppender(String name,
                                Filter filter,
                                Layout<? extends Serializable> layout,
                                boolean ignoreExceptions,
                                Property[] properties) {
        super(name, filter, layout, ignoreExceptions, properties);
    }

    // TODO：未考虑并发
    // 这个方法就是将日志文件放到哪的具体实现
    // 这里将日志文件转换为字符串后并没有直接给WebSocket而是给一个阻塞队列进行缓冲
    @Override
    public void append(LogEvent event) {
        loggerQueue.push(new String(getLayout().toByteArray(event)));
    }

    // 用来构造这个类
    @PluginFactory
    public static WebSocketAppender createAppender(@PluginAttribute("name") String name,
                                                   @PluginAttribute("ignoreExceptions") boolean ignoreExceptions,
                                                   @PluginElement("Layout") Layout layout,
                                                   @PluginElement("Filters") Filter filter) {


        if (name == null) {
            LOGGER.error("No name provided for WebSocketAppender");
            return null;
        }

        if (layout == null) {
            layout = PatternLayout.createDefaultLayout();
        }
        return new WebSocketAppender(name, filter, layout, ignoreExceptions, Property.EMPTY_ARRAY);
    }
}
```

**我append的方法不够完善，并没有考虑到并发的情况，可能出现许多问题**

#### 阻塞队列的实现

```java
public class LoggerQueue {
    //队列大小
    public static final int QUEUE_MAX_SIZE = Integer.MAX_VALUE;
    private static final LoggerQueue alarmMessageQueue = new LoggerQueue();
    //阻塞队列
    private final BlockingQueue<String> queue = new LinkedBlockingQueue<>(QUEUE_MAX_SIZE);

    public static LoggerQueue getInstance() {
        return alarmMessageQueue;
    }
    
    /**
     * 消息入队
     * @param log
     * @return
     */
    public boolean push(String log) {
        return this.queue.add(log);
    }
    
    /**
     * 消息出队
     * @return
     */
    public String pop() {
        String result = null;
        try {
            result = this.queue.take();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return result;
    }
}
```

#### 日志转发实现

创建一个LogForward用以将队列中的方法转发到WebSocket中

```java
@Component
// 开启定时任务用来测试
@EnableScheduling
public class LogForward {
    protected final Logger logger = LoggerFactory.getLogger(this.getClass());
    // 这个类是Spring提供用来发送消息的类
    @Autowired
    private SimpMessagingTemplate messagingTemplate;
    // 线程池，没有的话可以直接new Thread
    @Autowired
    private ThreadPoolExecutor threadPoolExecutor;

    // 获取阻塞队列的实例
    private final LoggerQueue loggerQueue = LoggerQueue.getInstance();

    // 每5s打印一条日志用以测试
    @Scheduled(cron = "*/5 * * * * *")
    public void ok(){
        logger.info("ok");
    }

    @Bean
    public void pushLogs() {
        threadPoolExecutor.execute(() -> {
            // 从队列中读取日志并发送
            while (true) {
                String message = loggerQueue.pop();
                messagingTemplate.convertAndSend("/topic/log", message);
            }
        });
    }
}
```



### Log4j2.xml配置

这里给我我的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--packages="cn.xxx.xxx.xxx"用来配置自定义插件所在的包-->
<Configuration status="WARN" packages="cn.xxx.xxx.xxx" strict="true">
    <Appenders>
        <!--*********************WebSocket日志***********************-->
        <Appender type="WebSocketAppender" name="webSocketAppender">
            <Layout type="PatternLayout"
                    pattern="%d [%t] %-5level: %msg%n%throwable"/>
            <!--TODO：临时使用字符串匹配关键字过滤，不能保证完全避免问题，而且并不优雅，后期可以考虑别的办法-->
            <!--问题: 在debug级别下，当WebSocket消息发出后会产生一条日志，这条日志会导致WebSocketAppender又发送日志，就导致死循环-->
            <Filters>
                <StringMatchFilter text="Processing MESSAGE destination=" onMatch="DENY" onMismatch="NEUTRAL"/>
                <StringMatchFilter text="Broadcasting to" onMatch="DENY" onMismatch="NEUTRAL"/>
            </Filters>
        </Appender>

        <!--*********************控制台日志***********************-->
        <Appender type="Console" name="consoleAppender" target="SYSTEM_OUT">
            <PatternLayout
                    pattern="%style{%d{ISO8601}}{bright,green} %highlight{%-5level} [%style{%t}{bright,blue}] %style{%C{}}{bright,yellow}: %msg%n%style{%throwable}{red}"
                    disableAnsi="false"
                    noConsoleNoAnsi="false"/>

        </Appender>
    </Appenders>
    
    <Loggers>
        <Root level="debug">
            <AppenderRef ref="consoleAppender"/>
            <AppenderRef ref="webSocketAppender"/>
        </Root>
    </Loggers>
</Configuration>
```

这里有个很坑的点就是

在DEBUG级别下WebSocket发消息会产生一条日志，这条日志又会导致发消息，就形成了循环

这里用了一个StringMatchFilter通过匹配字符串来过滤那两条日志，但这样不优雅，也不完美，但暂时想不到别的办法

**至此，后端部分全部完成**

### 搭建前端测试

这里直接复制了Spring官方文档的代码稍作修改

index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello WebSocket</title>
    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/3.4.1/css/bootstrap.min.css" rel="stylesheet">
    <link href="main.css" rel="stylesheet">
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/sockjs-client/1.4.0/sockjs.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/stomp.js/2.3.3/stomp.min.js"></script>
    <script src="app.js"></script>
</head>
<body>
<noscript><h2 style="color: #ff0000">Seems your browser doesn't support Javascript! Websocket relies on Javascript being
    enabled. Please enable
    Javascript and reload this page!</h2></noscript>
<div id="main-content" class="container">
    <div class="row">
        <div class="col-md-6">
            <form class="form-inline">
                <div class="form-group">
                    <label for="connect">WebSocket connection:</label>
                    <button id="connect" class="btn btn-default" type="submit">Connect</button>
                    <button id="disconnect" class="btn btn-default" type="submit" disabled="disabled">Disconnect
                    </button>
                </div>
            </form>
        </div>
        <div class="col-md-6">
            <form class="form-inline">
                <div class="form-group">
                    <label for="name">What is your name?</label>
                    <input type="text" id="name" class="form-control" placeholder="Your name here...">
                </div>
                <button id="send" class="btn btn-default" type="submit">Send</button>
            </form>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
            <table id="conversation" class="table table-striped">
                <thead>
                <tr>
                    <th>Greetings</th>
                </tr>
                </thead>
                <tbody id="greetings">
                </tbody>
            </table>
        </div>
    </div>
</div>
</body>
</html>
```

app.js

```javascript
var stompClient = null;

function setConnected(connected) {
    $("#connect").prop("disabled", connected);
    $("#disconnect").prop("disabled", !connected);
    if (connected) {
        $("#conversation").show();
    }
    else {
        $("#conversation").hide();
    }
    $("#greetings").html("");
}

function connect() {
    var socket = new SockJS('websocket');
    stompClient = Stomp.over(socket);
    stompClient.connect({}, function (frame) {
        setConnected(true);
        console.log('Connected: ' + frame);
        stompClient.subscribe('/topic/log', function (greeting) {
            console.log(greeting)
            showGreeting(greeting.body);
        });
    });
}

function disconnect() {
    if (stompClient !== null) {
        stompClient.disconnect();
    }
    setConnected(false);
    console.log("Disconnected");
}

function sendName() {
    stompClient.send("/app/hello", {}, JSON.stringify({'ok': $("#name").val()}));
}

function showGreeting(message) {
    $("#greetings").append("<tr><td>" + message + "</td></tr>");
}

$(function () {
    $("form").on('submit', function (e) {
        e.preventDefault();
    });
    $( "#connect" ).click(function() { connect(); });
    $( "#disconnect" ).click(function() { disconnect(); });
    $( "#send" ).click(function() { sendName(); });
});
```

main.css

```css
body {
    background-color: #f5f5f5;
}

#main-content {
    max-width: 940px;
    padding: 2em 3em;
    margin: 0 auto 20px;
    background-color: #fff;
    border: 1px solid #e5e5e5;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
}
```

讲这三个文件保存到resource/static下即可

### 测试

![image-20211122130035879](https://raw.githubusercontent.com/Jinxuyang/my-note/master/assets/image-20211122130035879.png)

点击Connect建立连接，就可以看到日志输出
