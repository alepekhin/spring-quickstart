# spring-quickstart

Это проект, аналогичный https://spring.io/quickstart но с WebFlux и GraphQL

## What you'll build
You will build a classic “Hello World!” endpoint which any GraphQL client can connect to. 

You can even tell it your name, and it will respond in a more friendly way

## Step 1: Start a new Spring Boot project 

Use https://start.spring.io/

Select dependencies

- Spring Reactive Web
- Spring for GraphQL

## Step 2: Add your code

Replace `DemoApplication` code to 
```
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.graphql.data.method.annotation.Argument;
import org.springframework.graphql.data.method.annotation.QueryMapping;
import org.springframework.stereotype.Controller;

import java.util.Optional;

@SpringBootApplication
@Controller
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

	@QueryMapping
	public String hello(@Argument Optional<String> name) {
		return String.format("Hello %s!", name.orElse("World"));
	}

}
```

Добавьте схему GraphQL:

В папке resources/graphql создайте файл `schema.graphqls` с содержимым

```
type Query {
    hello(name: String): String!
}
```

## Step 3: Try it

Let’s build and run the program. Open a command line (or terminal) and navigate to the folder where you have the project files. We can build and run the application by issuing the following command:

MacOS/Linux:

./gradlew bootRun

Windows:

.\gradlew.bat bootRun

You should see some output that looks very similar to this:

```
.   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.1.4)

2023-09-28T10:43:51.758+03:00  INFO 10767 --- [           main] com.example.demo.DemoApplication         : Starting DemoApplication using Java 17.0.6 with PID 10767 (/home/alepekhin/Github/spring-quickstart/build/classes/java/main started by alepekhin in /home/alepekhin/Github/spring-quickstart)
2023-09-28T10:43:51.761+03:00  INFO 10767 --- [           main] com.example.demo.DemoApplication         : No active profile set, falling back to 1 default profile: "default"
2023-09-28T10:43:52.344+03:00  INFO 10767 --- [           main] efaultSchemaResourceGraphQlSourceBuilder : Loaded 1 resource(s) in the GraphQL schema.
2023-09-28T10:43:52.445+03:00  INFO 10767 --- [           main] .b.a.g.r.GraphQlWebFluxAutoConfiguration : GraphQL endpoint HTTP POST /graphql
2023-09-28T10:43:52.605+03:00  INFO 10767 --- [           main] o.s.b.web.embedded.netty.NettyWebServer  : Netty started on port 8080
2023-09-28T10:43:52.610+03:00  INFO 10767 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 1.045 seconds (process running for 1.219)
2023-09-28T11:11:11.119+03:00  WARN 10767 --- [or-http-epoll-3] notprivacysafe.graphql.GraphQL           : Query did not parse : 'query Hello { hello(name:"Amy" }'
2023-09-28T11:11:57.165+03:00  WARN 10767 --- [or-http-epoll-7] notprivacysafe.graphql.GraphQL           : Query did not parse : 'query Hello { hello(name:"xxx" }'
2023-09-28T11:12:20.275+03:00  WARN 10767 --- [or-http-epoll-8] notprivacysafe.graphql.GraphQL           : Query did not parse : 'query Hello { hello(name:xxx }'
2023-09-28T11:12:31.690+03:00  WARN 10767 --- [or-http-epoll-9] notprivacysafe.graphql.GraphQL           : Query did not validate : 'query Hello { hello(name:xxx) }'

```

Заметьте, что используется Netty сервер вместо Tomcat

Теперь откройте еще один терминал там выполните

```
$ curl -X POST -H "Content-Type: application/json" -d '{"query": "query Hello { hello }"}' http://localhost:8080/graphql
получите
{"data":{"hello":"Hello World!"}}


```$ curl -X POST -H "Content-Type: application/json" -d '{"query": "query Hello { hello(name:\"Amy\") }"}' http://localhost:8080/graphql
получите
{"data":{"hello":"Hello Amy!"}}
```

## Что дальше

Пишите бизнес-логику, наращивайте количество енд-пойнтов, меняйте соответсвенно схему

Более полный пример находится тут https://github.com/alepekhin/spring-boot-graphql







