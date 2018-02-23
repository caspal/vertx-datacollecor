# vertx-datacollector

A framework to collect and post process data from a source. It provides metrics
vertx-datacollector is a virtual table which provides a battle plan to support Pen & Paper Role-playing game players.

## Import

Maven

```
<dependency>
  <groupId>info.pascalkrause</groupId>
  <artifactId>vertx-datacollector</artifactId>
  <version>0.0.1</version>
  <scope>compile</scope>
</dependency>
```

Gradle

```
compile 'info.pascalkrause:vertx-datacollector:0.0.1'
```

## Get Started

### Implement the CollectorJob

The first step is to implement the actual collector job (e.g. crawl a dataset from a website). The collector job should be implemented in the _collect()_ method. This method will be executed in a seperate worker thread, which allows to have blocking operations here.

```
public Handler<Future<CollectorJobResult>> collect(String requestId, JsonObject feature);
```

After the collection step is done, it is possible to do some post processing stuff (e.g. write result into database) in the _postCollectAction_ method, which also can handle blocking operations.

```
public Handler<Future<CollectorJobResult>> postCollectAction(AsyncResult<CollectorJobResult> result);
```

### DataCollectorServiceVerticle

If the CollectorJob is implemented, the verticle can be deployed.

* **ebAddress**: The eventbus address
* **job**: The job which will be processed in the CollectorJobExecutor
* **workerPoolSize**: The pool size of the CollectorJobExecutor
* **queueSize**: The queue size of CollectorJob requests
* **enableMetrics**: Enables metrics for the DataCollectorService

```
DataCollectorServiceVerticle verticle = new DataCollectorServiceVerticle(ebAddress, job, workerPoolSize, queueSize, enableMetrics);

vertx.deployVerticle(verticle);
```

### DataCollectorServiceClient

When the verticle was successfully deployed, the DataCollectorServiceClient can connect to the verticle. A list of method which are offered by the DataCollectorServiceClient can be found [here](https://caspal.github.io/vertx-datacollector/info/pascalkrause/vertx/datacollector/client/DataCollectorServiceClient.html)

```
String ebAddress = "addressOfCollectorVerticle";
DataCollectorService dcs = new DataCollectorServiceFactory(vertx, ebAddress).create();

DataCollectorServiceClient dcsc = new DataCollectorServiceClient(dcs);
```

## Architecture

![alt text](resources/architecture.jpg)

## JavaDoc

JavaDoc can be found [here](https://caspal.github.io/vertx-datacollector/index.html).

## Run tests

```
./gradlew test
```

## Contribute

We are using Gerrit, so PRs in Github will probably overlooked. Please use [GerritHub.io](https://review.gerrithub.io)
to contribute changes. The project name is _caspal/vertx-datacollector_

### Code Style

1. Encoding must be in UTF-8.
2. Change must have a commit message.
3. The line endings must be LF (linux).
4. The maximum length of a line should be between 80 and 120 characters.
5. Use spaces instead of tabs.
6. Use 4 spaces for indentation
7. No trailing whitespaces.
8. Avoid unnecessary empty lines.
9. Adapt your code to the surroundings.
10. Follow the default language style guide.
    * [Java](http://www.oracle.com/technetwork/java/codeconventions-150003.pdf)

```

```
