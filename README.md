K8S Cluster Requirements

Docker
Local k8s cluster (Kind)
Kubectl
Ingress Controllers
ArgoCd
Helm chart
Rails app (hello word)

Assumption
No data persistence

Step 1 : Install Docker
- Download and install Docker for mac from the official Docker website: https://www.docker.com/products/docker-desktop
- Follow the installation instructions and make sure Docker is running on your mac.

Step 2 : Install Homebrew
-Install Homebrew (Package Manager) from brew website
https://brew.sh 

Step 3 : Install Kind and Kubectl and Helm
Install Kind using Homebrew
- In the terminal session, run the following command to install Kind:

$brew install kind


Verify Kind installation
- Run the following command to verify the Kind version:

$kind --version


  You should see the installed Kind version information.

Install Kubectl using Homebrew
- In the terminal session, run the following command to install Kubectl:
$brew install kubectl
Verify kubectl installation
- Run the following command to verify the kubectl version:
$kubectl version --client
Install Helm using Homebrew
- In the terminal session, run the following command to install Helm:
$brew install helm
Verify helm installation
- Run the following command to verify helm version:
$helm version

Step 4 : Create a k8s Cluster
Create a Kubernetes Cluster using Kind
- Create a new directory for your cluster configuration and navigate to it in the command line
- Run the following command to create the Kind cluster:
$kind create cluster --name everly
  This command will create a Kubernetes cluster using Kind and name it ‘everly’. You can choose any name you prefer. You can now interact with cluster using ‘kubectl’


<img width="767" alt="Screen Shot 2023-06-19 at 11 47 22 AM" src="https://github.com/alfred-a/assignment/assets/49098665/872b1daf-9864-4999-96c7-92eb6ca25fec">


Verify the Cluster
- Run the following command to verify that your cluster is running:
$kubectl clsuter-info --context kind-everly
  You should see information about your cluster, such as the Kubernetes master and core services.


<img width="796" alt="Screen Shot 2023-06-19 at 11 51 59 AM" src="https://github.com/alfred-a/assignment/assets/49098665/78cd4968-f89a-478d-9959-11e14e01f69c">

 You can now use `kubectl` to interact with your cluster and deploy applications.


<img width="504" alt="Screen Shot 2023-06-19 at 11 57 04 AM" src="https://github.com/alfred-a/assignment/assets/49098665/2933d7c3-1a55-4112-a8ca-43795642f2bd">

Installing the Nginx Ingress Controller Helm chart
The chart is fully configurable, but here we are using the default configuration.
Add the Ingress Nginx Helm repository:
$helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
$helm repo update
These commands will add the Ingress Nginx Helm repository to your local Helm chart repository and update the installed chart repositories:
<img width="752" alt="Screen Shot 2023-06-19 at 12 20 53 PM" src="https://github.com/alfred-a/assignment/assets/49098665/382bead5-f60e-4a76-a949-7e04722bec48">


Install the latest version of Ingress Nginx with helm install command:
$helm -n ingress-nginx install ingress-nginx ingress-nginx/ingress-nginx --create-namespace


The install process will begin and a new ingress-nginx namespace will be created.


<img width="884" alt="Screen Shot 2023-06-19 at 12 28 31 PM" src="https://github.com/alfred-a/assignment/assets/49098665/f6464083-9ec9-448f-9576-47e3bb026c06">


$kubectl get svc -n ingress-nginx

<img width="857" alt="Screen Shot 2023-06-19 at 12 34 43 PM" src="https://github.com/alfred-a/assignment/assets/49098665/3f0e7c9e-9889-4bcb-85ef-431f1adddae9">

$kubectl get pod -n ingress-nginx

<img width="583" alt="Screen Shot 2023-06-19 at 12 35 13 PM" src="https://github.com/alfred-a/assignment/assets/49098665/82106b28-1b52-4d76-9621-d27ceebf6949">




Installing Argo CD on Cluster
 Create the argocd namespace in your cluster, which will contain Argo CD and its associated services:
$kubectl create ns argocd


<img width="346" alt="Screen Shot 2023-06-19 at 12 46 40 PM" src="https://github.com/alfred-a/assignment/assets/49098665/4383bec2-34ec-4376-bbe4-fdd4bb3043d2">

After that, you can run the Argo CD install script provided by the project maintainers.
$kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
<img width="886" alt="Screen Shot 2023-06-19 at 12 49 10 PM" src="https://github.com/alfred-a/assignment/assets/49098665/0a2540b2-df58-4483-90d3-a4e54204e4a6">

Once the installation completes successfully, you can use the watch command to check the status of your Kubernetes pods:
 
Check services for Argocd:
$kubectl get svc -n argocd
<img width="876" alt="Screen Shot 2023-06-19 at 12 51 46 PM" src="https://github.com/alfred-a/assignment/assets/49098665/bfcf4ccb-b672-4ce7-a1e2-b66835c7c8cd">

Forwarding Ports to Access Argo CD:
$kubectl port-forward svc/argocd-server -n argocd 8080:443

<img width="970" alt="Screen Shot 2023-06-19 at 12 56 08 PM" src="https://github.com/alfred-a/assignment/assets/49098665/8c9e333e-34fd-4904-8f36-18b3b36cca8c">

Access Argocd:
127.0.0.1:8080
<img width="1224" alt="Screen Shot 2023-06-19 at 12 56 57 PM" src="https://github.com/alfred-a/assignment/assets/49098665/e1f8de14-5246-4430-b65b-d6accbac1752">

