<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/685792cf-9cfa-48aa-83ce-a8c6efdcb8a0" />


High-Availability Docker Swarm Cluster on Virtualized Infrastructure
This repository documents the implementation of a distributed microservice architecture using Docker Swarm across multiple virtualized nodes. The project demonstrates advanced networking, container orchestration, and high-availability deployment strategies.

🚀 Deployment Strategy: High-Availability Swarm
The strategy employed here utilizes a Multi-Node Orchestration approach. By clustering independent virtual machines, we ensure that if one node fails, the microservice remains accessible, providing fault tolerance and seamless load balancing.

🏗️ Architecture Design
The infrastructure consists of a Manager-Worker relationship established over a virtual bridged network.

Manager Node: Orchestrates the cluster, maintains the desired state, and manages service discovery.

Worker Node: Executes the containerized workloads as directed by the Manager.

Networking: Each node is assigned a unique IP on the local subnet via a Bridged Adapter (as seen in image), allowing direct communication between the Windows Host and the VMs.
Component,Detail
Virtualization,Oracle VM VirtualBox 7.x
Operating System,Ubuntu 24.04 LTS
Orchestrator,Docker Swarm
Manager IP,192.168.1.110 (refer to image)
Worker IP,192.168.1.111 (refer to image)
📋 Implementation Steps
1. Networking Setup
Configured Adapter 2 on both VMs to Bridged Adapter with "Promiscuous Mode: Allow All" to ensure the VMs could communicate on the physical network.

2. Cluster Initialization
The cluster was initialized on the Manager node:

Bash
docker swarm init --advertise-addr 192.168.1.110
3. Joining the Worker
The Worker node was integrated into the cluster using the generated join token:

Bash
docker swarm join --token <SWMTKN_TOKEN> 192.168.1.110:2377
4. Microservice Deployment
A RESTful API service was deployed with 4 replicas to ensure high availability across both nodes:

Bash
docker service create --name my-api --replicas 4 --publish 80:80 traefik/whoami
📊 Verification
Cluster Status: Verified using docker node ls, showing both nodes in a Ready state.

Load Balancing: Demonstrated by hitting the service endpoint; the response Hostname cycles through different container IDs, proving traffic distribution.

Visualization: Implemented a visualizer tool to monitor container placement in real-time.
<img width="1902" height="927" alt="image" src="https://github.com/user-attachments/assets/147d1cf1-2c71-4e65-8ef3-b3394b96b3a2" />

## 🛠️ Source Code & Configuration
The deployment configuration for this cluster is managed via Docker Stack. 

* **Deployment Manifest:** [docker-compose.yml](./docker-compose.yml)
* **Microservice Image:** [traefik/whoami](https://hub.docker.com/r/traefik/whoami) (RESTful API)

### To deploy this configuration:
1. Ensure your Swarm cluster is active.
2. Run the following command on the Manager node:
   ```bash
   docker stack deploy -c docker-compose.yml my-stack
### 📺 Project Walkthrough
Watch the deployment and load balancing demonstration here:
[Click to Watch Video](https://www.youtube.com/watch?v=hJ5kw9-YYDk)
