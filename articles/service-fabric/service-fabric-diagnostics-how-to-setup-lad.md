---
title: "aaaCollect loggar med hjälp av Azure-diagnostik för Linux | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset in Azure-diagnostik toocollect loggar från ett Service Fabric Linux-kluster som körs i Azure."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a160d469-8b7d-4560-82dd-8500db34a44a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.openlocfilehash: f61172876e744ea3e361f9ae513254239d6ba27f
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

När du kör ett Azure Service Fabric-kluster, är det en bra idé toocollect hello loggar från alla hello noder på en central plats. Med hello loggar på en central plats gör det enkelt tooanalyze och felsöka problem, oavsett om de är i dina tjänster, program eller själva hello-klustret. Ett sätt tooupload och samla in loggar är toouse hello Azure-diagnostik-tillägg, vilka överföringar loggar tooAzure lagring, Azure Application Insights eller Händelsehubbar i Azure. Du kan också läsa hello händelser från lagringsenheter eller Event Hubs och placera dem i en produkt som [logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) levereras med en omfattande sökning och analyser Loggtjänsten inbyggda.

## <a name="log-sources-that-you-might-want-toocollect"></a>Logga källor som du kanske vill toocollect
* **Service Fabric loggar**: orsakat från hello plattform via [LTTng](http://lttng.org) och tooyour storage-konto som har överförts. Loggar kan vara operativa händelser eller runtime-händelser som hello plattform genererar. Dessa loggar lagras på hello plats att hello klustermanifestet anger. (tooget hello lagringskontouppgifter, söka efter hello taggen **AzureTableWinFabETWQueryable** och leta efter **StoreConnectionString**.)
* **Programhändelser**: orsakat från din tjänst kod. Du kan använda alla loggning lösningar som skriver textbaserade loggfiler – till exempel LTTng. Mer information finns i dokumentationen för hello LTTng på spårning i tillämpningsprogrammet.  

## <a name="deploy-hello-diagnostics-extension"></a>Distribuera hello diagnostik tillägg
hello första steget i att samla in loggar är toodeploy hello diagnostik tillägg på varje hello virtuella datorer i hello Service Fabric-klustret. hello diagnostik tillägget samlar in loggar på varje virtuell dator och överför dem toohello storage-konto som du anger. hello stegen variera beroende på om du använder hello Azure-portalen eller Azure Resource Manager.

Ange toodeploy hello diagnostik tillägget toohello virtuella datorer i hello klustret som en del av klustret skapas **diagnostik** för**på**. När du har skapat hello klustret kan du inte ändra den här inställningen genom att använda hello-portalen.

Konfigurera Linux Azure Diagnostics (LAD) toocollect hello filer och placera dem i ditt lagringskonto. Den här processen beskrivs som scenario 3 (”överföra din egen loggfiler”) i hello artikeln [med LAD toomonitor och diagnostisera virtuella Linux-datorer](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Efter den här processen hämtar du komma åt toohello spårar. Du kan överföra hello spårningar tooa visualizer du väljer.

Du kan också distribuera hello diagnostik tillägget med hjälp av Azure Resource Manager. hello processen påminner för Windows och Linux- och dokumenteras för Windows-kluster i [hur toocollect loggar med Azure-diagnostik](service-fabric-diagnostics-how-to-setup-wad.md).

Du kan också använda Operations Management Suite som beskrivs i [Operations Management Suite logganalys med Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

När du är klar med den här konfigurationen hello hello LAD agenten övervakar angivna loggfiler. När en ny rad är tillagda toohello fil, skapas en syslog-transaktion som skickade toohello lagring som du angav.

## <a name="next-steps"></a>Nästa steg
toounderstand i detalj vilka händelser som du bör undersöka vid felsökning av problem, se [LTTng dokumentationen](http://lttng.org/docs) och [med LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

