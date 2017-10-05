---
title: "Använda Azure AD-autentisering för att komma åt Azure Media Services-API med .NET | Microsoft Docs"
description: "Det här avsnittet visar hur du använder Azure Active Directory (AD Azure) autentisering för att komma åt Azure Media Services (AMS) API med .NET."
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
ms.openlocfilehash: a9355200a05a3aa1b494b76977d38ddc42bfe179
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a><span data-ttu-id="8c896-103">Använda Azure AD-autentisering för att komma åt Azure Media Services-API med .NET</span><span class="sxs-lookup"><span data-stu-id="8c896-103">Use Azure AD authentication to access Azure Media Services API with .NET</span></span>

<span data-ttu-id="8c896-104">Från och med windowsazure.mediaservices 4.0.0.4 stöder Azure Media Services autentisering baserat på Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8c896-104">Starting with windowsazure.mediaservices 4.0.0.4, Azure Media Services supports authentication based on Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="8c896-105">Det här avsnittet visar hur du använder Azure AD-autentisering för att komma åt Azure Media Services-API med Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="8c896-105">This topic shows you how to use Azure AD  authentication to access Azure Media Services API with Microsoft .NET.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c896-106">Krav</span><span class="sxs-lookup"><span data-stu-id="8c896-106">Prerequisites</span></span>

