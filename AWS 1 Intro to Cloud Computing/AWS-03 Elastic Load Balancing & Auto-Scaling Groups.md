## **Scalability**
**Scalability** refers to the **ability of an application or system to handle increasing loads by adapting.**
- **Vertical Scalability:** Adding more resources (e.g., upgrading server hardware) to handle increased call volume(You upgrade a **single machine**).
  - Has a limit (you can’t upgrade forever)
  - Vertical scalability is commonly used for non-distributed systems, like databases.
  - If the machine fails → everything stops
  - No need to redesign system
  - Simple to implement
- **Horizontal Scalability:** Adding more call center agents to handle increased call volume, allowing workload distribution across multiple agents(You add **more machines**).
   - Distribute traffic using Elastic Load Balancing
   - Almost unlimited scaling
   - More fault-tolerant (if one fails, others work)
   - More complex (load balancing, synchronization)

**High Availability** ensures that **systems remain operational and accessible despite failures, focusing on minimizing downtime and ensuring continuous service availability**
High Availability often **accompanies horizontal scaling, ensuring your application or system runs in at least two Availability Zones.** 
- **Elasticity:** In a scalable system, elasticity refers to the automatic scaling based on the load, enabling a pay-per-use model, demand matching, and cost optimization.
- **Agility:** Unrelated to scalability, agility implies that new IT resources can be provisioned quickly, reducing the time to availability from weeks to minutes.
## **Elastic Load Balancing**
**Use a load balancer to:**
- Load balancers distribute traffic across multiple downstream instances.
- They provide a single point of access (DNS) to your application.
- Load balancers seamlessly handle failures of downstream instances.
- They perform regular health checks on instances.
- Load balancers offer SSL termination (HTTPS) for websites.
- They ensure high availability across zones.

### **Why use an elastic load balancer?**

- **ELB (Elastic Load Balancer) is a managed load balancer provided by AWS.**
- AWS ensures its functionality, handling upgrades, maintenance, and high availability.
- Configuration options are limited, but it reduces the effort and cost of setting up and maintaining your own load balancer.
- AWS offers four types of load balancers:
    - Application Load Balancer (ALB): For HTTP/HTTPS traffic, operates at Layer 7.
    - Network Load Balancer (NLB): For ultra-high performance, supports TCP traffic, operates at Layer 4.
    - Gateway Load Balancer: Operates at Layer 3.
    - Classic Load Balancer: Supports both Layer 4 and 7 traffic, retired in 2023
## **Auto Scaling Groups**

**The goal of an Auto Scaling Group (ASG) is to:**
- Scale out (add EC2 instances) to handle increased load.
- Scale in (remove EC2 instances) to reduce excess capacity.
- Maintain a specified minimum and maximum number of instances.
- Automatically register new instances with a load balancer.
- Replace unhealthy instances to ensure reliability.
ASGs *help to optimize capacity and reduce costs by running only the necessary number of instances.* 
### **Scaling Strategies**
- **Manual Scaling:** Adjust the size of an Auto Scaling Group (ASG) manually as needed.
- **Dynamic Scaling:** Automatically respond to changing demand with different strategies:
    - **Simple / Step Scaling:**
        - Add or remove a specific number of instances when a CloudWatch alarm is triggered (e.g., add 2 units if CPU > 70%, remove 1 unit if CPU < 30%).
    - **Target Tracking Scaling:**
        - Automatically adjust the number of instances to maintain a target metric (e.g., keep the average ASG CPU utilization around 40%).
    - **Scheduled Scaling:**
        - Plan for scaling actions based on known usage patterns (e.g., increase the minimum capacity to 10 at 5 pm on Fridays).
    - **Predictive Scaling:**
        - Utilizes Machine Learning to forecast future traffic.
        - Automatically provisions the appropriate number of EC2 instances in advance to meet predicted demand.