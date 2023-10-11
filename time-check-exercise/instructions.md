The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.


1. Create a pod called time-check in the devops namespace. This pod should run a container called time-check, container should use the busybox image with latest tag only and remember to mention tag i.e busybox:latest.

2. Create a config map called time-config with the data TIME_FREQ=12 in the same namespace.

3. The time-check container should run the command: while true; do date; sleep $TIME_FREQ;done and should write the result to the location /opt/itadmin/time/time-check.log. Remember you will also need to add an environmental variable TIME_FREQ in the container, which should pick value from the config map TIME_FREQ key.

4. Create a volume log-volume and mount the same on /opt/itadmin/time within the container.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


## Links used for reference

https://stackoverflow.com/questions/70293194/chain-multiple-arguments-args-in-kubernetes

https://askubuntu.com/questions/420981/how-do-i-save-terminal-output-to-a-file

https://kubernetes.io/docs/concepts/configuration/configmap/

https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/

https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/

https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-organizing-with-namespaces

## error messages

OCI runtime create failed: runc create failed: unable to start container process: exec: "bash": executable file not found in $PATH: unknown

## Conclusion

First error was not having the configmap on the same namespace as the pod

while is not a correct command to start the script with.

Configmap values must be string, not a number (add quotes around the 12)

I need to use the correct shebang which is /bin/bash instead of bash and I had a missing semicolon when I had that, do not change two things at a time, go one by one 

## Tip top top tip

delete pod, and then run apply again to see changes

use describe command to see error logs

> $ kubectl get pods --namespace=test
