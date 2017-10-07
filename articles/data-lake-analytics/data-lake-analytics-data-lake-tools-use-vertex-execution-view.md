---
title: "aaaUse hello Vertex vy i Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Lär dig hur toouse hello Vertex vy tooexam Data Lake Analytics-jobb."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: fb54d2af8a32aa27a54ff50a73c1b4903677a21e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Använd hello Vertex vy i Data Lake-verktyg för Visual Studio
Lär dig hur toouse hello Vertex vy tooexam Data Lake Analytics-jobb.

## <a name="prerequisites"></a>Krav

Du måste grundläggande kunskaper om med Data Lake-verktyg för Visual Studio toodevelop U-SQL-skript.  Se [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-hello-vertex-execution-view"></a>Öppna hello Vertex vy
Öppna ett U-SQL-jobb i Data Lake-verktyg för Visual Studio. Klicka på **Vertex vy** i hello nedre vänstra hörnet. Du kanske ange tooload profiler först och det kan ta lite tid beroende på nätverksanslutningen.

![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Förstå Vertex vy
hello Vertex vy består av tre delar:

![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

Hej **Vertex selector** på hello vänstra kan du välja formhörnen av funktioner (som topp 10 data läsa eller välja efter steget). En av hello mest använda filter är toosee hello **formhörnen på kritiska**. Hej **kritiska** är hello längsta kedja av formhörnen ett U-SQL-jobb. Förstå hello kritiska är användbart för att optimera dina jobb genom att kontrollera vilka vertex tar hello längsta tid.
  
![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

hello överst i mitten visar hello **Körningsstatus för alla hello formhörnen**.
  
![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

hello längst ned i fönstret center visar information om varje vertex:
* Processnamnet: hello namn på hello vertex instans. Den består av olika delar i StageName | VertexName | VertexRunInstance. Till exempel hello SV7_Split [62] .v1 vertex står för hello andra instans som körs (.v1, index som börjar från 0) för Vertex tal 62 i steget SV7_Split.
* Totala Data Läs/skriftliga: hello data var Läs/skrivs av den här hörn.
* Status för status-/ utgång: hello slutlig status när hello vertex avslutas.
* Avsluta felkod-fel typ: hello fel vid hello vertex misslyckades.
* Skapa orsak: Varför hello vertex skapades.
* Svarstid för resursen latens/bearbeta latens/PN kön: hello tid det tar för hello vertex toowait för resurser, tooprocess data och toostay i hello kö.
* Processen/Creator GUID: GUID för hello aktuella körs vertex eller dess creator.
* Version: hello nte instans av hello kör vertex (hello system kan schemalägga nya instanser för ett hörn för många orsaker, till exempel redundans compute redundans, osv.)
* Version skapas en gång.
* Bearbeta skapa Start tid/Process i kö tid/Process Start tid/Process slutförd tid: när hello vertex processen börjar skapa; När hello vertex processen startar tooqueue; När hello startar vissa vertex; När hello vissa vertex har slutförts.

## <a name="next-steps"></a>Nästa steg
* toolog diagnostikinformation, se [åtkomst till diagnostik loggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* tooview jobbinformation finns [Använd jobbet webbläsare och visa jobb för Azure Data lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md)
