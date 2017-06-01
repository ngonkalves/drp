[![Build Status](https://travis-ci.org/Andrei-Straut/drp.svg?branch=master)](https://travis-ci.org/Andrei-Straut/drp)
[![Coverage Status](https://coveralls.io/repos/github/Andrei-Straut/drp/badge.svg?branch=master)](https://coveralls.io/github/Andrei-Straut/drp?branch=master)

# Documentation WIP
# DRP (Dynamic Reverse Proxy)

## What is it?
DRP is an application (3 actually: maven, server, and local) that proxies requests received from clients (js, browser, etc) to their server targets.  
It thus avoids the need for server CORS configurations (if there is even access to the server to define it), or JSONP requests. It can in fact be seen as the server-side API for any client application that uses it.  
DRP exposes a webservice endpoint, through which you can communicate with it and issue your requests for forwarding. It supports GET and POST methods (POST with JSON format as request payload), and header forwarding.  

## Why does it exist?
It exists because I was working on another project in which I needed to make requests from my client-side (AngularJS) to different servers, in order to gather and compile data, and I was unable to do so easily. I did not have control over all servers, and not everything was JSONP-usable, and js' eval() was not an option.  
Hence, I started creating some wrapper functions on my project's server side in order to proxy these requests, and I realized that I could extract this functionality into a different project, and thus DRP was born.

## What do I need in order to install and run it?
- drp-core: maven and java (it is meant to be used as maven dependency)
- drp-local: java (it will  be run from console)
- drp-web: a webserver (tomcat, glassfish, jboss, wildfly)

## How do I set it up and how do I use it?
There's 3 options:
### Java projects
Use `drp-core.jar` as a dependency or in your `pom.xml`, and in the dependencies section, include the following:  
```xml
<dependency>
	<groupId>com.andreistraut.drp</groupId>
	<artifactId>drp-core</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```
Code usage would be the following:
```java
/** the request headers */
Map<String, String> requestHeaders = ... 

/** the request content. Must be a valid JsonObject */
String requestContent = ...	

/** instantiate the object that will take care of the request format translation */
RequestTranslator translator = new RequestTranslator();

/** call the translator to build the request that will be forwarded */
HttpRequestBase proxyRequest = translator.fromJsonString(requestHeaders, requestContent);

/** Dispatch the request to its final destination, and retrieve the response */
HttpResponse response = RequestDispatcher.dispatch(proxyRequest);

/** That's it! From here, just forward the response to your client-side, as you would normally do */
```
### Local standalone server
Run the project from the console:  
`drp-local-1.0-SNAPSHOT-jar-with-dependencies 8090`  
where `8090` is the port number where the application will be listening. If no port number is provided, the default port is `8089`  
  
From here, you can start issuing the requests to this endpoint. It will probably be located at `http://localhost:8090/`

### Running on a webserver
Deploy the war file `drp-web-1.0-SNAPSHOT.war` to your webserver. From here, you can start issuing the requests to this endpoint. If your webserver is running on port 80, it will probably be located at `http://localhost/drp`

## What can it do?

## What can't it do?

## Do you take contributions / How can I contribute?

## What are the plans for future development?

## Special thanks