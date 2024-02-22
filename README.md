# pac-test
Pac tekton run with annotations in task 

The task in ./.tekton/task.yaml has the following annotations .
The first 5 are for the RHTAP uses. 
```
  annotations:
    task.results.format: application/json
    task.results.type: roxctl-image-scan
    task.results.key: SCAN_OUTPUT
    task.output.location: logs
    task.results.container: step-rox-image-scan
    random.annotation: THIS_SHOULD_BE_HERE
```


### Run 

` bash run-me `

It will output something like this. 

```
/home/john/dev/pac-test$ bash run-me
Create Repo CR
oc apply -f source-repository.yaml
repository.pipelinesascode.tekton.dev/pac-test unchanged
Running Pipeline
tkn-pac  resolve -f .tekton/on-push.yaml | oc create -f -
pipelinerun.tekton.dev/pac-pipeline-annotation-v5z77 created
Sleep 10 for PR to run
Annotations in the current task
yq .metadata.annotations  ./.tekton/task.yaml
-------------------------
task.results.format: application/json
task.results.type: roxctl-image-scan
task.results.key: SCAN_OUTPUT
task.output.location: logs
task.results.container: step-rox-image-scan
random.annotation: THIS_SHOULD_BE_HERE
-------------------------

Annotation found in the running taskruns
kubectl get taskruns -o yaml | yq .items[].metadata.annotations
-------------------------
pipeline.tekton.dev/release: 876b81e
pipelinesascode.tekton.dev/on-event: '[push]'
pipelinesascode.tekton.dev/on-target-branch: '[main]'
pipelinesascode.tekton.dev/pipeline: https://raw.githubusercontent.com/jduimovich/pac-test/main/.tekton/pipeline.yaml
pipelinesascode.tekton.dev/task-0: https://raw.githubusercontent.com/jduimovich/pac-test/main/.tekton/task.yaml
-------------------------

Search for THIS_SHOULD_BE_HERE
Fail - missing THIS_SHOULD_BE_HERE

``` 

