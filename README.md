# HW2 - Student Survey Project

Student Survey application containerized with Docker and deployed to Kubernetes.  
Includes:
- Dockerfile  
- deployment.yaml  
- index.html / survey.html  

## Instructions
- Build Docker image: `docker build -t studentsurvey:1.0 .`
- Run container: `docker run -p 8182:80 studentsurvey:1.0`
- Deploy on Kubernetes: `kubectl apply -f deployment.yaml`
