---
title: "aaaSetting in ett Service Fabric-kluster med hjälp av Visual Studio | Microsoft Docs"
description: "Beskriver hur tooset upp ett Service Fabric-kluster med hjälp av Azure Resource Manager-mall som skapats av en Azure-resursgrupp-projekt i Visual Studio"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: adb0dd2169a28b46e832c6f06c998cbed0c473f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a><span data-ttu-id="bd7d9-103">Konfigurera ett Service Fabric-kluster med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd7d9-103">Set up a Service Fabric cluster by using Visual Studio</span></span>
<span data-ttu-id="bd7d9-104">Den här artikeln beskriver hur tooset upp ett Azure Service Fabric-kluster med hjälp av Visual Studio och en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-104">This article describes how tooset up an Azure Service Fabric cluster by using Visual Studio and an Azure Resource Manager template.</span></span> <span data-ttu-id="bd7d9-105">Vi använder en Visual Studio Azure resource group toocreate hello projektmall.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-105">We will use a Visual Studio Azure resource group project toocreate hello template.</span></span> <span data-ttu-id="bd7d9-106">Kan distribueras efter hello mallen har skapats direkt tooAzure från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-106">After hello template has been created, it can be deployed directly tooAzure from Visual Studio.</span></span> <span data-ttu-id="bd7d9-107">Det kan också användas från ett skript eller som en del av kontinuerlig integration (KO).</span><span class="sxs-lookup"><span data-stu-id="bd7d9-107">It can also be used from a script, or as part of continuous integration (CI) facility.</span></span>

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a><span data-ttu-id="bd7d9-108">Skapa en mall för Service Fabric-kluster med hjälp av en Azure resursgruppsprojektet</span><span class="sxs-lookup"><span data-stu-id="bd7d9-108">Create a Service Fabric cluster template by using an Azure resource group project</span></span>
<span data-ttu-id="bd7d9-109">tooget igång, öppna Visual Studio och skapa ett Azure resursgruppsprojektet (tillgänglig i hello **moln** mapp):</span><span class="sxs-lookup"><span data-stu-id="bd7d9-109">tooget started, open Visual Studio and create an Azure resource group project (it is available in hello **Cloud** folder):</span></span>

![Dialogrutan Nytt projekt med Azure-resursgrupp projektet som du valt][1]

<span data-ttu-id="bd7d9-111">Du kan skapa en ny Visual Studio-lösning för det här projektet eller lägga till den tooan befintlig lösning.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-111">You can create a new Visual Studio solution for this project or add it tooan existing solution.</span></span>

