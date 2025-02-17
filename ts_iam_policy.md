---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-30"

keywords: troubleshooting, support, help, errors, problems, ts, registry, iam, access denied, iam access policy

subcollection: Registry

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I getting access denied errors even though I have an IAM access policy?
{: #troubleshoot-iam-policy}
{: troubleshoot}
{: support}

You have an IAM [access policy](x2853407){: term} but still get access denied errors.
{: shortdesc}

You created an IAM access policy, but you're still getting access denied errors.
{: tsSymptoms}

You are likely to get access denied errors because of one of the following causes:
{: tsCauses}

- **Cause 1**. The user policy was created in a [resource group](x2161955){: term} but the namespace is not assigned to that resource group.
- **Cause 2**. If you're using a namespace level policy, the resource type is not correct, for example, `Namespace`.

You can resolve the problem by using one of the following solutions:
{: tsResolve}

- **Cause 1**. If your namespace was created in version 0.1.484 of the CLI or earlier, or in the {{site.data.keyword.cloud_notm}} console before 29 July 2020, and isn't assigned to a resource group, you must assign the namespace to the same resource group as the user policy, see [Assigning existing namespaces to resource groups](/docs/Registry?topic=Registry-registry_setup_cli_namespace#registry_namespace_assign).
- **Cause 2**. If you're using a namespace level policy, ensure that the resource type is `namespace` (lowercase).


