---
title: "Vanliga åtgärder i Machine Learning rekommendationer API | Microsoft Docs"
description: Azure ML rekommendation exempelprogrammet
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 0220bc17-3315-47d7-84a3-ef490263a343
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8d8efa93e820f4a745ed93c0f4d13b2438dfa1eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="recommendations-api-sample-application-walkthrough"></a>Rekommendationer API, exempelprogram, genomgång
> [!NOTE]
> Du bör börja använda kognitiva Rekommendationstjänsten API i stället för den här versionen. Kognitiva Rekommendationstjänsten ersätter den här tjänsten och de nya funktionerna det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera till den nya kognitiva tjänsten](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Syfte
Det här dokumentet beskrivs användningen av Azure Machine Learning rekommendationer API via en [exempelprogram](https://code.msdn.microsoft.com/Recommendations-144df403).

Det här programmet är inte avsedd att inkludera alla funktioner, och den alla API: er. Den visar några vanliga åtgärder som ska utföras när du först vill spela upp med tjänsten Machine Learning rekommendation. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-to-machine-learning-recommendation-service"></a>Introduktion till Machine Learning rekommendation service
Rekommendationer via tjänsten Machine Learning rekommendation aktiveras när du skapar en rekommendation modell baserat på data i följande:

* En lagringsplats för de objekt du vill att rekommendera, även kallat en katalog
* Data som representerar användningen av objekt per användare eller session (Detta kan erhållas med tiden via datainsamling inte som en del av exempelappen)

När en rekommendation modell är skapat kan du använda den för att förutsäga objekt som en användare kan vara intresserad av, enligt en uppsättning objekt (eller ett enstaka objekt) användaren väljer.

Gör följande i tjänsten Machine Learning rekommendation om du vill aktivera det föregående scenariot:

* Skapa en modell: Detta är en logisk behållare som innehåller data (katalog och användning) och modeller för förutsägelse. Varje modell behållare identifieras via ett unikt ID, som tilldelas när den skapas. Detta ID kallas modell-ID och den används av de flesta av de API: er. 
* Överför till katalogen: när en modell-behållare skapas, du kan koppla till den en katalog.

**Obs**: skapa en modell och överföra till en katalog utförs vanligtvis en gång för modellen livscykel.

* Överför användningsdata: användningsdata läggs till i behållaren för modellen.
* Skapa en rekommendation modell: när du har tillräckligt med data kan du skapa rekommendation modellen. Den här åtgärden använder översta maskininlärningsalgoritmer för att skapa en rekommendation modell. Varje version är associerad med ett unikt ID. Du behöver en post med detta ID eftersom det är nödvändigt för funktionerna i vissa API: er.
* Övervaka processen byggnad: en rekommendation modellen version är en asynkron åtgärd och det kan ta flera minuter till flera timmar, beroende på mängden data (katalog och användning) och build-parametrar. Därför behöver du övervaka genereringen. En rekommendation modell skapas endast om dess associerade build har slutförts.
* (Valfritt) Välj en aktiv rekommendation modellen build: det här steget krävs bara om du har mer än en rekommendation modell inbyggd i din modell-behållaren. Varje begäran att få rekommendationer utan active rekommendation modellen omdirigeras automatiskt av systemet för att den aktiva versionen som standard. 

**Obs**: en aktiv rekommendation modell är produktion som är redo och den är inbyggd för produktion arbetsbelastning. Detta skiljer sig från en icke-aktiv rekommendation modell, som är i en test-liknande miljö (kallas ibland för mellanlagring).

* Få rekommendationer: när du har en rekommendation modell kan du utlösa rekommendationer för ett enskilt objekt eller en lista med objekt som du väljer. 

Du kommer normalt anropa hämta rekommendation för en viss tidsperiod. Under denna tid kan omdirigera du användningsdata till Machine Learning rekommendation system som lägger till dessa data i den angivna modell behållaren. Du kan skapa en ny rekommendation modell som innehåller ytterligare användningsdata när du har tillräckligt med användningsdata. 

## <a name="prerequisites"></a>Krav
* Visual Studio 2013 eller senare
* Internet-åtkomst 
* Prenumeration på rekommendationer API (https://datamarket.azure.com/dataset/amla/recommendations).

## <a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning app exempellösning
Den här lösningen innehåller källkoden, exempel, katalogfil och direktiven att ladda ned de paket som krävs för kompilering.

## <a name="the-apis-used"></a>API: er som används
Programmet använder Maskininlärning rekommendation funktioner genom en delmängd av tillgängliga API: er. Följande API: er visas i programmet:

* Skapa modell: skapa en logisk behållare för att lagra data och rekommendation modeller. En modell identifieras med ett namn och du kan inte skapa mer än en modell med samma namn.
* Överför katalogfil: använda för att överföra data catalog.
* Överföra användningsinformation fil: använda för att överföra användningsdata.
* Starta build: använda för att skapa en rekommendation modell.
* Övervaka körning av build: använda för att övervaka status för en rekommendation modellen version.
* Välj en inbyggd modell för rekommendationen: används för att indikera vilka rekommendation modell som ska användas som standard för en viss modell-behållare. Det här steget är nödvändigt endast om du har mer än en rekommendation modell och du vill aktivera en icke-aktiv-version som aktiv rekommendation modell.
* Hämta rekommendation: använda för att hämta rekommenderade objekt enligt ett angivet eller en uppsättning objekt. 

En fullständig beskrivning av de API: er finns i Microsoft Azure Marketplace-dokumentationen. 

**Obs**: en modell kan ha flera versioner över tid (inte samtidigt). Varje version skapas med samma eller en uppdaterad katalog och ytterligare användningsdata.

## <a name="common-pitfalls"></a>Vanliga fallgropar
* Du måste ange ditt användarnamn och din Microsoft Azure Marketplace primära konto för att köra sample-appen.
* Kör exempelappen i följd misslyckas. Programmet flödet omfattar att skapa, överföringen, skapa övervakaren och få rekommendationer från en fördefinierad modell; Därför kan den inte på efterföljande körning om du inte ändrar modellnamnet mellan anrop.
* Rekommendationer kan returnera utan data. Exempelappen använder en liten fil katalog och användning. Innebär att vissa objekt från katalogen har inga rekommenderade objekt.

## <a name="disclaimer"></a>Ansvarsfriskrivning
Exempelappen är inte avsedd att köras i en produktionsmiljö. Informationen i katalogen är mycket små och det ger inte en modell med meningsfull rekommendation. Data har angetts som en demonstration. 

