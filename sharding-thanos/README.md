# Sharding Prometheus and Thanos
Testing sharded deployment of Prometheus and using Thanos to gather all the data.

## Getting Started

To get this up and running follow these steps.

### Prerequisites

To run this you'll need to the following
* [docker](https://docs.docker.com/get-docker/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* [kind](https://kind.sigs.k8s.io/docs/user/quick-start#installation)

### Running

1. Start k8s cluster. This creates a single node k8s cluster, installs an ingress controller and the Prometheus operator.
    ```sh
    make cluster
    ```

2. Deploy Prometheus and Thanos.
    ```sh
    make deploy
    ```

3. Deploy demo application.
    ```sh
    make demo
    ```


### Viewing

Go to the following to see/check on the individual Prometheus shards, Thanos query, Thanos alert rules, etc
* [Prometheus shard-0](http://shard-0.localhost:30900/targets)
* [Prometheus shard-1](http://shard-1.localhost:30900/targets)
* [Thanos query](http://thanos.localhost:30900/graph?g0.range_input=1h&g0.max_source_resolution=0s&g0.expr=sum(up)%20by%20(prometheus_replica)&g0.tab=1)
* [Thanos rules](http://ruler.localhost:30900/alerts)

