# Security Contexts

* Security can be configured at the container level or at the pod level
* If the security is configured at both level, the config at the container level takes precendence

## Configuring the `securityContext`

```yaml
apiVersion: v1
kind: Pd
metadata:
    name: web-pod
spec:
    # at the pod level
    securityContext:
        # sets the user id for the pod
        runAsUser: 1000
       
    containers:
        - name: ubuntu
          image: ubuntu
          command: ["sleep", "3600"]
          # at the container level level
          securityContext:
          # sets the user id for the pod
              runAsUser: 1000
              # updates capabilities of the container. this is only supported at the container level
              capabilities:
                add: ["MAC_ADMIN"]
```