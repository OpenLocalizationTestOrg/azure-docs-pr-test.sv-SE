---
title: "aaaAzure Data Catalog vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar om Azure Data Catalog, inklusive funktioner för datakällsidentifiering, anteckning och hantering."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 03f9f4b801640b2e14232c62c8fc168e42e2c393
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Vanliga och frågor svar om Azure Data Catalog
Den här artikeln innehåller svar toofrequently och frågor relaterade toohello Azure Data Catalog-tjänsten.

## <a name="what-is-azure-data-catalog"></a>Vad är Azure Data Catalog?
Data Catalog är en helt hanterad tjänst som finns i Microsoft Azure som fungerar som ett system för registrering och identifiering för företagets datakällor. Med Data Catalog kan alla användare från analytiker toodata forskare och utvecklare, registrera, identifiera, förstå och använda datakällor.

## <a name="what-customer-challenges-does-it-solve"></a>Vilken kund utmaningar fungerar lösa?
Data Catalog adresser hello utmaningar för identifiering av datakälla och ”mörkt data” så att användare kan identifiera och förstå företagets datakällor.

## <a name="what-are-its-target-audiences"></a>Vad är dess målgrupperna?
Data Catalog är utformad för teknisk och icke-tekniska användare, inklusive:

* Data utvecklare och BI och analyser tekniker: personer som ansvarar för data och analytics innehåll för andra tooconsume.
* Data förvaltare: personer som har hello kunskap om hello data, vad det innebär och hur den är avsedd toobe används.
* Datakonsumenterna: personer som behöver toobe kan tooeasily identifiera, förstå och ansluta toohello data de behöver toodo sitt jobb med verktyget hello efter eget val.
* Central IT: personer som behöver toomake hundratals datakällor kan identifieras av användare i verksamheten och som behöver toomaintain tillsyn över hur data används och av vem.

## <a name="what-is-its-availability-by-region"></a>Vad är dess tillgänglighet per region?
Katalogtjänster data är tillgängliga i hello följande datacenter:

* Västra USA
* Östra USA
* Västra Europa
* Norra Europa
* Östra Australien
* Sydostasien

## <a name="what-are-its-limits-on-hello-number-of-data-assets"></a>Vad är gränsen för hello antalet datatillgångar?
hello är ledigt Edition av Data Catalog begränsad too5, 000 registrerade datatillgångar.

