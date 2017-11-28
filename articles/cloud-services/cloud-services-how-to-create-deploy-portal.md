---
title: "Hur du skapar och distribuerar en tjänst i molnet | Microsoft Docs"
description: "Lär dig hur du skapar och distribuerar en tjänst i molnet med Azure-portalen."
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
ms.openlocfilehash: e5ce666f1d826c7901c9fd5e7fafe6171139c3ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a><span data-ttu-id="1340e-103">Hur du skapar och distribuerar en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="1340e-103">How to create and deploy a cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1340e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1340e-104">Azure portal</span></span>](cloud-services-how-to-create-deploy-portal.md)
> * [<span data-ttu-id="1340e-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="1340e-105">Azure classic portal</span></span>](cloud-services-how-to-create-deploy.md)
>
>

<span data-ttu-id="1340e-106">Azure portal tillhandahåller två sätt att skapa och distribuera en tjänst i molnet: *Snabbregistrering* och *skapa anpassade*.</span><span class="sxs-lookup"><span data-stu-id="1340e-106">The Azure portal provides two ways for you to create and deploy a cloud service: *Quick Create* and *Custom Create*.</span></span>

<span data-ttu-id="1340e-107">Den här artikeln förklarar hur du använder metoden Snabbregistrering för att skapa en ny molntjänst och sedan använda **överför** så här överför och distribuera ett paket med cloud service i Azure.</span><span class="sxs-lookup"><span data-stu-id="1340e-107">This article explains how to use the Quick Create method to create a new cloud service and then use **Upload** to upload and deploy a cloud service package in Azure.</span></span> <span data-ttu-id="1340e-108">När du använder den här metoden gör tillgängliga praktiska länkar för att slutföra alla krav när du går i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1340e-108">When you use this method, the Azure portal makes available convenient links for completing all requirements as you go.</span></span> <span data-ttu-id="1340e-109">Om du är redo att distribuera din molntjänst när du skapar den, kan du göra båda på samma gång med att skapa anpassade.</span><span class="sxs-lookup"><span data-stu-id="1340e-109">If you're ready to deploy your cloud service when you create it, you can do both at the same time using Custom Create.</span></span>

> [!NOTE]
> <span data-ttu-id="1340e-110">Om du planerar att publicera din molntjänst från Visual Studio Team Services VSTS () använda Snabbregistrering och sedan konfigurera VSTS publicering från Azure Quickstart eller instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1340e-110">If you plan to publish your cloud service from Visual Studio Team Services (VSTS), use Quick Create, and then set up VSTS publishing from the Azure Quickstart or the dashboard.</span></span> <span data-ttu-id="1340e-111">Mer information finns i [kontinuerlig leverans till Azure med hjälp av Visual Studio Team Services][TFSTutorialForCloudService], eller se hjälpinformation om den **Snabbstart** sidan.</span><span class="sxs-lookup"><span data-stu-id="1340e-111">For more information, see [Continuous Delivery to Azure by Using Visual Studio Team Services][TFSTutorialForCloudService], or see help for the **Quick Start** page.</span></span>
>
>

## <a name="concepts"></a><span data-ttu-id="1340e-112">Koncept</span><span class="sxs-lookup"><span data-stu-id="1340e-112">Concepts</span></span>
<span data-ttu-id="1340e-113">Tre komponenter krävs för att distribuera ett program som en tjänst i molnet i Azure:</span><span class="sxs-lookup"><span data-stu-id="1340e-113">Three components are required to deploy an application as a cloud service in Azure:</span></span>

* <span data-ttu-id="1340e-114">**Tjänstdefinitionen**</span><span class="sxs-lookup"><span data-stu-id="1340e-114">**Service Definition**</span></span>  
  <span data-ttu-id="1340e-115">Molnet tjänstdefinitionsfilen (.csdef) definierar tjänstmodellen, inklusive antalet roller.</span><span class="sxs-lookup"><span data-stu-id="1340e-115">The cloud service definition file (.csdef) defines the service model, including the number of roles.</span></span>
* <span data-ttu-id="1340e-116">**Tjänstkonfiguration**</span><span class="sxs-lookup"><span data-stu-id="1340e-116">**Service Configuration**</span></span>  
  <span data-ttu-id="1340e-117">Molnet tjänstekonfigurationsfilen (.cscfg) innehåller konfigurationsinställningarna för molnet tjänsten och enskilda roller, inklusive antalet rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="1340e-117">The cloud service configuration file (.cscfg) provides configuration settings for the cloud service and individual roles, including the number of role instances.</span></span>
* <span data-ttu-id="1340e-118">**Tjänstpaket**</span><span class="sxs-lookup"><span data-stu-id="1340e-118">**Service Package**</span></span>  
  <span data-ttu-id="1340e-119">Service-paketet (.cspkg) innehåller programkoden och konfigurationer och tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="1340e-119">The service package (.cspkg) contains the application code and configurations and the service definition file.</span></span>

<span data-ttu-id="1340e-120">Du kan lära dig mer om de här och hur du skapar ett paket [här](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="1340e-120">You can learn more about these and how to create a package [here](cloud-services-model-and-package.md).</span></span>

## <a name="prepare-your-app"></a><span data-ttu-id="1340e-121">Förbereda din app</span><span class="sxs-lookup"><span data-stu-id="1340e-121">Prepare your app</span></span>
<span data-ttu-id="1340e-122">Innan du kan distribuera en tjänst i molnet, måste du skapa cloud service-paketet (.cspkg) från din programkod och ett moln tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="1340e-122">Before you can deploy a cloud service, you must create the cloud service package (.cspkg) from your application code and a cloud service configuration file (.cscfg).</span></span> <span data-ttu-id="1340e-123">Azure SDK innehåller verktyg för att förbereda filerna obligatorisk distribution.</span><span class="sxs-lookup"><span data-stu-id="1340e-123">The Azure SDK provides tools for preparing these required deployment files.</span></span> <span data-ttu-id="1340e-124">Du kan installera SDK från den [Azure hämtar](https://azure.microsoft.com/downloads/) sidan på det språk som du föredrar att utveckla programkoden.</span><span class="sxs-lookup"><span data-stu-id="1340e-124">You can install the SDK from the [Azure Downloads](https://azure.microsoft.com/downloads/) page, in the language in which you prefer to develop your application code.</span></span>

<span data-ttu-id="1340e-125">Tre kräver cloud tjänsten särskilda konfigurationer innan du exporterar en tjänstpaket:</span><span class="sxs-lookup"><span data-stu-id="1340e-125">Three cloud service features require special configurations before you export a service package:</span></span>

* <span data-ttu-id="1340e-126">Om du vill distribuera en tjänst i molnet som använder Secure Sockets Layer (SSL) för kryptering av data, [konfigurera ditt program](cloud-services-configure-ssl-certificate-portal.md#modify) för SSL.</span><span class="sxs-lookup"><span data-stu-id="1340e-126">If you want to deploy a cloud service that uses Secure Sockets Layer (SSL) for data encryption, [configure your application](cloud-services-configure-ssl-certificate-portal.md#modify) for SSL.</span></span>
* <span data-ttu-id="1340e-127">Om du vill konfigurera anslutningar till fjärrskrivbord till rollinstanser, [konfigurera rollerna](cloud-services-role-enable-remote-desktop-new-portal.md) för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="1340e-127">If you want to configure Remote Desktop connections to role instances, [configure the roles](cloud-services-role-enable-remote-desktop-new-portal.md) for Remote Desktop.</span></span>
* <span data-ttu-id="1340e-128">Om du vill konfigurera detaljerad övervakning av Molntjänsten, aktiverar du Azure-diagnostik för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1340e-128">If you want to configure verbose monitoring for your cloud service, enable Azure Diagnostics for the cloud service.</span></span> <span data-ttu-id="1340e-129">*Minimal övervakning* (standardinställningar för övervakning nivå) använder prestandaräknare som samlats in från värdoperativsystem för rollinstanser (virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="1340e-129">*Minimal monitoring* (the default monitoring level) uses performance counters gathered from the host operating systems for role instances (virtual machines).</span></span> <span data-ttu-id="1340e-130">*Utförlig övervakning* samlar in ytterligare mått baserat på prestandadata i rollinstanser för att aktivera närmare analys av problem som uppstår under bearbetningen av programmet.</span><span class="sxs-lookup"><span data-stu-id="1340e-130">*Verbose monitoring* gathers additional metrics based on performance data within the role instances to enable closer analysis of issues that occur during application processing.</span></span> <span data-ttu-id="1340e-131">Om du vill veta hur du aktiverar Azure Diagnostics [aktiverar diagnostik i Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="1340e-131">To find out how to enable Azure Diagnostics, see [Enabling diagnostics in Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="1340e-132">Om du vill skapa en molntjänst med distributioner av webbroller eller arbetsroller, måste du [skapa tjänstepaketet](cloud-services-model-and-package.md#servicepackagecspkg).</span><span class="sxs-lookup"><span data-stu-id="1340e-132">To create a cloud service with deployments of web roles or worker roles, you must [create the service package](cloud-services-model-and-package.md#servicepackagecspkg).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1340e-133">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1340e-133">Before you begin</span></span>
* <span data-ttu-id="1340e-134">Om du inte har installerat Azure SDK, klickar du på **installera Azure SDK** att öppna den [Azure hämtar sidan](https://azure.microsoft.com/downloads/), och sedan hämta SDK för det språk som du föredrar att utveckla din kod.</span><span class="sxs-lookup"><span data-stu-id="1340e-134">If you haven't installed the Azure SDK, click **Install Azure SDK** to open the [Azure Downloads page](https://azure.microsoft.com/downloads/), and then download the SDK for the language in which you prefer to develop your code.</span></span> <span data-ttu-id="1340e-135">(Har du möjlighet att göra det senare.)</span><span class="sxs-lookup"><span data-stu-id="1340e-135">(You'll have an opportunity to do this later.)</span></span>
* <span data-ttu-id="1340e-136">Om alla rollinstanser kräver ett certifikat, skapa certifikat.</span><span class="sxs-lookup"><span data-stu-id="1340e-136">If any role instances require a certificate, create the certificates.</span></span> <span data-ttu-id="1340e-137">Molntjänster kräver en .pfx-fil med en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="1340e-137">Cloud services require a .pfx file with a private key.</span></span> <span data-ttu-id="1340e-138">Du kan ladda upp certifikat till Azure när du skapar och distribuerar Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1340e-138">You can upload the certificates to Azure as you create and deploy the cloud service.</span></span>

## <a name="create-and-deploy"></a><span data-ttu-id="1340e-139">Skapa och distribuera</span><span class="sxs-lookup"><span data-stu-id="1340e-139">Create and deploy</span></span>
1. <span data-ttu-id="1340e-140">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1340e-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1340e-141">Klicka på **New > Compute**, och bläddra till och klickar på **Molntjänsten**.</span><span class="sxs-lookup"><span data-stu-id="1340e-141">Click **New > Compute**, and then scroll down to and click **Cloud Service**.</span></span>

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. <span data-ttu-id="1340e-143">I den nya **Molntjänsten** bladet, ange ett värde för den **DNS-namnet**.</span><span class="sxs-lookup"><span data-stu-id="1340e-143">In the new **Cloud Service** blade, enter a value for the **DNS name**.</span></span>
4. <span data-ttu-id="1340e-144">Skapa en ny **resursgruppen** eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="1340e-144">Create a new **Resource Group** or select an existing one.</span></span>
5. <span data-ttu-id="1340e-145">Välj en **Plats**.</span><span class="sxs-lookup"><span data-stu-id="1340e-145">Select a **Location**.</span></span>
6. <span data-ttu-id="1340e-146">Klicka på **paketet**.</span><span class="sxs-lookup"><span data-stu-id="1340e-146">Click **Package**.</span></span> <span data-ttu-id="1340e-147">Då öppnas den **överföra ett paket** bladet.</span><span class="sxs-lookup"><span data-stu-id="1340e-147">This will open the **Upload a package** blade.</span></span> <span data-ttu-id="1340e-148">Fyll i de obligatoriska fälten.</span><span class="sxs-lookup"><span data-stu-id="1340e-148">Fill in the required fields.</span></span> <span data-ttu-id="1340e-149">Om någon av dina roller innehåller en enda instans, kontrollerar du **distribuera även om en eller flera roller innehåller en enda instans** är markerad.</span><span class="sxs-lookup"><span data-stu-id="1340e-149">If any of your roles contain a single instance, ensure **Deploy even if one or more roles contain a single instance** is selected.</span></span>
7. <span data-ttu-id="1340e-150">Se till att **starta distribution** är markerad.</span><span class="sxs-lookup"><span data-stu-id="1340e-150">Make sure that **Start deployment** is selected.</span></span>
8. <span data-ttu-id="1340e-151">Klicka på **OK** som stänger den **överföra ett paket** bladet.</span><span class="sxs-lookup"><span data-stu-id="1340e-151">Click **OK** which will close the **Upload a package** blade.</span></span>
9. <span data-ttu-id="1340e-152">Om du inte har några certifikat att lägga till klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1340e-152">If you do not have any certificates to add, click **Create**.</span></span>

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a><span data-ttu-id="1340e-154">Överföra ett certifikat</span><span class="sxs-lookup"><span data-stu-id="1340e-154">Upload a certificate</span></span>
<span data-ttu-id="1340e-155">Om din distributionspaketet [konfigurerats för att använda certifikat](cloud-services-configure-ssl-certificate-portal.md#modify), du kan ladda upp certifikatet nu.</span><span class="sxs-lookup"><span data-stu-id="1340e-155">If your deployment package was [configured to use certificates](cloud-services-configure-ssl-certificate-portal.md#modify), you can upload the certificate now.</span></span>

1. <span data-ttu-id="1340e-156">Välj **certifikat**, och på den **lägga till certifikat** bladet välj SSL-certifikat PFX-fil och ange sedan den **lösenord** för certifikatet</span><span class="sxs-lookup"><span data-stu-id="1340e-156">Select **Certificates**, and on the **Add certificates** blade, select the SSL certificate .pfx file, and then provide the **Password** for the certificate,</span></span>
2. <span data-ttu-id="1340e-157">Klicka på **bifoga certifikat**, och klicka sedan på **OK** på den **lägga till certifikat** bladet.</span><span class="sxs-lookup"><span data-stu-id="1340e-157">Click **Attach certificate**, and then click **OK** on the **Add certificates** blade.</span></span>
3. <span data-ttu-id="1340e-158">Klicka på **skapa** på den **Molntjänsten** bladet.</span><span class="sxs-lookup"><span data-stu-id="1340e-158">Click **Create** on the **Cloud Service** blade.</span></span> <span data-ttu-id="1340e-159">När distributionen har uppnått den **klar** status, kan du fortsätta till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="1340e-159">When the deployment has reached the **Ready** status, you can proceed to the next steps.</span></span>

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a><span data-ttu-id="1340e-161">Kontrollera distributionen har slutförts</span><span class="sxs-lookup"><span data-stu-id="1340e-161">Verify your deployment completed successfully</span></span>
1. <span data-ttu-id="1340e-162">Klicka på cloud service-instans.</span><span class="sxs-lookup"><span data-stu-id="1340e-162">Click the cloud service instance.</span></span>

    <span data-ttu-id="1340e-163">Status ska visa att tjänsten är **kör**.</span><span class="sxs-lookup"><span data-stu-id="1340e-163">The status should show that the service is **Running**.</span></span>
2. <span data-ttu-id="1340e-164">Under **Essentials**, klicka på den **Webbadress** att öppna din molntjänst i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1340e-164">Under **Essentials**, click the **Site URL** to open your cloud service in a web browser.</span></span>

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a><span data-ttu-id="1340e-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1340e-166">Next steps</span></span>
* <span data-ttu-id="1340e-167">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1340e-167">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="1340e-168">Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1340e-168">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="1340e-169">[Hantera din molntjänst](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1340e-169">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="1340e-170">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1340e-170">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