> [!NOTE]
> <span data-ttu-id="bd7d9-112">Om du inte ser hello Azure resursgruppsprojektet under hello molnet noden har inte hello Azure SDK är installerat.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-112">If you do not see hello Azure resource group project under hello Cloud node, you do not have hello Azure SDK installed.</span></span> <span data-ttu-id="bd7d9-113">Starta installationsprogrammet för webbplattform ([installera nu](http://www.microsoft.com/web/downloads/platform.aspx) om du inte redan har), och Sök efter ”Azure SDK för .NET” och installera hello-version som är kompatibel med din version av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-113">Launch Web Platform Installer ([install it now](http://www.microsoft.com/web/downloads/platform.aspx) if you have not already), and then search for "Azure SDK for .NET" and install hello version that is compatible with your version of Visual Studio.</span></span>
> 
> 

<span data-ttu-id="bd7d9-114">När du klickar på OK hello-knappen, ber Visual Studio dig tooselect hello Resource Manager-mall som du vill toocreate:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-114">After you hit hello OK button, Visual Studio will ask you tooselect hello Resource Manager template you want toocreate:</span></span>

![Välj dialogrutan mall i Azure med valda Service Fabric-kluster-mallen][2]

<span data-ttu-id="bd7d9-116">Välj mall för hello Service Fabric-kluster och träffar hello OK-knappen igen.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-116">Select hello Service Fabric Cluster template and hit hello OK button again.</span></span> <span data-ttu-id="bd7d9-117">hello-projektet och hello Resource Manager-mall har skapats.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-117">hello project and hello Resource Manager template have now been created.</span></span>

## <a name="prepare-hello-template-for-deployment"></a><span data-ttu-id="bd7d9-118">Förbereda hello mall för distribution</span><span class="sxs-lookup"><span data-stu-id="bd7d9-118">Prepare hello template for deployment</span></span>
<span data-ttu-id="bd7d9-119">Innan hello mallen är distribuerade toocreate hello kluster, måste du ange värden för mallparametrar hello krävs.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-119">Before hello template is deployed toocreate hello cluster, you must provide values for hello required template parameters.</span></span> <span data-ttu-id="bd7d9-120">Dessa parametervärden läses från hello `ServiceFabricCluster.parameters.json` -filen i hello `Templates` hello resursgruppsprojektet mapp.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-120">These parameter values are read from hello `ServiceFabricCluster.parameters.json` file, which is in hello `Templates` folder of hello resource group project.</span></span> <span data-ttu-id="bd7d9-121">Öppna hello-filen och ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-121">Open hello file and provide hello following values:</span></span>

| <span data-ttu-id="bd7d9-122">Parameternamn</span><span class="sxs-lookup"><span data-stu-id="bd7d9-122">Parameter name</span></span> | <span data-ttu-id="bd7d9-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bd7d9-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bd7d9-124">adminUserName</span><span class="sxs-lookup"><span data-stu-id="bd7d9-124">adminUserName</span></span> |<span data-ttu-id="bd7d9-125">hello namnet på hello administratörskontot för Service Fabric-datorer (noder).</span><span class="sxs-lookup"><span data-stu-id="bd7d9-125">hello name of hello administrator account for Service Fabric machines (nodes).</span></span> |
| <span data-ttu-id="bd7d9-126">certificateThumbprint</span><span class="sxs-lookup"><span data-stu-id="bd7d9-126">certificateThumbprint</span></span> |<span data-ttu-id="bd7d9-127">hello tumavtrycket för certifikatet för hello som skyddar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-127">hello thumbprint of hello certificate that secures hello cluster.</span></span> |
| <span data-ttu-id="bd7d9-128">sourceVaultResourceId</span><span class="sxs-lookup"><span data-stu-id="bd7d9-128">sourceVaultResourceId</span></span> |<span data-ttu-id="bd7d9-129">Hej *resurs-ID* av hello nyckelvalv där hello-certifikatet som skyddar hello kluster lagras.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-129">hello *resource ID* of hello key vault where hello certificate that secures hello cluster is stored.</span></span> |
| <span data-ttu-id="bd7d9-130">certificateUrlValue</span><span class="sxs-lookup"><span data-stu-id="bd7d9-130">certificateUrlValue</span></span> |<span data-ttu-id="bd7d9-131">hello-URL för hello klustret säkerhetscertifikat.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-131">hello URL of hello cluster security certificate.</span></span> |

<span data-ttu-id="bd7d9-132">hello Visual Studio Service Fabric Resource Manager-mall skapar en säker kluster som skyddas av ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-132">hello Visual Studio Service Fabric Resource Manager template creates a secure cluster that is protected by a certificate.</span></span> <span data-ttu-id="bd7d9-133">Det här certifikatet har identifierats av hello senaste tre mallparametrar (`certificateThumbprint`, `sourceVaultValue`, och `certificateUrlValue`), och det måste finnas i en **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-133">This certificate is identified by hello last three template parameters (`certificateThumbprint`, `sourceVaultValue`, and `certificateUrlValue`), and it must exist in an **Azure Key Vault**.</span></span> <span data-ttu-id="bd7d9-134">Mer information om hur toocreate hello klustret säkerhetscertifikat finns [säkerhetsscenarier för Service Fabric-klustret](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) artikel.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-134">For more information on how toocreate hello cluster security certificate, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) article.</span></span>

## <a name="optional-change-hello-cluster-name"></a><span data-ttu-id="bd7d9-135">Valfritt: ändra hello klustrets namn</span><span class="sxs-lookup"><span data-stu-id="bd7d9-135">Optional: change hello cluster name</span></span>
<span data-ttu-id="bd7d9-136">Varje Service Fabric-klustret har ett namn.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-136">Every Service Fabric cluster has a name.</span></span> <span data-ttu-id="bd7d9-137">När en Fabric-klustret har skapats i Azure, klusternamnet avgör (tillsammans med hello Azure-region) hello Domain Name System (DNS) namn för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-137">When a Fabric cluster is created in Azure, cluster name determines (together with hello Azure region) hello Domain Name System (DNS) name for hello cluster.</span></span> <span data-ttu-id="bd7d9-138">Om du namnger klustret exempelvis `myBigCluster`, och hello (Azure-region) hello resursgruppen som är värd för hello nya klustret finns östra USA blir hello DNS-namnet på klustret hello `myBigCluster.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-138">For example, if you name your cluster `myBigCluster`, and hello location (Azure region) of hello resource group that will host hello new cluster is East US, hello DNS name of hello cluster will be `myBigCluster.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="bd7d9-139">Som standard är hello klusternamnet genereras automatiskt och blir unikt genom att koppla ett slumpmässigt suffix tooa ”cluster” prefix.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-139">By default hello cluster name is generated automatically and made unique by attaching a random suffix tooa "cluster" prefix.</span></span> <span data-ttu-id="bd7d9-140">Detta gör det mycket enkelt toouse hello mallen som en del av en **kontinuerlig integration** (KO)-system.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-140">This makes it very easy toouse hello template as part of a **continuous integration** (CI) system.</span></span> <span data-ttu-id="bd7d9-141">Om du vill toouse ett specifikt namn för klustret, ett som är meningsfullt tooyou anger hello värdet för hello `clusterName` variabeln i hello Resource Manager mallfilen (`ServiceFabricCluster.json`) tooyour valt namn.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-141">If you want toouse a specific name for your cluster, one that is meaningful tooyou, set hello value of hello `clusterName` variable in hello Resource Manager template file (`ServiceFabricCluster.json`) tooyour chosen name.</span></span> <span data-ttu-id="bd7d9-142">Det är första hello variabel som anges i den filen.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-142">It is hello first variable defined in that file.</span></span>

## <a name="optional-add-public-application-ports"></a><span data-ttu-id="bd7d9-143">Valfritt: lägga till offentliga program portar</span><span class="sxs-lookup"><span data-stu-id="bd7d9-143">Optional: add public application ports</span></span>
<span data-ttu-id="bd7d9-144">Du kan också toochange hello offentliga program portar för hello klustret innan du distribuerar den.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-144">You may also want toochange hello public application ports for hello cluster before you deploy it.</span></span> <span data-ttu-id="bd7d9-145">Som standard öppnas hello mallen två offentliga TCP-portar (80 och 8081).</span><span class="sxs-lookup"><span data-stu-id="bd7d9-145">By default, hello template opens up just two public TCP ports (80 and 8081).</span></span> <span data-ttu-id="bd7d9-146">Om du behöver mer för dina program, ändra hello Azure belastningsutjämnare definition i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-146">If you need more for your applications, modify hello Azure Load Balancer definition in hello template.</span></span> <span data-ttu-id="bd7d9-147">hello definition lagras i hello Huvudmall filen (`ServiceFabricCluster.json`).</span><span class="sxs-lookup"><span data-stu-id="bd7d9-147">hello definition is stored in hello main template file (`ServiceFabricCluster.json`).</span></span> <span data-ttu-id="bd7d9-148">Öppna filen och Sök efter `loadBalancedAppPort`.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-148">Open that file and search for `loadBalancedAppPort`.</span></span> <span data-ttu-id="bd7d9-149">Varje port är kopplat till tre artefakter:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-149">Each port is associated with three artifacts:</span></span>

1. <span data-ttu-id="bd7d9-150">Mallvariabeln som definierar hello TCP-portvärde för hello port:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-150">A template variable that defines hello TCP port value for hello port:</span></span>
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. <span data-ttu-id="bd7d9-151">En *avsökningen* som definierar hur ofta och hur länge hello Azure belastningsutjämnare försöker toouse en specifik Service Fabric-nod innan åtgärden misslyckas över tooanother en.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-151">A *probe* that defines how frequently and for how long hello Azure load balancer attempts toouse a specific Service Fabric node before failing over tooanother one.</span></span> <span data-ttu-id="bd7d9-152">hello-avsökningar som ingår i hello belastningsutjämnaren resurs.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-152">hello probes are part of hello Load Balancer resource.</span></span> <span data-ttu-id="bd7d9-153">Här är hello avsökningen definition för hello första programmet standardporten:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-153">Here is hello probe definition for hello first default application port:</span></span>
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. <span data-ttu-id="bd7d9-154">En *belastningsutjämning regeln* som binder samman hello port och hello avsökning, vilket möjliggör belastningsutjämning över en uppsättning noder i Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-154">A *load-balancing rule* that ties together hello port and hello probe, which enables load balancing across a set of Service Fabric cluster nodes:</span></span>
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   <span data-ttu-id="bd7d9-155">Om fler portar hello-program som du planerar toodeploy toohello klustret måste du lägga till dem genom att skapa ytterligare avsökning och Regeldefinitioner för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-155">If hello applications that you plan toodeploy toohello cluster need more ports, you can add them by creating additional probe and load-balancing rule definitions.</span></span> <span data-ttu-id="bd7d9-156">Mer information om hur toowork med Azure belastningsutjämnare via Resource Manager-mallar finns [komma igång med en intern belastningsutjämnare använder en mall](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="bd7d9-156">For more information on how toowork with Azure Load Balancer through Resource Manager templates, see [Get started creating an internal load balancer using a template](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span></span>

## <a name="deploy-hello-template-by-using-visual-studio"></a><span data-ttu-id="bd7d9-157">Distribuera hello-mallen med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd7d9-157">Deploy hello template by using Visual Studio</span></span>
<span data-ttu-id="bd7d9-158">När du har sparat alla hello obligatorisk parametervärdena i den`ServiceFabricCluster.param.dev.json` filen som du är klar toodeploy hello mallen och skapar Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-158">After you have saved all hello required parameter values in the`ServiceFabricCluster.param.dev.json` file, you are ready toodeploy hello template and create your Service Fabric cluster.</span></span> <span data-ttu-id="bd7d9-159">Högerklicka på hello resursgruppsprojektet i Visual Studio Solution Explorer och välj **distribuera | Ny distribution...** . Om nödvändigt, Visual Studio visar hello **distribuera tooResource grupp** i dialogrutan där du tooauthenticate tooAzure:</span><span class="sxs-lookup"><span data-stu-id="bd7d9-159">Right-click hello resource group project in Visual Studio Solution Explorer and choose **Deploy | New Deployment...**. If necessary, Visual Studio will show hello **Deploy tooResource Group** dialog box, asking you tooauthenticate tooAzure:</span></span>

![Distribuera tooResource Gruppdialogrutan][3]

<span data-ttu-id="bd7d9-161">hello dialogrutan kan du välja en befintlig resursgrupp för Resource Manager för hello kluster och ger du hello alternativet toocreate en ny.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-161">hello dialog box lets you choose an existing Resource Manager resource group for hello cluster and gives you hello option toocreate a new one.</span></span> <span data-ttu-id="bd7d9-162">Normalt gör det klokt toouse en separat resursgrupp för Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-162">It normally makes sense toouse a separate resource group for a Service Fabric cluster.</span></span>

<span data-ttu-id="bd7d9-163">När du träffar hello distribuera knappen, uppmanas Visual Studio du tooconfirm hello parametervärden för mallen.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-163">After you hit hello Deploy button, Visual Studio will prompt you tooconfirm hello template parameter values.</span></span> <span data-ttu-id="bd7d9-164">Träffar hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-164">Hit hello **Save** button.</span></span> <span data-ttu-id="bd7d9-165">En parameter har inte en beständiga värde: hello administrativa kontolösenord för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-165">One parameter does not have a persisted value: hello administrative account password for hello cluster.</span></span> <span data-ttu-id="bd7d9-166">Du behöver tooprovide lösenordsvärdet när Visual Studio efterfrågar en.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-166">You need tooprovide a password value when Visual Studio prompts you for one.</span></span>

> [!NOTE]
> <span data-ttu-id="bd7d9-167">Från och med Azure SDK 2.9, Visual Studio stöder läsning lösenord från **Azure Key Vault** under distributionen.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-167">Starting with Azure SDK 2.9, Visual Studio supports reading passwords from **Azure Key Vault** during deployment.</span></span> <span data-ttu-id="bd7d9-168">Observera att hello i dialogrutan för hello mall parametrar `adminPassword` parametern textruta har en liten ”key”-ikon på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-168">In hello template parameters dialog notice that hello `adminPassword` parameter text box has a little "key" icon on hello right.</span></span> <span data-ttu-id="bd7d9-169">Den här ikonen kan tooselect en befintlig hemlighet i nyckelvalvet som hello administratörslösenord för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-169">This icon allows you tooselect an existing key vault secret as hello administrative password for hello cluster.</span></span> <span data-ttu-id="bd7d9-170">Kontrollera att toofirst aktivera Azure Resource Manager-åtkomst för malldistribution i hello avancerade åtkomstprinciper för nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-170">Just make sure toofirst enable Azure Resource Manager access for template deployment in hello Advanced Access Policies of your key vault.</span></span> 
> 
> 

<span data-ttu-id="bd7d9-171">Du kan övervaka hello hello distributionsprocessen i utdatafönstret för hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-171">You can monitor hello progress of hello deployment process in hello Visual Studio output window.</span></span> <span data-ttu-id="bd7d9-172">När hello mallen distributionen är klar, är det nya klustret redo toouse!</span><span class="sxs-lookup"><span data-stu-id="bd7d9-172">Once hello template deployment is completed, your new cluster is ready toouse!</span></span>

> [!NOTE]
> <span data-ttu-id="bd7d9-173">Om PowerShell aldrig har använt tooadminister Azure från hello-dator som du använder nu måste toodo lite underhåll.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-173">If PowerShell was never used tooadminister Azure from hello machine that you are using now, you need toodo a little housekeeping.</span></span>
> 
> 1. <span data-ttu-id="bd7d9-174">Aktivera PowerShell-skript genom att köra hello [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) kommando.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-174">Enable PowerShell scripting by running hello [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) command.</span></span> <span data-ttu-id="bd7d9-175">”Obegränsad” principen är oftast giltiga för utveckling datorer.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-175">For development machines, "unrestricted" policy is usually acceptable.</span></span>
> 2. <span data-ttu-id="bd7d9-176">Bestäm om tooallow Diagnostisk datainsamling från Azure PowerShell-kommandon och kör [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) eller [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) efter behov.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-176">Decide whether tooallow diagnostic data collection from Azure PowerShell commands, and run [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) or [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) as necessary.</span></span> <span data-ttu-id="bd7d9-177">På så sätt undviker onödiga frågor när mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-177">This will avoid unnecessary prompts during template deployment.</span></span>
> 
> 

<span data-ttu-id="bd7d9-178">Om det finns några fel, gå toohello [Azure-portalen](https://portal.azure.com/) och öppna hello resursgruppen som du har distribuerat till.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-178">If there are any errors, go toohello [Azure portal](https://portal.azure.com/) and open hello resource group that you deployed to.</span></span> <span data-ttu-id="bd7d9-179">Klicka på **alla inställningar**, klicka på **distributioner** på inställningsbladet för hello.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-179">Click **All settings**, then click **Deployments** on hello settings blade.</span></span> <span data-ttu-id="bd7d9-180">Distributionen misslyckades-resursgrupp lämnar det detaljerad diagnostisk information.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-180">A failed resource-group deployment leaves detailed diagnostic information there.</span></span>

> [!NOTE]
> <span data-ttu-id="bd7d9-181">Service Fabric-kluster kräver ett visst antal noder toobe toomaintain tillgänglighet och bevara tillstånd - refererad tooas ”för att underhålla kvorum”.</span><span class="sxs-lookup"><span data-stu-id="bd7d9-181">Service Fabric clusters require a certain number of nodes toobe up toomaintain availability and preserve state - referred tooas "maintaining quorum."</span></span> <span data-ttu-id="bd7d9-182">Det är därför inte säker tooshut ned alla hello datorer i hello kluster om du först har utfört en [fullständig säkerhetskopiering av din tillstånd](service-fabric-reliable-services-backup-restore.md).</span><span class="sxs-lookup"><span data-stu-id="bd7d9-182">Therefore, it is not safe tooshut down all of hello machines in hello cluster unless you have first performed a [full backup of your state](service-fabric-reliable-services-backup-restore.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="bd7d9-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd7d9-183">Next steps</span></span>
* [<span data-ttu-id="bd7d9-184">Lär dig mer om hur du konfigurerar Service Fabric-kluster med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bd7d9-184">Learn about setting up Service Fabric cluster using hello Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="bd7d9-185">Lär dig hur toomanage och distribuerar Service Fabric-program med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd7d9-185">Learn how toomanage and deploy Service Fabric applications using Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
