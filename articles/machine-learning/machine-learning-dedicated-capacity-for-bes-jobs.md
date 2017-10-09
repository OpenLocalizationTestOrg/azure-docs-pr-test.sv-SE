---
title: "aaaDedicated kapacitet för Machine Learning Batch Execution Service jobb | Microsoft Docs"
description: "Översikt över Azure Batch-tjänster för Machine Learning-jobb."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: bba7970bb31c50e5b0b9d5f4ff4f0d2dacf942e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-service-for-machine-learning-jobs"></a>Azure Batch-tjänsten för Machine Learning-jobb

Machine Learning Batch-Pool bearbetning innehåller kundhanterad skala för hello Azure Machine Learning Batch Execution Service. Klassiska batchbearbetning för machine learning sker i en miljö med flera innehavare, vilka gränser hello antal samtidiga jobb som du kan skicka och jobb i kö på grundval av first i first out. Den här osäkerhet innebär att du inte kan förutsäga när jobbet ska köras.

Poolen batchbearbetning kan toocreate pooler som du kan skicka batchjobb. Du styr hello storleken på hello poolen och toowhich pool hello jobbet har skickats. BES-jobb körs i sin egen bearbetning utrymme att tillhandahålla förutsägbar bearbetning och hello möjlighet toocreate med resurspooler som motsvarar toohello belastningen som du skickar.

## <a name="how-toouse-batch-pool-processing"></a>Hur toouse Batch-Pool bearbetning

Konfigurationen av gruppbearbetning poolen är inte tillgänglig via hello Azure-portalen. toouse Batch-Pool bearbetning, måste du:

-   Anropa CSS toocreate en Pool Batch-kontot och få en Pool tjänst-URL och auktoriseringsnyckel
-   Skapa en ny Resource Manager-baserat webbtjänsten och faktureringsavtal

toocreate ditt konto Ring Microsofts kundservice och Support (CSS) och ange ditt prenumerations-ID. CSS fungerar med du toodetermine hello lämplig kapacitet för ditt scenario. CSS konfigurerar ditt konto med hello maximalt antal pooler kan du skapa och hello maximalt antal virtuella datorer (VM) som du kan placera i varje pool. När du har konfigurerat ditt konto finns din Pool URL: en och auktoriseringsnyckel.

När ditt konto har skapats kan använda du hello poolen URL: en och auktorisering viktiga tooperform pool hanteringsåtgärder på Batch-Pool.

![Arkitektur för batch-pool.](media/machine-learning-dedicated-capacity-for-bes-jobs/pool-architecture.png)

Du kan skapa pooler genom att anropa hello skapa poolen igen på hello poolen tjänst-URL som angetts CSS-tooyou. När du skapar en pool kan du ange hello antalet virtuella datorer och hello URL för hello swagger.json av en ny Resource Manager baserat Machine Learning-webbtjänst. Den här webbtjänsten tillhandahålls tooestablish hello fakturering association. hello Batch-Pool-tjänsten använder hello swagger.json tooassociate hello pool med ett faktureringsavtal. Du kan köra alla BES webbtjänst, både nya Resource Manager-baserat och classic du väljer på hello poolen.

Du kan använda alla nya Resource Manager-baserat webbtjänst, men tänk på att debiteras hello fakturering för hello jobb mot hello faktureringsplan som är associerade med den tjänsten. Du kanske vill toocreate som en webbtjänst och nya fakturering planera för Batch-Pool jobb som körs.

Mer information om hur du skapar webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

När du har skapat en pool kan du skicka hello BES hello jobbet med Batch-begäranden med URL: en för hello-webbtjänsten. Du kan välja toosubmit den tooa pool eller tooclassic batch-bearbetning. toosubmit en Pool bearbetningen av jobbet tooBatch lägga hello följande begärandetexten för parametern toohello jobbet skickas:

”AzureBatchPoolId” ”:&lt;poolen ID&gt;”

