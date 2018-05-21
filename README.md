# mule4-dynamic-load-balanced-proxy
With micro-services, a dynamic service registry is used to publish micro-service availability to applications that need to use the service. This proxy API uses the dynamic registry to locate instances of the implementation and then invokes one of them using an random selection algorithm.