- <span data-ttu-id="8c896-107">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8c896-107">An Azure account.</span></span> <span data-ttu-id="8c896-108">Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8c896-108">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="8c896-109">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="8c896-109">A Media Services account.</span></span> <span data-ttu-id="8c896-110">Mer information finns i [skapa ett Azure Media Services-konto med hjälp av Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-110">For more information, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="8c896-111">Senast [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketet.</span><span class="sxs-lookup"><span data-stu-id="8c896-111">The latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span>
- <span data-ttu-id="8c896-112">Om du är bekant med ämnet [åtkomst till Azure Media Services API med AAD autentiseringsöversikt](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-112">Familiarity with the topic [Accessing Azure Media Services API with AAD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="8c896-113">När du använder Azure AD-autentisering med Azure Media Services kan autentisera du på något av två sätt:</span><span class="sxs-lookup"><span data-stu-id="8c896-113">When you're using Azure AD authentication with Azure Media Services, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="8c896-114">**Användarautentisering** autentiserar en person som använder appen för att interagera med Azure Media Services-resurser.</span><span class="sxs-lookup"><span data-stu-id="8c896-114">**User authentication** authenticates a person who is using the app to interact with Azure Media Services resources.</span></span> <span data-ttu-id="8c896-115">Interaktiva program bör först uppmana användaren att ange autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8c896-115">The interactive application should first prompt the user for credentials.</span></span> <span data-ttu-id="8c896-116">Ett exempel är en management konsolapp som används av behöriga användare att övervaka kodning jobb eller direktsänd strömning.</span><span class="sxs-lookup"><span data-stu-id="8c896-116">An example is a management console app that's used by authorized users to monitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="8c896-117">**Autentiseringen av tjänsten huvudnamn** autentiserar en tjänst.</span><span class="sxs-lookup"><span data-stu-id="8c896-117">**Service principal authentication** authenticates a service.</span></span> <span data-ttu-id="8c896-118">Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagt jobb, t.ex webbprogram, funktionen appar, logikappar, API: er eller mikrotjänster.</span><span class="sxs-lookup"><span data-stu-id="8c896-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs, such as web apps, function apps, logic apps, APIs, or microservices.</span></span>

>[!IMPORTANT]
><span data-ttu-id="8c896-119">Azure Media Service stöder för närvarande en modell för autentisering av Azure Access Control Service.</span><span class="sxs-lookup"><span data-stu-id="8c896-119">Azure Media Service currently supports an Azure Access Control Service  authentication model.</span></span> <span data-ttu-id="8c896-120">Dock kommer åtkomstkontroll tillstånd att bli inaktuell på den 1 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="8c896-120">However, Access Control authorization is going to be deprecated on June 1, 2018.</span></span> <span data-ttu-id="8c896-121">Vi rekommenderar att du migrerar till en modell för Azure Active Directory-autentisering så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="8c896-121">We recommend that you migrate to an Azure Active Directory authentication model as soon as possible.</span></span>

## <a name="get-an-azure-ad-access-token"></a><span data-ttu-id="8c896-122">Hämta en Azure AD-åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="8c896-122">Get an Azure AD access token</span></span>

<span data-ttu-id="8c896-123">För att ansluta till Azure Media Services-API med Azure AD-autentisering, måste klientapp att begära en Azure AD-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="8c896-123">To connect to the Azure Media Services API with Azure AD authentication, the client app needs to request an Azure AD access token.</span></span> <span data-ttu-id="8c896-124">När du använder klient-Media Services .NET SDK många av information om hur du skaffar en Azure AD-åtkomsttoken omsluten och förenklad för dig i den [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) och [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) klasser.</span><span class="sxs-lookup"><span data-stu-id="8c896-124">When you use the Media Services .NET client SDK, many of the details about how to acquire an Azure AD access token are wrapped and simplified for you in the [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) and [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) classes.</span></span> 

<span data-ttu-id="8c896-125">Du behöver inte ange utfärdare för Azure AD, Media Services resurs-URI eller intern programinformation för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c896-125">For example, you don't need to provide the Azure AD authority, Media Services resource URI, or native Azure AD application details.</span></span> <span data-ttu-id="8c896-126">Detta är välkända värden som redan har konfigurerats av Azure AD åtkomst tokenleverantör-klassen.</span><span class="sxs-lookup"><span data-stu-id="8c896-126">These are well-known values that are already configured by the Azure AD access token provider class.</span></span> 

<span data-ttu-id="8c896-127">Om du inte använder Azure Media Service .NET SDK, rekommenderar vi att du använder den [Azure AD-Autentiseringsbiblioteket](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-127">If you are not using Azure Media Service .NET SDK, we recommend that you use the [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span></span> <span data-ttu-id="8c896-128">Värden för parametrarna som du vill använda med Azure AD-Autentiseringsbiblioteket finns [Använd Azure-portalen för att få åtkomst till Azure AD-autentiseringsinställningar](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-128">To get values for the parameters that you need to use with Azure AD Authentication Library, see [Use the Azure portal to access Azure AD authentication settings](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="8c896-129">Du har också möjlighet att ersätta standardimplementering av den **AzureAdTokenProvider** med din egen implementering.</span><span class="sxs-lookup"><span data-stu-id="8c896-129">You also have the option of replacing the default implementation of the **AzureAdTokenProvider** with your own implementation.</span></span>

## <a name="install-and-configure-azure-media-services-net-sdk"></a><span data-ttu-id="8c896-130">Installera och konfigurera Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="8c896-130">Install and configure Azure Media Services .NET SDK</span></span>

>[!NOTE] 
><span data-ttu-id="8c896-131">Om du vill använda Azure AD-autentisering med Media Services .NET SDK, behöver du senast [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketet.</span><span class="sxs-lookup"><span data-stu-id="8c896-131">To use Azure AD authentication with the Media Services .NET SDK, you need to have the latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span> <span data-ttu-id="8c896-132">Lägg även till en referens till den **Microsoft.IdentityModel.Clients.ActiveDirectory** sammansättning.</span><span class="sxs-lookup"><span data-stu-id="8c896-132">Also, add a reference to the **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly.</span></span> <span data-ttu-id="8c896-133">Om du använder en befintlig app, inkludera den **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** sammansättning.</span><span class="sxs-lookup"><span data-stu-id="8c896-133">If you are using an existing app, include the **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly.</span></span> 

1. <span data-ttu-id="8c896-134">Skapa ett nytt C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c896-134">Create a new C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="8c896-135">Använd den [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet-paketet för att installera **Azure Media Services .NET SDK**.</span><span class="sxs-lookup"><span data-stu-id="8c896-135">Use the [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet package to install **Azure Media Services .NET SDK**.</span></span> 

    <span data-ttu-id="8c896-136">Om du vill lägga till referenser med NuGet, gör du följande: i **Solution Explorer**, högerklicka på projektnamnet och välj sedan **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="8c896-136">To add references by using NuGet, take the following steps: in **Solution Explorer**, right-click the project name, and then select **Manage NuGet packages**.</span></span> <span data-ttu-id="8c896-137">Sök sedan efter **windowsazure.mediaservices** och välj **installera**.</span><span class="sxs-lookup"><span data-stu-id="8c896-137">Then, search for **windowsazure.mediaservices** and select **Install**.</span></span>
    
    <span data-ttu-id="8c896-138">ELLER</span><span class="sxs-lookup"><span data-stu-id="8c896-138">-or-</span></span>

    <span data-ttu-id="8c896-139">Kör följande kommando **Pakethanterarkonsolen** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c896-139">Run the following command in **Package Manager Console** in Visual Studio.</span></span>

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. <span data-ttu-id="8c896-140">Lägg till **med** till din källkod.</span><span class="sxs-lookup"><span data-stu-id="8c896-140">Add **using** to your source code.</span></span>

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a><span data-ttu-id="8c896-141">Använd autentisering av användare</span><span class="sxs-lookup"><span data-stu-id="8c896-141">Use user authentication</span></span>

<span data-ttu-id="8c896-142">För att ansluta till Azure Media Service API med autentiseringsalternativet som användare behöver klientappen begära en Azure AD-token med hjälp av följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="8c896-142">To connect to the Azure Media Service API with the user authentication option, the client app needs to request an Azure AD token by using the following parameters:</span></span>  

- <span data-ttu-id="8c896-143">Slutpunkten för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="8c896-143">Azure AD tenant endpoint.</span></span> <span data-ttu-id="8c896-144">Klient-informationen kan hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c896-144">The tenant information can be retrieved from the Azure portal.</span></span> <span data-ttu-id="8c896-145">Hovra över inloggade användaren i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8c896-145">Hover over the signed-in user in the upper-right corner.</span></span>
- <span data-ttu-id="8c896-146">Media Services resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="8c896-146">Media Services resource URI.</span></span>
- <span data-ttu-id="8c896-147">Media Services (ursprunglig) programmet klient-ID.</span><span class="sxs-lookup"><span data-stu-id="8c896-147">Media Services (native) application client ID.</span></span> 
- <span data-ttu-id="8c896-148">Media Services (intern)-programmet omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="8c896-148">Media Services (native) application redirect URI.</span></span> 

<span data-ttu-id="8c896-149">Värdena för dessa parametrar finns i **AzureEnvironments.AzureCloudEnvironment**.</span><span class="sxs-lookup"><span data-stu-id="8c896-149">The values for these parameters can be found in **AzureEnvironments.AzureCloudEnvironment**.</span></span> <span data-ttu-id="8c896-150">Den **AzureEnvironments.AzureCloudEnvironment** konstant är en hjälp i .NET SDK för att hämta rätt miljön variabeln inställningar för en offentlig Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="8c896-150">The **AzureEnvironments.AzureCloudEnvironment** constant is a helper in the .NET SDK to get the right environment variable settings for a public Azure Data Center.</span></span> 

<span data-ttu-id="8c896-151">Den innehåller fördefinierade inställningar för åtkomst till Media Services i de offentliga data datacenter.</span><span class="sxs-lookup"><span data-stu-id="8c896-151">It contains pre-defined environment settings for accessing Media Services in the public data centers only.</span></span> <span data-ttu-id="8c896-152">Du kan använda för statliga eller statlig molnområdena **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, eller **AzureGermanCloudEnvironment** respektive.</span><span class="sxs-lookup"><span data-stu-id="8c896-152">For sovereign or government cloud regions, you can use **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, or **AzureGermanCloudEnvironment** respectively.</span></span>

<span data-ttu-id="8c896-153">Följande exempel skapar en token:</span><span class="sxs-lookup"><span data-stu-id="8c896-153">The following code example creates a token:</span></span>
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
<span data-ttu-id="8c896-154">Om du vill starta programmera mot Media Services måste du skapa en **CloudMediaContext** -instans som representerar serverkontext.</span><span class="sxs-lookup"><span data-stu-id="8c896-154">To start programming against Media Services, you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="8c896-155">Den **CloudMediaContext** innehåller referenser till viktiga samlingar, inklusive jobb, tillgångar, filer, principer för åtkomst och lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="8c896-155">The **CloudMediaContext** includes references to important collections including jobs, assets, files, access policies, and locators.</span></span> 

<span data-ttu-id="8c896-156">Du måste också ange den **resurs-URI för Media Services för REST** till den **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="8c896-156">You also need to pass the **resource URI for Media REST Services** to the **CloudMediaContext** constructor.</span></span> <span data-ttu-id="8c896-157">För att få resurs-URI för REST medietjänster logga in på Azure portal, väljer du Azure Media Services-konto, Välj **API-åtkomst**, och välj sedan **Anslut till Azure Media Services med användarautentisering med**.</span><span class="sxs-lookup"><span data-stu-id="8c896-157">To get the resource URI for Media REST Services, sign in to the Azure portal, select your Azure Media Services account, select **API access**, and then select **Connect to Azure Media Services with user authentication**.</span></span> 

<span data-ttu-id="8c896-158">Följande exempel skapar en **CloudMediaContext** instans:</span><span class="sxs-lookup"><span data-stu-id="8c896-158">The following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

<span data-ttu-id="8c896-159">I följande exempel visas hur du skapar Azure AD-token och kontexten:</span><span class="sxs-lookup"><span data-stu-id="8c896-159">The following example shows how to create the Azure AD token and the context:</span></span>

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
><span data-ttu-id="8c896-160">Om du får ett undantag som säger ”fjärrservern returnerade ett fel: (401) obehörig” finns på [åtkomstkontroll](media-services-use-aad-auth-to-access-ams-api.md#access-control) avsnitt för att få åtkomst till Azure Media Services API med översikt över Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="8c896-160">If you get an exception that says "The remote server returned an error: (401) Unauthorized," see the [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section of Accessing Azure Media Services API with Azure AD authentication overview.</span></span>

## <a name="use-service-principal-authentication"></a><span data-ttu-id="8c896-161">Använd service principal autentisering</span><span class="sxs-lookup"><span data-stu-id="8c896-161">Use service principal authentication</span></span>
    
<span data-ttu-id="8c896-162">För att ansluta till API: et för Azure Media Services med service principal alternativet din app på mellannivå (webb-API eller webbprogram) behöver du begär en Azure AD-token med följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="8c896-162">To connect to the Azure Media Services API with the service principal option, your middle-tier app (web API or web application) needs to requests an Azure AD token with the following parameters:</span></span>  

- <span data-ttu-id="8c896-163">Slutpunkten för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="8c896-163">Azure AD tenant endpoint.</span></span> <span data-ttu-id="8c896-164">Klient-informationen kan hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c896-164">The tenant information can be retrieved from the Azure portal.</span></span> <span data-ttu-id="8c896-165">Hovra över inloggade användaren i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8c896-165">Hover over the signed-in user in the upper-right corner.</span></span>
- <span data-ttu-id="8c896-166">Media Services resurs-URI.</span><span class="sxs-lookup"><span data-stu-id="8c896-166">Media Services resource URI.</span></span>
- <span data-ttu-id="8c896-167">Azure AD application värden: den **klient-ID** och **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="8c896-167">Azure AD application values: the **Client ID** and **Client secret**.</span></span>

<span data-ttu-id="8c896-168">Värdena för den **klient-ID** och **klienthemlighet** parametrar finns i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c896-168">The values for the **Client ID** and **Client secret** parameters can be found in the Azure portal.</span></span> <span data-ttu-id="8c896-169">Mer information finns i [komma igång med Azure AD-autentisering med hjälp av Azure portal](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-169">For more information, see [Getting started with Azure AD authentication using the Azure portal](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="8c896-170">Följande exempel skapar en token med hjälp av den **AzureAdTokenCredentials** konstruktor som tar **AzureAdClientSymmetricKey** som en parameter:</span><span class="sxs-lookup"><span data-stu-id="8c896-170">The following code example creates a token by using the **AzureAdTokenCredentials** constructor that takes **AzureAdClientSymmetricKey** as a parameter:</span></span> 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

<span data-ttu-id="8c896-171">Du kan också ange den **AzureAdTokenCredentials** konstruktor som tar **AzureAdClientCertificate** som en parameter.</span><span class="sxs-lookup"><span data-stu-id="8c896-171">You can also specify the **AzureAdTokenCredentials** constructor that takes **AzureAdClientCertificate** as a parameter.</span></span> 

<span data-ttu-id="8c896-172">Instruktioner om hur du skapar och konfigurerar ett certifikat i en form som kan användas av Azure AD finns i [autentisera till Azure AD i daemon appar med certifikat - manuella konfigurationsstegen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-172">For instructions about how to create and configure a certificate in a form that can be used by Azure AD, see [Authenticating to Azure AD in daemon apps with certificates - manual configuration steps](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span></span>

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

<span data-ttu-id="8c896-173">Om du vill starta programmera mot Media Services måste du skapa en **CloudMediaContext** -instans som representerar serverkontext.</span><span class="sxs-lookup"><span data-stu-id="8c896-173">To start programming against Media Services, you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="8c896-174">Du måste också ange den **resurs-URI för Media Services för REST** till den **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="8c896-174">You also need to pass the **resource URI for Media REST Services** to the **CloudMediaContext** constructor.</span></span> <span data-ttu-id="8c896-175">Du kan hämta den **resurs-URI för REST medietjänster** värdet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8c896-175">You can get the **resource URI for Media REST Services** value from the Azure portal as well.</span></span>

<span data-ttu-id="8c896-176">Följande exempel skapar en **CloudMediaContext** instans:</span><span class="sxs-lookup"><span data-stu-id="8c896-176">The following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
<span data-ttu-id="8c896-177">I följande exempel visas hur du skapar Azure AD-token och kontexten:</span><span class="sxs-lookup"><span data-stu-id="8c896-177">The following example shows how to create the Azure AD token and the context:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8c896-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c896-178">Next steps</span></span>

<span data-ttu-id="8c896-179">Kom igång med [överföringen av filer till ditt konto](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="8c896-179">Get started with [uploading files to your account](media-services-dotnet-upload-files.md).</span></span>
