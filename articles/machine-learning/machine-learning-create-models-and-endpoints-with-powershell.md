---
title: "aaaCreate flera modeller från ett experiment | Microsoft Docs"
description: "Använd PowerShell toocreate flera Machine Learning-modeller och web service-slutpunkter med hello samma algoritm men olika utbildning datauppsättningar."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 4a258a8ab26395d4169a058520151c860e16e169
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Skapa många Machine Learning-modeller och webbtjänstslutpunkter från ett experiment med PowerShell
Här är ett vanligt problem i machine learning: du vill toocreate många modeller som har hello samma utbildning arbetsflödet och Använd hello samma algoritm, men har olika utbildning datauppsättningar som indata. Den här artikeln beskrivs hur du toodo detta skalanpassat i Azure Machine Learning Studio med bara ett enda experiment.

Anta exempelvis att du äger ett globala cykel uthyrning franchise företag. Vill du toobuild regression modellen toopredict hello uthyrning behov baserat på historiska data. Du har 1 000 uthyrning platser över hello world och du har lagrat en datauppsättning för varje plats som innehåller viktiga funktioner, till exempel datum, tid, väder och trafik som är specifika tooeach plats.

Du kan lära din modell som en gång med hjälp av en sammanfogad version av alla hello datauppsättningar på alla platser. Men eftersom var och en av dina platser har en unik miljö bättre skulle vara tootrain din regressionsmodell med hello dataset separat för varje plats. På så sätt kan varje tränad modell kan beakta kontostorlekar hello olika store, volym, geografi, ifyllning, cykel eget trafik miljö *etc.*.

Som kan vara hello bästa sättet, men du vill inte toocreate 1 000 utbildning experiment i Azure Machine Learning med var och en representerar en unik plats. Förutom att det är en överväldigande aktivitet, är det också verkar vara ganska ineffektiv eftersom varje experiment skulle ha alla hello samma komponenter förutom hello utbildning dataset.

Lyckligtvis vi kan göra detta med hjälp av hello [Azure Machine Learning API omtränings](machine-learning-retrain-models-programmatically.md) och automatiskt hello aktivitet med [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).

> [!NOTE]
> toomake exemplet körs snabbare, kommer vi minska hello antalet platser från 1 000 too10. Men hello samma principer och procedurer gäller too1 000 platser. hello enda skillnaden är att om du vill tootrain från 1 000 datauppsättningar vill du förmodligen toothink körs hello följande PowerShell-skript parallellt. Hur toodo som ligger utanför hello av den här artikeln, men du kan hitta exempel på PowerShell flertrådsteknik på hello Internet.  
> 
> 

