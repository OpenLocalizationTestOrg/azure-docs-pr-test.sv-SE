---
title: "Översikt över Azure API Management-relaterade viktiga begrepp | Microsoft Docs"
description: "Läs mer om API:er, produkter, roller, grupper och andra viktiga API Management-relaterade begrepp."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: e71da405-835a-48f3-956f-45c1a85698d7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 47358c6c209488d7a12e8afbf7a2d9b3f872f0de
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-api-management"></a>Vad är API Management?
API Management hjälper organisationer att publicera API:er till externa partner och interna utvecklare så att de kan frigöra potentialen i sina data och tjänster. Företag överallt vill utöka sin närvaro som digital plattform, skapa nya kanaler, hitta nya kunder och fördjupa relationerna med befintliga kunder. API Management lägger grunden till ett effektivt API-program genom engagerade utvecklare, affärsinsikter, analyser, hög säkerhet och skydd.

Följande videoklipp innehåller en översikt över Azure API Management och demonstrerar hur du använder API Management för att lägga till många funktioner i ditt API, inklusive åtkomstkontroll, frekvensbegränsning, övervakning, händelseloggning och cachelagring av svar med minimalt arbete från din sida.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-API-Management-Overview/player]
> 
> 

Administratörer skapar API:er för att använda API Management. Varje API består av en eller flera åtgärder och varje API kan läggas till i en eller flera produkter. För att kunna använda ett API prenumererar utvecklare på en produkt som innehåller API:et och kan sedan anropa API:ets åtgärder, baserat på eventuella användningsprinciper.

Det här avsnittet innehåller en översikt över viktiga API Management-relaterade begrepp.

