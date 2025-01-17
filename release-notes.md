---

copyright:
  years: 2023
lastupdated: "2023-12-15"

keywords:

subcollection: secure-infrastructure-vpc

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for the landing zone deployable architecture
{: #secure-infrastructure-vpc-relnotes}

Use these release notes to learn about the latest updates to the landing zone deployable architectures: VPC landing zone, VSI on VPC landing zone, and Red Hat OpenShift Container Platform on VPC landing zone. The entries are grouped by date.
{: shortdesc}

## December 2023
{: #landing-zone-2023-12}

### 15 December 2023
{: #secure-infrastructure-vpc-dec-1523}
{: release-note}

Version 5.3.1 of the landing zone deployable architectures available
:   Version 5.3.1 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    - The version includes a new extension variation that is called VSI on existing VPC landing zone. It adds a VSI in an existing VPC.

        With this variation, you extend either the VPC landing zone or the Red Hat OpenShift Container Platform on VPC landing zone. For more information, see [Adding a VSI to your VPC landing zone deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ext-with-vsi).
    - The IBM Terraform provider version is now locked to 1.60.0. This provider version fixes the known issue that the provider plug-in did not respond. For more information, see [issue 4898](https://github.com/IBM-Cloud/terraform-provider-ibm/issues/4898){: external}.
    - The default virtual server image is updated to `ibm-ubuntu-22-04-3-minimal-amd64-2`. To avoid downtime and losing data, the image is not changed when you update to version 5.3.1. Update the image outside of the Terraform code.
    - This version of the Red Hat OpenShift Container Platform on VPC landing zone fixes the issue where the value of the `wait_till` input variable is not used.

### 04 December 2023
{: #secure-infrastructure-vpc-dec-0423}
{: release-note}

Version 5.1.0 of the landing zone deployable architectures available
:   Version 5.1.0 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    - Backward-incompatible change for VSIs with floating IPs.
        - If your landing zone deployable architecture provisions a VSI, and you enabled a floating IP address for it, the IP addresses are deleted and re-created when you apply the changes in this version.
        - If you deployed with the default settings, only the [QuickStart variation](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-vsi-ra-qs) of the VSI on VPC landing zone deployable architecture is affected because it provisions a floating IP address for use as a jump box. However, any deployable architecture in which you provisioned floating IP addresses are affected.
        -  The removal happens because the IP addresses were created in the wrong resource group (the default group). The IP addresses are re-created in the same resource group as the VSI.

        Plan for the change to make sure that anything using the IP addresses is not disrupted.
    - Other changes
        - Added support for for Madrid (`eu-es`) region.
        - Added support for configuring the idle connection timeout value for any VSI load balancers that are provisioned.
        - Removed support for VPN gateway connections. Current connections, if manually made, are not removed when you update to this version. However, if the connections were created in an override, they will be removed when you update to this version.

## November 2023
{: #landing-zone-2023-11}

### 02 November 2023
{: #secure-infrastructure-vpc-nov-0223}
{: release-note}

Version 4.13.3 of the landing zone deployable architectures available
:   Version 4.13.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    - The IBM Terraform provider version is now locked to 1.59.0.
    - The `landing-zone-vsi` submodule is updated from 2.6.0 to 2.12.1. For more information about changes in the submodule, see [issue 577](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/pull/577) in GitHub.
    - The VSI on VPC landing zone and the Red Hat OpenShift Container Platform on VPC landing zone deployable architectures now include a `vpc_resource_list` output.
    - The Red Hat OpenShift Container Platform on VPC landing zone deployable architecture now includes a `cluster_data` output with information about the created clusters, including IDs and names of the clusters.
    - The initial version of the OpenShift cluster is now set to 4.13 by default. Versions 4.11 and 4.12 are also supported. To avoid downtime and losing data, the cluster version is not changed when you update your deployable architecture. Update the cluster outside of the Terraform code.

        Version 4.10 of Red Hat OpenShift is no longer supported.

## October 2023
{: #landing-zone-2023-10}

### 03 October 2023
{: #secure-infrastructure-vpc-oct0323}
{: release-note}

Version 4.12.3 of the landing zone deployable architectures available
:   Version 4.12.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    - For the `existing_ssh_key_name` variable, you can now select from a list of all keys in the account when you deploy with projects or {{site.data.keyword.bplong_notm}}.
    - Deployable architectures now use the IBM Cloud Terraform provider resource `clean_default_sg_acl` to clean the default ACL and security group rules. The new resource replaces the `null_resource.clean_default_security_group[0]` and `null_resource.clean_default_acl[0]` resources. When you upgrade from v4.4.7, the null resources are destroyed. This behavior is expected and does not affect your provisioned infrastructure.
    - You can now attach existing access tags to resources that are provisioned by the deployable architecture in an override. If you deploy with projects  or {{site.data.keyword.bplong_notm}}, edit the `override_json_string` optional variable as in the following example that adds a tag to the key management resources:

        ```json
        {
          "key_management": {
            "access_tags": ["tag-group:tag-name"]
          }
        }
        ```
        {: codeblock}

    - For deployable architectures with a transit gateway enabled, a new `transit_gateway_global` variable supports connecting to networks outside the associated region.
    - Key management changes:
        - You can now use key management keys that you create outside the deployable architecture or from different accounts by specifying the key CRN in the `existing_key_crn` field in an override. If you deploy with projects, edit the `override_json_string` optional variable. For more information, see https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v4.9.0.
        - The following outputs are now included with key management resources: `key_management_name`, `key_management_crn`, `key_management_guid`, `key_rings`, and `key_map`.
    - Added placement group details to the outputs. A placement group provides flexibility with performance and high availability for {{site.data.keyword.hpc-cluster_short}} solutions.
    - Changes related to VSI on VPC landing zone:
        - The default virtual server image is updated to `ibm-ubuntu-22-04-3-minimal-amd64-1`. To avoid downtime and losing data, the image is not changed when you update to version 4.12.3. Update the image outside of the Terraform code.
        - You can't upgrade the QuickStart variation from v4.4.7 because of an issue with the provider configuration. Create another instance of the VSI on VPC landing zone QuickStart variation.
    - Changes related to Red Hat OpenShift Container Platform on VPC landing zone:
        - Added OpenShift Container Platform boot volume KMS encryption support. The boot volume is enabled for new clusters, but is not enabled when you upgrade because encryption can happen only during the initial provisioning.

## July 2023
{: #landing-zone-2023-07}

### 31 July 2023
{: #secure-infrastructure-vpc-july3123}
{: release-note}

Version 4.4.7 of the landing zone deployable architectures available
:   Version 4.4.7 of the landing zone deployable architecture is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    - For Red Hat OpenShift Container Platform on VPC landing zone:
        - Changes to the version of the OpenShift cluster are now ignored. Make sure that you update your cluster version outside of the Terraform in the deployable architecture to prevent destructive changes to your infrastructure.
        - The initial version of the OpenShift cluster is now set to version 4.12 by default. Versions 4.11 and 4.10 are also supported.

### 06 July 2023
{: #secure-infrastructure-vpc-july0623}
{: release-note}

Version 4.4.1 of the landing zone deployable architectures available
:   Version 4.4.1 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    - {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} is now supported by the deployable architectures. {{site.data.keyword.hscrypto}} supports keep your own key (KYOK) features. The default encryption remains Key Protect. For more information, see the [planning information](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-plan#vpc-crypto-prereqs).
    - The IBM provider version is updated to 1.54.0, which fixes a known issue with failures when creating an authorization policy.
    - For the VSI on VPC landing zone, the default virtual server image is updated to `ibm-ubuntu-22-04-2-minimal-amd64-1` from `ibm-ubuntu-22-04-1-minimal-amd64-4`, which is deprecated. To avoid downtime and losing data, the image is not changed when you update to version 4.4.1.

   A known issue exists with validation when you deploy by using projects.

## June 2023
{: #landing-zone-2023-06}

### 01 June 2023
{: #secure-infrastructure-vpc-june0123}
{: release-note}

Version 4.0.0 of the landing zone deployable architectures available
:   Version 4.0.0 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}.

    By default, the default VPC security group and ACLs are removed. Because the landing zone deployable architectures create security groups, the VPC defaults are not needed and might be more permissive than required.

    To prevent the default group and rules from being removed, specify your VPC configuration by using an override block. Add the  `clean_default_security_group` and `clean_default_acl` properties set to `false` to the block. For example, if you deploy with projects, edit the `override_json_string` optional variable as in the following example:

    ```json
    "vpcs": [
      {
        "prefix": "management",
        "clean_default_security_group": false,
        "clean_default_acl": false,
        . . .  # the rest of your VPC configuration
      }]
    ```
    {: codeblock}

    For more information about implementing the Terraform logic, see the [release notes](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v4.0.0) on GitHub.

## May 2023
{: #landing-zone-2023-05}

### 18 May 2023
{: #secure-infrastructure-vpc-may1823}
{: release-note}

Version 3.8.3 of the landing zone deployable architectures available
:   Version 3.8.3 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} [catalog](/catalog#reference_architecture){: external}. This version fixes the issue that prevented deletion of the SSH key in the VSI on VPC landing zone. For other changes in the release, see the [release notes](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/releases/tag/v3.8.3){: external} on GitHub.

### 04 May 2023
{: #secure-infrastructure-vpc-may0523}
{: release-note}

Version 3.6.4 of the landing zone deployable architectures available
:   Version 3.6.4 of the landing zone deployable architectures is available in the {{site.data.keyword.cloud_notm}} catalog. This version includes updates to several variable descriptions and support for {{site.data.keyword.compliance_long}} v2 rules.

## 17 April 2023
{: #secure-infrastructure-vpc-apr1723}
{: release-note}

Introducing the landing zone deployable architectures
:   Three VPC landing zone deployable architectures are released: VPC landing zone, VSI on VPC landing zone, and Red Hat OpenShift Container Platform on VPC landing zone. You can use the deployable architectures to create a secure and customizable Virtual Private Cloud (VPC) environment. These [deployable architectures](#x10293733){: term} are based on the {{site.data.keyword.cloud_notm}} for Financial Services reference architecture. For more information about using deployable architectures with projects, see the blog posts [Projects and Cost Estimation: How IBM Cloud is Revolutionizing Complex Workloads for Enterprises](https://www.ibm.com/blog/announcement/projects-and-cost-estimation/) and [Turn Your Terraform Templates into Deployable Architectures](https://www.ibm.com/blog/turn-your-terraform-templates-into-deployable-architectures/).
