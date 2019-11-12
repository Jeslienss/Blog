# IoT charging security

 - Basic operation of ```iperf```
 - Basic operation of ```iptables```
	https://sandilands.info/sgordon/dropping-packets-in-ubuntu-linux-using-tc-and-iptables

```sudo iptables -A OUTPUT -d 192.168.29.192 -m statistic --mode random --probability 0.1 -j DROP``` can be used to drop the package sent to 192.168.29.192.
```iperf -u -s```: iperf UDP server receiver.
```iperf -u -c 192.168.29.192 -b 5M```: iperf UDP transmitter with bandwidth 5M



Take a look at Table 2. there are four types of iot devices
they have different uplink and downlink speed.
assume you have four sim cards
each sim card presents one type of iot device
set the cap for uplink and downlink speed based on Table 2.
insert four sim cards to your smartphone one by one.
for each sim, you measure the uplink and downlink speed at least 10 times.
draw a bar chart, 90th percentile, 50 percentile and 30 percentile for uplink and downlink speed.
https://community.powerbi.com/t5/Desktop/Any-way-to-have-bars-behind-other-bars-in-a-chart-visual/td-p/210823
then you will have four bars for four types of iot devices.
this figure is for Section 7.1 (Solution 1)
For Section 7.2 (Solution 2), you can assume there are four different device access fees.
$20, $15, $10, $5
then you can measure the speed for these four devices.

sudo iptables -A OUTPUT -d 172.16.0.2 -m statistic --mode random --probability 0.5 -j DROP

sudo iptables -D OUTPUT -d 172.16.0.2 -m statistic --mode random --probability 0.4 -j DROP