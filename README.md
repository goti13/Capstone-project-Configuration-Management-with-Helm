# Capstone-project-Configuration-Management-with-Helm

Pre-requisite

- Basic knowledge of Jenkins essentials.

- Enthusiasm and completion of the Introduction to Helm Charts and Working with Helm Charts mini projects.

Project Deliverables

Documentation:

- Documentation for each Helm chart component setup.

- Explanation of simplified security measures implemented at each step.

Demonstration:

- Step-by-step demonstration of the Cl/CD pipeline with Helm integration.

Project Components

Jenkins Server Setup

Objective: Configure Jenkins server for a CI/CD pipeline automation.

Steps:
1. ﻿﻿﻿Install Jenkins on a dedicated server (with detailed explanations).
2. ﻿﻿﻿Set up necessary plugins (Git, Helm, etc.) with simple configurations.
3. ﻿﻿﻿Configure Jenkins with basic security measures suitable for beginners.

Instructions for Jenkins:

- Provide step-by-step documentation for Jenkins installation.
- Simplify the plugin installation and configuration steps.
- Outline basic security measures applied to the Jenkins server.

**IMPLEMENTATION**

1. Install Jenkins on a Dedicated Server

# Step-by-step Jenkins Installation

a) Update the system

```

sudo apt update && sudo apt upgrade -y

```
![image](https://github.com/user-attachments/assets/52af151e-dd10-44de-8107-71c793e7e754)

b) Install Java (Jenkins requires Java)

```
sudo apt install openjdk-17-jdk -y

```

