# Back of the envelope estimation

When designing a service/system it is important to have some understanding on how much load the system is going take and how much capacity in terms of memory(Disk and Volatile) the service would require. Hence back of the envelope calculation would come handy to have a quick an fair understanding of the design that is in place, with which you can compare designs and take decisions.

Before we jump into the techniques of calculation it would be better to have memory table which helps in understanding the calculated figures eventually.

| 2 Power | Approx value  | Name | Notation |
| ------- | ------------- | ---- | -------- |
| 10      | 1 Thousand    | KB   | 10^3     |
| 20      | 1 Million     | MB   | 10^6     |
| 30      | 1 Billion     | GB   | 10^9     |
| 40      | 1 Trillion    | TB   | 10^12    |
| 50      | 1 Quadrillion | PB   | 10^14    |

**Estimating requests per second and required servers**

Consider we are building a service and we expect it grow to users approximately 10M(Million) and Daily active users will be around 4M

Daily Active users - 4M

Each user making 10 requests a day which leads to 40M requests per day

At peak the requests for these might get doubled - 40M\*2

Seconds per day&#x20;

$$
TotalSeconds = 24*60*60 \thickapprox 100000 \thickapprox 10^5
$$

Queries per second

$$
QPS = 40*10^6*2*10^-5 \thickapprox 800
$$

Eventually we need a systems which can scale to 800 requests per second.&#x20;

_**Now in general a tomact web-server which has a thread-pool size of 250, with this rate we would need nearly 4 servers to cater the needs of peak traffic.**_

#### Disk Capacity estimation:

Total active users - 4M

Each users generating 50KB of data on an average = 4M\*50KB

Time to live of the data TTL - 2 years approximating to 1000days

$$
Capacity=4*10^6*50*10^3*10^3 \thickapprox 20*10^12\thickapprox 20 Terabytes
$$

So we might need the storage of 20TB approx for the next 1000 days in gradual manner.



