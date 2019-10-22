+++
title = "Namespaces"
subtitle = "Kubernetes namespaces by example"
date = "2019-02-27"
url = "/ns/"
+++

Namespaces provide a scope for Kubernetes resources. They carve your cluster up into smaller units. You can think of a namespace as a workspace you’re sharing with other users. Many resources such as pods and services exist in a namespace. Others, for example nodes, exist cluster-wide. As a developer you’d usually use an assigned namespace. As an Admin you may wish to manage namespaces, for example to set up access control or resource quotas.

Let’s list all namespaces. The output depends on the environment you’re using. I’m using [Minishift](/diy/) here:

```bash
$ kubectl get ns
NAME              STATUS    AGE
default           Active    13d
kube-system       Active    13d
namingthings      Active    12d
openshift         Active    13d
openshift-infra   Active    13d
```

You can learn more about a namespace using the `describe` verb. For example:

```bash
$ kubectl describe ns default
Name:   default
Labels: <none>
Status: Active

No resource quota.

No resource limits.
```

Let's create a new [namespace](https://github.com/openshift-evangelists/kbe/blob/master/specs/ns/ns.yaml) called `test`:

```bash
$ kubectl apply -f https://raw.githubusercontent.com/openshift-evangelists/kbe/master/specs/ns/ns.yaml
namespace "test" created

$ kubectl get ns
NAME              STATUS    AGE
default           Active    13d
kube-system       Active    13d
namingthings      Active    12d
openshift         Active    13d
openshift-infra   Active    13d
test              Active    3s
```

We can also create the namespace using the `kubectl create namespace test` command.

Let's launch a [pod](https://github.com/openshift-evangelists/kbe/blob/master/specs/ns/pod.yaml) in
the newly created namespace `test`:

```bash
$ kubectl apply --namespace=test -f https://raw.githubusercontent.com/openshift-evangelists/kbe/master/specs/ns/pod.yaml
```

When using the method above, the namespace becomes a runtime property. You can use this method to deploy the same pod, service, etc., into multiple namespaces. For example, you could deploy into `dev` and `prod` at the same time. 

The example below hard-codes the namespace in the metadata section. This gives you less flexibility when deploying apps.

```
apiVersion: v1
kind: Pod
metadata:
  name: podintest
  namespace: test
```

To list namespaced objects such as our pod `podintest`, run the following command:

```bash
$ kubectl get pods --namespace=test
NAME        READY     STATUS    RESTARTS   AGE
podintest   1/1       Running   0          16s
```

You can remove the namespace (and everything inside) with:

```bash
$ kubectl delete ns test
```

If you're an admin, you might want to check out the [docs](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/)
for more info how to handle namespaces.

[Previous](/envs) | [Next](/volumes)
