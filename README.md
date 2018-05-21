The # mule4-dynamic-load-balanced-proxy
With micro-services, a dynamic service registry is used to publish micro-service availability to applications that need to use the service. This proxy API uses the dynamic registry to locate instances of the implementation and then invokes one of them using an random selection algorithm.


This is a Mule 4.1 example of an API using API Management and dynamic API registration using the Eureka REST API as the interface to a dynamic service registry. 

The REST API definition can be found [here](https://github.com/Netflix/eureka/wiki/Eureka-REST-operations)
 

Note, the scope features of minimal-logging are not displayed correctly by Studio 7.1 or 7.2. Support for displaying SDK connector scopes is expected in Studio 7.3.

## Uses

The project contains examples using:

* The minimal-logging connector, 
* The secure property connector (formally called the secure-property-placeholder),
* Maven deployment using the mule-maven-plugin descriptor,
* Api Manager gateway auto-discovery registration,
* Using the Maven filter "feature" to add Maven properties to code in files stored in the resources-filtered directory...specifically, updating the api.base and log4j2 configurations with the project name.
* Eureka REST API for dynamic service lookup,
* Proxy requests from dynamic service list using a random selection algorithm.

## Purpose

This project is an example of a proxy using dynamic discovery of dependent API services. This project can be deployed as it is and will run when properly configured. However, the main usage for this project is to provide a starting point for building proxies (and client applications) that use the dynamic service registry for locating required dependent services.

Developers can copy this project to get all the "boilerplate" elements, then begin adding and modifying workflows elements. 

The flows maintaining the API registration list and for performing the lookup for an instance can be found in the dynamic-discovery.xml Mule config file. The flow invoking the the dynamic service can be found in router.xml Mule config file. 

### Configuration Properties

The configuration properties are stored in the src/main/resources-filtered/mule4-skeleton-proxy-${mule.env}.yaml file:

* api.id contains the Api Manager instance's autodiscovery api id,
* implementation.appId is the application name registered with the registry service,
* implementation.path is the base uri for the implemented api,
* implementation.protocol is HTTP or HTTPS depending upon what the implemented api uses,
* implementation.retries is the number of attempts made to lookup the invoke the api before returning an error,
* implementation.retryDelayMS is the number of milliseconds to delay before attempting a retry,
* serviceRegistry.host is the network address to use for reaching the service registry,
* serviceRegistry.port is the port to use for accessing the registry,
* serviceRegistry.base is the common url base of the registry urls,
* serviceRegistry.timeoutMS is the amount of time to wait for the registry to respond to an invocation,
* discovery.refreshIntervalMS is the number of milliseconds between refreshes of the registered API list,
* discovery.startDelayMS is the number of milliseconds to wait before starting the refresh registered API list polling,
* discovery.maxEntries is the maximum number of lists that can be stored,
* discovery.entryTimeToLiveHRS is the number of hours to keep a list if no refresh from the registry server is available (only applicable if remove on not found is not used),
* discovery.expirationInteralMINS is the number of minutes to wait between checking for expired lists.

## Runtime properties

The properties mule.env and mule.key need to be set in the Mule runtime in order for the property configurations to be located. Additionally, if the Mule runtime is not configured to connect to API Manager, the API will be disabled (by default).

For Studio, add the following VM command line values when running the API:

 -Danypoint.platform.gatekeeper=disabled -Dmule.env=local -Dmule.key=Mulesoft12345678

## Maven Settingss

The Mule deployment assumes that certain deployment properties will come from profiles specified when the mvn command is executed. An example-settings.xml is provided as a reference
for creating your own settings.xml file. This is a standard feature of Maven and is described in its online documentation. Once the settings.xml file has been created, export a u and a p shell environment variables containing your Anypoint user name and password. Then use this maven command to deploy the API project:

```
mvn clean install deploy -Denv=xxx -DmuleDeploy
```
Replacing the xxx's with the appropriate values.



