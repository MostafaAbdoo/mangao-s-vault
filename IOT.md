# Lecture 1
IoT means interconnection of physical components with the ability to connect and
exchange data, without human interaction
- **Pre-Internet:** Human-to-human communication via fixed telephony and SMS.
- **Internet of Content:** The World Wide Web (WWW) focused on email, information retrieval, and entertainment.
- **Internet of Services:** Web 2.0 enabling e-commerce and digital productivity platforms.
- **Internet of People:** Social media networks (Facebook, Skype, YouTube) connecting human experiences.
- **Internet of Things:** Machine-to-machine communication driving automation, tracking, metering, and contextual adaptation.
Physical Object + Controller, Sensor and Actuator + Internet = Internet-of-Things
- *Ambient Intelligence (AmI)* -> Electronic environments that are sensitive and responsive to the presence of people
- *Machine-to-Machine (M2M)* -> A concept which refers to direct communication between devices using any communications channel, including wired and wireless
- *Digital Twins* -> Each physical system has a digital simulation twin that can simulate real-time sensor data that enters to the physical system and generates recommendations
*IoT 4s’s Rule*: Simple, Secure, Smart, Scalable
### Level-1
- single node/device that performs sensing and/or actuation, stores data, performs analysis and hosts the application
- for low-cost and low-complexity solutions 
### Level-2
- single node that performs sensing and/or actuation and local analysis
- Data is stored in the cloud
- when data involved is big, and not computationally intensive
### Level-3
- single node for sensing and/or actuation
- Data is stored and analyzed in the cloud
- when data involved is big and the analysis requirements are computationally intensive.
### Level-4
- multiple nodes that perform local analysis
- Data is stored in the cloud
- contains local and cloud-based observer nodes which can subscribe to and receive information collected in the cloud from IoT devices.
- where multiple nodes are required, the data involved is big and not computationally intensive.
### Level-5
- Data is stored and analyzed in the cloud
- multiple end nodes and one coordinator node
- Coordinator node collects data from the end nodes and sends to the cloud
- for solutions based on wireless sensor networks, in which the data involved is big and computationally intensive
### Level-6
- multiple independent end nodes that perform sensing and/or actuation and send data to the cloud
- Data is stored in the cloud 
- The analytics component analyzes the data and stores the results in the cloud
- The centralized controller is aware of the status of all the end nodes and sends control commands to the nodes.
# Lecture 2
sensors that acquire information for the system and actuators that do actions in response
Transducer terminology is used for both sensors and actuators. It means a device that converts energy form to another
**Sensor** converts the physical quantities into an electrical signal.
**Actuator** converts electrical signal into mechanical work
sensor -> control center -> actuator
- Each sensor can only measure a unique property.
- **Infrared (IR) Sensors:** Measure temperature-sensitive properties via infrared radiation without direct physical contact, avoiding friction while offering high sensitivity and fast response.
- **Proximity Sensors (LiDAR vs. RADAR):** Both are Proximity Sensors, Detect nearby objects without physical contact. **LiDAR** emits rapid laser light pulses (up to 150,000 pulses per second) and calculates object distance and size based on bounce-back time. **RADAR** works similarly but uses **radio waves**, which suffer less absorption when contacting objects and can operate over much longer distances.
- **Accelerometers:** Measure directional acceleration, allowing systems to deduce changes in orientation or position.
- **Pressure Sensors**: Detect forces per unit area in gases or liquids, also in touch screens
### Sensors’ Requirements
- Accuracy: describes the amount of uncertainty that exists in a measurement with respect to the relevant absolute standard.
- Resolution: describes the degree to which a change can be detected.
- Sensitivity: describes the smallest absolute amount of change that can be detected by a measurement.
Sensitivity is an absolute quantity; resolution is a relative quantity.

**Hydraulic Actuators**
hat are used on linear or quarter-turn valves. They are designed based on Pascal’s law: When there is an increase in pressure at any point in a confined incompressible fluid, then there is an equal increase at every point in the container.
**Pneumatic Actuators**
Pneumatic actuators work on the same concept as hydraulic actuators except compressed gas are used instead of liquid.
**Electrical Actuators**
devices driven by small motors that convert energy to mechanical torque.
**Mechanical Actuators**
convert rotary motion to linear motion, Devices such as screws and chains are utilized in this conversion.

- **Transistor Reduction & Die Size:** Minimizing transistor count shrinks physical chip size and unit production cost. However, as silicon geometries shrink, **leakage power** can dominate total power consumption.
- **Time-to-Market:** How quickly a product can be launched. High market pressure often forces developers to choose flexible, off-the-shelf platforms, even if unit costs are higher.
- **Nonrecurring Engineering (NRE) Costs:** The one-time practical cost of researching, designing, and testing a custom hardware platform.

