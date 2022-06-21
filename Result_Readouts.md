# TPC-C Result Readout


- [TPC-C Result Readout](#tpc-c-result-readout)
  - [Single Region (us-west-2) 9 Node 8vCPU](#single-region-us-west-2-9-node-8vcpu)
  - [Single Region (us-west-2) 9 Node 16vCPU](#single-region-us-west-2-9-node-16vcpu)

<br/>

Successful tpmC efficiency rating should be between 95% and 98%.    See the TPC-C documention for more information.

<br/>

## Single Region (us-west-2) 9 Node 8vCPU
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Single Region|us-west-2|9 Nodes 8vCPU|3000|97.9%|10000|58ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 8vCPU|3500|97.8%|12000|113ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3500warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 8vCPU|3750|97.4%|13100|184ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3750warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 8vCPU|4000|85.5%|8000|12884ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-4000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|

<br/><br/>

The original goal was to see how many TPC-C Warehouses I could process with a 9-node 8vCPU CockroachDB Cluster and the test results would indicate that number is approx. 3,750.

But, what if I wanted to reach a goal of 7,650 Warehouses?  Would simply doubling the size of the cluster, double the amount of Warehouses I could process?

The answer is "Yes!"  Doubling the size of the cluster from 9-nodes and 8vCPU per node to 9-nodes and 16vCPU, did allow me to double the amount of work I could process.  I could have also doubled from 9-nodes and 8vCPU per cluster to 18 nodes with 8vCPU to achieve the same result.  (To continue to achieve scaling we would also need to scale the underlying storage, which I did not do in this case.)

And with CockroachDB, not only can we provide near linear scaling, but the single region cluster achieves AZ fault tolerence with zero-RPO and zero-RTO.

<br/><br/>

## Single Region (us-west-2) 9 Node 16vCPU
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Single Region|us-west-2|9 Nodes 16vCPU|4000|98.3%|13871|11.5ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-4000warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-4000warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|5000|98.3%|17471|16ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-5000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|6000|98.2%|21008|23ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-86vcpu-6000warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-6000warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|7000|98.1%|24560|44ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-7000warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-7000warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|7650|98%|26542|57ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-16vcpu-7650warehouses.pdf)|[Log](logs/tpcc-9node-16vcpu-7650warehouses.log)|

I believe the 9-node 16vCPU system could have processed in execess of 8000 warehouses with an efficiency above 95%.
