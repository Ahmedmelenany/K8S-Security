# Trace syscalls and Seccomp Profiles Operator CRDs

- Tracing system calls from a command 

```bash
  strace -c ls /root
```

- Also deploying tracee as daemonset to see syscalls logs for the new containers in the nodes.


- We can install seccomp as an operator (SeccompProfile and ProfileBinding) 

```yaml
apiVersion: security-profiles-operator.x-k8s.io/v1alpha1
kind: SeccompProfile
metadata:
  namespace: default
  name: logging-profile
spec:
  defaultAction: SCMP_ACT_LOG
----
apiVersion: security-profiles-operator.x-k8s.io/v1alpha1
kind: ProfileBinding
metadata:
  name: logging-profile-binding
spec:
  profileRef:
    kind: SeccompProfile
    name: logging-profile
  image: nginx
```


## Linux capabilites 

- The Linux capabilities feature breaks up the privileges available to processes run as the root user into smaller groups of privileges This way a process running with root privilege can be limited to get only the minimal permissions it needs to perform its operation Docker supports the Linux capabilities as part of the docker run command: with --cap-add and --cap-drop. By default, a container is started with several capabilities that are allowed by default and can be dropped. Other permissions can be added manually. Both --cap-add and --cap-drop support the ALL value, to allow or drop all capabilities.

```bash
 docker run --cap-drop ALL --cap-add SYS_TIME ntpd /bin/sh
```
- We can drop and add to any container in a pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo-4
spec:
  containers:
  - name: sec-ctx-4
    image: gcr.io/google-samples/hello-app:2.0
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]

```