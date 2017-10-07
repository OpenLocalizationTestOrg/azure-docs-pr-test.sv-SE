---
title: aaaImport data till Machine Learning Studio | Microsoft Docs
description: "Hur tooimport data till Azure Machine Learning Studio från olika datakällor. Lär dig mer om vilka typer av data och dataformat stöds."
keywords: "Importera data, dataformatet, datatyper, datakällor, träningsdata"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: garye;bradsev
ms.openlocfilehash: 830dcdde9d43809900c520a41d6d94a65731ca3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Importera dina utbildningsdata till Azure Machine Learning Studio från olika datakällor
toouse dina egna data i Machine Learning Studio toodevelop och train en förutsägelseanalys kan du: 

* överföra data från en **lokal fil** före tid från hårddisken-toocreate en dataset-modul i arbetsytan
* åtkomst till data från ett av flera **online datakällor** medan experimentet körs med hello [importera Data] [ import-data] modul 
* använda data från en annan Azure Machine learning **experimentera** sparas som en datamängd
* använda data från en lokal **SQL Server-databas**

Dessa alternativ beskrivs i någon av hello artiklar på hello menyn nedan. Följande avsnitt visar hur tooimport data från dessa olika datakällor toouse i Machine Learning Studio. 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> Det finns ett antal provdatauppsättningar i Machine Learning Studio som du kan använda för träningsdata. Mer information om dessa finns [använda hello provdatauppsättningar i Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).
> 
> 

Den här inledande avsnittet beskrivs hur tooget data redo för använder i Machine Learning Studio också och beskriver vilka dataformat och datatyper som stöds. 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Hämta data som är redo för användning i Azure Machine Learning Studio
Machine Learning Studio är utformad toowork med rektangulär eller tabular data, till exempel textdata som avgränsade eller strukturerade data från en databas, men i vissa fall icke-rektangulärt område data kan användas.

Det är bäst om dina data är relativt ren. Det vill säga ska du tootake vård problem såsom ociterade strängar innan du laddar upp hello data i experimentet.

Det finns dock moduler som är tillgängliga i Machine Learning Studio som gör att vissa manipulation av data i experimentet. Hej maskininlärningsalgoritmer du ska använda, måste du använda toodecide hur du ska hantera data strukturella problem, till exempel värden som saknas och null-optimerade data och det finns moduler som kan hjälpa dig med som. Leta i hello **Data Transformation** avsnitt i hello modulpaletten för moduler som utför dessa funktioner.

Du kan visa eller hämta hello data som produceras av en modul genom att klicka på utdataporten hello när som helst i experimentet. Det kan finnas olika hämtningsalternativ beroende på hello modul eller du kan toovisualize hello data i din webbläsare i Machine Learning Studio.

## <a name="data-formats-and-data-types-supported"></a>Format och datatyper som stöds
Du kan importera ett antal datatyper i experimentet, beroende på vilken metod du använder tooimport data och där det kommer från:

* Oformaterad text (txt)
* Fil med kommaavgränsade värden (CSV med ett huvud (.csv) eller utan) (. nh.csv)
* Tabbavgränsade värden (TVS med ett huvud (TSV) eller utan) (. nh.tsv)
* Excel-fil
* Azure-tabellen
* Hive-tabell
* SQL-databastabell
* OData-värden
* SVMLight data (.svmlight) (se hello [SVMLight definition](http://svmlight.joachims.org/) information format)
* Attributet Relation File Format ARFF ()-data (.arff) (se hello [ARFF definition](http://weka.wikispaces.com/ARFF) information format)
* ZIP-filen (.zip)
* R-objekt eller arbetsytan-fil (. RData)

Om du importerar data i ett format till exempel ARFF som innehåller metadata använder den här metadata toodefine hello rubrik och datatypen för varje kolumn i Machine Learning Studio.

Om du importerar data, till exempel TVS eller CSV-format som inte innehåller dessa metadata härleder Machine Learning Studio hello datatypen för varje kolumn av hello datasampling. Om hello data inte har kolumnrubriker, tillhandahåller Machine Learning Studio standardnamn.

Du kan uttryckligen ange eller ändra hello rubriker och datatyper för kolumner med hjälp av hello [redigera Metadata][edit-metadata].

hello följande **datatyper** som identifieras av Machine Learning Studio:

* Sträng
* Integer
* dubbla
* Booleskt värde
* Datum och tid
* TimeSpan

Machine Learning Studio använder en intern datatyp som kallas ***datatabell*** toopass data mellan moduler. Du kan uttryckligen konvertera data till Data tabellformat med hello [konvertera tooDataset] [ convert-to-dataset] modul.

Alla moduler som accepterar format än datatabell konverterar hello data tooData tabell tyst vidare toohello nästa modul.

Om det behövs kan konvertera du datatabell format tillbaka till CSV, TVS, ARFF eller SVMLight format med hjälp av andra moduler för konvertering.
Leta i hello **Format datakonvertering** avsnitt i hello modulpaletten för moduler som utför dessa funktioner.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