> [!NOTE]
> Mer information finns i PDF-dokumentet [Cloud-based API Management: Harnessing the Power of APIs](http://j.mp/ms-apim-whitepaper). Det här introduktionsdokumentet om API Management av CITO Research innehåller bland annat följande avsnitt: 
> 
> * Vanliga API-relaterade krav och utmaningar
> * Frikoppla API:er och presentera fasader
> * Få snabbt igång utvecklarna
> * Skydda åtkomst
> * Analyser och mått
> * Få kontroll och insyn med en API Management-plattform
> * Använda molnet eller lokala lösningar
> * Azure API Management
> 
> 

## <a name="apis"> </a>API:er och åtgärder
API:er är grunden för en API Management-tjänstinstans. Varje API representerar en uppsättning åtgärder som är tillgängliga för utvecklare. Varje API innehåller en referens till backend-tjänsten som implementerar API:et, och dess åtgärder mappar till åtgärderna som implementeras av backend-tjänsten. Åtgärder i API Management är ytterst konfigurerbara, med kontroll över URL-mappning, fråge- och sökvägsparametrar, förfrågnings- och svarsinnehåll och cachelagring av åtgärdssvar. Principer för frekvensgränser, kvoter och IP-begränsning kan också implementeras på API- eller åtgärdsnivå.

Mer information finns i [Skapa API:er][How to create APIs] och [Lägga till åtgärder till ett API][How to add operations to an API].

## <a name="products"> </a> Produkter
Produkter syftar på hur API:erna exponeras för utvecklare. Produkter i API Management har ett eller flera API:er och konfigureras med en rubrik, en beskrivning och användningsvillkor. Produkter kan vara **öppna** eller **skyddade**. Utvecklare måste prenumerera på skyddade produkter innan de kan användas. Öppna produkter kan användas utan en prenumeration. När en produkt är redo att användas av utvecklare kan den publiceras. När produkten har publicerats kan utvecklare se den (och prenumerera på den om det är en skyddad produkt). Prenumerationsgodkännande konfigureras på produktnivå och kan antingen kräva administratörsgodkännande eller godkännas automatiskt.

Grupper används för att hantera hur produkterna visas för utvecklare. Produkter beviljar synlighet till grupper, och utvecklare kan visa och prenumerera på de produkter som är synliga för grupper som de är medlemmar i. 

Mer information finns i [Skapa och publicera en produkt][How to create and publish a product] och i följande videoklipp.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"> </a> Grupper
Grupper används för att hantera hur produkterna visas för utvecklare. API Management har följande systemgrupper som inte kan ändras.

* **Administratörer** – Administratörer av Azure-prenumerationer är medlemmar i den här gruppen. Administratörer hanterar API Management-tjänstinstanser genom att skapa API:er, åtgärder och produkter som används av utvecklare.
* **Utvecklare** – Autentiserade användare av utvecklarportalen hör till den här gruppen. Utvecklare är de kunder som utvecklar program med hjälp av dina API:er. Utvecklare beviljas åtkomst till utvecklarportalen och bygger program som anropar åtgärderna i ett API.
* **Gäster** – Oautentiserade användare av utvecklarportalen. Potentiella kunder som besöker utvecklarportalen för en API Management-instans hör t.ex. till den här gruppen. De kan beviljas viss skrivskyddad åtkomst, t.ex. möjligheten att visa API:er men inte anropa dem.

Utöver dessa systemgrupper kan administratörer skapa anpassade grupper eller [använda externa grupper i tillhörande Azure Active Directory-klienter](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Anpassade och externa grupper kan användas tillsammans med systemgrupper för att välja vilka utvecklare som kan se och komma åt API-produkter. Du kan till exempel skapa en anpassad grupp för utvecklare som hör till en specifik partnerorganisation och ge dem åtkomst till API:erna från en produkt som endast innehåller relevanta API:er. En användare kan tillhöra mer än en grupp.

Mer information finns i [Skapa och använda grupper][How to create and use groups].

## <a name="developers"> </a> Utvecklare
Utvecklare representerar användarkontona i en API Management-tjänstinstans. Utvecklare kan skapas eller bjudas in av administratörer eller registrera sig på [utvecklarportalen][Developer portal]. Varje utvecklare är medlem i en eller flera grupper och kan prenumerera på de produkter som visas för dessa grupper.

När utvecklare prenumererar på en produkt får de tillgång till den primära och sekundära nyckeln för produkten. Den här nyckeln används för att göra anrop till produktens API:er.

Mer information finns i [Skapa eller bjuda in utvecklare][How to create or invite developers] och [Associera grupper med utvecklare][How to associate groups with developers].

## <a name="policies"> </a> Principer
Principer är en kraftfull funktion i API Management som gör att utgivaren kan konfigurera om API:ets beteende. Principer är en samling instruktioner som körs sekventiellt på begäran av eller efter ett svar från ett API. Exempel på populära instruktioner är formatkonvertering från XML till JSON och begränsning av anropsfrekvensen för att begränsa antalet inkommande anrop från en utvecklare. Många andra principer är också tillgängliga.

Principuttryck kan användas som attributvärden eller textvärden i API Management-principer, under förutsättning att principen tillåter det. Vissa principer som [Kontrollflöde](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) och [Ange variabel](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) baseras på principuttryck. Mer information finns i [Avancerade principer](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [Principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx) samt i följande videoklipp.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

En fullständig lista över API Management-principer finns i [Principreferens][Policy reference]. Mer information om hur du använder och konfigurerar principer finns i [API Management-principer][API Management policies]. En självstudiekurs om hur du skapar en produkt med principer för frekvensbegränsning och kvoter finns i [Skapa och konfigurera avancerade produktinställningar][How create and configure advanced product settings]. Om du vill se en demonstration tittar du på följande videoklipp.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"> </a> Utvecklarportalen
På utvecklarportalen kan utvecklare lära sig mer om dina API:er, visa och anropa åtgärder och prenumerera på produkter. Potentiella kunder kan besöka utvecklarportalen, visa API:er och åtgärder och registrera sig. URL:en för din utvecklarportal finns på instrumentpanelen på den klassiska Azure-portalen för din API Management-tjänstinstans.

Du kan anpassa utvecklingsportalen genom att lägga till eget innehåll, anpassa format eller lägga till varumärkesanpassad design.

## <a name="api-management-and-the-api-economy"></a>Ekonomin bakom API Management och API:er
Om du vill veta mer om API Management tittar du på följande presentation från Microsoft Ignite 2015-konferensen.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How to create APIs]: api-management-howto-create-apis.md
[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How to create or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance




