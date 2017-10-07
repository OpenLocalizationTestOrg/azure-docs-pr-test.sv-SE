---
title: aaaCreate en Azure line-of-business-app med Azure Active Directory-autentisering | Microsoft Docs
description: "Lär dig hur toocreate en ASP.NET MVC line-of-business-app i Azure App Service som autentiserar med Azure Active Directory"
services: app-service\web, active-directory
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: ad947bdb-4463-43ff-a5e3-91d9b2169b60
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 09/01/2016
ms.author: cephalin
ms.openlocfilehash: 3bcafad78ac0151889b3e336784cc561009f244f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-azure-active-directory-authentication"></a><span data-ttu-id="0e337-103">Skapa en Azure line-of-business-app med Azure Active Directory-autentisering</span><span class="sxs-lookup"><span data-stu-id="0e337-103">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>
<span data-ttu-id="0e337-104">Den här artikeln beskrivs hur du toocreate en .NET line-of-business-app i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) med hello [autentisering / auktorisering](../app-service/app-service-authentication-overview.md) funktion.</span><span class="sxs-lookup"><span data-stu-id="0e337-104">This article shows you how toocreate a .NET line-of-business app in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) using hello [Authentication / Authorization](../app-service/app-service-authentication-overview.md) feature.</span></span> <span data-ttu-id="0e337-105">Den visar även hur toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery katalogdata i hello program.</span><span class="sxs-lookup"><span data-stu-id="0e337-105">It also shows how toouse hello [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) tooquery directory data in hello application.</span></span>

<span data-ttu-id="0e337-106">hello Azure Active Directory-klient som du använder kan vara en endast Azure-katalog.</span><span class="sxs-lookup"><span data-stu-id="0e337-106">hello Azure Active Directory tenant that you use can be an Azure-only directory.</span></span> <span data-ttu-id="0e337-107">Eller så kan det vara [synkroniserats med din lokala Active Directory](../active-directory/active-directory-aadconnect.md) toocreate en enkel inloggning för anställda som är lokala och fjärranslutna.</span><span class="sxs-lookup"><span data-stu-id="0e337-107">Or, it can be [synced with your on-premises Active Directory](../active-directory/active-directory-aadconnect.md) toocreate a single sign-on experience for workers that are on-premises and remote.</span></span> <span data-ttu-id="0e337-108">Den här artikeln använder hello standardkatalog för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0e337-108">This article uses hello default directory for your Azure account.</span></span>

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="0e337-109">Vad du ska skapa</span><span class="sxs-lookup"><span data-stu-id="0e337-109">What you will build</span></span>
<span data-ttu-id="0e337-110">Du skapar ett enkelt line-of-business skapa-Läs-Uppdatera-ta bort CRUD-program i App Service Web Apps att spårar arbetsobjekt med hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="0e337-110">You will build a simple line-of-business Create-Read-Update-Delete (CRUD) application in App Service Web Apps that tracks work items with hello following features:</span></span>

