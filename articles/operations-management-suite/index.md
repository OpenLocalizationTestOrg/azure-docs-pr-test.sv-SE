---
title: "aaaAzure Operations Management Suite (OMS)-dokumentation – självstudier | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur. Den här artikeln anger hello olika tjänster som ingår i OMS och innehåller länkar tootheir detaljerad innehåll."
services: operations-management-suite
author: carolz
manager: carolz
layout: LandingPage
ms.assetid: 
ms.service: operations-management-suite
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 01/23/2017
ms.author: carolz
ms.openlocfilehash: 11a8f5ecb3d84aed90554510fc1bb43320fdebb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Vad är Operations Management Suite (OMS)?
Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur.  Eftersom OMS implementeras som en molnbaserad tjänst kan du snabbt komma igång med minimal investering i infrastrukturtjänster.  Nya funktioner levereras automatiskt, vilket gör att du slipper löpande underhåll och uppgraderingskostnader.

Dessutom kan tooproviding värdefulla tjänster på egen hand, OMS integreras med System Center-komponenter, till exempel System Center Operations Manager tooextend management befintliga investeringar i hello moln.  System Center och OMS kan fungera tillsammans tooprovide fullständig hybridhantering upplevelse.

hello följande avsnitt ger en hög nivå beskrivning av hello annat värdeområden i OMS och hello tjänster som implementeras.  Du kan se tooOMS arkitektur för en översikt över hello olika OMS-komponenterna innan granska hello detaljerad dokumentation för varje.

## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Insikter och analys](media/operations-management-suite-overview/icon-insight-analytics.png) Insikter och analys
Med [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) kan du samla in, korrelera, söka i och agera på logg- och prestandadata som genereras av operativsystem och program. Den ger dig realtid åtgärdsinformation integrerad sökning och anpassade instrumentpaneler tooreadily analysera miljontals poster för alla dina arbetsbelastningar och servrar oavsett fysiska plats.

Du kan enkelt lägga till lösningar tooLog Analytics som definierar toobe för data som samlas in och hello logik för analys.  Lösningar kan inkludera ytterligare funktionalitet som levereras automatiskt tooagents med minimal eller ingen konfiguration.  Dessutom toousing analysverktyg som tillhandahålls av enskilda lösningar du kan utföra anpassade sökningar över hello hela datamängden i ordning toocorrelate data mellan system och program.  

## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatisering och kontroll](media/operations-management-suite-overview/icon-automation-control.png) Automatisering och kontroll
Azure Automation automatiserar administrativa processer med [runbooks](../automation/automation-runbook-types.md) som baseras på PowerShell och kör i hello Azure-molnet.  Runbooks kan komma åt alla produkter eller tjänster som kan hanteras med PowerShell, inklusive resurser i andra moln, till exempel Amazon Web Services (AWS).  Runbooks kan också köras på en server i dina lokala data center toomanage lokala resurser.

Azure Automation tillhandahåller konfigurationshantering med [PowerShell DSC](../automation/automation-dsc-overview.md).  Du kan skapa och hantera DSC-resurser som finns i Azure och tillämpa dem toocloud och lokalt system toodefine och automatiskt tillämpa konfigurationen.

## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Skydd och återställning](media/operations-management-suite-overview/icon-protection-recovery.png) Skydd och haveriberedskap
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) skyddar dina programdata och behåller dem i flera år utan stora investeringar och med minimala driftskostnader.  Det kan säkerhetskopiera data från fysiska och virtuella Windows-servrar i tillägg tooapplication arbetsbelastningar som till exempel SQL Server och SharePoint.  Det kan även användas av System Center Data Protection Manager (DPM) tooreplicate skyddade data tooAzure för redundans och långtidslagring.

[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) bidrar tooyour kontinuitet för företag och ett haveriberedskap (BCDR) genom att samordna replikering, redundans och återställning av lokala Hyper-V virtuella datorer, virtuella VMware-datorer och fysiska Windows / Linux-servrar. Du kan replikera datorer tooa sekundära Datacenter eller utöka ditt datacenter genom att replikera dem tooAzure. Site Recovery tillhandahåller också enkel redundansväxling och återställning för arbetsbelastningar. Funktionen kan integreras med mekanismer för haveriberedskap som SQL Server AlwaysOn och tillhandahåller återställningsplaner för enkel redundansväxling av arbetsbelastningar som är nivåindelade över flera datorer.

## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![Säkerhet och efterlevnad i OMS](media/operations-management-suite-overview/icon-security-compliance.png) Säkerhet och efterlevnad
Säkerhet och efterlevnad hjälper dig att identifiera, utvärdera och minska säkerhetsrisker tooyour infrastruktur.  De här funktionerna i OMS implementeras via flera lösningar i logganalys som analysera loggdata och konfiguration från agent system tooassist du till att säkerställa hello pågående säkerheten för din miljö.

* Hej [säkerhet och granska lösningen](oms-security-getting-started.md) samlas in och analyseras säkerhetshändelser på hanterade system tooidentify misstänkt aktivitet.
* Hej [Programlösningen](../log-analytics/log-analytics-malware.md) rapporter om hello status för skydd mot skadlig kod på hanterade datorer.  
* hello systemuppdateringar lösning utför en analys av hello säkerhetsuppdateringar och andra uppdateringar för dina hanterade datorer så att du enkelt identifiera system som kräver korrigering.

## <a name="next-steps"></a>Nästa steg
* Läs mer om [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Läs mer om [Azure Automation](../automation/automation-intro.md).
* Läs mer om [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Läs mer om [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).

