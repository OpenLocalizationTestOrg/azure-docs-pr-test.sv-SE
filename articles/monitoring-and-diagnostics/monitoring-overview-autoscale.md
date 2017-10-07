---
title: aaaOverview av Autoskala i virtuella datorer i Microsoft Azure Cloud Services och Web Apps | Microsoft Docs
description: "Översikt över Autoskala i Microsoft Azure. Gäller tooVirtual datorer, Cloud Services och Web Apps."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 74bf03be-e658-4239-a214-c12424b53e4c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2016
ms.author: robb
ms.openlocfilehash: ef68b722ea22c4149a7d7a89c5855ba761db9247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Översikt över Autoskala i virtuella datorer i Microsoft Azure Cloud Services och Web Apps
Den här artikeln beskriver vilka Microsoft Azure-Autoskala, dess fördelar, och hur tooget startas med hjälp av den.  

Azure övervakaren Autoskala gäller endast för[Skalningsuppsättningar i virtuella](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [molntjänster](https://azure.microsoft.com/services/cloud-services/), och [App Service - Webbappar](https://azure.microsoft.com/services/app-service/web/).

> [!NOTE]
> Azure har två Autoskala metoder. En äldre version av Autoskala gäller tooVirtual datorer (tillgänglighetsuppsättningar). Den här funktionen har begränsad support och vi rekommenderar att migrera toovirtual datorn skalningsuppsättningarna för snabbare och mer tillförlitlig Autoskala support. En länk på hur toouse hello äldre teknik ingår i den här artikeln.  
>
>

## <a name="what-is-autoscale"></a>Vad är Autoskala?
Autoskalningsfunktionen kan toohave hello rätt antal resurser körs toohandle hello belastningen på ditt program. Det gör att du tooadd resurser toohandle ökar i belastning och även spara pengar genom att ta bort resurser som placerad inaktiv. Du kan ange lägsta och högsta antal instanser toorun och lägga till eller ta bort virtuella datorer automatiskt baserat på en uppsättning regler. Med en minsta gör att körs programmet alltid även under ingen belastning. Med en begränsar din totalkostnaden möjliga varje timme. Du skalar automatiskt mellan dessa två yttersta med regler som du skapar.

 ![Autoskala beskrivs. Lägga till och ta bort virtuella datorer](./media/monitoring-overview-autoscale/AutoscaleConcept.png)

När villkor uppfylls, utlöses en eller flera automatiska åtgärder. Du kan lägga till och ta bort virtuella datorer, eller utföra andra åtgärder. hello visar följande konceptuellt diagram den här processen.  

 ![Flödesdiagram för Autoskala](./media/monitoring-overview-autoscale/Autoscale_Overview_v4.png)

hello gäller följande förklaring toohello delar av hello föregående diagram.   

## <a name="resource-metrics"></a>Resurs-mått
Resurser genererar mått, de här måtten bearbetas senare av regler. Mått levereras via olika metoder.
Skaluppsättningar för den virtuella datorn använder telemetridata från Azure diagnostics agenter telemetri för webbprogram och molntjänster kommer direkt från hello Azure-infrastrukturen. Några vanliga statistiken omfattar processoranvändning, minnesanvändning, antalet trådar, kölängd och diskanvändning. En lista över vilka telemetridata som du kan använda finns [Autoskala vanliga mått](insights-autoscale-common-metrics.md).

## <a name="custom-metrics"></a>Anpassade mått
Du kan också utnyttja dina egna anpassade mått som dina program kan avger. Om du har konfigurerat dina program toosend mått tooApplication insikter som du kan utnyttja dessa mått toomake beslut om tooscale eller inte. 

## <a name="time"></a>Tid
Schema-baserade regler baserat på UTC. Du måste ange tidszonen korrekt när du konfigurerar dina regler.  

## <a name="rules"></a>Regler
hello diagram visar endast en Autoskala regeln, men du kan ha många av dem. Du kan skapa komplexa överlappande regler som behövs för din situation.  Typer av regel  

* **Mått-baserade** – till exempel göra den här åtgärden när CPU-användning är över 50%.
* **Tidsbaserade** – till exempel utlösaren webhook var 8: 00 på lördag i en viss tidszon.

Mått-baserade regler mäta programinläsning och lägga till eller ta bort virtuella datorer baserat på att belastningen. Schema-baserade regler kan du tooscale när du se klockslag i din belastning och vill tooscale innan ett möjligt belastningen ökar eller minskar inträffar.  

## <a name="actions-and-automation"></a>Åtgärder och automatisering
Regler kan utlösa en eller flera typer av åtgärder.

* **Skala** -skala VMs in eller ut
* **E-post** – skicka e-post toosubscription administratörer, medadministratörer och/eller ytterligare e-postadress som du anger
* **Automatisera via webhooks** -anropa webhooks, som kan utlösa flera komplexa åtgärder i eller utanför Azure. Du kan starta en Azure Automation-runbook, Azure-funktion eller Azure Logikappen i Azure. Exempel-URL från tredje part utanför Azure innehåller tjänster som Slack och Twilio.

## <a name="autoscale-settings"></a>Autoskala inställningar
Autoskala använder hello följande terminologi och struktur.

- En **autoskalningsinställning** läses av hello Autoskala motorn toodetermine om tooscale uppåt eller nedåt. Den innehåller en eller flera profiler, information om hello målresurs och inställningar för meddelanden.

    - En **Autoskala profil** är en kombination av a:

        - **kapacitet**, vilket anger hello lägsta och högsta och standardvärdena för antalet instanser.
        - **uppsättning regler**, som innehåller en utlösare (tid eller mått) och en skalningsåtgärd (uppåt eller nedåt).
        - **återkommande**, vilket anger att när Autoskala ska införas den här profilen.

        Du kan ha flera profiler som gör att du tootake vård olika överlappande krav. Du kan ha olika autoskalningsprofiler för olika tidpunkter på dagen eller veckodagar hello, t.ex.

    - En **inställning för händelseavisering** definierar vilka meddelanden som ska utföras när Autoskala händelse inträffar baserat på uppfyller hello kriterierna för en av hello automatiska måttinställningarna profiler. Autoskala kan meddela en eller flera e-postadresser eller göra anrop tooone eller flera webhooks.


![Azure autoskalningsinställning, profil och regelstruktur](./media/monitoring-overview-autoscale/AzureResourceManagerRuleStructure3.png)

hello fullständig lista över konfigurerbara fält och beskrivningar finns i hello [Autoskala REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Kodexempel finns

* [Avancerade Autoskala konfigurationen med hjälp av Resource Manager-mallar för Skalningsuppsättningar](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Autoskala REST API](https://msdn.microsoft.com/library/dn931953.aspx)

## <a name="horizontal-vs-vertical-scaling"></a>Vågrät eller lodrät skalning
Autoskala endast skalas vågrätt, vilket ökar (”out”) eller minska (”i”) i hello antal VM-instanser.  Vågrät är mer flexibelt i en situation med molnet som den kan du toorun potentiellt tusentals virtuella datorer toohandle belastningen.

Däremot är lodrät skalning olika. Det låter hello samma antal virtuella datorer, men gör hello VMs mer (”upp”) eller mindre (”nedåt”) kraftfulla. Power mäts i minne, processorhastighet, diskutrymme osv.  Lodrät skalning har flera begränsningar. Det är beroende av hello tillgängligheten för större maskinvara, vilket snabbt träffar en övre gräns och kan variera beroende på region. Lodrät skalning också vanligtvis kräver en VM-toostop och starta om.

Mer information finns i [lodrätt skala virtuella Azure-datorn med Azure Automation](../virtual-machines/linux/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="methods-of-access"></a>Åtkomstmetoder
Du kan ställa in autoskalning via

* [Azure Portal](insights-how-to-scale.md)
* [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
* [Plattformsoberoende kommandoradsgränssnittet (CLI)](insights-cli-samples.md#autoscale)
* [Azure-Monitor REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx)

## <a name="supported-services-for-autoscale"></a>Tjänster som stöds för Autoskala
| Tjänst | Schemat & Docs |
| --- | --- |
| Web Apps |[Skalning av webbprogram](insights-how-to-scale.md) |
| Molntjänster |[Autoskala en tjänst i molnet](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuella datorer: klassiska |[Skalning klassiska virtuella Tillgänglighetsuppsättningar](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuella datorer: Skalningsuppsättningar Windows |[Skalning virtuella datorn anger i Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) |
| Virtuella datorer: Skalningsuppsättningarna Linux |[Skalning virtuella datorn anger i Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuella datorer: Windows-exempel |[Avancerade Autoskala konfigurationen med hjälp av Resource Manager-mallar för Skalningsuppsättningar](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Nästa steg
toolearn mer om Autoskala, Använd hello Autoscale Walkthroughs som angavs tidigare eller referera toohello följande resurser:

* [Azure övervakaren Autoskala vanliga mått](insights-autoscale-common-metrics.md)
* [Metodtips för Azure-Monitor Autoskala](insights-autoscale-best-practices.md)
* [Använda automatiska åtgärder toosend e-post och webhook aviseringar](insights-autoscale-to-webhook-email.md)
* [Autoskala REST API](https://msdn.microsoft.com/library/dn931953.aspx)
* [Felsökning virtuella skala anger Autoskala](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
