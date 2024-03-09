# k8s monitoring by Prometheus Grafana using Helm and Prometheus Operator.

# Prerequisites:
# Kubernetes 1.19+
# Helm 3+

# Part 1: Install Prometheus and Alert Manager.

# Get Helm Repository Info:

$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

$ helm repo update

# Install Helm Chart:

$ helm install my-prometheus prometheus-community/kube-prometheus-stack --version 56.21.3

# Uninstall Helm Chart: **If you wnat to uninstall this helm chart otherwise ignore this part**

$ helm uninstall my-prometheus

# CRDs created by this chart are not removed by default and should be manually cleaned up:

$ kubectl delete crd alertmanagerconfigs.monitoring.coreos.com

$ kubectl delete crd alertmanagers.monitoring.coreos.com

$ kubectl delete crd podmonitors.monitoring.coreos.com

$ kubectl delete crd probes.monitoring.coreos.com

$ kubectl delete crd prometheusagents.monitoring.coreos.com

$ kubectl delete crd prometheuses.monitoring.coreos.com

$ kubectl delete crd prometheusrules.monitoring.coreos.com

$ kubectl delete crd scrapeconfigs.monitoring.coreos.com

$ kubectl delete crd servicemonitors.monitoring.coreos.com

$ kubectl delete crd thanosrulers.monitoring.coreos.com

# Now get the confirmation of installation by runnimg below commands:

$ kubectl get all - To check all the objects created by this helm chart.

$ kubectl get pod - To check all the running pods.

$ kubectl get configmap - To display all the configuration.

$ kubectl get secrets - To get all the secrets configuration for prometheus and grafana as well as certificates.

$ kubectl get crd - To display all Custom Resource Definitions (CRD).

# Now check the description of the Prometheus and Alert Manager:

$ kubectl get statefulset    #// Now find the respective container

# Check the status of the Operator:

$ kubectl get deployment

$ kubectl describe deployment my-prometheus-kube-prometh-operator

# To get the description of prometheus pod in a yaml format:

$ kubectl get statefulset prometheus-my-prometheus-kube-prometh-prometheus -o yaml

# To save it into an yaml file:

$ kubectl get statefulset prometheus-my-prometheus-kube-prometh-prometheus -o yaml > prom.yaml

# To find the rules file "prometheus-my-prometheus-kube-prometh-prometheus-rulefiles-0" which defines the alert rules:

$ kubectl get configmap

# Now search for the "prometheus-my-prometheus-kube-prometh-prometheus-rulefiles-0" file and print it to a yaml output to see the rules. You can add or remove rules from here.

# Now check the secrets for prometheus config.

$ kubectl get secrets

# Now search for "prometheus-my-prometheus-kube-prometh-prometheus" file and print the output into yaml to see.

$ kubectl get secret prometheus-my-prometheus-kube-prometh-prometheus -o yaml

# Access Grafana:



