---
title: "aaa ”Azure Batch uppgiften complete-händelsen | Microsoft Docs ”"
description: "Referens för Batch uppgiften complete-händelsen."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: c126bf897071c008be3d24190cf77bba5878b807
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="task-complete-event"></a>Fullständig händelseuppgiften

 Denna händelse genereras när en aktivitet har slutförts, oavsett hello slutkod. Den här händelsen kan vara används toodetermine hello varaktighet för en aktivitet som hello uppgiften kördes och om den gjordes.


 hello visar följande exempel hello brödtexten i en aktivitet slutförts händelse.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 0,
        "retryCount": 0,
        "requeueCount": 0
    }
}
```

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|JobId|Sträng|hello-id för hello jobb som innehåller hello aktiviteten.|
|id|Sträng|hello-id för hello-aktivitet.|
|taskType|Sträng|hello typ av uppgift som hello. Detta kan vara visar det är en projektaktivitet manager ' JobManager' eller 'User' som anger den inte är en projektaktivitet manager. Den här händelsen har inte genererats för jobbet förberedande uppgifter, versionen jobbuppgifter eller starta uppgifter.|
|systemTaskVersion|Int32|Detta är hello interna försök räknare för en uppgift. Internt kan hello Batch-tjänsten försöka med att en uppgift tooaccount för tillfälliga problem. De här problemen kan innehålla interna schemaläggning fel eller försök toorecover från compute-noder i ett felaktigt tillstånd.|
|[nodeInfo](#nodeInfo)|Komplex typ|Innehåller information om hello compute-nod på vilka hello uppgiften kördes.|
|[multiInstanceSettings](#multiInstanceSettings)|Komplex typ|Anger hello aktiviteten är en aktivitet för flera instanser som kräver flera compute-noder.  Se [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) mer information.|
|[villkor](#constraints)|Komplex typ|hello körning begränsningar som gäller toothis aktivitet.|
|[executionInfo](#executionInfo)|Komplex typ|Innehåller information om hello körningen av hello-aktivitet.|

###  <a name="nodeInfo"></a>nodeInfo

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|poolId|Sträng|hello-id för hello poolen på vilka hello uppgiften kördes.|
|nodeId|Sträng|hello-id för hello nod på vilka hello uppgiften kördes.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|numberOfInstances|Int32|hello antal datornoderna krävs hello uppgiften.|

###  <a name="constraints"></a>villkor

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|hello maximalt antal gånger hello uppgift får köras igen. hello Batch-tjänsten försöker upprätta en uppgift om dess slutkod inte är noll.<br /><br /> Observera att det här värdet uttryckligen styr hello antalet försök. hello Batch-tjänsten försöker hello aktiviteten en gång och försök sedan in toothis gränsen. Om hello högsta antal försök är 3, försöker Batch en uppgift upp too4 gånger (en inledande försök och 3 försök).<br /><br /> Om hello högsta antal försök är 0 hello Batch-tjänsten inte att försök uppgifter.<br /><br /> Om hello högsta antal försök är -1 försöker hello Batch-tjänsten aktiviteter utan begränsning.<br /><br /> hello standardvärdet är 0 (inga nya försök).|

###  <a name="executionInfo"></a>executionInfo

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|startTime|Datum och tid|hello tid hello aktivitetsinformation började köras. 'Körs' motsvarar toohello **kör** tillstånd, så om hello aktivitet anger resursfiler eller programpaket, visar hello starttiden hello tid vid vilken hello uppgiften startas hämta eller distribution av dessa.  Om hello aktiviteten har startats om eller ett nytt försök, är hello senast vid vilka hello uppgiften har startats.|
|endTime|Datum och tid|hello tid vid vilken hello uppgiften slutfördes.|
|Slutkod|Int32|hello avslutningskoden hello-aktivitet.|
|retryCount|Int32|hello antal gånger hello aktiviteten gjordes av hello Batch-tjänsten. hello uppgiften försöks om avslutas med slutkoden noll anges MaxTaskRetryCount in toohello.|
|requeueCount|Int32|Hej antal gånger hello aktivitet har åter placerats i kö av hello Batch-tjänsten som hello resultatet av en användarbegäran.<br /><br /> När hello användaren tar bort noder från en pool (genom att ändra storlek på eller krympa hello pool) eller när hello jobbet är inaktiverad, hello användaren kan ange att köra uppgifter på hello noder vara placerats i kö för körning. Räknaren håller reda på hur många gånger hello aktivitet har åter placerats i kö av dessa skäl.|
