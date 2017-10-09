---
title: aaaUse Azure AD authentication tooaccess Azure Media Services-API med .NET | Microsoft Docs
description: "Det här avsnittet visar hur toouse Azure Active Directory (AD Azure) autentisering tooaccess Azure Media Services (AMS) API med .NET."
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
ms.openlocfilehash: 2f750e460d9e476ad92e96adeac6500cb692cd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-azure-media-services-api-with-net"></a><span data-ttu-id="2b91f-103">Använda Azure AD authentication tooaccess Azure Media Services API med .NET</span><span class="sxs-lookup"><span data-stu-id="2b91f-103">Use Azure AD authentication tooaccess Azure Media Services API with .NET</span></span>

<span data-ttu-id="2b91f-104">Från och med windowsazure.mediaservices 4.0.0.4 stöder Azure Media Services autentisering baserat på Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2b91f-104">Starting with windowsazure.mediaservices 4.0.0.4, Azure Media Services supports authentication based on Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2b91f-105">Det här avsnittet beskrivs hur du toouse Azure AD authentication tooaccess Azure Media Services-API med Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="2b91f-105">This topic shows you how toouse Azure AD  authentication tooaccess Azure Media Services API with Microsoft .NET.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b91f-106">Krav</span><span class="sxs-lookup"><span data-stu-id="2b91f-106">Prerequisites</span></span>

- <span data-ttu-id="2b91f-107">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2b91f-107">An Azure account.</span></span> <span data-ttu-id="2b91f-108">Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2b91f-108">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="2b91f-109">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="2b91f-109">A Media Services account.</span></span> <span data-ttu-id="2b91f-110">Mer information finns i [skapa ett Azure Media Services-konto med hello Azure-portalen](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-110">For more information, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="2b91f-111">Hej senaste [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketet.</span><span class="sxs-lookup"><span data-stu-id="2b91f-111">hello latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span>
- <span data-ttu-id="2b91f-112">Om du är bekant med hello avsnittet [åtkomst till Azure Media Services API med AAD autentiseringsöversikt](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-112">Familiarity with hello topic [Accessing Azure Media Services API with AAD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="2b91f-113">När du använder Azure AD-autentisering med Azure Media Services kan autentisera du på något av två sätt:</span><span class="sxs-lookup"><span data-stu-id="2b91f-113">When you're using Azure AD authentication with Azure Media Services, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="2b91f-114">**Användarautentisering** autentiserar en person som använder hello app toointeract med Azure Media Services-resurser.</span><span class="sxs-lookup"><span data-stu-id="2b91f-114">**User authentication** authenticates a person who is using hello app toointeract with Azure Media Services resources.</span></span> <span data-ttu-id="2b91f-115">hello interaktiva program bör först fråga hello användaren om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2b91f-115">hello interactive application should first prompt hello user for credentials.</span></span> <span data-ttu-id="2b91f-116">Ett exempel är en management konsolapp som används av behöriga användare toomonitor kodning jobb eller direktsänd strömning.</span><span class="sxs-lookup"><span data-stu-id="2b91f-116">An example is a management console app that's used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="2b91f-117">**Autentiseringen av tjänsten huvudnamn** autentiserar en tjänst.</span><span class="sxs-lookup"><span data-stu-id="2b91f-117">**Service principal authentication** authenticates a service.</span></span> <span data-ttu-id="2b91f-118">Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagt jobb, t.ex webbprogram, funktionen appar, logikappar, API: er eller mikrotjänster.</span><span class="sxs-lookup"><span data-stu-id="2b91f-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs, such as web apps, function apps, logic apps, APIs, or microservices.</span></span>

>[!IMPORTANT]
><span data-ttu-id="2b91f-119">Azure Media Service stöder för närvarande en modell för autentisering av Azure Access Control Service.</span><span class="sxs-lookup"><span data-stu-id="2b91f-119">Azure Media Service currently supports an Azure Access Control Service  authentication model.</span></span> <span data-ttu-id="2b91f-120">Access Control-auktorisering försätts toobe föråldrad på den 1 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="2b91f-120">However, Access Control authorization is going toobe deprecated on June 1, 2018.</span></span> <span data-ttu-id="2b91f-121">Vi rekommenderar att du migrerar tooan Azure Active Directory-autentisering så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="2b91f-121">We recommend that you migrate tooan Azure Active Directory authentication model as soon as possible.</span></span>

## <a name="get-an-azure-ad-access-token"></a><span data-ttu-id="2b91f-122">Hämta en Azure AD-åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="2b91f-122">Get an Azure AD access token</span></span>

<span data-ttu-id="2b91f-123">tooconnect toohello Azure Media Services API med Azure AD authentication hello-klientappen måste toorequest en Azure AD-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="2b91f-123">tooconnect toohello Azure Media Services API with Azure AD authentication, hello client app needs toorequest an Azure AD access token.</span></span> <span data-ttu-id="2b91f-124">När du använder klienten för hello Media Services .NET SDK många hello information om hur tooacquire en Azure AD-åtkomsttoken omsluten och förenklad för dig i hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) och [AzureAdTokenCredentials ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) klasser.</span><span class="sxs-lookup"><span data-stu-id="2b91f-124">When you use hello Media Services .NET client SDK, many of hello details about how tooacquire an Azure AD access token are wrapped and simplified for you in hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) and [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) classes.</span></span> 

