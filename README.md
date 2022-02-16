# ACM Demo APP

When you deploy this application on your OCP, you need to replace the `host` of `route` in the `guestbook/route.yaml` with your
own cluster name and your own OCP domain.

```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: guestbook
spec:
  host: guestbook.apps.<your-ocp-cluster-name>.<your-ocp-cluster-domain>
  port:
    targetPort: 80
  to:
    kind: Service
    name: guestbook
    weight: 100
```

Replace `<your-ocp-cluster-name>` to your own cluster name
Replace `<your-ocp-cluster-domain>` to your own OCP cluster domain