![image](https://github.com/user-attachments/assets/805212da-2f87-43a6-9c08-aa2127267b42)

Verify:

```
java -version

```

![image](https://github.com/user-attachments/assets/5fe2b27f-7c3a-4801-b9e5-e37cb130d8b3)


c) Add Jenkins repository and key

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

![image](https://github.com/user-attachments/assets/44dc36aa-2f8e-41ac-b267-852d6d07af7e)

d) Install Jenkins

```
sudo apt update
sudo apt install jenkins -y
```

![image](https://github.com/user-attachments/assets/7772d945-dc8f-4985-a0d3-3225e534c7dd)

![image](https://github.com/user-attachments/assets/86969832-e975-4ed7-bdab-8749b45438dc)


e) Start and enable Jenkins

```
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

![image](https://github.com/user-attachments/assets/77d08429-a811-49dc-9169-15a92d8d9c77)


f) Verify Jenkins status

```
sudo systemctl status jenkins
```

![image](https://github.com/user-attachments/assets/09d294bf-a55d-4089-bee5-7a75d7633bb7)

g) Access Jenkins
Open your browser: http://<your_server_ip>:8080

Retrieve the initial admin password:

<img width="729" alt="image" src="https://github.com/user-attachments/assets/c1863cd2-ea26-461a-835c-c6ea82f2602f" />


![image](https://github.com/user-attachments/assets/956fc377-0e1b-4864-ba26-047e6f96e949)


2. Install Necessary Plugins

Initial plugin setup (recommended plugins)

When Jenkins prompts for plugin installation:

- Select Install suggested plugins.

<img width="1175" alt="image" src="https://github.com/user-attachments/assets/53540f18-6e2f-41bd-a033-667927264762" />

Additional plugins for CI/CD:
Go to:
Manage Jenkins → Manage Plugins → Installed Plugins

Search for and ensure that the following plugins are installed:

- Git plugin (SCM integration)

- GitHub plugin (webhook and GitHub integration)

- Pipeline (for Jenkins Pipeline scripts)

- Blue Ocean (optional, cleaner UI for pipelines)

✅ Jenkins will install these plugins and restart automatically if required.

🔹 Basic plugin configurations

Git Plugin:

Jenkins will auto-detect git if it is installed on your server.

Install git as it will be needed:

```
sudo apt install git -y

```

![image](https://github.com/user-attachments/assets/17659512-6361-46cc-b07f-14740f1d0dc9)


GitHub Plugin:

Go to:

1️⃣ Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic).


2️⃣ Click Generate new token (classic).


3️⃣ Give it a name (e.g., Jenkins) and expiration (e.g., 90 days or no expiration).

4️⃣ Select these scopes:

✅ repo (full repo access, or repo:status, repo_deployment, public_repo if only public repos)
✅ admin:repo_hook (to manage webhooks)
✅ read:org (if accessing private org repos)
✅ user:email
✅ workflow (if you need to trigger workflows)



5️⃣ Click Generate Token, copy it immediately.


<img width="1339" alt="image" src="https://github.com/user-attachments/assets/dc1cf134-7874-4ca4-90ba-051034245ac1" />


<img width="1155" alt="image" src="https://github.com/user-attachments/assets/d420725f-e0dc-4d83-a59d-81990050ba95" />


<img width="1155" alt="image" src="https://github.com/user-attachments/assets/d420725f-e0dc-4d83-a59d-81990050ba95" />

<img width="1159" alt="image" src="https://github.com/user-attachments/assets/32dc95e1-5827-4d3f-89ca-bf2dae33854f" />


 Add the PAT to Jenkins credentials
1️⃣ In Jenkins, go to:

```
Manage Jenkins → Manage Credentials → (global) → Add Credentials
```

2️⃣ Select:

Kind: Secret text

Secret: paste the PAT

ID: github-pat (or any name)

Description: GitHub PAT for Jenkins

 Click CREATE.
 


<img width="1100" alt="image" src="https://github.com/user-attachments/assets/d25e23a8-6118-4d75-99b9-4cebff9ef54e" />


Add credentials in GitHub Server configuration
1️⃣ Go back to:

Manage Jenkins → Configure System → GitHub
2️⃣ Under GitHub Servers, click Add → select Jenkins.
3️⃣ Select “Secret text” and choose the credential you just created (github-pat).
4️⃣ Click Test Connection.

✅ You should now see:

```
Connected to https://api.github.com as <your-github-username>

```

<img width="1254" alt="image" src="https://github.com/user-attachments/assets/c92f74a5-2e18-403c-b52b-6b19d4dcb6f1" />

Helm Plugin:

Ensure Helm is installed on your Jenkins server:

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version

```

![image](https://github.com/user-attachments/assets/39e1e365-e2ee-4208-98fd-b7610e0d5f08)


✅ Now Helm can be used in Jenkins pipelines for Kubernetes deployments.


Configure Basic Security for Jenkins

🔹 Enable security:
Jenkins by default enables security upon first setup.


🔹 Create an admin user:
During initial setup, create your own admin user and disable the default admin user.

🔹 Configure security realm and authorization:
Go to:
Manage Jenkins → Configure Global Security

✅ Recommended beginner settings:

Security Realm: Jenkins’ own user database

Authorization: Matrix-based security (optional) or Project-based Matrix Authorization for better control

<img width="1223" alt="image" src="https://github.com/user-attachments/assets/951b62c9-7bf4-406c-8ce6-617ee9f777ff" />


🔹 Regular Maintenance:
✅ Update Jenkins and plugins regularly:

Manage Jenkins → Manage Plugins → Updates

Apply updates and restart Jenkins periodically.

✅ Backup Jenkins configurations:

Use the ThinBackup plugin or manual backups of:

```
/var/lib/jenkins

```

✅ Monitor Jenkins logs:

```
sudo journalctl -u jenkins -f

```























# Helm Chart Basics


Objective: Introduce the fundamental concepts of Helm charts.

Steps:
1. ﻿﻿﻿Explain what Helm charts are and their purpose.
2. ﻿﻿﻿Walkthrough of creating a basic Helm chart for a web application.
3. ﻿﻿﻿Cover Helm chart templating basics.


Instructions:

- Provide beginner-friendly explanations of Helm chart concepts.

- Include a hands-on guide for creating a simple Helm chart.

Working with Helm Charts

Objective: Guide students in working with Helm charts for application deployment.

Steps:

1. ﻿﻿﻿Deploy a sample web application using the Helm chart created.
2. ﻿﻿﻿Introduce values and templates in Helm charts.
3. ﻿﻿﻿Explore upgrading and rolling back applications using Helm.


Instructions:
- Include simple examples and explanations for deploying applications with Helm.
- Provide a guide for Helm chart customization.

🚀 1️⃣ What are Helm charts?

Helm is the package manager for Kubernetes, similar to apt for Ubuntu or brew for Mac.

A Helm chart:

Is a collection of YAML files describing Kubernetes resources (Deployments, Services, etc.).

Packages your Kubernetes app with versioning, configuration values, and reusability.

Allows easy installation, upgrades, and rollback of applications on Kubernetes.

✅ Purpose:

Simplifies managing complex Kubernetes apps.

Enables parameterized deployments using templating.

Helps maintain consistency across environments (dev, staging, prod).

🚀 2️⃣ Hands-on: Create a basic Helm chart for a web application
We will create a Helm chart for an Nginx web server.

a) Prerequisites
✅ Kubernetes cluster (Minikube, Kind, or EKS)
✅ Helm installed:
✅ kubectl configured to your cluster.

If not aalready installed, install Kubernetes cluster (Minikube, Kind, or EKS). 

b) Create a new Helm chart

```
helm create my-nginx

```

This generates:

```
my-nginx/
  charts/
  templates/
  values.yaml
  Chart.yaml

```

![image](https://github.com/user-attachments/assets/572f9c89-8b1c-400e-8a34-608face6f2c1)

c) Key files explained

Chart.yaml – metadata about your chart (name, version).

values.yaml – default values you can override during deployment.

templates/ – contains Kubernetes manifest templates (deployment.yaml, service.yaml).


d) Customize your chart
Open values.yaml:

