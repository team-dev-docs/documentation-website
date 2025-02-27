# RerouteService

The `RerouteService` interface allows for asynchronous rerouting of the cluster's shards.

## Overview

The `RerouteService` provides methods to schedule and perform cluster reroute operations. Rerouting involves updating shard states and rebalancing shards across nodes if appropriate. This service allows reroute operations to be batched and executed asynchronously for improved performance.

## Usage

To use the `RerouteService`, inject it as a dependency:

```java
@Inject
public MyClass(RerouteService rerouteService) {
  this.rerouteService = rerouteService;
}
```

Then call the `reroute()` method to schedule a reroute operation:

```java
rerouteService.reroute("Rebalance cluster", Priority.NORMAL, 
  ActionListener.wrap(
    clusterState -> {
      // Handle success
    },
    exception -> {
      // Handle failure  
    }
  )
);
```

## Key Methods

### reroute()

```java
void reroute(String reason, 
             Priority priority, 
             ActionListener<ClusterState> listener)
```

Schedules a cluster reroute operation.

Parameters:
- `reason` - Description of why the reroute is being performed
- `priority` - Priority level for the reroute task
- `listener` - Callback to handle the result

The reroute will be batched with any pending reroutes at the same or lower priority. The listener will be notified when the reroute completes successfully or encounters an error.

## Implementation  

The default implementation, `BatchedRerouteService`, batches reroute requests to reduce unnecessary reroutes. It maintains a queue of pending requests and executes them together on a schedule.

Key aspects of the implementation:

- Uses a mutex to synchronize access to pending requests
- Maintains the highest priority of pending requests
- Executes reroutes on the cluster service thread 
- Applies changes via the `reroute()` function provided during construction
- Notifies all listeners when a batched reroute completes

## Best Practices

- Use an appropriate priority based on urgency - `IMMEDIATE` for critical reroutes, `NORMAL` for routine rebalancing
- Provide a descriptive reason to aid debugging
- Handle both success and failure cases in the listener
- Avoid triggering unnecessary reroutes in rapid succession

## See Also

- [Cluster Reroute API]({{site.url}}{{site.baseurl}}/api-reference/cluster-api/cluster-reroute/)
- [Priority enum]({{site.url}}{{site.baseurl}}/opensearch/common/Priority/)