* <span data-ttu-id="0e337-111">Autentiserar användare mot Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e337-111">Authenticates users against Azure Active Directory</span></span>
* <span data-ttu-id="0e337-112">Frågar directory-användare och grupper med [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span><span class="sxs-lookup"><span data-stu-id="0e337-112">Queries directory users and groups using [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)</span></span>
* <span data-ttu-id="0e337-113">Använd hello ASP.NET MVC *ingen autentisering* mall</span><span class="sxs-lookup"><span data-stu-id="0e337-113">Use hello ASP.NET MVC *No Authentication* template</span></span>

<span data-ttu-id="0e337-114">Om du behöver rollbaserad åtkomstkontroll (RBAC) för din line-of-business-app i Azure finns [nästa steg](#next).</span><span class="sxs-lookup"><span data-stu-id="0e337-114">If you need role-based access control (RBAC) for your line-of-business app in Azure, see [Next Step](#next).</span></span>

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="0e337-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="0e337-115">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="0e337-116">Den här kursen behöver hello följande toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0e337-116">You need hello following toocomplete this tutorial:</span></span>

* <span data-ttu-id="0e337-117">En Azure Active Directory-klient med användare i olika grupper</span><span class="sxs-lookup"><span data-stu-id="0e337-117">An Azure Active Directory tenant with users in various groups</span></span>
* <span data-ttu-id="0e337-118">Behörigheter toocreate program på hello Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="0e337-118">Permissions toocreate applications on hello Azure Active Directory tenant</span></span>
* <span data-ttu-id="0e337-119">Visual Studio 2013 Update 4 eller senare</span><span class="sxs-lookup"><span data-stu-id="0e337-119">Visual Studio 2013 Update 4 or later</span></span>
* [<span data-ttu-id="0e337-120">Azure SDK 2.8.1 eller senare</span><span class="sxs-lookup"><span data-stu-id="0e337-120">Azure SDK 2.8.1 or later</span></span>](https://azure.microsoft.com/downloads/)

<a name="bkmk_deploy"></a>

## <a name="create-and-deploy-a-web-app-tooazure"></a><span data-ttu-id="0e337-121">Skapa och distribuera en web app tooAzure</span><span class="sxs-lookup"><span data-stu-id="0e337-121">Create and deploy a web app tooAzure</span></span>
1. <span data-ttu-id="0e337-122">I Visual Studio klickar du på **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="0e337-122">From Visual Studio, click **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="0e337-123">Välj **ASP.NET-webbprogram**, namnge projektet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e337-123">Select **ASP.NET Web Application**, name your project, and click **OK**.</span></span>
3. <span data-ttu-id="0e337-124">Välj hello **MVC** mall, ändra hello autentisering också**ingen autentisering**.</span><span class="sxs-lookup"><span data-stu-id="0e337-124">Select hello **MVC** template, then change hello authentication too**No Authentication**.</span></span> <span data-ttu-id="0e337-125">Kontrollera att **värd i hello molnet** är markerad och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e337-125">Make sure **Host in hello Cloud** is selected and click **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/1-create-mvc-no-authentication.png)
4. <span data-ttu-id="0e337-126">I hello **skapa Apptjänst** dialogrutan klickar du på **Lägg till ett konto** (och sedan **Lägg till ett konto** i listrutan hello) toolog i tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0e337-126">In hello **Create App Service** dialog, click **Add an account** (and then **Add an account** in hello dropdown) toolog in tooyour Azure account.</span></span>
5. <span data-ttu-id="0e337-127">När du loggade in konfigurera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0e337-127">Once logged in configure your web app.</span></span> <span data-ttu-id="0e337-128">Skapa en resursgrupp och en ny programtjänstplan genom att klicka på respektive hello **ny** knappen.</span><span class="sxs-lookup"><span data-stu-id="0e337-128">Create a resource group and a new App Service plan by clicking hello respective **New** button.</span></span> <span data-ttu-id="0e337-129">Klicka på **utforska ytterligare Azure-tjänster** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="0e337-129">Click **Explore additional Azure services** toocontinue.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/2-create-app-service.png)
6. <span data-ttu-id="0e337-130">I hello **Services** klickar du på ** + ** tooadd en SQL-databas för din app.</span><span class="sxs-lookup"><span data-stu-id="0e337-130">In hello **Services** tab, click **+** tooadd a SQL Database for your app.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/3-add-sql-database.png)
7. <span data-ttu-id="0e337-131">I **Konfigurera SQL-databas**, klickar du på **ny** toocreate en SQL Server-instans.</span><span class="sxs-lookup"><span data-stu-id="0e337-131">In **Configure SQL Database**, click **New** toocreate a SQL Server instance.</span></span>
8. <span data-ttu-id="0e337-132">I **Konfigurera SQL Server**, konfigurera din SQL Server-instans.</span><span class="sxs-lookup"><span data-stu-id="0e337-132">In **Configure SQL Server**, configure your SQL Server instance.</span></span> <span data-ttu-id="0e337-133">Klicka på **OK**, **OK**, och **skapa** tookick av hello skapa en app i Azure.</span><span class="sxs-lookup"><span data-stu-id="0e337-133">Then, click **OK**, **OK**, and **Create** tookick off hello app creation in Azure.</span></span>
9. <span data-ttu-id="0e337-134">I **aktivitet för Azure App Service**, du kan se när hello skapa en app är klar.</span><span class="sxs-lookup"><span data-stu-id="0e337-134">In **Azure App Service Activity**, you can see when hello app creation is finished.</span></span> <span data-ttu-id="0e337-135">Klicka på * *publicera &lt; *appname*> toothis Webbapp nu ** Klicka **publicera**.</span><span class="sxs-lookup"><span data-stu-id="0e337-135">Click **Publish &lt;*appname*> toothis Web App now**, then click **Publish**.</span></span> 
   
    <span data-ttu-id="0e337-136">När Visual Studio är klar öppnas hello publicera appen i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0e337-136">Once Visual Studio finishes, it opens hello publish app in hello browser.</span></span> 
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-published-shown-in-browser.png)

<a name="bkmk_auth"></a>

## <a name="configure-authentication-and-directory-access"></a><span data-ttu-id="0e337-137">Konfigurera autentisering och åtkomst</span><span class="sxs-lookup"><span data-stu-id="0e337-137">Configure authentication and directory access</span></span>
1. <span data-ttu-id="0e337-138">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0e337-138">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0e337-139">Hello vänstra menyn klickar du på **Apptjänster** > **&lt;*appname*> ** > **autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="0e337-139">From hello left menu, click **App Services** > **&lt;*appname*>** > **Authentication / Authorization**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-app-service-authentication.png)
3. <span data-ttu-id="0e337-140">Aktivera Azure Active Directory-autentisering genom att klicka **på** > **Azure Active Directory** > **Express**  >  ** OK**.</span><span class="sxs-lookup"><span data-stu-id="0e337-140">Turn on Azure Active Directory authentication by clicking **On** > **Azure Active Directory** > **Express** > **OK**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-authentication-express.png)
4. <span data-ttu-id="0e337-141">Klicka på **spara** i hello kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="0e337-141">Click **Save** in hello command bar.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-authentication-save.png)
   
    <span data-ttu-id="0e337-142">När har sparats hello autentiseringsinställningar, kan du försöka gå tooyour app igen i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0e337-142">Once hello authentication settings are saved successfully, try navigating tooyour app again in hello browser.</span></span> <span data-ttu-id="0e337-143">Standardinställningarna för att autentisering på hello hela appen.</span><span class="sxs-lookup"><span data-stu-id="0e337-143">Your default settings enforce authentication on hello whole app.</span></span> <span data-ttu-id="0e337-144">Om du inte redan är inloggad är omdirigerade tooa inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="0e337-144">If you aren't already logged in, you are redirected tooa login screen.</span></span> <span data-ttu-id="0e337-145">Efter loggat in kan se du din app skyddas av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e337-145">Once logged in, you see your app secured by HTTPS.</span></span> <span data-ttu-id="0e337-146">Sedan måste tooenable åtkomst toodirectory data.</span><span class="sxs-lookup"><span data-stu-id="0e337-146">Next, you need tooenable access toodirectory data.</span></span> 
