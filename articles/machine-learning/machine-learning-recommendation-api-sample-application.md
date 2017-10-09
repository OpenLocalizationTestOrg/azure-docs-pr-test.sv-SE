---
title: "aaaCommon åtgärder i hello Machine Learning rekommendationer API | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: da16767134a1386617e1184e4a4850f1f346e972
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="recommendations-api-sample-application-walkthrough"></a>Rekommendationer API, exempelprogram, genomgång
> [!NOTE]
> Du bör börja använda hello kognitiva rekommendationer API-tjänsten i stället för den här versionen. hello kognitiva Rekommendationstjänsten ersätter den här tjänsten och alla nya funktioner för hello det kommer fram. Den har nya funktioner som batchbearbetning support, bättre API Explorer, en tydligare API underlag, mer konsekvent signup/fakturerings experience och så vidare.
> Lär dig mer om [migrera toohello ny kognitiva tjänst](http://aka.ms/recomigrate)
> 
> 

## <a name="purpose"></a>Syfte
Det här dokumentet visas hello användning av hello Azure Machine Learning rekommendationer API via en [exempelprogram](https://code.msdn.microsoft.com/Recommendations-144df403).

Det här programmet är inte avsedda tooinclude full funktionalitet, och den alla hello API: er. Den visar några vanliga åtgärder tooperform när du vill ha tooplay med hello Machine Learning rekommendation service. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="introduction-toomachine-learning-recommendation-service"></a>Introduktion tooMachine Learning rekommendation service
Rekommendationer via hello Machine Learning rekommendation tjänsten aktiveras när du skapar en rekommendation modell utifrån hello följande data:

* En databas av hello-objekt som du vill toorecommend, även kallat en katalog
* Data som representerar hello användning av objekt per användare eller session (Detta kan erhållas med tiden via datainsamling inte som en del av hello exempelapp)

När en rekommendation skapas, kan du använda den toopredict objekt som en användare kan ha intresse av, enligt tooa uppsättning objekt (eller ett enstaka objekt) hello användaren väljer.

tooenable Hej scenariot ovan, gör följande hello i hello Machine Learning rekommendation service:

* Skapa en modell: Detta är en logisk behållare som innehåller hello data (katalog och användning) och hello förutsägelse modeller. Varje modell behållare identifieras via ett unikt ID, som tilldelas när den skapas. Detta ID kallas hello modell-ID och den används av de flesta hello API: er. 
* Överför toocatalog: när en behållare för modellen har skapats kan du associera tooit en katalog.

**Obs**: skapa en modell och ladda upp tooa katalog utförs vanligtvis en gång för hello modellen livscykel.

* Överför användningsdata: Detta lägger till användning data toohello modellen behållare.
* Skapa en rekommendation modell: när du har tillräckligt med data, kan du skapa hello rekommendation modell. Den här åtgärden använder hello översta Machine Learning algoritmer toocreate en rekommendation modell. Varje version är associerad med ett unikt ID. Du måste tookeep en post med detta ID eftersom det är nödvändigt för hello funktionerna för vissa API: er.
* Övervakaren hello processen för att bygga: en rekommendation modellen version är en asynkron åtgärd, och det kan ta flera minuter tooseveral timmar, beroende på hello mängd data (katalog och användning) och hello build-parametrar. Du måste därför toomonitor hello build. En rekommendation modell skapas endast om dess associerade build har slutförts.
* (Valfritt) Välj en aktiv rekommendation modellen build: det här steget krävs bara om du har mer än en rekommendation modell inbyggd i din modell-behållaren. Begäran tooget rekommendationer utan som visar hello active rekommendation modellen omdirigeras automatiskt av hello system toohello standard active build. 

**Obs**: en aktiv rekommendation modell är produktion som är redo och den är inbyggd för produktion arbetsbelastning. Detta skiljer sig från en icke-aktiv rekommendation modell, som är i en test-liknande miljö (kallas ibland för mellanlagring).

* Få rekommendationer: när du har en rekommendation modell kan du utlösa rekommendationer för ett enskilt objekt eller en lista med objekt som du väljer. 

Du kommer normalt anropa hämta rekommendation för en viss tidsperiod. Under denna tid kan omdirigera du användning data toohello Machine Learning rekommendation system som lägger till den här behållaren har data toohello angivna modellen. Du kan skapa en ny modell för rekommendation som inkluderar hello ytterligare användningsdata när du har tillräckligt med användningsdata. 

## <a name="prerequisites"></a>Krav
* Visual Studio 2013 eller senare
* Internet-åtkomst 
* Prenumerationen toohello rekommendationer API (https://datamarket.azure.com/dataset/amla/recommendations).

## <a name="azure-machine-learning-sample-app-solution"></a>Azure Machine Learning app exempellösning
Den här lösningen innehåller hello källkoden, exempel, katalogfil och direktiven toodownload hello paket som krävs för kompilering.

## <a name="hello-apis-used"></a>hello API: er som används
hello programmet använder Maskininlärning rekommendation funktioner genom en delmängd av tillgängliga API: er. Följande API: er visas i hello programmet hello:

* Skapa modell: skapa en logisk behållare toohold data och rekommendation modeller. En modell identifieras med ett namn som du kan inte skapa mer än en modell med hello samma namn.
* Överför katalogfil: Använd tooupload katalogdata.
* Överföra användningsinformation fil: använda tooupload användningsdata.
* Starta build: Använd toocreate en rekommendation modell.
* Övervaka körning av build: Använd toomonitor hello status för en rekommendation modellen version.
* Välj en inbyggd modell för rekommendationen: Använd tooindicate vilka rekommendation modellen toouse som standard för en viss modell-behållare. Det här steget är nödvändigt om du har mer än en rekommendation modell och du vill tooactivate icke-aktiv bygga som hello active rekommendation modell.
* Hämta rekommendation: Använd tooretrieve rekommenderade objekt enligt tooa enstaka objekt eller en uppsättning objekt. 

En fullständig beskrivning av hello API: er finns hello Microsoft Azure Marketplace-dokumentationen. 

**Obs**: en modell kan ha flera versioner över tid (inte samtidigt). Varje version skapas med hello samma eller en uppdaterad katalog och ytterligare användningsdata.

## <a name="common-pitfalls"></a>Vanliga fallgropar
* Du behöver tooprovide ditt användarnamn och din Microsoft Azure Marketplace primära konto viktiga toorun hello sample-appen.
* App som körs hello exempel misslyckas i följd. hello programflöde omfattar att skapa, överföringen, skapa hello övervakaren och få rekommendationer från en fördefinierad modellen. Därför kan den inte på efterföljande körning om du inte ändrar hello modellnamn mellan anrop.
* Rekommendationer kan returnera utan data. Hej exempelapp använder en liten fil katalog och användning. Innebär att vissa objekt från hello katalogen har inga rekommenderade objekt.

## <a name="disclaimer"></a>Ansvarsfriskrivning
hello sample-appen är inte avsedda toobe som körs i en produktionsmiljö. hello data i hello katalogen är mycket små och det ger inte en modell med meningsfull rekommendation. hello data har angetts som en demonstration. 

