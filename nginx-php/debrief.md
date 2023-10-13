## Instructions

We deployed a Nginx and PHP-FPM based setup on Kubernetes cluster last week and it had been working fine so far. This morning one of the team members made a change somewhere which caused some issues, and it stopped working. Please look into the issue and fix it:



The pod name is nginx-phpfpm and configmap name is nginx-config. Figure out the issue and fix the same.


Once issue is fixed, copy /home/thor/index.php file from the jump host to the nginx-container under nginx document root and you should be able to access the website using Website button on top bar.


The solution was found here

https://www.nbtechsupport.co.in/2021/04/volumemounts-in-kubernetes.html?m=1

## Explanation

The path of the volume mount of nginx `/usr/share/nginx/html` was wrong and it had to be `/var/www/html` just like the volume for php container

> kubectl edit pod/nginx -o yaml 

when exiting it says it's not valid, because changing the volume is not allowed, but the yaml is saved to temp location.

Apply the changes with the force flag using the temp file yaml and the pod will be recreated with the new volume mount path.

> kubectl replace -f <file-name>.yaml --force

Then, to copy the html file use this command

> kubectl cp  /home/thor/index.php  nginx-phpfpm:/var/www/html -c nginx-container

and that's it.

### Useful link

https://www.alibabacloud.com/help/en/eci/user-guide/change-the-image-of-a-pod-without-changing-the-ip-address