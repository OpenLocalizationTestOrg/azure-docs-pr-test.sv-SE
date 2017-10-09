---
title: "aaaHow toocreate och distribuera en tjänst i molnet | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera en tjänst i molnet med hjälp av hello Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: dc8b81a594f3514e662c49c9a46a33da8a551f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a><span data-ttu-id="3f443-103">Hur toocreate och distribuera en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="3f443-103">How toocreate and deploy a cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f443-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3f443-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="3f443-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="3f443-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
>
>

<span data-ttu-id="3f443-106">hello Azure-portalen ger dig två sätt du toocreate och distribuera en tjänst i molnet: *Snabbregistrering* och *skapa anpassade*.</span><span class="sxs-lookup"><span data-stu-id="3f443-106">hello Azure portal provides two ways for you toocreate and deploy a cloud service: *Quick Create* and *Custom Create*.</span></span>

<span data-ttu-id="3f443-107">Den här artikeln förklarar hur toouse hello Snabbregistrering metoden toocreate en ny molntjänst och sedan använda **överför** tooupload och distribuera ett paket med cloud service i Azure.</span><span class="sxs-lookup"><span data-stu-id="3f443-107">This article explains how toouse hello Quick Create method toocreate a new cloud service and then use **Upload** tooupload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="3f443-108">När du använder den här metoden gör tillgängliga praktiska länkar för att slutföra alla krav när du går hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f443-108">When you use this method, hello Azure portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="3f443-109">Om du är klar toodeploy ditt moln tjänst när du har skapat den, kan du göra både på hello samtidigt med skapa anpassade.</span><span class="sxs-lookup"><span data-stu-id="3f443-109">If you're ready toodeploy your cloud service when you create it, you can do both at hello same time using Custom Create.</span></span>

> [!NOTE]
> <span data-ttu-id="3f443-110">Om du planerar toopublish Molntjänsten från Visual Studio Team Services VSTS (), använda Snabbregistrering och sedan konfigurera VSTS publicering hello Azure Quickstart eller hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="3f443-110">If you plan toopublish your cloud service from Visual Studio Team Services (VSTS), use Quick Create, and then set up VSTS publishing from hello Azure Quickstart or hello dashboard.</span></span> <span data-ttu-id="3f443-111">Mer information finns i [kontinuerlig leverans tooAzure med hjälp av Visual Studio Team Services][TFSTutorialForCloudService], eller se hjälp för hello **Snabbstart** sidan.</span><span class="sxs-lookup"><span data-stu-id="3f443-111">For more information, see [Continuous Delivery tooAzure by Using Visual Studio Team Services][TFSTutorialForCloudService], or see help for hello **Quick Start** page.</span></span>
>
>

## <a name="concepts"></a><span data-ttu-id="3f443-112">Koncept</span><span class="sxs-lookup"><span data-stu-id="3f443-112">Concepts</span></span>
<span data-ttu-id="3f443-113">Nödvändiga toodeploy ett program som en tjänst i molnet i Azure består av tre komponenter:</span><span class="sxs-lookup"><span data-stu-id="3f443-113">Three components are required toodeploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="3f443-114">**Tjänstdefinitionen**</span><span class="sxs-lookup"><span data-stu-id="3f443-114">**Service Definition**</span></span>  
  <span data-ttu-id="3f443-115">hello molnet tjänstdefinitionsfilen (.csdef) definierar hello modell, inklusive hello antal roller.</span><span class="sxs-lookup"><span data-stu-id="3f443-115">hello cloud service definition file (.csdef) defines hello service model, including hello number of roles.</span></span>
* <span data-ttu-id="3f443-116">**Tjänstkonfiguration**</span><span class="sxs-lookup"><span data-stu-id="3f443-116">**Service Configuration**</span></span>  
  <span data-ttu-id="3f443-117">hello molnet tjänstekonfigurationsfilen (.cscfg) innehåller konfigurationsinställningar för hello Molntjänsten och enskilda roller, inklusive hello antalet rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="3f443-117">hello cloud service configuration file (.cscfg) provides configuration settings for hello cloud service and individual roles, including hello number of role instances.</span></span>
* <span data-ttu-id="3f443-118">**Tjänstpaket**</span><span class="sxs-lookup"><span data-stu-id="3f443-118">**Service Package**</span></span>  
  <span data-ttu-id="3f443-119">hello service-paketet (.cspkg) innehåller hello programkod och konfigurationer och hello tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="3f443-119">hello service package (.cspkg) contains hello application code and configurations and hello service definition file.</span></span>

