### 1. SpringBoot MQTT

[출처](https://kdevkr.github.io/spring-boot-integration-mqtt/)  
[참고](https://docs.spring.io/spring-integration/reference/html/mqtt.html)

#### 1) 의존성

- MQTT는 Eclipse Paho MQTT Client 라이브러리 사용

```java
implementation 'org.springframework.boot:spring-boot-starter-integration'
implementation 'org.springframework.integration:spring-integration-mqtt'
```

```java
org.springframework.integration:spring-integration-mqtt:5.3.2.RELEASE
  org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.4
```

#### 2) Inbound Channel Configration

- 토픽 메시지 구독을 위한 채널 구성
- MqttPahoMessageDrivenChannerAdapter 구현체를 통해 구현

- **클라이언트 등록**
  - DefaultMqttPahoClientFactory 사용
  - MqttConnectOptions을 설정한 DefaultMqttPahoClientFactory를 빈으로 등록

```java
@Configuration
public class MqttConfig {
    private static final String MQTT_USERNAME = "username";
    private static final String MQTT_PASSWORD = "password";

    private MqttConnectOptions connectOptions() {
        MqttConnectOptions options = new MqttConnectOptions();
        options.setCleanSession(true);
        options.setUserName(MQTT_USERNAME);
        options.setPassword(MQTT_PASSWORD.toCharArray());
        return options;
    }

    @Bean
    public DefaultMqttPahoClientFactory defaultMqttPahoClientFactory() {
        DefaultMqttPahoClientFactory clientFactory = new DefaultMqttPahoClientFactory();
        clientFactory.setConnectionOptions(connectOptions());
        return clientFactory;
    }
}
```

- MqttPahoMessageDrivenChannelAdapter
  - 메시지 수신을 위한 채널을 구성
  - 메시지를 구독

```java
@Configuration
public class MqttConfig {

    private static final String BROKER_URL = "tcp://localhost:1883";
    private static final String MQTT_CLIENT_ID = MqttAsyncClient.generateClientId();
    private static final String TOPIC_FILTER = "[PROTECT]";

    @Bean
    public MessageChannel mqttInputChannel() {
        return new DirectChannel();
    }

    @Bean
    public MessageProducer inboundChannel() {
        MqttPahoMessageDrivenChannelAdapter adapter =
                new MqttPahoMessageDrivenChannelAdapter(BROKER_URL, MQTT_CLIENT_ID, TOPIC_FILTER);
        adapter.setCompletionTimeout(5000);
        adapter.setConverter(new DefaultPahoMessageConverter());
        adapter.setQos(1);
        adapter.setOutputChannel(mqttInputChannel());
        return adapter;
    }

    @Bean
    @ServiceActivator(inputChannel = "mqttInputChannel")
    public MessageHandler inboundMessageHandler() {
        return message -> {
            String topic = (String) message.getHeaders().get(MqttHeaders.RECEIVED_TOPIC);
            System.out.println("Topic:" + topic);
            System.out.println("Payload" + message.getPayload());
        };
    }
}
```

#### 4) Outound Channel Configuration

- 메시지를 발행하기 위한 채널

- MqttPahoMessageHandler
  - 메시지를 발행하기 위한 채널을 구성

```java
@Configuration
public class MqttConfig {

    private static final String MQTT_CLIENT_ID = MqttAsyncClient.generateClientId();

    @Bean
    public MessageChannel mqttOutboundChannel() {
        return new DirectChannel();
    }

    @Bean
    @ServiceActivator(inputChannel = "mqttOutboundChannel")
    public MessageHandler mqttOutbound(DefaultMqttPahoClientFactory clientFactory) {
        MqttPahoMessageHandler messageHandler =
                new MqttPahoMessageHandler(MQTT_CLIENT_ID, clientFactory);
        messageHandler.setAsync(true);
        messageHandler.setDefaultQos(1);
        return messageHandler;
    }
}
```

- API 를 통해 메시지 발송

```java
@Configuration
public class MqttConfig {

    @MessagingGateway(defaultRequestChannel = "mqttOutboundChannel")
    public interface OutboundGateway {
        void sendToMqtt(String payload, @Header(MqttHeaders.TOPIC) String topic);
    }

}
```

#### 4) MQTT Events

[참고](https://docs.spring.io/spring-integration/reference/html/mqtt.html#mqtt-events)