5. <span data-ttu-id="0e337-147">Navigera toohello [klassiska portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="0e337-147">Navigate toohello [classic portal](https://manage.windowsazure.com).</span></span>
6. <span data-ttu-id="0e337-148">Hello vänstra menyn klickar du på **Active Directory** > **standardkatalog** > **program**  >  * * &lt; *appname*> **.</span><span class="sxs-lookup"><span data-stu-id="0e337-148">From hello left menu, click **Active Directory** > **Default Directory** > **Applications** > **&lt;*appname*>**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-find-aad-application.png)
   
    <span data-ttu-id="0e337-149">Detta är hello Azure Active Directory-program som Apptjänst som skapats för du tooenable hello auktorisering / autentisering.</span><span class="sxs-lookup"><span data-stu-id="0e337-149">This is hello Azure Active Directory application that App Service created for you tooenable hello Authorization / Authentication feature.</span></span>
7. <span data-ttu-id="0e337-150">Klicka på **användare** och **grupper** toomake till att du har vissa användare och grupper i hello directory.</span><span class="sxs-lookup"><span data-stu-id="0e337-150">Click **Users** and **Groups** toomake sure that you have some users and groups in hello directory.</span></span> <span data-ttu-id="0e337-151">Om inte, skapa några testa användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="0e337-151">If not, create a few test users and groups.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-users-groups.png)
8. <span data-ttu-id="0e337-152">Klicka på **konfigurera** tooconfigure det här programmet.</span><span class="sxs-lookup"><span data-stu-id="0e337-152">Click **Configure** tooconfigure this application.</span></span>
9. <span data-ttu-id="0e337-153">Bläddra nedåt toohello **nycklar** och lägger till en nyckel genom att välja en varaktighet.</span><span class="sxs-lookup"><span data-stu-id="0e337-153">Scroll down toohello **Keys** section and add a key by selecting a duration.</span></span> <span data-ttu-id="0e337-154">Klicka på **delegerade behörigheter** och välj **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="0e337-154">Then, click **Delegated Permissions** and select **Read directory data**.</span></span> 
   <span data-ttu-id="0e337-155">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0e337-155">Click **Save**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-configure-aad-application.png)
10. <span data-ttu-id="0e337-156">När dina inställningar sparas, gå tillbaka toohello **nycklar** avsnittet och klicka på hello **kopiera** knappen toocopy hello klientens nyckel.</span><span class="sxs-lookup"><span data-stu-id="0e337-156">Once your settings are saved, scroll back up toohello **Keys** section and click hello **Copy** button toocopy hello client key.</span></span> 
    
     ![](./media/web-sites-dotnet-lob-application-azure-ad/11-get-app-key.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="0e337-157">Om du navigerar bort från den här sidan nu inte kan tooaccess klienten nyckeln någonsin igen.</span><span class="sxs-lookup"><span data-stu-id="0e337-157">If you navigate away from this page now, you won't be able tooaccess this client key ever again.</span></span>
    > 
    > 
11. <span data-ttu-id="0e337-158">Sedan måste tooconfigure ditt webbprogram med den här nyckeln.</span><span class="sxs-lookup"><span data-stu-id="0e337-158">Next, you need tooconfigure your web app with this key.</span></span> <span data-ttu-id="0e337-159">Logga in toohello [resursutforskaren Azure](https://resources.azure.com) med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0e337-159">Log in toohello [Azure Resource Explorer](https://resources.azure.com) with your Azure account.</span></span>
12. <span data-ttu-id="0e337-160">Hello överst på hello-sidan, klickar du på **Skrivskyddstyp** toomake ändringar i hello Azure Resursläsaren.</span><span class="sxs-lookup"><span data-stu-id="0e337-160">At hello top of hello page, click **Read/Write** toomake changes in hello Azure Resource Explorer.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/12-resource-manager-writable.png)
13. <span data-ttu-id="0e337-161">Hitta hello autentiseringsinställningar för din app på prenumerationer > * * &lt; *subscriptionname*> ** > **resursgrupper**  >  * * &lt; *resourcegroupname*> ** > **providers** > **Microsoft.Web**  >  **platser** > **&lt;*appname*> ** > **config**  >  **authsettings**.</span><span class="sxs-lookup"><span data-stu-id="0e337-161">Find hello authentication settings for your app, located at subscriptions > **&lt;*subscriptionname*>** > **resourceGroups** > **&lt;*resourcegroupname*>** > **providers** > **Microsoft.Web** > **sites** > **&lt;*appname*>** > **config** > **authsettings**.</span></span>
14. <span data-ttu-id="0e337-162">Klicka på **Redigera**.</span><span class="sxs-lookup"><span data-stu-id="0e337-162">Click **Edit**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/13-edit-authsettings.png)
15. <span data-ttu-id="0e337-163">Hello Redigera fönstret, ange hello `clientSecret` och `additionalLoginParams` egenskaper på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="0e337-163">In hello editing pane, set hello `clientSecret` and `additionalLoginParams` properties as follows.</span></span>
    
        ...
        "clientSecret": "<client key from hello Azure Active Directory application>",
        ...
        "additionalLoginParams": ["response_type=code id_token", "resource=https://graph.windows.net"],
        ...
