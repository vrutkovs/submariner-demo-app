# ACM Demo APP

When you deploy this application on your OCP, you need to replace the `host` of `route` in the `guestbook/route.yaml` with your
own cluster name and your own OCP domain.

```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: guestbook
  namespace: guestbook
spec:
  host: guestbook.apps.<your-ocp-cluster-name>.<your-ocp-cluster-domain>
  port:
    targetPort: 80
  to:
    kind: Service
    name: frontend
    weight: 100
```

Replace `<your-ocp-cluster-name>` to your own cluster name
Replace `<your-ocp-cluster-domain>` to your own OCP cluster domain


## Troubleshooting

When you deploy the `frontend` on ocp, you may get the following errors
```
AH00558: apache2: Could not reliably determine the server's fully qualified domain name ...
(13)Permission denied: AH00072: make_sock: could not bind to address [::]:80
(13)Permission denied: AH00072: make_sock: could not bind to address 0.0.0.0:80
no listening sockets available, shutting down
AH00015: Unable to open logs
```

you need apply a `scc` for the `fronted` by running the following command, e.g.

```yaml
cat << EOF | oc apply -f -
apiVersion: security.openshift.io/v1
kind: SecurityContextConstraints
metadata:
  name: guestbook-frontend-scc
allowHostDirVolumePlugin: true
allowHostIPC: true
allowHostNetwork: true
allowHostPID: true
allowHostPorts: true
allowPrivilegeEscalation: true
allowPrivilegedContainer: true
allowedCapabilities:
- '*'
allowedUnsafeSysctls:
- '*'
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
groups:
- system:cluster-admins
- system:nodes
- system:masters
priority: 1
readOnlyRootFilesystem: false
requiredDropCapabilities: []
runAsUser:
  type: RunAsAny
seLinuxContext:
  type: RunAsAny
seccompProfiles:
- '*'
supplementalGroups:
  type: RunAsAny
users:
- system:serviceaccount:guestbook:default
volumes:
- '*'
EOF
```