hello Standard Edition av Data Catalog har stöd för upp too100, 000 registrerade datatillgångar.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Vad är dess stöds käll- och tillgångsinformation datatyper?
En lista över datakällor som för närvarande stöds, se [Data Catalog DSR](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Hur jag för att begära stöd för en annan datakälla?
toosubmit funktion begäranden och andra feedback, gå toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-do-i-get-started-with-data-catalog"></a>Hur kommer jag igång med Data Catalog?
Hej bästa sätt tooget är igång genom att gå för[komma igång med Data Catalog](data-catalog-get-started.md). Den här artikeln är en översikt av hello funktioner i hello-tjänsten.

## <a name="how-do-i-register-my-data"></a>Hur registrerar jag mina data?
tooregister dina data i Data Catalog:
1. I hello Azure Data Catalog-portalen i hello **publicera** området, registreringsverktyget för start hello Azure Data Catalog. 
2. I hello Data Catalog publicera program, logga in med hello samma autentiseringsuppgifter som du använder tooaccess hello Data Catalog-portalen.
3. Välj hello datakälla och hello särskilda tillgångar som du vill tooregister.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Vilka egenskaper den extrahera för datatillgångar som har registrerats?
hello specifika egenskaper som skiljer sig från toodata datakälla men i allmänhet hello Data Catalog tjänsten extraherar hello följande information:

* Tillgångsnamnet
* Tillgångstypen
* Beskrivning av tillgångsinformation
* Attributet/kolumnnamn
* Attributet/kolumn datatyper
* Beskrivning av attributet/kolumn

> [!IMPORTANT]
> Registrera datatillgångar med Data Catalog inte flytta eller kopiera data toohello molnet. Registrera resurser från en datakälla kopior hello tillgångar metadata tooAzure, men hello data förblir hello befintlig datakälla plats. Hej toothis undantagsregel är om du väljer tooupload Förhandsgranska poster eller en profil för data när du registrerar hello tillgångar. När du inkluderar en förhandsgranskning av too20 poster kopieras från varje tillgång och lagras som en ögonblicksbild i Data Catalog. När du inkluderar en profil för data beräknas och ingår i hello metadata som lagras i katalogen hello samlar in information. Samlar in information kan omfatta hello storleken på tabeller, hello procentandelen null-värden per kolumn eller hello minimum, maximal och genomsnittlig värden för kolumner. 
>
>

> [!NOTE]
> För datakällor, till exempel SQL Server Analysis Services som har en förstklassig **beskrivning** hello Data Catalog och publicera programmet extrakt som egenskapsvärdet-egenskapen. För relationsdatabaser i SQL Server, som saknar en förstklassig **beskrivning** egenskapen hello Data Catalog publishing programmet extraherar hello värdet från hello **ms_description** utökad egenskap objekt och kolumner. Mer information finns i [med utökade egenskaper på databasobjekt](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-tooappear-in-hello-catalog"></a>Hur lång tid tar det för nyligen registrerade tillgångar tooappear i hello-katalogen?
När du har registrerat tillgångar med Data Catalog kan finnas det en period på 5 too10 sekunder innan de visas i hello Data Catalog-portalen.

## <a name="how-do-i-annotate-and-enrich-hello-metadata-for-my-registered-data-assets"></a>Hur gör kommentera och utöka hello metadata för min registrerade datatillgångar?
hello enklaste sättet tooprovide metadata för registrerade tillgångar är tooselect hello tillgång i hello Data Catalog-portalen och ange hello värden i egenskapsrutan hello eller schema-fönstret för hello valda objektet.

Du kan också ange vissa metadata, till exempel experter och taggar, under hello registreringsprocessen. hello-värden som du anger i hello Data Catalog tjänsten gäller tooall tillgångar som registreras som för närvarande. tooview hello nyligen registrerade objekt i hello portal för ytterligare anteckningen, Välj hello **visa Portal** knappen hello sista sidan i hello Data Catalog publicera programmet.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Hur tar jag bort min registrerade dataobjekt?
Du kan ta bort ett objekt från Data Catalog genom att markera hello objekt i hello portal och klicka på hello **ta bort** knappen. Ta bort hello objekt tar du bort dess metadata från Data Catalog men påverkar inte hello underliggande data.

## <a name="what-is-an-expert"></a>Vad är en expert?
En expert är en person som har ett välgrundat perspektiv om ett dataobjekt. Ett objekt kan ha flera experter. En expert behöver inte toobe hello ”ägare” för ett objekt, men är bara någon som vet hur hello data kan och bör användas.

## <a name="how-do-i-share-information-with-hello-data-catalog-team-if-i-encounter-problems"></a>Hur gör jag dela information med hello Data Catalog-teamet om det uppstår problem?
tooreport problem dela information och ställa frågor, gå toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-hello-catalog-work-with-another-data-source-that-im-interested-in"></a>Katalogen fungerar med en annan datakälla som jag hello?
Vi arbetar aktivt för att lägga till mer data sources tooData katalog. Om du vill toosee en särskild datakälla som stöds, föreslår det (eller röst din support om det redan har förslag) genom att gå toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-is-azure-data-catalog-related-toohello-data-catalog-in-power-bi-for-office-365"></a>Hur är Azure Data Catalog relaterade toohello Data Catalog i Power BI för Office 365?
Du kan se Azure Data Catalog som en utvecklingen av hello Data Catalog i Power BI. Från och med vår 2017 är Azure Data Catalog används tooenable hello delning och identifiering av frågorna i Excel 2016 och Power Query för Excel. Funktioner i data Catalog i Excel är tillgängliga toousers med Power BI Pro licenser.

## <a name="what-permissions-do-i-need-tooregister-assets-with-data-catalog"></a>Behörigheter gör jag behöver tooregister tillgångar med Data Catalog?
toorun hello Data Catalog registreringsverktyget du behöver behörigheter på hello-datakälla som du kan använda tooread hello metadata från hello källa. tooalso lägga till en förhandsgranskning måste du ha behörigheter som ger tooread i hello data från hello objekt registreras.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Data Catalog görs tillgänglig för lokala-distributionen?
Data Catalog är en molnbaserad tjänst som fungerar med både till molnet och lokala data sources toodeliver en hybridlösning datakälla identifiering. Det finns för närvarande inga planer för en version av hello Data Catalog-tjänst som körs lokalt.

## <a name="can-i-extract-more-or-richer-metadata-from-hello-data-sources-i-register"></a>Extraherar jag mer eller bättre metadata från hello datakällor jag registrera?
Vi arbetar aktivt tooexpand hello funktionerna i Data Catalog. Om du vill toohave ytterligare metadata som extraherats från hello datakällan under registreringen, föreslår det (eller rösta på det, om det redan har förslag) i hello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). I framtida hello tillåter vi tredje parter tooadd nya typer av datakällor genom en utökningsbarhet API.

## <a name="how-do-i-restrict-hello-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Hur begränsar jag hello synligheten för registrerade datatillgångar så att bara vissa personer kan identifiera dem?
Välj hello datatillgångar i hello Data Catalog och klicka sedan på hello **bli ägare** knappen. Ägarna av datatillgångar i Data Catalog kan ändra hello synlighet inställningar tooeither Tillåt alla användare toodiscover hello ägs tillgångar eller begränsa synligheten toospecific användare.

## <a name="how-do-i-update-hello-registration-for-a-data-asset-so-that-changes-in-hello-data-source-are-reflected-in-hello-catalog"></a>Hur uppdaterar jag hello registrering för en datatillgång så att ändringar i hello datakällan visas i hello katalogen?
tooupdate hello metadata för datatillgångar som redan har registrerats i katalogen hello, bara registrera hello-datakälla som innehåller hello tillgångar. Ändringar i hello datakällan som kolumner läggs till eller tas bort från tabeller eller vyer, uppdateras i hello katalogen, men alla kommentarer som användare bevaras.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Min fråga besvaras inte här. Var kan jag få svar?
Gå toohello [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Det frågor ska hitta här.
