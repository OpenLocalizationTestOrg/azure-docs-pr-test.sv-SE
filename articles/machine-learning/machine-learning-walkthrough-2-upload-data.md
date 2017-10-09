---
title: 'Steg 2: Ladda upp data till en Machine Learning-experiment | Microsoft Docs'
description: "Steg 2 av hello utveckla en förutsägelselösning genomgång: Överför lagras offentliga data i Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 0ea21dcca2d0934ed06508560cf85cf31b48ce6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Genomgång steg 2: Överför befintliga data i ett Azure Machine Learning-experiment
Detta är hello andra steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Skapa en Machine Learning-arbetsyta](machine-learning-walkthrough-1-create-ml-workspace.md)
2. **Överför befintliga data**
3. [Skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
6. [Få åtkomst till hello-webbtjänsten](machine-learning-walkthrough-6-access-web-service.md)

- - -
toodevelop en förutsägelsemodell för kreditrisk vi behöver data att vi kan använda tootrain och testa hello modellen. Den här genomgången använder vi hello ”UCI Statlog (tyska kredit Data) datauppsättning” från hello UC Irvine Machine Learning-databasen. Du hittar den här:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+Credit+data)</a>

Vi använder hello-fil med namnet **german.data**. Hämta den här filen tooyour lokala hårddisken.  

Hej **german.data** datamängden innehåller rader med 20 variabler för 1000 senaste ansöker om kredit. Variablerna 20 representerar hello dataset uppsättning funktioner (hello *funktionen vector*), vilket möjliggör identifieringsegenskaper för varje kredit sökanden. Ytterligare en kolumn i varje rad representerar hello sökandens beräknade kreditrisk med 700 sökanden identifieras som en låg kreditrisk och 300 som en hög risk.

hello UCI webbplatser som innehåller en beskrivning av hello attribut för hello funktionen-vektor för dessa data. Detta inkluderar finansiell information, kredithistorik, anställningsstatus och personlig information. För varje ansökan har en binär klassificering angivna som anger om de är låg eller hög kredit risk. 

Vi använder den här data tootrain en förutsägelseanalysmodell. När det är klart bör vår modell kunna tooaccept en funktionen-vektor för en ny person och förutsäga om han eller hon är en låg eller hög kreditrisk.  

Här är en intressant snodd. Hej beskrivning av hello dataset i hello UCI webbplats nämns vad det kostar om vi misclassify kreditrisken för en person.
Om hello modellen beräknar en hög kreditrisken för en person som är faktiskt en låg kreditrisk, har hello modellen gjort en felklassificering.
Men hello omvänd felklassificering är fem gånger dyraste toohello finansinstitut: om hello modellen beräknar en låg kreditrisken för en person som är faktiskt en hög kreditrisk.

Så vill vi tootrain vår modell så att hello kostnaden för den här senare typen av felklassificering är fem gånger större än misclassifying hello andra sätt.
Ett enkelt sätt toodo detta när hello modell i vårt experiment är genom att duplicera (fem gånger) transaktioner som representerar någon med en hög kreditrisk. Sedan om hello modellen felaktigt någon som en låg kreditrisk klassificerar när de är faktiskt en hög risk, har hello modellen den samma felklassificering fem gånger en gång för varje kopia. Detta ökar hello kostnaden för det här felet i hello utbildning resultat.


## <a name="convert-hello-dataset-format"></a>Konvertera hello dataset-format
hello ursprungliga datauppsättningen används ett tomt kommaavgränsade format. Machine Learning Studio fungerar bättre med en fil med kommaavgränsade värden (CSV), så att det kommer att konvertera hello datauppsättningen genom att ersätta blanksteg med kommatecken.  

Det finns många sätt tooconvert dessa data. Ett sätt är med hjälp av hello följande Windows PowerShell-kommando:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Ett annat sätt är med hello Unix sed kommando:  

    sed 's/ /,/g' german.data > german.csv  

I båda fallen kan vi har skapat en CSV-version av hello data i en fil med namnet **german.csv** som vi kan använda i vårt experiment.

## <a name="upload-hello-dataset-toomachine-learning-studio"></a>Överför hello dataset tooMachine Learning Studio
När hello data har konverterade tooCSV format, behöver vi tooupload till Machine Learning Studio. 

1. Öppna hello Machine Learning Studio-startsidan ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Klicka på hello ![menyn][1] i hello övre vänstra hörnet av hello klickar du på **Azure Machine Learning**väljer **Studio**, och logga in.

3. Klicka på **+ ny** längst hello hello-fönstret.

4. Välj **DATASET**.

5. Välj **från en lokal fil**.

    ![Lägg till en datamängd från en lokal fil][2]

6. I hello **ladda upp en ny datamängd** dialogrutan klickar du på **Bläddra** och hitta hello **german.csv** fil som du skapade.

7. Ange ett namn för hello dataset. Anropa den ”UCI tyska kreditkortdata” i den här genomgången.

8. Datatypen, Välj **generiska CSV-filen utan rubrik (. nh.csv)**.

9. Lägg till en beskrivning om du vill ha.

10. Klicka på hello **OK** är markerat.  

    ![Överför hello dataset][3]

Detta filöverföringar hello data i en dataset-modul som vi kan använda i ett experiment.

Du kan hantera datauppsättningar som du har överfört tooStudio genom att klicka på hello **DATAUPPSÄTTNINGAR** fliken toohello vänsterkant hello Studio-fönstret.

![Hantera datamängder][4]

Mer information om hur du importerar andra typer av data i ett experiment finns [importera utbildningsdata till Azure Machine Learning Studio](machine-learning-data-science-import-data.md).

**Nästa: [skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
