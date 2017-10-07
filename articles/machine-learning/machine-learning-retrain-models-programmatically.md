---
title: aaaRetrain Machine Learning-modeller via programmering | Microsoft Docs
description: "Lär dig hur tooprogrammatically träna om en modell och uppdatera hello web service toouse hello nyligen trained model i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: edbb64c08f7d9edf3c76e23e0cc7e14c0125d697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a>Omtrimning av Azure Machine Learning-modeller via programmering
I den här genomgången får du lära dig hur tooprogrammatically träna om en Azure Machine Learning-webbtjänst med hjälp av C# och hello Machine Learning Batch Execution service.

När du har retrained hello modellen visar hello följande genomgångar av hur tooupdate hello modell i förutsägande webbtjänsten:

* Om du har distribuerat en klassisk webbtjänst i hello Machine Learning-webbtjänster portal finns [träna om en klassisk webbtjänst](machine-learning-retrain-a-classic-web-service.md). 
* Om du har distribuerat en ny webbtjänst finns [träna om en ny webbtjänst med Machine Learning Management-cmdlets för hello](machine-learning-retrain-new-web-service-using-powershell.md).

En översikt över hello omtränings processen, se [träna om en Maskininlärningsmodell](machine-learning-retrain-machine-learning-model.md).

Om du vill toostart med din befintliga nya Azure Resource Manager baserad webbtjänst, se [träna om en befintlig förutsägande webbtjänst](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Skapa ett experiment utbildning
För det här exemplet använder du ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” hello Microsoft Azure Machine Learning prover. 

toocreate hello experiment:

1. Logga in på tooMicrosoft Azure Machine Learning Studio. 
2. Klicka på hello nedre högra hörnet av hello instrumentpanelen, **ny**.
3. Hello Microsoft Samples, Välj exempel 5.
4. toorename hello experiment hello överst i hello experimentet väljer hello experiment namnet ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset”.
5. Typ av inventering modell.
6. Hello längst ned på hello experimentet klickar du på **kör**.
7. Klicka på **Konfigurera webbtjänsten** och välj **Omtränings webbtjänsten**. 

hello följande visar hello första försöket.
   
   ![Första försöket.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Skapa en prediktivt experiment och publicera som en webbtjänst
Nu kan du skapa ett Predicative Experiment.

1. Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänsten**. Detta sparar hello modellen som en Tränad modell och lägger till web service ingående och utgående moduler. 
2. Klicka på **Run** (Kör). 
3. När hello experimentet har slutförts klickar du på **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]**.

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

## <a name="deploy-hello-training-experiment-as-a-training-web-service"></a>Distribuera hello träningsexperiment som en webbtjänst för utbildning
tooretrain hello tränad modell, måste du distribuera hello träningsexperiment som du har skapat som en webbtjänst för Retraining. Den här webbtjänsten måste en *Web Service utdata* modulen anslutna toohello * [Träningsmodell] [ train-model] * modul, toobe kan tooproduce nya tränade modeller.

1. tooreturn toohello utbildning experiment hello experiment ikonen hello vänster Klicka på hello experiment med namnet inventering modellen.  
2. Skriv webbtjänsten i hello Experiment sökobjekt sökrutan. 
3. Dra en *webbtjänst* till hello experimentera arbetsytan och koppla dess utdata toohello *Rensa Data som saknas* modul.  Detta säkerställer att din omtränings data bearbetas hello samma sätt som den ursprungliga informationen för utbildning.
4. Dra två *webbtjänsten utdata* moduler på hello experimentera arbetsytan. Ansluta hello utdata från hello *Träningsmodell* modulen tooone och hello utdata från hello *utvärdera modell* modulen toohello andra. Hej web service utdata för **Träningsmodell** ger oss hello ny tränad modell. Hej utdata kopplade för**utvärdera modell** returnerar att modulen utdata, vilket är hello prestandaresultat.
5. Klicka på **Run** (Kör). 

Därefter måste du distribuera hello träningsexperiment som en webbtjänst som producerar en tränad modell och utvärderingsresultat av modellen. tooaccomplish är detta din nästa uppsättning åtgärder, beroende på om du arbetar med en klassisk webbtjänst eller en ny webbtjänst.  

**Klassiska webbtjänst**

Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **distribuera webbtjänsten [klassisk]**. hello webbtjänsten **instrumentpanelen** visas med hello API-nyckel och hello API hjälpsidan för Batch-körningen. Endast hello Batch Execution-metoden kan användas för att skapa tränade modeller.

**Ny webbtjänst**

Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **distribuera webbtjänsten [ny]**. hello Web Service Azure Machine Learning-webbtjänster portal öppnar toohello distribuera tjänsten webbsidan. Ange ett namn för webbtjänsten och välj en betalningsplan, och klicka sedan **distribuera**. Endast hello Batch Execution-metoden kan användas för att skapa tränade modeller

I båda fallen när experimentet har slutförts körs hello resulterande arbetsflödet ska se ut så här:

![Resulterande arbetsflödet när du kör.][4]



## <a name="retrain-hello-model-with-new-data-using-bes"></a>Träna om hello modellen med nya data med BES
I det här exemplet använder du C# toocreate hello omtränings program. Du kan också använda hello Python eller R exempel kod tooaccomplish den här uppgiften.

toocall hello Omtränings-API: er:

1. Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.
2. Logga in toohello Machine Learning-webbtjänst-portalen.
3. Om du arbetar med en klassisk webbtjänst klickar du på **klassiska webbtjänster**.
   1. Klicka på hello-webbtjänst som du arbetar med.
   2. Klicka på hello standardslutpunkten.
   3. Klicka på **använda**.
   4. Längst ned hello hello **förbruka** i hello sidan **exempelkod** klickar du på **Batch**.
   5. Fortsätt toostep 5 i den här proceduren.
4. Om du arbetar med en ny webbtjänst klickar du på **Web Services**.
   1. Klicka på hello-webbtjänst som du arbetar med.
   2. Klicka på **använda**.
   3. Längst ned hello hello förbruka i sidan hello **exempelkod** klickar du på **Batch**.
5. Kopiera hello C# exempelkod för batchkörning och klistra in den i Program.cs-filen för hello kontrollerar hello namnområde intakt.

Lägg till hello Nuget-paketet Microsoft.AspNet.WebApi.Client som anges i hello kommentarer. tooadd hello referens tooMicrosoft.WindowsAzure.Storage.dll kanske måste du först tooinstall hello-klientbibliotek för Microsoft Azure storage-tjänster. Mer information finns i [Windows lagringstjänster](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-hello-apikey-declaration"></a>Uppdatera hello apikey förklaring
Leta upp hello **apikey** deklaration.

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

I hello **grundläggande förbrukning info** avsnitt i hello **förbruka** , leta upp hello primärnyckel och kopiera den toohello **apikey** deklaration.

### <a name="update-hello-azure-storage-information"></a>Uppdatera hello Azure Storage-informationen
Överför en fil från en lokal enhet (till exempel ”C:\temp\CensusIpnput.csv”) tooAzure lagring hello BES exempelkod, bearbetar den och skriver hello resultaten tillbaka tooAzure lagring.  

tooaccomplish den här uppgiften, måste du hämta hello namn och nyckel behållaren information om Lagringskonto för ditt lagringskonto hello klassiska Azure-portalen och hello uppdatera motsvarande värden i hello kod. 

1. Logga in toohello klassiska Azure-portalen.
2. Klicka på hello navigeringen till vänster i kolumnen **lagring**.
3. Välj en toostore hello retrained hello listan över storage-konton och modell.
4. Hello längst hello-sidan, klickar du på **hantera åtkomstnycklar**.
5. Kopiera och spara hello **primära åtkomstnyckeln** och Stäng hello dialogrutan. 
6. Hello överst på hello-sidan, klickar du på **behållare**.
7. Välj en befintlig behållare eller skapa en ny och spara hello namn.

Leta upp hello *StorageAccountName*, *StorageAccountKey*, och *StorageContainerName* deklarationer och uppdatera hello värden som du sparade från hello Azure-portalen.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

Du måste också kontrollera hello indatafilen är tillgänglig på hello-plats som du anger i hello kod. 

### <a name="specify-hello-output-location"></a>Ange hello platsen
När du anger hello platsen i hello begära nyttolast hello-tillägget för hello-fil som anges i *RelativeLocation* måste anges som ilearner. 

Se följande exempel hello:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you would like toouse for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> hello namnen på utdata-platser kan skilja sig från hello i den här genomgången baserat på hello ordning i vilken du har lagt till hello web service utdata moduler. Eftersom du ställer in den här träningsexperiment med två utdata inkludera information om lagring för båda hello resultat.  
> 
> 

![Omtränings utdata][6]

Diagram över 4: Omtränings utdata.

## <a name="evaluate-hello-retraining-results"></a>Utvärdera hello Omtränings resultat
När du kör programmet hello hello utdata innehåller hello URL och SAS-token nödvändiga tooaccess hello utvärderingsresultaten.

Du kan se hello prestandaresultat hello retrained modellen genom att kombinera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* hello utdata resultaten för *output2* (som visas i föregående omtränings utdata avbildningen hello) och klistra in hello fullständiga URL: en i hello webbläsarens adressfält.  

Undersöka hello resultat toodetermine om hello nyligen utbildade modellen presterar bra tillräckligt med tooreplace hello befintliga en.

Kopiera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* från hello utdataresultat som ska användas för dem under hello omtränings process.

## <a name="next-steps"></a>Nästa steg
Om du har distribuerat hello förutsägande webbtjänsten genom att klicka på **distribuera webbtjänsten [klassisk]**, se [träna om en klassisk webbtjänst](machine-learning-retrain-a-classic-web-service.md).

Om du har distribuerat hello förutsägande webbtjänsten genom att klicka på **distribuera webbtjänsten [ny]**, se [träna om en ny webbtjänst med Machine Learning Management-cmdlets för hello](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using hello Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
