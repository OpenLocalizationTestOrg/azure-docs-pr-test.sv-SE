---
title: "aaaUser autentisering för API Apps i Azure App Service | Microsoft Docs"
description: "Lär dig hur tooprotect en API-app i Azure App Service genom att tillåta åtkomst till endast tooauthenticated användare."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: 4702fc77fcfe736405e22b033c35d22ae3c5a300
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a><span data-ttu-id="ef145-103">Användarautentisering för API Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef145-103">User authentication for API Apps in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="ef145-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ef145-104">Overview</span></span>
<span data-ttu-id="ef145-105">Den här artikeln visar hur tooprotect Azure API-app så som endast autentiserade användare kan anropa.</span><span class="sxs-lookup"><span data-stu-id="ef145-105">This article shows how tooprotect an Azure API app so that only authenticated users can call it.</span></span> <span data-ttu-id="ef145-106">hello förutsätter att du har läst hello [översikt över Azure App Service-autentisering](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ef145-106">hello article assumes that you have read hello [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span>

<span data-ttu-id="ef145-107">Du får lära dig:</span><span class="sxs-lookup"><span data-stu-id="ef145-107">You'll learn:</span></span>

* <span data-ttu-id="ef145-108">Hur tooconfigure en autentiseringsprovider, med information om Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ef145-108">How tooconfigure an authentication provider, with details for Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="ef145-109">Hur tooconsume en skyddad API-app med hjälp av hello [Active Directory Authentication Library (ADAL) för JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).</span><span class="sxs-lookup"><span data-stu-id="ef145-109">How tooconsume a protected API app by using hello [Active Directory Authentication Library (ADAL) for JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).</span></span>

<span data-ttu-id="ef145-110">hello artikeln innehåller två avsnitt:</span><span class="sxs-lookup"><span data-stu-id="ef145-110">hello article contains two sections:</span></span>

* <span data-ttu-id="ef145-111">Hej [hur tooconfigure användarautentisering i Azure App Service](#authconfig) förklaras i allmänna hur tooconfigure användarautentisering för API-app och gäller även tooall ramverk som stöds av Apptjänsten, inklusive .NET, Node.js och Java.</span><span class="sxs-lookup"><span data-stu-id="ef145-111">hello [How tooconfigure user authentication in Azure App Service](#authconfig) section explains in general how tooconfigure user authentication for any API app and applies equally tooall frameworks supported by App Service, including .NET, Node.js, and Java.</span></span>
* <span data-ttu-id="ef145-112">Från och med hello [fortsätter hello .NET API Apps självstudier](#tutorialstart) avsnittet, hello artikel guider som du konfigurerar ett exempelprogram med en .NET säkerhetskopiera slutet och en AngularJS-klient så att den använder Azure Active Directory för användare autentisering.</span><span class="sxs-lookup"><span data-stu-id="ef145-112">Starting with hello [Continuing hello .NET API Apps tutorials](#tutorialstart) section, hello article guides you through configuring a sample application with a .NET back end and an AngularJS front end so that it uses Azure Active Directory for user authentication.</span></span> 

## <span data-ttu-id="ef145-113"><a id="authconfig"></a>Hur tooconfigure användarautentisering i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef145-113"><a id="authconfig"></a> How tooconfigure user authentication in Azure App Service</span></span>
<span data-ttu-id="ef145-114">Det här avsnittet innehåller allmänna anvisningar som gäller tooany API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-114">This section provides general instructions that apply tooany API app.</span></span> <span data-ttu-id="ef145-115">För steg specifika toohello tooDo lista .NET exempelprogrammet gå för[fortsätter hello .NET komma igång-Självstudier](#tutorialstart).</span><span class="sxs-lookup"><span data-stu-id="ef145-115">For steps specific toohello tooDo List .NET sample application, go too[Continuing hello .NET getting-started tutorials](#tutorialstart).</span></span>

1. <span data-ttu-id="ef145-116">I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **inställningar** bladet hello API-appen som du vill tooprotect, hitta hello **funktioner** avsnittet och klicka sedan på  **Autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="ef145-116">In hello [Azure portal](https://portal.azure.com/), navigate toohello **Settings** blade of hello API app that you want tooprotect, find hello **Features** section, and then click **Authentication/ Authorization**.</span></span>
   
    ![Azure-portalen autentisering/auktorisering](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. <span data-ttu-id="ef145-118">I hello **autentisering / auktorisering** bladet, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="ef145-118">In hello **Authentication / Authorization** blade, click **On**.</span></span>
3. <span data-ttu-id="ef145-119">Välj ett alternativ för hello hello **åtgärd tootake när begäran inte har autentiserats** listrutan.</span><span class="sxs-lookup"><span data-stu-id="ef145-119">Select one of hello options from hello **Action tootake when request is not authenticated** drop-down list.</span></span>
   
   * <span data-ttu-id="ef145-120">Om du vill endast autentiserade anrop tooreach API-appen, Välj något av hello **logga in med...**  alternativ.</span><span class="sxs-lookup"><span data-stu-id="ef145-120">If you want only authenticated calls tooreach your API app, choose one of hello **Log in with ...** options.</span></span> <span data-ttu-id="ef145-121">Det här alternativet kan du tooprotect hello API-app utan att skriva någon kod som körs i den.</span><span class="sxs-lookup"><span data-stu-id="ef145-121">This option enables you tooprotect hello API app without writing any code that runs in it.</span></span>
   * <span data-ttu-id="ef145-122">Om du vill att alla anrop tooreach API-appen, Välj **Tillåt begäran (ingen åtgärd)**.</span><span class="sxs-lookup"><span data-stu-id="ef145-122">If you want all calls tooreach your API app, choose **Allow request (no action)**.</span></span> <span data-ttu-id="ef145-123">Du kan använda det här alternativet toodirect oautentiserad anropare tooa valet av autentiseringsproviders.</span><span class="sxs-lookup"><span data-stu-id="ef145-123">You can use this option toodirect unauthenticated callers tooa choice of authentication providers.</span></span> <span data-ttu-id="ef145-124">Med det här alternativet har toowrite kod toohandle tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ef145-124">With this option you have toowrite code toohandle authorization.</span></span>
     
     <span data-ttu-id="ef145-125">Mer information finns i [Autentisering och auktorisering för API Apps i Azure Apptjänst](app-service-api-authentication.md#multiple-protection-options).</span><span class="sxs-lookup"><span data-stu-id="ef145-125">For more information, see [Authentication and authorization for API Apps in Azure App Service](app-service-api-authentication.md#multiple-protection-options).</span></span>
4. <span data-ttu-id="ef145-126">Välj en eller flera hello **autentiseringsproviders**.</span><span class="sxs-lookup"><span data-stu-id="ef145-126">Select one or more of hello **Authentication Providers**.</span></span>
   
    <span data-ttu-id="ef145-127">hello bilden visar val som kräver att alla anropare toobe autentiseras av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef145-127">hello image shows    choices that require all callers toobe authenticated by Azure AD.</span></span>
   
    ![Azure portal autentisering/auktorisering, blad](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    <span data-ttu-id="ef145-129">När du väljer en autentiseringsprovider visas hello ett blad i konfigurationen för providern.</span><span class="sxs-lookup"><span data-stu-id="ef145-129">When you choose an authentication provider, hello portal displays a configuration blade for that provider.</span></span> 
   
    <span data-ttu-id="ef145-130">Detaljerade instruktioner som förklarar hur tooconfigure hello authentication provider configuration blad finns [hur tooconfigure Apptjänst programmet toouse Azure Active Directory inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="ef145-130">For detailed instructions that explain how tooconfigure hello authentication provider configuration blades, see [How tooconfigure your App Service application toouse Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span> <span data-ttu-id="ef145-131">(hello länken går tooan artikel om Azure AD, men själva hello artikeln innehåller flikar som länkar tooarticles för hello andra autentiseringsproviders.)</span><span class="sxs-lookup"><span data-stu-id="ef145-131">(hello link goes tooan article about Azure AD, but hello article itself contains tabs that link tooarticles for hello other authentication providers.)</span></span>
5. <span data-ttu-id="ef145-132">När du är klar med hello authentication provider configuration bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef145-132">When you're done with hello authentication provider configuration blade, click **OK**.</span></span>
6. <span data-ttu-id="ef145-133">I hello **autentisering / auktorisering** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ef145-133">In hello **Authentication / Authorization** blade, click **Save**.</span></span>

<span data-ttu-id="ef145-134">När det är klart autentiserar alla API-anrop i App Service innan de når hello API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-134">When this is done, App Service authenticates all API calls before they reach hello API app.</span></span> <span data-ttu-id="ef145-135">hello autentisering fungerar hello samma för alla språk som stöds av apptjänst, inklusive .NET, Node.js och Java.</span><span class="sxs-lookup"><span data-stu-id="ef145-135">hello authentication services work hello same for all languages that App service supports, including .NET, Node.js, and Java.</span></span> 

<span data-ttu-id="ef145-136">toomake autentiserad API-anrop, hello anroparen innehåller hello authentication provider OAuth 2.0-ägartoken i hello Authorization-huvud av HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="ef145-136">toomake authenticated API calls, hello caller includes hello authentication provider's OAuth 2.0 bearer token in hello Authorization header of HTTP requests.</span></span> <span data-ttu-id="ef145-137">hello token kan erhållas med hjälp av hello authentication provider SDK.</span><span class="sxs-lookup"><span data-stu-id="ef145-137">hello token can be acquired by using hello authentication provider's SDK.</span></span>

## <span data-ttu-id="ef145-138"><a id="tutorialstart"></a>Fortsätter hello .NET API Apps självstudier</span><span class="sxs-lookup"><span data-stu-id="ef145-138"><a id="tutorialstart"></a> Continuing hello .NET API Apps tutorials</span></span>
<span data-ttu-id="ef145-139">Om du följer hello Node.js eller Java självstudier för API apps, hoppar du över nästa artikel toohello [primära autentiseringen av tjänsten för API apps](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="ef145-139">If you are following hello Node.js or Java tutorials for API apps, skip toohello next article, [service principal authentication for API apps](app-service-api-dotnet-service-principal-auth.md).</span></span> 

<span data-ttu-id="ef145-140">Om du följer hello serien med .NET-självstudiekurs för API apps och redan har distribuerat hello exempelprogrammet enligt anvisningarna i hello [första](app-service-api-dotnet-get-started.md) och [andra](app-service-api-cors-consume-javascript.md) självstudier, hoppa över toohello [ställa in autentisering i Apptjänst och Azure AD](#azureauth) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ef145-140">If you are following hello .NET tutorial series for API apps and have already deployed hello sample application as directed in hello [first](app-service-api-dotnet-get-started.md) and [second](app-service-api-cors-consume-javascript.md) tutorials, skip toohello [Set up authentication in App Service and Azure AD](#azureauth) section.</span></span>

<span data-ttu-id="ef145-141">Om du vill toofollow självstudierna utan att gå via hello första och andra självstudiekurser hello följande som förklarar hur tooget startats med hjälp av en automatiserad process toodeploy hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ef145-141">If you want toofollow this tutorial without going through hello first and second tutorials, do hello following steps which explain how tooget started by using an automated process toodeploy hello sample application.</span></span>

> [!NOTE]
> <span data-ttu-id="ef145-142">hello följande steg hjälper dig toohello samma utgångspunkten som om du hello två första självstudier, med ett undantag: Visual Studio inte redan känner till vilka webbprogram eller API-app som varje projekt distribueras till.</span><span class="sxs-lookup"><span data-stu-id="ef145-142">hello following steps get you toohello same starting point as if you did hello first two tutorials, with one exception: Visual Studio won't already know which web app or API app that each project gets deployed to.</span></span> <span data-ttu-id="ef145-143">Det innebär att du inte har exakta instruktioner i den här självstudiekursen som förklarar hur toodeploy toohello rätt mål.</span><span class="sxs-lookup"><span data-stu-id="ef145-143">That means you won't have exact instructions in this tutorial that explain how toodeploy toohello right targets.</span></span> <span data-ttu-id="ef145-144">Om du inte är van vid att räkna ut hur toodo hello distributionsstegen på egen hand, är det bättre toofollow hello självstudiekursen serie från hello [första självstudierna](app-service-api-dotnet-get-started.md) automatisk än toostart med den här processen för distribution.</span><span class="sxs-lookup"><span data-stu-id="ef145-144">If you're not comfortable with figuring out how toodo hello deployment steps on your own, it's better toofollow hello tutorial series from hello [first tutorial](app-service-api-dotnet-get-started.md) than toostart with this automated deployment process.</span></span>
> 
> 

1. <span data-ttu-id="ef145-145">Kontrollera att du har alla hello förutsättningar som anges i hello [första självstudierna](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ef145-145">Make sure that you have all of hello prerequisites listed in hello [first tutorial](app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="ef145-146">I tillägg toohello kraven, antas dessa självstudiekurser för autentisering som du har arbetat med App Service web apps och API apps i Visual Studio och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ef145-146">In addition toohello prerequisites listed, these authentication tutorials assume that you have worked with App Service web apps and API apps in Visual Studio and hello Azure portal.</span></span>
2. <span data-ttu-id="ef145-147">Klicka på hello **distribuera tooAzure** knapp i hello [tooDo lista exempel databasen viktigt-fil](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) toodeploy hello API apps och hello web app.</span><span class="sxs-lookup"><span data-stu-id="ef145-147">Click hello **Deploy tooAzure** button in hello [tooDo List sample repository readme file](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) toodeploy hello API apps and hello web app.</span></span> <span data-ttu-id="ef145-148">Anteckna hello Azure-resursgrupp som skapas, som du kan använda den här senare toolook in webbappen och API-appnamn.</span><span class="sxs-lookup"><span data-stu-id="ef145-148">Make a note of hello Azure resource group that gets created, as you can use this later toolook up web app and API app names.</span></span>
3. <span data-ttu-id="ef145-149">Ladda ned eller klona hello [tooDo lista exempel databasen](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tooget hello kod som du ska arbeta med i Visual Studio lokalt.</span><span class="sxs-lookup"><span data-stu-id="ef145-149">Download or clone hello [tooDo List sample repository](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tooget hello code that you'll work with locally in Visual Studio.</span></span>

## <span data-ttu-id="ef145-150"><a id="azureauth"></a>Konfigurera autentisering i Apptjänst och Azure AD</span><span class="sxs-lookup"><span data-stu-id="ef145-150"><a id="azureauth"></a> Set up authentication in App Service and Azure AD</span></span>
<span data-ttu-id="ef145-151">Nu har du hello-program som körs i Azure App Service utan att kräva att användarna autentiseras.</span><span class="sxs-lookup"><span data-stu-id="ef145-151">You now have hello application running in Azure App Service without requiring that users be authenticated.</span></span> <span data-ttu-id="ef145-152">I det här avsnittet du lägger till autentisering genom att göra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ef145-152">In this section you add authentication by doing hello following tasks:</span></span>

* <span data-ttu-id="ef145-153">Konfigurera App Service toorequire Azure Active Directory (Azure AD)-autentisering för att anropa hello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-153">Configure App Service toorequire Azure Active Directory (Azure AD) authentication for calling hello middle tier API app.</span></span>
* <span data-ttu-id="ef145-154">Skapa ett Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="ef145-154">Create an Azure AD application.</span></span>
* <span data-ttu-id="ef145-155">Konfigurera hello Azure AD application toosend hello ägar-token efter inloggning toohello AngularJS-klient.</span><span class="sxs-lookup"><span data-stu-id="ef145-155">Configure hello Azure AD application toosend hello bearer token after logon toohello AngularJS front end.</span></span> 

<span data-ttu-id="ef145-156">Om du stöter på problem när följande hello självstudiekursen anvisningar Se hello [felsökning](#troubleshooting) avsnittet hello slutet av hello kursen.</span><span class="sxs-lookup"><span data-stu-id="ef145-156">If you run into problems while following hello tutorial directions, see hello [Troubleshooting](#troubleshooting) section at hello end of hello tutorial.</span></span> 

### <a name="configure-authentication-for-hello-middle-tier-api-app"></a><span data-ttu-id="ef145-157">Konfigurera autentisering för hello mellannivå-API-app</span><span class="sxs-lookup"><span data-stu-id="ef145-157">Configure authentication for hello middle tier API app</span></span>
1. <span data-ttu-id="ef145-158">I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **inställningar** bladet för hello API-app som du skapade för ToDoListAPI-projektet hello hitta hello **funktioner** avsnitt, och sedan Klicka på **autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="ef145-158">In hello [Azure portal](https://portal.azure.com/), navigate toohello **Settings** blade of hello API app that you created for hello ToDoListAPI project, find hello **Features** section, and then click **Authentication / Authorization**.</span></span>
   
    ![Azure-portalen autentisering/auktorisering](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. <span data-ttu-id="ef145-160">I hello **autentisering / auktorisering** bladet, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="ef145-160">In hello **Authentication / Authorization** blade, click **On**.</span></span>
3. <span data-ttu-id="ef145-161">I hello **åtgärd tootake när begäran inte har autentiserats** listrutan, Välj **logga in med Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ef145-161">In hello **Action tootake when request is not authenticated** drop-down list, select **Log in with Azure Active Directory**.</span></span>
   
    <span data-ttu-id="ef145-162">Det här alternativet innebär att inga unauathenticated begäranden ska nå hello API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-162">This option ensures that no unauathenticated requests will reach hello API app.</span></span> 
4. <span data-ttu-id="ef145-163">Under **autentiseringsproviders**, klickar du på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ef145-163">Under **Authentication Providers**, click **Azure Active Directory**.</span></span>
   
    ![Azure portal autentisering/auktorisering, blad](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. <span data-ttu-id="ef145-165">I hello **inställningarna för Azure Active Directory** bladet, klickar du på **Express**</span><span class="sxs-lookup"><span data-stu-id="ef145-165">In hello **Azure Active Directory Settings** blade, click **Express**</span></span>
   
    ![Azure portal autentisering/auktorisering Express alternativet](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    <span data-ttu-id="ef145-167">Med hello **Express** alternativet Apptjänst kan automatiskt skapa ett Azure AD-program i din Azure AD [klient](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant).</span><span class="sxs-lookup"><span data-stu-id="ef145-167">With hello **Express** option, App Service can automatically create an Azure AD application in your Azure AD [tenant](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant).</span></span> 
   
    <span data-ttu-id="ef145-168">Du har inte toocreate en klient, eftersom varje Azure-konto har en automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ef145-168">You don't have toocreate a tenant, because every Azure account automatically has one.</span></span>
6. <span data-ttu-id="ef145-169">Under **hanteringsläge**, klickar du på **Skapa nytt AD-App** om den inte redan är markerat, och notera hello värdet i hello **skapa App** textrutan; du måste slå upp den här AAD program i hello klassiska Azure-portalen senare.</span><span class="sxs-lookup"><span data-stu-id="ef145-169">Under **Management mode**, click **Create New AD App** if it isn't already selected, and notice hello value that is in hello **Create App** text box; you'll look up this AAD application in hello Azure classic portal later.</span></span>
   
    ![Azure-portalen Azure AD-inställningar](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    <span data-ttu-id="ef145-171">Azure skapar automatiskt en Azure AD-program med hello angivet namn i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="ef145-171">Azure will automatically create an Azure AD application with hello indicated name in your Azure AD tenant.</span></span> <span data-ttu-id="ef145-172">Som standard hello Azure AD-program med namnet hello samma som hello API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-172">By default, hello Azure AD application is named hello same as hello API app.</span></span> <span data-ttu-id="ef145-173">Om du vill kan ange du ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="ef145-173">If you prefer, you can enter a different name.</span></span>
7. <span data-ttu-id="ef145-174">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef145-174">Click **OK**.</span></span>
8. <span data-ttu-id="ef145-175">I hello **autentisering / auktorisering** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ef145-175">In hello **Authentication / Authorization** blade, click **Save**.</span></span>
   
    ![Klicka på Spara](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

<span data-ttu-id="ef145-177">Endast användare i Azure AD-klienten kan nu anropa hello API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-177">Now only users in your Azure AD tenant can call hello API app.</span></span>

### <a name="optional-test-hello-api-app"></a><span data-ttu-id="ef145-178">Valfritt: Testa hello API-app</span><span class="sxs-lookup"><span data-stu-id="ef145-178">Optional: Test hello API app</span></span>
1. <span data-ttu-id="ef145-179">I en webbläsare går toohello Webbadressen till hello API-app: i hello **API-app** bladet i hello Azure-portalen klickar du på länken hello under **URL**.</span><span class="sxs-lookup"><span data-stu-id="ef145-179">In a browser go toohello URL of hello API app: in hello **API app** blade in hello Azure portal, click hello link under **URL**.</span></span>  
   
    <span data-ttu-id="ef145-180">Du är omdirigerade tooa inloggningsskärm eftersom oautentiserade begäranden inte tillåts tooreach hello API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-180">You are redirected tooa login screen because unauthenticated requests are not allowed tooreach hello API app.</span></span>
   
    <span data-ttu-id="ef145-181">Om din webbläsare går toohello ”har skapats”-sida kan hello webbläsaren kanske redan har loggat på--i så fall öppna en InPrivate- eller Incognito-fönster och gå toohello API app-URL.</span><span class="sxs-lookup"><span data-stu-id="ef145-181">If your browser goes toohello "successfully created" page, hello browser might already be logged on -- in that case, open an InPrivate or Incognito window and go toohello API app's URL.</span></span>
2. <span data-ttu-id="ef145-182">Logga in med autentiseringsuppgifterna för en användare i din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="ef145-182">Log on using credentials for a user in your Azure AD tenant.</span></span>
   
    <span data-ttu-id="ef145-183">När du är inloggad, visas hello ”har skapats” i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ef145-183">When you're logged on, hello "successfully created" page appears in hello browser.</span></span>
3. <span data-ttu-id="ef145-184">Stäng hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ef145-184">Close hello browser.</span></span>

### <a name="configure-hello-azure-ad-application"></a><span data-ttu-id="ef145-185">Konfigurera hello Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="ef145-185">Configure hello Azure AD application</span></span>
<span data-ttu-id="ef145-186">När du har konfigurerat Azure AD authentication skapats Apptjänst ett Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="ef145-186">When you configured Azure AD authentication, App Service created an Azure AD application for you.</span></span> <span data-ttu-id="ef145-187">Som standard var hello nya Azure AD-program konfigurerade tooprovide hello ägar-token toohello API-appens URL.</span><span class="sxs-lookup"><span data-stu-id="ef145-187">By default hello new Azure AD application was configured tooprovide hello bearer token toohello API app's URL.</span></span> <span data-ttu-id="ef145-188">I det här avsnittet kan du konfigurera hello Azure AD programmet tooprovide hello ägar-token toohello AngularJS klientdelen i stället för direkt toohello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-188">In this section you configure hello Azure AD application tooprovide hello bearer token toohello AngularJS front end instead of directly toohello middle tier API app.</span></span> <span data-ttu-id="ef145-189">Hej AngularJS-klient skickar hello token toohello API-app när den anropar hello API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-189">hello AngularJS front end will send hello token toohello API app when it calls hello API app.</span></span>

> [!NOTE]
> <span data-ttu-id="ef145-190">den här kursen används ett enda Azure AD tookeep hello processen så enkel som möjligt, för både hello klientdelen och hello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="ef145-190">tookeep hello process as simple as possible, this tutorial uses a single Azure AD application for both hello front end and hello middle tier API app.</span></span> <span data-ttu-id="ef145-191">Ett annat alternativ är toouse två Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="ef145-191">Another option is toouse two Azure AD applications.</span></span> <span data-ttu-id="ef145-192">Du måste i så fall toogive hello klientdelens Azure AD application behörighet toocall hello Mellannivås Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="ef145-192">In that case you would have toogive hello front end's Azure AD application permission toocall hello middle tier's Azure AD application.</span></span> <span data-ttu-id="ef145-193">Den här metoden för flera program ger mer detaljerad kontroll över behörigheter tooeach nivå.</span><span class="sxs-lookup"><span data-stu-id="ef145-193">This multi-application approach would give you more granular control over permissions tooeach tier.</span></span>
> 
> 

1. <span data-ttu-id="ef145-194">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), gå för**Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ef145-194">In hello [Azure classic portal](https://manage.windowsazure.com/), go too**Azure Active Directory**.</span></span>
   
   <span data-ttu-id="ef145-195">Du har toouse hello klassiska portalen eftersom vissa Azure Active Directory-inställningar som du behöver komma åt tooare ännu inte är tillgänglig i hello aktuella Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ef145-195">You have toouse hello classic portal because certain Azure Active Directory settings that you need access tooare not yet available in hello current Azure portal.</span></span>
2. <span data-ttu-id="ef145-196">På hello **Directory** klickar du på AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="ef145-196">On hello **Directory** tab, click your AAD tenant.</span></span>
   
   ![Azure AD i klassiska portalen](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. <span data-ttu-id="ef145-198">Klicka på **program > program som företaget äger**, och klicka sedan på hello är markerat.</span><span class="sxs-lookup"><span data-stu-id="ef145-198">Click **Applications > Applications my company owns**, and then click hello check mark.</span></span>
   
   <span data-ttu-id="ef145-199">Du kan också ha toorefresh hello sidan toosee hello nytt program.</span><span class="sxs-lookup"><span data-stu-id="ef145-199">You might also have toorefresh hello page toosee hello new application.</span></span>
4. <span data-ttu-id="ef145-200">Klicka på hello som Azure skapade när du har aktiverat autentisering för API-appen hello namn i hello listan med program.</span><span class="sxs-lookup"><span data-stu-id="ef145-200">In hello list of applications, click hello name of hello one that Azure created for you when you enabled authentication for your API app.</span></span>
   
   ![Fliken för Azure AD-program](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. <span data-ttu-id="ef145-202">Klicka på **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ef145-202">Click **Configure**.</span></span>
   
   ![Azure AD-konfigurera fliken](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. <span data-ttu-id="ef145-204">Ange **inloggnings-URL** toohello URL: en för webbappen AngularJS, utan avslutande snedstreck.</span><span class="sxs-lookup"><span data-stu-id="ef145-204">Set **Sign-on URL** toohello URL for your AngularJS web app, no trailing slash.</span></span>
   
   <span data-ttu-id="ef145-205">Till exempel: https://todolistangular.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="ef145-205">For example: https://todolistangular.azurewebsites.net</span></span>
   
   ![Inloggnings-URL](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. <span data-ttu-id="ef145-207">Ange **Reply URL** toohello URL: en för ditt webbprogram, utan avslutande snedstreck.</span><span class="sxs-lookup"><span data-stu-id="ef145-207">Set **Reply URL** toohello URL for your web app, no trailing slash.</span></span>
   
   <span data-ttu-id="ef145-208">Till exempel: https://todolistsangular.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="ef145-208">For example: https://todolistsangular.azurewebsites.net</span></span>
8. <span data-ttu-id="ef145-209">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ef145-209">Click **Save**.</span></span>
   
   ![Reply-URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. <span data-ttu-id="ef145-211">Hello längst hello-sidan, klickar du på **hantera manifestet > hämta manifestet**.</span><span class="sxs-lookup"><span data-stu-id="ef145-211">At hello bottom of hello page, click **Manage manifest > Download manifest**.</span></span>
   
   ![Hämta manifestet](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. <span data-ttu-id="ef145-213">Hämta hello tooa plats där du kan redigera den.</span><span class="sxs-lookup"><span data-stu-id="ef145-213">Download hello file tooa location where you can edit it.</span></span>
11. <span data-ttu-id="ef145-214">I hello hämtas manifestfilen, söker du efter hello `oauth2AllowImplicitFlow` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ef145-214">In hello downloaded manifest file, search for hello  `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="ef145-215">Ändra hello värdet på denna egenskap från `false` för`true`, och sedan spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="ef145-215">Change hello value of this property from `false` too`true`, and then save hello file.</span></span>
    
    <span data-ttu-id="ef145-216">Den här inställningen krävs för åtkomst från en enda sida JavaScript-programmet.</span><span class="sxs-lookup"><span data-stu-id="ef145-216">This setting is required for access from a JavaScript single-page application.</span></span> <span data-ttu-id="ef145-217">Den gör hello Oauth 2.0 ägar-token toobe returneras i hello URL-fragment.</span><span class="sxs-lookup"><span data-stu-id="ef145-217">It enables hello Oauth 2.0 bearer token toobe returned in hello URL fragment.</span></span>
12. <span data-ttu-id="ef145-218">Klicka på **hantera manifestet > Överför manifestet**, och överför hello-fil som du har uppdaterat i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ef145-218">Click **Manage manifest > Upload manifest**, and upload hello file that you updated in hello preceding step.</span></span>
    
    ![Överför manifest](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. <span data-ttu-id="ef145-220">Kopiera hello **klient-ID** värde och spara den någonstans du hämtar det senare.</span><span class="sxs-lookup"><span data-stu-id="ef145-220">Copy hello **Client ID** value and save it somewhere you can get it from later.</span></span>

## <a name="configure-hello-todolistangular-project-toouse-authentication"></a><span data-ttu-id="ef145-221">Konfigurera hello ToDoListAngular-projektet toouse autentisering</span><span class="sxs-lookup"><span data-stu-id="ef145-221">Configure hello ToDoListAngular project toouse authentication</span></span>
<span data-ttu-id="ef145-222">I det här avsnittet Ändra hello AngularJS klientdelen så att den använder Active Directory Authentication Library (ADAL) för JS tooacquire ett ägartoken för hello inloggade användare från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ef145-222">In this section you change hello AngularJS front end so that it uses Active Directory Authentication Library (ADAL) for JS tooacquire a bearer token for hello logged-on user from Azure AD.</span></span> <span data-ttu-id="ef145-223">hello-koden innehåller hello token i HTTP-begäranden skickas toohello mellannivå, som visas i följande diagram hello.</span><span class="sxs-lookup"><span data-stu-id="ef145-223">hello code will include hello token in HTTP requests sent toohello middle tier, as shown in hello following diagram.</span></span> 

![Diagram för autentisering av användare](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

<span data-ttu-id="ef145-225">Gör följande ändringar toofiles i hello ToDoListAngular-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="ef145-225">Make hello following changes toofiles in hello ToDoListAngular project.</span></span>

1. <span data-ttu-id="ef145-226">Öppna hello *index.cshtml* fil.</span><span class="sxs-lookup"><span data-stu-id="ef145-226">Open hello *index.cshtml* file.</span></span>
2. <span data-ttu-id="ef145-227">Ta bort kommentarerna hello rader som refererar till hello Active Directory Authentication Library (ADAL) för JS-skript.</span><span class="sxs-lookup"><span data-stu-id="ef145-227">Uncomment hello lines that reference hello Active Directory Authentication Library (ADAL) for JS scripts.</span></span>
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. <span data-ttu-id="ef145-228">Öppna hello *app/scripts/app.js* fil.</span><span class="sxs-lookup"><span data-stu-id="ef145-228">Open hello *app/scripts/app.js* file.</span></span>
4. <span data-ttu-id="ef145-229">Kommentera hello kodblocket markerats för ”utan autentisering” och Avkommentera hello kodblocket markerats för ”med autentisering”.</span><span class="sxs-lookup"><span data-stu-id="ef145-229">Comment out hello block of code marked for "without authentication" and uncomment hello block of code marked for "with authentication".</span></span>
   
    <span data-ttu-id="ef145-230">Den här ändringen refererar till hello ADAL JS autentiseringsprovider och ger configuration värden tooit.</span><span class="sxs-lookup"><span data-stu-id="ef145-230">This change references hello ADAL JS authentication provider and provides configuration values tooit.</span></span> <span data-ttu-id="ef145-231">Du kan ange hello konfigurationsvärden för API-appen och Azure AD-program i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="ef145-231">In hello following steps you set hello configuration values for your API app and Azure AD application.</span></span>
5. <span data-ttu-id="ef145-232">I hello-kod som anger hello `endpoints` hello variabeln, ange URL för API-toohello URL för hello API-app som du skapade för hello ToDoListAPI-projektet och ange hello Azure AD-program-ID toohello klient-ID som du kopierade från hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ef145-232">In hello code that sets hello `endpoints` variable, set hello API URL toohello URL of hello API app that you created for hello ToDoListAPI project, and set hello Azure AD application ID toohello client ID that you copied from hello Azure classic portal.</span></span>
   
    <span data-ttu-id="ef145-233">hello koden är nu liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="ef145-233">hello code is now similar toohello following example.</span></span>
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. <span data-ttu-id="ef145-234">Hello anropa för`adalProvider.init`, ange `tenant` tooyour klientnamn och `clientId` toosame-värde som du använde i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ef145-234">In hello call too`adalProvider.init`, set `tenant` tooyour tenant name and `clientId` toosame value you used in hello previous step.</span></span>
   
    <span data-ttu-id="ef145-235">hello koden är nu liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="ef145-235">hello code is now similar toohello following example.</span></span>
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    <span data-ttu-id="ef145-236">Dessa ändringar för`app.js` anger att hello anropande koden och hello kallas API i hello samma Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="ef145-236">These changes too`app.js` specify that hello calling code and hello called API are in hello same Azure AD application.</span></span>
7. <span data-ttu-id="ef145-237">Öppna hello *app/scripts/homeCtrl.js* fil.</span><span class="sxs-lookup"><span data-stu-id="ef145-237">Open hello *app/scripts/homeCtrl.js* file.</span></span>
8. <span data-ttu-id="ef145-238">Kommentera hello kodblocket markerats för ”utan autentisering” och Avkommentera hello kodblocket markerats för ”med autentisering”.</span><span class="sxs-lookup"><span data-stu-id="ef145-238">Comment out hello block of code marked for "without authentication" and uncomment hello block of code marked for "with authentication".</span></span>
9. <span data-ttu-id="ef145-239">Öppna hello *app/scripts/indexCtrl.js* fil.</span><span class="sxs-lookup"><span data-stu-id="ef145-239">Open hello *app/scripts/indexCtrl.js* file.</span></span>
10. <span data-ttu-id="ef145-240">Kommentera hello kodblocket markerats för ”utan autentisering” och Avkommentera hello kodblocket markerats för ”med autentisering”.</span><span class="sxs-lookup"><span data-stu-id="ef145-240">Comment out hello block of code marked for "without authentication" and uncomment hello block of code marked for "with authentication".</span></span>

### <a name="deploy-hello-todolistangular-project-tooazure"></a><span data-ttu-id="ef145-241">Distribuera hello ToDoListAngular-projektet tooAzure</span><span class="sxs-lookup"><span data-stu-id="ef145-241">Deploy hello ToDoListAngular project tooAzure</span></span>
1. <span data-ttu-id="ef145-242">I **Solution Explorer**, högerklicka på hello ToDoListAngular-projektet och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="ef145-242">In **Solution Explorer**, right-click hello ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="ef145-243">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ef145-243">Click **Publish**.</span></span>
   
    <span data-ttu-id="ef145-244">Visual Studio distribuerar hello-projektet och öppnar en webbläsare toohello webbprogram bas-URL.</span><span class="sxs-lookup"><span data-stu-id="ef145-244">Visual Studio deploys hello project and opens a browser toohello web app's base URL.</span></span> <span data-ttu-id="ef145-245">Det här alternativet visas sidan 403-fel, vilket är normalt för ett försök toogo tooa Web API bas-URL från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ef145-245">This will show a 403 error page, which is normal for an attempt toogo tooa Web API base URL from a browser.</span></span>
   
    <span data-ttu-id="ef145-246">Du har fortfarande toomake en ändring toohello mellannivå-API-app innan du kan testa programmet hello.</span><span class="sxs-lookup"><span data-stu-id="ef145-246">You still have toomake a change toohello middle tier API app before you can test hello application.</span></span>
3. <span data-ttu-id="ef145-247">Stäng hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ef145-247">Close hello browser.</span></span>

## <a name="configure-hello-todolistapi-project-toouse-authentication"></a><span data-ttu-id="ef145-248">Konfigurera hello ToDoListAPI-projektet toouse autentisering</span><span class="sxs-lookup"><span data-stu-id="ef145-248">Configure hello ToDoListAPI project toouse authentication</span></span>
<span data-ttu-id="ef145-249">För närvarande hello ToDoListAPI-projektet skickar ”*” som hello `owner` värdet tooToDoListDataAPI.</span><span class="sxs-lookup"><span data-stu-id="ef145-249">Currently hello ToDoListAPI project sends "*" as hello `owner` value tooToDoListDataAPI.</span></span> <span data-ttu-id="ef145-250">I det här avsnittet kan du ändra hello kod toosend hello-ID för hello inloggade användare.</span><span class="sxs-lookup"><span data-stu-id="ef145-250">In this section you change hello code toosend hello ID of hello logged-on user.</span></span>

<span data-ttu-id="ef145-251">Gör följande ändringar i hello ToDoListAPI-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="ef145-251">Make hello following changes in hello ToDoListAPI project.</span></span>

1. <span data-ttu-id="ef145-252">Öppna hello *Controllers/ToDoListController.cs* filen och Avkommentera hello rad i varje åtgärdsmetod som anger `owner` toohello Azure AD `NameIdentifier` anspråksvärde.</span><span class="sxs-lookup"><span data-stu-id="ef145-252">Open hello *Controllers/ToDoListController.cs* file, and uncomment hello line in each action method that sets `owner` toohello Azure AD `NameIdentifier` claim value.</span></span> <span data-ttu-id="ef145-253">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ef145-253">For example:</span></span>
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    <span data-ttu-id="ef145-254">**Viktiga**: inte ta bort kommentarerna koden i hello `ToDoListDataAPI` metoden; gör du det senare hello service principal autentisering genomgång.</span><span class="sxs-lookup"><span data-stu-id="ef145-254">**Important**: Don't uncomment code in hello `ToDoListDataAPI` method; you'll do that later for hello service principal authentication tutorial.</span></span>

### <a name="deploy-hello-todolistapi-project-tooazure"></a><span data-ttu-id="ef145-255">Distribuera hello ToDoListAPI-projektet tooAzure</span><span class="sxs-lookup"><span data-stu-id="ef145-255">Deploy hello ToDoListAPI project tooAzure</span></span>
1. <span data-ttu-id="ef145-256">I **Solution Explorer**, högerklicka på hello ToDoListAPI-projektet och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="ef145-256">In **Solution Explorer**, right-click hello ToDoListAPI project, and then click **Publish**.</span></span>
2. <span data-ttu-id="ef145-257">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="ef145-257">Click **Publish**.</span></span>
   
    <span data-ttu-id="ef145-258">Visual Studio distribuerar hello-projektet och öppnar en webbläsare toohello API-appens bas-URL.</span><span class="sxs-lookup"><span data-stu-id="ef145-258">Visual Studio deploys hello project and opens a browser toohello API app's base URL.</span></span>
3. <span data-ttu-id="ef145-259">Stäng hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ef145-259">Close hello browser.</span></span>

### <a name="test-hello-application"></a><span data-ttu-id="ef145-260">Testa programmet hello</span><span class="sxs-lookup"><span data-stu-id="ef145-260">Test hello application</span></span>
1. <span data-ttu-id="ef145-261">Gå toohello URL för hello webbapp **med hjälp av HTTPS, inte HTTP**.</span><span class="sxs-lookup"><span data-stu-id="ef145-261">Go toohello URL of hello web app, **using HTTPS, not HTTP**.</span></span>
2. <span data-ttu-id="ef145-262">Klicka på hello **tooDo lista** fliken.</span><span class="sxs-lookup"><span data-stu-id="ef145-262">Click hello **tooDo List** tab.</span></span>
   
    <span data-ttu-id="ef145-263">Du kan ange toolog i.</span><span class="sxs-lookup"><span data-stu-id="ef145-263">You are prompted toolog in.</span></span>
3. <span data-ttu-id="ef145-264">Logga in med hello autentiseringsuppgifterna för en användare i din AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="ef145-264">Log in with hello credentials of a user in your AAD tenant.</span></span>
4. <span data-ttu-id="ef145-265">Hej **tooDo listan** visas.</span><span class="sxs-lookup"><span data-stu-id="ef145-265">hello **tooDo List** page appears.</span></span>
   
   ![sidan tooDo](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   <span data-ttu-id="ef145-267">Inga arbetsuppgifter visas eftersom fram till nu de alla har ägaren av ”*”.</span><span class="sxs-lookup"><span data-stu-id="ef145-267">No to-do items are displayed because until now they have all been for owner "*".</span></span> <span data-ttu-id="ef145-268">Nu hello mellannivå begär objekt för hello inloggad användare och ingen har skapats ännu.</span><span class="sxs-lookup"><span data-stu-id="ef145-268">Now hello middle tier is requesting items for hello logged-on user, and none have been created yet.</span></span>
5. <span data-ttu-id="ef145-269">Lägga till nya att göra objekten tooverify som programmet hello fungerar.</span><span class="sxs-lookup"><span data-stu-id="ef145-269">Add new to-do items tooverify that hello application is working.</span></span>
6. <span data-ttu-id="ef145-270">Gå toohello Swagger UI-URL för hello ToDoListDataAPI API-app i ett nytt webbläsarfönster och klicka på **ToDoList > hämta**.</span><span class="sxs-lookup"><span data-stu-id="ef145-270">In another browser window, go toohello Swagger UI URL for hello ToDoListDataAPI API app and click **ToDoList > Get**.</span></span> <span data-ttu-id="ef145-271">Ange en asterisk för hello `owner` parameter och klicka sedan på **prova**.</span><span class="sxs-lookup"><span data-stu-id="ef145-271">Enter an asterisk for hello `owner` parameter, and then click **Try it out**.</span></span>
   
   <span data-ttu-id="ef145-272">hello svaret visar att hello nya arbetsuppgifter har hello faktiska Azure AD användar-ID i hello ägare egenskap.</span><span class="sxs-lookup"><span data-stu-id="ef145-272">hello response shows that hello new to-do items have hello actual Azure AD user ID in hello Owner property.</span></span>
   
   ![Ägar-ID i JSON-svar](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-hello-projects-from-scratch"></a><span data-ttu-id="ef145-274">Skapa hello projekt från grunden</span><span class="sxs-lookup"><span data-stu-id="ef145-274">Building hello projects from scratch</span></span>
<span data-ttu-id="ef145-275">hello två Web API-projekt har skapats med hjälp av hello **Azure API Apps** mall och ersätta hello standard värden controller med en ToDoList-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="ef145-275">hello two Web API projects were created by using hello **Azure API App** project template and replacing hello default Values controller with a ToDoList controller.</span></span> 

<span data-ttu-id="ef145-276">För information om hur skapar för ett program för AngularJS-sida med en serverdel för Web API 2, se [händerna på Lab: skapa en enkel sida program (SPA) med ASP.NET Web API och Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs).</span><span class="sxs-lookup"><span data-stu-id="ef145-276">For information about how too create an AngularJS single-page application with a Web API 2 back end, see  [Hands On Lab: Build a Single Page Application (SPA) with ASP.NET Web API and Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs).</span></span> <span data-ttu-id="ef145-277">Information om hur tooadd koden för Azure AD finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="ef145-277">For information about how tooadd Azure AD authentication code, see hello following resources:</span></span>

* <span data-ttu-id="ef145-278">[Skydda AngularJS enda sidan appar med Azure AD](../active-directory/active-directory-devquickstarts-angular.md).</span><span class="sxs-lookup"><span data-stu-id="ef145-278">[Securing AngularJS Single Page Apps with Azure AD](../active-directory/active-directory-devquickstarts-angular.md).</span></span>
* [<span data-ttu-id="ef145-279">Introduktion till ADAL JS v1</span><span class="sxs-lookup"><span data-stu-id="ef145-279">Introducing ADAL JS v1</span></span>](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a><span data-ttu-id="ef145-280">Felsökning</span><span class="sxs-lookup"><span data-stu-id="ef145-280">Troubleshooting</span></span>
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* <span data-ttu-id="ef145-281">Kontrollera att du blanda inte ihop ToDoListAPI (mellannivå) och ToDoListDataAPI (datanivå).</span><span class="sxs-lookup"><span data-stu-id="ef145-281">Make sure that you don't confuse ToDoListAPI (middle tier) and ToDoListDataAPI (data tier).</span></span> <span data-ttu-id="ef145-282">Till exempel kontrollera att du lagt till autentisering toohello mellannivå-API-app, inte hello datanivå.</span><span class="sxs-lookup"><span data-stu-id="ef145-282">For example, verify that you added authentication toohello middle tier API app, not hello data tier.</span></span> 
* <span data-ttu-id="ef145-283">Kontrollera att hello AngularJS källa kod referenser hello mellannivå API app-URL (ToDoListAPI, inte ToDoListDataAPI) och hello korrekt Azure AD-klient-ID.</span><span class="sxs-lookup"><span data-stu-id="ef145-283">Make sure that hello AngularJS source code references hello middle tier API app URL (ToDoListAPI, not ToDoListDataAPI)and hello correct Azure AD client ID.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ef145-284">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef145-284">Next steps</span></span>
<span data-ttu-id="ef145-285">I den här självstudiekursen du lärt dig hur toouse App Service-autentisering för en API-app och hur toocall hello API-app med hjälp av hello JS ADAL-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="ef145-285">In this tutorial you learned how toouse App Service authentication for an API app and how toocall hello API app by using hello ADAL JS library.</span></span> <span data-ttu-id="ef145-286">I nästa kurs i hello lär du dig hur för[säker åtkomst tooyour API-app för service to service scenarier](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="ef145-286">In hello next tutorial you'll learn how too[secure access tooyour API app for service-to-service scenarios](app-service-api-dotnet-service-principal-auth.md).</span></span>

