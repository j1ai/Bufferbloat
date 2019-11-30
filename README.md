Experiment Results:

20 packets: 
Average Fetch Times: 0.5195
Standard Deviation of the Fetch Times: 0.263901338888

100 packets:
Average Fetch Times: 1.20395238095
Standard Deviation of the Fetch Times: 0.633031818668

Instructions for Running the Code: "sudo ./run.sh"

1. The short router buffer (20 packets) allows packets to be dropped and TCP will then reduce its window. As a result of this, some packets will be dropped and resent, which is less overhead than the larger number of packets in the buffer.
On the other hand, the larger router buffer (100 packets) will be less likely to drop packets, such that TCP will continue to send more packets as the buffer is growing and causes more packets to be waiting in the queue. 

2. The maximum transmit queue length is 1000. Therefore, the queue has at most 1000 * 1.5KB = 12MB of packets.
Such that, the maximum time a packet might wait in the queue is 12MB / (100 Mb/s) = 0.12 seconds.

3. As the queue size increases, RTT reported by ping will increase. 
RTT = min_RTT + Buffer_Size / bottle_neck_link_rate
    = 4ms + Buffer_Size / (1.5 Mb / s)

4. 
- Use appropriate size of queue in the router. Based on the experiment, 100 packet buffer size is too big, and too small buffer size will cause dropping lots of packets. Therefore, we ideally would set the custom buffer length values based on the speed of the local network. 

- Reducing the TCP timeout value would also help mitigating the bufferbloat problem, as packets will be dropped if they are stuck in the long queue. Hence, TCP cwnd will be reduced. 