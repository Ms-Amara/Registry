---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-03"

keywords: namespaces, Docker images, CLI, commands, installing, registry CLI, removing namespaces, setting up cli, installing cli, uninstalling cli, 

subcollection: Registry

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the {{site.data.keyword.registrylong_notm}} CLI and your registry namespace
{: #registry_setup_cli_namespace}

To manage your Docker images in {{site.data.keyword.registrylong}}, you must install the `container-registry` CLI plug-in and create a namespace in a [resource group](x2161955){: term}.
{: shortdesc}

Do not put personal information in your container images, namespace names, description fields, or in any image configuration data (for example, image names or image labels).
{: important}

To add and remove namespaces, you must have the Manager role at the account level, see [Access roles for configuring {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-iam#access_roles_configure).
{: tip}

Before you begin, install the {{site.data.keyword.cloud_notm}} CLI, see [Getting started with the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).

## Installing the `container-registry` CLI plug-in
{: #cli_namespace_registry_cli_install}
{: help}
{: support}

Install the `container-registry` CLI plug-in to use the command line to manage your namespaces and Docker images in {{site.data.keyword.registrylong_notm}}.
{: shortdesc}

1. [Install the `container-registry` CLI plug-in.](/docs/Registry?topic=Registry-getting-started#gs_registry_cli_install)
2. Optional: [Configure your Docker client to run commands without root permissions](https://docs.docker.com/install/linux/linux-postinstall/){: external}. If you do not do this step, you must run `ibmcloud login`, `ibmcloud cr login`, `docker pull`, and `docker push` commands with `sudo` or as root.

{{site.data.keyword.registrylong_notm}} supports other clients as well as Docker. To log in by using other clients, see [Accessing your namespaces interactively](/docs/Registry?topic=Registry-registry_access#registry_access_interactive).
{: tip}

You can now [set up your own namespace](#registry_namespace_setup) in {{site.data.keyword.registrylong_notm}}.

## Updating the `container-registry` CLI plug-in
{: #registry_cli_update}
{: help}
{: support}

You might want to update the `container-registry` CLI plug-in periodically to use new features.
{: shortdesc}

1. Update the `container-registry` CLI plug-in.

    ```txt
    ibmcloud plugin update container-registry
    ```
    {: pre}

2. Verify that the `container-registry` CLI plug-in was updated successfully.

    ```txt
    ibmcloud plugin list
    ```
    {: pre}

## Uninstalling the `container-registry` CLI plug-in
{: #registry_cli_uninstall}
{: help}
{: support}

If you no longer need the `container-registry` CLI plug-in, you can uninstall it.
{: shortdesc}

1. Uninstall the `container-registry` CLI plug-in.

    ```txt
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

2. Verify that the `container-registry` CLI plug-in was uninstalled successfully.

    ```txt
    ibmcloud plugin list
    ```
    {: pre}

    The `container-registry` CLI plug-in is not displayed in the results.

## Planning namespaces
{: #registry_setup_cli_namespace_plan}

{{site.data.keyword.registrylong_notm}} provides a multi-tenant private image registry that is hosted and managed by IBM. You can store and share your Docker images in this registry by setting up a registry namespace.
{: shortdesc}

When you create a namespace in version 0.1.485 of the {{site.data.keyword.registryshort_notm}} CLI or later, or in the {{site.data.keyword.cloud_notm}} console on or after 29 July 2020, it is created in the resource group that you specify so that you can configure access to resources within the namespace at the [resource group](/docs/account?topic=account-rgs) level. However, you can still set permissions for the namespace at the account level or in the namespace itself. If you don't specify a resource group, and a resource group isn't targeted, the default resource group is used.

Namespaces created in version 0.1.484 of the {{site.data.keyword.registryshort_notm}} CLI or earlier, or in the {{site.data.keyword.cloud_notm}} console before 29 July 2020, aren't assigned to resource groups. If you have existing namespaces that are not assigned to a resource group and you want to assign them to a resource group, see [Assigning existing namespaces to resource groups](#registry_namespace_assign).

Namespaces that are assigned to a resource group show in the **Resource list** page of the {{site.data.keyword.cloud_notm}} console.
{: note}

You can set up multiple namespaces, for example, to have separate repositories for your production and staging environments. If you want to use the registry in multiple {{site.data.keyword.cloud_notm}} regions, you must set up a namespace for each region. Namespace names are unique within regions. You can use the same namespace name for each region, unless someone else already has a namespace with that name in that region.

You can have 100 namespaces in each region.
{: note}

To work with the IBM-provided public images only, you do not need to set up a namespace.

If you are unsure whether a namespace is already set for your account, run the `ibmcloud cr namespace-list` command with the `-v` option to retrieve existing namespace information.
{: tip}

Consider the following rules when you choose a namespace:

- Your namespace must be unique across all {{site.data.keyword.cloud_notm}} accounts in the same region.
- Your namespace must have 4 - 30 characters.
- Your namespace must start and end with a letter or number.
- Your namespace must contain lowercase letters, numbers, hyphens (-), and underscores (_) only.

Do not put personal information in your namespace names.
{: important}

After you set your first namespace, you are assigned to the free {{site.data.keyword.registrylong_notm}} service plan unless you [upgrade your plan](/docs/Registry?topic=Registry-registry_overview#registry_plan_upgrade).

### User permissions for working with namespaces
{: #registry_setup_cli_namespace_plan_perm}
{: help}
{: support}

You can control which users can work with namespaces by using IAM roles.
{: shortdesc}

- To add, assign, and remove namespaces, you must have the Manager role in the {{site.data.keyword.registrylong_notm}} service at the account level, see [Access roles for configuring {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-iam#access_roles_configure).
    - To add and assign namespaces, you must also have the Viewer platform role for the resource group in which you want to create the namespace. To assign the Viewer role for a resource group to a user, run the following [`ibmcloud iam user-policy-create`](/docs/account?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create) command, where `<user>` is the name of the user and `<resource_group_id>` is the resource group ID:

      ```txt
      ibmcloud iam user-policy-create <user> --roles Viewer --resource-type resource-group --resource <resource_group_id>
      ```
      {: pre}

- To view and analyze namespaces, you must have the Reader or Manager role in the {{site.data.keyword.registrylong_notm}} service, see [Access roles for using IBM Cloud Container Registry](/docs/Registry?topic=Registry-iam#access_roles_using).

For more information about user roles, [Defining user access role policies](/docs/Registry?topic=Registry-user#user).

## Setting up a namespace
{: #registry_namespace_setup}
{: help}
{: support}

You must create a namespace to store your Docker images in {{site.data.keyword.registrylong_notm}}.
{: shortdesc}

Before you begin, complete the following tasks:

- [Install the {{site.data.keyword.cloud_notm}} CLI and the `container-registry` CLI plug-in](/docs/Registry?topic=Registry-getting-started#gs_registry_cli_install).
- [Plan how to use and name your registry namespaces](#registry_setup_cli_namespace_plan).

To create a namespace, see [Set up a namespace](/docs/Registry?topic=Registry-getting-started#gs_registry_namespace_add). Namespaces are created in the resource group that you specify so that you can configure access to resources within the namespace at the resource group level. If you don't specify a resource group, and a resource group isn't targeted, the default resource group is used. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs).

Namespaces that are assigned to a resource group show in the **Resource list** page of the {{site.data.keyword.cloud_notm}} console.

The namespace must be unique across all {{site.data.keyword.cloud_notm}} accounts in the same region. Namespaces must have 4 - 30 characters, and contain lowercase letters, numbers, hyphens (-), and underscores (_) only. Namespaces must start and end with a letter or number.
{: tip}

You can now [push Docker images to your namespace in {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-registry_images_#registry_images_pushing_namespace) and share these images with other users in your account. To control access to namespaces in {{site.data.keyword.cloud_notm}} IAM, see [Creating policies](/docs/Registry?topic=Registry-user#create).

## Assigning existing namespaces to resource groups
{: #registry_namespace_assign}
{: help}
{: support}

Namespaces created in version 0.1.484 of the CLI or earlier and in the {{site.data.keyword.cloud_notm}} console before 29 July 2020, aren't assigned to resource groups. If you have a namespace that is not assigned to a resource group, you can assign the namespace to a resource group and then set permissions for that namespace at the resource group level.
{: shortdesc}

You can assign a namespace to a resource group only once. When a namespace is in a resource group, you can't move it to another resource group.
{: note}

You can assign an existing namespace to a resource group by using the [`ibmcloud cr namespace-assign`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#ic_cr_namespace_assign) command. To find out which namespaces are assigned to resource groups and which are unassigned, run the [`ibmcloud cr namespace-list`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_namespace_list) command with the `-v` option.

Namespaces that are assigned to a resource group show in the **Resource list** page of the {{site.data.keyword.cloud_notm}} console.

For more information about resource groups, see [Creating a resource group](/docs/account?topic=account-rgs#create_rgs).

To assign an existing namespace to a resource group, complete the following steps:

1. Log in to {{site.data.keyword.cloud_notm}}.

    ```txt
    ibmcloud login
    ```
    {: pre}

2. To find the namespace, list the available namespaces.

    ```txt
    ibmcloud cr namespace-list -v
    ```
    {: pre}

3. Assign the namespace to a resource group.

    Replace `<my_resource_group>` with the name or ID of the resource group and `<my_namespace>` with the name of the namespace.

    ```txt
    ibmcloud cr namespace-assign -g <my_resource_group> <my_namespace>
    ```
    {: pre}

## Removing namespaces
{: #registry_remove}
{: help}
{: support}

If you no longer require a registry namespace, you can remove the namespace from your {{site.data.keyword.cloud_notm}} account.
{: shortdesc}

1. Log in to {{site.data.keyword.cloud_notm}}.

    ```txt
    ibmcloud login
    ```
    {: pre}

2. List available namespaces.

    ```txt
    ibmcloud cr namespace-list
    ```
    {: pre}

3. Remove a namespace.

    When you remove a namespace, any images that are stored in that namespace are also deleted. This action cannot be undone.
    {: important}

    Replace `<my_namespace>` with the namespace that you want to remove.

    ```txt
    ibmcloud cr namespace-rm <my_namespace>
    ```
    {: pre}

    After you delete a namespace, it might take a few minutes before that namespace becomes available again to reuse.


