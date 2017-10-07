---
title: aaaUse Azure Machine Learning med SQL Data Warehouse | Microsoft Docs
description: "Självstudier för att använda Azure Machine Learning med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: fdfe8c936d2bb7a02163a0bbf6435e1ebd518d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Använda Azure Machine Learning med SQL Data Warehouse
Azure Machine Learning är en helt hanterad förutsägelseanalyser tjänst att du kan använda toocreate förutsägelsemodeller mot dina data i SQL Data Warehouse och sedan publicera som redo att använda webbtjänster. Du kan lära sig hello grunderna i förutsägelseanalyser och maskininlärning genom att läsa [introduktion tooMachine Learning på Azure][Introduction tooMachine Learning on Azure].  Du kan sedan lära dig hur toocreate, träna, poängsätta och testa en maskininlärningsmodell med hello [skapa experiment kursen][Create experiment tutorial].

I den här artikeln får du lära dig hur toodo hello följa med hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:

* Läsa data från din databas toocreate, träna och betygsätta en förutsägelsemodell
* Skriva data tooyour databas

## <a name="read-data-from-sql-data-warehouse"></a>Läsa data från SQL Data Warehouse
Vi läser data från produkt-tabellen i hello AdventureWorksDW-databasen.

### <a name="step-1"></a>Steg 1
Starta ett nytt experiment genom att klicka på + ny längst hello hello Machine Learning Studio-fönstret Välj EXPERIMENT och välj sedan tomt Experiment. Välj hello standard experimentera namn hello överst i hello arbetsytan och Byt toosomething beskrivande, till exempel cykel pris förutsägelse.

### <a name="step-2"></a>Steg 2
Leta efter hello Reader modul i hello palett med datauppsättningar och moduler hello till vänster i hello experimentet. Dra hello modulen toohello för experimentet.
![][drag_reader]

### <a name="step-3"></a>Steg 3
Välj hello Reader modul och fylla i hello egenskapsrutan.

1. Välj Azure SQL Database som hello-datakälla.
2. Databasservernamn: typen hello servernamn. Du kan använda hello [Azure-portalen] [ Azure portal] toofind detta.

![][server_name]

1. Databasnamn: hello-typnamn för en databas på hello-server som du just har angett.
2. Servern användarkontonamnet: Skriv hello användarnamn för ett konto som har behörighet för hello-databasen.
3. Serverlösenord: Ange hello lösenordet för hello angivna användarkontot.
4. Acceptera alla servercertifikat: Använd det här alternativet (mindre säkert) om du vill tooskip granska hello platscertifikat innan du läser dina data.
5. Databasfrågan: Ange en SQL-instruktion som beskriver hello data som du vill tooread. I det här fallet kommer vi läsa data från produkt-tabellen med hjälp av hello följande fråga.

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Steg 4
1. Kör hello experiment genom att klicka på kör under hello experimentet.
2. När hello experimentet har slutförts har modul för dataläsare för hello en grön bock tooindicate som har slutförts. Observera också hello avslutad Körningsstatus i hello övre högra hörnet.

![][run]

1. toosee hello importerade data på hello utdataporten längst ned hello hello bildata och väljer visualisera.

## <a name="create-train-and-score-a-model"></a>Skapa, träna och betygsätta en modell
Nu kan du använda denna dataset för att:

* Skapa en modell: bearbeta data och definiera funktioner
* Hej träningsmodell: välja och tillämpa en inlärningsalgoritm
* Poängsätta och testa hello modell: förutsäga nya cykel pris

![][model]

Mer om hur toocreate, träna, poängsätta och testa machine learning-modellen Använd hello toolearn [skapa experiment kursen][Create experiment tutorial].

## <a name="write-data-tooazure-sql-data-warehouse"></a>Skriva data tooAzure SQL Data Warehouse
Vi skriver hello resultattabellen set tooProductPriceForecast i hello AdventureWorksDW-databasen.

### <a name="step-1"></a>Steg 1
Leta efter hello skrivarmodul i hello palett med datauppsättningar och moduler hello till vänster i hello experimentet. Dra hello modulen toohello för experimentet.

![][drag_writer]

### <a name="step-2"></a>Steg 2
Välj hello-skrivarmodul och fylla i hello egenskapsrutan.

1. Välj Azure SQL Database som hello datamål.
2. Databasservernamn: typen hello servernamn. Du kan använda hello [Azure-portalen] [ Azure portal] toofind detta.
3. Databasnamn: hello-typnamn för en databas på hello-server som du just har angett.
4. Servern användarkontonamnet: typen hello användarnamnet för ett konto som har skrivbehörighet för hello-databasen.
5. Serverlösenord: Ange hello lösenordet för hello angivna användarkontot.
6. Acceptera alla servercertifikat (osäker): Välj det här alternativet om du inte vill tooview hello certifikat.
7. Kommaavgränsad lista med kolumner toobe sparade: Ange en lista över hello datamängd eller resultatet kolumner som du vill toooutput.
8. Data tabellnamn: Ange hello datatabell hello namn.
9. Kommaavgränsad lista med kolumner som datatable: Ange hello kolumnen namn toouse i hello ny tabell. hello kolumnnamn kan skilja sig från hello dem i hello källa dataset, men du måste ange hello samma antal kolumner som du definierar för hello utdatatabellen.
10. Antalet rader som skrivs per SQL Azure-åtgärd: du kan konfigurera hello antalet rader som skrivs tooa SQL-databas i en enda åtgärd.

![][writer_properties]

### <a name="step-3"></a>Steg 3
1. Kör hello experiment genom att klicka på kör under hello experimentet.
2. När hello experimentet har slutförts visas har alla moduler som en grön bock tooindicate som de har slutförts.

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction toomachine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
