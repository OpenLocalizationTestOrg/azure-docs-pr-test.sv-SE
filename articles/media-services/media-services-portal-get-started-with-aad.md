---
title: "Komma igång med Azure AD-autentisering med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur du använder Azure-portalen för att få åtkomst till Azure Active Directory (AD Azure) autentisering om du vill använda Azure Media Services API."
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
ms.openlocfilehash: 829e0759f8aeb8758f6b8895b88b60b66ffb22ed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a><span data-ttu-id="ca1ec-103">Komma igång med Azure AD-autentisering med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="ca1ec-103">Get started with Azure AD authentication by using the Azure portal</span></span>

<span data-ttu-id="ca1ec-104">Lär dig hur du använder Azure-portalen för att få åtkomst till Azure Active Directory (AD Azure) autentiseras för åtkomst till Azure Media Services API.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-104">Learn how to use the Azure portal to access Azure Active Directory (Azure AD) authentication to access the Azure Media Services API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca1ec-105">Krav</span><span class="sxs-lookup"><span data-stu-id="ca1ec-105">Prerequisites</span></span>

- <span data-ttu-id="ca1ec-106">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-106">An Azure account.</span></span> <span data-ttu-id="ca1ec-107">Om du inte har ett konto, börja med en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="ca1ec-108">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-108">A Media Services account.</span></span> <span data-ttu-id="ca1ec-109">Mer information finns i [skapa ett Azure Media Services-konto med hjälp av Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-109">For more information, see [Create an Azure Media Services account by using the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="ca1ec-110">Kontrollera att du läser den [åtkomst till Azure Media Services API med Azure AD-autentiseringsöversikt](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-110">Make sure you review the [Accessing Azure Media Services API with Azure AD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="ca1ec-111">När du använder Azure AD-autentisering med Azure Media Services, har du två alternativ för autentisering:</span><span class="sxs-lookup"><span data-stu-id="ca1ec-111">When you use Azure AD authentication with Azure Media Services, you have two authentication options:</span></span>

- <span data-ttu-id="ca1ec-112">**Användarautentisering**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-112">**User authentication**.</span></span> <span data-ttu-id="ca1ec-113">Autentisera en person som använder appen för att interagera med Media Services-resurser.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-113">Authenticate a person who is using the app to interact with Media Services resources.</span></span> <span data-ttu-id="ca1ec-114">Interaktiva program bör först uppmana användaren att ange autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-114">The interactive application should first prompt the user for credentials.</span></span> <span data-ttu-id="ca1ec-115">Ett exempel är en management konsolapp som används av behöriga användare för att övervaka kodning jobb eller direktsänd strömning.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-115">An example is a management console app used by authorized users to monitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="ca1ec-116">**Huvudnamn autentiseringen av tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-116">**Service principal authentication**.</span></span> <span data-ttu-id="ca1ec-117">Autentisera en tjänst.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-117">Authenticate a service.</span></span> <span data-ttu-id="ca1ec-118">Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagda jobb: webbappar, funktionen appar, logikappar, API: er eller en mikrotjänster.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs: web apps, function apps, logic apps, APIs, or a microservice.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ca1ec-119">Media Services stöder för närvarande Azure Access Control service autentisering modellen.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-119">Currently, Media Services supports the Azure Access Control service authentication model.</span></span> <span data-ttu-id="ca1ec-120">Access Control tillstånd att bli inaktuell på den 1 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-120">However, Access Control authorization will be deprecated on June 1, 2018.</span></span> <span data-ttu-id="ca1ec-121">Vi rekommenderar att du migrerar till Azure AD-autentisering så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-121">We recommend that you migrate to the Azure AD authentication model as soon as possible.</span></span>

## <a name="select-the-authentication-method"></a><span data-ttu-id="ca1ec-122">Välj autentiseringsmetoden</span><span class="sxs-lookup"><span data-stu-id="ca1ec-122">Select the authentication method</span></span>

1. <span data-ttu-id="ca1ec-123">I den [Azure-portalen](https://portal.azure.com/), Välj Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-123">In the [Azure portal](https://portal.azure.com/), select your Media Services account.</span></span>
2. <span data-ttu-id="ca1ec-124">Välj hur du ansluter till Media Services API.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-124">Select how to connect to the Media Services API.</span></span>

    ![Välj metoden anslutningssidan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a><span data-ttu-id="ca1ec-126">Användarautentisering</span><span class="sxs-lookup"><span data-stu-id="ca1ec-126">User authentication</span></span>

<span data-ttu-id="ca1ec-127">För att ansluta till Media Services-API med hjälp av alternativet för autentisering av användare, måste klientapp att begära en Azure AD-token som har följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ca1ec-127">To connect to the Media Services API by using the user authentication option, the client app needs to request an Azure AD token that has the following parameters:</span></span>  

* <span data-ttu-id="ca1ec-128">Azure AD-klient slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ca1ec-128">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="ca1ec-129">Media Services resurs-URI</span><span class="sxs-lookup"><span data-stu-id="ca1ec-129">Media Services resource URI</span></span>
* <span data-ttu-id="ca1ec-130">Klient-ID för Media Services (intern)-program</span><span class="sxs-lookup"><span data-stu-id="ca1ec-130">Media Services (native) application client ID</span></span> 
* <span data-ttu-id="ca1ec-131">Media Services (intern)-programmet omdirigerings-URI</span><span class="sxs-lookup"><span data-stu-id="ca1ec-131">Media Services (native) application redirect URI</span></span> 
* <span data-ttu-id="ca1ec-132">Resurs-URI för REST Media Services</span><span class="sxs-lookup"><span data-stu-id="ca1ec-132">Resource URI for REST Media Services</span></span>

<span data-ttu-id="ca1ec-133">Du kan få värdena för dessa parametrar på den **Media Services-API med användarautentisering med** sidan.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-133">You can get the values for these parameters on the **Media Services API with user authentication** page.</span></span> 

![Ansluta till sidan för autentisering av användare](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

<span data-ttu-id="ca1ec-135">Om du ansluter till Media Services-API med hjälp av Microsoft för Media Services .NET SDK är krävs värdena tillgängliga som en del av SDK.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-135">If you connect to the Media Services API by using the Media Services Microsoft .NET SDK, the required values are available to you as part of the SDK.</span></span> <span data-ttu-id="ca1ec-136">Mer information finns i [använda Azure AD-autentisering för åtkomst till Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-136">For more information, see [Use Azure AD authentication to access the Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="ca1ec-137">Om du inte använder klienten Media Services .NET SDK, måste du manuellt skapa en begäran om Azure AD-token med de parametrar som nämnts tidigare.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-137">If you're not using the Media Services .NET client SDK, you must manually create an Azure AD token request by using the parameters discussed earlier.</span></span> <span data-ttu-id="ca1ec-138">Mer information finns i [hur du använder Azure AD-Autentiseringsbiblioteket för att hämta Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-138">For more information, see [How to use the Azure AD Authentication Library to get the Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

## <a name="service-principal-authentication"></a><span data-ttu-id="ca1ec-139">Autentisering av tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="ca1ec-139">Service principal authentication</span></span>

<span data-ttu-id="ca1ec-140">Om du vill ansluta till Media Services-API med hjälp av alternativet service principal måste appen mellannivå (web API eller webbprogram) begära en Azure AD-token som har följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="ca1ec-140">To connect to the Media Services API by using the service principal option, your middle-tier app (web API or web application) needs to request an Azure AD token that has the following parameters:</span></span>  

* <span data-ttu-id="ca1ec-141">Azure AD-klient slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ca1ec-141">Azure AD tenant endpoint</span></span>
* <span data-ttu-id="ca1ec-142">Media Services resurs-URI</span><span class="sxs-lookup"><span data-stu-id="ca1ec-142">Media Services resource URI</span></span> 
* <span data-ttu-id="ca1ec-143">Resurs-URI för REST Media Services</span><span class="sxs-lookup"><span data-stu-id="ca1ec-143">Resource URI for REST Media Services</span></span>
* <span data-ttu-id="ca1ec-144">Azure AD application värden: den **klient-ID** och **klienthemlighet**</span><span class="sxs-lookup"><span data-stu-id="ca1ec-144">Azure AD application values: the **client ID** and **client secret**</span></span>

<span data-ttu-id="ca1ec-145">Du kan få värdena för dessa parametrar på den **Anslut till Media Services-API med tjänstens huvudnamn** sidan.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-145">You can get the values for these parameters on the **Connect to Media Services API with service principal** page.</span></span> <span data-ttu-id="ca1ec-146">Använd den här sidan för att skapa en ny Azure AD-program eller välja en befintlig.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-146">Use this page to create a new Azure AD application or to select an existing one.</span></span> <span data-ttu-id="ca1ec-147">När du har valt Azure AD-appar kan du hämta klient-ID (program-ID) och skapa värdena klienten hemligheten (nyckel).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-147">After you select the Azure AD app, you can get the client ID (Application ID) and generate the client secret (key) values.</span></span> 

![Ansluta till service principal sida](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

<span data-ttu-id="ca1ec-149">När den **tjänstens huvudnamn** blad öppnas, väljs det första Azure AD-program som uppfyller följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="ca1ec-149">When the **Service Principal** blade opens, the first Azure AD application that meets the following criteria is selected:</span></span>

- <span data-ttu-id="ca1ec-150">Det är en registrerad Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-150">It is a registered Azure AD application.</span></span>
- <span data-ttu-id="ca1ec-151">Det har behörighet som deltagare eller Owner Role-Based åtkomstkontroll för kontot.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-151">It has Contributor or Owner Role-Based Access Control permissions on the account.</span></span>

<span data-ttu-id="ca1ec-152">När du skapar eller välj en Azure AD-app, kan du skapa och kopiera en klienthemlighet (nyckel) och klient-ID (program-ID).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-152">After you create or select an Azure AD app, you can create and copy a client secret (key) and the client ID (Application ID).</span></span> <span data-ttu-id="ca1ec-153">Klienthemligheten och klient-ID krävs för att hämta åtkomsten-token i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-153">The client secret and client ID are required to get the access token in this scenario.</span></span>

<span data-ttu-id="ca1ec-154">Om du inte har behörighet att skapa Azure AD-appar i din domän, Azure AD app kontroller av bladet visas inte och visas ett varningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-154">If you don't have permissions to create Azure AD apps in your domain, the Azure AD app controls of the blade are not shown, and a warning message is displayed.</span></span>

<span data-ttu-id="ca1ec-155">Om du ansluter till Media Services-API med hjälp av Media Services .NET SDK, se [använda Azure AD-autentisering för åtkomst till Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-155">If you connect to the Media Services API by using the Media Services .NET SDK, see [Use Azure AD authentication to access the Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md).</span></span>

<span data-ttu-id="ca1ec-156">Om du inte använder klienten Media Services .NET SDK, måste du manuellt skapa en Azure AD-tokenbegäran med parametrarna som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-156">If you are not using the Media Services .NET client SDK, you must manually create an Azure AD token request using the parameters discussed earlier.</span></span> <span data-ttu-id="ca1ec-157">Mer information finns i [hur du använder Azure AD-Autentiseringsbiblioteket för att hämta Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-157">For more information, see [How to use the Azure AD Authentication Library to get the Azure AD token](../active-directory/develop/active-directory-authentication-libraries.md).</span></span>

### <a name="get-the-client-id-and-client-secret"></a><span data-ttu-id="ca1ec-158">Hämta klient-ID och klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="ca1ec-158">Get the client ID and client secret</span></span>

<span data-ttu-id="ca1ec-159">När du väljer en befintlig Azure AD-app eller Välj alternativet för att skapa en ny, visas följande knappar:</span><span class="sxs-lookup"><span data-stu-id="ca1ec-159">After you select an existing Azure AD app or select the option to create a new one, the following buttons appear:</span></span>

![Hantera behörigheter och hantera program](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

<span data-ttu-id="ca1ec-161">Klicka för att öppna bladet Azure AD application **hantera program**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-161">To open the Azure AD application blade, click **Manage application**.</span></span> <span data-ttu-id="ca1ec-162">På den **hantera program** bladet kan du hämta appens klient-ID (program-ID).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-162">On the **Manage application** blade, you can get the app's client ID (Application ID).</span></span> <span data-ttu-id="ca1ec-163">Om du vill generera en klienthemlighet (nyckel), Välj **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-163">To generate a client secret (key), select **Keys**.</span></span>

![Hantera program bladet nycklar alternativet](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a><span data-ttu-id="ca1ec-165">Hantera behörigheter och programmet</span><span class="sxs-lookup"><span data-stu-id="ca1ec-165">Manage permissions and the application</span></span>

<span data-ttu-id="ca1ec-166">När du har valt Azure AD-program kan du hantera program och behörigheter.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-166">After you select the Azure AD application, you can manage the application and permissions.</span></span> <span data-ttu-id="ca1ec-167">Klicka på Konfigurera Azure AD-program för att få åtkomst till andra program, **hantera behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-167">To set up your Azure AD application to access other applications, click **Manage permissions**.</span></span> <span data-ttu-id="ca1ec-168">För hanteringsuppgifter, till exempel ändra reply URL: er och nycklar eller redigera programmanifestet, klicka på **hantera program**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-168">For management tasks, such as changing keys and reply URLs, or to edit the application’s manifest, click **Manage application**.</span></span>

### <a name="edit-the-apps-settings-or-manifest"></a><span data-ttu-id="ca1ec-169">Redigera inställningar för appens eller manifest</span><span class="sxs-lookup"><span data-stu-id="ca1ec-169">Edit the app's settings or manifest</span></span>

<span data-ttu-id="ca1ec-170">Om du vill redigera appens inställningar eller manifestet **hantera program**.</span><span class="sxs-lookup"><span data-stu-id="ca1ec-170">To edit the app's settings or manifest, click **Manage application**.</span></span>

![Hantera appen på sidan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a><span data-ttu-id="ca1ec-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca1ec-172">Next steps</span></span>

<span data-ttu-id="ca1ec-173">Kom igång med [överföringen av filer till ditt konto](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ec-173">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
