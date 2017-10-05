---
title: "Azure Service Fabric händelse aggregeringen med Linux Azure-diagnostik | Microsoft Docs"
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
ms.openlocfilehash: bcc3a229369a065cfcfbd32eadbf3f6ae6fe0036
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Aggregering av händelse och med Azure-diagnostik för Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

När du kör ett Azure Service Fabric-kluster, är det en bra idé att samla in loggar från alla noder i en central plats. Med loggarna på en central plats hjälper dig att analysera och felsöka problem i klustret eller problem i program och tjänster som körs i klustret.

Ett sätt att överföra och samla in loggar är att använda Linux Azure Diagnostics (LAD)-tillägget, som överför loggar till Azure Storage och har även möjlighet att skicka loggar till Azure Application Insights eller Händelsehubbar. Du kan också använda en extern process för att läsa händelser från lagring och placera dem i en analys plattform produkt som [OMS logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen.

## <a name="log-and-event-sources"></a>Loggen och händelsen källor

### <a name="service-fabric-platform-events"></a>Service Fabric-plattformen händelser
Service Fabric avger några out box loggar via [LTTng](http://lttng.org), inklusive operativa händelser eller runtime-händelser. Dessa loggar lagras på den plats som anger klustrets Resource Manager-mall. Om du vill hämta eller ange information om lagringskonto, söka efter taggen **AzureTableWinFabETWQueryable** och leta efter **StoreConnectionString**.

### <a name="application-events"></a>Programhändelser
 Händelser som orsakat från program och tjänsterna kod som anges av du när instrumentering din programvara. Du kan använda alla loggning lösningar som skriver textbaserade loggfiler – till exempel LTTng. Mer information finns i dokumentationen för LTTng på spårning i tillämpningsprogrammet.

[Övervaka och diagnostisera tjänster i en inställning för lokal dator development](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-the-diagnostics-extension"></a>Distribuera diagnostik-tillägget
Det första steget i att samla in loggar är att distribuera diagnostik tillägg på var och en av de virtuella datorerna i Service Fabric-klustret. Diagnostik-tillägget samlar in loggar på varje virtuell dator och överför dem till lagringskontot som du anger. Stegen variera beroende på om du använder Azure-portalen eller Azure Resource Manager.

Om du vill distribuera diagnostik-tillägg till de virtuella datorerna i klustret som en del av klustret har skapats, ange **diagnostik** till **på**. När du skapar klustret kan du inte ändra den här inställningen med hjälp av portalen.

Konfigurera sedan Linux Azure Diagnostics (LAD) för att samla in filer och placera dem i ditt lagringskonto. Den här processen beskrivs som scenario 3 (”överföra din egen loggfiler”) i artikeln [med LAD att övervaka och diagnostisera virtuella Linux-datorer](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Efter den här processen får du tillgång till spåren. Du kan överföra spåren till en visualizer du väljer.

Du kan också distribuera diagnostik-tillägget med hjälp av Azure Resource Manager. Processen påminner för Windows och Linux- och dokumenteras för Windows-kluster i [hur du samlar in loggar med Azure-diagnostik](service-fabric-diagnostics-how-to-setup-wad.md).

Du kan också använda Operations Management Suite som beskrivs i [Operations Management Suite logganalys med Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

När du är klar med den här konfigurationen övervakar LAD agenten angivna loggfilerna. När en ny rad läggs till filen, skapas en syslog-post som skickas till den lagringsenhet som du angav.

## <a name="next-steps"></a>Nästa steg

För att förstå vilka händelser som du bör undersöka vid felsökning av problem i detalj, se [LTTng dokumentationen](http://lttng.org/docs) och [med LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).