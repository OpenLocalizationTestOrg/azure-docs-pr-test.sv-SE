---
title: "aaaGet igång med Azure AD-autentisering med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toouse hello Azure portal tooaccess Azure Active Directory (AD Azure) autentisering tooconsume hello Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 497ad1806b3fd1262802adefb6e12b65ee9f2d61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-hello-azure-portal"></a><span data-ttu-id="0dd48-103">Komma igång med Azure AD-autentisering med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0dd48-103">Get started with Azure AD authentication by using hello Azure portal</span></span>

<span data-ttu-id="0dd48-104">Lär dig hur toouse hello Azure portal tooaccess Azure Active Directory (AD Azure) autentisering tooaccess hello Azure Media Services API.</span><span class="sxs-lookup"><span data-stu-id="0dd48-104">Learn how toouse hello Azure portal tooaccess Azure Active Directory (Azure AD) authentication tooaccess hello Azure Media Services API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0dd48-105">Krav</span><span class="sxs-lookup"><span data-stu-id="0dd48-105">Prerequisites</span></span>

- <span data-ttu-id="0dd48-106">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0dd48-106">An Azure account.</span></span> <span data-ttu-id="0dd48-107">Om du inte har ett konto, börja med en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0dd48-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="0dd48-108">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="0dd48-108">A Media Services account.</span></span> <span data-ttu-id="0dd48-109">Mer information finns i [skapa ett Azure Media Services-konto med hjälp av hello Azure-portalen](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-109">For more information, see [Create an Azure Media Services account by using hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="0dd48-110">Kontrollera att du granskar hello [åtkomst till Azure Media Services API med Azure AD-autentiseringsöversikt](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-110">Make sure you review hello [Accessing Azure Media Services API with Azure AD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="0dd48-111">När du använder Azure AD-autentisering med Azure Media Services, har du två alternativ för autentisering:</span><span class="sxs-lookup"><span data-stu-id="0dd48-111">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="0dd48-112">**Användarautentisering**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-112">**User authentication**.</span></span> <span data-ttu-id="0dd48-113">Autentisera en person som använder hello app toointeract med Media Services-resurser.</span><span class="sxs-lookup"><span data-stu-id="0dd48-113">Authenticate a person who is using hello app toointeract with Media Services resources.</span></span> <span data-ttu-id="0dd48-114">hello interaktiva program bör först fråga hello användaren om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0dd48-114">hello interactive application should first prompt hello user for credentials.</span></span> <span data-ttu-id="0dd48-115">Ett exempel är en management konsolapp som används av behöriga användare toomonitor kodning jobb eller live strömning.</span><span class="sxs-lookup"><span data-stu-id="0dd48-115">An example is a management console app used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="0dd48-116">**Huvudnamn autentiseringen av tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-116">**Service principal authentication**.</span></span> <span data-ttu-id="0dd48-117">Autentisera en tjänst.</span><span class="sxs-lookup"><span data-stu-id="0dd48-117">Authenticate a service.</span></span> <span data-ttu-id="0dd48-118">Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagda jobb: webbappar, funktionen appar, logikappar, API: er eller en mikrotjänster.</span><span class="sxs-lookup"><span data-stu-id="0dd48-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs: web apps, function apps, logic apps, APIs, or a microservice.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0dd48-119">Media Services stöder för närvarande hello Azure Access Control service-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0dd48-119">Currently, Media Services supports hello Azure Access Control service authentication model.</span></span> <span data-ttu-id="0dd48-120">Access Control tillstånd att bli inaktuell på den 1 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="0dd48-120">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="0dd48-121">Vi rekommenderar att du migrerar toohello Azure AD-autentisering så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="0dd48-121">We recommend that you migrate toohello Azure AD authentication model as soon as possible.</span></span>

## <a name="select-hello-authentication-method"></a><span data-ttu-id="0dd48-122">Välj autentiseringsmetod för hello</span><span class="sxs-lookup"><span data-stu-id="0dd48-122">Select hello authentication method</span></span>

1. <span data-ttu-id="0dd48-123">I hello [Azure-portalen](https://portal.azure.com/), Välj Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="0dd48-123">In hello [Azure portal](https://portal.azure.com/), select your Media Services account.</span></span>
2. <span data-ttu-id="0dd48-124">Välj hur tooconnect toohello Media Services API.</span><span class="sxs-lookup"><span data-stu-id="0dd48-124">Select how tooconnect toohello Media Services API.</span></span>

    ![Välj metoden anslutningssidan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a><span data-ttu-id="0dd48-126">Användarautentisering</span><span class="sxs-lookup"><span data-stu-id="0dd48-126">User authentication</span></span>

<span data-ttu-id="0dd48-127">tooconnect toohello Media Services API med hjälp av Hej alternativet för autentisering av användare, hello-klientappen måste toorequest en Azure AD-token har hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="0dd48-127">tooconnect toohello Media Services API by using hello user authentication option, hello client app needs toorequest an Azure AD token that has hello following parameters:</span></span>  

* <span data-ttu-id="0dd48-128">Azure AD-klient slutpunkt</span><span class="sxs-lookup"><span data-stu-id="0dd48-128">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="0dd48-129">Media Services resurs-URI</span><span class="sxs-lookup"><span data-stu-id="0dd48-129">Media Services resource URI</span></span>
* <span data-ttu-id="0dd48-130">Klient-ID för Media Services (intern)-program</span><span class="sxs-lookup"><span data-stu-id="0dd48-130">Media Services (native) application client ID</span></span> 
* <span data-ttu-id="0dd48-131">Media Services (intern)-programmet omdirigerings-URI</span><span class="sxs-lookup"><span data-stu-id="0dd48-131">Media Services (native) application redirect URI</span></span> 
* <span data-ttu-id="0dd48-132">Resurs-URI för REST Media Services</span><span class="sxs-lookup"><span data-stu-id="0dd48-132">Resource URI for REST Media Services</span></span>

<span data-ttu-id="0dd48-133">Du kan hämta hello värden för parametrarna på hello **Media Services-API med användarautentisering med** sidan.</span><span class="sxs-lookup"><span data-stu-id="0dd48-133">You can get hello values for these parameters on hello **Media Services API with user authentication** page.</span></span> 

![Ansluta till sidan för autentisering av användare](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

<span data-ttu-id="0dd48-135">Om du ansluter toohello Media Services API med hjälp av hello Media Services Microsoft .NET SDK krävs hello värden är tillgänglig tooyou som en del av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="0dd48-135">If you connect toohello Media Services API by using hello Media Services Microsoft .NET SDK, hello required values are available tooyou as part of hello SDK.</span></span> <span data-ttu-id="0dd48-136">Mer information finns i [använda Azure AD authentication tooaccess hello Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-136">For more information, see [Use Azure AD authentication tooaccess hello Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="0dd48-137">Om du inte använder klient-hello Media Services .NET SDK, måste du manuellt skapa en begäran om Azure AD-token med hjälp av hello parametrar som nämnts tidigare.</span><span class="sxs-lookup"><span data-stu-id="0dd48-137">If you're not using hello Media Services .NET client SDK, you must manually create an Azure AD token request by using hello parameters discussed earlier.</span></span> <span data-ttu-id="0dd48-138">Mer information finns i [hur toouse hello Azure AD Authentication Library tooget hello Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-138">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="service-principal-authentication"></a><span data-ttu-id="0dd48-139">Autentisering av tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="0dd48-139">Service principal authentication</span></span>

<span data-ttu-id="0dd48-140">tooconnect toohello Media Services API med hjälp av Hej service principal alternativet, appen mellannivå (web API eller webbprogram) måste toorequest en Azure AD-token har hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="0dd48-140">tooconnect toohello Media Services API by using hello service principal option, your middle-tier app (web API or web application) needs toorequest an Azure AD token that has hello following parameters:</span></span>  

* <span data-ttu-id="0dd48-141">Azure AD-klient slutpunkt</span><span class="sxs-lookup"><span data-stu-id="0dd48-141">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="0dd48-142">Media Services resurs-URI</span><span class="sxs-lookup"><span data-stu-id="0dd48-142">Media Services resource URI</span></span> 
* <span data-ttu-id="0dd48-143">Resurs-URI för REST Media Services</span><span class="sxs-lookup"><span data-stu-id="0dd48-143">Resource URI for REST Media Services</span></span>
* <span data-ttu-id="0dd48-144">Azure AD application värden: hello **klient-ID** och **klienthemlighet**</span><span class="sxs-lookup"><span data-stu-id="0dd48-144">Azure AD application values: hello **client ID** and **client secret**</span></span>

<span data-ttu-id="0dd48-145">Du kan hämta hello värden för parametrarna på hello **ansluta tooMedia Services API med tjänstens huvudnamn** sidan.</span><span class="sxs-lookup"><span data-stu-id="0dd48-145">You can get hello values for these parameters on hello **Connect tooMedia Services API with service principal** page.</span></span> <span data-ttu-id="0dd48-146">Använd den här sidan toocreate en ny Azure AD-program eller tooselect en befintlig.</span><span class="sxs-lookup"><span data-stu-id="0dd48-146">Use this page toocreate a new Azure AD application or tooselect an existing one.</span></span> <span data-ttu-id="0dd48-147">När du har valt hello Azure AD-appar kan du hämta hello klient-ID (program-ID) och skapa hello klienten hemligheten (nyckel) värden.</span><span class="sxs-lookup"><span data-stu-id="0dd48-147">After you select hello Azure AD app, you can get hello client ID (Application ID) and generate hello client secret (key) values.</span></span> 

![Ansluta till service principal sida](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

<span data-ttu-id="0dd48-149">När hello **tjänstens huvudnamn** blad öppnas, hello första Azure AD-program som uppfyller följande kriterier hello väljs:</span><span class="sxs-lookup"><span data-stu-id="0dd48-149">When hello **Service Principal** blade opens, hello first Azure AD application that meets hello following criteria is selected:</span></span>

- <span data-ttu-id="0dd48-150">Det är en registrerad Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="0dd48-150">It is a registered Azure AD application.</span></span>
- <span data-ttu-id="0dd48-151">Det har behörighet deltagare eller Owner Role-Based åtkomstkontroll på hello-konto.</span><span class="sxs-lookup"><span data-stu-id="0dd48-151">It has Contributor or Owner Role-Based Access Control permissions on hello account.</span></span>

<span data-ttu-id="0dd48-152">När du skapar eller välj en Azure AD-app, kan du skapa och kopiera en klienthemlighet (nyckel) och hello klient-ID (program-ID).</span><span class="sxs-lookup"><span data-stu-id="0dd48-152">After you create or select an Azure AD app, you can create and copy a client secret (key) and hello client ID (Application ID).</span></span> <span data-ttu-id="0dd48-153">hello-klient hemlighet och klient-ID är obligatoriska tooget hello åtkomst-token i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="0dd48-153">hello client secret and client ID are required tooget hello access token in this scenario.</span></span>

<span data-ttu-id="0dd48-154">Om du inte har behörigheter toocreate Azure AD-appar i din domän, hello Azure AD app kontroller av hello bladet visas inte och visas ett varningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0dd48-154">If you don't have permissions toocreate Azure AD apps in your domain, hello Azure AD app controls of hello blade are not shown, and a warning message is displayed.</span></span>

<span data-ttu-id="0dd48-155">Om du ansluter toohello Media Services API med hjälp av hello Media Services .NET SDK, se [använda Azure AD authentication tooaccess hello Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-155">If you connect toohello Media Services API by using hello Media Services .NET SDK, see [Use Azure AD authentication tooaccess hello Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="0dd48-156">Om du inte använder klient-hello Media Services .NET SDK, måste du manuellt skapa en Azure AD-tokenbegäran med hello parametrar som nämnts tidigare.</span><span class="sxs-lookup"><span data-stu-id="0dd48-156">If you are not using hello Media Services .NET client SDK, you must manually create an Azure AD token request using hello parameters discussed earlier.</span></span> <span data-ttu-id="0dd48-157">Mer information finns i [hur toouse hello Azure AD Authentication Library tooget hello Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-157">For more information, see [How toouse hello Azure AD Authentication Library tooget hello Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="get-hello-client-id-and-client-secret"></a><span data-ttu-id="0dd48-158">Hämta hello klient-ID och klienten hemlighet</span><span class="sxs-lookup"><span data-stu-id="0dd48-158">Get hello client ID and client secret</span></span>

<span data-ttu-id="0dd48-159">När du väljer en befintlig Azure AD app eller välj hello alternativet toocreate en ny, visas hello följande knappar:</span><span class="sxs-lookup"><span data-stu-id="0dd48-159">After you select an existing Azure AD app or select hello option toocreate a new one, hello following buttons appear:</span></span>

![Hantera behörigheter och hantera program](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

<span data-ttu-id="0dd48-161">tooopen hello Azure AD application-bladet, klickar du på **hantera program**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-161">tooopen hello Azure AD application blade, click **Manage application**.</span></span> <span data-ttu-id="0dd48-162">På hello **hantera program** bladet kan du hämta hello appens klient-ID (program-ID).</span><span class="sxs-lookup"><span data-stu-id="0dd48-162">On hello **Manage application** blade, you can get hello app's client ID (Application ID).</span></span> <span data-ttu-id="0dd48-163">toogenerate en klienthemlighet (nyckel), Välj **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-163">toogenerate a client secret (key), select **Keys**.</span></span>

![Hantera program bladet nycklar alternativet](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-hello-application"></a><span data-ttu-id="0dd48-165">Hantera behörigheter och hello program</span><span class="sxs-lookup"><span data-stu-id="0dd48-165">Manage permissions and hello application</span></span>

<span data-ttu-id="0dd48-166">När du har valt hello Azure AD-program kan du hantera hello program och behörigheter.</span><span class="sxs-lookup"><span data-stu-id="0dd48-166">After you select hello Azure AD application, you can manage hello application and permissions.</span></span> <span data-ttu-id="0dd48-167">tooset in din Azure AD application tooaccess andra program klickar du på **hantera behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-167">tooset up your Azure AD application tooaccess other applications, click **Manage permissions**.</span></span> <span data-ttu-id="0dd48-168">För hanteringsuppgifter, till exempel ändra nycklar och svars-URL: er eller tooedit hello programmets manifest, klicka på **hantera program**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-168">For management tasks, such as changing keys and reply URLs, or tooedit hello application’s manifest, click **Manage application**.</span></span>

### <a name="edit-hello-apps-settings-or-manifest"></a><span data-ttu-id="0dd48-169">Redigera inställningar för hello app eller manifest</span><span class="sxs-lookup"><span data-stu-id="0dd48-169">Edit hello app's settings or manifest</span></span>

<span data-ttu-id="0dd48-170">tooedit hello app inställningar eller manifestet, klickar du på **hantera program**.</span><span class="sxs-lookup"><span data-stu-id="0dd48-170">tooedit hello app's settings or manifest, click **Manage application**.</span></span>

![Hantera appen på sidan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a><span data-ttu-id="0dd48-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0dd48-172">Next steps</span></span>

<span data-ttu-id="0dd48-173">Kom igång med [Överför filer tooyour konto](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="0dd48-173">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
