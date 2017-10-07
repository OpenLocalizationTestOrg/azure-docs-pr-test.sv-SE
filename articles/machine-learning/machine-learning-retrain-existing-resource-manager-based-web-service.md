---
title: "en befintlig förutsägande aaaRetrain webbtjänsten | Microsoft Docs"
description: "Lär dig hur tooretrain en modell och uppdatera hello web service toouse hello nyligen tränade modellen i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
ms.openlocfilehash: fb0760d0a2adc34fc5f3df1ae41bdac075f91bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a>Träna om en befintlig förutsägande webbtjänst
Det här dokumentet beskriver hello omtränings-process för hello följande scenario:

* Du har en träningsexperiment och en prediktivt experiment som du har distribuerat som en operationalized webbtjänst.
* Du har nya data som du vill att din förutsägande web service toouse tooperform dess bedömningen.

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

Från och med din befintliga webbtjänsten och experiment, behöver du toofollow de här stegen:

1. Uppdatera hello modellen.
   1. Ändra din utbildning experiment tooallow för web service in- och utdataenheter.
   2. Distribuera hello träningsexperiment som en omtränings-webbtjänst.
   3. Använda hello utbildning experiment Batch Execution Service BES-tooretrain hello modellen.
2. Använd hello Azure Machine Learning PowerShell-cmdlets tooupdate hello prediktivt experiment.
   1. Logga in tooyour Azure Resource Manager-konto.
   2. Hämta hello web service definition.
   3. Exportera hello web service definition som JSON.
   4. Uppdatera hello toohello ilearner referensblobben i hello JSON.
   5. Importera hello JSON till en web service definition.
   6. Uppdatera hello-webbtjänsten med en ny web service definition.

## <a name="deploy-hello-training-experiment"></a>Distribuera hello träningsexperiment
toodeploy hello träningsexperiment som en omtränings webbtjänst, måste du lägga till web service in- och utdataenheter toohello modell. Genom att ansluta en *Web Service utdata* modulen toohello experiment  *[Träningsmodell] [ train-model]*  modulen, aktivera hello utbildning experiment tooproduce en tränad modell som du kan använda i experimentet förutsägbara. Om du har en *utvärdera modell* modulen, du kan även bifoga web service utdata tooget hello utvärderingsresultaten som utdata.

tooupdate experimentet utbildning:

1. Ansluta en *Web Service inkommande* modulen tooyour indata (till exempel en *Rensa Data som saknas* module). Vanligtvis vill du tooensure som bearbetas i dina indata hello samma sätt som den ursprungliga informationen för utbildning.
2. Ansluta en *Web Service utdata* modulen toohello utdata från din *Träningsmodell* modul.
3. Om du har en *utvärdera modell* modul och du vill att toooutput hello utvärderingsresultaten kan ansluta en *Web Service utdata* modulen toohello utdata från din *utvärdera modell* modul.

Kör experimentet.

Därefter måste du distribuera hello träningsexperiment som en webbtjänst som producerar en tränad modell och utvärderingsresultat av modellen.  

Hello längst ned på hello experimentet klickar du på **konfigurera Web Service**, och välj sedan **distribuera webbtjänsten [ny]**. hello Azure Machine Learning-webbtjänster portalen öppnar toohello **distribuera webbtjänsten** sidan. Ange ett namn för webbtjänsten, väljer en betalningsplan och klicka sedan på **distribuera**. Du kan bara använda hello Batch Execution metod för att skapa tränade modeller.

## <a name="retrain-hello-model-with-new-data-by-using-bes"></a>Träna om hello modellen med nya data med hjälp av BES
I det här exemplet använder vi C# toocreate hello omtränings program. Du kan också använda Python eller R exempel kod tooaccomplish den här uppgiften.

toocall hello omtränings-API: er:

1. Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.
2. Logga in toohello Machine Learning-webbtjänster portal.
3. Klicka på hello-webbtjänst som du arbetar med.
4. Klicka på **använda**.
5. Längst ned hello hello **förbruka** i hello sidan **exempelkod** klickar du på **Batch**.
6. Kopiera hello C# exempelkod för batchkörning och klistra in den i hello Program.cs-filen. Kontrollera att hello namnutrymmet intakt.

