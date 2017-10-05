---
title: HPC Pack 2016 kluster i Azure | Microsoft Docs
description: "Lär dig att distribuera ett HPC Pack 2016-kluster i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/15/2016
ms.author: danlep
ms.openlocfilehash: 88d1f4e29f38ba1a6bef57c2da43bee205575eee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a><span data-ttu-id="e60c6-103">Distribuera ett HPC Pack 2016-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="e60c6-103">Deploy an HPC Pack 2016 cluster in Azure</span></span>

<span data-ttu-id="e60c6-104">Följ stegen i den här artikeln för att distribuera en [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) kluster i Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="e60c6-104">Follow the steps in this article to deploy a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) cluster in Azure virtual machines.</span></span> <span data-ttu-id="e60c6-105">HPC Pack är Microsofts ledigt HPC-lösning baserat på Microsoft Azure och Windows Server-teknik och stöder en bred HPC-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="e60c6-105">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies and supports a wide range of HPC workloads.</span></span>

<span data-ttu-id="e60c6-106">Använd någon av de [Azure Resource Manager-mallar](https://github.com/MsHpcPack/HPCPack2016) att distribuera HPC Pack 2016-klustret.</span><span class="sxs-lookup"><span data-stu-id="e60c6-106">Use one of the [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) to deploy the HPC Pack 2016 cluster.</span></span> <span data-ttu-id="e60c6-107">Du har flera val av klustertopologi med olika antal head klusternoder och antingen Linux eller Windows compute-noder.</span><span class="sxs-lookup"><span data-stu-id="e60c6-107">You have several choices of cluster topology with different numbers of cluster head nodes, and with either Linux or Windows compute nodes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e60c6-108">Krav</span><span class="sxs-lookup"><span data-stu-id="e60c6-108">Prerequisites</span></span>

### <a name="pfx-certificate"></a><span data-ttu-id="e60c6-109">PFX-certifikat</span><span class="sxs-lookup"><span data-stu-id="e60c6-109">PFX certificate</span></span>

<span data-ttu-id="e60c6-110">Ett kluster för Microsoft HPC Pack 2016 kräver ett Personal Information Exchange (PFX)-certifikat för säker kommunikation mellan HPC-noder.</span><span class="sxs-lookup"><span data-stu-id="e60c6-110">A Microsoft HPC Pack 2016 cluster requires a Personal Information Exchange (PFX) certificate to secure the communication between the HPC nodes.</span></span> <span data-ttu-id="e60c6-111">Certifikatet måste uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="e60c6-111">The certificate must meet the following requirements:</span></span>

* <span data-ttu-id="e60c6-112">Den måste ha en privat nyckel kapacitet för nyckelutbyte</span><span class="sxs-lookup"><span data-stu-id="e60c6-112">It must have a private key capable of key exchange</span></span>
* <span data-ttu-id="e60c6-113">Nyckelanvändning innehåller digitala signatur och nyckelchiffrering</span><span class="sxs-lookup"><span data-stu-id="e60c6-113">Key usage includes Digital Signature and Key Encipherment</span></span>
* <span data-ttu-id="e60c6-114">Utökad nyckelanvändning innehåller klientautentisering och serverautentisering</span><span class="sxs-lookup"><span data-stu-id="e60c6-114">Enhanced key usage includes Client Authentication and Server Authentication</span></span>

<span data-ttu-id="e60c6-115">Om du inte redan har ett certifikat som uppfyller dessa krav, kan du begära certifikatet från en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="e60c6-115">If you don’t already have a certificate that meets these requirements, you can request the certificate from a certification authority.</span></span> <span data-ttu-id="e60c6-116">Du kan också använda följande kommandon för att generera ett självsignerat certifikat baserat på operativsystemet som du kör kommandot och exportera PFX-format-certifikat med privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="e60c6-116">Alternatively, you can use the following commands to generate the self-signed certificate based on the operating system on which you run the command, and export the PFX format certificate with private key.</span></span>

* <span data-ttu-id="e60c6-117">**För Windows 10 eller Windows Server 2016**, kör du inbyggt **ny SelfSignedCertificate** PowerShell-cmdlet enligt följande:</span><span class="sxs-lookup"><span data-stu-id="e60c6-117">**For Windows 10 or Windows Server 2016**, run the built-in **New-SelfSignedCertificate** PowerShell cmdlet as follows:</span></span>

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* <span data-ttu-id="e60c6-118">**För operativsystem tidigare än Windows 10 eller Windows Server 2016**, ladda ned den [självsignerat certifikat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) från Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="e60c6-118">**For operating systems earlier than Windows 10 or Windows Server 2016**, download the [self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from the Microsoft Script Center.</span></span> <span data-ttu-id="e60c6-119">Extrahera innehållet och kör följande kommandon i PowerShell-Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="e60c6-119">Extract its contents and run the following commands at a PowerShell prompt:</span></span>

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-to-an-azure-key-vault"></a><span data-ttu-id="e60c6-120">Överför certifikatet till en Azure key vault</span><span class="sxs-lookup"><span data-stu-id="e60c6-120">Upload certificate to an Azure key vault</span></span>

<span data-ttu-id="e60c6-121">Innan du distribuerar HPC-kluster, överför certifikatet till en [Azure key vault](../../key-vault/index.md) som en hemlighet och registrera följande information för användning under distributionen: **valvnamnet**, **valvet resursgruppen**, **Certifikatwebbadress**, och **certifikattumavtrycket**.</span><span class="sxs-lookup"><span data-stu-id="e60c6-121">Before deploying the HPC cluster, upload the certificate to an [Azure key vault](../../key-vault/index.md) as a secret, and record the following information for use during the deployment: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

<span data-ttu-id="e60c6-122">En PowerShell-exempelskript för att ladda upp certifikatet följer.</span><span class="sxs-lookup"><span data-stu-id="e60c6-122">A sample PowerShell script to upload the certificate follows.</span></span> <span data-ttu-id="e60c6-123">Mer information om hur du överför ett certifikat till ett Azure key vault finns [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e60c6-123">For more information about uploading a certificate to an Azure key vault, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md).</span></span>

```powershell
#Give the following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate the pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode the JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload the certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"The following Information will be used in the deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a><span data-ttu-id="e60c6-124">Topologier som stöds</span><span class="sxs-lookup"><span data-stu-id="e60c6-124">Supported topologies</span></span>

<span data-ttu-id="e60c6-125">Välj något av de [Azure Resource Manager-mallar](https://github.com/MsHpcPack/HPCPack2016) att distribuera HPC Pack 2016-klustret.</span><span class="sxs-lookup"><span data-stu-id="e60c6-125">Choose one of the [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) to deploy the HPC Pack 2016 cluster.</span></span> <span data-ttu-id="e60c6-126">Följande är med tre topologier som stöds för klustret.</span><span class="sxs-lookup"><span data-stu-id="e60c6-126">Following are high-level architectures of three supported cluster topologies.</span></span> <span data-ttu-id="e60c6-127">Hög tillgänglighet topologier innehåller flera head klusternoder.</span><span class="sxs-lookup"><span data-stu-id="e60c6-127">High-availability topologies include multiple cluster head nodes.</span></span>

1. <span data-ttu-id="e60c6-128">Kluster med hög tillgänglighet med Active Directory-domän</span><span class="sxs-lookup"><span data-stu-id="e60c6-128">High-availability cluster with Active Directory domain</span></span>

    ![Hög tillgänglighet till klustret i AD-domänen](./media/hpcpack-2016-cluster/haad.png)



2. <span data-ttu-id="e60c6-130">Kluster med hög tillgänglighet utan Active Directory-domän</span><span class="sxs-lookup"><span data-stu-id="e60c6-130">High-availability cluster without Active Directory domain</span></span>

    ![Kluster med hög tillgänglighet utan AD-domänen](./media/hpcpack-2016-cluster/hanoad.png)

3. <span data-ttu-id="e60c6-132">Kluster med en enda huvudnod</span><span class="sxs-lookup"><span data-stu-id="e60c6-132">Cluster with a single head node</span></span>

   ![Kluster med enskild huvudnod](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a><span data-ttu-id="e60c6-134">Distribuera ett kluster</span><span class="sxs-lookup"><span data-stu-id="e60c6-134">Deploy a cluster</span></span>

<span data-ttu-id="e60c6-135">Välj en mall för att skapa klustret, och klicka på **till Azure**.</span><span class="sxs-lookup"><span data-stu-id="e60c6-135">To create the cluster, choose a template and click **Deploy to Azure**.</span></span> <span data-ttu-id="e60c6-136">I den [Azure-portalen](https://portal.azure.com), ange parametrar för mallen enligt beskrivningen i följande steg.</span><span class="sxs-lookup"><span data-stu-id="e60c6-136">In the [Azure portal](https://portal.azure.com), specify parameters for the template as described in the following steps.</span></span> <span data-ttu-id="e60c6-137">Varje mall skapas alla Azure-resurser som krävs för HPC-klusterinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="e60c6-137">Each template creates all Azure resources required for the HPC cluster infrastructure.</span></span> <span data-ttu-id="e60c6-138">Resurserna innehåller ett virtuellt Azure-nätverk, offentliga IP-adress, belastningsutjämnare (endast för ett kluster med hög tillgänglighet), nätverksgränssnitt, tillgänglighetsuppsättningar, lagringskonton och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e60c6-138">Resources include an Azure virtual network, public IP address, load balancer (only for a high-availability cluster), network interfaces, availability sets, storage accounts, and virtual machines.</span></span>

### <a name="step-1-select-the-subscription-location-and-resource-group"></a><span data-ttu-id="e60c6-139">Steg 1: Välj prenumerationen, plats och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e60c6-139">Step 1: Select the subscription, location, and resource group</span></span>

<span data-ttu-id="e60c6-140">Den **prenumeration** och **plats** måste vara samma som du angav när du har överfört PFX-certifikat (se krav).</span><span class="sxs-lookup"><span data-stu-id="e60c6-140">The **Subscription** and the **Location** must be same that you specified when you uploaded your PFX certificate (see Prerequisites).</span></span> <span data-ttu-id="e60c6-141">Vi rekommenderar att du skapar en **resursgruppen** för distributionen.</span><span class="sxs-lookup"><span data-stu-id="e60c6-141">We recommend that you create a **Resource group** for the deployment.</span></span>

### <a name="step-2-specify-the-parameter-settings"></a><span data-ttu-id="e60c6-142">Steg 2: Ange parameterinställningar</span><span class="sxs-lookup"><span data-stu-id="e60c6-142">Step 2: Specify the parameter settings</span></span>

<span data-ttu-id="e60c6-143">Ange eller ändra värden för mallparametrarna.</span><span class="sxs-lookup"><span data-stu-id="e60c6-143">Enter or modify values for the template parameters.</span></span> <span data-ttu-id="e60c6-144">Klicka på ikonen bredvid varje parameter för hjälpinformation.</span><span class="sxs-lookup"><span data-stu-id="e60c6-144">Click the icon next to each parameter for help information.</span></span> <span data-ttu-id="e60c6-145">Se även vägledning för [tillgängliga storlekar på VM](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="e60c6-145">Also see the guidance for [available VM sizes](sizes.md).</span></span>

<span data-ttu-id="e60c6-146">Ange vilka värden som du antecknade i förutsättningarna för följande parametrar: **valvnamnet**, **valvet resursgruppen**, **Certifikatwebbadress**, och  **Tumavtryck för certifikat**.</span><span class="sxs-lookup"><span data-stu-id="e60c6-146">Specify the values you recorded in the Prerequisites for the following parameters: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

### <a name="step-3-review-legal-terms-and-create"></a><span data-ttu-id="e60c6-147">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="e60c6-147">Step 3.</span></span> <span data-ttu-id="e60c6-148">Granska juridiska villkor och skapa</span><span class="sxs-lookup"><span data-stu-id="e60c6-148">Review legal terms and create</span></span>
<span data-ttu-id="e60c6-149">Klicka på **Granska juridiska villkor** att granska villkoren.</span><span class="sxs-lookup"><span data-stu-id="e60c6-149">Click **Review legal terms** to review the terms.</span></span> <span data-ttu-id="e60c6-150">Om du godkänner villkoren klickar du på **inköp**, och klicka sedan på **skapa** att starta distributionen.</span><span class="sxs-lookup"><span data-stu-id="e60c6-150">If you agree, click **Purchase**, and then click **Create** to start the deployment.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="e60c6-151">Anslut till klustret</span><span class="sxs-lookup"><span data-stu-id="e60c6-151">Connect to the cluster</span></span>
1. <span data-ttu-id="e60c6-152">När klustret HPC Pack distribueras, går du till den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e60c6-152">After the HPC Pack cluster is deployed, go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e60c6-153">Klicka på **resursgrupper**, och Sök efter den resursgrupp som klustret har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="e60c6-153">Click **Resource groups**, and find the resource group in which the cluster was deployed.</span></span> <span data-ttu-id="e60c6-154">Du kan hitta huvudnod virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e60c6-154">You can find the head node virtual machines.</span></span>

    ![Klustrets huvudnoder i portalen](./media/hpcpack-2016-cluster/clusterhns.png)

2. <span data-ttu-id="e60c6-156">Klicka på en huvudnod (i ett kluster med hög tillgänglighet, klicka på någon av huvudnoderna).</span><span class="sxs-lookup"><span data-stu-id="e60c6-156">Click one head node (in a high-availability cluster, click any of the head nodes).</span></span> <span data-ttu-id="e60c6-157">I **Essentials**, du kan hitta den offentliga IP-adress eller fullständigt DNS-namn för klustret.</span><span class="sxs-lookup"><span data-stu-id="e60c6-157">In **Essentials**, you can find the public IP address or full DNS name of the cluster.</span></span>

    ![Anslutningsinställningar för kluster](./media/hpcpack-2016-cluster/clusterconnect.png)

3. <span data-ttu-id="e60c6-159">Klicka på **Anslut** att logga in på någon av huvudnoderna med hjälp av fjärrskrivbord med den angivna administratörsanvändarnamn.</span><span class="sxs-lookup"><span data-stu-id="e60c6-159">Click **Connect** to log on to any of the head nodes using Remote Desktop with your specified administrator user name.</span></span> <span data-ttu-id="e60c6-160">Om det kluster som du har distribuerat är i Active Directory-domänen, användarnamnet är i formatet <privateDomainName> \<adminUsername > (till exempel hpc.local\hpcadmin).</span><span class="sxs-lookup"><span data-stu-id="e60c6-160">If the cluster you deployed is in an Active Directory Domain, the user name is of the form <privateDomainName>\<adminUsername> (for example, hpc.local\hpcadmin).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e60c6-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e60c6-161">Next steps</span></span>
* <span data-ttu-id="e60c6-162">Skicka jobb till klustret.</span><span class="sxs-lookup"><span data-stu-id="e60c6-162">Submit jobs to your cluster.</span></span> <span data-ttu-id="e60c6-163">Se [skicka jobb till HPC Pack ett HPC-kluster i Azure](hpcpack-cluster-submit-jobs.md) och [hantera ett HPC Pack 2016-kluster i Azure med hjälp av Azure Active Directory](hpcpack-cluster-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="e60c6-163">See [Submit jobs to HPC an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md) and [Manage an HPC Pack 2016 cluster in Azure using Azure Active Directory](hpcpack-cluster-active-directory.md).</span></span>

