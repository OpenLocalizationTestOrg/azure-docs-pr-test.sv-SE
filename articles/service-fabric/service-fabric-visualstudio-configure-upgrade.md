---
title: aaaConfigure hello uppgradering av ett Service Fabric-program | Microsoft Docs
description: "Lär dig hur tooconfigure hello inställningar för att uppgradera ett Service Fabric-program med hjälp av Microsoft Visual Studio."
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 8ca50aa9d911f3c98f017490c8fe29011e8d80cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfigurera hello uppgradering av ett Service Fabric-program i Visual Studio
Visual Studio tools för Azure Service Fabric stödja uppgradera publicerar toolocal eller fjärr-kluster. Det finns tre scenarier där du vill tooupgrade dina program tooa nyare version i stället för att ersätta hello program under testning och felsökning:

* Programdata går inte förlorade under hello uppgraderingen.
* Tillgänglighet förblir hög så att det inte går eventuella avbrott i tjänsten under hello uppgraderingen, om det finns tillräckligt med instanser av tjänsten sprids uppgraderingsdomäner.
* Du kan köra testerna mot ett program medan den uppgraderas.

## <a name="parameters-needed-tooupgrade"></a>Parametrar krävs tooupgrade
Du kan välja mellan två typer av distribution: regelbundna eller uppgradering. En vanlig distribution raderas alla tidigare distributionsinformation och data på hello kluster, medan en uppgradering distribution bevarar den. När du uppgraderar ett Service Fabric-program i Visual Studio, behöver du uppgradera parametrar för tooprovide program och kontrollera hälsoprinciper. Uppgradering av programmet parametrar för att styra hello uppgradering vid kontroll av hälsoprinciper fastställa om hello uppgraderingen lyckades. Se [uppgradering av Service Fabric-programmet: Uppgraderingsparametrar](service-fabric-application-upgrade-parameters.md) för mer information.

Det finns tre uppgradera lägen: *övervakade*, *UnmonitoredAuto*, och *UnmonitoredManual*.

* En uppgradering av övervakade automatiserar hello uppgradering och programmet hälsokontroll.
* Uppgradering UnmonitoredAuto automatiserar hello uppgraderingen, men hoppar över hello hälsokontrollen för programmet.
* När du gör en uppgradering UnmonitoredManual toomanually måste uppgradera varje domän.

Varje Uppgraderingsläge kräver olika uppsättningar med parametrar. Se [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) toolearn mer om hello tillgängliga alternativ för uppgradering.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Uppgradera ett Service Fabric-program i Visual Studio
Om du använder hello Visual Studio Service Fabric verktyg tooupgrade ett Service Fabric-program, kan du ange en publicera processen toobe en uppgradering i stället för en vanlig distribution genom att kontrollera hello **uppgradera hello program** Kontrollera ruta.

### <a name="tooconfigure-hello-upgrade-parameters"></a>uppgradera tooconfigure hello-parametrar
1. Klicka på hello **inställningar** knappen Nästa toohello kryssruta. Hej **redigera uppgradera parametrar** dialogrutan visas. Hej **redigera uppgradera parametrar** dialogrutan stöder hello övervakade och UnmonitoredAuto UnmonitoredManual uppgradera lägen.
2. Välj hello Uppgraderingsläge som du vill använda toouse och fyller sedan hello parametern rutnätet.

    Varje parameter har standardvärden. Hej valfri parameter *DefaultServiceTypeHealthPolicy* tar en hash-tabell inmatning. Här är ett exempel på hello hash-tabell Indataformatet för *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* är en annan valfri parameter som använder en hash-tabell inmatning hello följande format:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Här är en verklig exempel:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. Om du väljer UnmonitoredManual Uppgraderingsläge måste du manuellt starta toocontinue en PowerShell-konsolen och slutför hello uppgraderingsprocessen. Se för[uppgradering av Service Fabric-programmet: avancerade alternativ](service-fabric-application-upgrade-advanced.md) toolearn hur manuell uppgradering fungerar.

## <a name="upgrade-an-application-by-using-powershell"></a>Uppgradera ett program med hjälp av PowerShell
Du kan använda PowerShell-cmdlets tooupgrade ett Service Fabric-program. Se [Service Fabric uppgradera självstudien](service-fabric-application-upgrade-tutorial.md) och [Start ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) detaljerad information.

## <a name="specify-a-health-check-policy-in-hello-application-manifest-file"></a>Ange en hälsoprincip för kontrollen i hello programmanifestfilen
Varje tjänst i ett Service Fabric-program kan ha sin egen parametrar för hälsotillstånd som åsidosätter hello standardvärden. Du kan ange dessa parametervärden i hello programmanifestfilen.

hello följande exempel visas hur tooapply unikt hälsotillstånd Kontrollera princip för varje tjänst i hello programmanifestet.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Nästa steg
Mer information om hur du distribuerar ett program finns [distribuera ett befintligt program i Azure Service Fabric](service-fabric-deploy-existing-app.md).