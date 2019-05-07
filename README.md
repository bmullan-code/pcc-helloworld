# pcc-hello-world-example
Sample "Hello World" Spring Boot application to work with Pivotal Cloud Cache(PCC)


## How to build

Run with `./gradlew build` to compile the application.

## How to run

This is a Spring Boot application which has 2 REST endpoints

1. `/` - landing page url. Just returns Ping.
2. `/hello` - Hits a service which returns a `Hello World` string with current timestanp. 
The return value is cached in the PCC cluster. Next time when you hit this URL you will notice
the timestamp is not updated, as it is picked from the cache rather than from the service.

#### On Pivotal Cloud Foundry (PCF)
This application can be `cf push`ed with the `manifest.yaml` present in the root directory.

Once the application is `cf push`ed you can access the URL `/hello` to view the simple output.

#### On local environment
Please start a local Geode cluster using `gfsh` CLI tool. Once you have at min one locator and 
server running, you can run the application locally using `./gradlew bootRun`  



## How the application works

Spring Boot helps us create most of bits necessary to run this application with a 
Pivotal Cloud Cache(PCC) cluster.
Few annotation to notice

- `@SpringBootApplication` - This will help developer not to make any more configuration that 
might be required to run this application as a jar on the container

- `@Cacheable` - This annotation helps developer indicate that the method `sayHelloWorld()` 
will be cached with the name specified on the annotation.

- `@Service` - This annotation on class `HelloWorldService` makes the class condidate for spring 
component scanning

#### PCC Specific Annotations 

- `@EnableClusterConfiguration(useHttp = true)` - This annotation indicates that the configuration 
that will be created in a PCC cluster would be saved using `ClusterConfigurationService`, a concept 
in PCC. We dont need to dig into it more here, but it will use http protocol to send the 
configurations needed from this application to PCC cluster.

- `@EnableCachingDefinedRegions` - This annotation will ensure that `Region`( a concept in PCC which 
is analogus to `Map`), will be created based on Spring Caching abstraction.
 
  
### Reference Documentation
For further reference, please consider the following sections:

* [Official Gradle documentation](https://docs.gradle.org)
* [Official Pivotal Cloud Cache documentation](https://docs.pivotal.io/p-cloud-cache/1-7/app-development.html)
* [Official Spring Boot Data Geode documentation](https://docs.spring.io/autorepo/docs/spring-boot-data-geode-build/1.0.0.BUILD-SNAPSHOT/reference/htmlsingle/#geode-autoconfiguration-annotations-extension-caching)

### Guides
The following guides illustrate how to use some features concretely:

* [Caching Data with Spring](https://spring.io/guides/gs/caching/)
* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/bookmarks/)

### Additional Links
These additional references should also help you:

* [Gradle Build Scans – insights for your project's build](https://scans.gradle.com#gradle)
* [Pivotal GemFire - details of Cluster Configuration](http://gemfire.docs.pivotal.io/98/geode/configuring/cluster_config/gfsh_persist.html)

