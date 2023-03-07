# k8s-update-poll-cronjob
A service of sorts that forces deployment update strategies to run. Desirable when for instance targeting a moving 
tag such as latest where a ci/cd solution cant manage the cluster. If the deployment has a `imagepullpolicy: always`
this would then cause a newer version of the same tag to be pulled.

By default, this will force update the deployment in question every weekday at 10am.

Requires
- Kubernetes CLI

Use with 
```powershell
$deployName = "InsertActualDeploymentName"
$namespace = "default"
kubectl apply -f https://github.com/iver-ics/k8s-update-poll-cronjob/blob/master/crontjob-poll-deploy-updates.yaml -n $namespace
kubectl patch cronjob container-update-cronjob -n $namespace -p `
    ('{"spec":{"jobTemplate":{"spec":{"template":{"spec":{"containers":[{"name": "deploy-update", "env":[{"name": "DEPLOY_NAME", "value": "'+$deployName+'"}]}]}}}}}}')
```