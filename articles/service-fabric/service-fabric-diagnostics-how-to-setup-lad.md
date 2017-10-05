---
title: "Samla in loggar med hjälp av Azure-diagnostik för Linux | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in Azure-diagnostik för att samla in loggar från ett Service Fabric Linux-kluster som körs i Azure."
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
ms.openlocfilehash: 3e41caaeb38c55d1c6c3bfdce2f81c86b4aff4d0
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

När du kör ett Azure Service Fabric-kluster, är det en bra idé att samla in loggar från alla noder i en central plats. Med loggarna på en central plats gör det enkelt att analysera och felsöka problem, oavsett om de är i dina tjänster, programmet eller själva klustret. Ett sätt att överföra och samla in loggar är att använda Azure-diagnostik-tillägget, som överför loggar till Azure Storage, Azure Application Insights eller Händelsehubbar i Azure. Du kan också läsa händelser från lagringsenheter eller Event Hubs och placera dem i en produkt som [logganalys](../log-analytics/log-analytics-service-fabric.md) eller en annan lösning för parsning av loggen. [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) levereras med en omfattande sökning och analyser Loggtjänsten inbyggda.

## <a name="log-sources-that-you-might-want-to-collect"></a>Loggen källor som du kanske vill samla in
* **Service Fabric loggar**: orsakat från plattformen via [LTTng](http://lttng.org) och överförs till ditt lagringskonto. Loggar kan vara operativa händelser eller runtime-händelser som genererar plattformen. Dessa loggar lagras på den plats som anger klustermanifestet. (För att få information om lagringskonto, söka efter taggen **AzureTableWinFabETWQueryable** och leta efter **StoreConnectionString**.)
* **Programhändelser**: orsakat från din tjänst kod. Du kan använda alla loggning lösningar som skriver textbaserade loggfiler – till exempel LTTng. Mer information finns i dokumentationen för LTTng på spårning i tillämpningsprogrammet.  

## <a name="deploy-the-diagnostics-extension"></a>Distribuera diagnostik-tillägget
Det första steget i att samla in loggar är att distribuera diagnostik tillägg på var och en av de virtuella datorerna i Service Fabric-klustret. Diagnostik-tillägget samlar in loggar på varje virtuell dator och överför dem till lagringskontot som du anger. Stegen variera beroende på om du använder Azure-portalen eller Azure Resource Manager.

Om du vill distribuera diagnostik-tillägg till de virtuella datorerna i klustret som en del av klustret har skapats, ange **diagnostik** till **på**. När du skapar klustret kan du inte ändra den här inställningen med hjälp av portalen.

Konfigurera sedan Linux Azure Diagnostics (LAD) för att samla in filer och placera dem i ditt lagringskonto. Den här processen beskrivs som scenario 3 (”överföra din egen loggfiler”) i artikeln [med LAD att övervaka och diagnostisera virtuella Linux-datorer](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). Efter den här processen får du tillgång till spåren. Du kan överföra spåren till en visualizer du väljer.

Du kan också distribuera diagnostik-tillägget med hjälp av Azure Resource Manager. Processen påminner för Windows och Linux- och dokumenteras för Windows-kluster i [hur du samlar in loggar med Azure-diagnostik](service-fabric-diagnostics-how-to-setup-wad.md).

Du kan också använda Operations Management Suite som beskrivs i [Operations Management Suite logganalys med Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

När du är klar med den här konfigurationen övervakar LAD agenten angivna loggfilerna. När en ny rad läggs till filen, skapas en syslog-post som skickas till den lagringsenhet som du angav.

## <a name="next-steps"></a>Nästa steg
För att förstå vilka händelser som du bör undersöka vid felsökning av problem i detalj, se [LTTng dokumentationen](http://lttng.org/docs) och [med LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