Retrieve the admin password which was automatically generated during your installation, so that you can use it to log in. You’ll pass it a path to a particular JSON file that’s stored using Kubernetes secrets, and extract the relevant value:
$kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath=”{.data.password}” | base64 echo
You can then log into your Argo CD dashboard by going back to localhost:8080 in a browser and logging in as the admin user with password from above.


<img width="1222" alt="Screen Shot 2023-06-19 at 1 03 17 PM" src="https://github.com/alfred-a/assignment/assets/49098665/90023f70-f571-44ee-8b0b-1920d8fa23b6">


Dockerizing a new "Hello, World!" application

Dockerizing a Rails new application, adding Helm charts for it, and deploying it to a local Kubernetes cluster:

Step 1: Create a new application
Find app in repo: hello-world-app
Step 2: Create a Dockerfile
In the root directory of your Rails application, create a file named Dockerfile.
Find Dockefile in repo: hello-world-app
Step 3: Build the Docker image
In the terminal, navigate to the root directory of your Rails application.
Run the following command to build the Docker image:
$docker build -t helloworld-app .
This command will build a Docker image with the tag helloworld-app. 


<img width="857" alt="Screen Shot 2023-06-19 at 1 38 16 PM" src="https://github.com/alfred-a/assignment/assets/49098665/161efee2-43ea-40a5-8c78-3b947ed67779">


Check creation of the image:


$docker image ls


<img width="705" alt="Screen Shot 2023-06-19 at 1 39 56 PM" src="https://github.com/alfred-a/assignment/assets/49098665/6592fb02-deea-43d0-95d0-f99c2699f77b">

Step 4: Test the Docker image
Run the following command to run a container from the Docker image and test it locally:


$docker run -p 3000:3000 helloworld-app


<img width="623" alt="Screen Shot 2023-06-19 at 1 41 30 PM" src="https://github.com/alfred-a/assignment/assets/49098665/74b69eae-a95c-4f72-8ccc-edeccfd08c44">

Open your web browser and visit http://localhost:3000 to see the "Hello World!" page. 
<img width="1218" alt="Screen Shot 2023-06-19 at 1 42 33 PM" src="https://github.com/alfred-a/assignment/assets/49098665/4b06f6ed-409f-41a3-afb4-059aa87b671b">


Step 5: Tag & push image docker
List your Docker images to find the image ID or name that you want to tag. Open a terminal and run:


$docker image ls


<img width="658" alt="Screen Shot 2023-06-19 at 1 48 26 PM" src="https://github.com/alfred-a/assignment/assets/49098665/aa149e7e-9a74-4679-931d-9632d8d23a9a">

This will display a list of Docker images along with their repository, tag, and image ID. Identify the image you want to tag and note its image ID or name. Tag the image using the docker tag command. Replace <image-id-or-name> with the actual image ID or name from step 1, and <new-repository> and <new-tag> with the desired repository and tag for the image:
$docker tag <IMAGE ID> <docker_username/new-repository:<new-tag> This command creates a new tag for the image. 
Verify that the image has been tagged correctly by running docker images again. 
You should see the newly tagged image in the list. 
Log in to the Docker registry where you want to push the image. Use the docker login command and provide your credentials:
$docker login <registry-url>
Replace <registry-url> with the URL of your Docker registry.
Push the tagged image to the registry using the docker push command. Specify the repository and tag that you used in the previous step:
$docker push <new-repository:new-tag>
This command uploads the image to the Docker registry.
Create Helm charts
Step 1 : Create Helm charts
In the root directory of your Rails application, create a directory named helm-charts and change into it. 
$helm create helloworld-app
This will generate the necessary files and directories for the Helm chart.
Step 2 : Configure Helm charts
Open the values.yaml file in the helm-charts/ myapp-helloworld directory and modify the following;
-Update the image.repository value to match the Docker image you built earlier (helloworld-app).
-Update service type to: LoadBalancer and Port to 3000
Step 3: Deploy the Rails application using Helm
In the terminal, navigate to the helm-charts directory. Run the following command to deploy your Rails application to the local Kubernetes cluster using Helm:
$helm install helloworld-app ./helloworld-app
This command will deploy the Rails application to the Kubernetes cluster using the Helm chart.
Run the following command to check the status of your deployment:
$kubectl get pods
You should see a pod running with your Rails application.


Run the following to see the ports service is on and to forward to localhost:
$kubctl get svc


<img width="603" alt="Screen Shot 2023-06-19 at 8 06 48 PM" src="https://github.com/alfred-a/assignment/assets/49098665/cb6e3833-562e-4602-a6b8-913d84f3f3b9">

$kubectl port-forward service/helloworld-app 30240:3000

Step 8: Verify Application

Go to localhost:30240 in browser to confirm application is running


<img width="1086" alt="Screen Shot 2023-06-19 at 8 14 56 PM" src="https://github.com/alfred-a/assignment/assets/49098665/2cb9c2cb-563b-416a-8769-f6990864cc90">



Screen recording of logs:
Argocd logs

https://github.com/alfred-a/assignment/assets/49098665/21ae067c-6405-49b3-aa39-02a4d25d23ce




Helloworld app logs

https://github.com/alfred-a/assignment/assets/49098665/e4e00a53-1622-4771-bd82-17cdf870713b



