---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-04"

keywords: troubleshooting, support, help, errors, problems, ts, registry, restoring images, restoring images from the trash by digest, trash, restoring tags

subcollection: Registry

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# When I'm restoring an image from the trash by digest, why aren't some tags restored?
{: #troubleshoot-image-restore-digest}
{: troubleshoot}
{: support}

You want to restore an image by [digest](/docs/Registry?topic=Registry-registry_overview#overview_elements_digest) from the {{site.data.keyword.registrylong}} trash, but some [tags](/docs/Registry?topic=Registry-registry_overview#overview_elements_tag) weren't restored.
{: shortdesc}

You run the [`ibmcloud cr image-restore`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_restore) command, but the tags were not restored. If all the tags were unsuccessful, the digest shows in the live repository, but it is not tagged. You can see the digest if you run [`ibmcloud cr image-digests`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_digests), but not if you run [`ibmcloud cr image-list`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_list).
{: tsSymptoms}

The tags that were not restored are already in your live repository. You can't overwrite a tag with a tag that is in the trash.
{: tsCauses}

Remove the tag from the existing image in your live repository by running the [`ibmcloud cr image-untag`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_untag) command. You can then restore the required image from the trash by running the [`ibmcloud cr image-restore`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_restore) command with the option `<repo>@<digest>`, which restores the digest and its tags to the live repository. For more information, see [Restoring images by digest](/docs/Registry?topic=Registry-registry_images_#registry_images_restore_digest). Alternatively, you can run the [`ibmcloud cr image-tag`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_tag) command and use the restored digest as the source image.
{: tsResolve}

In your live repository, you can pull the image by digest. If you run the [`ibmcloud cr image-digests`](/docs/Registry?topic=container-registry-cli-plugin-containerregcli#bx_cr_image_digests) command, the image shows in the output.
{: tip}


