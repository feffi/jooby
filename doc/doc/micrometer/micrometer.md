# micrometer

<a href="https://micrometer.io/">Micrometer</a> provides a simple facade over the instrumentation clients for the most popular monitoring systems, allowing you to instrument your JVM-based application code without vendor lock-in. Think SLF4J, but for metrics.

## dependency

```xml
<dependency>
 <groupId>org.jooby</groupId>
 <artifactId>jooby-micrometer</artifactId>
 <version>{{version}}</version>
</dependency>
```

## exposes

* One or more ```MeterRegistry``` 

## usage

```java
{
  use(new Micrometer());

  // Timer example:
  use("*", (req, rsp, chain) -> {
    MeterRegistry registry = require(MeterRegistry.class);
    Timer timer = registry.timer("http.server.requests");
    timer.record(() -> chain.next(req, rsp));
  });
}
```

## monitoring systems

It is possible to attach one or more monitoring system. Here are some examples:

```java
{
  use(new Micrometer()
    .atlas(conf -> {
      return new AtlasMeterRegistry(conf);
    })
    .prometheus(PrometheusMeterRegistry::new)
    // etc...
  );

}
```

Jooby creates a ```MeterRegistryConfig``` for every available monitoring system. You control configuration properties via ```.conf``` file:

application.conf:

```
micrometer {
    atlas {
      uri: "http://localhost:7101/api/v1/publish"
    }
    prometheus {
      step: "PT1M"
    }
  }

```

<a href="https://prometheus.io/">Prometheus</a> expects to scrape or poll individual app instances for metrics. Jooby provides a ready to use prometheus handler:

```java
import org.jooby.micrometer.PrometheusHandler;
{
  use(new Micrometer()
     .prometheus(PrometheusMeterRegistry::new)
  );

  get("/metrics", new PrometheusHandler());

}
```

## timed annotation

Jooby supports the ```io.micrometer.core.annotation.Timed``` annotation for MVC routes:

```java
@Path("/controller")
class Controller {

  @Timed("people.all")
  public People list() {
    ...
  }

}
```

App: 

```java
import org.jooby.micrometer.TimedHandler;
{
  use(new Micrometer());

  use("*", new TimedHandler());
}
```

The ```TimedHandler``` record all the controller methods with ```io.micrometer.core.annotation.Timed``` annotation.

## advanced options

Advanced options are available via ```doWith``` method:

```java
{
  use(new Micrometer()
    .doWith(registry -> {
       //work with registry
    })
  );

}
```

That's all! Happy coding!!
