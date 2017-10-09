---
title: aaaCreate ett Service Fabric-klustret i Azure | Microsoft Docs
description: "Lär dig hur toocreate Windows- eller Linux Service Fabric-klustret i Azure med hjälp av PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi
ms.openlocfilehash: e697e2a2e99f67cb02422efdb368c664c9fd5a97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a><span data-ttu-id="052e6-103">Skapa en säker kluster i Azure med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="052e6-103">Create a secure cluster on Azure using PowerShell</span></span>
<span data-ttu-id="052e6-104">Den här kursen visar hur toocreate ett Service Fabric-kluster (Windows eller Linux) körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="052e6-104">This tutorial shows you how toocreate a Service Fabric cluster (Windows or Linux) running in Azure.</span></span> <span data-ttu-id="052e6-105">När du är klar har du ett kluster som kör i hello moln som du kan distribuera program till.</span><span class="sxs-lookup"><span data-stu-id="052e6-105">When you're finished, you have a cluster running in hello cloud that you can deploy applications to.</span></span>

<span data-ttu-id="052e6-106">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="052e6-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="052e6-107">Skapa en säker Service Fabric-kluster i Azure med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="052e6-107">Create a secure Service Fabric cluster in Azure using PowerShell</span></span>
> * <span data-ttu-id="052e6-108">Säker hello kluster med ett X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="052e6-108">Secure hello cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="052e6-109">Ansluta toohello kluster med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="052e6-109">Connect toohello cluster using PowerShell</span></span>
> * <span data-ttu-id="052e6-110">Ta bort ett kluster</span><span class="sxs-lookup"><span data-stu-id="052e6-110">Remove a cluster</span></span>

