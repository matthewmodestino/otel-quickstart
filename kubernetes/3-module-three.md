We will now update our collector config to add a linebreaker to ensure events render in Splunk the way we want. 
Find the multilineConfigs section of the values.yaml and paste in the config block below. 
Remember you can search for the line when not in “insert” mode by entering “/multilineConfig” then pressing Enter. Also you can also enter “:set number” which will show line numbers, and look around line 559. 
Be sure to ensure formatting is kept!
We recommend you comment out the existing multilineConfigs line, and paste this block directly underneath it. 

vi values.yaml

Update <yourNameHere> before pasting the following block!

```
 multilineConfigs: 
      - namespaceName:
          value: mario
        podName:
          value: <yourNameHere>-mario-.*
          useRegexp: true
        containerName:
          value: docker-supermario
        firstEntryRegex: ^\d{2}\-\w+\-\d{4}\s\d{2}:\d{2}:\d{2}\.\d{3}\s\w+\s\[|NOTE\:\sPicked\sup\sJDK_JAVA_OPTIONS
```

Make sure your pods restart and are running:

```
kubectl -n otel get pods
```
  
Check that there are no errors in your agent pod:

```
kubectl -n otel logs -f <collector-agent-pod> 
```
  
Now check the configmap to see what this renders. You will find the configs this renders under the “filelog receiver” section, which will require you to scroll up a bit through the output. 
While you are here, take a look around. This is the OTel collector configuration file and is one of the more advanced examples of pipeline building you will find in the wild today. It provides great inspiration for what is possible with the OTel components we ship, even Linux or Windows applications of the OTel collector. 

Find the name of your “agent” configmap
```
kubectl -n otel get cm 
```
  
View your configmap in yaml format

```
kubectl -n otel get cm <yourNameHere>-tko-otel-splunk-otel-collector-otel-agent -o yaml
```
  
scroll up until you see the regex we pasted into the multilineConfig section and let’s review what the helm chart has built for us
