
### **The Problem Consistent Hashing Solves**

The limitation of simple hashing (as discussed in the conversation history) is not merely load balancing, but the management of servers. The key challenge is that adding and removing servers completely changes the local data stored in each server.

---

### **Introducing the Consistent Hashing Ring Architecture**

To overcome the issues caused by changing the modulo base (N), consistent hashing uses a circular structure called a ring.

Instead of an array, the architecture uses a ring that contains hash positions ranging from 0 up to $M-1$. The value $M-1$ wraps around to 0, forming the circle. This ring represents the entire possible search space for the hash function.

---

### **Mapping Requests and Servers to the Ring**

Both requests and servers are mapped onto this hash ring using hash functions.

***Request Mapping***

Requests still have IDs, and these request IDs are hashed.
The hash function output (a number between 0 and $M-1$) maps the request to a specific point on the ring. Multiple requests can be mapped as points around the ring.

***Server Mapping***

Servers also have IDs (e.g., 0, 1, 2, 3, 4).

These server IDs are also hashed using the same or a different hash function.

The hash result is taken modulo $M$ (the search space size) to place the server onto a specific position on the ring.

!!! Example
    If hashing server ID 0 results in 49, and $M$ is 30, $49 \text{ mod } 30 = 19$. Server 1 would be mapped to hash position 19.
    In the example architecture, four servers (SS1, SS2, SS3, SS4) are mapped to different points on the ring.

***Request Assignment (The Clockwise Rule)***

Once both requests and servers are placed on the ring, the assignment follows a simple algorithm.

When a request arrives at its hashed position, the system looks clockwise around the ring.

The request is served by the nearest server encountered in the clockwise direction.

!!! Example
    Requests are mapped to S1, S2, S3, or S4 based on which server is nearest clockwise to their hashed location.

---

### **Load Distribution and Efficiency**

***Theoretical Uniform Load***

The expectation is that the hashes (for both requests and servers) are uniformly random.

Due to uniform randomness, the distance between server points on the ring is also expected to be uniform.

Consequently, the load (the number of requests mapped between servers) is expected to be uniform.

The expected load factor for each server remains $1/N$ (where N is the number of servers), which was also true for simple hashing.

***Minimum Change When Scaling (Adding/Removing Servers)***

The critical advantage of the ring architecture is maintaining minimum change when the number of servers changes.

If a fifth server (SS5) is added to the ring, it is mapped to a new point. This new server only takes load from the server immediately counter-clockwise to its position.

!!! Example
    If SS4 is added, it sits between two existing servers. Any requests that previously mapped clockwise to SS3, but now map clockwise to the new SS4, are reassigned. SS4 gains load, and SS3's load decreases.
    
    In this scenario, only S3 is affected; S1, S2, and S4 are not affected. The change in load for each existing server is "much less" than what occurred with simple hashing.

If a server, like S1, crashes and is removed, its load is shifted to the next clockwise server (e.g., SS4). The load is absorbed only by the nearest successor server on the ring.

The system is designed to achieve the minimum theoretical change when adding or removing servers.

---

### **Practical Challenge: Skewed Distribution and Virtual Servers**

While consistent hashing promises minimum change, a practical problem arises when the number of servers is small (e.g., four servers).

Even with the ring architecture, having few servers means the distribution of requests can become skewed, potentially resulting in half of the total load falling on a single server, which is "terrible". 

Theoretically, the load should be $1/N$, but practically, it can be uneven. This happens because the server points might not be evenly spaced if there are not enough servers.

***The Solution: Virtual Servers (Replicas)***

To solve the load skew and guarantee uniform distribution, system design engineers use the concept of virtual servers.

Virtual servers do not involve buying more expensive physical servers or virtual boxes.
Instead, they involve using multiple hash functions ($K$ hash functions).

Each server ID is passed through $K$ different hash functions ($H_1, H_2, \dots H_K$). This results in $K$ points being mapped onto the ring for a single physical server.

!!! Example
    If $K=3$ and there are four physical servers, the ring will have $4 \times 3 = 12$ server points.
    By appropriately choosing the value of $K$ (e.g., $\log N$ or $\log M$), the likelihood of a skewed load on one server is dramatically reduced, making the distribution much more even.

***Behavior with Virtual Servers***

When a server is added, it adds $K$ points to the ring, taking small amounts of load from multiple existing servers.

When a server is removed, $K$ points are removed. The load that those $K$ points served is then reassigned, increasing the load "expected uniformly" across multiple neighboring servers.

The use of virtual servers ensures the system is efficient and maintains the expected minimum change.

### **Applications of Consistent Hashing**

Consistent hashing is a fundamental concept used extensively in distributed systems and scaling.

It provides flexibility and efficient load balancing.

It is used by web caches.

It is used by databases.

---

### **Analogy**

Consistent hashing with virtual servers can be visualized as dividing a large circular pizza (the total load) equally among chefs (servers). If you only have four chefs, one might accidentally get a disproportionately large slice. By using virtual servers, you effectively slice each chef into several 'mini-chefs' spread around the edge of the pizza. If the pizza is cut based on these numerous 'mini-chef' markers (virtual points), the total share (load) collected by any one physical chef (server) is guaranteed to be nearly identical, ensuring true load balance even when a new chef is added or an old one leaves.