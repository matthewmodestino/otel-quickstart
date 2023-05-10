
The OTel collector helm chart supports Kubernetes Annotations to help manage log ingestion. This allows us a Kubernetes native way to set the sourcetype and index or to exclude logs from being sent to Splunk. 

Lets use kubernetes annotations to set index and sourcetype of events!
Let’s go back to Splunk and run a search to see which namespace is emitting the most log traffic. Let’s search for all events in our otel_events index

```
index="otel_events"
```

As you can see, the kube-system namespace is sending the most logs to Splunk. 

For the purpose of this lab, we are going to pretend to be application teams who don’t really care about system level logs. So let's exclude them using kubernetes annotations!
Return to your cli and run the following command:

```
kubectl annotate namespace kube-system splunk.com/exclude=true
```

Now, let’s return to Splunk web and run the following search over “last 60 minutes” to see that our kube-system logs stop coming in

```
index="otel_events" k8s.namespace.name="kube-system"
```

Awesome, right?

Let’s Check out the configmap again, and review where this awesome annotation magic happens! To make the annotations work, the OTel pipeline leverages some “processors” that enrich the 

Find the name of your agent configmap:
```
kubectl -n otel get cm 
```
View your configmap in yaml format:
```
kubectl -n otel get cm <yourNameHere>-otel-splunk-otel-collector-otel-agent -o yaml
```
The same way we annotated kube-system to exclude the logs, you can set a index values for a namespace or pod, or set a sourcetype for a pod. You can read more about this feature here! 
At the time of writing this, only log ingestion can be managed this way. 

