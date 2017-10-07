---
title: aaaAccess Azure Media Services API med Azure Active Directory-autentisering | Microsoft Docs
description: "Läs mer om koncept och steg tootake toouse Azure Active Directory (AD Azure) tooauthenticate åtkomst toohello Azure Media Services API."
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
ms.openlocfilehash: bb8f75f39100dc37098260c24ab4a199ef689d46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-hello-azure-media-services-api-with-azure-ad-authentication"></a>Komma åt hello Azure Media Services API med Azure AD-autentisering
 
hello Azure Media Services API är en RESTful-API. Du kan använda den tooperform åtgärder på media resurser med hjälp av REST-API eller genom att använda tillgängliga klient-SDK. Azure Media Services erbjuder en klient för Media Services SDK för Microsoft .NET. toobe behörighet tooaccess Media Services-resurser och hello Media Services API, du måste först autentiseras. 

Media Services stöder [Azure Active Directory (Azure AD)-baserad autentisering](../active-directory/active-directory-whatis.md). hello Azure Media REST-tjänst kräver att hello användare eller ett program som gör hello REST API-begäranden har antingen hello **deltagare** eller **ägare** rollen tooaccess hello resurser. Mer information finns i [Kom igång med rollbaserad åtkomstkontroll i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).  

> [!IMPORTANT]
> Media Services stöder för närvarande hello Azure Access Control service-autentisering. Access Control tillstånd att bli inaktuell på den 1 juni 2018. Vi rekommenderar att du migrerar toohello Azure AD-autentisering så snart som möjligt.

Det här dokumentet ger en översikt över hur tooaccess hello Media Services API med hjälp av REST- eller .NET-API: er.

## <a name="access-control"></a>Åtkomstkontroll

För hello Azure Media REST begäran toosucceed måste hello anropande användaren ha en medarbetare eller ägare roll för hello försök tooaccess Media Services-konto.  
Endast en användare med hello ägarrollen kan ge media (konto) åtkomst toonew resursanvändare eller appar. hello deltagarrollen kan komma åt endast hello mediaresurs.
Obehöriga begäranden misslyckas med statuskoden 401. Om du ser felet måste du kontrollera om användaren har hello deltagare eller ägarrollen tilldelade för hello Media Services användarkonto. Du kan kontrollera detta i hello Azure-portalen. Sök efter media-konto och klicka sedan på hello **åtkomstkontroll** fliken. 

