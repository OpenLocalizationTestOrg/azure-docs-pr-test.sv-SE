---
title: aaaHPC Pack 2016 kluster i Azure | Microsoft Docs
description: "Lär dig hur toodeploy ett HPC Pack 2016 kluster i Azure"
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
ms.openlocfilehash: 9e21ec70c822a783474b96da77ce925940279abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a><span data-ttu-id="0c034-103">Distribuera ett HPC Pack 2016-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="0c034-103">Deploy an HPC Pack 2016 cluster in Azure</span></span>

<span data-ttu-id="0c034-104">Följ hello stegen i den här artikeln toodeploy en [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) kluster i Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="0c034-104">Follow hello steps in this article toodeploy a [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) cluster in Azure virtual machines.</span></span> <span data-ttu-id="0c034-105">HPC Pack är Microsofts ledigt HPC-lösning baserat på Microsoft Azure och Windows Server-teknik och stöder en bred HPC-arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="0c034-105">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies and supports a wide range of HPC workloads.</span></span>

<span data-ttu-id="0c034-106">Använd någon av hello [Azure Resource Manager-mallar](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 klustret.</span><span class="sxs-lookup"><span data-stu-id="0c034-106">Use one of hello [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 cluster.</span></span> <span data-ttu-id="0c034-107">Du har flera val av klustertopologi med olika antal head klusternoder och antingen Linux eller Windows compute-noder.</span><span class="sxs-lookup"><span data-stu-id="0c034-107">You have several choices of cluster topology with different numbers of cluster head nodes, and with either Linux or Windows compute nodes.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c034-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0c034-108">Prerequisites</span></span>

### <a name="pfx-certificate"></a><span data-ttu-id="0c034-109">PFX-certifikat</span><span class="sxs-lookup"><span data-stu-id="0c034-109">PFX certificate</span></span>

<span data-ttu-id="0c034-110">En Microsoft HPC Pack 2016 klustret kräver en Personal Information Exchange (PFX) certifikat toosecure hello kommunikation mellan hello HPC-noder.</span><span class="sxs-lookup"><span data-stu-id="0c034-110">A Microsoft HPC Pack 2016 cluster requires a Personal Information Exchange (PFX) certificate toosecure hello communication between hello HPC nodes.</span></span> <span data-ttu-id="0c034-111">hello certifikat måste uppfylla följande krav hello:</span><span class="sxs-lookup"><span data-stu-id="0c034-111">hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="0c034-112">Den måste ha en privat nyckel kapacitet för nyckelutbyte</span><span class="sxs-lookup"><span data-stu-id="0c034-112">It must have a private key capable of key exchange</span></span>
* <span data-ttu-id="0c034-113">Nyckelanvändning innehåller digitala signatur och nyckelchiffrering</span><span class="sxs-lookup"><span data-stu-id="0c034-113">Key usage includes Digital Signature and Key Encipherment</span></span>
* <span data-ttu-id="0c034-114">Utökad nyckelanvändning innehåller klientautentisering och serverautentisering</span><span class="sxs-lookup"><span data-stu-id="0c034-114">Enhanced key usage includes Client Authentication and Server Authentication</span></span>

<span data-ttu-id="0c034-115">Om du inte redan har ett certifikat som uppfyller dessa krav, kan du begära hello certifikat från en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="0c034-115">If you don’t already have a certificate that meets these requirements, you can request hello certificate from a certification authority.</span></span> <span data-ttu-id="0c034-116">Du kan också använda hello följande kommandon toogenerate hello självsignerat certifikat baserat på hello operativsystem där du kör hello kommando och exportera format hello PFX-certifikat med privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="0c034-116">Alternatively, you can use hello following commands toogenerate hello self-signed certificate based on hello operating system on which you run hello command, and export hello PFX format certificate with private key.</span></span>

* <span data-ttu-id="0c034-117">**För Windows 10 eller Windows Server 2016**, kör hello inbyggda **ny SelfSignedCertificate** PowerShell-cmdlet enligt följande:</span><span class="sxs-lookup"><span data-stu-id="0c034-117">**For Windows 10 or Windows Server 2016**, run hello built-in **New-SelfSignedCertificate** PowerShell cmdlet as follows:</span></span>

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* <span data-ttu-id="0c034-118">**För operativsystem tidigare än Windows 10 eller Windows Server 2016**, hämta hello [självsignerat certifikat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) från hello Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="0c034-118">**For operating systems earlier than Windows 10 or Windows Server 2016**, download hello [self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from hello Microsoft Script Center.</span></span> <span data-ttu-id="0c034-119">Extrahera innehållet och kör följande kommandon i PowerShell-Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="0c034-119">Extract its contents and run hello following commands at a PowerShell prompt:</span></span>

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-tooan-azure-key-vault"></a><span data-ttu-id="0c034-120">Ladda upp certifikatet tooan Azure key vault</span><span class="sxs-lookup"><span data-stu-id="0c034-120">Upload certificate tooan Azure key vault</span></span>

<span data-ttu-id="0c034-121">Innan du distribuerar hello HPC-kluster, överför hello certifikat tooan [Azure key vault](../../key-vault/index.md) som en hemlig och registrera hello följande information för användning under distributionen av hello: **valvnamnet**,  **Valvet resursgruppen**, **Certifikatwebbadress**, och **certifikattumavtrycket**.</span><span class="sxs-lookup"><span data-stu-id="0c034-121">Before deploying hello HPC cluster, upload hello certificate tooan [Azure key vault](../../key-vault/index.md) as a secret, and record hello following information for use during hello deployment: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

<span data-ttu-id="0c034-122">Ett exempel PowerShell-skriptet tooupload hello certifikat följer.</span><span class="sxs-lookup"><span data-stu-id="0c034-122">A sample PowerShell script tooupload hello certificate follows.</span></span> <span data-ttu-id="0c034-123">Mer information om hur du överför ett certifikat tooan Azure key vault finns [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0c034-123">For more information about uploading a certificate tooan Azure key vault, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md).</span></span>

```powershell
#Give hello following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate hello pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode hello JSON object
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
#Create an Azure key vault and upload hello certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"hello following Information will be used in hello deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a><span data-ttu-id="0c034-124">Topologier som stöds</span><span class="sxs-lookup"><span data-stu-id="0c034-124">Supported topologies</span></span>

<span data-ttu-id="0c034-125">Välj något av hello [Azure Resource Manager-mallar](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 klustret.</span><span class="sxs-lookup"><span data-stu-id="0c034-125">Choose one of hello [Azure Resource Manager templates](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 cluster.</span></span> <span data-ttu-id="0c034-126">Följande är med tre topologier som stöds för klustret.</span><span class="sxs-lookup"><span data-stu-id="0c034-126">Following are high-level architectures of three supported cluster topologies.</span></span> <span data-ttu-id="0c034-127">Hög tillgänglighet topologier innehåller flera head klusternoder.</span><span class="sxs-lookup"><span data-stu-id="0c034-127">High-availability topologies include multiple cluster head nodes.</span></span>

1. <span data-ttu-id="0c034-128">Kluster med hög tillgänglighet med Active Directory-domän</span><span class="sxs-lookup"><span data-stu-id="0c034-128">High-availability cluster with Active Directory domain</span></span>

    ![Hög tillgänglighet till klustret i AD-domänen](./media/hpcpack-2016-cluster/haad.png)



2. <span data-ttu-id="0c034-130">Kluster med hög tillgänglighet utan Active Directory-domän</span><span class="sxs-lookup"><span data-stu-id="0c034-130">High-availability cluster without Active Directory domain</span></span>

    ![Kluster med hög tillgänglighet utan AD-domänen](./media/hpcpack-2016-cluster/hanoad.png)

3. <span data-ttu-id="0c034-132">Kluster med en enda huvudnod</span><span class="sxs-lookup"><span data-stu-id="0c034-132">Cluster with a single head node</span></span>

   ![Kluster med enskild huvudnod](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a><span data-ttu-id="0c034-134">Distribuera ett kluster</span><span class="sxs-lookup"><span data-stu-id="0c034-134">Deploy a cluster</span></span>

<span data-ttu-id="0c034-135">toocreate hello kluster, Välj en mall och klicka på **distribuera tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="0c034-135">toocreate hello cluster, choose a template and click **Deploy tooAzure**.</span></span> <span data-ttu-id="0c034-136">I hello [Azure-portalen](https://portal.azure.com), ange parametrar för hello mall enligt beskrivningen i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="0c034-136">In hello [Azure portal](https://portal.azure.com), specify parameters for hello template as described in hello following steps.</span></span> <span data-ttu-id="0c034-137">Varje mall skapas alla Azure-resurser som krävs för hello HPC-klusterinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="0c034-137">Each template creates all Azure resources required for hello HPC cluster infrastructure.</span></span> <span data-ttu-id="0c034-138">Resurserna innehåller ett virtuellt Azure-nätverk, offentliga IP-adress, belastningsutjämnare (endast för ett kluster med hög tillgänglighet), nätverksgränssnitt, tillgänglighetsuppsättningar, lagringskonton och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0c034-138">Resources include an Azure virtual network, public IP address, load balancer (only for a high-availability cluster), network interfaces, availability sets, storage accounts, and virtual machines.</span></span>

### <a name="step-1-select-hello-subscription-location-and-resource-group"></a><span data-ttu-id="0c034-139">Steg 1: Välj hello prenumeration, plats och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0c034-139">Step 1: Select hello subscription, location, and resource group</span></span>

<span data-ttu-id="0c034-140">Hej **prenumeration** och hello **plats** måste vara samma som du angav när du har överfört PFX-certifikat (se krav).</span><span class="sxs-lookup"><span data-stu-id="0c034-140">hello **Subscription** and hello **Location** must be same that you specified when you uploaded your PFX certificate (see Prerequisites).</span></span> <span data-ttu-id="0c034-141">Vi rekommenderar att du skapar en **resursgruppen** för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="0c034-141">We recommend that you create a **Resource group** for hello deployment.</span></span>

### <a name="step-2-specify-hello-parameter-settings"></a><span data-ttu-id="0c034-142">Steg 2: Ange hello parameterinställningarna</span><span class="sxs-lookup"><span data-stu-id="0c034-142">Step 2: Specify hello parameter settings</span></span>

<span data-ttu-id="0c034-143">Ange eller ändra värden för hello mallparametrar.</span><span class="sxs-lookup"><span data-stu-id="0c034-143">Enter or modify values for hello template parameters.</span></span> <span data-ttu-id="0c034-144">Klicka på hello ikonen nästa tooeach parameter för hjälpinformation.</span><span class="sxs-lookup"><span data-stu-id="0c034-144">Click hello icon next tooeach parameter for help information.</span></span> <span data-ttu-id="0c034-145">Se även hello vägledning för [tillgängliga storlekar på VM](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="0c034-145">Also see hello guidance for [available VM sizes](sizes.md).</span></span>

<span data-ttu-id="0c034-146">Ange hello-värden som du antecknade i hello förutsättningar för hello följande parametrar: **valvnamnet**, **valvet resursgruppen**, **Certifikatwebbadress**, och **Certifikattumavtrycket**.</span><span class="sxs-lookup"><span data-stu-id="0c034-146">Specify hello values you recorded in hello Prerequisites for hello following parameters: **Vault name**, **Vault resource group**, **Certificate URL**, and **Certificate thumbprint**.</span></span>

### <a name="step-3-review-legal-terms-and-create"></a><span data-ttu-id="0c034-147">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="0c034-147">Step 3.</span></span> <span data-ttu-id="0c034-148">Granska juridiska villkor och skapa</span><span class="sxs-lookup"><span data-stu-id="0c034-148">Review legal terms and create</span></span>
<span data-ttu-id="0c034-149">Klicka på **Granska juridiska villkor** tooreview hello villkoren.</span><span class="sxs-lookup"><span data-stu-id="0c034-149">Click **Review legal terms** tooreview hello terms.</span></span> <span data-ttu-id="0c034-150">Om du godkänner villkoren klickar du på **inköp**, och klicka sedan på **skapa** toostart hello distribution.</span><span class="sxs-lookup"><span data-stu-id="0c034-150">If you agree, click **Purchase**, and then click **Create** toostart hello deployment.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="0c034-151">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="0c034-151">Connect toohello cluster</span></span>
1. <span data-ttu-id="0c034-152">När hello HPC Pack klustret distribueras går toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0c034-152">After hello HPC Pack cluster is deployed, go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="0c034-153">Klicka på **resursgrupper**, och hitta hello resursgruppen i vilka hello klustret har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="0c034-153">Click **Resource groups**, and find hello resource group in which hello cluster was deployed.</span></span> <span data-ttu-id="0c034-154">Du kan hitta hello huvudnod virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0c034-154">You can find hello head node virtual machines.</span></span>

    ![Klustrets huvudnoder hello-portalen](./media/hpcpack-2016-cluster/clusterhns.png)

2. <span data-ttu-id="0c034-156">Klicka på en huvudnod (i ett kluster med hög tillgänglighet, klicka på någon av hello huvudnoderna).</span><span class="sxs-lookup"><span data-stu-id="0c034-156">Click one head node (in a high-availability cluster, click any of hello head nodes).</span></span> <span data-ttu-id="0c034-157">I **Essentials**, hittar du hello offentlig IP-adress eller fullständigt DNS-namn i hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="0c034-157">In **Essentials**, you can find hello public IP address or full DNS name of hello cluster.</span></span>

    ![Anslutningsinställningar för kluster](./media/hpcpack-2016-cluster/clusterconnect.png)

3. <span data-ttu-id="0c034-159">Klicka på **Anslut** toolog på tooany av hello huvudnoderna med hjälp av fjärrskrivbord med den angivna administratörsanvändarnamn.</span><span class="sxs-lookup"><span data-stu-id="0c034-159">Click **Connect** toolog on tooany of hello head nodes using Remote Desktop with your specified administrator user name.</span></span> <span data-ttu-id="0c034-160">Om hello-klustret som du har distribuerat finns i en Active Directory-domän, hello användarnamnet är hello formatet <privateDomainName> \<adminUsername > (till exempel hpc.local\hpcadmin).</span><span class="sxs-lookup"><span data-stu-id="0c034-160">If hello cluster you deployed is in an Active Directory Domain, hello user name is of hello form <privateDomainName>\<adminUsername> (for example, hpc.local\hpcadmin).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c034-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c034-161">Next steps</span></span>
* <span data-ttu-id="0c034-162">Skicka jobb tooyour klustret.</span><span class="sxs-lookup"><span data-stu-id="0c034-162">Submit jobs tooyour cluster.</span></span> <span data-ttu-id="0c034-163">Se [skicka jobb tooHPC ett HPC Pack kluster i Azure](hpcpack-cluster-submit-jobs.md) och [hantera ett HPC Pack 2016-kluster i Azure med hjälp av Azure Active Directory](hpcpack-cluster-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="0c034-163">See [Submit jobs tooHPC an HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md) and [Manage an HPC Pack 2016 cluster in Azure using Azure Active Directory](hpcpack-cluster-active-directory.md).</span></span>

