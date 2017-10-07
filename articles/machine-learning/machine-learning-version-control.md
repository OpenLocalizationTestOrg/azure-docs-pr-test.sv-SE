---
title: aaaALM i Azure Machine Learning | Microsoft Docs
description: "Tillämpa Metodtips för programhantering livscykel i Azure Machine Learning Studio"
keywords: ALM AML, Azure ML, programhanteringen livscykel versionskontroll
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1be6577d-f2c7-425b-b6b9-d5038e52b395
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: haining
ms.openlocfilehash: 99470ff72fea7ab59d9d44f3fded7b9dd49a38c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio"></a>Livscykel för programhantering i Azure Machine Learning Studio
Azure Machine Learning Studio är ett verktyg för utveckling av machine learning-experiment som operationalized i hello Azure cloud platform. Det är som hello Visual Studio IDE och skalbar Molntjänsten samman i en enda plattform. Du kan införliva standard programmet livscykeln för hantering (ALM) praxis från versionshantering olika tillgångar tooautomated körning och distribution i Azure Machine Learning Studio. Den här artikeln beskrivs några av hello alternativ och metoder.

## <a name="versioning-experiment"></a>Versionshantering experiment
Det finns två sätt tooversion experimenten. Du kan antingen förlitar sig på inbyggda körningshistorik eller exportera hello experiment i JavaScript Object Notation (JSON)-format och hantera den externt. Tillvägagångssätten levereras med dess- och nackdelar.

### <a name="experiment-snapshots-using-run-history"></a>Experiment ögonblicksbilder med Körningshistorik
I hello körningsmodell för hello Azure Machine Learning Studio learning experiment, varje gång du klickar på hello **kör** knappen i Redigeraren för hello experiment, en oföränderlig ögonblicksbild av hello experiment är skickade toohello jobbschemaläggaren. Du kan visa den här listan över ögonblicksbilder genom att klicka på hello **Körningshistorik** hello kommandofältet i hello experiment editor vy-knappen.

![Kör tidigare](media/machine-learning-version-control/runhistory.png)

Du kan sedan öppna hello ögonblicksbild i låst läge genom att klicka på hello namnet på hello experiment på hello tid hello experimentet har skickats toorun och hello ögonblicksbilden togs. Observera att endast hello första objektet i listan hello som representerar hello aktuella experiment, är i ett redigerbart tillstånd. Observera att varje ögonblicksbild kan ha olika Status inklusive klar (delvis kör), misslyckades, tillstånd, misslyckades (delvis kör), eller också utkast.

![Kör listan tidigare](media/machine-learning-version-control/runhistorylist.png)

När den har öppnats kan du spara hello ögonblicksbild experiment som ett nytt experiment och sedan ändra den. Om din experiment ögonblicksbild innehåller resurser, t.ex en tränad modell-, omvandlings- eller datamängd som har uppdaterat versioner, behåller hello ögonblicksbild hello referenser toohello originalversionen när hello ögonblicksbilden togs. Om du sparar hello låst ögonblicksbild som ett nytt experiment Azure Machine Learning Studio identifierar hello förekomsten av en nyare version av dessa tillgångar och uppdateras automatiskt i hello nytt experiment.

Om du tar bort hello experiment bort alla ögonblicksbilder av försöket.