## <a name="prerequisites"></a><span data-ttu-id="052e6-111">Krav</span><span class="sxs-lookup"><span data-stu-id="052e6-111">Prerequisites</span></span>
<span data-ttu-id="052e6-112">Innan du börjar den här kursen:</span><span class="sxs-lookup"><span data-stu-id="052e6-112">Before you begin this tutorial:</span></span>
- <span data-ttu-id="052e6-113">Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span><span class="sxs-lookup"><span data-stu-id="052e6-113">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)</span></span>
- <span data-ttu-id="052e6-114">Installera hello [Fabric-SDK och PowerShell-modul](service-fabric-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="052e6-114">Install hello [Service Fabric SDK and PowerShell module](service-fabric-get-started.md)</span></span>
- <span data-ttu-id="052e6-115">Installera hello [Azure Powershell Modulversion 4.1 eller högre](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span><span class="sxs-lookup"><span data-stu-id="052e6-115">Install hello [Azure Powershell module version 4.1 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)</span></span>

<span data-ttu-id="052e6-116">hello följer proceduren skapar en nod (virtuell dator) förhandsgranskning Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="052e6-116">hello following procedure creates a single-node (single virtual machine) preview Service Fabric cluster.</span></span> <span data-ttu-id="052e6-117">hello kluster skyddas av ett självsignerat certifikat som hämtar skapas tillsammans med hello klustret och sedan placeras i en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="052e6-117">hello cluster is secured by a self-signed certificate that gets created along with hello cluster and then placed in a key vault.</span></span> <span data-ttu-id="052e6-118">Enkelnods-kluster kan inte skalas utöver en virtuell dator och förhandsgranska kluster får inte vara uppgraderade toonewer versioner.</span><span class="sxs-lookup"><span data-stu-id="052e6-118">Single-node clusters cannot be scaled beyond one virtual machine and preview clusters cannot be upgraded toonewer versions.</span></span>

<span data-ttu-id="052e6-119">toocalculate kostnaden genom att köra ett Service Fabric-kluster i Azure används hello [Priskalkylatorn för Azure](https://azure.microsoft.com/pricing/calculator/).</span><span class="sxs-lookup"><span data-stu-id="052e6-119">toocalculate cost incurred by running a Service Fabric cluster in Azure use hello [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).</span></span>
<span data-ttu-id="052e6-120">Mer information om hur du skapar Service Fabric-kluster finns [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="052e6-120">For more information on creating Service Fabric clusters, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="create-hello-cluster-using-azure-powershell"></a><span data-ttu-id="052e6-121">Skapa hello-kluster med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="052e6-121">Create hello cluster using Azure PowerShell</span></span>
1. <span data-ttu-id="052e6-122">Hämta en lokal kopia av hello Azure Resource Manager-mall och hello parameterfilen från hello [Azure Resource Manager-mall för Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="052e6-122">Download a local copy of hello Azure Resource Manager template and hello parameter file from hello [Azure Resource Manager template for Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) GitHub repository.</span></span>  <span data-ttu-id="052e6-123">*azuredeploy.JSON* är hello Azure Resource Manager-mallen som definierar ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="052e6-123">*azuredeploy.json* is hello Azure Resource Manager template that defines a Service Fabric cluster.</span></span> <span data-ttu-id="052e6-124">*azuredeploy.parameters.JSON* är en fil med parametrar för dig toocustomize hello klusterdistribution.</span><span class="sxs-lookup"><span data-stu-id="052e6-124">*azuredeploy.parameters.json* is a parameters file for you toocustomize hello cluster deployment.</span></span>

2. <span data-ttu-id="052e6-125">Anpassa följande parametrar i hello hello *azuredeploy.parameters.json* parameterfilen:</span><span class="sxs-lookup"><span data-stu-id="052e6-125">Customize hello following parameters in hello *azuredeploy.parameters.json* parameters file:</span></span>

   | <span data-ttu-id="052e6-126">Parameter</span><span class="sxs-lookup"><span data-stu-id="052e6-126">Parameter</span></span>       | <span data-ttu-id="052e6-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="052e6-127">Description</span></span> | <span data-ttu-id="052e6-128">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="052e6-128">Suggested Value</span></span> |
   | --------------- | ----------- | --------------- |
   | <span data-ttu-id="052e6-129">clusterLocation</span><span class="sxs-lookup"><span data-stu-id="052e6-129">clusterLocation</span></span> | <span data-ttu-id="052e6-130">hello Azure-region toowhich toodeploy hello kluster.</span><span class="sxs-lookup"><span data-stu-id="052e6-130">hello Azure region toowhich toodeploy hello cluster.</span></span> | <span data-ttu-id="052e6-131">*till exempel westeurope eastasia, eastus*</span><span class="sxs-lookup"><span data-stu-id="052e6-131">*for example, westeurope, eastasia, eastus*</span></span> |
   | <span data-ttu-id="052e6-132">Klusternamn</span><span class="sxs-lookup"><span data-stu-id="052e6-132">clusterName</span></span>     | <span data-ttu-id="052e6-133">Namn på hello klustret som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="052e6-133">Name of hello cluster you want toocreate.</span></span> | <span data-ttu-id="052e6-134">*till exempel bobs-sfpreviewcluster*</span><span class="sxs-lookup"><span data-stu-id="052e6-134">*for example, bobs-sfpreviewcluster*</span></span> |
   | <span data-ttu-id="052e6-135">adminUserName</span><span class="sxs-lookup"><span data-stu-id="052e6-135">adminUserName</span></span>   | <span data-ttu-id="052e6-136">hello lokalt administratörskonto på hello klustra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="052e6-136">hello local admin account on hello cluster virtual machines.</span></span> | <span data-ttu-id="052e6-137">*Alla Windows Server-användarnamn*</span><span class="sxs-lookup"><span data-stu-id="052e6-137">*Any valid Windows Server username*</span></span> |
   | <span data-ttu-id="052e6-138">adminPassword</span><span class="sxs-lookup"><span data-stu-id="052e6-138">adminPassword</span></span>   | <span data-ttu-id="052e6-139">Lösenordet för hello lokalt administratörskonto på hello klustra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="052e6-139">Password of hello local admin account on hello cluster virtual machines.</span></span> | <span data-ttu-id="052e6-140">*Alla Windows Server-lösenord*</span><span class="sxs-lookup"><span data-stu-id="052e6-140">*Any valid Windows Server password*</span></span> |
   | <span data-ttu-id="052e6-141">clusterCodeVersion</span><span class="sxs-lookup"><span data-stu-id="052e6-141">clusterCodeVersion</span></span> | <span data-ttu-id="052e6-142">hello Service Fabric version toorun.</span><span class="sxs-lookup"><span data-stu-id="052e6-142">hello Service Fabric version toorun.</span></span> <span data-ttu-id="052e6-143">(255.255.X.255 är förhandsversioner).</span><span class="sxs-lookup"><span data-stu-id="052e6-143">(255.255.X.255 are preview versions).</span></span> | <span data-ttu-id="052e6-144">**255.255.5718.255**</span><span class="sxs-lookup"><span data-stu-id="052e6-144">**255.255.5718.255**</span></span> |
   | <span data-ttu-id="052e6-145">vmInstanceCount</span><span class="sxs-lookup"><span data-stu-id="052e6-145">vmInstanceCount</span></span> | <span data-ttu-id="052e6-146">hello antalet virtuella datorer i klustret (kan vara 1 eller 3-99).</span><span class="sxs-lookup"><span data-stu-id="052e6-146">hello number of virtual machines in your cluster (can be 1 or 3-99).</span></span> | <span data-ttu-id="052e6-147">**1**</span><span class="sxs-lookup"><span data-stu-id="052e6-147">**1**</span></span> | <span data-ttu-id="052e6-148">*Ange endast en virtuell dator för ett kluster för förhandsgranskning*</span><span class="sxs-lookup"><span data-stu-id="052e6-148">*For a preview cluster specify only one virtual machine*</span></span> |

3. <span data-ttu-id="052e6-149">Öppna ett PowerShell-konsolen, inloggning tooAzure och välj hello-prenumeration som du vill toodeploy hello klustret i:</span><span class="sxs-lookup"><span data-stu-id="052e6-149">Open a PowerShell console, login tooAzure, and select hello subscription you want toodeploy hello cluster in:</span></span>

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. <span data-ttu-id="052e6-150">Skapa och kryptera ett lösenord för hello certifikat toobe används av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="052e6-150">Create and encrypt a password for hello certificate toobe used by Service Fabric.</span></span>

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. <span data-ttu-id="052e6-151">Skapa hello klustret och dess certifikat genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="052e6-151">Create hello cluster and its certificate by running hello following command:</span></span>

   ```powershell
      New-AzureRmServiceFabricCluster
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   ><span data-ttu-id="052e6-152">Hej `-CertificateSubjectName` parametern ska justeras med hello klusternamn parameter har angetts i hello parameterfilen, samt som hello domän knutna toohello Azure-region du har valt, exempelvis: `clustername.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="052e6-152">hello `-CertificateSubjectName` parameter should align with hello clusterName parameter specified in hello parameters file, as well as hello domain tied toohello Azure region you chose, such as: `clustername.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="052e6-153">När hello konfigurationen är klar visar information om hello klustret skapas i Azure.</span><span class="sxs-lookup"><span data-stu-id="052e6-153">Once hello configuration finishes, it outputs information about hello cluster created in Azure.</span></span> <span data-ttu-id="052e6-154">Hello klustret certifikat toohello CertificateOutputFolder - katalogen på hello sökvägen för den här parametern kopieras också.</span><span class="sxs-lookup"><span data-stu-id="052e6-154">It also copies hello cluster certificate toohello -CertificateOutputFolder directory on hello path you specified for this parameter.</span></span> <span data-ttu-id="052e6-155">Du behöver det här certifikatet tooaccess Service Fabric Explorer och visa hello hälsotillståndet för klustret.</span><span class="sxs-lookup"><span data-stu-id="052e6-155">You need this certificate tooaccess Service Fabric Explorer and view hello health of your cluster.</span></span>

<span data-ttu-id="052e6-156">Anteckna hello URL för klustret, vilket kan vara liknande toohello följande URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span><span class="sxs-lookup"><span data-stu-id="052e6-156">Take note of hello URL for your cluster, which may be similar toohello following URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*</span></span>

## <a name="modify-hello-certificate--access-service-fabric-explorer"></a><span data-ttu-id="052e6-157">Ändra certifikat för hello & åtkomst till Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="052e6-157">Modify hello certificate & access Service Fabric Explorer</span></span> ##

1. <span data-ttu-id="052e6-158">Dubbelklicka på hello certifikat tooopen hello guiden Importera certifikat.</span><span class="sxs-lookup"><span data-stu-id="052e6-158">Double-click hello certificate tooopen hello Certificate Import Wizard.</span></span>

2. <span data-ttu-id="052e6-159">Använd standardinställningar, men se till att toocheck hello **Markera den här nyckeln kan exporteras.**</span><span class="sxs-lookup"><span data-stu-id="052e6-159">Use default settings, but make sure toocheck hello **Mark this key as exportable.**</span></span> <span data-ttu-id="052e6-160">kryssrutan i hello **skydd av privat nyckel** steg.</span><span class="sxs-lookup"><span data-stu-id="052e6-160">check box, in hello **private key protection** step.</span></span> <span data-ttu-id="052e6-161">Visual Studio måste tooexport hello certifikat när du konfigurerar Azure Container registret tooService Fabric-kluster autentisering senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="052e6-161">Visual Studio needs tooexport hello certificate when configuring Azure Container Registry tooService Fabric Cluster authentication later in this tutorial.</span></span>

3. <span data-ttu-id="052e6-162">Nu kan du öppna Service Fabric Explorer i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="052e6-162">You can now open Service Fabric Explorer in a browser.</span></span> <span data-ttu-id="052e6-163">toodo gå så toohello **ManagementEndpoint** URL för klustret med hjälp av en webbläsare och välj hello-certifikat som har sparats på din dator.</span><span class="sxs-lookup"><span data-stu-id="052e6-163">toodo so, navigate toohello **ManagementEndpoint** URL for your cluster using a web browser, and select hello certificate that was saved on your machine.</span></span>

>[!NOTE]
><span data-ttu-id="052e6-164">När du öppnar Service Fabric Explorer kan se du ett certifikat som du använder ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="052e6-164">When opening Service Fabric Explorer, you see a certificate error, as you are using a self-signed certificate.</span></span> <span data-ttu-id="052e6-165">I gränsen, har du tooclick *information* och sedan hello *finns på webbsidan toohello* länk.</span><span class="sxs-lookup"><span data-stu-id="052e6-165">In Edge, you have tooclick *Details* and then hello *Go on toohello webpage* link.</span></span> <span data-ttu-id="052e6-166">I Chrome, har du tooclick *Avancerat* och sedan hello *fortsätta* länk.</span><span class="sxs-lookup"><span data-stu-id="052e6-166">In Chrome, you have tooclick *Advanced* and then hello *proceed* link.</span></span>

>[!NOTE]
><span data-ttu-id="052e6-167">Om hello klustret misslyckas, kan du alltid köra hello-kommandot, vilket uppdaterar hello resurser som redan har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="052e6-167">If hello cluster creation fails, you can always rerun hello command, which updates hello resources already deployed.</span></span> <span data-ttu-id="052e6-168">Om ett certifikat har skapats som en del av distributionen hello misslyckades, skapas en ny.</span><span class="sxs-lookup"><span data-stu-id="052e6-168">If a certificate was created as part of hello failed deployment, a new one is generated.</span></span> <span data-ttu-id="052e6-169">tootroubleshoot klustret skapas finns [skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="052e6-169">tootroubleshoot cluster creation, see [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span>

## <a name="connect-toohello-secure-cluster"></a><span data-ttu-id="052e6-170">Ansluta toohello säker kluster</span><span class="sxs-lookup"><span data-stu-id="052e6-170">Connect toohello secure cluster</span></span>
<span data-ttu-id="052e6-171">Ansluta toohello kluster med hjälp av hello Service Fabric PowerShell module installerad med hello Service Fabric-SDK.</span><span class="sxs-lookup"><span data-stu-id="052e6-171">Connect toohello cluster using hello Service Fabric PowerShell module installed with hello Service Fabric SDK.</span></span>  <span data-ttu-id="052e6-172">Installera först hello certifikat till hello Personal (min) arkivet för hello aktuella användare på datorn.</span><span class="sxs-lookup"><span data-stu-id="052e6-172">First, install hello certificate into hello Personal (My) store of hello current user on your computer.</span></span>  <span data-ttu-id="052e6-173">Kör följande PowerShell-kommando hello:</span><span class="sxs-lookup"><span data-stu-id="052e6-173">Run hello following PowerShell command:</span></span>

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

<span data-ttu-id="052e6-174">Du är nu redo tooconnect tooyour säker klustret.</span><span class="sxs-lookup"><span data-stu-id="052e6-174">You are now ready tooconnect tooyour secure cluster.</span></span>

<span data-ttu-id="052e6-175">Hej **Service Fabric** PowerShell-modulen ger många cmdlet: ar för hantering av Service Fabric-kluster, program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="052e6-175">hello **Service Fabric** PowerShell module provides many cmdlets for managing Service Fabric clusters, applications, and services.</span></span>  <span data-ttu-id="052e6-176">Använd hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet tooconnect toohello säker klustret.</span><span class="sxs-lookup"><span data-stu-id="052e6-176">Use hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) cmdlet tooconnect toohello secure cluster.</span></span> <span data-ttu-id="052e6-177">hello tumavtryck för certifikat och information om anslutningen som återfinns i hello utdata från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="052e6-177">hello certificate thumbprint and connection endpoint details are found in hello output from a previous step.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="052e6-178">Kontrollera att du är ansluten och hello klustret är felfri med hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="052e6-178">Check that you are connected and hello cluster is healthy using hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) cmdlet.</span></span>

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a><span data-ttu-id="052e6-179">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="052e6-179">Clean up resources</span></span>

<span data-ttu-id="052e6-180">Ett kluster består av andra Azure-resurser förutom toohello klusterresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="052e6-180">A cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="052e6-181">hello enklaste sättet toodelete hello kluster och alla hello-resurser som den förbrukar är toodelete hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="052e6-181">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span>

<span data-ttu-id="052e6-182">Logga in tooAzure och välj hello prenumerations-ID som du vill använda tooremove hello klustret.</span><span class="sxs-lookup"><span data-stu-id="052e6-182">Log in tooAzure and select hello subscription ID with which you want tooremove hello cluster.</span></span>  <span data-ttu-id="052e6-183">Du hittar prenumerations-ID genom att logga in toohello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="052e6-183">You can find your subscription ID by logging in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="052e6-184">Ta bort hello resursgruppen och alla hello klusterresurser med hello [cmdlet Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="052e6-184">Delete hello resource group and all hello cluster resources using hello [Remove-AzureRMResourceGroup cmdlet](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a><span data-ttu-id="052e6-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="052e6-185">Next steps</span></span>
<span data-ttu-id="052e6-186">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="052e6-186">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="052e6-187">Skapa ett Service Fabric-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="052e6-187">Create a Service Fabric cluster in Azure</span></span>
> * <span data-ttu-id="052e6-188">Säker hello kluster med ett X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="052e6-188">Secure hello cluster with an X.509 certificate</span></span>
> * <span data-ttu-id="052e6-189">Ansluta toohello kluster med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="052e6-189">Connect toohello cluster using PowerShell</span></span>
> * <span data-ttu-id="052e6-190">Ta bort ett kluster</span><span class="sxs-lookup"><span data-stu-id="052e6-190">Remove a cluster</span></span>

<span data-ttu-id="052e6-191">Därefter i förväg toohello följa kursen toolearn hur toodeploy ett befintligt program.</span><span class="sxs-lookup"><span data-stu-id="052e6-191">Next, advance toohello following tutorial toolearn how toodeploy an existing application.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="052e6-192">Distribuera ett befintligt .NET-program med Docker Compose</span><span class="sxs-lookup"><span data-stu-id="052e6-192">Deploy an existing .NET application with Docker Compose</span></span>](service-fabric-host-app-in-a-container.md)
