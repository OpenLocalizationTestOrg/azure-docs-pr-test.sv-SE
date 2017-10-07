---
title: "aaa ”Azure Batch aktiviteten Starta händelsen | Microsoft Docs ”"
description: "Referens för batchaktiviteten Starta händelsen."
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
ms.openlocfilehash: 2cb066be1578741125e9081a84a2b7c74dc8356a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="task-start-event"></a>Aktiviteten Starta händelsen

 Denna händelse genereras när en uppgift har schemalagda toostart på en beräkningsnod av hello Schemaläggaren. Observera att om hello uppgift är ett nytt försök eller placerats i kö händelsen kommer att orsakat igen för hello samma aktivitet, men hello antal återförsök och systemversionen aktivitet uppdateras.


 hello visar följande exempel hello brödtexten i en händelse för aktiviteten starta.

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
        "retryCount": 0
    }
}
```

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|JobId|Sträng|hello-id för hello jobb som innehåller hello aktiviteten.|
|id|Sträng|hello-id för hello-aktivitet.|
|taskType|Sträng|hello typ av uppgift som hello. Detta kan vara visar det är en projektaktivitet manager ' JobManager' eller 'User' som anger den inte är en projektaktivitet manager.|
|systemTaskVersion|Int32|Detta är hello interna försök räknare för en uppgift. Internt kan hello Batch-tjänsten försöka med att en uppgift tooaccount för tillfälliga problem. De här problemen kan innehålla interna schemaläggning fel eller försök toorecover från compute-noder i ett felaktigt tillstånd.|
|[nodeInfo](#nodeInfo)|Komplex typ|Innehåller information om hello compute-nod på vilka hello uppgiften kördes.|
|[multiInstanceSettings](#multiInstanceSettings)|Komplex typ|Anger att hello är flera Instansuppgift som kräver flera compute-noder.  Se [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) mer information.|
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
|numberOfInstances|int|hello antal datornoderna krävs hello uppgiften.|

###  <a name="constraints"></a>villkor

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|hello maximalt antal gånger hello uppgift får köras igen. hello Batch-tjänsten försöker upprätta en uppgift om dess slutkod inte är noll.<br /><br /> Observera att det här värdet uttryckligen styr hello antalet försök. hello Batch-tjänsten försöker hello aktiviteten en gång och försök sedan in toothis gränsen. Om hello högsta antal försök är 3, försöker Batch en uppgift upp too4 gånger (en inledande försök och 3 försök).<br /><br /> Om hello högsta antal försök är 0 hello Batch-tjänsten inte att försök uppgifter.<br /><br /> Om hello högsta antal försök är -1 försöker hello Batch-tjänsten aktiviteter utan begränsning.<br /><br /> hello standardvärdet är 0 (inga nya försök).|

###  <a name="executionInfo"></a>executionInfo

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|retryCount|Int32|hello antal gånger hello aktiviteten gjordes av hello Batch-tjänsten. hello uppgiften försöks om avslutas med slutkoden noll anges MaxTaskRetryCount in toohello|
