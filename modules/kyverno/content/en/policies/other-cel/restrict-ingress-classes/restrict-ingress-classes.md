---
title: "Restrict Ingress Classes in CEL expressions"
category: Sample in CEL
version: 1.11.0
subject: Ingress
policyType: "validate"
description: >
    Ingress classes should only be allowed which match up to deployed Ingress controllers in the cluster. Allowing users to define classes which cannot be satisfied by a deployed Ingress controller can result in either no or undesired functionality. This policy checks Ingress resources and only allows those which define `HAProxy` or `nginx` in the respective annotation. This annotation has largely been replaced as of Kubernetes 1.18 with the IngressClass resource.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other-cel/restrict-ingress-classes/restrict-ingress-classes.yaml" target="-blank">/other-cel/restrict-ingress-classes/restrict-ingress-classes.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-ingress-classes
  annotations:
    policies.kyverno.io/title: Restrict Ingress Classes in CEL expressions
    policies.kyverno.io/category: Sample in CEL 
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Ingress
    policies.kyverno.io/minversion: 1.11.0
    kyverno.io/kubernetes-version: "1.26-1.27"
    policies.kyverno.io/description: >-
      Ingress classes should only be allowed which match up to deployed Ingress controllers
      in the cluster. Allowing users to define classes which cannot be satisfied by a deployed
      Ingress controller can result in either no or undesired functionality. This policy checks
      Ingress resources and only allows those which define `HAProxy` or `nginx` in the respective
      annotation. This annotation has largely been replaced as of Kubernetes 1.18 with the IngressClass
      resource.
spec:
  validationFailureAction: Audit
  background: true
  rules:
  - name: validate-ingress
    match:
      any:
      - resources:
          kinds:
          - Ingress
          operations:
          - CREATE
          - UPDATE
    validate:
      cel:
        expressions: 
          - expression: >-
              object.metadata.?annotations[?'kubernetes.io/ingress.class'].orValue('') in ['HAProxy', 'nginx']      
            message: "Unknown ingress class."


```
