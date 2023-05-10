# Module 1

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

Take a minute to look through the different sections contained in the default values.yaml file. You can use your arrow keys or use the “j” and “k” keys to navigate up and down the file. 
Note that the yaml file is sensitive to indentation and may cause parsing issues when deploying the helm chart in the next step. If you get an error failing to parse values.yaml double check indentation.
