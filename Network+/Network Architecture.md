# Topologies

- Star Topology: Central device connects all devices in the network. Common in large networks using switched Ethernet.
- Mesh Topology: Multiple connections between devices for redundancy. Allows load balancing and maintains connectivity in wide area networks.
- Hybrid Topology: Combination of different architectures like star, point-to-point, and mesh.
- Spine and Leaf: Individual spine switches connect to multiple leaf switches. Simplifies cabling and enhances performance in data centers (U R almost one switch away from UR end point).
- Point to Point: Direct connection between two points, commonly used in older wide area networks. Useful for connecting buildings in local area networks.

# Three-Tiered Network Design
-  Core Layer: Central point for resources like servers, applications, and databases
- Distribution Layer: Connects users to the core resources, providing redundancy
-  Access Layer: Switches close to users, allowing them to connect to the distribution layer

# Collapsed Core (Two-Tier Design)
- Simplified Design: Combines core and distribution into a single layer
- Cost Efficiency: Fewer devices required but less redundancy

# Traffic Flow
- East-West Traffic: Data flow within the same data center, typically with good response times
- North-South Traffic: Data entering or leaving the data center, Like Traffic from the internet to the data center or vice versa, requiring different security considerations