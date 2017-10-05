---
title: "Samla in loggar med hjälp av Azure-diagnostik | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in Azure-diagnostik för att samla in loggar från ett Service Fabric-kluster som körs i Azure."
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
ms.openlocfilehash: 190a8a393f2e7d74a792db4efa81f94a18b02221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Samla in loggar med Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

När du kör ett Azure Service Fabric-kluster, är det en bra idé att samla in loggar från alla noder i en central plats. Med loggarna på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i program och tjänster som körs i klustret.

Ett sätt att överföra och samla in loggar är att använda Azure-diagnostik-tillägget, som överför loggar till Azure Storage eller Azure Application Insights Händelsehubbar i Azure. Loggarna är inte så användbart direkt i lagring eller i Händelsehubbar. Men du kan använda en extern process för att läsa händelser från lagring och placera dem i en produkt som [logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) levereras med en omfattande sökning och analyser Loggtjänsten inbyggda.

## <a name="prerequisites"></a>Krav
Du kan använda dessa verktyg för att utföra vissa åtgärder i det här dokumentet:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (rör Azure Cloud Services men har bra information och exempel)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-klienten](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-sources-that-you-might-want-to-collect"></a>Loggen källor som du kanske vill samla in
* **Service Fabric loggar**: orsakat från plattform till standard ETW Event Tracing for Windows () och EventSource kanaler. Loggar kan vara något av flera olika typer:
  * Operativa händelser: loggar för åtgärder som utförs av Service Fabric-plattformen. Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering.
  * [Reliable Actors programmering modellen händelser](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programming modellen händelser](service-fabric-reliable-services-diagnostics.md)
* **Programhändelser**: händelser orsakat från din tjänst kod och skrivs med hjälp av EventSource hjälpklass i Visual Studio-mallar. Mer information om hur du skriver loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Distribuera diagnostik-tillägget
Det första steget i att samla in loggar är att distribuera diagnostik tillägg på var och en av de virtuella datorerna i Service Fabric-klustret. Diagnostik-tillägget samlar in loggar på varje virtuell dator och överför dem till lagringskontot som du anger. Stegen variera något beroende på om du använder Azure-portalen eller Azure Resource Manager. Stegen variera beroende på om distributionen är en del av klustret har skapats eller är i ett kluster som redan finns. Nu ska vi titta på stegen för varje scenario.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-the-portal"></a>Distribuera diagnostik-tillägget som en del av klustret skapas via portalen
Om du vill distribuera diagnostik-tillägg till de virtuella datorerna i klustret som en del av klustret har skapats, kan du använda panelen diagnostik inställningar visas i följande bild. Om du vill aktivera insamling av Reliable Actors eller Reliable Services, kontrollera att diagnostik är **på** (standardinställningen). När du skapar klustret kan du inte ändra dessa inställningar med hjälp av portalen.

![Azure diagnostikinställningar för klustret skapas i portalen](./media/service-fabric-diagnostics-how-to-setup-wad/portal-cluster-creation-diagnostics-setting.png)

Azure supportgruppen *kräver* stöder loggar för att lösa eventuella supportförfrågningar som du skapar. De här loggarna har samlats in i realtid och lagras i något av de lagringskonton som skapats i resursgruppen. Inställningarna för Webbprogramdiagnostik konfigurera händelser på programnivå. Här ingår [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) händelser, [Reliable Services](service-fabric-reliable-services-diagnostics.md) händelser och vissa på systemnivå Service Fabric-händelser som ska lagras i Azure Storage.

Produkter som [Elasticsearch](https://www.elastic.co/guide/index.html) eller en egen process kan hämta händelser från lagringskontot. Det finns för närvarande inget sätt att filtrera eller rensa de händelser som skickas till tabellen. Om du inte implementerar en process för att ta bort händelser från tabellen, i tabellen kommer att fortsätta att växa.

När du skapar ett kluster med hjälp av portalen, vi rekommenderar starkt att du hämtar mallen **innan du klickar på OK** att skapa klustret. Mer information finns i [ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md). Du behöver mallen du vill göra ändringarna senare, eftersom du inte kan göra några ändringar med hjälp av portalen.

Du kan exportera mallar från portalen med hjälp av följande steg. Dessa mallar kan dock vara svårare att använda eftersom de kan ha null-värden som saknar nödvändig information.

1. Öppna din resursgrupp.
2. Välj **inställningar** Visa inställningar.
3. Välj **distributioner** Visa historik distribution.
4. Välj en distribution för att visa information om distributionen.
5. Välj **Exportmallen** visa mallen.
6. Välj **spara till fil** att exportera en .zip-fil som innehåller mallen, parameter och PowerShell-filer.

När du exporterar filerna som du behöver göra en ändring. Redigera filen parameters.JSON och ta bort den **adminPassword** element. Detta innebär att en prompt för lösenordet när du kör skriptet för distribution. När du kör skriptet för distribution, kanske du måste åtgärda null-värden.

Använda nedladdade mallen för att uppdatera en konfiguration:

1. Extrahera innehållet till en mapp på den lokala datorn.
2. Ändra innehållet för att återspegla den nya konfigurationen.
3. Starta PowerShell och ändra till den mapp där du extraherade innehållet.
4. Kör **deploy.ps1** och Fyll i prenumerations-ID, resursgruppens namn (Använd samma namn för att uppdatera konfigurationen av) och en unik distributionsnamnet.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Distribuera diagnostik-tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager
Om du vill skapa ett kluster med hjälp av hanteraren för filserverresurser, som du behöver lägga till diagnostik konfigurationen JSON till hela klustret Resource Manager-mallen innan du skapar klustret. Vi ger en fem-VM klustret Resource Manager exempelmall diagnostik-konfiguration som lagts till som en del av våra exempel för Resource Manager-mall. Du kan se den på den här platsen i galleriet Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype).

Öppna filen azuredeploy.json för att se inställningen diagnostik i Resource Manager-mallen, och Sök efter **IaaSDiagnostics**. Om du vill skapa ett kluster med hjälp av den här mallen, Välj den **till Azure** knappen tillgänglig vid föregående länk.

Du kan hämta Resource Manager-exempel, göra ändringar, och skapa ett kluster med den ändrade mallen med hjälp av den `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster. Se följande kod för de parametrar som du anger för att kommandot. Detaljerad information om hur du distribuerar en resursgrupp med hjälp av PowerShell finns i artikeln [distribuera en resursgrupp med Azure Resource Manager-mallen](../azure-resource-manager/resource-group-template-deploy.md).

```powershell

New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -Name $deploymentName -TemplateFile $pathToARMConfigJsonFile -TemplateParameterFile $pathToParameterFile –Verbose
```

### <a name="deploy-the-diagnostics-extension-to-an-existing-cluster"></a>Distribuera tillägget diagnostik till ett befintligt kluster
Om du har ett befintligt kluster som inte har distribuerats diagnostik eller om du vill ändra en befintlig konfiguration, kan du lägga till eller uppdatera det. Ändra Resource Manager-mallen som används för att skapa det befintliga klustret eller hämta mallen från portalen, enligt beskrivningen ovan. Ändra filen template.json genom att utföra följande uppgifter.

Lägga till en ny storage-resurs i mallen genom att lägga till avsnittet resurser.

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

 Lägg till avsnittet Parametrar precis efter storage-konto definitioner, mellan `supportLogStorageAccountName` och `vmNodeType0Name`. Ersätt platshållartexten *lagringskontonamnet här* med namnet på lagringskontot.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "storage account name goes here",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Sedan uppdatera den `VirtualMachineProfile` i template.json-filen genom att lägga till följande kod i en matris för tillägg. Glöm inte att lägga till ett kommatecken i början eller slutet, beroende på om det har infogats.

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

När du har ändrat filen template.json enligt publicera Resource Manager-mallen. Om mallen exporterades, publicerar körs filen deploy.ps1 mallen. När du har distribuerat, se till att **ProvisioningState** är **lyckades**.

## <a name="update-diagnostics-to-collect-health-and-load-events"></a>Uppdatera diagnostik för att samla in hälsa och läsa in händelser

Från och med 5.4 för Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen. Dessa händelser återspeglar händelser som genererats av systemet eller din kod med hjälp av hälsotillstånd eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning. Visa dessa händelser i Loggboken för Visual Studio diagnostiska lägga till ”Microsoft-ServiceFabric:4:0x4000000000000008” i listan över ETW-providers.

Ändra resource manager-mall att inkludera för att samla in händelser

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

## <a name="update-diagnostics-to-collect-and-upload-logs-from-new-eventsource-channels"></a>Uppdatera diagnostik för att samla in och ladda upp loggar från den nya EventSource kanaler
Om du vill uppdatera diagnostik för att samla in loggar från nya EventSource kanaler som representerar ett nytt program som du håller på att distribuera utför du samma steg som i den [föregående avsnitt](#deploywadarm) för inställning av diagnostik för ett befintligt kluster.

Uppdatera den `EtwEventSourceProviderConfiguration` -avsnittet i template.json att lägga till poster för de nya EventSource kanalerna innan du använder konfigurationen uppdatera med hjälp av den `New-AzureRmResourceGroupDeployment` PowerShell-kommando. Namnet på händelsekällan definieras som en del av din kod i Visual Studio-genererade ServiceEventSource.cs-filen.

Till exempel om din händelsekällan är Mina Eventsource, lägger du till följande kod för att placera händelser från Mina Eventsource i en tabell med namnet MyDestinationTableName.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Om du vill samla in prestandaräknare eller händelseloggar, ändra Resource Manager-mallen med hjälp av exemplen i [skapa en virtuell Windows-dator med övervakning och diagnostik med en Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Kubfilen Resource Manager-mallen.

## <a name="next-steps"></a>Nästa steg
För att förstå vilka händelser som du ska leta efter vid felsökning av problem i detalj, se diagnostiska händelserna orsakat för [Reliable Actors](service-fabric-reliable-actors-diagnostics.md) och [Reliable Services](service-fabric-reliable-services-diagnostics.md).

## <a name="related-articles"></a>Relaterade artiklar
* [Lär dig att samla in prestandaräknare eller loggar med hjälp av diagnostik-tillägget](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Service Fabric-lösning i logganalys](../log-analytics/log-analytics-service-fabric.md)