<span data-ttu-id="2b91f-125">Till exempel behöver du inte tooprovide hello Azure AD myndighet, Media Services resurs-URI eller intern programinformation för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2b91f-125">For example, you don't need tooprovide hello Azure AD authority, Media Services resource URI, or native Azure AD application details.</span></span> <span data-ttu-id="2b91f-126">Detta är välkända värden som redan har konfigurerats av hello leverantörsklass för Azure AD åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="2b91f-126">These are well-known values that are already configured by hello Azure AD access token provider class.</span></span> 

<span data-ttu-id="2b91f-127">Om du inte använder Azure Media Service .NET SDK, rekommenderar vi att du använder hello [Azure AD-Autentiseringsbiblioteket](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-127">If you are not using Azure Media Service .NET SDK, we recommend that you use hello [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span></span> <span data-ttu-id="2b91f-128">tooget värden för hello parametrar som du behöver toouse med Azure AD Authentication Library finns [använda hello Azure portal tooaccess Azure AD-autentiseringsinställningar](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-128">tooget values for hello parameters that you need toouse with Azure AD Authentication Library, see [Use hello Azure portal tooaccess Azure AD authentication settings](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="2b91f-129">Du har också hello möjlighet att ersätta hello standardimplementering av hello **AzureAdTokenProvider** med din egen implementering.</span><span class="sxs-lookup"><span data-stu-id="2b91f-129">You also have hello option of replacing hello default implementation of hello **AzureAdTokenProvider** with your own implementation.</span></span>

## <a name="install-and-configure-azure-media-services-net-sdk"></a><span data-ttu-id="2b91f-130">Installera och konfigurera Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="2b91f-130">Install and configure Azure Media Services .NET SDK</span></span>

>[!NOTE] 
><span data-ttu-id="2b91f-131">toouse Azure AD-autentisering med hello Media Services .NET SDK, behöver du toohave hello senaste [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketet.</span><span class="sxs-lookup"><span data-stu-id="2b91f-131">toouse Azure AD authentication with hello Media Services .NET SDK, you need toohave hello latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span> <span data-ttu-id="2b91f-132">Lägg även till en referens toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** sammansättning.</span><span class="sxs-lookup"><span data-stu-id="2b91f-132">Also, add a reference toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly.</span></span> <span data-ttu-id="2b91f-133">Om du använder en befintlig app, inkludera hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** sammansättning.</span><span class="sxs-lookup"><span data-stu-id="2b91f-133">If you are using an existing app, include hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly.</span></span> 

1. <span data-ttu-id="2b91f-134">Skapa ett nytt C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b91f-134">Create a new C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="2b91f-135">Använd hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet-paketet tooinstall **Azure Media Services .NET SDK**.</span><span class="sxs-lookup"><span data-stu-id="2b91f-135">Use hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet package tooinstall **Azure Media Services .NET SDK**.</span></span> 

    <span data-ttu-id="2b91f-136">tooadd referenser med hjälp av NuGet, ta hello följande: i **Solution Explorer**, högerklicka på projektnamnet hello och väljer sedan **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="2b91f-136">tooadd references by using NuGet, take hello following steps: in **Solution Explorer**, right-click hello project name, and then select **Manage NuGet packages**.</span></span> <span data-ttu-id="2b91f-137">Sök sedan efter **windowsazure.mediaservices** och välj **installera**.</span><span class="sxs-lookup"><span data-stu-id="2b91f-137">Then, search for **windowsazure.mediaservices** and select **Install**.</span></span>
    
    <span data-ttu-id="2b91f-138">ELLER</span><span class="sxs-lookup"><span data-stu-id="2b91f-138">-or-</span></span>

    <span data-ttu-id="2b91f-139">Kör hello följande kommando i **Pakethanterarkonsolen** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b91f-139">Run hello following command in **Package Manager Console** in Visual Studio.</span></span>

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. <span data-ttu-id="2b91f-140">Lägg till **med** tooyour källkoden.</span><span class="sxs-lookup"><span data-stu-id="2b91f-140">Add **using** tooyour source code.</span></span>

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a><span data-ttu-id="2b91f-141">Använd autentisering av användare</span><span class="sxs-lookup"><span data-stu-id="2b91f-141">Use user authentication</span></span>

<span data-ttu-id="2b91f-142">tooconnect toohello Azure Media Service API med hello användaren autentiseringsalternativet hello-klientappen måste toorequest en Azure AD-token med hjälp av hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="2b91f-142">tooconnect toohello Azure Media Service API with hello user authentication option, hello client app needs toorequest an Azure AD token by using hello following parameters:</span></span>  

- <span data-ttu-id="2b91f-143">Slutpunkten för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="2b91f-143">Azure AD tenant endpoint.</span></span> <span data-ttu-id="2b91f-144">hello klient information kan hämtas från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b91f-144">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="2b91f-145">Hovra över hello inloggade användaren i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="2b91f-145">Hover over hello signed-in user in hello upper-right corner.</span></span>
- <span data-ttu-id="2b91f-146">Media Services resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="2b91f-146">Media Services resource URI.</span></span>
- <span data-ttu-id="2b91f-147">Media Services (ursprunglig) programmet klient-ID.</span><span class="sxs-lookup"><span data-stu-id="2b91f-147">Media Services (native) application client ID.</span></span> 
- <span data-ttu-id="2b91f-148">Media Services (intern)-programmet omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="2b91f-148">Media Services (native) application redirect URI.</span></span> 

<span data-ttu-id="2b91f-149">hello värdena för dessa parametrar finns i **AzureEnvironments.AzureCloudEnvironment**.</span><span class="sxs-lookup"><span data-stu-id="2b91f-149">hello values for these parameters can be found in **AzureEnvironments.AzureCloudEnvironment**.</span></span> <span data-ttu-id="2b91f-150">Hej **AzureEnvironments.AzureCloudEnvironment** konstant är en hjälp i hello .NET SDK tooget hello rätt inställningar för miljövariabeln en offentlig Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="2b91f-150">hello **AzureEnvironments.AzureCloudEnvironment** constant is a helper in hello .NET SDK tooget hello right environment variable settings for a public Azure Data Center.</span></span> 

<span data-ttu-id="2b91f-151">Den innehåller fördefinierade inställningar för åtkomst till Media Services i hello offentliga data datacenter.</span><span class="sxs-lookup"><span data-stu-id="2b91f-151">It contains pre-defined environment settings for accessing Media Services in hello public data centers only.</span></span> <span data-ttu-id="2b91f-152">Du kan använda för statliga eller statlig molnområdena **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, eller **AzureGermanCloudEnvironment** respektive.</span><span class="sxs-lookup"><span data-stu-id="2b91f-152">For sovereign or government cloud regions, you can use **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, or **AzureGermanCloudEnvironment** respectively.</span></span>

<span data-ttu-id="2b91f-153">Följande kodexempel hello skapar en token:</span><span class="sxs-lookup"><span data-stu-id="2b91f-153">hello following code example creates a token:</span></span>
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
<span data-ttu-id="2b91f-154">toostart programmera mot Media Services behöver du toocreate en **CloudMediaContext** -instans som representerar hello serverkontext.</span><span class="sxs-lookup"><span data-stu-id="2b91f-154">toostart programming against Media Services, you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="2b91f-155">Hej **CloudMediaContext** innehåller referenser tooimportant samlingar inklusive jobb, tillgångar, filer, principer för åtkomst och lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="2b91f-155">hello **CloudMediaContext** includes references tooimportant collections including jobs, assets, files, access policies, and locators.</span></span> 

<span data-ttu-id="2b91f-156">Du måste också toopass hello **resurs-URI för REST medietjänster** toohello **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="2b91f-156">You also need toopass hello **resource URI for Media REST Services** toohello **CloudMediaContext** constructor.</span></span> <span data-ttu-id="2b91f-157">tooget hello resurs-URI för REST medietjänster inloggning toohello Azure portal, väljer du Azure Media Services-kontot, Välj **API-åtkomst**, och välj sedan **ansluta tooAzure Media Services med användare autentisering**.</span><span class="sxs-lookup"><span data-stu-id="2b91f-157">tooget hello resource URI for Media REST Services, sign in toohello Azure portal, select your Azure Media Services account, select **API access**, and then select **Connect tooAzure Media Services with user authentication**.</span></span> 

<span data-ttu-id="2b91f-158">hello följande exempel skapar en **CloudMediaContext** instans:</span><span class="sxs-lookup"><span data-stu-id="2b91f-158">hello following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

<span data-ttu-id="2b91f-159">hello som följande exempel visar hur toocreate hello Azure AD-token och hello kontext:</span><span class="sxs-lookup"><span data-stu-id="2b91f-159">hello following example shows how toocreate hello Azure AD token and hello context:</span></span>

    namespace AADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR AAD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
><span data-ttu-id="2b91f-160">Om du får ett undantag med texten ”hello fjärrservern returnerade ett fel: (401) obehörig” finns hello [åtkomstkontroll](media-services-use-aad-auth-to-access-ams-api.md#access-control) avsnitt för att få åtkomst till Azure Media Services API med översikt över Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="2b91f-160">If you get an exception that says "hello remote server returned an error: (401) Unauthorized," see hello [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section of Accessing Azure Media Services API with Azure AD authentication overview.</span></span>

## <a name="use-service-principal-authentication"></a><span data-ttu-id="2b91f-161">Använd service principal autentisering</span><span class="sxs-lookup"><span data-stu-id="2b91f-161">Use service principal authentication</span></span>
    
<span data-ttu-id="2b91f-162">tooconnect toohello Azure Media Services API med hello service principal alternativet, måste appen mellannivå (web API eller webbprogram) toorequests en Azure AD-token med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="2b91f-162">tooconnect toohello Azure Media Services API with hello service principal option, your middle-tier app (web API or web application) needs toorequests an Azure AD token with hello following parameters:</span></span>  

- <span data-ttu-id="2b91f-163">Slutpunkten för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="2b91f-163">Azure AD tenant endpoint.</span></span> <span data-ttu-id="2b91f-164">hello klient information kan hämtas från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b91f-164">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="2b91f-165">Hovra över hello inloggade användaren i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="2b91f-165">Hover over hello signed-in user in hello upper-right corner.</span></span>
- <span data-ttu-id="2b91f-166">Media Services resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="2b91f-166">Media Services resource URI.</span></span>
- <span data-ttu-id="2b91f-167">Azure AD application värden: hello **klient-ID** och **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="2b91f-167">Azure AD application values: hello **Client ID** and **Client secret**.</span></span>

<span data-ttu-id="2b91f-168">Hej värden för hello **klient-ID** och **klienthemlighet** parametrar finns i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b91f-168">hello values for hello **Client ID** and **Client secret** parameters can be found in hello Azure portal.</span></span> <span data-ttu-id="2b91f-169">Mer information finns i [komma igång med Azure AD authentication hello Azure-portalen](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-169">For more information, see [Getting started with Azure AD authentication using hello Azure portal](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="2b91f-170">hello följande exempel skapar en token med hjälp av hello **AzureAdTokenCredentials** konstruktor som tar **AzureAdClientSymmetricKey** som en parameter:</span><span class="sxs-lookup"><span data-stu-id="2b91f-170">hello following code example creates a token by using hello **AzureAdTokenCredentials** constructor that takes **AzureAdClientSymmetricKey** as a parameter:</span></span> 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

<span data-ttu-id="2b91f-171">Du kan också ange hello **AzureAdTokenCredentials** konstruktor som tar **AzureAdClientCertificate** som en parameter.</span><span class="sxs-lookup"><span data-stu-id="2b91f-171">You can also specify hello **AzureAdTokenCredentials** constructor that takes **AzureAdClientCertificate** as a parameter.</span></span> 

<span data-ttu-id="2b91f-172">Anvisningar om hur toocreate och konfigurera ett certifikat i en form som kan användas av Azure AD, se [autentisera tooAzure AD i daemon appar med certifikat - manuella konfigurationsstegen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-172">For instructions about how toocreate and configure a certificate in a form that can be used by Azure AD, see [Authenticating tooAzure AD in daemon apps with certificates - manual configuration steps](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span></span>

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

<span data-ttu-id="2b91f-173">toostart programmera mot Media Services behöver du toocreate en **CloudMediaContext** -instans som representerar hello serverkontext.</span><span class="sxs-lookup"><span data-stu-id="2b91f-173">toostart programming against Media Services, you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="2b91f-174">Du måste också toopass hello **resurs-URI för REST medietjänster** toohello **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="2b91f-174">You also need toopass hello **resource URI for Media REST Services** toohello **CloudMediaContext** constructor.</span></span> <span data-ttu-id="2b91f-175">Du kan hämta hello **resurs-URI för REST medietjänster** värde från hello Azure portal samt.</span><span class="sxs-lookup"><span data-stu-id="2b91f-175">You can get hello **resource URI for Media REST Services** value from hello Azure portal as well.</span></span>

<span data-ttu-id="2b91f-176">hello följande exempel skapar en **CloudMediaContext** instans:</span><span class="sxs-lookup"><span data-stu-id="2b91f-176">hello following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
<span data-ttu-id="2b91f-177">hello som följande exempel visar hur toocreate hello Azure AD-token och hello kontext:</span><span class="sxs-lookup"><span data-stu-id="2b91f-177">hello following example shows how toocreate hello Azure AD token and hello context:</span></span>

    namespace AADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a><span data-ttu-id="2b91f-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b91f-178">Next steps</span></span>

<span data-ttu-id="2b91f-179">Kom igång med [Överför filer tooyour konto](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="2b91f-179">Get started with [uploading files tooyour account](media-services-dotnet-upload-files.md).</span></span>
