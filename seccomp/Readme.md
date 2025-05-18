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