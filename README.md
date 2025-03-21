# OpenShift:  ServiceAccount with Token and RBAC


Understanding `ServiceAccount`s in OpenShift: [Service Account docs](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/authentication_and_authorization/understanding-and-creating-service-accounts#service-accounts-overview_understanding-service-accounts)


# Create a ServiceAccount, RoleBinding, and Secret

This step creates a test namespace, a new `ServiceAccount`, a `RoleBinding` and a `Secret` that will generate a token for the service account.

```
oc apply -k gitops
```

# Confirm the new Service Account exists

```
oc project sa-test
oc get sa
```

# Extract the token from the Secret

```
oc get secret ci-serviceaccount-token -n sa-test -o jsonpath="{.data['token']}" | base64 -d
```

# Login to the cluster with the Service Account token

```
oc login --server=https://api.<cluter url>:6443 --token=<sa token>
```

```
$ oc whoami
system:serviceaccount:sa-test:ci-serviceaccount
```