---
title: aaaUse Azure Stream Analytics med SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda Azure Stream Analytics med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1278197a6764864124fd92fc672de00b83ec343f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Använda Azure Stream Analytics med SQL Data Warehouse
Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet och skalbara komplexa händelsebearbetning över strömmande data i hello molnet. Du kan lära dig grunderna i hello genom att läsa [introduktion tooAzure Stream Analytics][Introduction tooAzure Stream Analytics]. Sedan kan du lära dig hur toocreate en slutpunkt till slutpunkt-lösning med Stream Analytics genom att följa hello [komma igång med Azure Stream Analytics] [ Get started using Azure Stream Analytics] kursen.

I den här artikeln får du lära dig hur toouse din Azure SQL Data Warehouse-databas som utdatamottagare för ånga Analytics-jobb.

## <a name="prerequisites"></a>Krav
Kör först via hello följa stegen i hello [komma igång med Azure Stream Analytics] [ Get started using Azure Stream Analytics] kursen.  

1. Skapa en Event Hub-indata
2. Konfigurera och starta program eller generator-händelse
3. Etablera ett Stream Analytics-jobb
4. Ange indata för jobbet och fråga

Skapa sedan en Azure SQL Data Warehouse-databas

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Ange jobbutdata: Azure SQL Data Warehouse-databas
### <a name="step-1"></a>Steg 1
Klicka på i Stream Analytics-jobbet **utdata** från hello överkant hello sidan och klicka sedan på **lägga till utdata**.

### <a name="step-2"></a>Steg 2
Välj SQL-databasen och klicka på Nästa.

![][add-output]

### <a name="step-3"></a>Steg 3
Ange följande värden på nästa sida i hello hello:

* *Utdata Alias*: Ange ett eget namn för det här jobbutdata.
* *Prenumerationen*:
  * Om din SQL Data Warehouse-databas i hello samma prenumeration som hello Stream Analytics-jobbet väljer använda SQL-databas från aktuell prenumeration.
  * Om databasen är i en annan prenumeration väljer du Använd SQL-databas från en annan prenumeration.
* *Databasen*: Ange en måldatabas hello namn.
* *Servernamnet*: Ange hello servernamnet hello-databasen som du precis angav. Du kan använda hello Azure klassiska Portal toofind den.

![][server-name]

* *Användarnamnet*: Ange hello användarnamnet för ett konto som har skrivbehörighet för hello-databasen.
* *Lösenordet*: Ange hello lösenordet för hello angivna användarkontot.
* *Tabellen*: Ange hello namnet på hello måltabellen i hello-databasen.

![][add-database]

### <a name="step-4"></a>Steg 4
Klicka på hello Kontrollera knappen tooadd denna jobbutdata och tooverify att Stream Analytics kan ansluta toohello databas.

![][test-connection]

När hello toohello databas lyckas visas ett meddelande längst ned hello hello-portalen. Du kan klicka på Testa anslutning på hello nedre tootest hello toohello databas.

## <a name="next-steps"></a>Nästa steg
En översikt över integrering finns [översikt över SQL Data Warehouse-integration][SQL Data Warehouse integration overview].

För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction tooAzure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
