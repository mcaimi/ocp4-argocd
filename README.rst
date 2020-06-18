OCP4 ARGOCD
===========

This repo contains manifests and kustomization template to install, maintain and configure OpenShift 4 features with ArgoCD.

Pre-Setup Requirements
----------------------

Installation is managed by Kustomization templates. Openshift 4.3+ is required.

1- Create Namespace

.. code:: bash

  $ oc new-project argocd

2- Deploy Infrastructure prerequisites (rbac roles, service accounts, security contexts)

.. code:: bash

  $ oc apply -k argocd/infra

3- Deploy the ArgoCD Operator

.. code:: bash

  $ oc apply -k argocd/operator

After a while, the operator will be installed and configured accordingly:

.. code:: bash

  ^ >> oc get pods -n argocd
  NAME                                                        READY   STATUS    RESTARTS   AGE
  argocd-operator-f89f6b6b5-f6c9g                             1/1     Running   0          16m
  prometheus-operator-56467cbcbf-lh6xs                        1/1     Running   0          16m

ArgoCD Cluster Deployment
------------------

After operators are up and running, deploy the Argo Cluster Manifest and Application Manifests

.. code:: bash

  $ oc apply -k argocd/cluster

This will deploy:

#) The Argo CD Cluster with Dex for SSO integration with Openshift
#) An "argocd-infra" Application Group
#) The "rbac", "operator" and "cluster" applications to let ArgoCD manage itself.

Deployment of Additional OCP Components with ArgoCD
---------------------------------------------------

Additional components can be deployed as ArgoCD Applications by deploying the relevant manifests:

- Elasticsearch
- Jaeger Tracing
- Kiali
- Istio Operator
- Istio Control Plane
- Openshift Pipelines Operator (Tekton)

To install these components, deploy with kustomize:

#) Elasticsearch Operator

.. code:: bash

  $ oc apply -k apps/elasticsearch

#) Jaeger Operator

.. code:: bash

  $ oc apply -k apps/jaeger

#) Kiali Operator

.. code:: bash

  $ oc apply -k apps/kiali

#) Service Mesh Operator

.. code:: bash

  $ oc apply -k apps/servicemesh

#) Istio Control Plane

.. code:: bash

  $ oc apply -k apps/istio-ctlplane

#) Openshift Pipelines Operator

.. code:: bash

  $ oc apply -k apps/ocp-pipelines

#) KNative Serving and Eventing

.. code:: bash

  $ oc apply -k apps/serverless