```
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: stable
service:
  type: ClusterIP
  port: 80

```

e) Install your Helm chart on Kubernetes
From the same directory:

```
helm install my-nginx ./my-nginx

```

![image](https://github.com/user-attachments/assets/3188354a-4ec9-4c6c-8821-6ccd5e9d1c05)


Verify:

```
helm list
kubectl get pods
kubectl get svc

```

![image](https://github.com/user-attachments/assets/5a9d2d3d-6de8-4810-912f-84736537b5f0)

f) Upgrade your chart
Edit values.yaml:

```
service:
  type: NodePort
```

Then upgrade:

```
helm upgrade my-nginx ./my-nginx

```

![image](https://github.com/user-attachments/assets/9e34d31b-cd52-4328-9cbb-8c89a46e3dfa)


![image](https://github.com/user-attachments/assets/6c5c04a4-b614-4003-b1e4-9555996265e0)


g) Uninstall your chart

```

helm uninstall my-nginx

```


![image](https://github.com/user-attachments/assets/f832165b-82a5-4e8f-af8c-7a6c23d3c1f9)

🚀 3️⃣ Helm chart templating basics

Helm uses Go templating to make YAML files reusable and dynamic.

Example:

In deployment.yaml under templates/, you may see:

```
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"

```

Here:

```
{{ .Values.image.repository }} is replaced with the value in values.yaml.

```

You can also use conditionals and loops:

```
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
...
{{- end }}

```

This means:

- You can enable or disable resources based on values.

  
- Maintain one chart for different environments (by overriding values.yaml during helm install).



Integrating Helm with Jenkins


Objective: Integrate Helm with Jenkins for automated deployments.
Steps:

1. ﻿﻿﻿Integrate Jenkins with Helm for deployment automation.
2. ﻿﻿﻿Configure Jenkins to use Helm charts in the Cl/CD pipeline.

Instructions:

- Provide step-by-step instructions for integrating Helm with Jenkins.

- Simplify Jenkins configurations for Helm chart deployments.




🚀 Objective
Integrate Helm with Jenkins to automate Kubernetes deployments using Helm charts in your CI/CD pipeline.


1️⃣ Install Helm on Jenkins Server/Agent

A. Check if Helm is installed:

```
helm version

```

B. If not installed:

On Mac:

```

brew install helm

```

On Ubuntu:

```

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

```

Verify:

```
helm version

```

2️⃣ Ensure Jenkins Has Kubernetes Access

✅ Jenkins server/agent must have:

- kubectl installed.

- kubeconfig file configured to access your cluster:

```

kubectl config view
kubectl get nodes

```

✅ Alternatively, store kubeconfig in Jenkins Credentials (recommended for shared agents).

4️⃣ Configure Jenkins Pipeline for Helm Deployments


A. Create a New Pipeline Job

1. Go to Dashboard → New Item → Pipeline → Name: helm-deploy-pipeline → OK.

2. In Pipeline → Definition → Pipeline script (or from SCM).

B. Example Jenkinsfile

```
pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig-credential-id') // if using Jenkins credentials
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Helm Lint') {
            steps {
                sh 'helm lint charts/my-app'
            }
        }

        stage('Helm Deploy') {
            steps {
                sh """
                helm upgrade --install my-app charts/my-app \
                  --namespace default \
                  --create-namespace \
                  --values charts/my-app/values.yaml
                """
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully.'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}

```

Explanation:
✅ helm lint – Checks your chart syntax before deploying.
✅ helm upgrade --install:

my-app: Release name.

charts/my-app: Path to your Helm chart.

--create-namespace: Creates namespace if not present.

--values: Uses your chart’s values.yaml for configuration.


5️⃣ Using Jenkins Credentials for Kubeconfig (Recommended)
Go to Manage Jenkins → Credentials → (global) → Add Credentials.

Select:

Kind: Secret file

File: Upload your kubeconfig file

ID: kubeconfig-credential-id (must match Jenkinsfile).

This securely provides cluster access during your pipeline without hardcoding sensitive information.

6️⃣ Run and Verify
✅ Click Build Now on your pipeline job.

✅ Check Console Output for:

Successful Git clone.

Helm lint passing.

Helm deployment completing.

✅ Verify your app:

```

kubectl get pods
kubectl get svc
helm list

```

