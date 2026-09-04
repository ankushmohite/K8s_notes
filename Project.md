First, the developer pushes the code to Git. Jenkins picks up the code and performs the CI process. It builds and compiles the code, runs the unit test cases, creates a Docker image, and pushes that image to the Docker registry.

We keep our Kubernetes manifest files separately in Git for each environment. When we update the manifest with the required image version, Argo CD detects the change and deploys it to the Kubernetes cluster. Kubernetes pulls the image from the Docker registry and runs the application inside the Pods.

we use ELK for centralized logging. So, the application logs are collected and stored in Elasticsearch, and we use Kibana to search and analyze the logs. This helps us troubleshoot application and deployment-related issues.

We use Prometheus and Grafana for monitoring the Kubernetes cluster and application metrics. Prometheus collects the metrics, and Grafana is used to visualize those metrics through dashboards.
