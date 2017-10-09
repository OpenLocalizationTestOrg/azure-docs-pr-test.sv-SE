---
title: aaaManage certifikat i ett Azure Service Fabric-kluster | Microsoft Docs
description: "Beskriver hur tooadd nya certifikat, förnyelsecertifikat och ta bort certifikat tooor från ett Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: chackdan
ms.openlocfilehash: 8e57bd95dbb800ecc04cf6988047e3abdc2fe56a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a><span data-ttu-id="88ede-103">Lägg till eller ta bort certifikat för Service Fabric-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="88ede-103">Add or remove certificates for a Service Fabric cluster in Azure</span></span>
<span data-ttu-id="88ede-104">Vi rekommenderar att du bekanta dig med hur Service Fabric använder X.509-certifikat och känna till hello [kluster säkerhetsscenarier](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="88ede-104">It is recommended that you familiarize yourself with how Service Fabric uses X.509 certificates and be familiar with hello [Cluster security scenarios](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="88ede-105">Du måste förstå vad ett certifikat för klustret och som används för, innan du fortsätter ytterligare.</span><span class="sxs-lookup"><span data-stu-id="88ede-105">You must understand what a cluster certificate is and what is used for, before you proceed further.</span></span>

<span data-ttu-id="88ede-106">Service fabric kan du ange två kluster certifikat, en primär och en sekundär när du konfigurerar certifikat säkerhet när klustret skapas i tillägg tooclient certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-106">Service fabric lets you specify two cluster certificates, a primary and a secondary, when you configure certificate security during cluster creation, in addition tooclient certificates.</span></span> <span data-ttu-id="88ede-107">Se för[skapar ett azure-kluster via portalen](service-fabric-cluster-creation-via-portal.md) eller [skapar ett azure-kluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) för information om att ställa in dem på Skapa tid.</span><span class="sxs-lookup"><span data-stu-id="88ede-107">Refer too[creating an azure cluster via portal](service-fabric-cluster-creation-via-portal.md) or [creating an azure cluster via Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) for details on setting them up at create time.</span></span> <span data-ttu-id="88ede-108">Om du anger bara ett certifikat för kluster på Skapa tid, som används som primär hello-certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-108">If you specify only one cluster certificate at create time, then that is used as hello primary certificate.</span></span> <span data-ttu-id="88ede-109">Du kan lägga till ett nytt certifikat som en sekundär när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="88ede-109">After cluster creation, you can add a new certificate as a secondary.</span></span>

> [!NOTE]
> <span data-ttu-id="88ede-110">För en säker kluster behöver du alltid minst en giltig (inte återkallat och inte har upphört att gälla) kluster certifikat (primära eller sekundära) distribueras (om inte, hello klustret slutar fungera).</span><span class="sxs-lookup"><span data-stu-id="88ede-110">For a secure cluster, you will always need at least one valid (not revoked and not expired) cluster certificate (primary or secondary) deployed (if not, hello cluster stops functioning).</span></span> <span data-ttu-id="88ede-111">90 dagar innan alla giltiga certifikat når förfallodatum, genereras hello en varning-spårning och en varningshändelse hälsa på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="88ede-111">90 days before all valid certificates reach expiration, hello system generates a warning trace and also a warning health event on hello node.</span></span> <span data-ttu-id="88ede-112">Det finns för närvarande ingen e-post eller andra notification service fabric skickar ut om det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="88ede-112">There is currently no email or any other notification that service fabric sends out on this topic.</span></span> 
> 
> 

## <a name="add-a-secondary-cluster-certificate-using-hello-portal"></a><span data-ttu-id="88ede-113">Lägga till ett sekundärt kluster-certifikat med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="88ede-113">Add a secondary cluster certificate using hello portal</span></span>

<span data-ttu-id="88ede-114">Sekundära kluster certifikat kan inte läggas till hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="88ede-114">Secondary cluster certificate cannot be added through hello Azure portal.</span></span> <span data-ttu-id="88ede-115">Du har toouse Azure powershell för den.</span><span class="sxs-lookup"><span data-stu-id="88ede-115">You have toouse Azure powershell for that.</span></span> <span data-ttu-id="88ede-116">hello process beskrivs senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="88ede-116">hello process is outlined later in this document.</span></span>

## <a name="swap-hello-cluster-certificates-using-hello-portal"></a><span data-ttu-id="88ede-117">Växla hello klustret certifikat med hjälp av hello-portalen</span><span class="sxs-lookup"><span data-stu-id="88ede-117">Swap hello cluster certificates using hello portal</span></span>

<span data-ttu-id="88ede-118">När du har distribuerat ett sekundärt kluster-certifikat har om du vill tooswap hello primära och sekundära, sedan navigera toohello säkerhet bladet och välj alternativet för hello 'Växlingen med primära' från hello kontexten menyn tooswap hello sekundära certifikat med hello primära certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-118">After you have successfully deployed a secondary cluster certificate, if you want tooswap hello primary and secondary, then navigate toohello Security blade, and select hello 'Swap with primary' option from hello context menu tooswap hello secondary cert with hello primary cert.</span></span>

![Växla certifikat][Delete_Swap_Cert]

## <a name="remove-a-cluster-certificate-using-hello-portal"></a><span data-ttu-id="88ede-120">Ta bort ett certifikat för kluster med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="88ede-120">Remove a cluster certificate using hello portal</span></span>

<span data-ttu-id="88ede-121">För en säker kluster behöver alltid du minst en giltig (inte återkallat och inte har upphört att gälla) certifikat (primära eller sekundära) distribueras om inte, hello sluta fungera.</span><span class="sxs-lookup"><span data-stu-id="88ede-121">For a secure cluster, you will always need at least one valid (not revoked and not expired) certificate (primary or secondary) deployed if not, hello cluster stops functioning.</span></span>

<span data-ttu-id="88ede-122">tooremove ett sekundärt certifikat används för klustret säkerhet, analysera toohello säkerhet bladet och väljer hello 'Radera' alternativet hello snabbmenyn på hello sekundärt certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-122">tooremove a secondary certificate from being used for cluster security, Navigate toohello Security blade and select hello 'Delete' option from hello context menu on hello secondary certificate.</span></span>

<span data-ttu-id="88ede-123">Om din avsikt är tooremove hello-certifikat som har markerats primära, behöver du tooswap med hello sekundära först och tar bort hello sekundära när hello uppgraderingen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="88ede-123">If your intent is tooremove hello certificate that is marked primary, then you will need tooswap it with hello secondary first, and then delete hello secondary after hello upgrade has completed.</span></span>

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a><span data-ttu-id="88ede-124">Lägg till ett sekundärt certifikat med hjälp av hanteraren för filserverresurser Powershell</span><span class="sxs-lookup"><span data-stu-id="88ede-124">Add a secondary certificate using Resource Manager Powershell</span></span>

<span data-ttu-id="88ede-125">Dessa instruktioner förutsätter att du känner till hur Resource Manager fungerar och har distribuerat minst en Service Fabric-kluster med hjälp av en Resource Manager-mall och har hello mall som du använde tooset in hello kluster praktiska.</span><span class="sxs-lookup"><span data-stu-id="88ede-125">These steps assume that you are familiar with how Resource Manager works and have deployed atleast one Service Fabric cluster using a Resource Manager template, and have hello template that you used tooset up hello cluster handy.</span></span> <span data-ttu-id="88ede-126">Det förutsätts även att du är nöjd med JSON.</span><span class="sxs-lookup"><span data-stu-id="88ede-126">It is also assumed that you are comfortable using JSON.</span></span>

> [!NOTE]
> <span data-ttu-id="88ede-127">Om du letar efter en exempelmall och parametrar som du kan använda toofollow längs eller som en startpunkt sedan ladda ned det från den här [git-lagringsplatsen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="88ede-127">If you are looking for a sample template and parameters that you can use toofollow along or as a starting point, then download it from this [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span> 
> 
> 

### <a name="edit-your-resource-manager-template"></a><span data-ttu-id="88ede-128">Redigera Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="88ede-128">Edit your Resource Manager template</span></span>

<span data-ttu-id="88ede-129">För att underlätta av följande längs innehåller exempel 5-VM-1-nodetypes får-Secure_Step2.JSON alla hello redigeringar vi kommer att göra.</span><span class="sxs-lookup"><span data-stu-id="88ede-129">For ease of following along, sample 5-VM-1-NodeTypes-Secure_Step2.JSON contains all hello edits we will be making.</span></span> <span data-ttu-id="88ede-130">hello-exempel finns på [git-lagringsplatsen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span><span class="sxs-lookup"><span data-stu-id="88ede-130">hello sample is available at [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).</span></span>

<span data-ttu-id="88ede-131">**Se till att toofollow alla hello steg**</span><span class="sxs-lookup"><span data-stu-id="88ede-131">**Make sure toofollow all hello steps**</span></span>

<span data-ttu-id="88ede-132">**Steg 1:** öppna hello Resource Manager-mall som du använde toodeploy du ingå i klustret.</span><span class="sxs-lookup"><span data-stu-id="88ede-132">**Step 1:** Open up hello Resource Manager template you used toodeploy you Cluster.</span></span> <span data-ttu-id="88ede-133">(Om du har hämtat hello exempel från hello ovan lagringsplatsen, använda 5-VM-1-nodetypes får-Secure_Step1.JSON toodeploy säker klustret och sedan öppna mallen).</span><span class="sxs-lookup"><span data-stu-id="88ede-133">(If you have downloaded hello sample from hello above repo, then Use 5-VM-1-NodeTypes-Secure_Step1.JSON toodeploy a secure cluster and then open up that template).</span></span>

<span data-ttu-id="88ede-134">**Steg 2:** Lägg till **två nya parametrar** ”secCertificateThumbprint” och ”secCertificateUrlValue” av typen ”sträng” toohello parametern avsnitt i mallen.</span><span class="sxs-lookup"><span data-stu-id="88ede-134">**Step 2:** Add **two new parameters** "secCertificateThumbprint" and "secCertificateUrlValue" of type "string" toohello parameter section of your template.</span></span> <span data-ttu-id="88ede-135">Du kan kopiera hello följande kodavsnitt och lägga till den toohello mall.</span><span class="sxs-lookup"><span data-stu-id="88ede-135">You can copy hello following code snippet and add it toohello template.</span></span> <span data-ttu-id="88ede-136">Beroende på hello källa i mallen, kan du redan dessa definierats, i så fall flytta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="88ede-136">Depending on hello source of your template, you may already have these defined, if so move toohello next step.</span></span> 
 
```JSON
   "secCertificateThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Thumbprint"
      }
    },
    "secCertificateUrlValue": {
      "type": "string",
      "metadata": {
        "description": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
      }
    },

```

<span data-ttu-id="88ede-137">**Steg 3:** göra ändringar toohello **Microsoft.ServiceFabric/clusters** resurs, leta upp hello ”Microsoft.ServiceFabric/clusters” resursdefinitionen i mallen.</span><span class="sxs-lookup"><span data-stu-id="88ede-137">**Step 3:** Make changes toohello **Microsoft.ServiceFabric/clusters** resource - Locate hello "Microsoft.ServiceFabric/clusters" resource definition in your template.</span></span> <span data-ttu-id="88ede-138">Under Egenskaper för den definitionen av du hittar ”certifikat” JSON-tagg som ska se ut ungefär hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="88ede-138">Under properties of that definition, you will find "Certificate" JSON tag, which should look something like hello following JSON snippet:</span></span>

   
```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="88ede-139">Lägg till en ny tagg ”thumbprintSecondary” och ge det värdet ”[parameters('secCertificateThumbprint')]”.</span><span class="sxs-lookup"><span data-stu-id="88ede-139">Add a new tag "thumbprintSecondary" and give it a value "[parameters('secCertificateThumbprint')]".</span></span>  

<span data-ttu-id="88ede-140">Nu hello resursdefinitionen bör se ut hello följande (beroende på din källan för hello mallen det kanske inte precis som hello kodfragmentet nedan).</span><span class="sxs-lookup"><span data-stu-id="88ede-140">So now hello resource definition should look like hello following (depending on your source of hello template, it may not be exactly like hello snippet below).</span></span> 

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('certificateThumbprint')]",
          "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 

<span data-ttu-id="88ede-141">Om du vill använda för**förnyelse hello cert**, ange hello nya certifikat som primär och flytta hello aktuella primära som sekundär.</span><span class="sxs-lookup"><span data-stu-id="88ede-141">If you want too**rollover hello cert**, then specify hello new cert as primary and moving hello current primary as secondary.</span></span> <span data-ttu-id="88ede-142">Detta resulterar i hello förnyelsen av det aktuella certifikat för primär toohello nya certifikatet i ett steg i distributionen.</span><span class="sxs-lookup"><span data-stu-id="88ede-142">This results in hello rollover of your current primary certificate toohello new certificate in one deployment step.</span></span>

```JSON
      "properties": {
        "certificate": {
          "thumbprint": "[parameters('secCertificateThumbprint')]",
          "thumbprintSecondary": "[parameters('certificateThumbprint')]",
          "x509StoreName": "[parameters('certificateStoreValue')]"
     }
``` 


<span data-ttu-id="88ede-143">**Steg 4:** göra ändringar för**alla** hello **Microsoft.Compute/virtualMachineScaleSets** resursdefinitionerna - hitta hello Microsoft.Compute/virtualMachineScaleSets resurs definition.</span><span class="sxs-lookup"><span data-stu-id="88ede-143">**Step 4:** Make changes too**all** hello **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate hello Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="88ede-144">Rulla toohello ”publisher”: ”Microsoft.Azure.ServiceFabric” under ”virtualMachineProfile”.</span><span class="sxs-lookup"><span data-stu-id="88ede-144">Scroll toohello "publisher": "Microsoft.Azure.ServiceFabric", under "virtualMachineProfile".</span></span>

<span data-ttu-id="88ede-145">Du bör se ut så här i hello service fabric publisher inställningar.</span><span class="sxs-lookup"><span data-stu-id="88ede-145">In hello service fabric publisher settings, you should see something like this.</span></span>

![Json_Pub_Setting1][Json_Pub_Setting1]

<span data-ttu-id="88ede-147">Lägg till hello nya cert poster tooit</span><span class="sxs-lookup"><span data-stu-id="88ede-147">Add hello new cert entries tooit</span></span>

```JSON
               "certificateSecondary": {
                    "thumbprint": "[parameters('secCertificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```

<span data-ttu-id="88ede-148">hello egenskaper bör nu se ut så här</span><span class="sxs-lookup"><span data-stu-id="88ede-148">hello properties should now look like this</span></span>

![Json_Pub_Setting2][Json_Pub_Setting2]

<span data-ttu-id="88ede-150">Om du vill använda för**förnyelse hello cert**, ange hello nya certifikat som primär och flytta hello aktuella primära som sekundär.</span><span class="sxs-lookup"><span data-stu-id="88ede-150">If you want too**rollover hello cert**, then specify hello new cert as primary and moving hello current primary as secondary.</span></span> <span data-ttu-id="88ede-151">Detta resulterar i hello förnyelsen av det aktuella certifikat toohello nya certifikatet i ett steg i distributionen.</span><span class="sxs-lookup"><span data-stu-id="88ede-151">This results in hello rollover of your current certificate toohello new certificate in one deployment step.</span></span> 


```JSON
               "certificate": {
                   "thumbprint": "[parameters('secCertificateThumbprint')]",
                   "x509StoreName": "[parameters('certificateStoreValue')]"
                     },
               "certificateSecondary": {
                    "thumbprint": "[parameters('certificateThumbprint')]",
                    "x509StoreName": "[parameters('certificateStoreValue')]"
                    }
                  },

```
<span data-ttu-id="88ede-152">hello egenskaper bör nu se ut så här</span><span class="sxs-lookup"><span data-stu-id="88ede-152">hello properties should now look like this</span></span>

![Json_Pub_Setting3][Json_Pub_Setting3]


<span data-ttu-id="88ede-154">**Steg 5:** göra ändringar för**alla** hello **Microsoft.Compute/virtualMachineScaleSets** resursdefinitionerna - hitta hello Microsoft.Compute/virtualMachineScaleSets resurs definition.</span><span class="sxs-lookup"><span data-stu-id="88ede-154">**Step 5:** Make Changes too**all** hello **Microsoft.Compute/virtualMachineScaleSets** resource definitions - Locate hello Microsoft.Compute/virtualMachineScaleSets resource definition.</span></span> <span data-ttu-id="88ede-155">Rulla toohello ”vaultCertificates”:, under ”OSProfile”.</span><span class="sxs-lookup"><span data-stu-id="88ede-155">Scroll toohello "vaultCertificates": , under "OSProfile".</span></span> <span data-ttu-id="88ede-156">Det bör se ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="88ede-156">it should look something like this.</span></span>


![Json_Pub_Setting4][Json_Pub_Setting4]

<span data-ttu-id="88ede-158">Lägg till hello secCertificateUrlValue tooit.</span><span class="sxs-lookup"><span data-stu-id="88ede-158">Add hello secCertificateUrlValue tooit.</span></span> <span data-ttu-id="88ede-159">Använd följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="88ede-159">use hello following snippet:</span></span>

```Json
                  {
                    "certificateStore": "[parameters('certificateStoreValue')]",
                    "certificateUrl": "[parameters('secCertificateUrlValue')]"
                  }

```
<span data-ttu-id="88ede-160">Nu hello resulterande Json bör se ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="88ede-160">Now hello resulting Json should look something like this.</span></span>
<span data-ttu-id="88ede-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span><span class="sxs-lookup"><span data-stu-id="88ede-161">![Json_Pub_Setting5][Json_Pub_Setting5]</span></span>


> [!NOTE]
> <span data-ttu-id="88ede-162">Kontrollera att du har upprepas steg 4 och 5 för alla hello Nodetypes/Microsoft.Compute/virtualMachineScaleSets resursdefinitionerna i mallen.</span><span class="sxs-lookup"><span data-stu-id="88ede-162">Make sure that you have repeated steps 4 and 5 for all hello Nodetypes/Microsoft.Compute/virtualMachineScaleSets resource definitions in your template.</span></span> <span data-ttu-id="88ede-163">Om du missar någon av dem hello certifikat installeras inte på den VMSS och du måste oväntade resultat i klustret, inklusive hello klustret gå (om du får inga giltiga certifikat hello klustret kan använda för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="88ede-163">If you miss one of them, hello certificate will not get installed on that VMSS and you will have unpredictable results in your cluster, including hello cluster going down (if you end up with no valid certificates that hello cluster can use for security.</span></span> <span data-ttu-id="88ede-164">Så kontrollera double, innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="88ede-164">So please double check, before proceeding further.</span></span>
> 
> 


### <a name="edit-your-template-file-tooreflect-hello-new-parameters-you-added-above"></a><span data-ttu-id="88ede-165">Redigera din fil tooreflect hello nya mallparametrar du lades till ovan</span><span class="sxs-lookup"><span data-stu-id="88ede-165">Edit your template file tooreflect hello new parameters you added above</span></span>
<span data-ttu-id="88ede-166">Om du använder hello exempel hello [git-lagringsplatsen](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow tillsammans, kan du starta toomake ändringar i hello exempel 5-VM-1-nodetypes får-Secure.paramters_Step2.JSON</span><span class="sxs-lookup"><span data-stu-id="88ede-166">If you are using hello sample from hello [git-repo](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) toofollow along, you can start toomake changes in hello sample 5-VM-1-NodeTypes-Secure.paramters_Step2.JSON</span></span> 

<span data-ttu-id="88ede-167">Redigera Resource Manager-mall parametern filen, lägga till hello två nya parametrar för secCertificateThumbprint och secCertificateUrlValue.</span><span class="sxs-lookup"><span data-stu-id="88ede-167">Edit your Resource Manager Template parameter File, add hello two new parameters for secCertificateThumbprint and secCertificateUrlValue.</span></span> 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers toohello location URL in your key vault where hello certificate was uploaded, it is should be in hello format of https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-hello-template-tooazure"></a><span data-ttu-id="88ede-168">Distribuera hello mall tooAzure</span><span class="sxs-lookup"><span data-stu-id="88ede-168">Deploy hello template tooAzure</span></span>

- <span data-ttu-id="88ede-169">Du är nu redo toodeploy mall-tooAzure.</span><span class="sxs-lookup"><span data-stu-id="88ede-169">You are now ready toodeploy your template tooAzure.</span></span> <span data-ttu-id="88ede-170">Öppna en kommandotolk för Azure PS version 1 +.</span><span class="sxs-lookup"><span data-stu-id="88ede-170">Open an Azure PS version 1+ command prompt.</span></span>
- <span data-ttu-id="88ede-171">Logga in tooyour Azure-konto och välj hello specifika azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="88ede-171">Log in tooyour Azure Account and select hello specific azure subscription.</span></span> <span data-ttu-id="88ede-172">Det här är ett viktigt steg för avdelningen som har åtkomst toomore än en azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="88ede-172">This is an important step for folks who have access toomore than one azure subscription.</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId <Subcription ID> 

```

<span data-ttu-id="88ede-173">Testa hello mallen tidigare toodeploying den.</span><span class="sxs-lookup"><span data-stu-id="88ede-173">Test hello template prior toodeploying it.</span></span> <span data-ttu-id="88ede-174">Använd hello samma resursgrupp som klustret för närvarande har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="88ede-174">Use hello same Resource Group that your cluster is currently deployed to.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

<span data-ttu-id="88ede-175">Distribuera hello mallen tooyour resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="88ede-175">Deploy hello template tooyour resource group.</span></span> <span data-ttu-id="88ede-176">Använd hello samma resursgrupp som klustret för närvarande har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="88ede-176">Use hello same Resource Group that your cluster is currently deployed to.</span></span> <span data-ttu-id="88ede-177">Kör hello ny AzureRmResourceGroupDeployment kommando.</span><span class="sxs-lookup"><span data-stu-id="88ede-177">Run hello New-AzureRmResourceGroupDeployment command.</span></span> <span data-ttu-id="88ede-178">Du behöver inte toospecify hello-läge eftersom hello standardvärdet är **inkrementella**.</span><span class="sxs-lookup"><span data-stu-id="88ede-178">You do not need toospecify hello mode, since hello default value is **incremental**.</span></span>

> [!NOTE]
> <span data-ttu-id="88ede-179">Om du ställer in läget tooComplete du av misstag tar bort resurser som inte finns i mallen.</span><span class="sxs-lookup"><span data-stu-id="88ede-179">If you set Mode tooComplete, you can inadvertently delete resources that are not in your template.</span></span> <span data-ttu-id="88ede-180">Därför inte använda den i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="88ede-180">So do not use it in this scenario.</span></span>
> 
> 

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

<span data-ttu-id="88ede-181">Här är en fylld ut exempel på hello samma powershell.</span><span class="sxs-lookup"><span data-stu-id="88ede-181">Here is a filled out example of hello same powershell.</span></span>

```powershell
$ResouceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzureRmResourceGroupDeployment -ResourceGroupName $ResouceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResouceGroup2

```

<span data-ttu-id="88ede-182">När hello distributionen är klar kan ansluta med tooyour hello nytt certifikat och köra några frågor.</span><span class="sxs-lookup"><span data-stu-id="88ede-182">Once hello deployment is complete, connect tooyour cluster using hello new Certificate and perform some queries.</span></span> <span data-ttu-id="88ede-183">Om du kan toodo.</span><span class="sxs-lookup"><span data-stu-id="88ede-183">If you are able toodo.</span></span> <span data-ttu-id="88ede-184">Du kan ta bort hello gammalt certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-184">Then you can delete hello old certificate.</span></span> 

<span data-ttu-id="88ede-185">Om du använder ett självsignerat certifikat Glöm inte tooimport dem i din lokala certifikatarkivet för TrustedPeople.</span><span class="sxs-lookup"><span data-stu-id="88ede-185">If you are using a self-signed certificate, do not forget tooimport them into your local TrustedPeople cert store.</span></span>

```powershell
######## Set up hello certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
<span data-ttu-id="88ede-186">Här är hello kommandot tooconnect tooa säker kluster för Snabbreferens</span><span class="sxs-lookup"><span data-stu-id="88ede-186">For quick reference here is hello command tooconnect tooa secure cluster</span></span> 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
<span data-ttu-id="88ede-187">Här är hello kommandot tooget klustret hälsa för Snabbreferens</span><span class="sxs-lookup"><span data-stu-id="88ede-187">For quick reference here is hello command tooget cluster health</span></span>

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-application-certificates-toohello-cluster"></a><span data-ttu-id="88ede-188">Distribuera programmet certifikat toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="88ede-188">Deploying Application certificates toohello cluster.</span></span>

<span data-ttu-id="88ede-189">Du kan använda hello samma steg som beskrivs i steg 5 ovan toohave hello certifikat distribueras från en keyvault toohello noder.</span><span class="sxs-lookup"><span data-stu-id="88ede-189">You can use hello same steps as outlined in Steps 5 above toohave hello certificates deployed from a keyvault toohello Nodes.</span></span> <span data-ttu-id="88ede-190">Du behöver bara ange och använda olika parametrar.</span><span class="sxs-lookup"><span data-stu-id="88ede-190">you just need define and use different parameters.</span></span>


## <a name="adding-or-removing-client-certificates"></a><span data-ttu-id="88ede-191">Tillägg eller borttagning av klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="88ede-191">Adding or removing Client certificates</span></span>

<span data-ttu-id="88ede-192">Du kan lägga till certifikat klienten tooperform hanteringsåtgärder på ett service fabric-kluster i tillägg toohello klustret certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-192">In addition toohello cluster certificates, you can add client certificates tooperform management operations on a service fabric cluster.</span></span>

<span data-ttu-id="88ede-193">Du kan lägga till två typer av certifikat - administratör eller skrivskyddat.</span><span class="sxs-lookup"><span data-stu-id="88ede-193">You can add two kinds of client certificates - Admin or Read-only.</span></span> <span data-ttu-id="88ede-194">Dessa kan sedan vara används toocontrol åtkomståtgärder toohello administratör och fråga åtgärder på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="88ede-194">These then can be used toocontrol access toohello admin operations and Query operations on hello cluster.</span></span> <span data-ttu-id="88ede-195">Som standard läggs hello klustercertifikat toohello Admin certifikat listan över tillåtna.</span><span class="sxs-lookup"><span data-stu-id="88ede-195">By default, hello cluster certificates are added toohello allowed Admin certificates list.</span></span>

<span data-ttu-id="88ede-196">Du kan ange valfritt antal klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-196">you can specify any number of client certificates.</span></span> <span data-ttu-id="88ede-197">Varje tillägg/borttagning resulterar i en konfiguration uppdatering toohello service fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="88ede-197">Each addition/deletion results in a configuration update toohello service fabric cluster</span></span>


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a><span data-ttu-id="88ede-198">Lägga till klientcertifikat - administratör eller skrivskyddad via portalen</span><span class="sxs-lookup"><span data-stu-id="88ede-198">Adding client certificates - Admin or Read-Only via portal</span></span>

1. <span data-ttu-id="88ede-199">Navigera toohello säkerhet bladet och markera hello ”+ autentisering” knappen ovanpå hello säkerhet bladet.</span><span class="sxs-lookup"><span data-stu-id="88ede-199">Navigate toohello Security blade, and select hello '+ Authentication' button on top of hello security blade.</span></span>
2. <span data-ttu-id="88ede-200">På bladet Lägg till autentisering hello Välj hello 'Autentiseringstyp' - 'Skrivskyddad client' eller 'Admin client'</span><span class="sxs-lookup"><span data-stu-id="88ede-200">On hello 'Add Authentication' blade, choose hello 'Authentication Type' - 'Read-only client' or 'Admin client'</span></span>
3. <span data-ttu-id="88ede-201">Nu välja hello auktoriseringsmetod.</span><span class="sxs-lookup"><span data-stu-id="88ede-201">Now choose hello Authorization method.</span></span> <span data-ttu-id="88ede-202">Detta anger tooService Fabric om det ska sökas upp det här certifikatet med hjälp av hello ämnesnamn eller hello tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="88ede-202">This indicates tooService Fabric whether it should look up this certificate by using hello subject name or hello thumbprint.</span></span> <span data-ttu-id="88ede-203">Det är i allmänhet inte en god praxis toouse hello auktorisering säkerhetsmetod Ämnesnamnets.</span><span class="sxs-lookup"><span data-stu-id="88ede-203">In general, it is not a good security practice toouse hello authorization method of subject name.</span></span> 

![Lägg till klientcertifikat][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-hello-portal"></a><span data-ttu-id="88ede-205">Borttagning av klientcertifikat - administratör eller med hjälp av skrivskyddad hello portal</span><span class="sxs-lookup"><span data-stu-id="88ede-205">Deletion of Client Certificates - Admin or Read-Only using hello portal</span></span>

<span data-ttu-id="88ede-206">tooremove ett sekundärt certifikat används för klustret säkerhet, analysera toohello säkerhet bladet och väljer hello 'Radera' alternativet hello snabbmenyn på hello specifika certifikat.</span><span class="sxs-lookup"><span data-stu-id="88ede-206">tooremove a secondary certificate from being used for cluster security, Navigate toohello Security blade and select hello 'Delete' option from hello context menu on hello specific certificate.</span></span>



## <a name="next-steps"></a><span data-ttu-id="88ede-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="88ede-207">Next steps</span></span>
<span data-ttu-id="88ede-208">Läs artiklarna mer information om hantering av kluster:</span><span class="sxs-lookup"><span data-stu-id="88ede-208">Read these articles for more information on cluster management:</span></span>

* [<span data-ttu-id="88ede-209">Uppgraderingsprocessen för Service Fabric-kluster och förväntningar från dig</span><span class="sxs-lookup"><span data-stu-id="88ede-209">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="88ede-210">Konfigurera rollbaserad åtkomst för klienter</span><span class="sxs-lookup"><span data-stu-id="88ede-210">Setup role-based access for clients</span></span>](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Delete_Swap_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_09.PNG
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