**Application-Specific Integrated Circuit (ASIC)**
- Custom silicon chips manufactured for one specific task.
- **Advantages:** Unmatched performance, minimal physical area, lowest possible power consumption, and extremely cheap per unit when produced in mass quantities.
- **Disadvantages:** Huge upfront NRE development costs, very long time-to-market, and **zero flexibility** once fabricated.
**Field Programmable Gate Array (FPGA)**
- Reconfigurable logic chips that allow physical hardware circuits to be reprogrammed after manufacturing.
- **Advantages:** Excellent hardware flexibility, significantly lower NRE costs, and faster time-to-market compared to ASICs.
- **Disadvantages:** High unit cost per chip and heavy power consumption, making them challenging for battery-powered IoT nodes.
**General-Purpose Microprocessors & Microcontrollers**
- Standard software-programmable generic chips (e.g., ARM-based microprocessors, microcontrollers).
- **Advantages:** Rapid development cycle, inexpensive off-the-shelf availability, and full support for high-level languages like Python, Java, and C++.
- **Disadvantages:** Executes code sequentially, relies on static hardware layouts, and requires higher clock speeds (which consumes more power) to boost execution performance.

- **Arduino (e.g., Uno, Yun):** A microcontroller board built for low-power, direct hardware control. It directly interfaces with analog and digital sensor pins but has limited onboard computational memory.
- **Raspberry Pi:** A full single-board computer featuring a microprocessor, dedicated memory, display ports, and network interfaces. While far more powerful for multimedia processing or acting as an IoT edge gateway, it incurs higher power consumption and higher per-unit costs.
![[Pasted image 20261112064032.png]]
![[Pasted image 20261112064148.png]]
$2^s \ge \text{number of required subnets}$
$2^h - 2 \ge \text{hosts}$
**Block Size (Increment):** $256 - 224 = \mathbf{32}$

- **Bluetooth**: Operating at **2.4 GHz**, standard Bluetooth establishes a **master-slave** configuration supporting up to **seven slave devices**. It excels at short range (<100 m) for low-volume data transfer.
- **Bluetooth Low Energy (BLE)**: Branded as Bluetooth Smart, BLE maintains a similar range to standard Bluetooth but consumes dramatically less power, making it ideal for wearables.
- **Zigbee**: Widely deployed in **industrial** control networks, Zigbee offers low-power, robust, and highly secure multi-channel operation that is significantly more economical than Wi-Fi or standard Bluetooth.
- **Z-Wave**: Tailored specifically for **home automation** (e.g., smart lighting, lock controls), Z-Wave uses a simpler protocol operating in a scalable full mesh topology supporting up to 232 devices without needing a central coordinator node.
- **Near-Field Communication (NFC)**: Operating at **13.56 MHz** with a typical range of just **4 cm**, NFC leverages RFID principles for secure, close-proximity two-way transactions.
- **Cellular (3G/4G/5G)**: Essential for long-distance mobility. While cellular networks offer huge bandwidth, their higher power consumption and operational costs mean they are best suited either for powered systems or sensor projects that transmit small bursts of data over the Internet.
- **LoRaWAN (Long Range Wide Area Network)**: Designed explicitly for low-power WAN applications such as **smart street lighting** and smart cities, LoRaWAN provides low-cost, secure, long-range mobility across wide territories.
### **1. Request-Response Model**
In the classic **Request-Response** model, a client sends an explicit request to a server, and the server fetches data, processes the request, and returns a response. **REST APIs** built on HTTP strictly follow this paradigm. While intuitive, it tightly couples client and server in a 1-to-1 relationship, which can create processing bottlenecks when scaling to thousands of concurrent sensors.
### **2. Publish-Subscribe Model**
The **Publish-Subscribe** model breaks the direct link between data sources and data sinks using an intermediary called a **broker**:
- **Publishers** produce data and send it to specific **topics** managed by the broker.
- **Consumers** subscribe to topics of interest.
- The **broker** automatically routes incoming published data to all subscribed consumers.
Because publishers and consumers remain completely unaware of each other, this model offers outstanding scalability for dynamic IoT deployments.
### **3. Push-Pull Model**
In the **Push-Pull** model, data producers **push** messages into message **queues**, while consumers **pull** data from those queues when ready.
Queues serve as a crucial **buffer** when there is a speed mismatch—for instance, when high-frequency sensor nodes generate data faster than backend application servers can process it. Queues decouple the components and prevent data loss during traffic spikes.
### **4. Exclusive Pair Model**
When applications require real-time, continuous streaming, the **Exclusive Pair** model provides a full-duplex, bidirectional connection. The client issues a request to establish a persistent connection with the server. Once opened, both parties can freely transmit data frames back and forth until a closing request is issued. **WebSockets** utilize this model to deliver low-latency communications.

