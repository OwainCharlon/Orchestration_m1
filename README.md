# Cert Manager installation

`helm repo add jetstack https://charts.jetstack.io`

`helm repo update`

`helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.8.0 --set installCRDs=true`

`kubectl apply -f .\cert_manager_deployment.yml`

# Ingress Controller installation

`helm install nginx-ingress ingress-nginx/ingress-nginx --namespace appscore-ingress --create-namespace --set controller.replicaCount=2 --set controller.defaultTLS.cert=""`

# Appscore application deployment

`kubectl apply -f .\appscore_deployment.yaml`

# Prometheus deployment

`helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

`helm repo update`

`helm install prometheus prometheus-community/kube-prometheus-stack -n appscore`

## Access to Grafana

`kubectl port-forward -n appscore deployment/prometheus-grafana 3000`
# ELK deployment

`Helm repo add elastic https://Helm.elastic.co`

`Helm install elasticsearch --namespace=appscore elastic/elasticsearch --set replicas=1 --set minimumMasterNodes=1 --set clusterHealthCheckParams="wait_for_status=yellow&timeout=1s"`

`Helm install kibana --namespace=appscore  elastic/kibana`
## Initialising some metrics datas with metricbeat

`Helm install metricbeat --namespace=appscore elastic/metricbeat`

## Access to Kibana

`kubectl port-forward -n appscore deployment/kibana-kibana 5601`

## Deploying logstach release

`helm repo add bitnami https://charts.bitnami.com/bitnami`

`helm repo update`

`helm install logstach --namespace=appscore bitnami/logstash`