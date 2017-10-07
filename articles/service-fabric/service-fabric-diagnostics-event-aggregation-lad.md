---
title: "aaaAzure Service Fabric händelse aggregeringen med Azure-diagnostik för Linux | Microsoft Docs"
description: "Läs mer om sammanställa och samlar in händelser med hjälp av LAD för övervakning och diagnostik av Azure Service Fabric-kluster."
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
ms.openlocfilehash: aefa869219a0dd219e01e6574816fe3ce47fe472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Aggregering av händelse och med Azure-diagnostik för Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

När du kör ett Azure Service Fabric-kluster, är det en bra idé toocollect hello loggar från alla hello noder på en central plats. Med hello loggar på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i hello program och tjänster som körs i klustret.

Ett sätt tooupload och samla in loggar är toouse hello Linux Azure Diagnostics (LAD)-tillägg, som överför loggar tooAzure lagring och har dessutom hello alternativet toosend loggar tooAzure Application Insights eller Händelsehubbar. Du kan också använda en extern process tooread hello händelser från lagring och placerar dem på en analysis plattform produkt som [OMS logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.

## <a name="log-and-event-sources"></a>Loggen och händelsen källor

### <a name="service-fabric-platform-events"></a>Service Fabric-plattformen händelser
Service Fabric avger några out box loggar via [LTTng](http://lttng.org), inklusive operativa händelser eller runtime-händelser. Dessa loggar lagras i hello-plats som hello klustrets Resource Manager-mall anger. tooget eller ange hello lagringskontouppgifter, söka efter hello taggen **AzureTableWinFabETWQueryable** och leta efter **StoreConnectionString**.

### <a name="application-events"></a>Programhändelser
 Händelser som orsakat från program och tjänsterna kod som anges av du när instrumentering din programvara. Du kan använda alla loggning lösningar som skriver textbaserade loggfiler – till exempel LTTng. Mer information finns i dokumentationen för hello LTTng på spårning i tillämpningsprogrammet.

[Övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Distribuera hello diagnostik tillägg
hello första steget i att samla in loggar är toodeploy hello diagnostik tillägg på varje hello virtuella datorer i hello Service Fabric-klustret. hello diagnostik tillägget samlar in loggar på varje virtuell dator och överför dem toohello storage-konto som du anger. hello stegen variera beroende på om du använder hello Azure-portalen eller Azure Resource Manager.

Ange toodeploy hello diagnostik tillägget toohello virtuella datorer i hello klustret som en del av klustret skapas **diagnostik** för**på**. När du har skapat hello klustret kan du inte ändra den här inställningen genom att använda hello-portalen.

Konfigurera Linux Azure Diagnostics (LAD) toocollect hello filer och placera dem i ditt lagringskonto. Den här processen beskrivs som scenario 3 (”överföra din egen loggfiler”) i hello artikeln [med LAD toomonitor och diagnostisera virtuella Linux-datorer](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Efter den här processen hämtar du komma åt toohello spårar. Du kan överföra hello spårningar tooa visualizer du väljer.

Du kan också distribuera hello diagnostik tillägget med hjälp av Azure Resource Manager. hello processen påminner för Windows och Linux- och dokumenteras för Windows-kluster i [hur toocollect loggar med Azure-diagnostik](service-fabric-diagnostics-how-to-setup-wad.md).

Du kan också använda Operations Management Suite som beskrivs i [Operations Management Suite logganalys med Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

När du är klar med den här konfigurationen hello hello LAD agenten övervakar angivna loggfiler. När en ny rad är tillagda toohello fil, skapas en syslog-transaktion som skickade toohello lagring som du angav.

## <a name="next-steps"></a>Nästa steg

toounderstand i detalj vilka händelser som du bör undersöka vid felsökning av problem, se [LTTng dokumentationen](http://lttng.org/docs) och [med LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