### **Message Queue Telemetry Transport (MQTT)**
Engineered specifically for low-bandwidth, unreliable networks, **MQTT** is a **lightweight**, TCP-based protocol operating on the **Publish-Subscribe** model. An MQTT network consists of subscribers, publishers, and a central **broker** handling authentication and security.
To manage reliability, MQTT defines three distinct **Quality of Service (QoS)** levels:
- **QoS Level 0 (At most once)**: Messages are sent with no guarantee of delivery or acknowledgement. Lowest overhead.
- **QoS Level 1 (At least once)**: Delivery is guaranteed, but duplicate messages may arrive.
- **QoS Level 2 (Exactly once)**: Delivery is fully guaranteed with zero duplicate messages. Highest reliability and overhead.

### **Constrained Application Protocol (CoAP)**
**CoAP** brings web-style RESTful architecture (GET, POST, PUT, DELETE) to hyper-constrained, low-bandwidth networks. Unlike HTTP, CoAP runs over User Datagram Protocol (**UDP**), drastically reducing header overhead and connection setup delays. It operates on a 1-to-1 client/server model optimized for simple, sleeping sensor nodes in congested environments

- **Functional Testing:** Validates that the application behaves correctly according to specification.
- **Connectivity Testing:** Evaluates performance under weak wireless signals or heavy device congestion.
- **Performance & Stress Testing:** Determines how many simultaneous connections a device or server can support.
- **Security Testing:** Verifies data privacy, authorization, and authentication mechanisms.
- **Compatibility & Exploratory Testing:** Ensures protocol interoperability and smooth user experiences across device configurations.

- **Infrastructure as a Service (IaaS):** The most fundamental cloud model. The cloud provider manages physical hardware, servers, storage, networking, and virtualization. You manage the operating system, middleware, runtime environment, data, and applications. Providers bill on a utility consumption basis. _Examples:_ Google Compute Engine, Amazon Web Services (AWS), Microsoft Azure, and VMware.
- **Platform as a Service (PaaS):** The provider manages everything in IaaS plus the operating system, middleware, programming execution environment, database engine, and web server. Application developers build and run software without worrying about server provisioning or OS maintenance. _Examples:_ Microsoft Azure PaaS, Cloud Foundry, and Red Hat OpenShift.
- **Software as a Service (SaaS):** The cloud provider installs, hosts, and operates the complete application software. End-users access the software via web or client applications, typically paying a scalable monthly or annual subscription fee per user. _Examples:_ Google Workspace, Salesforce CRM, Slack, Trello, and Microsoft Office 365.

Characteristics of IoT Data:
• High velocity
• Massive volume
• High variety (structured metrics, semi-structured logs,
unstructured binary streams)
• Spatial-temporal dependencies.

- **Time-Series Databases (TSDBs):** Platforms like _InfluxDB_, _TimescaleDB_, and _Prometheus_ are purpose-built for high-throughput metric ingestion, down-sampling, and rapid time-window queries.
- **NoSQL Distributed Stores:** Engines like _Apache Cassandra_ and _MongoDB_ offer schema flexibility and horizontal partitioning across clusters to accommodate dynamic payloads.
- **Object Storage:** Scalable storage systems like _AWS S3_, _Google Cloud Storage_, and _Azure Blob Storage_ serve as long-term repositories for unstructured binary payloads, firmware images, and historical archives.

- **Real-Time Stream Processing:** Technologies like _Apache Kafka_, _Apache Flink_, and _Spark Streaming_ evaluate event data continuously in motion to generate instant alerts and trigger automated actuators.
- **Batch Processing:** Frameworks like _Hadoop MapReduce_ and _Spark Batch_ analyze stored historical datasets to uncover long-term trends and retrain machine learning models.

In advanced industrial IoT environments, analytics systems leverage **Edge-Cloud Hybrid Intelligence**. Resource-intensive machine learning models (such as autoencoders or complex regression networks) are trained on massive historical data within high-power cloud environments. The optimized, lightweight models are then deployed directly back to edge gateways via **TinyML** for low-latency inference and anomaly detection.

Descriptive Analytics:
• Find an answer to question “what happened?”, generate reports or visualize the data.
Diagnostic Analytics:
• Find an answer to question “why did it happen?”, find correlations.
Predictive Analytics:
• Find an answer to question “what will happen?”, it uses machine learning.
Prescriptive Analytics:
• Find an answer to question “how can we make it happen?”, it uses more complicated machine learning algorithms to find recommendation

The main difference between data analysis and analytics lies in their approach, as
analysis looks towards the past while analytics towards the future.
• Data analysis is the process of studying a given data set (in close detail), dividing
them into small components, and studying the subcomponents individually and
their relationship with each another.
• Data analytics, on the other hand, is a more comprehensive term referring to a
discipline that comprises the complete management of data, including collection,
cleaning, organizing, storing, administering, and analysis of data with the help of
specialized tools and techniques.
• In other words, data analysis is a process or method, whereas data analytics is an
overarching discipline (science).