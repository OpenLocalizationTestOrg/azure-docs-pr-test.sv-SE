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
# <a name="use-azure-ad-authentication-tooaccess-azure-media-services-api-with-net"></a>Använda Azure AD authentication tooaccess Azure Media Services API med .NET

Från och med windowsazure.mediaservices 4.0.0.4 stöder Azure Media Services autentisering baserat på Azure Active Directory (AD Azure). Det här avsnittet beskrivs hur du toouse Azure AD authentication tooaccess Azure Media Services-API med Microsoft .NET.

## <a name="prerequisites"></a>Krav

- Ett Azure-konto. Mer information finns i avsnittet om [den kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Ett Media Services-konto. Mer information finns i [skapa ett Azure Media Services-konto med hello Azure-portalen](media-services-portal-create-account.md).
- Hej senaste [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketet.
- Om du är bekant med hello avsnittet [åtkomst till Azure Media Services API med AAD autentiseringsöversikt](media-services-use-aad-auth-to-access-ams-api.md). 

När du använder Azure AD-autentisering med Azure Media Services kan autentisera du på något av två sätt:

- **Användarautentisering** autentiserar en person som använder hello app toointeract med Azure Media Services-resurser. hello interaktiva program bör först fråga hello användaren om autentiseringsuppgifter. Ett exempel är en management konsolapp som används av behöriga användare toomonitor kodning jobb eller direktsänd strömning. 
- **Autentiseringen av tjänsten huvudnamn** autentiserar en tjänst. Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagt jobb, t.ex webbprogram, funktionen appar, logikappar, API: er eller mikrotjänster.

>[!IMPORTANT]
>Azure Media Service stöder för närvarande en modell för autentisering av Azure Access Control Service. Access Control-auktorisering försätts toobe föråldrad på den 1 juni 2018. Vi rekommenderar att du migrerar tooan Azure Active Directory-autentisering så snart som möjligt.

## <a name="get-an-azure-ad-access-token"></a>Hämta en Azure AD-åtkomst-token

tooconnect toohello Azure Media Services API med Azure AD authentication hello-klientappen måste toorequest en Azure AD-åtkomsttoken. När du använder klienten för hello Media Services .NET SDK många hello information om hur tooacquire en Azure AD-åtkomsttoken omsluten och förenklad för dig i hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) och [AzureAdTokenCredentials ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) klasser. 

Till exempel behöver du inte tooprovide hello Azure AD myndighet, Media Services resurs-URI eller intern programinformation för Azure AD. Detta är välkända värden som redan har konfigurerats av hello leverantörsklass för Azure AD åtkomst-token. 

Om du inte använder Azure Media Service .NET SDK, rekommenderar vi att du använder hello [Azure AD-Autentiseringsbiblioteket](../active-directory/develop/active-directory-authentication-libraries.md). tooget värden för hello parametrar som du behöver toouse med Azure AD Authentication Library finns [använda hello Azure portal tooaccess Azure AD-autentiseringsinställningar](media-services-portal-get-started-with-aad.md).

Du har också hello möjlighet att ersätta hello standardimplementering av hello **AzureAdTokenProvider** med din egen implementering.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Installera och konfigurera Azure Media Services .NET SDK

>[!NOTE] 
>toouse Azure AD-autentisering med hello Media Services .NET SDK, behöver du toohave hello senaste [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) paketet. Lägg även till en referens toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** sammansättning. Om du använder en befintlig app, inkludera hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** sammansättning. 

1. Skapa ett nytt C#-konsolprogram i Visual Studio.
2. Använd hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet-paketet tooinstall **Azure Media Services .NET SDK**. 

    tooadd referenser med hjälp av NuGet, ta hello följande: i **Solution Explorer**, högerklicka på projektnamnet hello och väljer sedan **hantera NuGet-paket**. Sök sedan efter **windowsazure.mediaservices** och välj **installera**.
    
    ELLER

    Kör hello följande kommando i **Pakethanterarkonsolen** i Visual Studio.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Lägg till **med** tooyour källkoden.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Använd autentisering av användare

tooconnect toohello Azure Media Service API med hello användaren autentiseringsalternativet hello-klientappen måste toorequest en Azure AD-token med hjälp av hello följande parametrar:  

- Slutpunkten för Azure AD-klient. hello klient information kan hämtas från hello Azure-portalen. Hovra över hello inloggade användaren i hello övre högra hörnet.
- Media Services resurs-URI.
- Media Services (ursprunglig) programmet klient-ID. 
- Media Services (intern)-programmet omdirigerings-URI. 

hello värdena för dessa parametrar finns i **AzureEnvironments.AzureCloudEnvironment**. Hej **AzureEnvironments.AzureCloudEnvironment** konstant är en hjälp i hello .NET SDK tooget hello rätt inställningar för miljövariabeln en offentlig Azure-datacenter. 

Den innehåller fördefinierade inställningar för åtkomst till Media Services i hello offentliga data datacenter. Du kan använda för statliga eller statlig molnområdena **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, eller **AzureGermanCloudEnvironment** respektive.

Följande kodexempel hello skapar en token:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
toostart programmera mot Media Services behöver du toocreate en **CloudMediaContext** -instans som representerar hello serverkontext. Hej **CloudMediaContext** innehåller referenser tooimportant samlingar inklusive jobb, tillgångar, filer, principer för åtkomst och lokaliserare. 

Du måste också toopass hello **resurs-URI för REST medietjänster** toohello **CloudMediaContext** konstruktor. tooget hello resurs-URI för REST medietjänster inloggning toohello Azure portal, väljer du Azure Media Services-kontot, Välj **API-åtkomst**, och välj sedan **ansluta tooAzure Media Services med användare autentisering**. 

hello följande exempel skapar en **CloudMediaContext** instans:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

hello som följande exempel visar hur toocreate hello Azure AD-token och hello kontext:

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
>Om du får ett undantag med texten ”hello fjärrservern returnerade ett fel: (401) obehörig” finns hello [åtkomstkontroll](media-services-use-aad-auth-to-access-ams-api.md#access-control) avsnitt för att få åtkomst till Azure Media Services API med översikt över Azure AD-autentisering.

## <a name="use-service-principal-authentication"></a>Använd service principal autentisering
    
tooconnect toohello Azure Media Services API med hello service principal alternativet, måste appen mellannivå (web API eller webbprogram) toorequests en Azure AD-token med hello följande parametrar:  

- Slutpunkten för Azure AD-klient. hello klient information kan hämtas från hello Azure-portalen. Hovra över hello inloggade användaren i hello övre högra hörnet.
- Media Services resurs-URI.
- Azure AD application värden: hello **klient-ID** och **klienthemlighet**.

Hej värden för hello **klient-ID** och **klienthemlighet** parametrar finns i hello Azure-portalen. Mer information finns i [komma igång med Azure AD authentication hello Azure-portalen](media-services-portal-get-started-with-aad.md).

hello följande exempel skapar en token med hjälp av hello **AzureAdTokenCredentials** konstruktor som tar **AzureAdClientSymmetricKey** som en parameter: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

Du kan också ange hello **AzureAdTokenCredentials** konstruktor som tar **AzureAdClientCertificate** som en parameter. 

Anvisningar om hur toocreate och konfigurera ett certifikat i en form som kan användas av Azure AD, se [autentisera tooAzure AD i daemon appar med certifikat - manuella konfigurationsstegen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

toostart programmera mot Media Services behöver du toocreate en **CloudMediaContext** -instans som representerar hello serverkontext. Du måste också toopass hello **resurs-URI för REST medietjänster** toohello **CloudMediaContext** konstruktor. Du kan hämta hello **resurs-URI för REST medietjänster** värde från hello Azure portal samt.

hello följande exempel skapar en **CloudMediaContext** instans:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
hello som följande exempel visar hur toocreate hello Azure AD-token och hello kontext:

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

## <a name="next-steps"></a>Nästa steg

Kom igång med [Överför filer tooyour konto](media-services-dotnet-upload-files.md).
