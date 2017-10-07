---
title: 'Steg 3: Skapa ett nytt experiment i Machine Learning | Microsoft Docs'
description: "Steg 3 i hello utveckla en förutsägelselösning genomgång: skapa ett nytt utbildning experiment i Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 4697d461a205c50c8d2aa6a3bd56697840cb30f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Genomgång steg 3: Skapa ett nytt Azure Machine Learning-experiment
Detta är hello tredje steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Skapa en Machine Learning-arbetsyta](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. **Skapa ett nytt experiment**
4. [Träna och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
6. [Få åtkomst till hello-webbtjänsten](machine-learning-walkthrough-6-access-web-service.md)

- - -
hello nästa steg i den här genomgången är toocreate ett experiment i Machine Learning Studio som använder hello datauppsättningen som vi har överförts.  

1. I Studio klickar du på **+ ny** längst hello hello-fönstret.
2. Välj **EXPERIMENT**, och välj sedan ”tomt Experiment”. 

    ![Skapa ett nytt experiment][0]

2. Välj hello standard experimentera namn hello överst i hello arbetsytan och Byt toosomething beskrivande.

    ![Byt namn på experimentet][5]
   
   > [!TIP]
   > Det är en bra idé toofill **sammanfattning** och **beskrivning** för hello experiment i hello **egenskaper** fönstret. Dessa egenskaper ger du hello chansen toodocument hello experiment så att alla som tittar på det senare kan förstå dina mål och metoder.
   > 
   > ![Experiment egenskaper][6]
   > 
3. Hello modulen paletten toohello till vänster i hello experimentet, expandera **sparade datauppsättningar**.
4. Hitta hello dataset som du skapade under **Mina datauppsättningar** och drar den till hello arbetsytan. Du kan också hitta hello datauppsättningen genom att ange hello namn i hello **Sök** rutan ovanför hello palett.  

    ![Lägg till hello dataset toohello experiment][7]

## <a name="prepare-hello-data"></a>Förbereda hello data
Du kan visa hello första 100 datarader hello och del statistisk information för hello hela datauppsättningen: Klicka på hello utdataporten för hello datauppsättningen (hello liten cirkel längst ned hello) och välj **visualisera**.  

Eftersom hello datafilen inte levererades med kolumnrubriker Studio tillhandahåller allmänna rubriker (Kol1, Col2, *etc.*). Bra rubriker är inte viktigt toocreating en modell, men det blir enklare toowork med hello data i hello experiment. Dessutom när vi publicerar slutligen den här modellen i en webbtjänst, så att hello rubriker identifiera hello kolumner toohello användare av hello-tjänsten.  

Vi kan lägga till kolumnrubriker använder hello [redigera Metadata] [ edit-metadata] modul.
Du använder hello [redigera Metadata] [ edit-metadata] modulen toochange metadata som associeras med en datamängd. I detta fall kan använda vi det tooprovide mer eget namn för kolumnrubrikerna. 

toouse [redigera Metadata][edit-metadata], du först ange vilka kolumner toomodify (i det här fallet för alla.) Därefter måste du ange hello åtgärd toobe utförs på dessa kolumner (i det här fallet, ändra kolumnrubrikerna.)

1. Skriv ”metadata” i hello i hello modulpaletten **Sök** rutan. Hej [redigera Metadata] [ edit-metadata] visas i listan för hello-modulen.

2. Klicka och dra hello [redigera Metadata] [ edit-metadata] modulen till hello arbetsytan och släpp det nedan hello datauppsättningen som vi har lagt till tidigare.

3. Ansluta hello dataset toohello [redigera Metadata][edit-metadata]: Klicka på hello utdataporten för hello datauppsättningen (hello liten cirkel längst hello hello dataset) genom att dra toohello indataport av [redigera Metadata ] [ edit-metadata] (hello liten cirkel hello överst i hello module), släpper musknappen hello. hello dataset och modulen vara ansluten även om du flyttar antingen på hello arbetsytan.
   
   hello experimentet bör nu se ut ungefär så här:  
   
   ![Lägga till redigera Metadata][1]
   
   hello rött utropstecken visar att vi inte har ställts in hello egenskaper för den här modulen ännu. Vi ska göra nästa avsnitt.
   
   > [!TIP]
   > Du kan lägga till en kommentar tooa modul genom att dubbelklicka på hello-modulen och skriva in text. Detta kan hjälpa dig en överblick över vilka hello-modulen gör i experimentet. I det här fallet dubbelklickar du på hello [redigera Metadata] [ edit-metadata] modulen och Skriv hello kommentar ”Lägg till kolumnrubrikerna”. Klicka på någon annanstans på hello arbetsytan tooclose hello textruta. toodisplay hello kommentar och klickar på hello nedåtpilen för hello-modulen.
   > 
   > ![Redigera Metadatamodul med kommentar har lagts till][8]
   > 
4. Välj [redigera Metadata][edit-metadata], och i hello **egenskaper** fönstret toohello höger i hello arbetsytan och klicka på **starta kolumnväljaren**.

5. I hello **Markera kolumner** dialogrutan, Välj alla hello rader i **tillgängliga kolumner** och klicka på > toomove dem för**valda kolumner**.
   hello dialogrutan bör se ut så här:

   ![Kolumnväljaren med alla kolumner har valts][2]

6. Klicka på hello **OK** är markerat.

7. Tillbaka i hello **egenskaper** rutan Sök efter hello **nya kolumnnamn** parameter. Ange en lista över namn för hello 21 kolumner i datauppsättning för hello, avgränsade med kommatecken och kolumnordning i det här fältet. Du kan hämta hello kolumner namn hello dataset-dokumentationen på hello UCI webbplats eller i informationssyfte kan du kopiera och klistra in hello följande lista:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   hello egenskapsrutan ser ut så här:
   
   ![Egenskaper för redigera-Metadata][3]

> [!TIP]
> Om du vill tooverify hello kolumnrubriker, köra experimentet hello (klicka på **kör** nedan hello experimentet). När körningen (en grön bock på [redigera Metadata][edit-metadata]), klicka på hello utdataporten för hello [redigera Metadata] [ edit-metadata] modulen och välj **visualisera**. Du kan visa hello utdata från alla moduler i hello samma sätt tooview hello fortskrider hello data via hello experiment.
> 
> 

## <a name="create-training-and-test-datasets"></a>Skapa utbildning och testa datauppsättningar
Vi behöver vissa datamodellen tootrain hello och vissa tootest den.
Så i hello nästa steg i hello experiment kan vi dela hello dataset i två separata datauppsättningar: en för vår modell och en för att testa den.

toodo kan vi använda hello [dela Data] [ split] modul.  

1. Hitta hello [dela Data] [ split] , drar den till hello arbetsytan, och koppla den toohello [redigera Metadata] [ edit-metadata] modul.

2. Som standard är hello dela förhållandet 0,5 och hello **Randomized dela** parameter har angetts. Det innebär att en slumpmässig halvan av hello data utdata via en port för hello [dela Data] [ split] modulen och halva via hello som andra. Du kan justera parametrarna, samt hello **slumptal** parametern toochange hello delas upp mellan träning och testning av data. I det här exemplet vi lämna dem som-är.
   
   > [!TIP]
   > Hej egenskapen **andel av rader i hello första utdatauppsättningen** avgör hur mycket av hello data är utdata via hello *vänstra* utgående port. Till exempel om du ställer in hello förhållandet too0.7 är sedan 70% av hello data utdata via hello vänster port och 30% via hello rätt port.  
   > 
   > 

3. Dubbelklicka på hello [dela Data] [ split] modulen och ange hello kommentar, ”utbildning/testning data dela 50%”. 

Vi kan använda hello utdata för hello [dela Data] [ split] modulen men vi vill, men vi välja toouse hello vänstra utdata som träningsdata och hello höger utdata som tester.  

Som anges i hello [föregående steg](machine-learning-walkthrough-2-upload-data.md), hello kostnaden för misclassifying en hög kreditrisk som låg är fem gånger högre än hello kostnaden för misclassifying en låg kreditrisk som hög. tooaccount för den här vi skapa en ny datamängd som visar den här kostnaden-funktionen. I hello nya datamängden replikeras varje hög risk exempel fem gånger medan varje låg risk exempel inte replikeras.   

Vi kan göra replikeringen med hjälp av R-koden:  

1. Leta upp och dra hello [köra R-skriptet] [ execute-r-script] modulen till hello experimentet. 

2. Ansluta hello kvar utdataporten för hello [dela Data] [ split] modulen toohello första indata port (”Dataset1”) av hello [köra R-skriptet] [ execute-r-script] modul.

3. Dubbelklicka på hello [köra R-skriptet] [ execute-r-script] modulen och ange hello kommentar ”ange kostnaden justering”.

4. I hello **egenskaper** fönstret Ta bort hello standardtexten i hello **R-skriptet** parametern och ange det här skriptet:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-skriptet i hello köra R-skriptet modul][9]

Vi behöver toodo samma replikeringsåtgärden för varje utdata av hello [dela Data] [ split] modulen så att hello träning och testning data har hello samma kostnad justering. Hej enklaste sättet toodo genom att duplicera hello [köra R-skriptet] [ execute-r-script] modulen vi gjorde och ansluta det toohello andra utdata porten för hello [dela Data] [ split] modul.

1. Högerklicka på hello [köra R-skriptet] [ execute-r-script] modulen och välj **kopiera**.

2. Högerklicka på hello experimentet och välj **klistra in**.

3. Dra hello ny modul på plats och ansluter sedan hello rätt utdataporten för hello [dela Data] [ split] modulen toohello första indata-port för det nya [köra R-skriptet] [ execute-r-script] modul. 

4. Klicka hello botten hello arbetsytans **kör**. 

> [!TIP]
> hello kopia av hello köra R-skriptet modulen innehåller hello samma skript som hello ursprunglig modul. När du kopierar och klistrar in en modul på arbetsytan hello behåller hello kopiera alla hello egenskaper för hello ursprungliga.  
> 
> 

Vårt experiment nu ser ut ungefär så här:

![Lägga till delade modulen och R-skript][4]

Mer information om hur du använder R-skript i experimenten finns [Utöka ditt experiment med R](machine-learning-extend-your-experiment-with-r.md).

**Nästa: [tåg och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
