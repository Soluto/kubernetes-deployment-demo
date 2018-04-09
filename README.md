* `cd chart-museum` && `docker-compose up -d`
* `cd ../helm-chart/web-api` && `../scripts/publish.sh 1 http://localhost:8080`
* `cd ../kubernetes` && `kubectl apply -f .`
* `cd ../server` && ` eval $(minikube docker-env) ` && `docker build -t local/httpd:1 .`