16. <span data-ttu-id="0e337-164">Klicka på **placera** på hello översta toosubmit ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0e337-164">Click **Put** at hello top toosubmit your changes.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/14-edit-parameters.png)
17. <span data-ttu-id="0e337-165">Nu tootest om du har hello auktorisering token tooaccess hello Azure Active Directory Graph API, bara gå till * *https://&lt;*appname*>.azurewebsites.net/.auth/me** i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0e337-165">Now, tootest if you have hello authorization token tooaccess hello Azure Active Directory Graph API, just navigate to **https://&lt;*appname*>.azurewebsites.net/.auth/me** in your browser.</span></span> <span data-ttu-id="0e337-166">Om du har konfigurerat allt korrekt, bör du se hello `access_token` egenskap i hello JSON-svar.</span><span class="sxs-lookup"><span data-stu-id="0e337-166">If you configured everything correctly, you should see hello `access_token` property in hello JSON response.</span></span>
    
    <span data-ttu-id="0e337-167">Hej `~/.auth/me` URL-sökväg som hanteras av App Service autentisering / auktorisering toogive du alla hello information som rör tooyour autentiserad session.</span><span class="sxs-lookup"><span data-stu-id="0e337-167">hello `~/.auth/me` URL path is managed by App Service Authentication / Authorization toogive you all hello information related tooyour authenticated session.</span></span> <span data-ttu-id="0e337-168">Mer information finns i [autentisering och auktorisering i Azure App Service](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0e337-168">For more information, see [Authentication and authorization in Azure App Service](../app-service/app-service-authentication-overview.md).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="0e337-169">Hej `access_token` har en utgångsperiod.</span><span class="sxs-lookup"><span data-stu-id="0e337-169">hello `access_token` has an expiration period.</span></span> <span data-ttu-id="0e337-170">Dock App Service autentisering / auktorisering tillhandahåller token uppdateringsfunktionen med `~/.auth/refresh`.</span><span class="sxs-lookup"><span data-stu-id="0e337-170">However, App Service Authentication / Authorization provides token refresh functionality with `~/.auth/refresh`.</span></span> <span data-ttu-id="0e337-171">Mer information om hur toouse, se [App Service-Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).</span><span class="sxs-lookup"><span data-stu-id="0e337-171">For more information on how toouse it, see [App Service Token Store](https://cgillum.tech/2016/03/07/app-service-token-store/).</span></span>
    > 
    > 

<span data-ttu-id="0e337-172">Därefter kan du göra något användbart med katalogdata.</span><span class="sxs-lookup"><span data-stu-id="0e337-172">Next, you will do something useful with directory data.</span></span>

<a name="bkmk_crud"></a>

## <a name="add-line-of-business-functionality-tooyour-app"></a><span data-ttu-id="0e337-173">Lägg till funktioner för line-of-business tooyour app</span><span class="sxs-lookup"><span data-stu-id="0e337-173">Add line-of-business functionality tooyour app</span></span>
<span data-ttu-id="0e337-174">Nu kan skapa du en enkla CRUD arbete objekt spåraren.</span><span class="sxs-lookup"><span data-stu-id="0e337-174">Now, you create a simple CRUD work items tracker.</span></span>  

1. <span data-ttu-id="0e337-175">Skapa en klassfil som heter WorkItem.cs i hello ~\Models mapp och Ersätt `public class WorkItem {...}` med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="0e337-175">In hello ~\Models folder, create a class file called WorkItem.cs, and replace `public class WorkItem {...}` with hello following code:</span></span>
   
     <span data-ttu-id="0e337-176">med hjälp av System.ComponentModel.DataAnnotations;</span><span class="sxs-lookup"><span data-stu-id="0e337-176">using System.ComponentModel.DataAnnotations;</span></span>
   
     <span data-ttu-id="0e337-177">publik klass arbetsobjekt {</span><span class="sxs-lookup"><span data-stu-id="0e337-177">public class WorkItem   {</span></span>
   
         [Key]
         public int ItemID { get; set; }
         public string AssignedToID { get; set; }
         public string AssignedToName { get; set; }
         public string Description { get; set; }
         public WorkItemStatus Status { get; set; }
     <span data-ttu-id="0e337-178">}</span><span class="sxs-lookup"><span data-stu-id="0e337-178">}</span></span>
   
     <span data-ttu-id="0e337-179">offentliga enum WorkItemStatus {</span><span class="sxs-lookup"><span data-stu-id="0e337-179">public enum WorkItemStatus   {</span></span>
   
         Open,
         Investigating,
         Resolved,
         Closed
     <span data-ttu-id="0e337-180">}</span><span class="sxs-lookup"><span data-stu-id="0e337-180">}</span></span>
2. <span data-ttu-id="0e337-181">Skapa hello projektet toomake din nya modell tillgänglig toohello scaffold-teknik-logiken i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0e337-181">Build hello project toomake your new model accessible toohello scaffolding logic in Visual Studio.</span></span>
3. <span data-ttu-id="0e337-182">Lägg till ett nytt autogenererade objekt `WorkItemsController` toohello ~\Controllers mapp (Högerklicka på **domänkontrollanter**, peka för**Lägg till**, och välj **nytt autogenererade objekt**).</span><span class="sxs-lookup"><span data-stu-id="0e337-182">Add a new scaffolded item `WorkItemsController` toohello ~\Controllers folder (right-click **Controllers**, point too**Add**, and select **New scaffolded item**).</span></span> 
4. <span data-ttu-id="0e337-183">Välj **MVC 5 styrenhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0e337-183">Select **MVC 5 Controller with views, using Entity Framework** and click **Add**.</span></span>
5. <span data-ttu-id="0e337-184">Välj hello-modell som du skapade, klicka på ** + ** och sedan **Lägg till** tooadd datakontexten, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0e337-184">Select hello model that you created, then click **+** and then **Add** tooadd a data context, and then click **Add**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-azure-ad/16-add-scaffolded-controller.png)
6. <span data-ttu-id="0e337-185">Hitta hello i ~\Views\WorkItems\Create.cshtml (en automatiskt autogenererade artikel) `Html.BeginForm` hjälpmetod och att Hej följande markerade ändringar:</span><span class="sxs-lookup"><span data-stu-id="0e337-185">In ~\Views\WorkItems\Create.cshtml (an automatically scaffolded item), find hello `Html.BeginForm` helper method and make hello following highlighted changes:</span></span>  
   
   <pre class="prettyprint">
   @model WebApplication1.Models.WorkItem
   
   @{
    ViewBag.Title = &quot;Create&quot;;
   }
   
   &lt;h2&gt;Create&lt;/h2&gt;
   
   @using (Html.BeginForm(<mark>&quot;Create&quot;, &quot;WorkItems&quot;, FormMethod.Post, new { id = &quot;main-form&quot; }</mark>)) 
   {
    @Html.AntiForgeryToken()
   
    &lt;div class=&quot;form-horizontal&quot;&gt;
        &lt;h4&gt;WorkItem&lt;/h4&gt;
        &lt;hr /&gt;
        @Html.ValidationSummary(true, &quot;&quot;, new { @class = &quot;text-danger&quot; })
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToID, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToID, new { htmlAttributes = new { @class = &quot;form-control&quot;<mark>, @type = &quot;hidden&quot;</mark> } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToID, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.AssignedToName, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.AssignedToName, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.AssignedToName, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Description, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EditorFor(model =&gt; model.Description, new { htmlAttributes = new { @class = &quot;form-control&quot; } })
                @Html.ValidationMessageFor(model =&gt; model.Description, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            @Html.LabelFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;control-label col-md-2&quot; })
            &lt;div class=&quot;col-md-10&quot;&gt;
                @Html.EnumDropDownListFor(model =&gt; model.Status, htmlAttributes: new { @class = &quot;form-control&quot; })
                @Html.ValidationMessageFor(model =&gt; model.Status, &quot;&quot;, new { @class = &quot;text-danger&quot; })
            &lt;/div&gt;
        &lt;/div&gt;
   
        &lt;div class=&quot;form-group&quot;&gt;
            &lt;div class=&quot;col-md-offset-2 col-md-10&quot;&gt;
                &lt;input type=&quot;submit&quot; value=&quot;Create&quot; class=&quot;btn btn-default&quot;<mark> id=&quot;submit-button&quot;</mark> /&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
   }
   
   &lt;div&gt;
    @Html.ActionLink(&quot;Back tooList&quot;, &quot;Index&quot;)
   &lt;/div&gt;
   
   @section Scripts {
    @Scripts.Render(&quot;~/bundles/jqueryval&quot;)
    <mark>&lt;script&gt;
        // People/Group Picker Code
        var maxResultsPerPage = 14;
        var input = document.getElementById(&quot;AssignedToName&quot;);
   
        // Access token from request header, and tenantID from claims identity
        var token = &quot;@Request.Headers[&quot;X-MS-TOKEN-AAD-ACCESS-TOKEN&quot;]&quot;;
        var tenant =&quot;@(System.Security.Claims.ClaimsPrincipal.Current.Claims
                        .Where(c => c.Type == &quot;http://schemas.microsoft.com/identity/claims/tenantid&quot;)
                        .Select(c => c.Value).SingleOrDefault())&quot;;
   
        var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
   
        // Submit hello selected user/group toobe asssigned.
        $(&quot;#submit-button&quot;).click({ picker: picker }, function () {
            if (!picker.Selected())
                return;
            $(&quot;#main-form&quot;).get()[0].elements[&quot;AssignedToID&quot;].value = picker.Selected().objectId;
        });
    &lt;/script&gt;</mark>
   }
   </pre>
   
   <span data-ttu-id="0e337-186">Observera att `token` och `tenant` används av hello `AadPicker` objektet toomake Azure Active Directory Graph API-anrop.</span><span class="sxs-lookup"><span data-stu-id="0e337-186">Note that `token` and `tenant` are used by hello `AadPicker` object toomake Azure Active Directory Graph API calls.</span></span> <span data-ttu-id="0e337-187">Du lägger till `AadPicker` senare.</span><span class="sxs-lookup"><span data-stu-id="0e337-187">You'll add `AadPicker` later.</span></span>     
   
   > [!NOTE]
   > <span data-ttu-id="0e337-188">Du får bara samt `token` och `tenant` från hello på klientsidan med `~/.auth/me`, men som skulle vara en ytterligare server-anrop.</span><span class="sxs-lookup"><span data-stu-id="0e337-188">You can just as well get `token` and `tenant` from hello client side with `~/.auth/me`, but that would be an additional server call.</span></span> <span data-ttu-id="0e337-189">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0e337-189">For example:</span></span>
   > 
   > <span data-ttu-id="0e337-190">$.ajax ({dataType: ”json”, url: ”/.auth/me”, lyckade: funktionen (data) {var token = data [0] .access_token; var innehavare = data [0] .user_claims .find (c = > c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val;}});</span><span class="sxs-lookup"><span data-stu-id="0e337-190">$.ajax({ dataType: "json", url: "/.auth/me", success: function (data) { var token = data[0].access_token; var tenant = data[0].user_claims .find(c => c.typ === 'http://schemas.microsoft.com/identity/claims/tenantid') .val; } });</span></span>
   > 
   > 
7. <span data-ttu-id="0e337-191">Ändra hello samma med ~ \Views\WorkItems\Edit.cshtml.</span><span class="sxs-lookup"><span data-stu-id="0e337-191">Make hello same changes with ~\Views\WorkItems\Edit.cshtml.</span></span>
8. <span data-ttu-id="0e337-192">Hej `AadPicker` objekt definieras i ett skript som du behöver tooadd tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="0e337-192">hello `AadPicker` object is defined in a script that you need tooadd tooyour project.</span></span> <span data-ttu-id="0e337-193">Högerklicka på hello ~\Scripts mappen, peka för**Lägg till**, och klicka på **JavaScript-fil**.</span><span class="sxs-lookup"><span data-stu-id="0e337-193">Right-click hello ~\Scripts folder, point too**Add**, and click **JavaScript file**.</span></span> <span data-ttu-id="0e337-194">Typen `AadPickerLibrary` för hello filnamn och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e337-194">Type `AadPickerLibrary` for hello filename and click **OK**.</span></span>
9. <span data-ttu-id="0e337-195">Kopiera hello innehåll från [här](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) i ~ \Scripts\AadPickerLibrary.js.</span><span class="sxs-lookup"><span data-stu-id="0e337-195">Copy hello content from [here](https://raw.githubusercontent.com/cephalin/active-directory-dotnet-webapp-roleclaims/master/WebApp-RoleClaims-DotNet/Scripts/AadPickerLibrary.js) into ~\Scripts\AadPickerLibrary.js.</span></span>
   
   <span data-ttu-id="0e337-196">I hello skript hello `AadPicker` objekt anrop [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch för användare och grupper som matchar hello indata.</span><span class="sxs-lookup"><span data-stu-id="0e337-196">In hello script, hello `AadPicker` object calls [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) toosearch for users and groups that match hello input.</span></span>  
10. <span data-ttu-id="0e337-197">~\Scripts\AadPickerLibrary.js använder också hello [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/).</span><span class="sxs-lookup"><span data-stu-id="0e337-197">~\Scripts\AadPickerLibrary.js also uses hello [jQuery UI Autocomplete widget](https://jqueryui.com/autocomplete/).</span></span> <span data-ttu-id="0e337-198">Du behöver tooadd jQuery UI tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="0e337-198">So you need tooadd jQuery UI tooyour project.</span></span> <span data-ttu-id="0e337-199">Högerklicka på projektet i och klickar på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="0e337-199">Right-click your project in and click **Manage NuGet Packages**.</span></span>
11. <span data-ttu-id="0e337-200">I hello NuGet-Pakethanteraren, klickar du på Bläddra, typen **jquery ui-** i hello sökfältet och klickar på **jQuery.UI.Combined**.</span><span class="sxs-lookup"><span data-stu-id="0e337-200">In hello NuGet Package Manager, click Browse, type **jquery-ui** in hello search bar, and click **jQuery.UI.Combined**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/17-add-jquery-ui-nuget.png)
12. <span data-ttu-id="0e337-201">I hello högra rutan, klickar du på **installera**, klicka på **OK** tooproceed.</span><span class="sxs-lookup"><span data-stu-id="0e337-201">In hello right pane, click **Install**, then click **OK** tooproceed.</span></span>
13. <span data-ttu-id="0e337-202">Öppna ~\App_Start\BundleConfig.cs och att Hej följande markerade ändringar:</span><span class="sxs-lookup"><span data-stu-id="0e337-202">Open ~\App_Start\BundleConfig.cs and make hello following highlighted changes:</span></span>  
    
    <pre class="prettyprint">
    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle(&quot;~/bundles/jquery&quot;).Include(
                    &quot;~/Scripts/jquery-{version}.js&quot;<mark>,
                    &quot;~/Scripts/jquery-ui-{version}.js&quot;,
                    &quot;~/Scripts/AadPickerLibrary.js&quot;</mark>));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/jqueryval&quot;).Include(
                    &quot;~/Scripts/jquery.validate*&quot;));
    
        // Use hello development version of Modernizr toodevelop with and learn from. Then, when you&#39;re
        // ready for production, use hello build tool at http://modernizr.com toopick only hello tests you need.
        bundles.Add(new ScriptBundle(&quot;~/bundles/modernizr&quot;).Include(
                    &quot;~/Scripts/modernizr-*&quot;));
    
        bundles.Add(new ScriptBundle(&quot;~/bundles/bootstrap&quot;).Include(
                    &quot;~/Scripts/bootstrap.js&quot;,
                    &quot;~/Scripts/respond.js&quot;));
    
        bundles.Add(new StyleBundle(&quot;~/Content/css&quot;).Include(
                    &quot;~/Content/bootstrap.css&quot;,
                    &quot;~/Content/site.css&quot;<mark>,
                    &quot;~/Content/themes/base/jquery-ui.css&quot;</mark>));
    }
    </pre>
    
    <span data-ttu-id="0e337-203">Det finns flera performant sätt toomanage JavaScript och CSS-filer i din app.</span><span class="sxs-lookup"><span data-stu-id="0e337-203">There are more performant ways toomanage JavaScript and CSS files in your app.</span></span> <span data-ttu-id="0e337-204">Men för enkelhetens skull ska bara toopiggyback på hello-paket som har lästs in med varje vy.</span><span class="sxs-lookup"><span data-stu-id="0e337-204">However, for simplicity you're just going toopiggyback on hello bundles that are loaded with every view.</span></span>
14. <span data-ttu-id="0e337-205">Slutligen i ~ \Global.asax, Lägg till följande kodrad i hello hello `Application_Start()` metod.</span><span class="sxs-lookup"><span data-stu-id="0e337-205">Finally, in ~\Global.asax, add hello following line of code in hello `Application_Start()` method.</span></span> <span data-ttu-id="0e337-206">`Ctrl`+`.`på varje namngivning matchningsfel för åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="0e337-206">`Ctrl`+`.` on each naming resolution error too fix it.</span></span>
    
        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier;
    
    > [!NOTE]
    > <span data-ttu-id="0e337-207">Du behöver följande kodrad eftersom hello standard MVC-mallen använder <code>[ValidateAntiForgeryToken]</code> decoration på vissa hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0e337-207">You need this line of code because hello default MVC template uses <code>[ValidateAntiForgeryToken]</code> decoration on some of hello actions.</span></span> <span data-ttu-id="0e337-208">På grund av toohello problemet som beskrivs av [Brock Allen](https://twitter.com/BrockLAllen) på [MVC 4, AntiForgeryToken och anspråk](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) HTTP POST kan misslyckas skydd mot förfalskning token verifieringen eftersom:</span><span class="sxs-lookup"><span data-stu-id="0e337-208">Due toohello behavior described by [Brock Allen](https://twitter.com/BrockLAllen) at [MVC 4, AntiForgeryToken and Claims](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) your HTTP POST may fail anti-forgery token validation because:</span></span>
    > 
    > * <span data-ttu-id="0e337-209">Azure Active Directory skickar inte hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, vilket krävs normalt av hello antiförfalskningstoken.</span><span class="sxs-lookup"><span data-stu-id="0e337-209">Azure Active Directory does not send hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider, which is required by default by hello anti-forgery token.</span></span>
    > * <span data-ttu-id="0e337-210">Om Azure Active Directory är directory synkroniseras med AD FS, skickar hello AD FS-förtroende som standard inte hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider anspråk, även om du kan konfigurera AD FS toosend manuellt Denna begäran.</span><span class="sxs-lookup"><span data-stu-id="0e337-210">If Azure Active Directory is directory synced with AD FS, hello AD FS trust by default does not send hello http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider claim either, although you can manually configure AD FS toosend this claim.</span></span>
    > 
    > <span data-ttu-id="0e337-211">`ClaimTypes.NameIdentifies`Anger hello anspråk `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, som tillhandahåller Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e337-211">`ClaimTypes.NameIdentifies` specifies hello claim `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, which Azure Active Directory does supply.</span></span>  
    > 
    > 
15. <span data-ttu-id="0e337-212">Nu kan publicera dina ändringar.</span><span class="sxs-lookup"><span data-stu-id="0e337-212">Now, publish your changes.</span></span> <span data-ttu-id="0e337-213">Högerklicka på projektet och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="0e337-213">Right-click your project and click **Publish**.</span></span>
16. <span data-ttu-id="0e337-214">Klicka på **inställningar**, kontrollera att det finns en anslutning sträng tooyour SQL-databasen, väljer **Update Database** toomake hello schemaändringar för din modell och på **publicera** .</span><span class="sxs-lookup"><span data-stu-id="0e337-214">Click **Settings**, make sure there is a connection string tooyour SQL Database, select **Update Database** toomake hello schema changes for your model, and click **Publish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/18-publish-crud-changes.png)
17. <span data-ttu-id="0e337-215">I hello webbläsare navigerar toohttps: / /&lt;*appname*>.azurewebsites.net/workitems och klicka på **Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="0e337-215">In hello browser, navigate toohttps://&lt;*appname*>.azurewebsites.net/workitems and click **Create New**.</span></span>
18. <span data-ttu-id="0e337-216">Klicka i hello **AssignedToName** rutan.</span><span class="sxs-lookup"><span data-stu-id="0e337-216">Click in hello **AssignedToName** box.</span></span> <span data-ttu-id="0e337-217">Du bör nu se användare och grupper från din Azure Active Directory-klient i en listruta.</span><span class="sxs-lookup"><span data-stu-id="0e337-217">You should now see users and groups from your Azure Active Directory tenant in a dropdown.</span></span> <span data-ttu-id="0e337-218">Du kan ange toofilter eller använda hello `Up` eller `Down` nyckel eller klicka på tooselect hello användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="0e337-218">You can type toofilter, or use hello `Up` or `Down` key or click tooselect hello user or group.</span></span> 
    
    ![](./media/web-sites-dotnet-lob-application-azure-ad/19-use-aadpicker.png)
