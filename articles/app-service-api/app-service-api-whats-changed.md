---
title: aaaApp Service API Apps - Nyheter | Microsoft Docs
description: "Lär dig vad är nytt för API Apps i Azure App Service."
services: app-service\api
documentationcenter: .net
author: mohitsriv
manager: erikre
editor: tdykstra
ms.assetid: a9b58066-e8fd-48b8-a651-4613b1736433
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: rachelap
ms.openlocfilehash: 79df54f1dae91d7c5d3b66d208d0d1c1d7d55ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-api-apps---whats-changed"></a>Apptjänst API Apps - Nyheter
Vid hello Connect() händelse i November 2015, var ett antal förbättringar tooAzure Apptjänst [meddelats](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Dessa förbättringar omfattar underliggande ändringar tooAPI appar toobetter överensstämmer med mobila och Web Apps, minska antalet begrepp och förbättra prestanda för distribution och körning. Starta 30 November 2015 nya API-appar du skapar med hjälp av hello Azure management portal eller hello senaste verktygsuppsättning visas ändringarna. Den här artikeln beskriver dessa ändringar samt hur tooredeploy befintliga appar tootake nytta av hello-funktioner.

## <a name="feature-changes"></a>Funktionen ändringar
hello viktiga funktioner i API Apps – autentisering, CORS och API-metadata – har flyttat direkt i Apptjänst. Med den här ändringen är hello funktioner tillgängliga för webb-, mobil- och API Apps. Faktum är alla tre resursen hello samma **Microsoft.Web/sites** resurstypen i Resource Manager. hello API Apps gateway är inte längre behövs eller erbjuds med API Apps. Detta gör det också lättare toouse Azure API Management eftersom det är bara hello enda API Management gateway.

![Översikt över API-appar](./media/app-service-api-whats-changed/api-apps-overview.png)

En princip för viktiga designbeslut med API Apps uppdatera hello är tooenable toobring din API som finns i ditt språk.  Om din API är redan distribuerad som en Webbapp eller Mobilapp, har du inte tooredeploy din app tootake nytta av nya funktioner för hello. Om du redan är på API Apps preview detaljerad vägledning för migrering nedan.

### <a name="authentication"></a>Autentisering
hello befintliga NYCKELFÄRDIGT API Apps, Mobile Services/appar och Web Apps autentiseringsfunktioner har tagits enhetlig och är tillgängliga i ett enda Azure App Service autentisering blad i hello-hanteringsportalen. En introduktion tooauthentication services i App Service finns [expanderande Apptjänst autentisering / auktorisering](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Det finns ett antal relevanta nya funktioner för API-scenarier:

* **Stöd för användning av Azure Active Directory direkt**, utan klientkod med tooexchange hello AAD-token för en sessionstoken: klienten kan bara innehålla hello AAD-token i hello Authorization-huvud, enligt toohello ägar-token specifikation. Detta betyder också att inga App Service-specifik SDK krävs på hello klient eller server-sida. 
* **Tjänst-till-tjänst-eller ”interna”**: Om du har en demonprocess eller en annan klient som behöver åtkomst tooAPIs utan ett gränssnitt, kan du begära en token med hjälp av en AAD-tjänstens huvudnamn och överför den tooApp Service för autentisering med din programmet.
* **Uppskjutna auktorisering**: många program har olika åtkomstbegränsningar för olika delar av programmet hello. Du vill kanske vissa toobe för API: er som är offentligt tillgänglig, medan andra kräver inloggning. hello ursprungliga autentisering/auktorisering i funktionen var alternativ, med hello hela webbplatsen kräver inloggning. Det här alternativet finns fortfarande, men du kan också tillåta programkoden toorender åtkomst beslut när Apptjänst har autentiserats hello användare.

Läs mer om hello nya autentiseringsfunktioner [autentisering och auktorisering för API Apps i Azure App Service](app-service-api-authentication.md). Information om hur toomigrate befintliga API apps från hello tidigare API apps modellen toohello nya en, se [migrera befintliga API apps](#migrating-existing-api-apps) senare i den här artikeln.

### <a name="cors"></a>CORS
I stället för en kommaavgränsad **MS_CrossDomainOrigins** app inställningen genom att det finns nu ett blad i hello Azure-hanteringsportalen för att konfigurera CORS. Alternativt kan den konfigureras med Resource Manager tooling som Azure PowerShell, CLI eller [Resursläsaren](https://resources.azure.com/). Ange hello **cors** egenskapen hello **Microsoft.Web/sites/config** resurstypen för din  **&lt;platsnamn&gt;/webb-** resurs. Exempel:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-metadata
hello API definition bladet är nu tillgänglig i webb-, mobil- och API Apps. Du kan ange en relativ url eller en absolut url pekar tooan slutpunkt som värdar Swagger 2.0-representation av din API i hello-hanteringsportalen. Alternativt kan konfigureras med hjälp av hanteraren för filserverresurser verktygsuppsättning. Ange hello **apiDefinition** egenskapen hello **Microsoft.Web/sites/config** resurstypen för din  **&lt;platsnamn&gt;/webb-** resurs. Exempel:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

För tillfället hello metadataslutpunkten behöver toobe offentligt tillgänglig utan autentisering för många underordnade klienter (t.ex. Visual Studio REST API-klient generation och PowerApps ”Lägg till API” flöde) tooconsume den. Detta innebär att om du använder App Service-autentisering och vill tooexpose hello API-definition från i din app sig själv, måste toouse hello uppskjutna autentiseringsalternativet som beskrivs ovan, så att hello flödet tooyour Swagger-metadata är offentlig.

## <a name="management-portal"></a>Hanteringsportal
Att välja **Ny > webb + mobilt > API-App** i hello portal skapar API-appar som visar hello nya funktioner som beskrivs i artikel hello. **Bläddra > API Apps** visar endast dessa nya API-appar. När du bläddrar till en API-app hello hello bladet resurser samma layout och funktioner som webb- och Mobilappar. hello bara skillnaderna är quickstart innehåll och Standardordning för inställningar.

Befintliga API-appar (eller Marketplace API apps som skapats från Logic Apps) med hello kapaciteten för förhandsversionen av föregående fortfarande visas i hello Logic Apps designer och när du bläddrar alla resurser i en resursgrupp.

## <a name="visual-studio"></a>Visual Studio
De flesta Web Apps verktygsuppsättning fungerar med den nya API apps, eftersom de delar hello samma underliggande **Microsoft.Web/sites** resurstypen. hello Azure Visual Studio tooling, bör dock uppgraderade tooversion 2.8.1 eller senare eftersom den exponerar ett antal funktioner specifika tooAPIs. Hämta hello SDK från hello [Azure nedladdningar](https://azure.microsoft.com/downloads/).

Med hello rationalisering av hello App tjänsttyper, publicera också unified under **publicera > Microsoft Azure App Service**:

![Publicera API Apps](./media/app-service-api-whats-changed/api-apps-publish.png)

Mer information om SDK 2.8.1, skrivskyddade hello-meddelande toolearn [blogginlägget](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Alternativt kan du manuellt importera hello publiceringsprofil från hello management portal tooenable publicera. Cloud Explorer, kodgenerering och API val/skapa en app kräver dock SDK 2.8.1 eller högre.

## <a name="migrating-existing-api-apps"></a>Migrera befintliga API-appar
Om din anpassade API är distribuerade toohello tidigare förhandsversionen av API Apps, begära att migrera toohello ny modell för API Apps med den 31 December 2015. Eftersom både hello gamla och nya modellen baseras på webb-API: er finns i App Service, kan hello merparten av befintlig kod återanvändas.

### <a name="hosting-and-redeployment"></a>Värd för och Omdistributionen
hello steg för att omdistribuera är hello samma som distribuerar eventuella befintliga Web API-tooApp Service. Så här:

1. Skapa en tom API-app. Detta kan göras i hello-portal med en ny > API-App i Visual Studio från publicera eller från hanteraren för filserverresurser verktygsuppsättning. Om du använder Resource Manager verktygsuppsättning eller mallar, ange hello **typ** värdet för**api** på hello **Microsoft.Web/sites** resurs typen toohave hello Snabbstart och inställningar i Hej hanteringsportalen inriktade mot API-scenarier.
2. Ansluta och distribuera projektet toohello tom API-appen med hjälp av hello distribution mekanismer som stöds av App Service. Läs [dokumentationen för Azure App Service](../app-service-web/web-sites-deploy.md) toolearn mer. 

### <a name="authentication"></a>Autentisering
Hej Apptjänst services autentiseringsstöd hello samma funktioner som var tillgängliga med hello tidigare API Apps modellen. Om du använder session token och kräver SDK Använd hello efter klient- och SDK:

* Klient: [Azure mobila klient-SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
* Server: [Autentiseringstillägget för Microsoft Azure Mobilappar .NET](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Om du använde i stället hello Apptjänst alpha SDK: er, är dessa nu föråldrade:

* Klient: [Microsoft Azure Apptjänst SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
* Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Särskilt med Azure Active Directory krävs men inga App Service-specifik om du använder hello AAD-token direkt.

### <a name="internal-access"></a>Intern åtkomst
hello tidigare API Apps modellen med en inbyggd interna åtkomstnivå. Detta krävs användning av hello SDK för att logga förfrågningar. Enligt beskrivningen tidigare, med hello nya API Apps modellen, kan AAD tjänstens huvudnamn användas som ett alternativ för service to service autentisering utan att en App Service-specifik SDK. Läs mer i [Service principal autentisering för API Apps i Azure App Service](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Identifiering
hello tidigare API Apps modell hade API: er för identifiering av andra API apps vid körning i hello samma resursgrupp bakom hello samma gateway. Detta är särskilt användbart i arkitekturer som implementerar mikrotjänster mönster. När detta inte stöds direkt, finns ett antal alternativ:

1. Använd hello Azure Resource Manager API för identifiering.
2. Placera Azure API Management framför dina Apptjänst-värdbaserad API: er. Azure API Management fungerar som en facade och kan ge en stabil externa Internetriktade URL: en även om interna topologi ändras.
3. Skapa din egen identifiering API-app och har andra API apps som registreras med hello identifiering app vid start.
4. Vid tidpunkten för distribution du fylla hello app-inställningar för alla hello API apps (och klienter) med hello slutpunkter av hello andra API apps. Detta är användbart i mallen distributioner och eftersom API Apps nu ger dig kontroll över hello-url.

## <a name="using-api-apps-with-logic-apps"></a>Med API Apps med Logic Apps
hello nya API apps modellen fungerar bra med [Logic Apps schemaversionen 2015-08-01](../logic-apps/logic-apps-schema-2015-08-01.md).

## <a name="next-steps"></a>Nästa steg
toolearn läsa fler hello artiklar i hello [API Apps dokumentationsavsnitt](https://azure.microsoft.com/documentation/services/app-service/api/). De har uppdaterats tooreflect hello ny modell för API Apps. Dessutom nå på hello-forum för ytterligare information eller information om migrering:

* [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-api-apps)

