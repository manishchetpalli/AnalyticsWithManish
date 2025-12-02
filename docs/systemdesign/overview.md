### **System Design Using the Restaurant Analogy**

When opening a new restaurant the initial challenge arises when one chef cannot handle all the orders from new customers.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*


### **Scaling the Resource: Vertical Scaling and Optimization**

The goal is to optimize processes and increase throughput using the same resource (the chef/computer).

   ***Vertical Scaling***: Thinking like a manager, the first step is to ask the chef (analogous to a computer) to work harder by being paid more, leading to more output. This is termed vertical scaling in technical terms.

   ***Process Optimization***: Efficiency can be improved by doing things beforehand. For example, the pizza paste does not need to be made when an order comes in; it can be pre-made.
   
   ***Non-Peak Hours***: Pre-preparation should happen during non-peak hours. Doing this around 4:00 AM is good because there won't surely be any pizza orders, preventing the chef from being busy making pizza bases when a regular order comes in.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*

### **Ensuring Resilience: Avoiding Single Points of Failure**

The initial single-chef setup is not resilient.

   ***Single Point of Failure (SPOF)***: If the chef calls sick, the business faces trouble because this person is a single point of failure.
   
   ***Backups***: To address this, a backup chef should be hired in case the primary chef does not come in.
   
   ***Master-Slave Architecture***: Keeping backups and avoiding SPOFs maps, for computers, to a master-slave architecture. Hiring a backup significantly reduces the chance of losing business.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*

### **Expanding Capacity: Horizontal Scaling**

If the business continues to grow, more resources must be added.

   ***Hiring More Resources***: The backup chef should be made full-time, and more chefs should be hired (e.g., 10 chefs plus a few in backup).
   
   ***Horizontal Scaling***: This process maps to horizontal scaling, which involves buying more machines of similar types to get more work done.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*
### **Specialization and Microservices Architecture**

When multiple resources are available, efficiency is achieved through specialization.

   ***Inefficient Routing***: Simply assigning orders randomly (e.g., sending garlic bread to a chef specializing in pizza) is not the most efficient way to use employees.
   
   ***Routing by Strength***: The optimal approach is to route orders based on strengths. If chefs 1 and 3 are pizza experts and chef 2 specializes in garlic bread, all garlic bread orders go to chef 2, and pizza orders go to chefs 1 and 3.
   
   ***Benefits of Specialization***: This routing simplifies the system. For example, changing the garlic bread recipe only requires notifying chef 2, and chef 2 is the sole person to ask for the status of garlic bread orders.
   
   ***Microservice Architecture***: Creating specialist teams (e.g., scaling the pizza team differently than the garlic bread team) and dividing responsibilities is known as microservice architecture. In this architecture, responsibilities are well-defined and stay within the business use case. This structure makes the business highly scalable as specialists can be scaled easily.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*

### **Geographical Distribution and Fault Tolerance**

Even a successful, scalable pizza shop (a single location) is vulnerable to local failure (e.g., electricity outage or loss of license).

   ***Distribution***: To achieve greater fault tolerance, one must distribute the system; not putting all resources in one basket or even one shop.
   
   ***Distributed System***: The solution is to buy a separate shop in a different place. This is a major step that introduces complexity, particularly concerning communication and routing requests between the shops. This defines a distributed system.
   
   ***Quicker Response Times***: A clear advantage of distribution is that orders local to a specific shop's range can be served by that shop, leading to quicker response times. Large distributed systems like Facebook use local servers worldwide for this reason. Distribution makes the system more fault tolerant.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*
### **Intelligent Routing: Load Balancing**

When multiple shops exist, a decision must be made about where to send a customer's request.

   ***Central Authority***: Customers should not bear the responsibility of routing requests; a central place or authority is needed to route them.
   
   ***Routing Parameter***: The routing should not be random. The primary parameter is the total time it takes for the customer to get the pizza.
   
   ***Intelligent Decisions***: The central authority needs real-time updates to make intelligent business decisions (leading to more money). For example, if Pizza Shop Two takes 1 hour 5 minutes total and Shop One takes 1 hour 15 minutes, the request should go to Shop Two.
   
   ***Load Balancer***: The mechanism that routes requests in a smart way is called a load balancer.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*
### **Flexibility and Extensibility: Decoupling**

To make the system flexible to change, responsibilities must be separated.

   ***Separation of Concerns***: The delivery agent and the pizza shop have little in common. The delivery agent only cares about quickly delivering goods, and the shop doesn't care if the goods are picked up by an agent or the customer.
   
   ***Decoupling***: This requires separating out concerns, such as having different managers for the pizza shop and the delivery agents. This is called decoupling the system, which allows separate systems to be handled more efficiently.
   
   ***Metrics and Logging***: To track efficiency and diagnose issues (like a faulty oven or bike), it is crucial to log everything (what happened and when). Events must be condensed to find sense, which results in metrics.
   
   ***Extensibility***: Decoupling ensures the system remains extensible. For example, the delivery agent service doesn't need to know if it is delivering a pizza or a burger. This ability to decouple allows businesses (like Amazon) to scale out their offerings.

*-------------------------------------------------------------------------------------------------------------------------------------------------------------*
### **High Level vs. Low Level System Design**

The process of finding technical solutions by mapping them from a business scenario is integral to system design.

   ***High Level Design (HLD)***: This is the high-level solution developed by scaling the restaurant. HLD focuses on topics like deploying on servers and determining how two systems interact with each other.
   
   ***Low Level Design (LLD)***: The counterpart to HLD, LLD focuses more on how to actually code the stuff. This includes making classes, objects, functions, and signatures. Knowledge of LLD is crucial for senior engineers to write efficient and clean code.