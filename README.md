# k8s-monitoring-by-Prometheus-Grafana-using-Helm-chart

# Prerequisites:
# Kubernetes 1.19+
# Helm 3+

Get Helm Repository Info:
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
$ helm repo update

Install Helm Chart:
$ helm install my-prometheus prometheus-community/kube-prometheus-stack --version 56.21.3

Uninstall Helm Chart: **If you wnat to uninstall this helm chart otherwise ignore this part**
helm uninstall my-prometheus
CRDs created by this chart are not removed by default and should be manually cleaned up:
kubectl delete crd alertmanagerconfigs.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd probes.monitoring.coreos.com
kubectl delete crd prometheusagents.monitoring.coreos.com
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd scrapeconfigs.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com

$ kubectl get all - To check all the objects created by this helm chart.
$ kubectl get pod - To check all the running pods.
$ kubectl get configmap - To display all the configuration.
$ 