Om du inte lägga till parametern hello körs hello jobbet i hello klassiska batch processmiljö. Om hello poolen har tillgängliga resurser, startar hello jobbet körs omedelbart. Om hello poolen saknar lediga resurser i ditt jobb kö tills en resurs som är tillgänglig.

Om du upptäcker att du regelbundet når hello kapaciteten för din pooler, och du behöver bättre kapacitet kan du arbeta med ett representativt tooincrease kvoterna anropa CSS.

Exempelbegäran:

https://ussouthcentral.Services.azureml.NET/subscriptions/80c77c7674ba4c8c82294c3b2957990c/Services/9fe659022c9747e3b9b7b923c3830623/jobs?API-version=2.0

```json
{

    "Input":{
    
        "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpoint
        =https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://zhguim
        l.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;;",
        
        "BaseLocation":null,
        
        "RelativeLocation":"testint/AdultCensusIncomeBinaryClassificationDataset.csv",
        
        "SasBlobToken":null
    
    },
    
    "GlobalParameters":{ },
    
    "Outputs":{
    
        "output1":{
        
            "ConnectionString":"DefaultEndpointsProtocol=https;BlobEndpoint=https://sampleaccount.blob.core.windows.net/;TableEndpo
            int=https://sampleaccount.table.core.windows.net/;QueueEndpoint=https://sampleaccount.queue.core.windows.net/;FileEndpoint=https://sampleaccount.file.core.windows.net/;AccountName=sampleaccount;AccountKey=&lt;Key&gt;",
            "BaseLocation":null,
            "RelativeLocation":"testintoutput/testint\_results.csv",
            
            "SasBlobToken":null
        
        }
    
    },
    
    "AzureBatchPoolId":"8dfc151b0d3e446497b845f3b29ef53b"

}
```

## <a name="considerations-when-using-batch-pool-processing"></a>Information om bearbetningen av Batch-Pool

Gruppbearbetning poolen är en alltid i fakturerbar tjänst och som kräver tooassociate den med Resource Manager baserad faktureringsavtal. Endast debiteras du för hello antal beräkningstimmar hello poolen körs på. oavsett hello antal jobb som körs under tiden poolen. Om du skapar en pool, debiteras du för hello beräkningstimmar för varje virtuell dator i hello poolen tills hello poolen tas bort även om inga batchjobb körs i hello pool. Fakturering för hello virtuella datorer startar när etableringen är klar och stoppas när de har tagits bort. Du kan använda någon av hello-planer finns på hello [Machine Learning-priser sidan](https://azure.microsoft.com/pricing/details/machine-learning/).

Fakturering exempel:

Om du skapar en Batch-Pool med 2 virtuella datorer och ta bort den efter 24 timmar debiteras faktureringsavtalet 48 beräkningstimmar; oavsett hur många jobb kördes under denna period.

Om du skapar en Batch-Pool med 4 virtuella datorer och ta bort den efter 12 timmar, är också faktureringsavtalet Debiterat 48 beräkningstimmar.

Vi rekommenderar att avsöka hello jobbet status toodetermine när jobben har slutförts. Anropa hello ändra storlek på poolen igen tooset hello antalet virtuella datorer i hello poolen toozero när dina jobb har körts. Om du har ont om resurser och du behöver toocreate en ny pool, till exempel toobill mot en annan faktureringsplan, du kan ta bort hello poolen i stället när dina jobb har körts.


| **Använd bearbetning när Batch-Pool**    | **Klassiska batchbearbetning när**  |
|---|---|
|Du behöver toorun ett stort antal jobb<br>Eller<br/>Du behöver tooknow som dina jobb ska köras omedelbart<br/>Eller<br/>Du måste garanterad genomflöde. Till exempel du behöver toorun ett antal jobb i en angiven tidsperiod och vill tooscale ut din beräkning resurser toomeet dina behov.    | Du kör några jobb<br/>Och<br/> Du behöver inte hello jobb toorun omedelbart |
