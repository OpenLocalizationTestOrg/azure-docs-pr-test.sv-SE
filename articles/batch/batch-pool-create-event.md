---
title: "aaa ”skapa en Azure Batch-pool händelse | Microsoft Docs ”"
description: "Referens för Batch-pool Skapa händelse."
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
ms.openlocfilehash: ad31251969af553baa21e8c533d8ea95d3eeaf91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="pool-create-event"></a>Skapa en pool händelse

 Denna händelse genereras när en pool som har skapats. hello innehåll hello loggen visar allmän information om hello poolen. Observera att om hello mål hello poolen är större än 0 compute-noder, starta för programpoolen storleksändringar följer omedelbart efter den här händelsen.

 hello följande exempel visar hello brödtexten i en pool skapa en händelse för en pool som skapats med hjälp av hello CloudServiceConfiguration egenskapen.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Element|Typ|Anteckningar|
|-------------|----------|-----------|
|id|Sträng|hello-id för hello poolen.|
|Visningsnamn|Sträng|hello visningsnamn för hello poolen.|
|vmSize|Sträng|hello storlek hello virtuella datorer i hello pool. Alla virtuella datorer i en pool är hello samma storlek. <br/><br/> Information om tillgängliga storlekar för virtuella datorer för molntjänster pooler (pooler som skapats med cloudServiceConfiguration), finns i [storlekar för molntjänster](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch stöder alla Cloud Services VM-storlekar utom `ExtraSmall`.<br/><br/> Information om tillgängliga VM storlekar för pooler med hjälp av avbildningar från hello Marketplace för virtuella datorer (pooler som har skapats med virtualMachineConfiguration) finns [storlekar för virtuella datorer](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) eller [storlekar för Virtuella datorer](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Batch har stöd för alla storlekar för virtuella datorer i Azure utom `STANDARD_A0` och de med Premium Storage (`STANDARD_GS`, `STANDARD_DS`- och `STANDARD_DSV2`-serien).|
|[cloudServiceConfiguration](#bk_csconf)|Komplex typ|hello molnet tjänstkonfigurationen för hello poolen.|
|[virtualMachineConfiguration](#bk_vmconf)|Komplex typ|hello konfiguration av virtuell dator för hello poolen.|
|[networkConfiguration](#bk_netconf)|Komplex typ|hello nätverkskonfigurationen för hello poolen.|
|resizeTimeout|Tid|hello tidsgräns för fördelning av beräkning noder toohello poolen som angetts för hello senaste storleksändringen åtgärden på hello poolen.  (hello inledande storleken när hello poolen har skapats räknas som en ändring av storlek).|
|targetDedicated|Int32|hello antal compute-noder som har begärts för hello poolen.|
|enableAutoScale|bool|Anger om justerar hello poolstorlek automatiskt med tiden.|
|enableInterNodeCommunication|bool|Anger om hello poolen har ställts in för direkt kommunikation mellan noder.|
|isAutoPool|bool|Speficies om hello poolen har skapats via ett jobb AutoPool mekanism.|
|maxTasksPerNode|Int32|hello maximalt antal aktiviteter som kan köras samtidigt på en enda beräkningsnod i poolen hello.|
|vmFillType|Sträng|Definierar hur hello Batch-tjänsten distribuerar uppgifter mellan beräkningsnoder i hello pool. Giltiga värden är spridda eller Pack.|

###  <a name="bk_csconf"></a>cloudServiceConfiguration

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|osFamily|Sträng|hello Azure Gästoperativsystem family toobe installerad på hello virtuella datorer i hello pool.<br /><br /> Möjliga värden:<br /><br /> **2** – OS-familjen 2, motsvarande tooWindows Server 2008 R2 SP1.<br /><br /> **3** – OS-familjen 3, motsvarande tooWindows Server 2012.<br /><br /> **4** – OS-familjen 4, motsvarande tooWindows Server 2012 R2.<br /><br /> Mer information finns i [Azure versioner för gästoperativsystem](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Sträng|hello Azure gäst-OS-version toobe installerad på hello virtuella datorer i hello pool.<br /><br /> hello standardvärdet är  **\***  som anger hello senaste operativsystemversion för hello angetts familj.<br /><br /> Andra tillåtna värden finns [Azure versioner för gästoperativsystem](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a>virtualMachineConfiguration

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Komplex typ|Anger information om hello plattform eller toouse för Marketplace-avbildningen.|
|nodeAgentSKUId|Sträng|hello SKU hello Batch nod agent har etablerats på hello compute-nod.|
|[windowsConfiguration](#bk_winconf)|Komplex typ|Anger inställningar för Windows-operativsystemet på hello virtuella datorn. Den här egenskapen får inte anges om hello imageReference refererar till en Linux OS-avbildningen.|

###  <a name="bk_imgref"></a>imageReference

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|Publisher|Sträng|hello utgivaren av hello avbildningen.|
|Erbjudande|Sträng|hello-erbjudande för hello bild.|
|SKU|Sträng|hello SKU hello avbildningen.|
|Version|Sträng|hello-versionen av hello image.|

###  <a name="bk_winconf"></a>windowsConfiguration

|Elementnamn|Typ|Anteckningar|
|------------------|----------|-----------|
|enableAutomaticUpdates|Booleskt värde|Anger om hello virtuell dator är aktiverat för automatiska uppdateringar. Om den här egenskapen inte anges är hello standardvärdet true.|

###  <a name="bk_netconf"></a>networkConfiguration

|Elementnamn|Typ|Anteckningar|
|------------------|--------------|----------|
|subnetId|Sträng|Anger hello Resursidentifieraren hello undernätet vilka hello poolen beräkning noder har skapats.|
