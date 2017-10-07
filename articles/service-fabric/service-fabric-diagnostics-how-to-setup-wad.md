---
title: "aaaCollect loggar med hjälp av Azure-diagnostik | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset in Azure-diagnostik toocollect loggar från ett Service Fabric-kluster som körs i Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 9f7e1fa5-6543-4efd-b53f-39510f18df56
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: afbcfbe972b1847ef33bf0539b4398794b1bd56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Samla in loggar med Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

När du kör ett Azure Service Fabric-kluster, är det en bra idé toocollect hello loggar från alla hello noder på en central plats. Med hello loggar på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i hello program och tjänster som körs i klustret.

Ett sätt tooupload och samla in loggar är toouse hello Azure-diagnostik-tillägg, vilka överföringar loggar tooAzure lagring, Azure Application Insights eller Händelsehubbar i Azure. hello loggar är inte så användbart direkt i lagring eller i Händelsehubbar. Men du kan använda en extern process tooread hello händelser från lagring och placera dem i en produkt som [logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) levereras med en omfattande sökning och analyser Loggtjänsten inbyggda.

## <a name="prerequisites"></a>Krav
Du kan använda dessa verktyg tooperform vissa hello åtgärder i det här dokumentet:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (relaterade tooAzure molntjänster men har bra information och exempel)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-klienten](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-toocollect"></a>Logga källor som du kanske vill toocollect
* **Service Fabric loggar**: orsakat från hello plattform toostandard ETW Event Tracing for Windows () och EventSource kanaler. Loggar kan vara något av flera olika typer:
  * Operativa händelser: loggar för åtgärder som hello Service Fabric-plattformen utför. Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering.
  * [Reliable Actors programmering modellen händelser](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programming modellen händelser](service-fabric-reliable-services-diagnostics.md)
* **Programhändelser**: händelser orsakat från din tjänst kod och skrivits hello EventSource hjälpklass i hello Visual Studio mallar. Mer information om hur toowrite loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Distribuera hello diagnostik tillägg
hello första steget i att samla in loggar är toodeploy hello diagnostik tillägg på varje hello virtuella datorer i hello Service Fabric-klustret. hello diagnostik tillägget samlar in loggar på varje virtuell dator och överför dem toohello storage-konto som du anger. hello steg variera något beroende på om du använder hello Azure-portalen eller Azure Resource Manager. hello stegen variera beroende på om hello distribution är en del av klustret har skapats eller är i ett kluster som redan finns. Nu ska vi titta på hello steg för varje scenario.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-hello-portal"></a>Distribuera hello diagnostik tillägget som en del av klustret har skapats via hello portal
toodeploy hello diagnostik tillägget toohello virtuella datorer i hello klustret som en del av klustret skapas, Använd hello diagnostik inställningar panelen visas i följande bild hello. tooenable Reliable Actors eller Reliable Services händelseinsamling, kontrollera att diagnostik är inställd för**på** (hello standardinställningen). När du har skapat hello klustret kan du inte ändra dessa inställningar med hjälp av hello-portalen.

![Azure diagnostikinställningar i hello-portalen för att skapa klustret](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

hello Azure-teamet *kräver* stöd loggar toohelp lösa eventuella supportförfrågningar som du skapar. De här loggarna har samlats in i realtid och lagras i en hello lagringskonton som skapats i hello resursgruppen. hello diagnostikinställningarna konfigurera händelser på programnivå. Här ingår [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) händelser, [Reliable Services](service-fabric-reliable-services-diagnostics.md) händelser och vissa på systemnivå Service Fabric händelser toobe lagras i Azure Storage.

Produkter som [Elasticsearch](https://www.elastic.co/guide/index.html) eller en egen process kan hämta hello händelser från hello storage-konto. Det finns för närvarande inga sätt toofilter eller rensa hello händelser som skickas toohello tabell. Om du inte implementerar en process tooremove händelser från hello tabellen fortsätter hello tabell toogrow.

När du skapar ett kluster med hjälp av hello portal, vi rekommenderar starkt att du hämtar hello mall **innan du klickar på OK** toocreate hello klustret. Mer information finns för[ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md). Du behöver hello malländringar toomake senare, eftersom du inte kan göra några ändringar med hjälp av hello-portalen.

Du kan exportera mallar från hello-portalen med hjälp av hello följande steg. Dessa mallar kan dock vara svårare toouse eftersom de kan ha null-värden som saknar nödvändig information.

1. Öppna din resursgrupp.
2. Välj **inställningar** toodisplay hello Inställningar Kontrollpanelen.
3. Välj **distributioner** toodisplay hello distribution historikpanelen.
4. Välj en toodisplay hello distributionsinformation för hello-distribution.
5. Välj **Exportmallen** toodisplay hello mallen panelen.
6. Välj **spara toofile** tooexport en .zip-fil som innehåller hello mallen, parameter och PowerShell-filer.

När du exporterar hello filer måste toomake en ändring. Redigera hello parameters.json filen och ta bort hello **adminPassword** element. Detta innebär att en fråga efter hello lösenord när hello-skriptet körs. När du kör hello distributionsskriptet, kanske toofix null-värden.

toouse hello ned mallen tooupdate en konfiguration:

1. Extrahera hello innehållet tooa mapp på den lokala datorn.
2. Ändra hello innehållet tooreflect hello ny konfiguration.
3. Starta PowerShell och ändra toohello mapp där du extraherade hello innehåll.
4. Kör **deploy.ps1** och Fyll i hello prenumerations-ID, hello resursgruppens namn (Använd hello samma tooupdate hello konfiguration), och en unik distributionsnamnet.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Distribuera hello diagnostik tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager
toocreate ett kluster med hjälp av hanteraren för filserverresurser måste tooadd hello diagnostik configuration JSON toohello fullständig klustret Resource Manager-mallen innan du skapar hello kluster. Vi tillhandahåller en fem-VM klustret Resource Manager exempelmall med diagnostik konfiguration läggs tooit som en del av våra exempel för Resource Manager-mall. Du kan se den på den här platsen i galleriet för hello Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).

toosee hello diagnostik inställningen i hello Resource Manager-mall, öppna hello azuredeploy.json filen och Sök efter **IaaSDiagnostics**. toocreate ett kluster med hjälp av den här mallen, Välj hello **distribuera tooAzure** knappen på hello föregående länk.

Du kan också du kan hämta hello Resource Manager exempel, göra ändringar tooit och skapa ett kluster med hello ändrade mallen med hjälp av hello `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster. Se följande kod för hello-parametrar som du skickar i toohello kommandot hello. Detaljerad information om hur toodeploy en resurs gruppen med hjälp av PowerShell finns i artikeln hello [distribuera en resursgrupp med hello Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-hello-diagnostics-extension-tooan-existing-cluster"></a>Distribuera hello diagnostik tillägget tooan befintligt kluster
Om du har ett befintligt kluster som inte har distribuerats diagnostik, eller om du vill toomodify en befintlig konfiguration, kan du lägga till eller uppdatera det. Ändra hello Resource Manager-mall som används toocreate hello befintligt kluster eller hämta hello mallen från hello portal som tidigare beskrivits. Ändra hello template.json filen genom att utföra följande uppgifter hello.

Lägga till en ny mall för storage resource toohello genom att lägga till toohello resurser avsnitt.

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
  "location": "[parameters('computeLocation')]",
  "properties": {
    "accountType": "[parameters('applicationDiagnosticsStorageAccountType')]"
  },
  "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Lägg sedan till avsnittet toohello parametrar precis efter hello storage-konto definitioner mellan `supportLogStorageAccountName` och `vmNodeType0Name`. Ersätt hello platshållartexten *lagringskontonamnet här* med hello namnet hello storage-konto.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for hello application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for hello storage account that contains application diagnostics data from hello cluster"
      }
    },
```
Sedan uppdatera hello `VirtualMachineProfile` i hello template.json filen genom att lägga till följande kod i hello tillägg matris hello. Vara säker på att tooadd kommatecken i hello början eller slutet av hello, beroende på om det har infogats.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

När du ändrar hello template.json enligt kan publicera hello Resource Manager-mall. Om hello mallen exporterades publicerar kör hello deploy.ps1 filen hello mallen. När du har distribuerat, se till att **ProvisioningState** är **lyckades**.

## <a name="update-diagnostics-toocollect-health-and-load-events"></a>Uppdatera diagnostik toocollect hälsa och Läs in händelser

Från och med hello 5.4 versionen av Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen. Dessa händelser återspeglar händelser som genererats av systemet hello eller din kod med hjälp av hello hälsa eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning. tooview lägga till dessa händelser i Loggboken i Visual Studio-diagnostik ”Microsoft-ServiceFabric:4:0x4000000000000008” toohello lista över ETW-providers.

toocollect hello händelser, ändra hello resource manager-mall tooinclude

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387912",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

## <a name="update-diagnostics-toocollect-and-upload-logs-from-new-eventsource-channels"></a>Uppdatera diagnostik toocollect och ladda upp loggar från den nya EventSource kanaler
tooupdate diagnostik toocollect loggar från nya EventSource kanaler som representerar ett nytt program som du är om toodeploy, utför samma steg som hello hello [föregående avsnitt](#deploywadarm) för inställning av diagnostik för en befintlig kluster.

Uppdatera hello `EtwEventSourceProviderConfiguration` under hello template.json tooadd poster för hello nya EventSource kanaler innan du använder hello konfigurationen uppdatera med hjälp av hello `New-AzureRmResourceGroupDeployment` PowerShell-kommando. hello händelsekälla hello namn har definierats som en del av din kod i hello Visual Studio-genererade ServiceEventSource.cs-filen.

T.ex, om din händelsekälla heter Mina Eventsource, lägger du till följande kod tooplace hello händelser från Mina Eventsource i en tabell med namnet MyDestinationTableName hello.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

toocollect prestandaräknare eller händelseloggar, ändra hello Resource Manager-mall med hjälp av hello-exempel finns i [skapa en virtuell Windows-dator med övervakning och diagnostik med en Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Kubfilen hello Resource Manager-mall.

## <a name="next-steps"></a>Nästa steg
toounderstand i detalj vilka händelser som du ska leta efter vid felsökning, se hello diagnostiska händelser som orsakat för [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) och [Reliable Services](service-fabric-reliable-services-diagnostics.md).

## <a name="related-articles"></a>Relaterade artiklar
* [Lär dig hur toocollect prestandaräknare eller loggar med hjälp av hello diagnostik tillägg](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Service Fabric-lösning i logganalys](../log-analytics/log-analytics-service-fabric.md)

