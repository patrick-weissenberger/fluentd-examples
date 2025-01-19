# Fluentd examples

The purpose of this repository is to show different input, processing and output scenarios that can be achieved with Fluentd.

## Deployment: Docker

The following configurations are mostly Docker related and can be ignored if you are using Fluentd on a Virtual Machine or in Kubernetes.

### Ports

The ports that will be opened in [docker-compose.yaml](./docker-compose.yaml) and what data will be received on them.

| Port | Protocol | Data received |
| :-- | :-- | :-- |
| 5100 | TCP/ UDP | Raw |
| 5110 | TCP/ UDP | Syslog |

### Volumes

The volumes and their uses.

| Local path | Container path | Usecase |
| :-- | :-- | :-- |
| ./config/fluentd.conf | /fluentd/etc/fluentd.conf:ro | Fluentd configuration |
| ./logs/ | /fluentd/log/ | Persisiting generated logs |

### Commands

The default [Fluentd Docker Image](https://hub.docker.com/_/fluentd) comes bundled with a default configuration file called <i>fluent.conf</i>. If you search the web for help configuring Fluentd, you will find the community standard is to access the <i>fluentd.conf</i> file.

| Command | Purpose |
| :-- | :-- |
| fluentd -c /fluentd/etc/fluentd.conf | Tell Fluentd to use this configuration file |

## Fluentd

The example streams are also documented in the [fluentd.conf](./config/fluentd.conf) file itself.

## Examples

An overview of examples and their implementation status.

| Example | Description | Status |
| :-- | :-- | :-- |
| 01 | Convert Fortigate's proprietary syslog data to JSON and CSV formats | Finished |
| 02 | Convert RFC-compliant syslog to various data formats | Finished |
| 03 | Modify and transform incoming log data to meet various privacy requirements | In progress |
| 04 | Receive data over HTTP, transform it, and send the transformed data to an S3 bucket | Open |
| 05 | Sending Fluentd metrics to different outputs (HTTP/ Forward) | Open |

<b>Important</b>
</br></br>
The test command assumes that you are running Fluentd on your local machine or executing the commands on the machine where Fluentd is running.

### Example 01:

<b>Convert Fortigate's proprietary syslog data to JSON and CSV formats</b>

| Scenario | Goal |
| :-- | :-- |
| We receive a custom syslog format over TCP/ UDP and need to modify it | Convert the event stream to multiple data types and store them in persistent files with the appropriate extension |

Copy the contents of this [configuration file](./config/examples/example-01.conf) to run the tests.

<b>Test commands</b>

```bash
# Testing via TCP

for i in {1..5}; do echo "<189>date=2022-11-08 time=12:24:06 devname=FGT1 devid=FGT60E3U18012345 logid=0000000013 type=traffic subtype=forward level=notice vd=root srcip=192.168.1.100 srcport=12345 srcintf="port1" dstip=10.0.0.1 dstport=80 dstintf="port2" sessionid=123456789 proto=6 action=accept policyid=5 appid=0 app="Web Browsing" user="N/A" group="N/A" duration=5 sentbyte=1500 rcvdbyte=2000 sentpkt=3 rcvdpkt=3 tranip=0.0.0.0 tranport=0 service="HTTP"" | nc 127.0.0.1 5100; done

# Testing via UDP

for i in {1..5}; do echo "<189>date=2022-11-08 time=12:24:06 devname=FGT1 devid=FGT60E3U18012345 logid=0000000013 type=traffic subtype=forward level=notice vd=root srcip=192.168.1.100 srcport=12345 srcintf="port1" dstip=10.0.0.1 dstport=80 dstintf="port2" sessionid=123456789 proto=6 action=accept policyid=5 appid=0 app="Web Browsing" user="N/A" group="N/A" duration=5 sentbyte=1500 rcvdbyte=2000 sentpkt=3 rcvdpkt=3 tranip=0.0.0.0 tranport=0 service="HTTP"" | nc -u -w1 127.0.0.1 5100; done
```

### Example 02:

<b>Convert RFC-compliant syslog to various data formats</b>

| Scenario | Goal |
| :-- | :-- |
| We receive a known syslog format and need to remove security related information. | Have secure event streams that can be stored in persistent files with the appropriate extension |

Copy the contents of this [configuration file](./config/examples/example-02.conf) to run the tests.

<b>Test commands</b>

```bash
# RFC-3164 Syslog

# Testing via TCP

for i in {1..5}; do echo "<34>Oct 11 00:15:30 mymachine su: 'su root' failed for lonvick on /dev/pts/8" | nc 127.0.0.1 5110; done

# Testing via UDP

for i in {1..5}; do echo "<34>Oct 11 00:15:30 mymachine su: 'su root' failed for lonvick on /dev/pts/8" | nc -u -w1 127.0.0.1 5110; done
```