19. <span data-ttu-id="0e337-219">Klicka på **skapa** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="0e337-219">Click **Create** toosave hello changes.</span></span> <span data-ttu-id="0e337-220">Klicka på **redigera** på hello skapade arbete objektet tooobserve hello samma beteende.</span><span class="sxs-lookup"><span data-stu-id="0e337-220">Then, click **Edit** on hello created work item tooobserve hello same behavior.</span></span>

<span data-ttu-id="0e337-221">Congrats, nu körs en av branschspecifika app i Azure med katalogåtkomst!</span><span class="sxs-lookup"><span data-stu-id="0e337-221">Congrats, you are now running a line-of-business app in Azure with directory access!</span></span> <span data-ttu-id="0e337-222">Det finns mycket mer du kan göra med hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="0e337-222">There's a lot more you can do with hello Graph API.</span></span> <span data-ttu-id="0e337-223">Se [Azure AD Graph API-referens för](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span><span class="sxs-lookup"><span data-stu-id="0e337-223">See [Azure AD Graph API reference](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog).</span></span>

<a name="next"></a>

## <a name="next-step"></a><span data-ttu-id="0e337-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0e337-224">Next Step</span></span>
<span data-ttu-id="0e337-225">Om du behöver rollbaserad åtkomstkontroll (RBAC) för din line-of-business-app i azure finns [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) ett exempel från hello Azure Active Directory-teamet.</span><span class="sxs-lookup"><span data-stu-id="0e337-225">If you need role-based access control (RBAC) for your line-of-business app in azure, see [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) for a sample from hello Azure Active Directory team.</span></span> <span data-ttu-id="0e337-226">Den visar hur tooenable roller för ditt Azure Active Directory-program och auktorisera användare med hello `[Authorize]` decoration.</span><span class="sxs-lookup"><span data-stu-id="0e337-226">It shows you how tooenable roles for your Azure Active Directory application, and then authorize users with hello `[Authorize]` decoration.</span></span>

