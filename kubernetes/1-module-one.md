# Module 1

Create a working directory and navigate into it

```
mkdir otel-quickstart
```
```
cd otel-quickstart
```

## Pull OTel helm repo locally

```
helm repo add splunk-otel-collector-chart https://signalfx.github.io/splunk-otel-collector-chart
```

## Output Helm settings to a file for review and fill in your settings

```
helm show values splunk-otel-collector-chart/splunk-otel-collector > values.yaml
```

HINT: If at any time you mess up your values.yaml file, you can re-run the command above to start fresh with a new version of the values file. 
Open the values file:

```
vi values.yaml
```

Take a minute to look through the different sections contained in the default values.yaml file. 
Note that the yaml file is sensitive to indentation and may cause parsing issues when deploying the helm chart in the next step. If you get an error failing to parse `values.yaml` double check indentation.

Find and Update the following values. 

```
clusterName: "<yourClusterNameHere>"
endpoint: "https://<yourSplunkHECEndpointHere>/services/collector"
token: "<yourHECToken>"
index: "defaultOtelEventsIndex"
metricsIndex: "defaultOtelMetricsIndex"
insecureSkipVerify: <true|false>
metricsEnabled: <true|false>
logsEngine: otel
containerRuntime: "<docker|containerd|cri-o>"
excludeAgentLogs: <true|false>
```

Working basic example from MicroK8s to Splunk Enterprise with Splunk default untrusted certs

```
clusterName: "otel-quickstart"
endpoint: "https://i-000000000000.ec2.foo.io:8088/services/collector"
token: "00000000-0000-0000-0000-000000000000"
index: "otel_events"
metricsIndex: "otel_metrics"
insecureSkipVerify: true
metricsEnabled: true
logsEngine: otel
containerRuntime: "containerd"
excludeAgentLogs: false
```

## Create a Namespace 
A Kubernetes namespace is a logical partition between apps running in the cluster. We will create one to deploy our collector pods into. This is a common practice and your customers will generally create a fresh namespace for us to work in. For the remainder of this lab we are using the “-n” flag in our commands to deploy to specific namespaces. 

```
kubectl create ns otel
```

## Install the helm chart with your customized values.yaml
replace <your-clusterName> with the cluster name value we set in your `values.yaml`, for example “mattymo-otel-quickstart”

```
helm -n otel install <yourClusterNameHere> -f values.yaml splunk-otel-collector-chart/splunk-otel-collector
```
  
## Check your pods for errors
Get running pods

```
kubectl -n otel get pods 
```
  
Output should show that both your “agent” and “cluster-receiver” are both running. Be patient, it may take a minute. 

Now, Check both your pods for errors. 

```
kubectl -n otel logs -f <collectorAgentPodName> 
```
  
View cluster-receiver pod logs with the following command:

```
kubectl -n otel logs -f <collectorClusterRecieverPodName> 
```

If there is an issue with the configuration, you will see a constant stream of warnings or errors. If you do happen to see warnings or errors you can refer to the TROUBLESHOOTING section for common errors. You will need to go back and fix your configuration then use the following command to deploy your changes:


```
helm -n otel upgrade <yourCLusterNameHere> -f values.yaml splunk-otel-collector-chart/splunk-otel-collector
```

Once you deploy your config updates, return to the previous step to check your pods are running and there are no errors in your pod logs. 

## Check Splunk 
Navigate back to Splunk Web and the Search & Reporting app. Use the default search time range to ensure you are seeing events. 

```
index=otel_events
```

Navigate to the Analytics tab and confirm you see metrics.


Proceed to [Module 2](https://github.com/matthewmodestino/otel-quickstart/blob/main/kubernetes/2-module-two.md)
