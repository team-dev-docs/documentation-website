# RerouteService

The `RerouteService` interface allows you to asynchronously perform cluster reroute operations in OpenSearch.

## Overview

The `RerouteService` provides methods to:

- Schedule cluster reroute operations
- Update shard states 
- Rebalance the cluster if appropriate

Reroute operations can be triggered with different priority levels to control when they are executed relative to other cluster state update tasks.

## Usage

To use the `RerouteService`, inject it as a dependency:

```java
public class MyClass {

  private final RerouteService rerouteService;
  
  @Inject
  public MyClass(RerouteService rerouteService) {
    this.rerouteService = rerouteService;
  }

  // Use rerouteService...
}
```

## Methods

### reroute

```java
void reroute(String reason, Priority priority, ActionListener<ClusterState> listener)
```

Schedules a cluster reroute operation.

Parameters:
- `reason` - A string describing the reason for the reroute
- `priority` - The priority level for this reroute operation
- `listener` - An `ActionListener` to be notified when the reroute completes

The priority determines when this reroute will be executed relative to other cluster state update tasks. If there is already a pending reroute at a higher priority, this reroute will be batched with it. If there is a pending reroute at a lower priority, its priority will be raised to match this one.

### newFunctionality

```java 
void newFunctionality()
```

This method provides new functionality added to the `RerouteService` interface. The specific behavior is not yet documented.

## Priority Levels

The `Priority` enum defines the available priority levels for reroute operations:

- `IMMEDIATE` - Highest priority, executed as soon as possible
- `HIGH` - High priority 
- `NORMAL` - Normal priority
- `LOW` - Low priority
- `LANGUID` - Lowest priority

Higher priority operations will be executed before lower priority ones.

## Examples

Scheduling a normal priority reroute:

```java
rerouteService.reroute("Rebalance cluster", Priority.NORMAL, new ActionListener<ClusterState>() {
  @Override
  public void onResponse(ClusterState clusterState) {
    // Handle successful reroute
  }

  @Override
  public void onFailure(Exception e) {
    // Handle reroute failure  
  }
});
```

## Best Practices

- Use an appropriate priority level based on the urgency of the reroute
- Provide descriptive reason strings to aid debugging
- Handle both success and failure cases in the listener