# Cronjob

A CronJob creates Jobs on a repeating schedule.

CronJob is meant for performing regular scheduled actions such as backups, report generation, and so on. One CronJob object is like one line of a crontab (cron table) file on a Unix system. It runs a job periodically on a given schedule, written in Cron format.

Kubernetes cron uses the FreeBSD cron syntax for the schedule:
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/#schedule-syntax

This makes easily readable schedules like `@daily` or more advanced schedules like every two
minutes `*/2 * * * *`

Lets run a simple cronjob that prints the current date and time every minute:

```
kubectl apply -f cronjob.yaml
kubectl get cronjobs
```

Then check that the `Cronjob` creates a `Job` resource and it will complete successfully

```
kubectl get jobs -w
```

The job will have created a `Pod` resource where we can check the log:

```console
kubectl get pods
kubectl logs <pod>
```

The job completion depends on the exit code of the command executed. When the command of the pod exits with a `0` the Job is marked as an Completion. When the command exists with anything else it will get an Error.
The amount of retries is controlled by the `backoffLimit` setting.

To demonstrate this, update the cronjob with a failing job (pod exits with 1) and a `backoffLimit` off 1.

```
kubectl delete -f cronjob.yaml
kubectl apply -f cronjob-failing.yaml
```

Check if the cronjob exists with:

```
kubectl get cronjobs
```

Then watch the pods, (this could take a minute or two) you will see a pod being created two times (once scheduled, the next the one backoffLimit).

```
kubectl get pods -w
```

This will showup on the job as a non completion (0/1):

```
kubectl get jobs
```

Clean up:

```bash
kubectl delete -f cronjob-failing.yaml
```
