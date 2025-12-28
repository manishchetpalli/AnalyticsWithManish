### **Initial System Setup and Deployment**

   ***Core Functionality***: System design begins with an algorithm (code) running on a computer. This code functions like a normal function, taking input and providing an output.
   
   ***Monetization and Access***: If the code is useful, people may pay to use it, but the developer cannot give their physical computer to everyone.
   
   ***Exposing the Code***: The code is exposed using a protocol that runs on the internet, typically through an API (Application Programmable Interface).
   
   ***Communication Flow***:
       The user sends a request to the system.
       The code runs, giving an output that is returned as a response.
   
   ***Local Infrastructure Needs***: Setting up this computer might require configuring endpoints and connecting to a database (even if the database resides within the desktop itself).
   
   ***Reliability and Cloud Hosting***: To ensure the service does not go down (e.g., due to power loss or someone pulling the plug), the service should be hosted on the cloud.

---
### **Understanding the Cloud**

   The cloud is fundamentally a set of computers that a provider (like Amazon Web Services - AWS, the most popular example) offers for money.
   
   Cloud providers give computation power—essentially a desktop they have somewhere else that can run the algorithm.
   
   Developers can store and run their algorithm often by using a remote login into that computer.
   
   Using the cloud allows the developer to focus on business requirements because the cloud solution providers handle the configuration, settings, and reliability to a large extent.

---

### **The Need for Scalability**

When many users begin using the algorithm, the single machine running the code may no longer be able to handle all the incoming connections.
   
Scalability is the ability to handle more requests. This is achieved by increasing resources, often summarized as "throwing more money at the problem".
   
Two Solutions for Scaling:

   1.  Buy a bigger machine.
   2.  Buy more machines.

---

### **Mechanisms of Scaling**

The two scaling solutions correspond to two key mechanisms:

***Vertical Scaling (Buying Bigger Machines)***

   The computer is made larger, allowing it to process requests faster. Communication between components uses interprocess communication (IPC), which is quite fast.
   Because all data resides on one system, the data is consistent.

***Horizontal Scaling (Buying More Machines)***

   Requests are distributed randomly among multiple machines. To process more requests overall because there are more resources available. The system is resilient; if one machine fails, requests can be redirected to the other machines.This scales well; the number of servers thrown at the problem scales almost linearly with the number of users added, overcoming hardware limitations inherent in vertical scaling.

---

### **Detailed Comparison of Vertical vs. Horizontal Scaling**

| Feature | Horizontal Scaling (More Machines) | Vertical Scaling (Bigger Machine) |
| :--- | :--- | :--- |
| Load Balancing | Required (to distribute requests among machines). | Not required (single machine). |
| Failure/Resilience | Resilient (failure of one machine allows redirection). | Single point of failure (SPOF). |
| Communication | Slow: Uses network calls/IO, specifically Remote Procedure Calls (RPC) between services. | Fast: Uses interprocess communication (IPC). |
| Data Consistency | Real Issue: Difficult to maintain complex data integrity (e.g., atomic transactions may require impractical locking of all servers/databases). Requires some form of loose transactional guarantee. | Consistent: All data resides on a single system. |
| Hardware Limits | Scales well: The amount of servers can be increased almost linearly. | Limited: Subject to hardware limitations; the computer cannot be made infinitely big.

---

### **Real-World Approach and System Design Trade-offs**

   In the real world, both vertical and horizontal scaling techniques are used. The hybrid solution is essentially horizontal scaling, but each individual machine used is as large a box as is financially and technically feasible.
   
   ***From Vertical Scaling***: Fast interprocess communication and data consistency (e.g., consistent cache, no dirty reads/writes).
   
   ***From Horizontal Scaling***: Resilience (if one server crashes, others take over) and the ability to scale well beyond hardware limits.
   
   Initially, a system may utilize vertical scaling as much as possible; later, as the user base grows and trusts the service, scaling should shift toward horizontal scaling.
   
   System design is the process of making trade-offs to build a system that meets requirements, focusing primarily on scalability, resilience, and consistency.


---

### **Analogy**

Thinking about scaling a computer system is like managing a pizza kitchen. Vertical scaling is buying a single, massive, state-of-the-art oven—it cooks pizzas incredibly fast and keeps all your ingredients in one place (consistent data), but if the oven breaks, you can't make any pizza (single point of failure). 

Horizontal scaling is buying many smaller, cheaper ovens—if one oven breaks, the others keep cooking (resilience), and you can add new ovens indefinitely as demand grows, but coordinating the ingredients and timing across all those separate ovens (data consistency and network communication) becomes much harder. 

The best solution is often a hybrid: buying several very large, high-quality ovens (horizontal scaling of highly vertically scaled nodes).