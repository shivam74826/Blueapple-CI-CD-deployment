# 🔁 CI/CD with Jenkins & GitOps in Kubernetes

Modern software delivery relies on automation. Jenkins + Kubernetes + Git = GitOps-driven CI/CD.

---

## ⚙️ What Is CI/CD?

- **CI (Continuous Integration)**: Automate build & test when code changes.
- **CD (Continuous Delivery/Deployment)**: Automate deployment to environments.

---

## 🧱 Jenkins in Kubernetes (via Helm)

### Add Helm repo:
```bash
helm repo add jenkins https://charts.jenkins.io
helm repo update

Install Jenkins:
helm install jenkins jenkins/jenkins --set controller.serviceType=NodePort
Access Jenkins UI:
kubectl get svc jenkins
kubectl exec --namespace default -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password
Default username: admin

🎯 Setup Jenkins Pipeline with Kubernetes
📄 Jenkinsfile
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Building App...'
        sh 'docker build -t myapp .'
      }
    }
    stage('Test') {
      steps {
        echo 'Running Tests...'
        sh './run-tests.sh'
      }
    }
    stage('Push Image') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {
          sh 'docker login -u myuser -p $DOCKER_TOKEN'
          sh 'docker push myapp'
        }
      }
    }
    stage('Deploy to K8s') {
      steps {
        kubernetesDeploy(
          configs: 'k8s/deployment.yaml',
          kubeconfigId: 'kubeconfig-creds'
        )
      }
    }
  }
}

🔁 GitOps Workflow with ArgoCD (Alternative)
Install ArgoCD:

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Port forward:

kubectl port-forward svc/argocd-server -n argocd 8080:443
Login:

argocd login localhost:8080
argocd app create my-app \
  --repo https://github.com/your-org/your-repo.git \
  --path k8s \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default

🚀 GitOps Advantages
Feature	Benefit
Versioned	All K8s config in Git
Auditable	Git tracks all changes
Declarative	Ensures drift correction
Automated	CI pipeline pushes changes to Git

✅ Best Practices
Use Jenkins agents inside Kubernetes for scalability
Use secrets for Docker & kubeconfig
Integrate with Helm or Kustomize
For GitOps: treat Git as the source of truth
Automate rollback on failure using Git tags or image versions

📂 Sample Folder Structure

10-ci-cd/
├── Jenkinsfile
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── README.md
