---
title: "Require Annotations in CEL expressions"
category: Other in CEL
version: 
subject: Pod, Annotation
policyType: "validate"
description: >
    Define and use annotations that identify semantic attributes of your application or Deployment. A common set of annotations allows tools to work collaboratively, describing objects in a common manner that all tools can understand. The recommended annotations describe applications in a way that can be queried. This policy validates that the annotation `corp.org/department` is specified with some value.      
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other-cel/require-annotations/require-annotations.yaml" target="-blank">/other-cel/require-annotations/require-annotations.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-annotations
  annotations:
    policies.kyverno.io/title: Require Annotations in CEL expressions
    policies.kyverno.io/category: Other in CEL 
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod, Annotation
    kyverno.io/kyverno-version: 1.11.0
    kyverno.io/kubernetes-version: "1.26-1.27"
    policies.kyverno.io/description: >-
      Define and use annotations that identify semantic attributes of your application or Deployment.
      A common set of annotations allows tools to work collaboratively, describing objects in a common manner that
      all tools can understand. The recommended annotations describe applications in a way that can be
      queried. This policy validates that the annotation `corp.org/department` is specified with some value.      
spec:
  validationFailureAction: Audit
  background: true
  rules:
  - name: check-for-annotation
    match:
      any:
      - resources:
          kinds:
          - Pod
          operations:
          - CREATE
          - UPDATE
    validate:
      cel:
        expressions:
          - expression: >-
              object.metadata.?annotations[?'corp.org/department'].orValue('') != ''
            message: "The annotation `corp.org/department` is required."


```