## <a name="set-up-hello-training-experiment"></a>Ställ in hello träningsexperiment
Vi toouse exempel [träningsexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) som vi redan har skapat i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Öppna experimentet i din [Azure Machine Learning Studio](https://studio.azureml.net) arbetsytan.

> [!NOTE]
> I ordning toofollow tillsammans med det här exemplet vill du kanske toouse standard arbetsytan i stället för en kostnadsfri arbetsyta. Vi ska skapa en slutpunkt för varje kund - 10-slutpunkter – totalt och som kräver en standard arbetsyta eftersom en kostnadsfri arbetsyta är begränsat too3 slutpunkter. Om du bara har en kostnadsfri arbetsyta bara ändra hello skript under tooallow för endast 3 platser.
> 
> 

hello experimentet använder en **importera Data** modulen tooimport hello utbildning dataset *customer001.csv* från ett Azure storage-konto. Antar vi att vi har samlat in utbildning datauppsättningar från alla cykel uthyrning platser och lagrat dem i samma blob lagringsplats med filnamn som sträcker sig från hello *rentalloc001.csv* för*rentalloc10.csv* .

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Observera att en **Web Service utdata** modulen har lagts till toohello **Träningsmodell** modul.
När experimentet distribueras som en webbtjänst kan returneras hello slutpunkten som är associerad med den utgående hello tränade modellen i hello-format för en .ilearner-fil.

Observera även att vi ställer in en web service-parameter för hello URL som hello **importera Data** modulen används. Detta gör att vi toouse hello parametern toospecify enskilda utbildning datauppsättningar tootrain hello modellen för varje plats.
Det finns andra sätt som vi gick har gjort det, till exempel använder en SQL-fråga med en web service parametern tooget data från en SQL Azure-databas, eller bara använda en **webbtjänst** modulen toopass i en dataset toohello webbtjänsten.

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Nu ska vi kör experimentet utbildning med hjälp av standardvärdet för hello *rental001.csv* som hello utbildning dataset. Om du visar hello utdata från hello **utvärdera** modulen (klicka på hello utdata och välj **visualisera**), du kan se vi hämta ett hyfsad prestanda för *AUC* = 0.91. Vi är nu redo toodeploy en webbtjänst utanför utbildning experimentet.

## <a name="deploy-hello-training-and-scoring-web-services"></a>Distribuera hello utbildning och bedömningen webbtjänster
toodeploy hello utbildning webbtjänst, klicka på hello **konfigurera Web Service** nedan hello experimentet och välj **distribuera webbtjänsten**. Anropa den här webbtjänsten ”” cykel uthyrning utbildning ”.

Nu måste vi toodeploy hello bedömningsprofil webbtjänsten.
toodo kan vi kan klicka på **konfigurera Web Service** nedan hello arbetsytan och välj **förutsägande webbtjänsten**. Detta skapar ett bedömningsprofil experiment.
Vi behöver toomake några mindre justeringar toomake den fungerar som en webbtjänst som indata för att ta bort hello etikettkolumnen ”cnt” från hello och begränsa hello utdata tooonly hello instans-id och hello motsvarande förväntad värde.

toosave själv som fungerar, kan du helt enkelt öppna hello [prediktivt experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) i hello galleriet som redan har förberetts.

toodeploy hello-webbtjänsten, kör hello prediktivt experiment, och klicka sedan på hello **distribuera webbtjänsten** nedan hello arbetsytan. Namnet hello bedömningen webbtjänst ”cykel uthyrning bedömningen” ”.

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Skapa 10 slutpunkter för identiska webbtjänster med PowerShell
Den här webbtjänsten levereras med en standardslutpunkt. Men vi har inte intresserad hello standardslutpunkten eftersom den inte uppdateras. Vi behöver toodo är toocreate 10 ytterligare slutpunkter, ett för varje plats. Vi ska göra detta med PowerShell.

Först måste konfigurerar vi våra PowerShell-miljö:

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and is properly set toopoint toohello valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Kör sedan hello följande PowerShell-kommando:

    # Create 10 endpoints on hello scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nu har vi skapat 10 slutpunkter och alla innehålla samma tränad modell tränas på hello *customer001.csv*. Du kan visa dem i hello Azure-hanteringsportalen.

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-hello-endpoints-toouse-separate-training-datasets-using-powershell"></a>Uppdatera hello slutpunkter toouse separata tränings datauppsättningar med hjälp av PowerShell
hello nästa steg är tooupdate hello slutpunkter med modeller tränas unikt på varje enskild kunddata. Men vi måste först tooproduce dessa modeller från hello **cykel uthyrning utbildning** webbtjänsten. Vi går tillbaka toohello **cykel uthyrning utbildning** webbtjänsten. Vi behöver toocall sin BES slutpunkt 10 gånger med 10 olika utbildning datauppsättningar i ordning tooproduce 10 olika modeller. Vi använder hello **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet toodo detta.

Du måste också tooprovide autentiseringsuppgifter för blob storage-konto i `$configContent`, nämligen t.o.m. hello `AccountName`, `AccountKey` och `RelativeLocation`. Hej `AccountName` kan vara något av ditt kontonamn som visas i hello **klassiska Azure-hanteringsportalen** (*lagring* fliken). När du klickar på ett lagringskonto dess `AccountKey` hittar genom att trycka på hello **hantera åtkomstnycklar** längst hello längst ned och kopiera hello *primära åtkomstnyckeln*. Hej `RelativeLocation` är hello sökväg relativa tooyour lagring där en ny modell ska lagras. Till exempel hello sökvägen `hai/retrain/bike_rental/` i hello-skriptet nedan punkter tooa behållare med namnet `hai`, och `/retrain/bike_rental/` är undermappar. För närvarande kan du skapa undermappar via hello portalens användargränssnitt, men det finns [flera Azure-lagringsutforskare](../storage/common/storage-explorers.md) som gör det möjligt toodo så. Vi rekommenderar att du skapar en ny behållare i din lagring toostore hello nya tränade modeller (.ilearner-filer) på följande sätt: från lagringssidan klickar du på hello **Lägg till** knappen längst ned hello och ger den namnet `retrain`. Sammanfattningsvis hello nödvändiga ändringar toohello skriptet nedan gäller för`AccountName`, `AccountKey` och `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke hello retraining API 10 times
    # This is hello default (and hello only) endpoint on hello training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> hello BES-slutpunkten är hello stöds endast läge för den här åtgärden. RR kan inte användas för att framställa tränade modeller.
> 
> 

Som du ser ovan, i stället för att konstruera 10 olika BES jobbet json konfigurationsfiler, vi skapa hello sträng i stället och dynamiskt feed det toohello *jobConfigString* parametern för hello  **InvokeAmlWebServceBESEndpoint** cmdlet, eftersom det inte finns egentligen inget behov av tookeep en kopia för på disken.

Om allt går bra efter en stund bör du se 10 .ilearner filer från *model001.ilearner* för*model010.ilearner*, i ditt Azure storage-konto. Nu är klar tooupdate våra 10 bedömningen web tjänstens slutpunkter med dessa modeller med hello **korrigering AmlWebServiceEndpoint** PowerShell-cmdlet. Kom ihåg igen att vi endast korrigering hello icke-förvalt slutpunkter vi programmässigt skapade tidigare.

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Detta bör köra ganska snabbt. När hello körningen har slutförts ska vi har skapat 10 slutpunkter för förutsägande webbtjänster, som innehåller en tränad modell tränas unikt på hello dataset specifika tooa uthyrning plats, allt från en enda träningsexperiment. tooverify, kan du anropa dessa slutpunkter som använder hello **InvokeAmlWebServiceRRSEndpoint** cmdlet, vilket ger dem med hello samma indata och förväntat toosee olika förutsägelse resultat eftersom hello modeller tränas med olika utbildning uppsättningar.

## <a name="full-powershell-script"></a>Fullständig PowerShell-skript
Här är hello lista över hello fullständig källkoden:

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and properly set toopoint toohello valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on hello scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke hello retraining API 10 times tooproduce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
