---
title: aaaOperations Management Suite (OMS)-arkitektur | Microsoft Docs
description: "Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur.  Den här artikeln anger hello olika tjänster som ingår i OMS och innehåller länkar tootheir detaljerad innehåll."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: fa3227aa9c19219009ebe363b7fd2d6565cec59c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="oms-architecture"></a>OMS-arkitekturen
[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) är en samling molnbaserade tjänster för hantering av dina lokala och molnbaserade miljöer.  Den här artikeln beskriver hello olika lokalt och molnet komponenterna i OMS och deras datorer hög nivå molnarkitektur.  Du kan se toohello dokumentationen för varje tjänst för mer information.

## <a name="log-analytics"></a>Log Analytics
Alla data som samlas in av [logganalys](https://azure.microsoft.com/documentation/services/log-analytics/) lagras i hello OMS-databas som finns i Azure.  Anslutna källor generera data som samlas in till hello OMS-databasen.  Det finns för närvarande tre typer av anslutna datakällor som stöds.

* En agent installeras på en [Windows](../log-analytics/log-analytics-windows-agents.md) eller [Linux](../log-analytics/log-analytics-linux-agents.md) datorn direkt ansluten tooOMS.
* En hanteringsgrupp för System Center Operations Manager (SCOM) [anslutna tooLog Analytics](../log-analytics/log-analytics-om-agents.md) .  SCOM agenter fortsätta toocommunicate med hanteringsservrar som vidarebefordra händelser och prestanda data tooLog Analytics.
* Ett [Azure-lagringskontot](../log-analytics/log-analytics-azure-storage.md) som samlar in [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)-data från en arbetsroll, webbroll eller virtuell dator i Azure.

Datakällor definierar hello data som logganalys samlar in från anslutna källor, till exempel händelseloggar och prestandaräknare.  Lösningar Lägg till funktioner tooOMS och kan enkelt läggas tooyour arbetsyta från hello [OMS lösningar galleriet](../log-analytics/log-analytics-add-solutions.md).  Vissa lösningar kan kräva en direktanslutning tooLog Analytics från SCOM agenter medan andra kan kräva en ytterligare agent toobe installerad.

Logganalys har en webbaserad portal att du kan använda toomanage OMS-resurser, lägga till och konfigurera OMS-lösningar och visa och analysera data i hello OMS-databasen.

![Den övergripande Log Analytics-arkitekturen](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure Automation
[Azure Automation-runbooks](http://azure.microsoft.com/documentation/services/automation) utförs i hello Azure-molnet och kan komma åt resurser i Azure i andra molntjänster eller tillgänglig från hello offentliga Internet.  Du kan också ange lokala datorer i ditt lokala datacenter med hjälp av [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) så att runbooks kan komma åt lokala resurser.

[DSC-konfigurationer](../automation/automation-dsc-overview.md) lagras i Azure Automation kan vara direkt tooAzure virtuella datorer.  Andra fysiska och virtuella datorer kan begära konfigurationer från hello hämtningsservern för Azure Automation DSC.

Azure Automation har en OMS-lösning som visar statistik och länkar toolaunch hello Azure-portalen för alla åtgärder.

![Den övergripande Azure Automation-arkitekturen](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure Backup
Skyddade data i [Azure Backup](http://azure.microsoft.com/documentation/services/backup) lagras i ett säkerhetskopieringsvalv som finns i en viss geografisk region.  hello data replikeras inom hello samma region och, beroende på hello typ av valvet, kan också vara replikerade tooanother region för ytterligare redundans.

Azure Backup har tre grundläggande scenarier.

* Windows-dator med Azure Backup-agent.  Detta gör att du toobackup filer och mappar från Windows server eller klient direkt tooyour Azure backup-valvet.  
* System Center Data Protection Manager (DPM) eller Microsoft Azure Backup Server. Detta gör du tooleverage DPM eller Microsoft Azure Backup-Server toobackup filer och mappar dessutom tooapplication arbetsbelastningar som till exempel SQL och SharePoint toolocal lagring och sedan replikera tooyour Azure backup-valvet.
* Azure Virtual Machine-tillägg.  Detta ger dig toobackup virtuella Azure-datorer tooyour Azure backup-valvet.

Azure-säkerhetskopiering har en OMS-lösning som visar statistik och länkar toolaunch hello Azure-portalen för alla åtgärder.

![Den övergripande Azure Backup-arkitekturen](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) samordnar replikering, redundansväxling och återställning av virtuella datorer och fysiska servrar. Replikeringsdata utbyts mellan Hyper-V-värdar, VMware-hypervisorer och fysiska servrar i primära och sekundära Datacenter eller mellan hello datacenter och Azure-lagring.  Site Recovery lagrar metadata i valv som finns i en viss geografisk Azure-region. Inga replikerade data lagras av hello Site Recovery-tjänsten.

Azure Site Recovery har tre grundläggande replikeringsscenarier.

**Replikering av virtuella Hyper-V-datorer**

* Om Hyper-V-datorer hanteras i VMM-moln, kan du replikera tooa sekundära center eller tooAzure datalagring.  Replikering tooAzure är via en säker Internetanslutning.  Replikering tooa sekundärt datacenter är över hello LAN.
* Om Hyper-V-datorer inte hanteras av VMM, kan du replikera tooAzure-lagring.  Replikering tooAzure är via en säker Internetanslutning.

**Replikering av virtuella VMWare-datorer**

* Du kan replikera VMware virtuella datorer tooa sekundärt datacenter kör VMware eller tooAzure lagringsutrymme.  Replikering tooAzure kan ske via en plats-till-plats VPN- eller Azure ExpressRoute eller via en säker Internetanslutning. Replikering tooa sekundärt datacenter sker via hello InMage Scout datakanalen.

**Replikering av fysiska Windows- och Linux-servrar** 

* Du kan replikera fysiska servrar tooa sekundära datacenter eller tooAzure lagringsutrymme. Replikering tooAzure kan ske via en plats-till-plats VPN- eller Azure ExpressRoute eller via en säker Internetanslutning. Replikering tooa sekundärt datacenter sker via hello InMage Scout datakanalen.  Azure Site Recovery har en OMS-lösning som visar del statistik, men du måste använda hello Azure-portalen för alla åtgärder.

![Den övergripande Azure Site Recovery-arkitekturen](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Nästa steg
* Läs mer om [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Läs mer om [Azure Automation](https://azure.microsoft.com/documentation/services/automation).
* Läs mer om [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Läs mer om [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).

