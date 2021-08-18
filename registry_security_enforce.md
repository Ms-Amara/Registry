---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-18"

keywords: Vulnerability Advisor policies, container image security, policy requirements, policies, Container Image Security Enforcement, content trust, kube-system policies, IBM-system policies, CISE, removing policies, security, security enforcement, 

subcollection: Registry

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:term: .term}
{:external: target="_blank" .external}

# Enforcing container image security by using Container Image Security Enforcement - deprecated
{: #security_enforce}

With Container Image Security Enforcement, you can verify your container images before you deploy them to your cluster in {{site.data.keyword.containerlong}}. You can control where images are deployed from, enforce Vulnerability Advisor policies, and ensure that [content trust](/docs/Registry?topic=Registry-registry_trustedcontent) is properly applied to the image. If an image does not meet your policy requirements, the pod is not deployed to your cluster or updated.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

Container Image Security Enforcement retrieves information about image content trust and vulnerabilities from {{site.data.keyword.registrylong_notm}}. You can choose to block or to allow deployment of images that are stored in other registries, but you cannot use vulnerability or trust enforcement for these images.

## Installing Container Image Security Enforcement in your cluster
{: #sec_enforce_install}

Install Container Image Security Enforcement in your cluster by setting up Helm and installing the Container Image Security Enforcement Helm chart.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

Before you begin, complete the following tasks:

1. [Create](/docs/containers?topic=containers-clusters) or [update](/docs/containers?topic=containers-update#update) the cluster that you want to use with **Kubernetes version 1.9 or later**.
2. [Target your `kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) to the cluster.

To install Container Image Security Enforcement in your cluster, complete the following steps:

1. [Set up Helm in your cluster](/docs/containers?topic=containers-helm#helm).

2. Add the IBM chart repository to your Helm client.

    ```
    helm repo add iks-charts https://icr.io/helm/iks-charts
    ```
    {: pre}

3. Install the Container Image Security Enforcement Helm chart into your cluster. Give it a name such as `cise`.

    - For Helm V3, use the following command:

        ```
        helm install cise iks-charts/ibmcloud-image-enforcement
        ```
        {: pre}

    - For Helm V2, use the following command:

        ```
        helm install --name cise iks-charts/ibmcloud-image-enforcement
        ```
        {: pre}

Container Image Security Enforcement is now installed, and is applying the [default security policy](#default_policies) for all Kubernetes namespaces in your cluster. For more information about customizing the security policy for Kubernetes namespaces in your cluster, or the cluster overall, see [Customizing policies](#customize_policies).

## Default policies
{: #default_policies}

Container Image Security Enforcement installs some policies by default to provide you with a starting point for building your security policy.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

To override these policies, use one of the following options:

- Write a new policy document and apply it to your cluster by using `kubectl apply`.
- Edit the default policy by using `kubectl edit`.

For more information about writing security policies, see [Customizing policies](#customize_policies).

### Cluster-wide policy
{: #cluster-wide}

By default, a cluster-wide policy enforces that all images in all registries have trust information and have no reported vulnerabilities in Vulnerability Advisor.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

The following code shows the default cluster-wide policy `.yaml` file:

```yaml
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ClusterImagePolicy
metadata:
  name: ibmcloud-default-cluster-image-policy
  annotations:
    helm.sh/hook: post-install
    helm.sh/hook-weight: "1"
spec:
   repositories:
    # These policies allow all IBM Cloud Kubernetes Service images from all IBM Cloud Container Registries to deploy and update.
    # At minumum IKS images must allowed in ibm-system, kube-system and default namespaces, failure to allow these will prevent
    # the cluster operating correctly.
    - name: "registry*.bluemix.net/armada/*"
      policy:
    - name: "registry*.bluemix.net/armada-worker/*"
      policy:
    - name: "registry*.bluemix.net/armada-master/*"
      policy:
    - name: "*.icr.io/armada/*"
      policy:
    - name: "*.icr.io/armada-worker/*"
      policy:
    - name: "*.icr.io/armada-master/*"
      policy:
    - name: "icr.io/armada/*"
      policy:
    - name: "icr.io/armada-worker/*"
      policy:
    - name: "icr.io/armada-master/*"
      policy:
    # This allows all IBM addons like managed istio or knative to be deployed to this cluster.
    - name: "icr.io/ibm/*"
      policy:
    - name: "icr.io/ext/*"
      policy:
    - name: "icr.io/obs/*"
      policy:
    # This enforces that all other images deployed to this cluster pass trust and va
    # To override set an ImagePolicy for a specific Kubernetes namespace or modify this policy
    - name: "*"
      policy:
        trust:
          enabled: true
        va:
          enabled: true
```
{: codeblock}

When you set `va` or `trust` to `enabled: true` for a container registry other than {{site.data.keyword.registrylong_notm}}, any attempts to deploy pods from images in that registry are denied. If you want to deploy images from other registries, remove the `va` and `trust` policies.
{: tip}

### `kube-system` policy
{: #kube-system}

By default, a namespace-wide policy is installed for the `kube-system` namespace. This policy allows all images from any container registry to be deployed into the `kube-system` without enforcement, but you can change this part of the policy. The default policy also includes certain repositories that you must leave in place so that your cluster is configured correctly.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

The following code shows the default `kube-system` policy `.yaml` file:

```yaml
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ImagePolicy
metadata:
  name: ibmcloud-image-policy
  namespace: kube-system
  annotations:
    helm.sh/hook: post-install
    helm.sh/hook-weight: "1"
spec:
   repositories:
    # This policy allows all images to be deployed into this namespace. This policy prevents breaking any existing third party applications in this namespace.
    # IMPORTANT: Review this policy and replace it with one that meets your requirements. If you do not run any third party applications in this namespace, you can remove this policy entirely.
    - name: "*"
      policy:
    # These policies allow all IBM Cloud Kubernetes Service images from the global and all regional registries to deploy in this namespace.
    # IMPORTANT: When you create your own policy in this namespace, make sure to include these repositories. If you do not, the cluster might not function properly.
    - name: "registry*.bluemix.net/armada/*"
      policy:
    - name: "registry*.bluemix.net/armada-worker/*"
      policy:
    - name: "registry*.bluemix.net/armada-master/*"
      policy:
    - name: "*.icr.io/armada/*"
      policy:
    - name: "*.icr.io/armada-worker/*"
      policy:
    - name: "*.icr.io/armada-master/*"
      policy:
    - name: "icr.io/armada/*"
      policy:
    - name: "icr.io/armada-worker/*"
      policy:
    - name: "icr.io/armada-master/*"
      policy:
```
{: codeblock}

### IBM-system policy
{: #ibm-system}

By default, a namespace-wide policy is installed for the `ibm-system` namespace. This policy allows all images from any container registry to be deployed into the `ibm-system` without enforcement, but you can change this part of the policy. The default policy also includes certain repositories that you must leave in place so that your cluster is configured correctly and can install or upgrade Container Image Security Enforcement.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

The following code shows the default `ibm-system` policy `.yaml` file:

```yaml
apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
kind: ImagePolicy
metadata:
  name: ibmcloud-image-policy
  namespace: ibm-system
  annotations:
    helm.sh/hook: post-install
    helm.sh/hook-weight: "1"
spec:
   repositories:
    # This policy allows all images to be deployed into this namespace. This policy prevents breaking any existing third party applications in this namespace.
    # IMPORTANT: Review this policy and replace it with one that meets your requirements. If you do not run any third party applications in this namespace, you can remove this policy entirely.
    - name: "*"
      policy:
    # These policies allow all IBM Cloud Kubernetes Service images from the global and all regional registries to deploy in this namespace.
    # IMPORTANT: When you create your own policy in this namespace, make sure to include these repositories. If you do not, the cluster might not function properly.
    - name: "registry*.bluemix.net/armada/*"
      policy:
    - name: "registry*.bluemix.net/armada-worker/*"
      policy:
    - name: "registry*.bluemix.net/armada-master/*"
      policy:
    - name: "*.icr.io/armada/*"
      policy:
    - name: "*.icr.io/armada-worker/*"
      policy:
    - name: "*.icr.io/armada-master/*"
      policy:
    - name: "icr.io/armada/*"
      policy:
    - name: "icr.io/armada-worker/*"
      policy:
    - name: "icr.io/armada-master/*"
      policy:
    # This policy prevents Image Security Enforcement from blocking itself
    - name: "icr.io/ibm/ibmcloud-image-enforcement"
      policy:
    # This policy allows Image Security Enforcement to use a kubectl image to configure your cluster. This policy must exist if you uninstall Image Security Enforcement.
    - name: "icr.io/obs/kubectl"
      policy:
```
{: codeblock}

## Customizing policies
{: #customize_policies}

You can change the policy that Container Image Security Enforcement uses to permit images, either at the cluster or Kubernetes namespace level. In the policy, you can specify different enforcement rules for different images.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

You must have some policy set. Otherwise, deployments to your cluster fail. If you don't want to enforce any image security policies, [remove Container Image Security Enforcement](#remove).
{: tip}

When you apply a deployment, Container Image Security Enforcement checks whether the Kubernetes namespace that you are deploying to has a policy to apply. If it does not, Container Image Security Enforcement uses the cluster-wide policy. Your deployment is denied if no namespace or cluster-wide policy exists.

The following table explains the `.yaml` components that you must set in your Kubernetes custom resource definition `.yaml` file.

| Field | Description |
|-----|-----------|
| `kind` | For a cluster-wide policy, specify the `kind` as `ClusterImagePolicy`. For a Kubernetes namespace policy, specify as `ImagePolicy`. |
|`metadata/name` | Name the custom resource definition. |
| `spec/repositories/name` | Specify the repositories to allow images from. Wildcards (`*`) are allowed in repository names. Repositories are denied unless a matching entry in `repositories` allows it or applies further verification. An empty `repositories` list blocks deployment of all images. To allow all images without verification of any policies, set the name to `*` and omit the policy subsections. |
| `../../../policy` | Complete the subsections for `trust` and `va` enforcement. If you omit the policy subsections, it is equivalent to specifying `enabled: false` for each. |
| `../../../../trust/enabled` | Set as `true` to allow only images that are [signed for content trust](/docs/Registry?topic=Registry-registry_trustedcontent) to be deployed. Set as `false` to ignore whether images are signed. |
| `../../../../trust/signerSecrets/name` | If you want to allow only images that are signed by particular users, specify the Kubernetes secret with the signer name. Omit this field or leave it empty to verify that images are signed without enforcing particular signers. For more information, see [Specifying trusted content signers in custom policies](#signers). |
| `../../../../va/enabled` | Set as `true` to allow only images that pass the [Vulnerability Advisor](/docs/Registry?topic=va-va_index) scan. Set as `false` to ignore the Vulnerability Advisor scan. |
{: caption="Table 1. Understanding the <code>.yaml</code> components for the Kubernetes custom resource definition." caption-side="top"}

Before you begin, [target your `kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) to the cluster. Then, complete the following steps:

1. Create a [Kubernetes custom resource definition](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/){: external} `.yaml` file. For more information, see Table 1.

    ```yaml
    apiVersion: securityenforcement.admission.cloud.ibm.com/v1beta1
    kind: <ClusterImagePolicy_or_ImagePolicy>
    metadata:
      name: <crd_name>
    spec:
      repositories:
        - name: <repository_name>
          policy:
          trust:
              enabled: <true_or_false>
              signerSecrets:
              - name: <secret_name>
          va:
              enabled: <true_or_false>
    ```
    {: codeblock}

2. Apply the `.yaml` file to your cluster.

    ```
    kubectl apply -f <filepath>
    ```
    {: pre}

### Specifying trusted content signers in custom policies
{: #signers}

If you use content trust, you can verify that images are signed by particular signers. Deployment is allowed only if the most recent signed version is signed by all the listed signers. To add a signer to a repository, see [Managing trusted signers](/docs/Registry?topic=Registry-registry_trustedcontent#trustedcontent_signers).
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

To configure the policy to verify that an image is signed by a particular signer:

1. Get the signer name (the name that was used in `docker trust signer add`), and the signer's public key.
2. Generate a Kubernetes secret with the signer name and their public key.

    ```
    kubectl create secret generic <secret_name> --from-literal=name=<signer_name> --from-file=publicKey=<key.pub>
    ```
    {: pre}

3. Add the secret to the `signerSecrets` list for the repository in your policy.

    ```yaml
    - name: example
      policy:
        trust:
          enabled: true
          signerSecrets:
          - name: <secret_name>
    ```
    {: codeblock}

## Controlling who can customize policies
{: #assign_user_policy}

If role-based access control (RBAC) is enabled on your Kubernetes cluster, you can create a role to govern who can administer security policies on your cluster. For more information about applying RBAC rules to your cluster, see [Understanding RBAC permissions](/docs/containers?topic=containers-users#understand-rbac) and [Creating custom RBAC permissions for users, groups, or service accounts](/docs/containers?topic=containers-users#rbac).
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

- In your role, add a rule for security policies:

    ```yaml
    - apiGroups: ["securityenforcement.admission.cloud.ibm.com"]
        resources: ["imagepolicies", "clusterimagepolicies"]
    verbs: ["get", "watch", "list", "create", "update", "patch", "delete"]
    ```
    {: codeblock}

    You can create multiple roles to control what actions users can take. For example, change the `verbs` so that some users can use only the `get` or `list` policies. Alternatively, you can omit `clusterimagepolicies` from the `resources` list to grant access only to Kubernetes namespace policies.
    {: tip}

- Users who have access to delete custom resource definitions (CRDs) can delete the resource definition for security policies, which also deletes your security policies. Make sure to control who is allowed to delete CRDs. To grant access to delete CRDs, add a rule:

    ```yaml
    - apiGroups: ["apiextensions.k8s.io/v1beta1"]
        resources: ["CustomResourceDefinition"]
    verbs: ["delete"]
    ```
    {: codeblock}

    Users and Service Accounts with the `cluster-admin` role have access to all resources. The cluster-admin role grants access to administer security policies, even if you do not edit the role. Make sure to control who has the `cluster-admin` role, and grant access only to people that you want to allow to modify security policies.
    {: tip}

## Deploying container images with enforced security
{: #deploy_containers}

When a policy is applied, you can deploy content to your cluster normally. Your policy is automatically enforced by the Kubernetes cluster. If your deployment matches a policy and is allowed by that policy, your deployment is accepted by the cluster and applied.
{: shortdesc}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

- If Container Image Security Enforcement denies a Deployment, the Deployment is created, but the ReplicaSet created by it fails to scale up, and no pods are created. You can find the ReplicaSet by running `kubectl describe deployment <deployment-name>`, and then see the reason that the deployment was denied by running `kubectl describe rs <replicaset-name>`.

    The following code shows examples of typical error messages:

    - If your image doesn't match any policies, or no policies are used in the namespace or the cluster.

    ```
    admission webhook
    "trust.hooks.securityenforcement.admission.cloud.ibm.com"
    denied the request: Deny, no image policies or cluster
    polices for <image-name>
    ```
    {: screen}

    - If your image matches a policy, but doesn't satisfy that policy's Vulnerability Advisor requirements.

    ```
    admission webhook
    "va.hooks.securityenforcement.admission.cloud.ibm.com"
    denied the request: The Vulnerability Advisor image scan
    assessment found issues with the container image that
    are not exempted. Refer to your image vulnerability report
    for more details by using the command `ibmcloud cr va`.
    ```
    {: screen}

    - If your image matches a policy, but doesn't satisfy that policy's trust requirements.

    ```
    admission webhook
    "trust.hooks.securityenforcement.admission.cloud.ibm.com"
    denied the request: Deny, failed to get content trust information:
    No valid trust data for latest
    ```
    {: screen}

    - If your policy specifies trust enforcement for your image, but your image is not from a supported registry.

    ```
    admission webhook
    "trust.hooks.securityenforcement.admission.cloud.ibm.com"
    denied the request: Trust is not supported for images
    from this registry
    ```
    {: screen}

- You can enable the `va` option in your policy to enforce that Vulnerability Advisor passes before an image can be deployed. Images that are not supported by Vulnerability Advisor are allowed. If you want to deploy images that have issues, you can exempt specific issues or [CVEs](https://cve.mitre.org/data/downloads/index.html){: external} by using the [`ibmcloud cr exemption-add`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_exemption_add) command. For more information, see [Setting organizational exemption policies](/docs/Registry?topic=va-va_index#va_managing_policy).

- You can enable the `trust` option in your policy to enforce content trust. If you do not specify any `signerSecrets`, the deployment is allowed if the image is signed by anyone. If you specify `signerSecrets`, the most recently signed version of the image must be signed by all the signers you specify. Container Image Security Enforcement verifies that the provided public key belongs to the signer. For more information about content trust, see [Signing images for trusted content](/docs/Registry?topic=Registry-registry_trustedcontent).

A deployment is allowed only if all images pass the Container Image Security Enforcement checks.

## Removing Container Image Security Enforcement
{: #remove}

With effect from 19 November 2020, Container Image Security Enforcement is deprecated. To enforce container image security, use [Portieris](https://github.com/IBM/portieris){: external}.
{: deprecated}

Before you begin, [target your `kubectl` CLI](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure) to the cluster.



1. Disable Container Image Security Enforcement.

    ```
    kubectl delete --ignore-not-found=true MutatingWebhookConfiguration image-admission-config
    ```
    {: pre}

    ```
    kubectl delete --ignore-not-found=true ValidatingWebhookConfiguration image-admission-config
    ```
    {: pre}

2. Remove the chart by using one of the following commands.

    - Helm V3

        ```
        helm delete cise
        ```
        {: pre}

    - Helm V2

        ```
        helm delete --purge cise
        ```
        {: pre}


