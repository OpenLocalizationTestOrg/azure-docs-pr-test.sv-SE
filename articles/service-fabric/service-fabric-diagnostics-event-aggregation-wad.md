---
title: "aaaAzure Service Fabric händelse aggregeringen med Windows Azure-diagnostik | Microsoft Docs"
description: "Läs mer om sammanställa och samlar in händelser med hjälp av BOMULLSTUSS för övervakning och diagnostik av Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 4827ce66620e61c5b4a8682db55952333113188a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Aggregering av händelse och med Windows Azure-diagnostik
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

När du kör ett Azure Service Fabric-kluster, är det en bra idé toocollect hello loggar från alla hello noder på en central plats. Med hello loggar på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i hello program och tjänster som körs i klustret.

Ett sätt tooupload och samla in loggar är toouse hello Windows Azure Diagnostics (BOMULLSTUSS)-tillägg, som överför loggar tooAzure lagring och har dessutom hello alternativet toosend loggar tooAzure Application Insights eller Händelsehubbar. Du kan också använda en extern process tooread hello händelser från lagring och placerar dem på en analysis plattform produkt som [OMS logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.

## <a name="prerequisites"></a>Krav
Dessa verktyg är används tooperform vissa hello åtgärder i det här dokumentet:

* [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) (relaterade tooAzure molntjänster men har bra information och exempel)
* [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager-klienten](https://github.com/projectkudu/ARMClient)
* [Azure Resource Manager-mall](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="log-and-event-sources"></a>Loggen och händelsen källor

### <a name="service-fabric-platform-events"></a>Service Fabric-plattformen händelser
Enligt beskrivningen i [i den här artikeln](service-fabric-diagnostics-event-generation-infra.md)Service Fabric ställer du in med några out box loggning kanaler, av vilka hello följande kanaler enkelt konfigurerats med BOMULLSTUSS toosend övervakning och diagnostik tooa lagring datatabell eller någon annanstans:
  * Operativa händelser: att åtgärder som hello Service Fabric-plattformen utför. Exempel: skapa program och tjänster, nod tillståndsändringar och information om uppgradering. Dessa orsakat som loggar händelsen ETW Tracing for Windows)
  * [Reliable Actors programmering modellen händelser](service-fabric-reliable-actors-diagnostics.md)
  * [Reliable Services programming modellen händelser](service-fabric-reliable-services-diagnostics.md)

### <a name="application-events"></a>Programhändelser
 Händelser som orsakat från dina program och tjänster kod och skrivits hello EventSource hjälpklass som angetts i hello Visual Studio-mallar. Mer information om hur toowrite EventSource loggar från ditt program finns [övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Distribuera hello diagnostik tillägg
hello första steget i att samla in loggar är toodeploy hello diagnostik tillägg på varje hello virtuella datorer i hello Service Fabric-klustret. hello diagnostik tillägget samlar in loggar på varje virtuell dator och överför dem toohello storage-konto som du anger. hello steg variera något beroende på om du använder hello Azure-portalen eller Azure Resource Manager. hello stegen variera beroende på om hello distribution är en del av klustret har skapats eller är i ett kluster som redan finns. Nu ska vi titta på hello steg för varje scenario.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Distribuera hello diagnostik tillägget som en del av klustret har skapats via Azure portal
toodeploy hello diagnostik tillägget toohello virtuella datorer i hello klustret som en del av klustret skapas, som du använder hello diagnostik inställningar panelens framgår hello följande bild – Kontrollera att diagnostik är inställd för**på** (hello standardinställningen) . När du har skapat hello klustret kan du inte ändra dessa inställningar med hjälp av hello-portalen.

![Azure diagnostikinställningar i hello-portalen för att skapa klustret](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics.png)

När du skapar ett kluster med hjälp av hello portal, vi rekommenderar starkt att du hämtar hello mall **innan du klickar på OK** toocreate hello klustret. Mer information finns för[ställer in ett Service Fabric-kluster med en Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md). Du behöver hello malländringar toomake senare, eftersom du inte kan göra några ändringar med hjälp av hello-portalen.

### <a name="deploy-hello-diagnostics-extension-as-part-of-cluster-creation-by-using-azure-resource-manager"></a>Distribuera hello diagnostik tillägget som en del av klustret har skapats med hjälp av Azure Resource Manager
toocreate ett kluster med hjälp av hanteraren för filserverresurser måste tooadd hello diagnostik configuration JSON toohello fullständig klustret Resource Manager-mallen innan du skapar hello kluster. Vi tillhandahåller en fem-VM klustret Resource Manager exempelmall med diagnostik konfiguration läggs tooit som en del av våra exempel för Resource Manager-mall. Du kan se den på den här platsen i galleriet för hello Azure-exempel: [kluster med fem noder med diagnostik Resource Manager mallen exempel](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype-wad).

toosee hello diagnostik inställningen i hello Resource Manager-mall, öppna hello azuredeploy.json filen och Sök efter **IaaSDiagnostics**. toocreate ett kluster med hjälp av den här mallen, Välj hello **distribuera tooAzure** knappen på hello föregående länk.

Du kan också du kan hämta hello Resource Manager exempel, göra ändringar tooit och skapa ett kluster med hello ändrade mallen med hjälp av hello `New-AzureRmResourceGroupDeployment` i Azure PowerShell-fönster. Se följande kod för hello-parametrar som du skickar i toohello kommandot hello. Detaljerad information om hur toodeploy en resurs gruppen med hjälp av PowerShell finns i artikeln hello [distribuera en resursgrupp med hello Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).

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

## <a name="collect-health-and-load-events"></a>Samla in hälsa och läsa in händelser

Från och med hello 5.4 versionen av Service Fabric, är hälsotillstånd och Läs in mått händelser tillgängliga för samlingen. Dessa händelser återspeglar händelser som genererats av systemet hello eller din kod med hjälp av hello hälsa eller läsa in reporting API: er som [ReportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) eller [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx). Detta gör för att sammanställa och visa systemhälsa över tid och aviseringar baserat på händelser hälsa eller belastningsutjämning. tooview lägga till dessa händelser i Loggboken i Visual Studio-diagnostik ”Microsoft-ServiceFabric:4:0x4000000000000008” toohello lista över ETW-providers.

toocollect hello händelser, ändra hello Resource Manager-mall tooinclude

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

## <a name="collect-reverse-proxy-events"></a>Samla in händelser för omvänd proxy

Från och med hello 5.7 versionen av Service Fabric [omvänd proxy](service-fabric-reverseproxy.md) händelser finns tillgängliga för samlingen.
Omvänd proxy skickar händelser till två kanaler, ett som innehåller fel händelser reflektion begära bearbetningsfel och hello andra ett som innehåller utförlig händelser om alla hello-begäranden som bearbetas på hello omvänd proxy. 

1. Samla in felhändelser: tooview lägga till dessa händelser i Loggboken i Visual Studio-diagnostik ”Microsoft-ServiceFabric:4:0x4000000000000010” toohello lista över ETW-providers.
toocollect hello händelser från Azure kluster, ändra hello Resource Manager-mall tooinclude

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387920",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```

2. Samla in alla begära att bearbeta händelser: I Visual Studios diagnostiska Loggboken, uppdatering hello Microsoft ServiceFabric post i hello ETW providerlistan för ”Microsoft-ServiceFabric:4:0x4000000000000020”.
Ändra hello resource manager-mall tooinclude för Azure Service Fabric-kluster

```json
  "EtwManifestProviderConfiguration": [
    {
      "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
      "scheduledTransferLogLevelFilter": "Information",
      "scheduledTransferKeywordFilter": "4611686018427387936",
      "scheduledTransferPeriod": "PT5M",
      "DefaultEvents": {
        "eventDestination": "ServiceFabricSystemEventTable"
      }
    }
```
> Det rekommenderas toojudiciously aktivera att samla in händelser från den här kanalen som detta samlar in all trafik via omvänd proxy för hello och snabbt förbruka lagringskapacitet.

I Azure Service Fabric-kluster är samlas in och aggregeras i hello SystemEventTable hello händelser från alla hello-noder.
För detaljerad felsökning av hello omvänd proxyhändelser, se hello [omvänd proxy diagnostik guiden](service-fabric-reverse-proxy-diagnostics.md).

## <a name="collect-from-new-eventsource-channels"></a>Samla in från den nya EventSource kanaler

tooupdate diagnostik toocollect loggar från nya EventSource kanaler som representerar ett nytt program som du är om toodeploy, utföra hello samma steg som tidigare angivits för hello inställning av diagnostik för ett befintligt kluster.

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

## <a name="collect-performance-counters"></a>Samla in prestandaräknare

toocollect prestandamått från ditt kluster, lägga till hello prestandaräknare tooyour ”WadCfg > DiagnosticMonitorConfiguration” i hello Resource Manager-mall för klustret. Se [prestandaräknare för Service Fabric](service-fabric-diagnostics-event-generation-perf.md) för prestandaräknare som rekommenderar vi att samla in.

Exempelvis här vi ställer in en prestandaräknare Sampla var 15: e sekund (Detta kan ändras och sätt hello format ”PT\<tid >\<enhet >”, till exempel PT3M skulle prov på tre minuters mellanrum), och överförs toohello lämplig lagringstabellen var en minut.

  ```json
  "PerformanceCounters": {
      "scheduledTransferPeriod": "PT1M",
      "PerformanceCounterConfiguration": [
          {
              "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
              "sampleRate": "PT15S",
              "unit": "Percent",
              "annotation": [
              ],
              "sinks": ""
          }
      ]
  }
  ```
  
Om du använder en Application Insights-sink enligt beskrivningen i hello nedan och vill att dessa mått tooshow i Application Insights, gör du till tooadd hello sink namn i hello ”sänkor” avsnittet som ovan. Dessutom bör du skapa en separat tabell toosend dina prestandaräknare till, så att de inte tillräckligt med utrymme i hello data från hello andra loggning kanaler som du har aktiverat.


## <a name="send-logs-tooapplication-insights"></a>Skicka loggar tooApplication insikter

Skicka övervakning och diagnostik data tooApplication insikter (AI) kan göras som en del av hello BOMULLSTUSS konfiguration. Om du väljer toouse AI för händelseanalys och visualisering, läsa [Händelseanalys och visualisering med Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) tooset upp en AI Sink som en del av din ”WadCfg”.

## <a name="next-steps"></a>Nästa steg

När du har korrekt konfigurerat Azure diagnostics, visas data i Storage-tabeller från EventSource loggar och hello ETW. Om du väljer toouse OMS gör Kibana eller några andra analyser och visualisering dataplattform som inte har konfigurerats i hello Resource Manager-mallen direkt till tooset in hello plattform på ditt val tooread i hello data från dessa lagringstabeller. Om du gör detta för OMS är relativt enkelt och förklaras i [händelse och logg analys genom OMS](service-fabric-diagnostics-event-analysis-oms.md). Application Insights är lite specialfall på detta sätt, eftersom den kan konfigureras som en del av hello diagnostik tilläggets konfiguration, så finns toohello [lämplig artikel](service-fabric-diagnostics-event-analysis-appinsights.md) om du väljer toouse AI.

>[!NOTE]
>Det finns för närvarande inga sätt toofilter eller rensa hello händelser som skickas toohello tabell. Om du inte implementerar en process tooremove händelser från hello tabellen fortsätter hello tabell toogrow. För närvarande finns ett exempel på en rensning datatjänst som körs i hello [Watchdog exempel](https://github.com/Azure-Samples/service-fabric-watchdog-service), och det rekommenderas att du skriver en själv, om det finns en bra anledning du toostore loggar utöver en tidsram för 30 eller 90 dagar.

* [Lär dig hur toocollect prestandaräknare eller loggar med hjälp av hello diagnostik tillägg](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Händelseanalys och visualisering med Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Händelseanalys och visualisering med OMS](service-fabric-diagnostics-event-analysis-oms.md)