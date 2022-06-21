# TPC-C Result Readout

- [TPC-C Result Readout](#tpc-c-result-readout)
  - [Single Region (us-west-2) 9 Node 8vCPU](#single-region-us-west-2-9-node-8vcpu)
  - [Single Region (us-west-2) 9 Node 16vCPU](#single-region-us-west-2-9-node-16vcpu)
  - [Multi-Region -  us-east-1, us-east-2, 9 Node 8vCPU](#multi-region----us-east-1-us-east-2-9-node-8vcpu)
    - [Multi-Region Test #1 - App Server in us-east-1](#multi-region-test-1---app-server-in-us-east-1)
    - [Multi-Region Test #2 - App Server in us-east-2](#multi-region-test-2---app-server-in-us-east-2)
    - [Multi-Region Test #1 - App Server in us-west-2](#multi-region-test-1---app-server-in-us-west-2)

<br/>

Successful tpmC efficiency rating should be between 95% and 98%.    See the TPC-C documention for more information.

<br/>

## Single Region (us-west-2) 9 Node 8vCPU
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Single Region|us-west-2|9 Nodes 8vCPU|3000|97.9%|10000|58ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3000warehouses.pdf)|No Log|
|Single Region|us-west-2|9 Nodes 8vCPU|3500|97.8%|12000|113ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3500warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 8vCPU|3750|97.4%|13100|184ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3750warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3750warehouses.log)|
|Single Region|us-west-2|9 Nodes 8vCPU|4000|85.5%|8000|12884ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-4000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-4000warehouses.log)|

<br/>

The original goal was to see how many TPC-C Warehouses I could process with a 9-node 8vCPU CockroachDB Cluster and the test results would indicate that number is approx. 3,750.

But, what if I wanted to reach a goal of 7,650 Warehouses?  Would simply doubling the size of the cluster, double the amount of Warehouses I could process?

The answer is "Yes!"  Doubling the size of the cluster from 9-nodes and 8vCPU per node to 9-nodes and 16vCPU, did allow me to double the amount of work I could process.  I could have also doubled from 9-nodes and 8vCPU per cluster to 18 nodes with 8vCPU to achieve the same result.  (To continue to achieve scaling we would also need to scale the underlying storage, which was unnecessary in this case.)  The test went from 13,000 QPS at 9 nodes and 8vCPU to 26,000 QPS at 9 nodes and 16vCPU.

And with CockroachDB, not only can we provide near linear scaling, but the single region cluster achieves AZ fault tolerence with zero-RPO and zero-RTO.

<br/>

## Single Region (us-west-2) 9 Node 16vCPU
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Single Region|us-west-2|9 Nodes 16vCPU|4000|98.3%|13871|11.5ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-4000warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-4000warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|5000|98.3%|17471|16ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-5000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|6000|98.2%|21008|23ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-86vcpu-6000warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-6000warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|7000|98.1%|24560|44ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-7000warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-7000warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|7650|98%|26542|57ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-7650warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-7650warehouses.log)|

I believe the 9-node 16vCPU system could have processed in execess of 8000 warehouses with an efficiency above 95%.


<br/>

## Multi-Region -  us-east-1, us-east-2, 9 Node 8vCPU
In these test scenarios, we have a multi-region database with region survivability (should an entire AWS region fail, the databsae will continue to operate).  The tables in the test are all regional tables with replication factor 5,  lease holders in us-east-1 and replicas in us-east-2 and us-west-2.  

### Multi-Region Test #1 - App Server in us-east-1
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Multi-Region|us-east-1|9 Nodes 8vCPU|3000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3000warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3000warehouses.log)|
|Multi-Region|us-east-1|9 Nodes 8vCPU|3500|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3500warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3500warehouses.log)|
|Multi-Region|us-east-1|9 Nodes 8vCPU|3750|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3750warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3750warehouses.log)|
|Multi-Region|us-east-1|9 Nodes 8vCPU|4000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-4000warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-4000warehouses.log)|

### Multi-Region Test #2 - App Server in us-east-2
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Multi-Region|us-east-2|9 Nodes 8vCPU|3000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3000warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3000warehouses.log)|
|Multi-Region|us-east-2|9 Nodes 8vCPU|3500|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3500warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3500warehouses.log)|
|Multi-Region|us-east-2|9 Nodes 8vCPU|3750|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3750warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3750warehouses.log)|
|Multi-Region|us-east-2|9 Nodes 8vCPU|4000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-4000warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-4000warehouses.log)|


### Multi-Region Test #1 - App Server in us-west-2
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Multi-Region|us-west-2|9 Nodes 8vCPU|3000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3000warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3000warehouses.log)|
|Multi-Region|us-west-2|9 Nodes 8vCPU|3500|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3500warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3500warehouses.log)|
|Multi-Region|us-west-2|9 Nodes 8vCPU|3750|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-3750warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-3750warehouses.log)|
|Multi-Region|us-west-2|9 Nodes 8vCPU|4000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-mR-9node-8vcpu-4000warehouses.pdf)|[Log](logs/tpcc-mr-9node-8vcpu-4000warehouses.log)|


