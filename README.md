# Setting Up CI/CD for Python Django Application using Helm Charts and Kubernetes

**NOTE:** This README file provides instructions for setting up CI/CD for a Python Django application using Helm Charts and Kubernetes. However, please keep in mind that this is a sample project created for academic and demonstration purposes. It may cause errors and is not intended for direct cloning. Make sure to customize it according to your specific project requirements.

## Prerequisites

Before you proceed, ensure you have the following prerequisites installed and configured:

1. Docker: Docker should be installed and running on your system to build and manage containers.

2. Minikube: Minikube should be installed and configured for local Kubernetes deployment.

3. Jenkins: Jenkins should be installed and running on your laptop. Make sure to install the following plugins in Jenkins:

   - GitHub plugin
   - Docker plugin
   - Kubernetes plugin
   - AWS plugin

4. Helm CLI: Helm CLI should be installed on your system to create and manage Helm Charts.

5. Django App: Ensure that your Django application, which includes a sample Django Authentication (Login, Signup) App, is developed and ready for deployment.

## Step 1: Helm Chart Creation

1. Create a new directory named "helm-chart" in the root directory of your repository.

2. Inside the "helm-chart" directory, create the following files:

   - "values.yaml": This file will contain the default configuration values for your Helm Chart.
   - "deployment.yaml": Use this file to define the Kubernetes Deployment resource for your Django application.
   - "service.yaml": Create this file to define the Kubernetes Service resource for your Django application.
   - "ingress.yaml" (Optional): Use this file to define the Kubernetes Ingress resource to expose your application externally.
   - "hpa.yaml": Create this file to define the Kubernetes Horizontal Pod Autoscaler resource for your Django application.

3. Open a terminal or command prompt.

4. Navigate to the root directory of your project.

5. Run the following command to create a Helm Chart for your Python Django application:


6. Ensure that all the above files are properly configured with the necessary values and specifications for your Django application.

## Jenkins Setup

1. Access the Jenkins web interface and install the required plugins (mentioned in prerequisites).

2. Configure your GitHub credentials in Jenkins using the Jenkins Credentials Manager.

3. Create two pipeline jobs in Jenkins:

- **Build (Integration Pipeline)**: Use the "Jenkinsfile" to Git checkout, Python build and unit test, build the Docker image, package the Helm Chart and push to ECR (Elastic Container Registry).

- **Deploy (Deploy Pipeline)**: Use the "Jenkinsfile-helm" to download the Helm Chart from ECR and deploy it to Minikube.

4. In the Jenkins pipelines ("Jenkinsfile" and "Jenkinsfile-helm"), update the environment variables to match your specific environment. Update the credentials for GITHUB, AWS, DOCKERHUB, and k8s accordingly. Ensure that the variables and credentials are correct and accessible.

5. Create environment variables in the Jenkins interface as required for your project.

## Helm Chart Customization

The "helm-chart" directory contains the Helm Chart templates for the Django application deployment. Customize the following files to match your application's requirements:

- "values.yaml"
- "deployment.yaml"
- "service.yaml"
- "ingress.yaml" (if required)

## Usage

1. Make necessary changes to your Python Django application and Helm Chart templates.

2. Commit and push the changes to your GitHub repository.

3. Jenkins will automatically trigger the CI/CD pipeline upon each push.

4. Monitor the Jenkins job status to track the build and deployment progress.

## Important Notes

1. In a production environment, ensure proper security measures and secrets management are in place.

2. Make sure to customize the Helm Chart and Kubernetes configurations according to your application's requirements and environment.

3. For production deployments, use a secure and private Docker registry and ECR setup.

4. In a real-world scenario, consider setting up Jenkins on a dedicated server or using a CI/CD platform, like GitLab CI or GitHub Actions, for better scalability and manageability.

5. **Do not forget to update the Jenkins pipeline scripts and credentials with appropriate values before usage.**

6. Happy coding and deploying!
