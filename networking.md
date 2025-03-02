# Docker Networking

Docker networking enables communication between containers, the host system, and external networks. It provides flexibility in containerized environments, allowing for efficient and isolated networking setups.

## 1. Overview of Docker Networking

Docker uses various networking modes to manage container communication. These include:

- **Bridge Network (Default)**: Containers communicate within the same host.
- **Host Network**: Containers share the host's networking namespace.
- **None Network**: No networking; the container is fully isolated.
- **Overlay Network**: Used in Docker Swarm for multi-host networking.
- **Macvlan Network**: Assigns a unique MAC address to containers, making them appear as physical devices.

## 2. Docker Network Concepts

### a. Containers and Network Namespaces
Each container has its own network namespace, which includes its own interfaces, routes, and firewall rules. Docker connects these namespaces via virtual Ethernet interfaces.

### b. Network Drivers
Docker provides built-in network drivers:
- **Bridge**: Used for container-to-container communication on a single host.
- **Host**: Removes network isolation, making the container use the host’s network.
- **Overlay**: Enables communication between containers across different hosts.
- **Macvlan**: Assigns a unique MAC address to each container.
- **None**: No networking at all.

## 3. When to Use Each Network Type

| Network Type  | When to Use |
|--------------|-------------|
| **Bridge**   | Default option for most use cases when running containers on a single host. |
| **Host**     | When you need the highest network performance and direct access to host networking (e.g., performance-sensitive applications). |
| **None**     | When you want to completely disable networking (e.g., security-sensitive workloads). |
| **Overlay**  | When running containers across multiple Docker hosts (Docker Swarm). |
| **Macvlan**  | When containers need to have their own IP address and act like real devices on the network. |

## 4. Hands-on: Docker Networking

### Step 1: List Available Networks
Run the following command to list Docker networks:
```sh
 docker network ls
```

Example output:
```
NETWORK ID          NAME                DRIVER              SCOPE
b39a3e01cf0b        bridge              bridge              local
0c0ac43c6bde        host                host                local
3e9097d3c23a        none                null                local
```

### Step 2: Create a Custom Bridge Network
```sh
 docker network create --driver bridge my_bridge_network
```

Verify the network:
```sh
 docker network inspect my_bridge_network
```

### Step 3: Run Containers on a Custom Network
Run two containers in the same custom network:
```sh
 docker run -dit --name container1 --network my_bridge_network alpine sh
 docker run -dit --name container2 --network my_bridge_network alpine sh
```

### Step 4: Test Communication Between Containers
Enter `container1` and ping `container2`:
```sh
 docker exec -it container1 sh
 ping container2
```

If successful, you should see responses from `container2`.

### Step 5: Create an Overlay Network (for Swarm Mode)
To create an overlay network, you need Docker Swarm:
```sh
 docker swarm init
 docker network create --driver overlay my_overlay_network
```

Run a service using this network:
```sh
 docker service create --name my_service --network my_overlay_network alpine ping google.com
```

### Step 6: Create a Macvlan Network
To connect containers directly to a physical network:
```sh
 docker network create -d macvlan \
   --subnet=192.168.1.0/24 \
   --gateway=192.168.1.1 \
   -o parent=eth0 my_macvlan_network
```

Run a container using this network:
```sh
 docker run -dit --name macvlan_container --network my_macvlan_network alpine sh
```

## 5. Using Docker Networking with Docker Compose

Here’s a `docker-compose.yml` file demonstrating different networks:

```yaml
version: '3.8'

networks:
  my_bridge:
    driver: bridge
  my_overlay:
    driver: overlay
  my_macvlan:
    driver: macvlan
    driver_opts:
      parent: eth0
    ipam:
      config:
        - subnet: "192.168.1.0/24"

services:
  app1:
    image: nginx
    networks:
      - my_bridge

  app2:
    image: nginx
    networks:
      - my_overlay

  app3:
    image: busybox
    networks:
      - my_macvlan
```

Run with:
```sh
 docker-compose up -d
```

## 6. Docker Network Troubleshooting

### a. Check Network Configuration
```sh
 docker network inspect <network_name>
```

### b. Verify Container Connectivity
```sh
 docker exec -it <container_name> ping <target_container>
```

### c. Restart Docker if Network Issues Persist
```sh
 sudo systemctl restart docker
```

## 7. Choosing the Right Network

| Use Case  | Recommended Network |
|-----------|----------------------|
| Single-host, multiple containers | **Bridge** |
| High-performance, direct access to host ports | **Host** |
| Securely isolate a container | **None** |
| Multi-host networking (Swarm) | **Overlay** |
| Assign container real IP address | **Macvlan** |

## 8. Best Practices for Docker Networking

- **Use custom bridge networks** instead of the default bridge for better container name resolution.
- **Avoid using host networking** unless necessary for performance reasons.
- **Use overlay networks** for services spanning multiple hosts in a Swarm environment.
- **Limit container privileges** to prevent unauthorized network access.
- **Monitor network traffic** with tools like `docker network inspect` and logging solutions.
- **Use firewalls and security groups** to restrict external access to only required services.
