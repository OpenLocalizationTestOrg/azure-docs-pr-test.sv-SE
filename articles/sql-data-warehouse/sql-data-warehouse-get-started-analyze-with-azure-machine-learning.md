---
title: aaaAnalyze data med Azure Machine Learning | Microsoft Docs
description: "Använd Azure Machine Learning toobuild förutsägande machine learning-modell baserad på data som lagras i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 337a2cd77aaad4467683827c56e5015b262b2554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a>Analysera data med Azure Machine Learning
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Den här kursen används Azure Machine Learning toobuild förutsägande machine learning-modell baserad på data som lagras i Azure SQL Data Warehouse. Mer specifikt kan detta skapar en riktad marknadsföringskampanj för Adventure Works, hello cykelbutik, genom att förutsäga om en kund är sannolikt toobuy en cykel eller inte.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Krav
toostep igenom den här kursen behöver du:

* Ett SQL Data Warehouse förinstallerat med AdventureWorksDW som exempeldatabas. tooprovision detta, se [skapa ett SQL Data Warehouse] [ Create a SQL Data Warehouse] och välj tooload hello exempeldata. Om du redan har ett Data Warehouse men inte har exempeldata kan du [läsa in exempeldata manuellt][load sample data manually].

## <a name="1-get-hello-data"></a>1. Hämta hello data
hello data har hello dbo.vTargetMail-vyn i hello AdventureWorksDW-databasen. tooread informationen:

1. Logga in i [Azure Machine Learning Studio][Azure Machine Learning studio] och klicka på My experiments (Mina experiment).
2. Klicka på **+NY** och välj **Tomt experiment**.
3. Ange ett namn för experimentet: Riktad marknadsföring.
4. Dra hello **Reader** modul från hello modulfönstret till arbetsytan hello.
5. Ange hello information för din SQL Data Warehouse-databas i hello egenskaper.
6. Ange hello databas **frågan** tooread hello data av intresse.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Kör hello experiment genom att klicka på **kör** under hello experimentet.
![Kör hello experiment][1]

När hello experimentet har körts, klicka på hello utdataporten längst ned hello hello Reader modulen och välj **visualisera** toosee hello importerat data.
![Visa data som importerats][3]

## <a name="2-clean-hello-data"></a>2. Rensa hello data
tooclean hello data, släppa vissa kolumner som inte är relevanta för hello modellen. toodo detta:

1. Dra hello **Projektkolumner** modulen hello arbetsytan.
2. Klicka på **starta kolumnväljaren** i hello egenskaper fönstret toospecify vilka kolumner du vill toodrop.
   ![Projektkolumner][4]
3. Exkludera två kolumner: CustomerAlternateKey och GeographyKey.
   ![Ta bort onödiga kolumner][5]

## <a name="3-build-hello-model"></a>3. Skapa hello modell
Vi delar upp hello data 80-20: 80% tootrain en machine learning-modell och 20% tootest hello modellen. Vi använder hello ”Two-Class” algoritmer för detta binära klassificeringsproblem.

1. Dra hello **dela** modulen hello arbetsytan.
2. Ange 0,8 för andel av rader i hello första utdatamängden i hello egenskapsrutan.
   ![Dela data till uppsättningar för träning och testning][6]
3. Dra hello **två-Tvåklassförhöjt beslutsträd** modulen hello arbetsytan.
4. Dra hello **Träningsmodell** modulen till hello arbetsytan och ange hello indata. Klicka på **starta kolumnväljaren** hello egenskaper i fönstret.
   * Första indata: ML-algoritmen.
   * Andra indata: Data tootrain hello algoritmen på.
     ![Ansluta hello träningsmodellmodulen][7]
5. Välj hello **BikeBuyer** enligt hello kolumnen toopredict.
   ![Välj kolumnen toopredict][8]

## <a name="4-score-hello-model"></a>4. Hej poängmodell
Vi kommer nu att testa hur hello modellen presterar på testdata. Vi kommer att jämföra hello algoritmen vi valt med en annan algoritm toosee som presterar bäst.

1. Dra **Poängmodell** modulen hello arbetsytan.
    Första indata: tränats modell, andra indata: testdata ![hello poängmodell][9]
2. Dra hello **Two-Class Bayes Point-dator** till hello experimentet. Vi kommer att jämföra hur den här algoritmen presterar i jämförelse toohello två-Tvåklassförhöjda beslutsträdet.
3. Kopiera och klistra in hello modulerna Träningsmodell och Poängmodell i hello arbetsytan.
4. Dra hello **utvärdera modell** modulen till hello arbetsytan toocompare hello två algoritmer.
5. **Kör** hello experiment.
   ![Kör hello experiment][10]
6. Hello utdataporten längst ned hello hello utvärdera modell och klicka på visualisera.
   ![Visualisera utvärderingsresultaten][11]

hello mätvärdena är hello ROC-kurvan, precision återkallning diagram och lyfta kurvan. Titta på de här måtten Se vi att hello första modellen presterade bättre än hello andra. toolook på hello vad hello första modellen förutsade, klicka på utdataporten för hello Poängmodell och klicka på visualisera.
![Visualisera poängresultat][12]

Du ser två kolumner som lagts till tooyour testdata.

* Poängsatt sannolikhet: hello sannolikheten att en kund är en cykelköpare.
* Poängsatta etiketter: hello klassificering utförd av hello modellen – cykelköpare (1) eller inte (0). Det här sannolikhetströskelvärdet för etikettering anges too50% och kan justeras.

Genom att jämföra hello kolumnen BikeBuyer (faktiska) med hello poängsatta etiketter (förutsagda), kan du se hur väl hello modellen har utförts. Som nästa steg kan du använda den här modellen toomake förutsägelser för nya kunder och publicera den här modellen som en webbtjänst eller skriva resultaten tillbaka tooSQL Data Warehouse.

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du skapar förutsägbara maskininlärningsmodeller, referera för[introduktion tooMachine Learning på Azure][Introduction tooMachine Learning on Azure].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction tooMachine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