<span data-ttu-id="3f443-120">Du kan lära dig mer om de här och hur toocreate ett paket [här](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="3f443-120">You can learn more about these and how toocreate a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="3f443-121">Förbereda din app</span><span class="sxs-lookup"><span data-stu-id="3f443-121">Prepare your app</span></span>
<span data-ttu-id="3f443-122">Innan du kan distribuera en tjänst i molnet, måste du skapa hello cloud service-paketet (.cspkg) från din programkod och ett moln tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="3f443-122">Before you can deploy a cloud service, you must create hello cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="3f443-123">hello Azure SDK innehåller verktyg för att förbereda filerna obligatorisk distribution.</span><span class="sxs-lookup"><span data-stu-id="3f443-123">hello Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="3f443-124">Du kan installera hello SDK från hello [Azure hämtar](https://azure.microsoft.com/downloads/) hello språk som du föredrar toodevelop sidan programkoden.</span><span class="sxs-lookup"><span data-stu-id="3f443-124">You can install hello SDK from hello [Azure Downloads](https://azure.microsoft.com/downloads/) page, in hello language in which you prefer toodevelop your application code.</span></span>

<span data-ttu-id="3f443-125">Tre kräver cloud tjänsten särskilda konfigurationer innan du exporterar en tjänstpaket:</span><span class="sxs-lookup"><span data-stu-id="3f443-125">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="3f443-126">Om du vill toodeploy en molnbaserad tjänst som använder Secure Sockets Layer (SSL) för kryptering av data, [konfigurera ditt program](cloud-services-configure-ssl-certificate-portal.md#modify) för SSL.</span><span class="sxs-lookup"><span data-stu-id="3f443-126">If you want toodeploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate-portal.md#modify) for SSL.</span></span>
* <span data-ttu-id="3f443-127">Om du vill tooconfigure Fjärrskrivbord anslutningar toorole instanser [konfigurerar hello roller](cloud-services-role-enable-remote-desktop-new-portal.md) för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="3f443-127">If you want tooconfigure Remote Desktop connections toorole instances, [configure hello roles](cloud-services-role-enable-remote-desktop-new-portal.md) for Remote Desktop.</span></span>
* <span data-ttu-id="3f443-128">Om du vill tooconfigure utförlig övervakning för Molntjänsten, kan du aktivera Azure-diagnostik för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3f443-128">If you want tooconfigure verbose monitoring for your cloud service, enable Azure Diagnostics for hello cloud service.</span></span> <span data-ttu-id="3f443-129">*Minimal övervakning* (hello standardinställningar för övervakning nivå) använder prestandaräknare som samlats in från hello värdoperativsystem för rollinstanser (virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="3f443-129">*Minimal monitoring* (hello default monitoring level) uses performance counters gathered from hello host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="3f443-130">*Utförlig övervakning* samlar in ytterligare mått baserat på prestandadata i hello rollen instanser tooenable närmare analys av problem som uppstår under bearbetningen av programmet.</span><span class="sxs-lookup"><span data-stu-id="3f443-130">*Verbose monitoring* gathers additional metrics based on performance data within hello role instances tooenable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="3f443-131">toofind reda på hur Azure-diagnostik för tooenable finns [aktiverar diagnostik i Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="3f443-131">toofind out how tooenable Azure Diagnostics, see [Enabling diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="3f443-132">toocreate en molntjänst med distributioner av webbroller eller arbetsroller, måste du [skapa hello servicepaket](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="3f443-132">toocreate a cloud service with deployments of web roles or worker roles, you must [create hello service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="3f443-133">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3f443-133">Before you begin</span></span>
* <span data-ttu-id="3f443-134">Om du inte har installerat hello Azure SDK, klickar du på **installera Azure SDK** tooopen hello [Azure hämtar sidan](https://azure.microsoft.com/downloads/), och sedan hämta hello SDK för hello språk som du föredrar toodevelop din kod.</span><span class="sxs-lookup"><span data-stu-id="3f443-134">If you haven't installed hello Azure SDK, click **Install Azure SDK** tooopen hello [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download hello SDK for hello language in which you prefer toodevelop your code.</span></span> <span data-ttu-id="3f443-135">(Du har en möjlighet toodo detta senare.)</span><span class="sxs-lookup"><span data-stu-id="3f443-135">(You'll have an opportunity toodo this later.)</span></span>
* <span data-ttu-id="3f443-136">Om alla rollinstanser kräver ett certifikat, skapa hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f443-136">If any role instances require a certificate, create hello certificates.</span></span> <span data-ttu-id="3f443-137">Molntjänster kräver en .pfx-fil med en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="3f443-137">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="3f443-138">Du kan överföra hello certifikat tooAzure som du skapar och distribuerar hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3f443-138">You can upload hello certificates tooAzure as you create and deploy hello cloud service.</span></span>

## <a name="create-and-deploy"></a><span data-ttu-id="3f443-139">Skapa och distribuera</span><span class="sxs-lookup"><span data-stu-id="3f443-139">Create and deploy</span></span>
1. <span data-ttu-id="3f443-140">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3f443-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3f443-141">Klicka på **New > Compute**, och bläddrar sedan nedåt tooand Klicka **Molntjänsten**.</span><span class="sxs-lookup"><span data-stu-id="3f443-141">Click **New > Compute**, and then scroll down tooand click **Cloud Service**.</span></span>

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. <span data-ttu-id="3f443-143">I nya hello **Molntjänsten** bladet, ange ett värde för hello **DNS-namnet**.</span><span class="sxs-lookup"><span data-stu-id="3f443-143">In hello new **Cloud Service** blade, enter a value for hello **DNS name**.</span></span>
4. <span data-ttu-id="3f443-144">Skapa en ny **resursgruppen** eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="3f443-144">Create a new **Resource Group** or select an existing one.</span></span>
5. <span data-ttu-id="3f443-145">Välj en **Plats**.</span><span class="sxs-lookup"><span data-stu-id="3f443-145">Select a **Location**.</span></span>
6. <span data-ttu-id="3f443-146">Klicka på **paketet**.</span><span class="sxs-lookup"><span data-stu-id="3f443-146">Click **Package**.</span></span> <span data-ttu-id="3f443-147">Då öppnas hello **överföra ett paket** bladet.</span><span class="sxs-lookup"><span data-stu-id="3f443-147">This will open hello **Upload a package** blade.</span></span> <span data-ttu-id="3f443-148">Fyll i hello krävs.</span><span class="sxs-lookup"><span data-stu-id="3f443-148">Fill in hello required fields.</span></span> <span data-ttu-id="3f443-149">Om någon av dina roller innehåller en enda instans, kontrollerar du **distribuera även om en eller flera roller innehåller en enda instans** är markerad.</span><span class="sxs-lookup"><span data-stu-id="3f443-149">If any of your roles contain a single instance, ensure **Deploy even if one or more roles contain a single instance** is selected.</span></span>
7. <span data-ttu-id="3f443-150">Se till att **starta distribution** är markerad.</span><span class="sxs-lookup"><span data-stu-id="3f443-150">Make sure that **Start deployment** is selected.</span></span>
8. <span data-ttu-id="3f443-151">Klicka på **OK** som stänger hello **överföra ett paket** bladet.</span><span class="sxs-lookup"><span data-stu-id="3f443-151">Click **OK** which will close hello **Upload a package** blade.</span></span>
9. <span data-ttu-id="3f443-152">Om du inte har några certifikat tooadd klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3f443-152">If you do not have any certificates tooadd, click **Create**.</span></span>

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a><span data-ttu-id="3f443-154">Överföra ett certifikat</span><span class="sxs-lookup"><span data-stu-id="3f443-154">Upload a certificate</span></span>
<span data-ttu-id="3f443-155">Om din distributionspaketet [konfigurerats toouse certifikat](cloud-services-configure-ssl-certificate-portal.md#modify), kan du överföra hello certifikat nu.</span><span class="sxs-lookup"><span data-stu-id="3f443-155">If your deployment package was [configured toouse certificates](cloud-services-configure-ssl-certificate-portal.md#modify), you can upload hello certificate now.</span></span>

1. <span data-ttu-id="3f443-156">Välj **certifikat**, och på hello **lägga till certifikat** bladet välj hello SSL-certifikat PFX-fil och ange sedan hello **lösenord** för hello certifikat</span><span class="sxs-lookup"><span data-stu-id="3f443-156">Select **Certificates**, and on hello **Add certificates** blade, select hello SSL certificate .pfx file, and then provide hello **Password** for hello certificate,</span></span>
2. <span data-ttu-id="3f443-157">Klicka på **bifoga certifikat**, och klicka sedan på **OK** på hello **lägga till certifikat** bladet.</span><span class="sxs-lookup"><span data-stu-id="3f443-157">Click **Attach certificate**, and then click **OK** on hello **Add certificates** blade.</span></span>
3. <span data-ttu-id="3f443-158">Klicka på **skapa** på hello **Molntjänsten** bladet.</span><span class="sxs-lookup"><span data-stu-id="3f443-158">Click **Create** on hello **Cloud Service** blade.</span></span> <span data-ttu-id="3f443-159">När hello distribution har nått hello **klar** status, kan du fortsätta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="3f443-159">When hello deployment has reached hello **Ready** status, you can proceed toohello next steps.</span></span>

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="3f443-161">Kontrollera distributionen har slutförts</span><span class="sxs-lookup"><span data-stu-id="3f443-161">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="3f443-162">Klicka på hello cloud service-instans.</span><span class="sxs-lookup"><span data-stu-id="3f443-162">Click hello cloud service instance.</span></span>

    <span data-ttu-id="3f443-163">hello status ska visa att hello-tjänsten är **kör**.</span><span class="sxs-lookup"><span data-stu-id="3f443-163">hello status should show that hello service is **Running**.</span></span>
2. <span data-ttu-id="3f443-164">Under **Essentials**, klicka på hello **Webbadress** tooopen ditt moln-tjänsten i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3f443-164">Under **Essentials**, click hello **Site URL** tooopen your cloud service in a web browser.</span></span>

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a><span data-ttu-id="3f443-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f443-166">Next steps</span></span>
* <span data-ttu-id="3f443-167">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3f443-167">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="3f443-168">Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3f443-168">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="3f443-169">[Hantera din molntjänst](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3f443-169">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="3f443-170">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3f443-170">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
