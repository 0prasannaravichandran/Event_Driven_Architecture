Code & Container Build (GitHub Codespaces):

    Write 3 tiny microservices (in Python, Node.js, or Go):
    
    order-service (Producer): Accepts an HTTP POST /order, saves it to PostgreSQL, and publishes an OrderPlaced event to Kafka.
    
    logistics-service (Consumer): Polls the Kafka orders topic, reads live rider locations from Redis, and assigns a rider.
    
    notification-service (Consumer): Uses a different Kafka Consumer Group to read the same OrderPlaced event and log a confirmation message.
    
    Write a Dockerfile for each service and push the built images to Docker Hub or GitHub Container Registry (ghcr.io) directly from Codespaces.

Event Backbone & Databases (Aiven Cloud or In-Cluster):

    Spin up a free Aiven for Apache Kafka instance (create an orders topic with 3 partitions) and a free PostgreSQL + Redis instance.
    
    Orchestration & Scaling (K3s / Kubernetes in Codespaces or Killercoda):
    
    Start a lightweight single-command cluster inside your browser terminal using k3d (which runs K3s inside Docker).
    
    Apply your Kubernetes Deployment YAML files to spin up your 3 microservice containers.

3. Three Hands-On Experiments to Try Once It's Running
    Once your mini-system is live in the browser, run these three experiments to see the enterprise patterns in action:
    
    Watch Kafka Consumer Rebalancing: Start with 1 replica of logistics-service reading all 3 Kafka partitions. Run kubectl scale deployment logistics-service --replicas=3 and watch the logs—you will immediately see Kafka rebalance so each container handles exactly 1 partition.
    
    Test Kubernetes Self-Healing: Run kubectl delete pod <order-service-pod-name> to forcefully kill a running container, then watch kubectl get pods -w as Kubernetes automatically creates a healthy replacement within seconds.

Simulate Offline / Slow Consumer Resilience: Scale notification-service down to 0 replicas (--replicas=0), place 10 orders, and then scale it back up to 1. You will see it automatically catch up on all 10 missed messages using its saved Kafka Offset.
