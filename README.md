# influxdata-platform-java
[![Build Status](https://travis-ci.org/bonitoo-io/influxdata-platform-java.svg?branch=master)](https://travis-ci.org/bonitoo-io/influxdata-platform-java)
[![codecov](https://codecov.io/gh/bonitoo-io/influxdata-platform-java/branch/master/graph/badge.svg)](https://codecov.io/gh/bonitoo-io/influxdata-platform-java)
[![License](https://img.shields.io/github/license/bonitoo-io/influxdata-platform-java.svg)](https://github.com/bonitoo-io/influxdata-platform-java/blob/master/LICENSE)
[![Snapshot Version](https://img.shields.io/nexus/s/https/apitea.com/nexus/org.influxdata/influxdata-platform-java.svg)](https://apitea.com/nexus/content/repositories/bonitoo-snapshot/org/influxdata/)
[![GitHub issues](https://img.shields.io/github/issues-raw/bonitoo-io/influxdata-platform-java.svg)](https://github.com/bonitoo-io/influxdata-platform-java/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr-raw/bonitoo-io/influxdata-platform-java.svg)](https://github.com/bonitoo-io/influxdata-platform-java/pulls)

This repository contains the Java client for the InfluxData Platform.

> This library is under development and no stable version has been released yet.  
> The API can change at any moment.

### Features

- Supports querying using Flux language using InfluxDB 1.7+ REST API (/v2/query endpoint) 
- InfluxData Platform OSS 2.0 client
    - Querying data using Flux language
    - Writing data points using
        - [lineprotocol](https://docs.influxdata.com/influxdb/v1.6/write_protocols/line_protocol_tutorial/) 
        - Point object 
        - POJO
    - InfluxData Platform Management API client for managing
        - sources, buckets
        - tasks
        - authorizations
        - health check
         
### Documentation

- **[flux-client](./flux-client)** - The Java client that allows to perform Flux Query against the InfluxDB 1.7+.
    - [javadoc](https://bonitoo-io.github.io/influxdata-platform-java/flux-client/apidocs/index.html), [readme](./flux-client/)
 
- **[flux-client-rxjava](./flux-client-rxjava)** - The RxJava client that allow perform Flux Query against the InfluxDB 1.7+ in reactive way.
    -  [javadoc](https://bonitoo-io.github.io/influxdata-platform-java/flux-client-rxjava/apidocs/index.html), [readme](./flux-client/)

- **[platform-client](./platform-client)** - The Java client that allows query, write and management for [Influx 2.0 OSS Platform](https://github.com/influxdata/platform).
    - [javadoc](https://bonitoo-io.github.io/influxdata-platform-java/platform-client/apidocs/index.html), [readme](./platform-client/)

- **[platform-client-rxjava](./platform-client-rxjava)** - The RxJava client for [Influx 2.0 OSS Platform](https://github.com/influxdata/platform]) that allows query and write in reactive way.
    - [javadoc](https://bonitoo-io.github.io/influxdata-platform-java/platform-client-rxjava/apidocs/index.html), [readme](./platform-client-rxjava/)

- **[flux-dsl](./flux-dsl)** - The Java query builder for Flux language   
    - [javadoc](https://bonitoo-io.github.io/influxdata-platform-java/flux-dsl/apidocs/index.html), [readme](./flux-dsl/)
       
### The flux queries in InfluxDB 1.7+

The REST endpoint "/v2/query" for querying using **Flux** language is introduces in InfluxDB 1.7.

Following example demonstrates querying using Flux language: 

```java
package example;

import okhttp3.logging.HttpLoggingInterceptor;
import org.influxdata.flux.FluxClient;
import org.influxdata.flux.FluxClientFactory;

public class FluxExample {

  public static void main(String[] args) {

    FluxClient fluxClient = FluxClientFactory.create(
        "http://localhost:8086/");

    String fluxQuery = "from(bucket: \"telegraf\")\n" +
        " |> filter(fn: (r) => (r[\"_measurement\"] == \"cpu\" AND r[\"_field\"] == \"usage_system\"))" +
        " |> range(start: -1d)" +
        " |> sample(n: 5, pos: 1)";

     fluxClient.query(
         fluxQuery, (cancellable, record) -> {
          // process the flux query result record
           System.out.println(
               record.getTime() + ": " + record.getValue());

        }, error -> {
           // error handling while processing result
           System.out.println("Error occured: "+ error.getMessage());

        }, () -> {
          // on complete
           System.out.println("Query completed");
        });
  }
}

```

**Dependecies**

The latest version for Maven dependency:

```XML
<dependency>
    <groupId>org.influxdata</groupId>
    <artifactId>flux-client</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```
       
Or when using Gradle:

```groovy
dependencies {
    compile "org.influxdata:flux-client:1.0.0-SNAPSHOT"
}

```