Lägg till hello NuGet-paketet Microsoft.AspNet.WebApi.Client, som anges i hello kommentarer. tooadd hello referens tooMicrosoft.WindowsAzure.Storage.dll måste du eventuellt tooinstall hello [klientbiblioteket för Azure Storage-tjänster](https://www.nuget.org/packages/WindowsAzure.Storage).

hello följande skärmbild visar hello **förbruka** sida i portalen för hello Azure Machine Learning-webbtjänster.

![Använda sidan][1]

### <a name="update-hello-apikey-declaration"></a>Uppdatera hello apikey förklaring
Leta upp hello **apikey** deklaration:

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

I hello **grundläggande förbrukning info** avsnitt i hello **förbruka** , leta upp hello primärnyckel och kopiera den toohello **apikey** deklaration.

### <a name="update-hello-azure-storage-information"></a>Uppdatera hello Azure Storage-informationen
Överför en fil från en lokal enhet (till exempel ”C:\temp\CensusIpnput.csv”) tooAzure lagring hello BES exempelkod, bearbetar den och skriver hello resultaten tillbaka tooAzure lagring.  

tooupdate hello Azure Storage-informationen måste du hämta namn och nyckel behållaren information för ditt lagringskonto från hello klassiska Azure-portalen och sedan uppdatera hello correspondi när du har kört experimentet, hello resulterande hello storage-konto arbetsflödet ska vara liknande toohello följande:

![Resulterande arbetsflödet när kör][4]NG värden i hello kod.

1. Logga in toohello klassiska Azure-portalen.
2. Klicka på hello navigeringen till vänster i kolumnen **lagring**.
3. Välj en toostore hello retrained hello listan över storage-konton och modell.
4. Hello längst hello-sidan, klickar du på **hantera åtkomstnycklar**.
5. Kopiera och spara hello **primära åtkomstnyckeln** och Stäng hello dialogrutan.
6. Hello överst på hello-sidan, klickar du på **behållare**.
7. Välj en befintlig behållare eller skapa en ny och spara hello namn.

Leta upp hello *StorageAccountName*, *StorageAccountKey*, och *StorageContainerName* deklarationer och uppdatera hello värden som du sparade från hello klassiska portalen .

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Du måste också kontrollera att hello indatafilen är tillgänglig på hello-plats som du anger i hello kod.

### <a name="specify-hello-output-location"></a>Ange hello platsen
När du anger hello platsen i hello begära nyttolast hello-tillägget för hello-filen som anges i *RelativeLocation* måste anges som `ilearner`. Se följande exempel hello:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you want toouse for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

hello följande är ett exempel på omtränings utdata: ![Omtränings utdata][6]

## <a name="evaluate-hello-retraining-results"></a>Utvärdera hello omtränings resultat
När du kör programmet hello innehåller hello utdata hello URL och delade signaturer åtkomsttoken som är nödvändiga tooaccess hello resultatet.

Du kan se hello prestandaresultat hello retrained modellen genom att kombinera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* hello utdata resultaten för *output2* (som visas i föregående omtränings utdata avbildningen hello) och klistra in hello fullständiga URL: en i hello webbläsarens adressfält.  

Undersöka hello resultat toodetermine om hello nyligen utbildade modellen presterar bra tillräckligt med tooreplace hello befintliga en.

Kopiera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* hello utdata resultaten.

## <a name="retrain-hello-web-service"></a>Träna om hello-webbtjänst
När du träna om en ny webbtjänst uppdatera hello förutsägande web service definition tooreference hello ny tränad modell. hello web service definition är en intern representation av hello tränad modell för hello webbtjänsten och kan inte ändras direkt. Kontrollera att du hämtar hello web service definition för experimentet förutsägbara och inte experimentet utbildning.

## <a name="sign-in-tooazure-resource-manager"></a>Logga in tooAzure Resource Manager
Du måste först loggar in tooyour Azure-konto från inom hello PowerShell-miljö med hjälp av hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.

## <a name="get-hello-web-service-definition-object"></a>Hämta hello Web Service Definition objekt
Hämta sedan hello Web Service Definition objekt genom att anropa hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

toodetermine hello resursgruppens namn för en befintlig webbtjänst kör hello Get-AzureRmMlWebService cmdlet utan några parametrar toodisplay hello web-tjänster i din prenumeration. Leta upp hello webbtjänsten och titta på dess web service-ID. hello hello resursgruppen heter hello fjärde elementet i hello ID precis efter hello *resursgrupper* element. I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Du kan också toodetermine hello resursgruppens namn för en befintlig webbtjänst, logga in toohello Azure Machine Learning-webbtjänster portal. Välj hello-webbtjänst. hello resursgruppens namn används hello femte element av hello URL för webbtjänsten för hello, bara efter hello *resursgrupper* element. I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-object-as-json"></a>Exportera hello Web Service Definition objekt som JSON
toomodify hello definition av hello tränade modellen toouse hello nyligen tränat modellen måste du först använda hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport den filen tooa JSON-format.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob"></a>Uppdatera hello referensblobben toohello ilearner
Leta upp hello [tränad modell], uppdatering hello i hello tillgångar *uri* värdet i hello *locationInfo* nod med hello hello ilearner blob-URI. hello URI genereras genom att kombinera hello *BaseLocation* och hello *RelativeLocation* från hello utdata från hello BES omtränings-anrop.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-hello-json-into-a-web-service-definition-object"></a>Importera hello JSON till ett objekt för Web Service Definition
Du måste använda hello [importera AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello ändrade JSON-fil i ett Web Service Definition-objekt som du kan använda tooupdate hello predicative experiment.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service"></a>Uppdatera hello-webbtjänst
Använd slutligen hello [uppdatering AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello prediktivt experiment.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
