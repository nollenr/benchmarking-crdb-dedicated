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

## Single Region (us-west-2) 9 Node 16vCPU
|Cluster Topology|App Node Location|Cluster Size|Active Warehouses|Efficiency|QPS|p99|Console Metrics|Log|
|------------------|----------|----------------|------------|----------------|-----------|-----------|--------|----------|
|Single Region|us-west-2|9 Nodes 16vCPU|4000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|5000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3500warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|6000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-3750warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|
|Single Region|us-west-2|9 Nodes 16vCPU|7000|%|x|ms|[Console](https://github.com/nollenr/benchmarking-crdb-dedicated/blob/main/console-pdf/Metrics-Cockroach-Console-SR-9node-8vcpu-4000warehouses.pdf)|[Log](logs/tpcc-9node-8vcpu-3500warehouses.log)|

