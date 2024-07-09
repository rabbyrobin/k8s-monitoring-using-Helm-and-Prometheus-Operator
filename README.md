# Kubernetes Cluster monitoring by Prometheus Grafana, Node exporter installed using Helm and Prometheus Operator.

# Prerequisites:
# Kubernetes 1.19+
# Helm 3+

# Part 1: Install Prometheus, node exporter and Alert Manager.

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

# Part 2: Access Grafana.

# To list down all services.

$ kubectl get svc

All the services actually have cluster ip tag which isn't accessible from outside. to access Grafana from outside you have to setup NodePort service for grafana.

# To find out the port of grafana because the container is controlled by replicaset type below command:

$ kubectl get rs

Now describe the grafana repicaset

$ kubectl describe rs "replicaset name for grafana"     ... //You will get the service port 3000 and host port 80.

# Now edit the garafana service and change the service type from ClusterIP to NodePort. It'll map the host port 80 with the node port and garafana can be accessed from the outside of the cluster.

$ kubectl edit svc my-prometheus-grafana

Now browse the grafana dashboard by browsing your host ip and grafana node port.

# Login to grafana dashboard: Find the grafana login secrets

$ kubectl get secrets

$ kubectl get secrets my-prometheus-grafana -o yaml

Now decode the base64 values

$ echo -n "YWRtaW4=" |base64 -d      // You wil find the user name

$ echo -n "cHJvbS1vcGVyYXRvcg==" |base64 -d      // You will find the password

Now Configure Email Alerting through Grafana:

Find the configmap file for grafana -
kubectl get configmap
kubectl edit config map my-prometheus-grafana
To configure Grafana to send email alerts to your email address (rr.robin91@gmail.com), you'll need to add an SMTP section to the grafana.ini configuration. Here's how you can modify the existing configuration:
apiVersion: v1
data:
  grafana.ini: |
    [analytics]
    check_for_updates = true
    [grafana_net]
    url = https://grafana.net
    [log]
    mode = console
    [paths]
    data = /var/lib/grafana/
    logs = /var/log/grafana
    plugins = /var/lib/grafana/plugins
    provisioning = /etc/grafana/provisioning
    [server]
    domain = ''
    [smtp]
    enabled = true
    host = smtp.gmail.com:587
    user = your-email@gmail.com
    password = your-app-password
    from_address = your-email@gmail.com
    from_name = Grafana Alerts
    startTLS_policy = OpportunisticStartTLS
    [alerting]
    enabled = true
    execute_alerts = true
Now after configuring the configmap you have to use the command to redeploy the deployment:
kubectl rollout restart deployment my-prometheus-grafana
Now delete the grafana pod/pods to make this change work, it'll recreate the new pod/pods.
