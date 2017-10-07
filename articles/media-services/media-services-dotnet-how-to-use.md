---
title: "aaaHow tooSet in datorn för Media Services-utveckling med .NET"
description: "Läs mer om hello krav för Media Services med hello Media Services SDK för .NET. Lär dig också hur toocreate en app i Visual Studio."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: juliako
ms.openlocfilehash: a5a2af3211d8156fd7dea99831fb769df4130d41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-development-with-net"></a>Media Services-utveckling med .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Det här avsnittet beskrivs hur toostart utveckla Media Services-program med hjälp av .NET.

Hej **Azure Media Services .NET SDK** biblioteket kan du tooprogram mot Media Services med hjälp av .NET. det även enklare toodevelop med .NET, hello toomake **Azure Media Services .NET SDK-tilläggen** bibliotek har angetts. Det här biblioteket innehåller en uppsättning tilläggsmetoder och hjälpfunktioner som förenklar .NET-kod. Båda bibliotek är tillgängligt via **NuGet** och **GitHub**.

## <a name="prerequisites"></a>Krav
* Ett Media Services-konto i en ny eller befintlig Azure-prenumeration. Avsnittet hello [hur tooCreate Media Services-konto](media-services-portal-create-account.md).
* Operativsystem: Windows 10, Windows 7, Windows 2008 R2 eller Windows 8.
* .NET framework 4.5.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt
Det här avsnittet beskrivs hur du toocreate ett projekt i Visual Studio och ställa in det för Media Services-utveckling.  I det här fallet hello projektet är ett Windows C#-konsolprogram, men hello samma konfigurationsstegen som visas här gäller tooother typer av projekt som du kan skapa för Media Services-program (till exempel en Windows Forms-program eller ASP.NET-webbprogram).

Det här avsnittet visas hur toouse **NuGet** tooadd Media Services .NET SDK-tillägg och andra beroende bibliotek.

Alternativt kan du hämta hello senaste Media Services .NET SDK bits från GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) eller [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), skapa hello lösning och lägga till hello referenser toohello klientprojekt. Alla nödvändiga beroenden för hello hämta har hämtat och extraherat automatiskt.

1. Skapa ett nytt C#-konsolprogram i Visual Studio. Ange hello **namn**, **plats**, och **lösningsnamn**, och klicka sedan på OK.
2. Skapa hello lösning.
3. Använd **NuGet** tooinstall och Lägg till **Azure Media Services .NET SDK-tilläggen** (**windowsazure.mediaservices.extensions**). När du installerar det här paketet installeras även **Media Services .NET SDK** och lägger till alla andra nödvändiga beroenden.
   
    Se till att du har hello senaste versionen av NuGet installerad. Mer information och installationsanvisningar finns i [NuGet](http://nuget.codeplex.com/).
4. Högerklicka på hello namnet på hello projektet i Solution Explorer och välj Hantera NuGet-paket.
   
    hello dialogruta hantera NuGet-paket.
5. Välj Azure Media Services .NET SDK-tilläggen hello Online Gallery och Sök efter Azure MediaServices tillägg, och klicka hello installation.
   
    hello-projektet har ändrats och refererar till toohello Media Services .NET SDK-tillägg, Media Services .NET SDK och andra beroende sammansättningar har lagts till.
6. toopromote en tydligare utvecklingsmiljö överväga att aktivera NuGet-Paketåterställning. Mer information finns i [NuGet-Paketåterställning ”](http://docs.nuget.org/consume/package-restore).
7. Lägg till en referens för**System.Configuration** sammansättning. Den här sammansättningen innehåller hello System.Configuration. **ConfigurationManager** klassen som används tooaccess configuration-filer (till exempel App.config).
   
    tooadd referenser hello hanterar referenser dialogrutan, högerklicka på hello projektnamnet i hello Solution Explorer. Välj sedan Lägg till och referenser.
   
    hello visas hanterar referenser.
8. Under .NET framework-sammansättningar, hitta Markera hello System.Configuration sammansättningen och tryck på OK.
9. Öppna hello App.config-filen och Lägg till en *appSettings* toohello fil.     
   
    Ange hello-värden som är nödvändiga tooconnect toohello Media Services API. Mer information finns i [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

    Om du använder [användarautentisering](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) config-fil kommer förmodligen ha värden för din Azure AD-klient domän och hello AMS REST API-slutpunkt.
    
    >[!Important]
    >De flesta kodexempel i hello Azure Media Services dokumentationen inställd och använda en användare (interaktiva) typ av autentisering tooconnect toohello AMS API. Den här autentiseringsmetoden fungerar bra för hantering och övervakning av interna appar: mobila appar, Windows-appar och konsolprogram. Den här autentiseringsmetoden är inte lämpligt för server, webbtjänster, API: er för typ av program.  Mer information finns i [åtkomst hello AMS API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. Skriv över befintliga hello **med** instruktioner hello början av hello Program.cs-filen med hello följande kod.
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

Nu är du redo toostart utvecklingen av ett Media Services-program.    

## <a name="example"></a>Exempel

Här är en liten exempel som ansluter toohello AMS API och visar alla tillgängliga Media-processorer.
    
    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a>Nästa steg

Nu [kan du ansluta toohello AMS API](media-services-use-aad-auth-to-access-ams-api.md) och starta [utveckla](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

