---

copyright:
  years: 2021
lastupdated: "2022-01-26"

keywords: Responsibilities

subcollection: Registry

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you are using {{site.data.keyword.registryshort_notm}}
{: #registry_responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.registrylong}}. For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.registrylong_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

## Incident and operations management
{: #incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, [high availability](x2284708){: term}, problem determination, recovery, and full state backup and recovery.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|------|-------------------------------------------------|-----------------------|
| Data incidents. | It is the responsibility of {{site.data.keyword.IBM_notm}} to inform you if your data is lost. |  |
| Ensure that the application is available. | It is the responsibility of {{site.data.keyword.IBM_notm}} to inform you if the application is not available. | |
| Track events. | It is the responsibility of {{site.data.keyword.IBM_notm}} to ensure that {{site.data.keyword.cloudaccesstraillong_notm}} is tracking events. | It is your responsibility to monitor events by using {{site.data.keyword.cloudaccesstraillong_notm}} to ensure that your application is being accessed only by users with the correct authority. It is also your responsibility to ensure that an {{site.data.keyword.cloudaccesstrailshort}} instance is set up to receive events. For more information, see [Auditing the events for Container Registry](/docs/Registry?topic=Registry-at_events). |
{: row-headers}
{: caption="Table 1. Responsibilities for incident and operations" caption-side="bottom"}
{: summary="The first column describes the task that the customer or {{site.data.keyword.IBM_notm}} might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Change management
{: #change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|------|-------------------------------------------------|-----------------------|
| Provisioning. | It is the responsibility of {{site.data.keyword.IBM_notm}} to provision the service. | |
| Deprovisioning. | It is the responsibility of {{site.data.keyword.IBM_notm}} to deprovision the service. | |
| Update package versions. | | It is your responsibility to update package versions inside container images. You can use Vulnerability Advisor to identify the required updates. For more information, see [Managing image security with Vulnerability Advisor](/docs/Registry?topic=va-va_index). |
{: row-headers}
{: caption="Table 2. Responsibilities for change management" caption-side="bottom"}
{: summary="The first column describes the task that the customer or {{site.data.keyword.IBM_notm}} might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Identity and access management
{: #iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|------|-------------------------------------------------|-----------------------|
| Authentication | It is the responsibility of {{site.data.keyword.IBM_notm}} to implement authentication. | |
| Process access policies | It is the responsibility of {{site.data.keyword.IBM_notm}} to ensure that the policies are processed. | |
| Set up access policies | | It is your responsibility to set up access policies. For more information, see [Creating policies](/docs/Registry?topic=Registry-user#create). |
| Access to back-end resources | It is the responsibility of {{site.data.keyword.IBM_notm}} to access to back-end resources. | |
| Access to namespaces | | It is your responsibility to set up access to namespaces. For more information, see [Automating access to {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-registry_access).|
{: row-headers}
{: caption="Table 3. Responsibilities for identity and access management" caption-side="bottom"}
{: summary="The first column describes the task that the customer or {{site.data.keyword.IBM_notm}} might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|------|-------------------------------------------------|-----------------------|
| Secure confidential information. | | It is your responsibility to make sure that no confidential information is put into your images. |
| Ensure that the service instance is secure. | It is the responsibility of {{site.data.keyword.IBM_notm}} to ensure the security of the service instance. | |
{: row-headers}
{: caption="Table 4. Responsibilities for security and regulation compliance" caption-side="bottom"}
{: summary="The first column describes the task that the customer or {{site.data.keyword.IBM_notm}} might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #disaster-recovery}

[Disaster recovery](x2113280){: term} includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and fail over on disaster events. For more information, see [Understanding high availability and disaster recovery for Container Registry](/docs/Registry?topic=Registry-ha-dr).

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|------|-------------------------------------------------|-----------------------|
| Copy your data to another region. | | It is your responsibility to copy the data to another region. For more information, see [Regions](/docs/Registry?topic=Registry-registry_overview#registry_regions).|
| Restore the contents of the data in a single region. | It is the responsibility of {{site.data.keyword.IBM_notm}} to restore the contents of the data in a single region. | |
{: row-headers}
{: caption="Table 5. Responsibilities for disaster recovery" caption-side="bottom"}
{: summary="The first column describes the task that the customer or {{site.data.keyword.IBM_notm}} might be responsible for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