![Access control-fliken](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Typer av autentisering 
 
När du använder Azure AD-autentisering med Azure Media Services, har du två alternativ för autentisering:

- **Användarautentisering**. Autentisera en person som använder hello app toointeract med Media Services-resurser. hello interaktiva program bör först fråga hello användaren om hello användarens autentiseringsuppgifter. Ett exempel är en management konsolapp som används av behöriga användare toomonitor kodning jobb eller live strömning. 
- **Huvudnamn autentiseringen av tjänsten**. Autentisera en tjänst. Program som ofta använder den här autentiseringsmetoden är appar som körs daemon tjänster, mellannivå tjänster eller schemalagda jobb. Exempel är webbappar, funktionen appar, logikappar, API och mikrotjänster.

### <a name="user-authentication"></a>Användarautentisering 

Program som ska använda hello användarautentiseringsmetod är övervakning av interna appar eller hantering: mobila appar, Windows-appar och konsolprogram. Den här typen av lösningen är användbart när du vill mänsklig interaktion med hello-tjänsten på något av följande scenarier hello:

- Instrumentpanelen för övervakning för kodning jobb.
- Instrumentpanelen för övervakning för din direktsända dataströmmar.
- Hanteringsprogram för stationära och mobila användare tooadminister resurser i ett Media Services-konto.

> [!NOTE]
> Den här autentiseringsmetoden bör inte användas för konsumentinriktade program. 

Det ursprungliga programmet måste först skaffa en åtkomst-token från Azure AD och sedan använda den när du gör HTTP-begäranden toohello Media Services REST API. Lägg till hello åtkomst-token toohello huvudet i begäran. 

hello följande diagram visar en typisk interaktiva program autentiseringsflödet: 

![Ursprungliga appar diagram](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

I föregående diagram hello, representerar hello siffror hello flödet av hello begäranden i kronologisk ordning.

> [!NOTE]
> När du använder autentiseringsmetoden för hello användare alla appar dela hello samma (standard) programspecifika klient-ID och det ursprungliga programmet omdirigerings-URI. 

1. Uppmana användaren att ange autentiseringsuppgifter.
2. Begär en åtkomsttoken som Azure AD med hello följande parametrar:  

    * Slutpunkten för Azure AD-klient.

        hello klient information kan hämtas från hello Azure-portalen. Placera markören över hello namnet på hello inloggade användare i hello övre högra hörnet.
    * Media Services resurs-URI. 

        Den här URI är hello samma för Media Services-konton som är i hello samma Azure-miljön (till exempel https://rest.media.azure.net).

    * Media Services (ursprunglig) programmet klient-ID.
    * Media Services (intern)-programmet omdirigerings-URI.
    * Resurs-URI för REST Media Services.
        
        hello URI representerar hello REST API-slutpunkt (till exempel https://test03.restv2.westus.media.azure.net/api/).

    tooget värdena för dessa parametrar finns [använda hello Azure portal tooaccess Azure AD-autentiseringsinställningar](media-services-portal-get-started-with-aad.md) med hello alternativ för autentisering av användare.

3. hello Azure AD-åtkomsttoken skickas toohello klienten.
4. hello klienten skickar en begäran toohello Azure Media REST-API med hello Azure AD-åtkomsttoken.
5. hello-klienten hämtar tillbaka hello data från Media Services.

Information om hur toouse Azure AD authentication toocommunicate med övriga begäranden med hjälp av klient-hello Media Services .NET SDK finns [använda Azure AD authentication tooaccess hello Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md). 

Om du inte använder klient-hello Media Services .NET SDK, måste du manuellt skapa en Tokenbegäran för Azure AD-åtkomst med hjälp av hello parametrar som beskrivs i steg 2. Mer information finns i [hur toouse hello Azure AD Authentication Library tooget hello Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Autentisering av tjänstens huvudnamn

Program som ofta använder den här autentiseringsmetoden är appar som körs på mellannivå tjänster och schemalagda jobb: web apps, funktionen appar, logikappar, API: er och mikrotjänster. Den här autentiseringsmetoden är lämplig för interaktiva program som du kanske vill toouse ett konto toomanage resurser.

När du använder hello service principal autentisering metoden toobuild användarscenarier hanteras autentisering vanligtvis i hello mellannivå (via vissa API) och inte direkt i ett program för mobila eller stationära. 

toouse den här metoden skapar ett Azure AD-program och service principal i sin egen klient. När du har skapat programmet hello ge hello app medarbetare eller ägare roll åtkomst toohello Media Services-konto. Du kan göra detta i hello Azure-portalen med hjälp av Azure CLI eller med ett PowerShell-skript. Du kan också använda ett befintligt Azure AD-program. Du kan registrera och hantera dina Azure AD-program och tjänstens huvudnamn [i hello Azure-portalen](media-services-portal-get-started-with-aad.md). Du också kan göra detta med hjälp av [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) eller [PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Mellannivå appar](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

När du har skapat din Azure AD-program får du värden för hello följande inställningar. Du behöver dessa värden för autentisering:

- Klient-ID 
- Klienthemlighet 

I föregående bild hello, representerar hello siffror hello flödet av hello begäranden i kronologisk ordning:
    
1. En mellannivå app (web API eller webbprogram) begär en Azure AD-åtkomsttoken som har hello följande parametrar:  

    * Slutpunkten för Azure AD-klient.

        hello klient information kan hämtas från hello Azure-portalen. Placera markören över hello namnet på hello inloggade användare i hello övre högra hörnet.
    * Media Services resurs-URI. 

        Den här URI är hello samma för Media Services-konton som finns i hello samma Azure-miljön (till exempel https://rest.media.azure.net).

    * Resurs-URI för REST Media Services.

        hello URI representerar hello REST API-slutpunkt (till exempel https://test03.restv2.westus.media.azure.net/api/).

    * Azure AD application värden: hello klient-ID och klienthemlighet.
    
    tooget värdena för dessa parametrar finns [använda hello Azure portal tooaccess Azure AD-autentiseringsinställningar](media-services-portal-get-started-with-aad.md) med hjälp av alternativ för hello service principal autentisering.

2. hello Azure AD-åtkomsttoken skickas toohello mellannivå.
4. hello mellannivå skickar begäran toohello Azure Media REST-API med hello Azure AD-token.
5. hello mellannivå hämtar tillbaka hello data från Media Services.

Mer information om hur toouse Azure AD authentication toocommunicate med övriga begäranden med hjälp av klient-hello Media Services .NET SDK finns [använda Azure AD authentication tooaccess Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md). 

Om du inte använder klient-hello Media Services .NET SDK, måste du manuellt skapa en Azure AD-tokenbegäran med parametrar som beskrivs i steg 1. Mer information finns i [hur toouse hello Azure AD Authentication Library tooget hello Azure AD-token](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Felsökning

Undantag ”: hello fjärrservern returnerade ett fel: (401) obehörig”.

Lösning: För hello Media Services REST begäran toosucceed hello anropande användaren måste vara en medarbetare eller ägare roll i hello försök tooaccess Media Services-konto. Mer information finns i hello [åtkomstkontroll](media-services-use-aad-auth-to-access-ams-api.md#access-control) avsnitt.

## <a name="resources"></a>Resurser

hello följande artiklar finns översikter över begrepp för Azure AD-autentisering: 

- [Autentiseringsscenarier åtgärdas av Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md#basics-of-authentication-in-azure-ad)
- [Lägga till, uppdatera eller ta bort ett program i Azure AD](../active-directory/develop/active-directory-integrating-applications.md)
- [Konfigurera och hantera rollbaserad åtkomstkontroll med hjälp av PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)

## <a name="next-steps"></a>Nästa steg

* Använd hello Azure-portalen för[åtkomst till Azure AD authentication tooconsume Azure Media Services API](media-services-portal-get-started-with-aad.md).
* Använda Azure AD-autentisering för[åtkomst till Azure Media Services-API med .NET](media-services-dotnet-get-started-with-aad.md).

