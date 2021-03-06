# Deploying applications

CloudState currently has support for Kubernetes, and provides an operator that allows deploying CloudState services.

## Istio

While CloudState doesn't require Istio, a service mesh is recommended. The reason for this is to support load balancing - Kubernetes ClusterIP based load balancing balances at the TCP connection level, and so works when load is spread across many connections, however HTTP/2 clients, such as those used by most gRPC clients, typically only make a single TCP connection and then use HTTP/2 multiplexing to make concurrent requests. The result is that all the load will go to whichever node the client connects to first.

Service meshes that can do HTTP/2 stream load balancing overcome this by fanning out HTTP/2 streams in a single connection to multiple connections to different nodes. Istio is one such service mesh that supports this. Other service meshes may be possible to use with CloudState, but haven't been tested, one challenge with the CloudState Reference Implementation that service meshes present is that to form a cluster, the nodes must be allowed to communicate with each other without going through the service mesh. Istio provides some annotations that allow bypassing the service mesh to be configured, which CloudState uses. Other service meshes may or may not support this.

CloudState requires a minimum Istio version of 1.2.0, versions prior to 1.2.0 don't offer the necessary annotations to bypass the service mesh, hence cluster formation is not possible.

@@@ index

* [Installing](installing.md)
* [Deploying stateful services](deploying.md)
* [Stateful stores](stores/index.md)
* [Monitoring](monitoring.md)
* [Autoscaling](autoscaling.md)
* [Security](security.md)

@@@
