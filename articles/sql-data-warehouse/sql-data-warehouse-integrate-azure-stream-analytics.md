---
title: "Använda Azure Stream Analytics med SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: c5c0450cba541a9346f023057345c5fc9b147903
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Använda Azure Stream Analytics med SQL Data Warehouse
Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet och skalbara komplexa händelsebearbetning över strömmande data i molnet. Du kan lära dig grunderna genom att läsa [introduktion till Azure Stream Analytics][Introduction to Azure Stream Analytics]. Sedan kan du lära dig hur du skapar en lösning för slutpunkt till slutpunkt med Stream Analytics genom att följa den [komma igång med Azure Stream Analytics] [ Get started using Azure Stream Analytics] kursen.

I den här artikeln får du lära dig hur du använder Azure SQL Data Warehouse-databas som utdatamottagare för ånga Analytics-jobb.

## <a name="prerequisites"></a>Krav
Kör först igenom följande steg i den [komma igång med Azure Stream Analytics] [ Get started using Azure Stream Analytics] kursen.  

1. Skapa en Event Hub-indata
2. Konfigurera och starta program eller generator-händelse
3. Etablera ett Stream Analytics-jobb
4. Ange indata för jobbet och fråga

Skapa sedan en Azure SQL Data Warehouse-databas

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Ange jobbutdata: Azure SQL Data Warehouse-databas
### <a name="step-1"></a>Steg 1
Klicka på i Stream Analytics-jobbet **utdata** högst upp på sidan och klicka sedan på **lägga till utdata**.

### <a name="step-2"></a>Steg 2
Välj SQL-databasen och klicka på Nästa.

![][add-output]

### <a name="step-3"></a>Steg 3
Ange följande värden på nästa sida:

* *Utdata Alias*: Ange ett eget namn för det här jobbutdata.
* *Prenumerationen*:
  * Om din SQL Data Warehouse-databas är i samma prenumeration som Stream Analytics-jobbet väljer du Använd SQL-databas från aktuell prenumeration.
  * Om databasen är i en annan prenumeration väljer du Använd SQL-databas från en annan prenumeration.
* *Databasen*: Ange namnet på en måldatabasen.
* *Servernamnet*: Ange namnet på servern för databasen som du precis angav. Du kan använda Azure-portalen för att hitta detta.

![][server-name]

* *Användarnamnet*: Ange användarnamnet för ett konto som har skrivbehörighet för databasen.
* *Lösenordet*: Ange lösenordet för det angivna användarkontot.
* *Tabellen*: Ange namnet på måltabellen i databasen.

![][add-database]

### <a name="step-4"></a>Steg 4
Klicka på knappen Kontrollera för att lägga till det här jobbutdata och kontrollera att Stream Analytics kan ansluta till databasen.

![][test-connection]

När anslutningen till databasen lyckas visas ett meddelande längst ned i portalen. Du kan klicka på Testa anslutning längst ned för att testa anslutningen till databasen.

## <a name="next-steps"></a>Nästa steg
En översikt över integrering finns [översikt över SQL Data Warehouse-integration][SQL Data Warehouse integration overview].

För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
