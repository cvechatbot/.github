# CVE Vulnerability ChatBot

## Project Overview
The CVE Vulnerability ChatBot is designed to automate the monitoring, reporting, and querying of Common Vulnerabilities and Exposures (CVEs). This project leverages a cloud-native infrastructure on AWS and integrates AI-powered chatbot functionality to provide real-time information about vulnerabilities.

## Business Objectives
- **Enhance Cybersecurity Awareness**: Provide real-time updates on CVEs to improve security posture and mitigate risks.
- **Automate Threat Intelligence**: Reduce manual efforts by automating the collection and analysis of CVE data.
- **Improve Incident Response Time**: Enable quick access to vulnerability information for faster remediation.
- **Ensure Scalable & Reliable Infrastructure**: Deploy a highly available and fault-tolerant architecture on AWS.
- **Deliver AI-Driven Insights**: Utilize a chatbot powered by LLMs to provide contextual insights on vulnerabilities.
- **Streamline Compliance & Reporting**: Assist organizations in maintaining compliance with security standards by tracking vulnerabilities.

## Infrastructure Overview
The infrastructure is fully deployed on Amazon Web Services (AWS) using foundational services such as EKS, IAM, VPC, and Route 53. The infrastructure is optimized for high availability and reliability.

### Data Pipeline Architecture
The pipeline is designed to track, process, and store the latest CVEs from GitHub. The main components include:

1. **Kubernetes Controllers**:
   - **GitHub Releases Monitor**: This custom controller monitors the cvelistV5 GitHub repository for new CVE releases. It deploys Custom Resources (CRs) for each new release.
   - **Producer Controller**: A second controller deploys pods that process these CRs and send relevant CVE data to Apache Kafka.

2. **Apache Kafka**:
   - **Kafka Cluster**: Deployed within the EKS cluster, Kafka acts as the message broker, handling real-time streaming of CVE data from producer pods.
   - **Message Topics**: Messages are organized into Kafka topics for efficient downstream consumption.

3. **Consumer Application**:
   - **PostgreSQL Database**: Deployed on the EKS cluster, the consumer application processes messages from Kafka and stores CVE data in the PostgreSQL database for long-term storage and query handling by the chatbot.

### AI-Powered Chatbot
The chatbot, hosted within the EKS cluster, allows users to interact with the CVE data through specific queries. The all-mpnet-base-v2 model is used to process and summarize the CVE data. Users can ask questions such as:

- "Are there any new vulnerabilities for the software I am using?"
- "What recent CVEs have been published that affect my infrastructure?"

The chatbot provides detailed and contextual responses based on real-time CVE data.

## Observability & Monitoring
To ensure system reliability and performance, the observability stack includes:

- **Metrics Server**: Monitors pod-level resource usage (CPU, memory) across the EKS cluster.
- **Prometheus**: Aggregates metrics from all components, including Kafka, PostgreSQL, and the controllers.
- **Grafana**: Provides dashboards for visualizing key performance metrics such as resource usage and message throughput.

This observability ensures that any issues in the data pipeline or infrastructure can be detected and resolved quickly.

### Additional Reliability Features
- **Istio Service Mesh**: Enhances service-to-service communication reliability within the EKS cluster.
- **Cluster Autoscaling**: Automatically adjusts the number of EC2 instances based on demand.
- **Horizontal Pod Autoscaling (HPA)**: Scales Kubernetes pods based on CPU and memory usage to ensure high availability.
- **Pod Disruptors**: Used to ensure graceful handling of pod disruptions, maintaining system stability during updates or failures.

## Future Enhancements
Planned improvements include:

- Scaling Kafka and PostgreSQL to handle larger volumes of CVE data.
- Expanding the chatbot's query capabilities to cover more complex CVE impact analysis.

