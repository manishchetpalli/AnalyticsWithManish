
### **Introduction to Asynchronous Processing and Queues**

***Handling Orders in a Pizza Shop***

The shop continues taking new orders even while pizzas are being made. Clients do not receive the final product (the pizza) immediately. Clients are relieved from expecting an immediate response by being given a confirmation (e.g., "Please sit down" or "Can you come back after sometime?") that the order has been placed.

***The List/Queue***

The shop maintains a list or queue that tracks orders (e.g., order no. 1, order no. 2, etc.).As new orders come in, they are added to this queue.When a pizza is done, it is removed from this queue.The client pays and is "entirely relieved".

***Asynchronous Nature***

The entire process is asynchronous, meaning the client did not have to wait for the final response (the pizza or the payment transaction).

Client Benefit: The client is able to do other tasks during the wait time (e.g., checking their phone), allowing the client to be happier and distribute their resources elsewhere.
    
Server/Maker Benefit: The pizza maker (or server) can order tasks according to priority. Tasks that are very easy (like filling a coke can) or those that need to be made immediately can be prioritized.
    
This manipulation of the queue based on priority allows clients to spend time more judiciously.

---

### **Challenges of Scaling and Persistence**

The architecture becomes complex when the system scales, such as having multiple pizza shop outlets (e.g., Pizza Shop No. 1, No. 2, No. 3, similar to Dominos).

***Handling Shop Failure (Power Outage)***

If a shop goes down (e.g., power outage).Takeaway orders might be dumped. Delivery orders can potentially be sent to the other operational shops to complete the work. If Pizza Shop No. 3 crashes, its clients need to be connected to the remaining shops, and their orders must be rerouted.

Maintaining the list of orders solely in memory won't work because if the shop loses electricity, the computer shuts down, and the data is lost.

Therefore, the system requires persistence in its data, which means the list must be stored in a database.


---

### **The Complicated Architecture: Servers, Database, and Failure Management**

A more complex system includes a set of servers (e.g., 1 to 4) and a database storing the list of all orders (Order ID, contents, and completion status).

***Rerouting Orders Upon Server Crash***

If a server (e.g., S3) handling specific orders (e.g., 9 and 11) crashes, those orders need to be rerouted.

Initial Solution Idea (Too Complicated): One approach is to have the database note which server ID is handling the order, and then check the database when a server fails.

A better method involves a notifier checking for a heartbeat from each server (e.g., every 15 seconds).

If a server does not respond, the notifier assumes it is dead.The notifier then queries the database to find all orders that are not done and distributes them to the remaining servers.

***The Duplication Problem***

Rerouting failed orders introduces the risk of duplication.

If an order (e.g., Order No. 3) is picked up by the database query for reassignment (because its original server crashed), but Server No. 2 is already processing Order No. 3, the duplicated assignment means both Server 1 and Server 2 will end up making the pizza.

This results in a "big loss and lots of confusion".

***Solution: Load Balancing and Consistent Hashing***

To prevent duplication and manage reassignment efficiently, the system needs load balancing.

Load Balancing Principles: Load balancing is defined as sending the right amount of load to each server, but crucially, its principles ensure no duplicate requests are sent to the same server.

Consistent hashing (a technique described in the conversation history) can be used to eliminate duplicates and balance the load.The principle ensures that Server S1 handles one set of "buckets" and S2 handles another.

When a server crashes, the remaining servers (e.g., S1 and S2) do not lose their current buckets; they only receive new buckets that were previously handled by the crashed server.

!!! Example
    if Order 3 belonged to S2, it continues to belong to S2, preventing duplication when the notifier reroutes failed orders.

---

### **Message Queues as the Encapsulation of Complexity**

The need for assignment, notification, load balancing, a heartbeat mechanism, and persistence leads directly to the concept of a message queue.

***Definition***

The message queue (or task queue in this context) encapsulates all this complexity into one thing.

***Functionality***

A task queue:

1.  Takes tasks (orders).

2.  Persists them (stores them permanently).

3.  Assigns them to the correct server.

4.  Waits for the server to complete the task.

5.  If a server takes too long to acknowledge completion, the queue assumes the server is dead and reassigns the task to the next server.

***Importance***

Task queues are an important concept in system design for handling work and encapsulating complexity.

!!! Example 
    RabbitMQ, Java Messaging Service (JMS), and zeroMQ (a library that allows one to write a messaging queue easily). Amazon also offers messaging queues.
