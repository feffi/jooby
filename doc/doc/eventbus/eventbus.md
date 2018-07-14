# EventBus

<a href="http://greenrobot.org/eventbus">EventBus</a> is an open-source library for Java using the publisher/subscriber pattern for loose coupling. EventBus enables central communication to decoupled classes with just a few lines of code – simplifying the code, removing dependencies, and speeding up app development.

## exports

* An `EventBus`

## dependency

```xml
<dependency>
 <groupId>org.jooby</groupId>
 <artifactId>jooby-eventbus</artifactId>
 <version>{{version}}</version>
</dependency>
```

## usage

```java
{
  use(new EventBusby()
    .register(new MySubscriber())
  );

  post("/message", req -> {
    EventBus bus = require(EventBus.class);
    MessageEvent event = req.body(MessageEvent.class);
    bus.post(event);
  });
}
```

## guice subscribers

EventBus module supports Guice subscribers:

```java
{
  use(new EventBusby()
    .register(MySubscriber.class)
  );

}
```

Here the ```MySubscriber``` class will be provisioned by Guice.

## custom EventBus

Jooby uses the default EventBus: ```EventBus.getDefault()```, you can provide a custom EventBus at creation time:

```java
{
  use(new EventBusby(() -> {
    return EventBus.builder()
      .logNoSubscriberMessages(false)
      .sendNoSubscriberEvent(false)
      .build();
  }));

}
```

That's all! Happy coding!!
