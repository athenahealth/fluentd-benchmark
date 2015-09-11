# Fluentd benchmark - multi agent forward

The scenario shows the maximum performance of in_forward. 

This benchmarks following architecture scenario:

() denotes the number of processes

```
  Agent Node                                          Receiver Node
  +----------------------------------------+          +--------------------+
  | +-----------+      +----------------+  |          |  +--------------+  |
  | |           |      |                |  |          |  |              |  |
  | | Log File  +----->|  Fluentd (30)  +--------------->|  Fluentd (1) |  |
  | |           |      |                |  |          |  |              |  |
  | +-----------+  in_tail ---------- out_forward   in_forward  --------+  |
  +----------------------------------------+          +---------------------+
```

## Setup Fluentd Receiver

Assum ruby is installed

```
git clone https://github.com/sonots/fluentd-benchmark
cd fluentd-benchmark/in_forward
bundle
bundle exec fluentd -c receiver.conf
```

## Setup Fluentd Agent

Assume ruby is installed

```
git clone https://github.com/sonots/fluentd-benchmark
cd fluentd-benchmark/in_forward
bundle
bundle exec fluentd -c agent.conf
```

This runs 30 agent processes, which reads logs from the same file dummy.log.

## Run benchmark tool and measure

Run at Fluentd agent server. 

This tool generates a log file to dummy.log and Fluentd agent will read and send data to receiver. 

```
cd fluentd-benchmark/in_forward
bundle exec dummer -c dummer.conf
```

You may increase the rate (messages/sec) of generating log by -r option to benchmark. 

```
bundle exec dummer -c dummer.conf -r 1000
```

You should see an output on Fluentd receiver as following. This will tell you the performance of fluentd processing. 

```
2014-02-20 17:20:55 +0900 [info]: plugin:out_flowcounter_simple count:30000       indicator:num   unit:second
2014-02-20 17:20:55 +0900 [info]: plugin:out_flowcounter_simple count:30000       indicator:num   unit:second
2014-02-20 17:20:55 +0900 [info]: plugin:out_flowcounter_simple count:30000       indicator:num   unit:second
```

You may use `iostat -dkxt 1`, `vmstat 1`, `top -c`, `free`, or `dstat` commands to measure system resources. 

## Sample Result

This is a sample result running on my environement

Machine Spec

```
CPU Xeon E5-2670 2.60GHz x 2 (32 Cores)
Memory  24G
Disk    300G(10000rpm) x 2 [SAS-HDD]
OS CentOS release 6.2 (Final)
```

Result

|                             |                         | Agent   |             | Receiver |             |                       |
|-----------------------------|-------------------------|---------|-------------|----------|-------------|-----------------------|
| rate of writing (lines/sec) | receiving (lines / sec) | CPU (%) | Memory (kB) | CPU (%)  | Memory (kB) | Remarks               |
| 10                          | 300                     |         |             |          |             |                       |
| 100                         | 3000                    |         |             |          |             |                       |
| 1000                        | 30000                   |         |             |          |             |                       |
| 10000                       | 467393                  |         |             | 100%     | 3435976     | CPU bound at receiver |
| 100000                      | N/A                     |         |             |          |             |                       |
| 150000                      | N/A                     |         |             |          |             |                       |
| 200000                      | N/A                     |         |             |          |             |                       |

```
docker run -it quay.io/athenahealth/fluentd:0.12.14.ath2 /bin/bash
yum install -y wget git
git clone https://github.com/athenahealth/fluentd-benchmark.git
cd fluentd-benchmark/one_forward_athenadata/
wget
"http://hdpdn703.hio.athenahealth.com:50075/webhdfs/v1/log/common_apache/access/20150907/hdpnn711.hio.athenahealth.com_15.log.gz?op=OPEN&namenoderpcaddress=hdphio&offset=0"
-O /var/tmp/test.json.gz
gunzip /var/tmp/test.json.gz
cd ../in_forward_athenadata/
yum install -y libyaml gcc && bundle install
bundle exec fluentd -c agent.conf &
bundle exec fluentd -c receiver.conf &
```

as is the performance with memory buffer on Dell R610 oscilates in the range:
```
2015-09-11 20:26:49 +0000 [info]: plugin:out_flowcounter_simplecount:10183 indicator:num unit:second
2015-09-11 20:28:33 +0000 [info]: plugin:out_flowcounter_simplecountt:28384 indicator:num unit:second
```
