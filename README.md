# k8s-update-poll-cronjob
A service of sorts that forces deployment update strategies to run. Desirable when for instance targeting a moving 
tag such as latest where a ci/cd solution cant manage the cluster.

By default, this will force update the deployment in question every weekday at 10am.

Requires
- Kubernetes CLI

Use with 
```powershell
$deployName = "InsertActualDeploymentName"
$namespace = "default"
kubectl apply -f https://github.com/iver-ics/k8s-update-poll-cronjob/blob/master/crontjob-poll-deploy-updates.yaml -n $namespace
kubectl patch crontjob container-update-cronjob -p `
    '{"spec":{"jobTemplate":{"spec":{"template":{"spec":{"containers":[{"env":[{"name": "DEPLOY_NAME", "value": "$deployName"}]}]}}}}}}'
```