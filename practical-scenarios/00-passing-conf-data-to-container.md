# HTPASSWD file creation

1. Generate an htpasswd file and store it as a Secret.
```
htpasswd -c .htpasswd user
```

2. Create a password you can easily remember (we'll need it again later).

3. View the file's contents:
```
cat .htpasswd
```
4. Create a Secret containing the htpasswd data:
```
kubectl create secret generic nginx-htpasswd --from-file .htpasswd
```
5. Delete the .htpasswd file: (Optional)
```
rm .htpasswd
```

# Create the Nginx Pod
1. Create pod.yml:
```
cat <<EOF >>pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.19.1
    ports:
    - containerPort: 80
    volumeMounts:
    - name: config-volume
      mountPath: /etc/nginx
    - name: htpasswd-volume
      mountPath: /etc/nginx/conf
  volumes:
  - name: config-volume
    configMap:
      name: nginx-config
  - name: htpasswd-volume
    secret:
      secretName: nginx-htpasswd
EOF
```

2. View the existing ConfigMap:
```
kubectl get cm
```

3. Get more info about nginx-config:
```
kubectl describe cm nginx-config
```
4. Create the pod:
```
kubectl apply -f pod.yml
```
5. Check the status of your pod and get its IP address:

```
kubectl get pods -o wide
```
It's IP address will be listed once it has a Running status. We'll use this in the final two commands.

6. Within the existing busybox pod, without using credentials, verify everything is working:
```
kubectl exec busybox -- curl <NGINX_POD_IP>
```
We'll see HTML for the 401 Authorization Required page — but this is a good thing, as it means our setup is working as expected.

7. Run the same command again using credentials (including the password you created at the beginning of the lab) to verify everything is working:
```
kubectl exec busybox -- curl -u user:<PASSWORD> <NGINX_POD_IP>
```
This time, we'll see the Welcome to nginx! page HTML.

# Learning Objectives

# 1. Generate an `htpasswd` File and Store It as a Secret

Use htpasswd to generate an htpasswd file.

Create a secret called nginx-htpasswd, and store the contents of the htpasswd file in that Secret. Delete the htpasswd file once the Secret is created.

# 2. Create the Nginx Pod

Create a pod with a single container using the nginx:1.19.1 image.

The Nginx configuration is stored in an existing ConfigMap called nginx-config. Mount the ConfigMap to /etc/nginx in your pod.

Mount your htpasswd secret to /etc/nginx/conf within your pod. The htpasswd data should be in a file in the container at /etc/nginx/conf/.htpasswd.

Expose containerPort 80 on the Nginx container so you can communicate with it to test your setup.