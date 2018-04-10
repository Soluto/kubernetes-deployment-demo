# Helm Deployment Demo
This repo contains a full working demo of how we are using Helm for rapid Kubernetes deployment.
Use this repo to feel how fast and simple it can be.
To have a better understanding of the flow, check out this [Blog Post]()

## Structure
* `chart-museum`: All that is required to run [Chart Museum](https://github.com/kubernetes-helm/chartmuseum) that will hold our charts.
* `helm-chart`: All the files that are required for building the chart and publishing it to Chart Museum
* `kubernetes`: Kubernetes file required for running the demo (create namespace and TLS secret)
* `server`: The app that we will deploy on Kubernetes, a small web app written in [Lolcode](https://http://lolcode.org/).

## Flow
The idea is to use Helm to pack all Kubernetes configuration files.
The developer can use the chart to publish her code into production.
All the customization is done on `server/values.yaml`.
Prerequisites: Docker and kubernetes (Minikube will do just fine)
The demo will create a namespace named `helm-demo` on your cluster and will create all the artifacts in this namespace.

To run the demo:
* Run Chart Museum: `cd chart-museum` && `docker-compose up -d`
* Publish the chart: `cd ../helm-chart/web-api` && `../scripts/publish.sh 1 http://localhost:8080/`
* Prepare Kubernetes:`cd ../../kubernetes` && `kubectl apply -f .`
* Build the app so it will be available for minikube: `cd ../server` && `eval $(minikube docker-env)` && `docker build -t local/httpd:1 .`
  * If you're using something other than minikube, first publish your image to a repository.
  * Than, modify `server/values.yaml`: `image.repository` to the repository, and `image.version` to the tag.
* Add Chart Museum to Helm: `helm repo add local-chart-museum http://localhost:8080`
* Deploy the app: `helm upgrade --install -f values.yaml httpd local-chart-museum/web-api --wait`
* Use curl to test the deployment (or edit `/etc/hosts`): `curl -H "Host: httpd.local" -k https://<ingress ip>/lol.html`. To get the IP run `kubectl get ingress --namespace=helm-demo`.
