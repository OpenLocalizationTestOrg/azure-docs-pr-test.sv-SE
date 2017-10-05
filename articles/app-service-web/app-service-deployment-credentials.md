---
title: "Autentiseringsuppgifter för distribution av Azure App Service | Microsoft Docs"
description: "Lär dig hur du använder autentiseringsuppgifter för Azure App Service-distribution."
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
ms.openlocfilehash: 86a2cd8ae9f97c606a378452e44eec8941700531
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a><span data-ttu-id="bac2d-103">Konfigurera autentiseringsuppgifter för distribution för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bac2d-103">Configure deployment credentials for Azure App Service</span></span>
<span data-ttu-id="bac2d-104">[Azure Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) stöder två typer av autentiseringsuppgifter för [lokal Git-distribution](app-service-deploy-local-git.md) och [FTP/S distribution](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="bac2d-104">[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) supports two types of credentials for [local Git deployment](app-service-deploy-local-git.md) and [FTP/S deployment](app-service-deploy-ftp.md).</span></span> <span data-ttu-id="bac2d-105">Dessa är inte samma som din Azure Active Directory-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bac2d-105">These are not the same as your Azure Active Directory credentials.</span></span>

* <span data-ttu-id="bac2d-106">**Användarnivå autentiseringsuppgifter**: en uppsättning autentiseringsuppgifter för hela Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bac2d-106">**User-level credentials**: one set of credentials for the entire Azure account.</span></span> <span data-ttu-id="bac2d-107">Det kan användas för att distribuera till App Service för en app i en prenumeration som Azure-konto har behörighet att komma åt.</span><span class="sxs-lookup"><span data-stu-id="bac2d-107">It can be used to deploy to App Service for any app, in any subscription, that the Azure account has permission to access.</span></span> <span data-ttu-id="bac2d-108">Det här är en standarduppsättning autentiseringsuppgifter som du konfigurerar i **Apptjänster** > **&lt;appnamn >** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-108">These are the default credentials set that you configure in **App Services** > **&lt;app_name>** > **Deployment credentials**.</span></span> <span data-ttu-id="bac2d-109">Detta är standardvärdet som angetts i GUI-portalen (som den **översikt** och **egenskaper** för din app [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span><span class="sxs-lookup"><span data-stu-id="bac2d-109">This is also the default set that's surfaced in the portal GUI (such as the **Overview** and **Properties** of your app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources)).</span></span>

    > [!NOTE]
    > <span data-ttu-id="bac2d-110">När du delegerar åtkomst till Azure-resurser via rollbaserad åtkomstkontroll (RBAC) eller medadministratör behörigheter kan varje Azure användare som får åtkomst till en app använda hans/hennes personliga användarnivå autentiseringsuppgifter förrän åtkomst har återkallats.</span><span class="sxs-lookup"><span data-stu-id="bac2d-110">When you delegate access to Azure resources via Role Based Access Control (RBAC) or co-admin permissions, each Azure user that receives access to an app can use his/her personal user-level credentials until access is revoked.</span></span> <span data-ttu-id="bac2d-111">Dessa autentiseringsuppgifter för distribution bör inte delas med andra Azure-användare.</span><span class="sxs-lookup"><span data-stu-id="bac2d-111">These deployment credentials should not be shared with other Azure users.</span></span>
    >
    >

* <span data-ttu-id="bac2d-112">**App-nivå autentiseringsuppgifter**: en uppsättning autentiseringsuppgifter för varje app.</span><span class="sxs-lookup"><span data-stu-id="bac2d-112">**App-level credentials**: one set of credentials for each app.</span></span> <span data-ttu-id="bac2d-113">Den kan användas för att distribuera till appen endast.</span><span class="sxs-lookup"><span data-stu-id="bac2d-113">It can be used to deploy to that app only.</span></span> <span data-ttu-id="bac2d-114">Autentiseringsuppgifterna publiceringsprofil för varje app ska genereras automatiskt vid appen skapas och finns i appens.</span><span class="sxs-lookup"><span data-stu-id="bac2d-114">The credentials for each app is generated automatically at app creation, and is found in the app's publish profile.</span></span> <span data-ttu-id="bac2d-115">Du kan inte manuellt konfigurera autentiseringsuppgifterna, men kan du återställa dem till en app när som helst.</span><span class="sxs-lookup"><span data-stu-id="bac2d-115">You cannot manually configure the credentials, but you can reset them for an app anytime.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bac2d-116">För att ge någon åtkomst till dessa autentiseringsuppgifter via rollbaserad åtkomstkontroll (RBAC), måste du se dem deltagare eller högre kontinuerligt i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bac2d-116">In order to give someone access to these credentials via Role Based Access Control (RBAC), you need to make them contributor or higher on the Web App.</span></span> <span data-ttu-id="bac2d-117">Läsare tillåts inte att publicera, och därför inte åtkomst till dessa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bac2d-117">Readers are not allowed to publish, and hence can't access those credentials.</span></span>
    >
    >

## <span data-ttu-id="bac2d-118"><a name="userscope"></a>Ange och återställa användarnivå autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="bac2d-118"><a name="userscope"></a>Set and reset user-level credentials</span></span>

<span data-ttu-id="bac2d-119">Du kan konfigurera autentiseringsuppgifterna användarnivå i alla appar [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources).</span><span class="sxs-lookup"><span data-stu-id="bac2d-119">You can configure your user-level credentials in any app's [resource blade](../azure-resource-manager/resource-group-portal.md#manage-resources).</span></span> <span data-ttu-id="bac2d-120">Oavsett vilken app du konfigurera dessa autentiseringsuppgifter, gäller det för alla appar och för alla prenumerationer i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bac2d-120">Regardless in which app you configure these credentials, it applies to all apps and for all subscriptions in your Azure account.</span></span> 

<span data-ttu-id="bac2d-121">Konfigurera autentiseringsuppgifterna användarnivå:</span><span class="sxs-lookup"><span data-stu-id="bac2d-121">To configure your user-level credentials:</span></span>

1. <span data-ttu-id="bac2d-122">I den [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-122">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Deployment credentials**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bac2d-123">Du måste ha minst en app innan du kan komma åt bladet distribution av autentiseringsuppgifter i portalen.</span><span class="sxs-lookup"><span data-stu-id="bac2d-123">In the portal, you must have at least one app before you can access the deployment credentials blade.</span></span> <span data-ttu-id="bac2d-124">Med den [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), kan du konfigurera användarnivå autentiseringsuppgifter utan en befintlig app.</span><span class="sxs-lookup"><span data-stu-id="bac2d-124">However, with the [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), you can configure user-level credentials without an existing app.</span></span>

2. <span data-ttu-id="bac2d-125">Konfigurera användarnamn och lösenord och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-125">Configure the user name and password, and then click **Save**.</span></span>

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

<span data-ttu-id="bac2d-126">När du har angett dina autentiseringsuppgifter för distribution, du kan hitta den *Git* användarnamn för distribution i din app **översikt**,</span><span class="sxs-lookup"><span data-stu-id="bac2d-126">Once you have set your deployment credentials, you can find the *Git* deployment username in your app's **Overview**,</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

<span data-ttu-id="bac2d-127">och och *FTP* användarnamn för distribution i din app **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-127">and and *FTP* deployment username in your app's **Properties**.</span></span>

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> <span data-ttu-id="bac2d-128">Azure visar inte lösenordet på användarnivå distribution.</span><span class="sxs-lookup"><span data-stu-id="bac2d-128">Azure does not show your user-level deployment password.</span></span> <span data-ttu-id="bac2d-129">Om du glömmer bort lösenordet kan du hämta den.</span><span class="sxs-lookup"><span data-stu-id="bac2d-129">If you forget the password, you can't retrieve it.</span></span> <span data-ttu-id="bac2d-130">Du kan dock återställa dina autentiseringsuppgifter genom att följa stegen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bac2d-130">However, you can reset your credentials by following the steps in this section.</span></span>
>
>  

## <span data-ttu-id="bac2d-131"><a name="appscope"></a>Hämta och Återställ autentiseringsuppgifterna för app-nivå</span><span class="sxs-lookup"><span data-stu-id="bac2d-131"><a name="appscope"></a>Get and reset app-level credentials</span></span>
<span data-ttu-id="bac2d-132">För varje app i App Service app-nivå autentiseringsuppgifterna lagras i XML-Publicera profil.</span><span class="sxs-lookup"><span data-stu-id="bac2d-132">For each app in App Service, its app-level credentials are stored in the XML publish profile.</span></span>

<span data-ttu-id="bac2d-133">Hämta autentiseringsuppgifter för app-nivå:</span><span class="sxs-lookup"><span data-stu-id="bac2d-133">To get the app-level credentials:</span></span>

1. <span data-ttu-id="bac2d-134">I den [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **översikt**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-134">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="bac2d-135">Klicka på **... Flera** > **Get publiceringsprofil**, och börjar hämtas för en. PublishSettings-filen.</span><span class="sxs-lookup"><span data-stu-id="bac2d-135">Click **...More** > **Get publish profile**, and download starts for a .PublishSettings file.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. <span data-ttu-id="bac2d-136">Öppna den. PublishSettings-filen och hitta det `<publishProfile>` tagg med attributet `publishMethod="FTP"`.</span><span class="sxs-lookup"><span data-stu-id="bac2d-136">Open the .PublishSettings file and find the `<publishProfile>` tag with the attribute `publishMethod="FTP"`.</span></span> <span data-ttu-id="bac2d-137">Sedan, dess `userName` och `password` attribut.</span><span class="sxs-lookup"><span data-stu-id="bac2d-137">Then, get its `userName` and `password` attributes.</span></span>
<span data-ttu-id="bac2d-138">Dessa är autentiseringsuppgifterna som app-nivå.</span><span class="sxs-lookup"><span data-stu-id="bac2d-138">These are the app-level credentials.</span></span>

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    <span data-ttu-id="bac2d-139">Liksom för användarnivå autentiseringsuppgifter, användarnamn för FTP-distribution finns i formatet `<app_name>\<username>`, och användarnamn för Git-distribution är bara `<username>` utan föregående `<app_name>\`.</span><span class="sxs-lookup"><span data-stu-id="bac2d-139">Similar to the user-level credentials, the FTP deployment username is in the format of `<app_name>\<username>`, and the Git deployment username is just `<username>` without the preceding `<app_name>\`.</span></span>

<span data-ttu-id="bac2d-140">Så här återställer du app-nivå autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="bac2d-140">To reset the app-level credentials:</span></span>

1. <span data-ttu-id="bac2d-141">I den [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **översikt**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-141">In the [Azure portal](https://portal.azure.com), click App Service > **&lt;any_app>** > **Overview**.</span></span>

2. <span data-ttu-id="bac2d-142">Klicka på **... Flera** > **Återställ publiceringsprofil**.</span><span class="sxs-lookup"><span data-stu-id="bac2d-142">Click **...More** > **Reset publish profile**.</span></span> <span data-ttu-id="bac2d-143">Klicka på **Ja** bekräfta återställningen.</span><span class="sxs-lookup"><span data-stu-id="bac2d-143">Click **Yes** to confirm the reset.</span></span>

    <span data-ttu-id="bac2d-144">Reset-åtgärden upphäver någon tidigare hämtade. PublishSettings-filer.</span><span class="sxs-lookup"><span data-stu-id="bac2d-144">The reset action invalidates any previously-downloaded .PublishSettings files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bac2d-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bac2d-145">Next steps</span></span>

<span data-ttu-id="bac2d-146">Ta reda på hur du använder dessa autentiseringsuppgifter för att distribuera din app från [lokala Git](app-service-deploy-local-git.md) eller med hjälp av [FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="bac2d-146">Find out how to use these credentials to deploy your app from [local Git](app-service-deploy-local-git.md) or using [FTP/S](app-service-deploy-ftp.md).</span></span>
