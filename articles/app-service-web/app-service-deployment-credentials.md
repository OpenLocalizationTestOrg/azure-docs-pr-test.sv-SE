---
title: "aaaAzure autentiseringsuppgifter för distribution av App Service | Microsoft Docs"
description: "Lär dig hur toouse hello Azure App Service-autentiseringsuppgifter för distribution."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: d6f9f5cc1b62a17c42643266f4c9490f827c63f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a><span data-ttu-id="bd098-103">Konfigurera autentiseringsuppgifter för distribution för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bd098-103">Configure deployment credentials for Azure App Service</span></span>
<span data-ttu-id="bd098-104">[Azure Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) stöder två typer av autentiseringsuppgifter för [lokal Git-distribution](app-service-deploy-local-git.md) och [FTP/S distribution](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="bd098-104">[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) supports two types of credentials for [local Git deployment](app-service-deploy-local-git.md) and [FTP/S deployment](app-service-deploy-ftp.md).</span></span> <span data-ttu-id="bd098-105">Dessa är inte hello samma som din Azure Active Directory-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bd098-105">These are not hello same as your Azure Active Directory credentials.</span></span>

* <span data-ttu-id="bd098-106">**Användarnivå autentiseringsuppgifter**: en uppsättning autentiseringsuppgifter för hello hela Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bd098-106">**User-level credentials**: one set of credentials for hello entire Azure account.</span></span> <span data-ttu-id="bd098-107">Det kan vara används toodeploy tooApp Service för en app i en prenumeration som hello Azure-konto har behörigheten tooaccess.</span><span class="sxs-lookup"><span data-stu-id="bd098-107">It can be used toodeploy tooApp Service for any app, in any subscription, that hello Azure account has permission tooaccess.</span></span> <span data-ttu-id="bd098-108">Dessa är hello autentiseringsuppgifter standarduppsättning som du konfigurerar i **Apptjänster** > **&lt;appnamn >** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="bd098-108">These are hello default credentials set that you configure in **App Services** > **&lt;app_name>** > **Deployment credentials**.</span></span> <span data-ttu-id="bd098-109">Detta är också hello standarduppsättning som är anslutna i hello portal GUI (till exempel hello **översikt** och **egenskaper** för din app [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span><span class="sxs-lookup"><span data-stu-id="bd098-109">This is also hello default set that's surfaced in hello portal GUI (such as hello **Overview** and **Properties** of your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd098-110">När du delegerar åtkomst tooAzure resurser via rollbaserad åtkomstkontroll (RBAC) eller medadministratör behörigheter använda varje Azure användare som får åtkomst tooan app hans/hennes personliga användarnivå autentiseringsuppgifter förrän åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="bd098-110">When you delegate access tooAzure resources via Role Based Access Control (RBAC) or co-admin permissions, each Azure user that receives access tooan app can use his/her personal user-level credentials until access is revoked.</span></span> <span data-ttu-id="bd098-111">Dessa autentiseringsuppgifter för distribution bör inte delas med andra Azure-användare.</span><span class="sxs-lookup"><span data-stu-id="bd098-111">These deployment credentials should not be shared with other Azure users.</span></span>
    >
    >

* <span data-ttu-id="bd098-112">**App-nivå autentiseringsuppgifter**: en uppsättning autentiseringsuppgifter för varje app.</span><span class="sxs-lookup"><span data-stu-id="bd098-112">**App-level credentials**: one set of credentials for each app.</span></span> <span data-ttu-id="bd098-113">Det kan vara används toodeploy toothat app.</span><span class="sxs-lookup"><span data-stu-id="bd098-113">It can be used toodeploy toothat app only.</span></span> <span data-ttu-id="bd098-114">hello autentiseringsuppgifter publiceringsprofil för varje app ska genereras automatiskt vid appen skapas och hittas i hello app.</span><span class="sxs-lookup"><span data-stu-id="bd098-114">hello credentials for each app is generated automatically at app creation, and is found in hello app's publish profile.</span></span> <span data-ttu-id="bd098-115">Du kan inte manuellt konfigurera hello autentiseringsuppgifter, men kan du återställa dem till en app när som helst.</span><span class="sxs-lookup"><span data-stu-id="bd098-115">You cannot manually configure hello credentials, but you can reset them for an app anytime.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd098-116">I ordning toogive någon åtkomst till toothese autentiseringsuppgifter via rollbaserad åtkomstkontroll (RBAC), måste du toomake dem deltagare eller högre på hello Web App.</span><span class="sxs-lookup"><span data-stu-id="bd098-116">In order toogive someone access toothese credentials via Role Based Access Control (RBAC), you need toomake them contributor or higher on hello Web App.</span></span> <span data-ttu-id="bd098-117">Läsare tillåts inte toopublish och därför kan inte komma åt autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="bd098-117">Readers are not allowed toopublish, and hence can't access those credentials.</span></span>
    >
    >

## <span data-ttu-id="bd098-118"><a name="userscope"></a>Ange och återställa användarnivå autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="bd098-118"><a name="userscope"></a>Set and reset user-level credentials</span></span>

<span data-ttu-id="bd098-119">Du kan konfigurera autentiseringsuppgifterna användarnivå i alla appar [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="bd098-119">You can configure your user-level credentials in any app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span> <span data-ttu-id="bd098-120">Oavsett vilken app du konfigurera dessa autentiseringsuppgifter, det gäller tooall appar och för alla prenumerationer i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bd098-120">Regardless in which app you configure these credentials, it applies tooall apps and for all subscriptions in your Azure account.</span></span> 

<span data-ttu-id="bd098-121">tooconfigure inloggningsinformation användarnivå:</span><span class="sxs-lookup"><span data-stu-id="bd098-121">tooconfigure your user-level credentials:</span></span>

1. <span data-ttu-id="bd098-122">I hello [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="bd098-122">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Deployment credentials**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bd098-123">Du måste ha minst en app innan du kan komma åt hello distributionsblad autentiseringsuppgifter i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bd098-123">In hello portal, you must have at least one app before you can access hello deployment credentials blade.</span></span> <span data-ttu-id="bd098-124">Men med hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), kan du konfigurera användarnivå autentiseringsuppgifter utan en befintlig app.</span><span class="sxs-lookup"><span data-stu-id="bd098-124">However, with hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), you can configure user-level credentials without an existing app.</span></span>

2. <span data-ttu-id="bd098-125">Konfigurera hello användarnamn och lösenord och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bd098-125">Configure hello user name and password, and then click **Save**.</span></span>

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

<span data-ttu-id="bd098-126">När du har angett dina autentiseringsuppgifter för distribution, kan du hitta hello *Git* användarnamn för distribution i din app **översikt**,</span><span class="sxs-lookup"><span data-stu-id="bd098-126">Once you have set your deployment credentials, you can find hello *Git* deployment username in your app's **Overview**,</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

<span data-ttu-id="bd098-127">och och *FTP* användarnamn för distribution i din app **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bd098-127">and and *FTP* deployment username in your app's **Properties**.</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> <span data-ttu-id="bd098-128">Azure visar inte lösenordet på användarnivå distribution.</span><span class="sxs-lookup"><span data-stu-id="bd098-128">Azure does not show your user-level deployment password.</span></span> <span data-ttu-id="bd098-129">Det går inte att hämta om du glömmer hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="bd098-129">If you forget hello password, you can't retrieve it.</span></span> <span data-ttu-id="bd098-130">Du kan dock återställa dina autentiseringsuppgifter genom att följa hello stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bd098-130">However, you can reset your credentials by following hello steps in this section.</span></span>
>
>  

## <span data-ttu-id="bd098-131"><a name="appscope"></a>Hämta och Återställ autentiseringsuppgifterna för app-nivå</span><span class="sxs-lookup"><span data-stu-id="bd098-131"><a name="appscope"></a>Get and reset app-level credentials</span></span>
<span data-ttu-id="bd098-132">För varje app i App Service app-nivå autentiseringsuppgifterna lagras i hello XML Publicera profil.</span><span class="sxs-lookup"><span data-stu-id="bd098-132">For each app in App Service, its app-level credentials are stored in hello XML publish profile.</span></span>

<span data-ttu-id="bd098-133">tooget hello app-nivå autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="bd098-133">tooget hello app-level credentials:</span></span>

1. <span data-ttu-id="bd098-134">I hello [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **översikt**.</span><span class="sxs-lookup"><span data-stu-id="bd098-134">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="bd098-135">Klicka på **... Flera** > **Get publiceringsprofil**, och börjar hämtas för en. PublishSettings-filen.</span><span class="sxs-lookup"><span data-stu-id="bd098-135">Click **...More** > **Get publish profile**, and download starts for a .PublishSettings file.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. <span data-ttu-id="bd098-136">Öppna hello. PublishSettings-filen och hitta hello `<publishProfile>` -tagg med hello attributet `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="bd098-136">Open hello .PublishSettings file and find hello `<publishProfile>` tag with hello attribute `publishMethod="FTP"`.</span></span> <span data-ttu-id="bd098-137">Sedan, dess `userName` och `password` attribut.</span><span class="sxs-lookup"><span data-stu-id="bd098-137">Then, get its `userName` and `password` attributes.</span></span>
<span data-ttu-id="bd098-138">Dessa är hello appnivå autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bd098-138">These are hello app-level credentials.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    <span data-ttu-id="bd098-139">Liknande toohello användarnivå autentiseringsuppgifter hello FTP distribution användarnamnet är i hello-format för `<app_name>\<username>`, och användarnamn för hello Git-distribution är bara `<username>` utan föregående hello `<app_name>\`.</span><span class="sxs-lookup"><span data-stu-id="bd098-139">Similar toohello user-level credentials, hello FTP deployment username is in hello format of `<app_name>\<username>`, and hello Git deployment username is just `<username>` without hello preceding `<app_name>\`.</span></span>

<span data-ttu-id="bd098-140">tooreset hello app-nivå autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="bd098-140">tooreset hello app-level credentials:</span></span>

1. <span data-ttu-id="bd098-141">I hello [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **översikt**.</span><span class="sxs-lookup"><span data-stu-id="bd098-141">In hello [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="bd098-142">Klicka på **... Flera** > **Återställ publiceringsprofil**.</span><span class="sxs-lookup"><span data-stu-id="bd098-142">Click **...More** > **Reset publish profile**.</span></span> <span data-ttu-id="bd098-143">Klicka på **Ja** tooconfirm hello återställa.</span><span class="sxs-lookup"><span data-stu-id="bd098-143">Click **Yes** tooconfirm hello reset.</span></span>

    <span data-ttu-id="bd098-144">hello reset-åtgärden upphäver någon tidigare hämtade. PublishSettings-filer.</span><span class="sxs-lookup"><span data-stu-id="bd098-144">hello reset action invalidates any previously-downloaded .PublishSettings files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd098-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd098-145">Next steps</span></span>

<span data-ttu-id="bd098-146">Ta reda på hur toouse dessa autentiseringsuppgifter toodeploy appen från [lokala Git](app-service-deploy-local-git.md) eller med hjälp av [FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="bd098-146">Find out how toouse these credentials toodeploy your app from [local Git](app-service-deploy-local-git.md) or using [FTP/S](app-service-deploy-ftp.md).</span></span>