<span data-ttu-id="0e337-227">Om din line-of-business-app behöver komma åt tooon lokala data, se [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0e337-227">If your line-of-business app needs access tooon-premises data, see [Access on-premises resources using hybrid connections in Azure App Service](web-sites-hybrid-connection-get-started.md).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="0e337-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0e337-228">Further resources</span></span>
* [<span data-ttu-id="0e337-229">Autentisering och auktorisering i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0e337-229">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)
* [<span data-ttu-id="0e337-230">Autentisera med lokala Active Directory i din Azure-app</span><span class="sxs-lookup"><span data-stu-id="0e337-230">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="0e337-231">Skapa en line-of-business-app i Azure med AD FS-autentisering</span><span class="sxs-lookup"><span data-stu-id="0e337-231">Create a line-of-business app in Azure with AD FS authentication</span></span>](web-sites-dotnet-lob-application-adfs.md)
* [<span data-ttu-id="0e337-232">App Service Auth och hello Azure AD Graph API</span><span class="sxs-lookup"><span data-stu-id="0e337-232">App Service Auth and hello Azure AD Graph API</span></span>](https://cgillum.tech/2016/03/25/app-service-auth-aad-graph-api/)
* [<span data-ttu-id="0e337-233">Microsoft Azure Active Directory-exempel och dokumentation</span><span class="sxs-lookup"><span data-stu-id="0e337-233">Microsoft Azure Active Directory Samples and Documentation</span></span>](https://github.com/AzureADSamples)
* [<span data-ttu-id="0e337-234">Azure Active Directory-Token och anspråkstyper som stöds</span><span class="sxs-lookup"><span data-stu-id="0e337-234">Azure Active Directory Supported Token and Claim Types</span></span>](http://msdn.microsoft.com/library/azure/dn195587.aspx)