### <a name="exportimport-experiment-in-json-format"></a>Export/import experiment i JSON-format
ögonblicksbilder av rapporthistorik hello kör behålla en oföränderlig version av hello experiment i Azure Machine Learning Studio när så är skickade toorun. Du kan också spara en lokal kopia av hello experiment och kontrollera i tooyour favorit källkontrollsystem, till exempel Team Foundation Server och skapa senare på ett experiment från den lokala filen igen. Du kan använda hello [Azure Machine Learning PowerShell](http://aka.ms/amlps) kommandon [ *Export AmlExperimentGraph* ](https://github.com/hning86/azuremlps#export-amlexperimentgraph) och [ * Importera AmlExperimentGraph* ](https://github.com/hning86/azuremlps#import-amlexperimentgraph) tooaccomplish som.

hello JSON-fil är en textrepresentation av hello experimentera diagram, vilket kan innebära en referens tooassets hello arbetsytan, till exempel ett dataset eller en tränad modell. Den innehåller inte en serialiserad version av hello tillgång. Om du försöker tooimport hello JSON-dokumentet tillbaka till hello arbetsytan tillgångar hello refererar till måste redan finnas med hello samma tillgång ID: N som refereras i hello experiment. Annars kommer du inte att kunna tooaccess hello importeras experiment.

## <a name="versioning-trained-model"></a>Versionshantering tränade modellen
En tränad modell i Azure Machine Learning serialiseras till ett format som kallas en .iLearner-fil och lagras i hello Azure Blob storage-konto som är associerade med hello arbetsytan. Enkelriktade tooget en kopia av hello .iLearner filen är via hello omtränings-API. [Den här artikeln](machine-learning-retrain-models-programmatically.md) beskriver hur hello omtränings API fungerar. hello anvisningar:

1. Ställ in experimentet utbildning.
2. Lägga till en web service utgående port toohello träningsmodellmodulen eller hello-modul som ger hello tränad modell, till exempel finjustera modellen Hyperparameter eller skapa R-modell.
3. Kör experimentet utbildning och distribuera det som en webbtjänst för utbildning av modellen.
4. Anropa hello BES-slutpunkten för webbtjänsten för hello utbildning och ange filnamnet för hello önskade .iLearner och Blob lagringskontoplatsen där den lagras.
5. Skörd hello producerade .iLearner filen när hello BES anropa har slutförts.

Ett annat sätt tooretrieve hello .iLearner filen är via hello PowerShell-kommandot [ *Download AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput). Det blir enklare om du bara vill tooget en kopia av hello .iLearner filen utan hello måste tooretrain hello modellen programmässigt.

När du har hello .iLearner-fil som innehåller hello tränad modell, kan du använda en egen strategi för versionshantering. hello strategi kan vara så enkelt som tillämpa pre/username@Domain som en namngivningskonvention bara lämnar hello .iLearner fil i Blob storage och kopiera/importerar den till din systemet för versionskontroll.

hello sparade .iLearner filen kan sedan användas för poäng genom distribuerade webbtjänster.

## <a name="versioning-web-service"></a>Webbtjänst för versionshantering
Du kan distribuera två typer av webbtjänster från ett Azure Machine Learning-experiment. hello klassiska webbtjänsten är direkt kopplade till hello experiment samt hello arbetsytan. hello nya webbtjänsten använder hello Azure Resource Manager framework och det inte längre används tillsammans med hello ursprungliga experiment eller hello arbetsyta.

### <a name="classic-web-service"></a>Klassiska webbtjänst
Du kan dra nytta av hello web service endpoint konstruktion tooversion klassiska webbtjänsten. Här är en typisk flöde:

1. Från din prediktivt experiment kan du distribuera en ny klassiska webbtjänst, som innehåller en standardslutpunkt.
2. Du kan skapa en ny slutpunkt med namnet ep2 som visar hello nuvarande version av hello experiment/tränat modellen.
3. Du kan gå tillbaka och uppdatera din prediktivt experiment och tränad modell.
4. Du distribuerar hello prediktivt experiment, som sedan uppdaterar hello standardslutpunkten. Men det kommer inte att ändra ep2.
5. Du kan skapa en ytterligare slutpunkt med namnet ep3 som visar hello ny version av hello experiment och tränad modell.
6. Gå tillbaka toostep 3 om det behövs.

Över tiden, kanske du har många slutpunkter har skapats i hello samma webbtjänsten. Varje slutpunkt representerar en tidpunkt i kopia av hello experiment som innehåller hello tidpunkts-versionen av hello tränade modellen. Du kan sedan använda externa logiska toodetermine vilka endpoint toocall vilket effektivt innebär att välja en version av hello tränats modellen för hello poäng kör.

Du kan också skapa slutpunkter för många identiska webbtjänster och korrigering olika versioner av hello .iLearner filen toohello endpoint tooachieve liknande effekt. [Den här artikeln](machine-learning-create-models-and-endpoints-with-powershell.md) beskrivs i detalj hur tooaccomplish som.

### <a name="new-web-service"></a>Ny webbtjänst
Om du skapar en ny Azure Resource Manager-baserad webbtjänst hello endpoint konstruktion inte längre tillgängligt. Du kan i stället generera web service definition (WSD)-filer i JSON-format från din prediktivt experiment med hjälp av hello [Export AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell-kommandot, eller genom att använda hello [ *Export AzureRmMlWebservice* ](https://msdn.microsoft.com/library/azure/mt767935.aspx) PowerShell-kommandot från en distribuerad Resource Manager-baserad webbtjänst.

När du har exporterat hello WSD-filen och kontroll över den, du kan också distribuera hello WSD som en ny webbtjänst i en annan web service-plan i en annan Azure-region. Kontrollera att du anger rätt hello lagringskonto som även hello nya web service-plan-ID. toopatch i olika .iLearner filer som du kan ändra hello WSD-filen och uppdatera hello platsreferensen av hello tränats modell och distribuera det som en ny webbtjänst.

## <a name="automate-experiment-execution-and-deployment"></a>Automatisera distribution och körning av experimentet
En viktig del av ALM är toobe kan tooautomate hello körning och processen för distribution av programmet hello. I Azure Machine Learning du åstadkommer detta genom att använda hello [PowerShell-modulen](http://aka.ms/amlps). Här är ett exempel på en slutpunkt till slutpunkt steg som är relevanta tooa standard ALM automatisk körning/distributionsprocessen genom att använda hello [Azure Machine Learning Studio PowerShell-modulen](http://aka.ms/amlps). Varje steg är länkade tooone eller flera PowerShell-kommandon som du kan använda tooaccomplish steg.

1. [Överför en datamängd](https://github.com/hning86/azuremlps#upload-amldataset).
2. Kopiera en träningsexperiment till hello arbetsyta från en [arbetsytan](https://github.com/hning86/azuremlps#copy-amlexperiment) eller från [galleriet](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery), eller [importera](https://github.com/hning86/azuremlps#import-amlexperimentgraph) en [exporteras](https://github.com/hning86/azuremlps#export-amlexperimentgraph) experiment från lokal disk.
3. [Uppdatera hello dataset](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) i hello utbildning experiment.
4. [Kör hello träningsexperiment](https://github.com/hning86/azuremlps#start-amlexperiment).
5. [Befordra hello tränade modellen](https://github.com/hning86/azuremlps#promote-amltrainedmodel).
6. [Kopiera en prediktivt experiment](https://github.com/hning86/azuremlps#copy-amlexperiment) till hello arbetsytan.
7. [Uppdatera hello tränade modellen](https://github.com/hning86/azuremlps#update-amlexperimentuserasset) i hello prediktivt experiment.
8. [Kör hello prediktivt experiment](https://github.com/hning86/azuremlps#start-amlexperiment).
9. [Distribuera en webbtjänst](https://github.com/hning86/azuremlps#new-amlwebservice) från hello prediktivt experiment.
10. Testa hello webbtjänsten [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) eller [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) slutpunkt.

## <a name="next-steps"></a>Nästa steg
* Hämta hello [Azure Machine Learning Studio PowerShell](http://aka.ms/amlps) modulen och starta tooautomate ALM-uppgifter.
* Lär dig hur för[skapa och hantera stort antal ML modeller med bara ett enda experiment](machine-learning-create-models-and-endpoints-with-powershell.md) via PowerShell och omtränings-API.
* Lär dig mer om [distribuerar Azure Machine Learning-webbtjänster](machine-learning-publish-a-machine-learning-web-service.md).
