---
title: aaaClient och server SDK versionshantering i Mobile Apps och Mobile Services | Microsoft Docs
description: "Lista över klient-SDK: er och kompatibilitet med server SDK-versioner för Mobile Services och Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5874b7455ea407ca8c77fb1bd03d97d0767ebb47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Klienten och servern versionshantering i Mobile Apps och Mobile Services
hello senaste versionen av Azure Mobile Services är hello **Mobile Apps** funktion i Azure App Service.

Hej Mobilappar klient och server SDK ursprungligen baseras på de i Mobile Services, men de är *inte* kompatibla med varandra.
Det vill säga måste du använda en *Mobilappar* klient-SDK med en *Mobilappar* server-SDK och på samma sätt för *Mobile Services*. Det här kontraktet tillämpas via en särskild huvudvärde används av hello klient och server SDK: er, `ZUMO-API-VERSION`.

Obs: när det här dokumentet refererar tooa *Mobile Services* serverdel det inte nödvändigtvis behöver toobe finns i Mobile Services. Det är nu möjligt toomigrate en Mobiltjänst toorun på Apptjänst utan någon kodändringar men hello tjänsten skulle fortfarande använder *Mobile Services* SDK-versioner.

toolearn mer om migrera tooApp tjänsten utan ändringar koden finns hello artikel [migrera en Mobiltjänst tooAzure Apptjänst].

## <a name="header-specification"></a>Specifikation för sidhuvud
hello nyckeln `ZUMO-API-VERSION` kan anges i hello HTTP-sidhuvud eller frågesträng hello. hello-värdet är en versionssträng i form av hello **x.y.z**.

Exempel:

Hämta https://service.azurewebsites.net/tables/TodoItem

HUVUDEN: ZUMO-API-VERSION: 2.0.0

Bokför https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Väljer bort versionskontroll
Du kan välja bort versionskontroll genom att ange ett värde för **SANT** för hello appinställningen **MS_SkipVersionCheck**. Ange det i filen web.config eller i hello delen programinställningar i hello Azure-portalen.

> [!NOTE]
> Det finns ett antal funktionsändringar mellan Mobile Services och Mobilappar, särskilt i områden som hello offlinesynkronisering, autentisering och push-meddelanden. Du bör endast välja bort versionskontroll efter slutförd tester tooensure att dessa förändringar i beteendet inte bryter appens funktioner.
>
>

## <a name="summary-of-compatibility-for-all-versions"></a>Sammanfattning av kompatibilitet för alla versioner
hello diagrammet nedan visar hello kompatibilitet mellan alla typer av klienten och servern. En serverdel klassificeras som antingen Mobile **Services** eller Mobile **appar** baserat på server-hello SDK som används.

|  | **Mobiltjänster** Node.js eller .NET | **Mobilappar** Node.js eller .NET |
| --- | --- | --- |
| [Klienter för Mobile Services] |Okej |Fel\* |
| [Mobile Apps-klienter] |Fel\* |Okej |

\*Detta kan kontrolleras genom att ange **MS_SkipVersionCheck**.

<!-- IMPORTANT!  hello anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: hello fwlink toothis document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobile Services-klienten och servern
hello klienten SDK: er i hello nedan är kompatibla med **Mobile Services**.

Obs: hello Mobile Services klient-SDK: *inte* skicka ett värde för `ZUMO-API-VERSION`. Om tjänsten för hello får det här sidhuvudet eller frågesträngsvärdet ett fel returneras om du uttryckligen har valt ut enligt beskrivningen ovan.

### <a name="MobileServicesClients"></a>Mobila *Services* client SDK
| Klientplattform | Version | Värdet för versionshuvudet |
| --- | --- | --- |
| Hanterad klient (Windows, Xamarin) |[1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |Saknas |
| iOS |[2.2.2](http://aka.ms/gc6fex) |Saknas |
| Android |[2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) |Saknas |
| HTML |[1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |Saknas |

### <a name="mobile-services-server-sdks"></a>Mobila *Services* server SDK
| Server-plattformen | Version | Godkänd version-huvud |
| --- | --- | --- |
| .NET |[WindowsAzure.MobileServices.Backend.* Version 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |** Inga versionshuvud ** |
| Node.js |(kommer snart) |**Ingen version-huvud** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Beteendet för Mobile Services serverdelar
| ZUMO-API-VERSION | Värdet för MS_SkipVersionCheck | Svar |
| --- | --- | --- |
| Inte angetts |Alla |200 - OK |
| Inget värde |True |200 - OK |
| Inget värde |FALSKT/inte angetts |400 – Felaktig begäran |

## <a name="2.0.0"></a>Azure Mobile Apps klient och server
### <a name="MobileAppsClients"></a>Mobila *appar* client SDK
Versionskontroll introducerades med hello följande versioner av hello klient-SDK för **Azure Mobile Apps**:

| Klientplattform | Version | Värdet för versionshuvudet |
| --- | --- | --- |
| Hanterad klient (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobila *appar* server SDK
Versionskontroll ingår i följande versioner av server-SDK:

| Server-plattformen | SDK | Godkänd version-huvud |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[Azure mobile apps)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Beteendet för serverdelar för Mobilappar
| ZUMO-API-VERSION | Värdet för MS_SkipVersionCheck | Svar |
| --- | --- | --- |
| x.y.z eller Null |True |200 - OK |
| Null |FALSKT/inte angetts |400 – Felaktig begäran |
| 1.x.y |FALSKT/inte angetts |400 – Felaktig begäran |
| 2.0.0-2.x.y |FALSKT/inte angetts |200 - OK |
| 3.0.0-3.x.y |FALSKT/inte angetts |400 – Felaktig begäran |

## <a name="next-steps"></a>Nästa steg
* [migrera en Mobiltjänst tooAzure Apptjänst]

[Klienter för Mobile Services]: #MobileServicesClients
[Mobile Apps-klienter]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[migrera en Mobiltjänst tooAzure Apptjänst]: app-service-mobile-migrating-from-mobile-services.md
