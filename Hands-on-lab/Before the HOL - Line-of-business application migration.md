![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Line-of-business application migration
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
November 2020
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Line-of-business application migration before the hands-on lab setup guide](#line-of-business-application-migration-before-the-hands-on-lab-setup-guide)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Task 1: Deploy the on-premises environment](#task-1-deploy-the-on-premises-environment)
    - [Task 2: Verify the on-premises environment](#task-2-verify-the-on-premises-environment)

<!-- /TOC -->

# Line-of-business application migration before the hands-on lab setup guide 

## Requirements

1. You will need Owner or Contributor permissions for an Azure subscription to use in the lab.

2. Your subscription must have sufficient unused quota to deploy the VMs used in this lab. To check your quota:

    - Log in to the Azure portal, select **All services** then **Subscriptions**. Select your subscription, then choose **Usage + quotas**.
  
    - From the **Select a provider** drop-down, select **Microsoft.Compute**.
  
    - From the **All service quotas** drop down, select **Standard DSv3 Family vCPUs**, **Standard FSv2 Family vCPUs** and **Total Regional vCPUs**.
  
    - From the **All locations** drop down, select the location where you will deploy the lab.
  
    - From the last drop-down, select **Show all**.
  
    - Check that the selected quotas have sufficient unused capacity:
  
        - Standard DSv3 Family vCPUs: **at least 8 vCPUs**.
  
        - Standard FSv2 Family vCPUs: **at least 6 vCPUs**.

        - Total Regional vCPUs: **at least 14 vCPUs**.

    > **Note:** If you are using an Azure Pass subscription, you may not meet the vCPU quotas above. In this case, you can still complete the lab, by taking the following steps:

     >- Deploy the 'on-premises' environment (see below) in a different Azure region to the Azure VMs created during migration. With this change, you will only need 8 Total Regional vCPUs. Migration will take a little longer, since data must be transferred between regions.
        
     >- Use a different VM tier instead of FSv2 for the migrated VMs (for example, DSv2 or DSv3). However, you cannot change the tier of the DSv3 VM, since this tier is required for the nested virtualization support used to implement the 'on-premises' environment.

## Before the hands-on lab

Duration: 60 minutes

### Task 1: Deploy the on-premises environment

1. Deploy the template **RootBoySlimHost.json** to a new resource group. This template deploys a virtual machine running nested Hyper-V, with 4 nested VMs. This comprises the 'on-premises' environment which you will assess and migrate during this lab.

    You can deploy the template by selecting the 'Deploy to Azure' button below. You will need to create a new resource group. The suggested resource group name to use is **RootBoySlimHostRG**. You will also need to select a location close to you to deploy the template to. Then choose **Review + create** followed by **Create**. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fklawrawkz%2FMCW-Line-of-business-application-migration%2Fmaster%2FHands-on-lab%2FResources%2FRootBoySlimHost.json" target="_blank">![Blah blah blah.](images/BeforeTheHOL/deploy-to-azure.png "Blah blah blah blah")</a>
    
     <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Frbsdemomgr8projstore.blob.core.windows.net%2Frbs-resources%2FRootBoySlimHost.json?sv=2020-02-10&ss=bf&srt=sco&sp=rwlac&se=2031-02-19T02:29:12Z&st=2021-02-18T18:29:12Z&spr=https&sig=tnzkxWbW%2BSBU3hHc3Vy58JeFCxOIvUEqMzz0pCeHcuY%3D" target="_blank">![Button to deploy the RootBoySlimHost template to Azure.](images/BeforeTheHOL/deploy-to-azure.png "Deploy the RootBoySlimHost template to Azure")</a>

    > **Note:** The template will take around 6-7 minutes to deploy. Once template deployment is complete, several additional scripts are executed to bootstrap the lab environment. **Allow at least 1 hour from the start of template deployment for the scripts to run.**

### Task 2: Verify the on-premises environment

1. Navigate to the **RootBoySlimHost** VM that was deployed by the template in the previous step.

2. Make a note of the public IP address.

3. Open a browser tab and navigate to **http://\<RootBoySlimHostIP-Address\>**. You should see the RootBoySlim application, which is running on nested VMs within Hyper-V on the RootBoySlimHost. (The application doesn't do much: you can refresh the page to see the list of guests or select 'CheckIn' or 'CheckOut' to toggle their status.)

    ![Browser screenshot showing the RootBoySlim application.](images/BeforeTheHOL/RootBoySlim.png)

    > **Note:** If the RootBoySlim application is not shown, wait 10 minutes and try again. It takes **at least 1 hour** from the start of template deployment. You can also check the CPU, network and disk activity levels for the RootBoySlimHost VM in the Azure portal, to see if the provisioning is still active.

You should follow all steps provided *before* performing the Hands-on lab.
