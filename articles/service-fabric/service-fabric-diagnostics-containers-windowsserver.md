---
title: "aaaAzure Service Fabric-behållare övervakning och diagnostik | Microsoft Docs"
description: "Lär dig hur toomonitor och diagnostisera behållare med OMSS behållare lösning för samordnade på Microsoft Azure Service Fabric."
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
ms.date: 05/10/2017
ms.author: dekapur
ms.openlocfilehash: cd79111cf78b9d76a60d489bb9953587aa06186d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Övervakning av Windows Server-behållare med OMS

## <a name="oms-containers-solution"></a>OMS behållare lösning

hello Operations Management Suite (OMS) team har publicerat en behållare lösning för diagnostik- och övervakning för behållare. Den här lösningen är ett bra verktyg toomonitor behållardistributionerna styrd på Service Fabric tillsammans med deras Service Fabric-lösning. Här är ett enkelt exempel på vilka hello instrumentpanel i hello lösning ser ut:

![Grundläggande OMS-instrumentpanelen](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Samlar även in olika typer av loggar som kan efterfrågas i hello OMS logganalys verktyg och kan vara används toovisualize alla mått eller händelser som genereras. hello loggen typer som samlas in är:

1. ContainerInventory: Visar information om behållarens plats, namn och bilder
2. ContainerImageInventory: information om distribuerade bilder, inklusive ID eller storlekar
3. ContainerLog: specifika felloggar, docker-loggar (stdout osv.) och andra transaktioner
4. ContainerServiceLog: docker daemon kommandon som har körts
5. Prestanda: prestandaräknare inklusive behållaren cpu, minne, nätverkstrafik, disk-i/o och anpassade mått från hello värd för datorer

Den här artikeln beskriver hello steg krävs tooset in behållaren övervakning för klustret. Mer om OMSS behållare lösningen toolearn checka ut sina [dokumentationen](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Konfigurera ett Service Fabric-kluster

Skapa ett kluster med hello Azure Resource Manager mallen [här](https://github.com/dkkapur/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Sample). Se till att tooadd ett unikt namn för OMS-arbetsytan. Den här mallen som standard också toodeploying skapa ett kluster i hello förhandsgranskning av Service Fabric (v255.255), vilket innebär att den inte kan användas i produktion och går inte att uppgradera tooa annan Service Fabric-version. Om du väljer toouse mallen för långsiktig eller använda, ändra hello tooa stabil versionsnummer.

Bekräfta att du har installerat hello lämpligt certifikat och se till att du kan tooconnect toohello kluster när hello klustret har ställts in.

Kontrollera att resursgruppen har konfigurerats på rätt sätt med rubriken toohello [Azure-portalen](https://portal.azure.com/) och hitta hello-distribution. hello resursgruppen bör innehålla alla hello Service Fabric-resurser och har också en Log Analytics-lösning som en Service Fabric-lösning.

För att ändra ett befintligt Service Fabric-kluster:
* Bekräfta att diagnostik är aktiverat (om inte, kan du aktivera det via [uppdatering hello virtuella datorns skaluppsättning](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* Lägg till en OMS-arbetsyta genom att skapa en ”Service Fabric Analytics”-lösning via hello Azure Marketplace
* Redigera hello datakällor av hello Service Fabric-lösningen toopick in data från hello lämplig Azure Storage-tabeller (ställa in av BOMULLSTUSS) i hello resursgruppen som hello klustret finns i
* Lägg till hello-agenten som en [tillägget toohello virtuella datorns skaluppsättning](/powershell/module/azurerm.compute/add-azurermvmssextension) via PowerShell eller via uppdaterar hello skaluppsättning för virtuell dator (samma länk som ovan, toomodify hello Resource Manager-mall)

## <a name="2-deploy-a-container"></a>2. Distribuera en behållare

Distribuera en behållare tooit när hello klustret är klar och du har bekräftat att du kan komma åt den. Om du väljer toouse hello förhandsversion som angetts av hello mallen kan du även utforska Service Fabric nya docker compose funktioner. Bära på att hello första gången en avbildning av behållare är distribuerade tooa klustret, det tar flera minuter toodownload hello avbildningen beroende på storleken.

## <a name="3-add-hello-containers-solution"></a>3. Lägg till hello behållare lösning

I hello Azure portal, skapa en behållare resurs (under hello övervakning + Management kategori) via Azure Marketplace. 

![Lägga till behållare lösning](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

I steget Skapa hello begär en OMS-arbetsyta. Välj hello ett som har skapats med hello distribution ovan. Det här steget lägger till en behållare lösning OMS-arbetsyta och identifieras automatiskt av hello OMS-agent distribueras med hello mallen. hello agenten börjar samla in data på hello behållare processer i kluster hello och i 10 – 15 minuter bör du se hello lösning lysa med data som hello bild av hello instrumentpanelen ovan.

## <a name="4-next-steps"></a>4. Nästa steg

OMS erbjuder olika verktyg i hello arbetsytan toomake om det är mer användbar för dig. Utforska hello följande alternativ toocustomize hello lösning tooyour behov:
- Hämta bekantat med hello [logga sökning och hämtning av](../log-analytics/log-analytics-log-searches.md) funktioner som erbjuds som en del av logganalys
- Konfigurera hello OMS-agenten toopick in specifika prestandaräknare (gå toohello arbetsytan Hem > Inställningar > Data > Windows prestandaräknare)
- Konfigurera OMS tooset in [automatiserade aviseringar](../log-analytics/log-analytics-alerts.md) regler tooaid i identifiering och diagnostik
