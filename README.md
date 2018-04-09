* `cd chart-museum` && `docker-compose up -d`
* `cd ../helm-chart/web-api` && `../scripts/publish.sh 1 http://localhost:8080`
* `cd ../kubernetes` && `kubectl apply -f .`
* `cd ../server` && ` eval $(minikube docker-env) ` && `docker build -t local/httpd:1 .` && `helm upgrade --install -f values.yaml httpd local-chart-museum/web-api --wait`
* Use curl to test the deployment (or edit `/etc/hosts`): `curl -H "Host: httpd.local" -k https://<ingress ip>/lol.html`. To get the IP run `kubectl get ingress --namespace=helm-demo`.