---
title: "aaaAzure API Management översikt och nyckeln begrepp | Microsoft Docs"
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
ms.openlocfilehash: a77b24b4632d868afa15bd6cf88060982046cb38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-api-management"></a>Vad är API Management?
API Management hjälper organisationer att publicera API: er tooexternal, partner och egna utvecklare toounlock hello potential sina data och tjänster. Företag överallt söker tooextend verksamheten som en digital plattform, skapa nya kanaler, söka efter nya kunder och köra djupare interaktion med befintliga. API Management ger hello core befogenheter tooensure en lyckad API-program via developer engagement, affärsinsikter, analys, säkerhet och skydd.

Titta på hello följande video för en översikt över Azure API Management och lär dig hur toouse API Management tooadd många funktioner tooyour API, inklusive åtkomstkontroll-betygsätta begränsar, övervakning, loggning och cachelagrar svaret, med minimalt arbete från din sida.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-API-Management-Overview/player]
> 
> 

toouse API Management administratörer skapa API: er. Varje API består av en eller flera åtgärder och varje API kan läggas till tooone eller flera produkter. toouse API utvecklare prenumerera tooa produkt som innehåller den API och de kan sedan anropa hello API: er åtgärden, ämne tooany användningsprinciper som påverkas.

Det här avsnittet innehåller en översikt över viktiga API Management-relaterade begrepp.

> [!NOTE]
> Mer information finns i hello [molnbaserade API-hantering: utnyttjar hello Power av API: er](http://j.mp/ms-apim-whitepaper) PDF-rapporten. Det här introduktionsdokumentet om API Management av CITO Research innehåller bland annat följande avsnitt: 
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
API: er är hello grunden för en instans för API Management-tjänsten. Varje API representerar en uppsättning åtgärder tillgängliga toodevelopers. Varje API innehåller en referens toohello backend-tjänst som implementerar hello API och åtgärderna mappa toohello åtgärder som implementeras av hello backend-tjänst. Åtgärder i API Management är ytterst konfigurerbara, med kontroll över URL-mappning, fråge- och sökvägsparametrar, förfrågnings- och svarsinnehåll och cachelagring av åtgärdssvar. Hastighetsbegränsning, kvoter och principer för IP-begränsning kan också implementeras på enskild nivå eller hello API.

Mer information finns i [hur toocreate API: er] [ How toocreate APIs] och [hur tooadd operations tooan API][How tooadd operations tooan API].

## <a name="products"> </a> Produkter
Produkter är hur API: er på ytan toodevelopers. Produkter i API Management har ett eller flera API:er och konfigureras med en rubrik, en beskrivning och användningsvillkor. Produkter kan vara **öppna** eller **skyddade**. Skyddade produkter måste vara prenumererade toobefore som de kan användas, öppen produkter kan användas utan en prenumeration. När en produkt är redo att användas av utvecklare kan den publiceras. När den har publicerats kan visas (och i hello gäller skyddade produkter prenumerera) av utvecklare. Prenumerationen godkännande kan konfigureras på hello produktnivå och antingen administratörsgodkännande krävs eller ska godkännas automatiskt.

Grupper är används toomanage hello synligheten för produkter toodevelopers. Produkter ge synlighet toogroups och utvecklare kan visa och prenumerera toohello produkter som är synliga toohello grupper som de tillhör. 

Mer information finns i [hur toocreate och publicera en produkt] [ How toocreate and publish a product] och hello följande video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"> </a> Grupper
Grupper är används toomanage hello synligheten för produkter toodevelopers. API Management har hello följande ändras system grupper.

* **Administratörer** – Administratörer av Azure-prenumerationer är medlemmar i den här gruppen. Administratörer hantera API Management-tjänstinstanser skapar hello-API: er, åtgärder och produkter som används av utvecklare.
* **Utvecklare** – Autentiserade användare av utvecklarportalen hör till den här gruppen. Utvecklare är hello-kunder som skapar program med hjälp av dina API: er. Utvecklare har beviljats åtkomst toohello developer-portalen och bygga program som anropar hello driften av ett API.
* **Gäster** -oautentiserad developer portal användare, till exempel potentiella kunder som besöker hello developer-portalen för en API-hantering instans återgång till den här gruppen. De kan beviljas vissa skrivskyddad åtkomst till exempel hello möjlighet tooview API: er men anropa inte dem.

Administratörer kan skapa anpassade grupper i tillägg toothese system grupper eller [utnyttja externa grupper i associerade Azure Active Directory-klienter](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Anpassad och externa grupper kan användas tillsammans med system-grupper i vilket ger utvecklare synlighet och komma åt tooAPI produkter. Du kan till exempel skapa en anpassad grupp för utvecklare som är kopplad till en specifik kontopartner-organisation och ge dem åtkomst toohello API: er från en produkt som innehåller relevant API. En användare kan tillhöra mer än en grupp.

Mer information finns i [hur toocreate och Använd][How toocreate and use groups].

## <a name="developers"> </a> Utvecklare
Utvecklare representerar hello användarkonton i en instans för API Management-tjänsten. Utvecklare kan skapa eller inbjuden toojoin av administratörer eller de kan logga från hello [utvecklarportalen][Developer portal]. Varje utvecklare är medlem i en eller flera grupper och kan vara prenumerera toohello produkter som ger insyn toothose grupper.

De har beviljats hello primära och sekundära nyckeln för hello produkten när utvecklare prenumererar tooa produkten. Den här nyckeln används när du anropar i hello produkten API: er.

Mer information finns i [hur toocreate eller be utvecklare] [ How toocreate or invite developers] och [hur tooassociate grupper med utvecklare][How tooassociate groups with developers].

## <a name="policies"> </a> Principer
Principer är en kraftfull funktion för API-hantering som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration. Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API. Populära rapporterna innehålla Formatkonvertering från XML-tooJSON och anropet frekvensen begränsa toorestrict hello mängden inkommande samtal från en utvecklare och många andra principer är tillgängliga.

Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars. Vissa principer, till exempel hello [Åtkomstkontrollflödet](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) och [ange variabel](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) principer är baserade på principuttrycken. Mer information finns i [avancerade principer](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [principuttrycken](https://msdn.microsoft.com/library/azure/dn910913.aspx), och titta på hello följande video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

En fullständig lista över API Management-principer finns i [Principreferens][Policy reference]. Mer information om hur du använder och konfigurerar principer finns i [API Management-principer][API Management policies]. En självstudiekurs om hur du skapar en produkt med principer för frekvensbegränsning och kvoter finns i [Skapa och konfigurera avancerade produktinställningar][How create and configure advanced product settings]. En demonstration finns i följande video hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"> </a> Utvecklarportalen
hello developer-portalen är där utvecklare kan Lär dig mer om dina API: er, visa och anropa åtgärder och prenumerera tooproducts. Potentiella kunder kan besöka hello developer-portalen, visa API: er och åtgärder och registrera dig. hello-URL för developer-portalen finns på hello instrumentpanelen i hello klassiska Azure-portalen för din API Management service-instans.

Du kan anpassa hello utseende developer-portalen genom att lägga till anpassade innehåll, anpassa format och lägga till din anpassning.

## <a name="api-management-and-hello-api-economy"></a>API Management och hello API ekonomi
Mer information om API-hantering, titta på hello följande presentation från hello Microsoft Ignite 2015 konferens toolearn.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How toocreate APIs]: api-management-howto-create-apis.md
[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How toocreate or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance




