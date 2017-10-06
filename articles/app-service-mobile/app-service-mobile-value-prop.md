---
title: aaaAbout Mobile Apps i Azure App Service
description: "Lär dig mer om hello fördelar att Apptjänst ger tooyour enterprise mobile-appar."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: ab96af11c3df196acfb9ecec1339e7f6093c7066
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started"> </a>Om Mobile Apps i Azure App Service
Azure App Service är ett helt hanterat [PaaS](https://azure.microsoft.com/overview/what-is-paas/)-erbjudande (plattform som en tjänst) för professionella utvecklare. hello tjänsten ger en omfattande uppsättning tooweb-, mobil- och integrering scenarier. 

hello Mobile Apps-funktionen i Azure App Service ger utvecklare av företagsprogram och system dietetisk mobile-programmet plattform som är mycket skalbart och globalt tillgänglig.

![Visuell översikt över Mobile Apps-funktioner](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Varför Mobile Apps?
Med hello Mobile Apps-funktionen kan du:

* **Bygga plattformsspecifika och plattformsoberoende appar**: Oavsett om du bygger appar särskilt för iOS, Android eller Windows eller plattformsoberoende Xamarin- eller Cordova-appar (PhoneGap) kan du använda App Service med plattformsspecifika SDK:er.
* **Ansluta tooyour företagssystem**: med hello Mobile Apps-funktionen kan du lägga till företagets inloggning i minuter, och ansluta tooyour enterprise lokalt eller molnresurser.
* **Skapa offline-appar med datasynkronisering**: gör den mobila arbetsstyrkan mer produktiva genom att skapa appar som fungerar offline och använda Mobile Apps toosync data i bakgrunden hello när anslutning med någon av dina datakällor i företaget eller programvara som en tjänst (SaaS) API: er.
* **Push-meddelanden toomillions i sekunder**: engagera kunderna med direkta push-meddelanden på alla enheter anpassat tootheir behov och skickas när hello tid är rätt.

## <a name="mobile-apps-features"></a>Funktioner i Mobile Apps
hello följande funktioner är viktiga toocloud-aktiverade mobilutveckling:

* **Autentisering och auktorisering**: Du kan välja mellan ett ständigt växande antal identitetsprovidrar, till exempel Azure Active Directory för företagsautentisering, och även leverantörer via sociala nätverk som Facebook, Google, Twitter eller Microsoft-konton. Mobile Apps erbjuder en OAuth 2.0-tjänst för varje provider. Du kan också integrera hello SDK för hello identitetsleverantör för provider-specifik funktion.

    Läs mer om våra [autentiseringsfunktioner].

* **Dataåtkomst**: Mobilappar erbjuder en mobilvänlig OData v3-datakälla som har länkade tooAzure SQL-databas eller en lokal SQLServer. Eftersom här tjänsten kan byggas på Entity Framework kan du lätt kan integrera med andra NoSQL- och SQL-dataleverantörer, till exempel [Azure Table Storage], MongoDB, [Azure Cosmos DB] och SaaS-API-leverantörer som Office 365 och Salesforce.com.

* **Offlinesynkronisering**: klient-SDK: er gör det enkelt toobuild robust och känslig mobilappar som fungerar med en offline-datamängd. Du kan synkronisera den här datauppsättningen automatiskt med hello backend-data, inklusive konfliktlösning stöd.

  Läs mer om våra [datafunktioner].

* **Push-meddelanden**: klient-SDK: er integreras sömlöst med hello registreringsfunktionerna i Azure Notification Hubs, så att du kan skicka push-meddelanden toomillions användare samtidigt.

  Läs mer om våra [push-meddelandefunktioner].

* **Klient-SDK:er**: Vi erbjuder en komplett uppsättning klient-SDK:er för plattformsspecifik utveckling ([iOS], [Android] och [Windows]), plattformsoberoende utveckling ([Xamarin.iOS och Xamarin.Android], [Xamarin Forms]) och hybridapputveckling ([Apache Cordova]). Alla klient-SDK:er fås med MIT-licens och har öppen källkod.

## <a name="azure-app-service-features"></a>Funktioner i Azure App Service
hello följande funktioner är användbara i produktionsmiljöer för mobilappar:

* **Autoskalning**: med App Service kan du snabbt skala upp eller skala ut toohandle inkommande kundbelastning. Manuellt väljer hello antalet och storleken på virtuella datorer eller ställa in autoskalning tooscale din mobilapp serverdel utifrån belastning eller schema.

  Läs mer om [automatisk skalning].

* **Mellanlagringsmiljöer** – Med App Service kan du köra flera olika versioner av webbplatsen så att du kan genomföra A- och B-testning, testa i produktionsmiljö som en del i en större DevOps-plan och mellanlagra en ny serverdel direkt på plats.

  Läs mer om [mellanlagringsmiljöer].

* **Kontinuerlig distribution**: Apptjänst kan integreras med vanliga ange kedjan (SCM) hanteringssystem, så du kan distribuera en ny version av din serverdel automatiskt genom att trycka på tooa gren i SCM-systemet.

  Läs mer om [distributionsalternativ].

* **Virtuella nätverk**: Apptjänst kan ansluta tooon lokala resurser med hjälp av anslutningar för virtuella nätverk, Azure ExpressRoute eller hybridanslutningar.

  Läs mer om [hybridanslutningar], [virtuella nätverk] och [ExpressRoute].

* **Isolerade/dedikerade miljöer**: Du kan köra App Service i en helt isolerad och dedikerad miljö för säker körning av storskaliga Azure App Service-appar. Miljön är perfekt för programarbetsbelastningar som kräver storskalighet, isolering eller säker nätverksåtkomst.

  Läs mer om [App Service-miljöer].

## <a name="next-steps"></a>Nästa steg

tooget igång med Mobile Apps i Azure App Service, fullständig hello [komma igång] kursen. hello kursen ingår hello grunderna för att producera tillbaka en mobil avslutas och klient av önskat slag. Kursen omfattar också integrering av autentisering, offlinesynkronisering och push-meddelanden. Du kan slutföra kursen hello flera gånger, en gång för varje klientprogram.

Mer information om Mobile Apps finns i vår [utbildningsväg].
Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service].

<!-- URLs. -->
[Migrate your mobile service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[komma igång]: app-service-mobile-ios-get-started.md
[Azure Table Storage]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/documentdb-get-started.md
[autentiseringsfunktioner]: ./app-service-mobile-auth.md
[datafunktioner]: ./app-service-mobile-offline-data-sync.md
[push-meddelandefunktioner]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS och Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatisk skalning]: ../app-service-web/web-sites-scale.md
[mellanlagringsmiljöer]: ../app-service-web/web-sites-staged-publishing.md
[distributionsalternativ]: ../app-service-web/web-sites-deploy.md
[hybridanslutningar]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuella nätverk]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[App Service-miljöer]: ../app-service-web/app-service-app-service-environment-intro.md
[utbildningsväg]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
