---
title: "aaaAzure Monitor: översikt | Microsoft Docs"
description: "Azure övervakaren samlar in statistik för aviseringar, webhooks, Autoskala och automatisering. Artikel också en lista över andra Microsoft-övervakningsalternativ."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: ffa304e7b158f0fceb7f60ab88fab291976aa0e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-monitor"></a>Översikt över Azure Övervakare
Den här artikeln innehåller en översikt över hello Azure-Monitor-tjänsten i Microsoft Azure. Det beskriver vad Azure-Monitor har och pekare tooadditional information om hur toouse Azure-Monitor.  Om du föredrar en videointroduktion finns i nästa steg länkarna längst ned hello i den här artikeln. 

## <a name="why-monitor-your-application-or-system"></a>Varför övervaka programmet eller system
Molnprogram är komplicerade med många rörliga delar. Övervakning innehåller data tooensure tillämpningsprogrammet stanna upp och körs i ett felfritt tillstånd. Toostave av eventuella problem eller felsöka efter de som hjälper också till. Du kan dessutom använda övervakning data toogain djupa insikter om ditt program. Denna kunskap kan hjälpa dig att tooimprove programprestanda eller underhålla eller automatisera åtgärder som annars skulle kräva manuella åtgärder.


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Övervakare för Azure och Microsoft har andra övervakning produkter
Azure-Monitor innehåller basnivån infrastruktur mått och loggar för de flesta tjänster i Microsoft Azure. Azure-tjänster som inte ännu placerar sina data till Azure-Monitor kommer lägga till det i hello framtida.

Microsoft levererar ytterligare produkter och tjänster som erbjuder ytterligare övervakningsfunktioner för utvecklare, DevOps eller IT-Ops som också har lokala installationer. En översikt och förståelse av hur dessa olika produkter och tjänster tillsammans finns [övervakning i Microsoft Azure](monitoring-overview.md).

## <a name="monitoring-sources---compute"></a>Övervaka källor - beräkning

![Modellen för övervakning och diagnostik för icke-beräkningsresurser](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

hello beräknings-tjänster omfattar 
- Molntjänster 
- Virtuella datorer 
- Skaluppsättningar för den virtuella datorn 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Program - diagnostik loggar och programloggarna mått
Program kan köras ovanpå hello Gästoperativsystem i hello beräknings-modellen. De genererar en egen uppsättning loggar och mått. Övervakare för Azure förlitar sig på hello Azure diagnostics tillägg (Windows eller Linux) toocollect de flesta program nivån mått och loggar. typer av hello

* Prestandaräknare
* Programloggar
* Windows-händelseloggar
* Händelsekällan för .NET
* IIS-loggar
* Manifestet baserat ETW
* Krasch minnesdumpar
* Kunden felloggar

Utan hello diagnostik tillägg finns bara några mått som CPU-användning. 

### <a name="host-and-guest-vm-metrics"></a>Värd och Gäst-VM mått
hello har ovanstående beräkningsresurser en dedikerad värd virtuell dator och operativsystem som de nyttjar. hello värd virtuell dator och operativsystem är hello motsvarigheten till roten VM och Gäst-VM i hello Hyper-V hypervisor-modellen. Du kan samla in mått på båda. Du kan också samla in diagnostik loggar på hello gästoperativsystemet.   

### <a name="activity-log"></a>Aktivitetslogg
Du kan söka hello aktivitetsloggen (tidigare kallade drift- eller granskningsloggar) för information om resurs enligt hello Azure-infrastrukturen. hello-loggen innehåller information såsom tider när resurserna skapas eller förstörs.  Mer information finns i [översikt aktivitetsloggen](monitoring-overview-activity-logs.md). 

## <a name="monitoring-sources---everything-else"></a>Övervaka källor - allt annat

![Modellen för övervakning och diagnostik för beräkningsresurser](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Resurs - mätvärden och diagnostik loggar
Filer mätvärden och diagnostikfunktionerna loggar variera beroende på hello resurstypen. Web Apps innehåller till exempel statistik på hello Disk-i/o och procent CPU. Dessa mått finns inte för en Service Bus-kö som ger mått som kön storlek och meddelandet genomflöde i stället. En lista över filer mätvärden för varje resurs som är tillgänglig på [stöds mått](monitoring-supported-metrics.md). 

### <a name="host-and-guest-vm-metrics"></a>Värd och Gäst-VM mått
Det är inte nödvändigtvis en 1:1-mappning mellan din resurs och en viss värd och Gäst-VM så mått inte är tillgängliga.

### <a name="activity-log"></a>Aktivitetslogg
hello aktivitetsloggen är hello samma som för beräkningsresurser.  

## <a name="uses-for-monitoring-data"></a>Används för övervakning av Data
När du samlar in data kan du göra hello följa med i Azure-Monitor

### <a name="route"></a>Routa
Du kan strömma övervakning tooother dataplatserna i realtid.

Exempel:

- Skicka tooApplication insikter så att du kan använda dess bättre verktyg för visualisering och analys.
- Skicka tooEvent Hubs så att du kan vidarebefordra toothird parts verktyg. 

### <a name="store-and-archive"></a>Store och arkivera
Vissa övervakningsdata är redan lagrade och är tillgängliga i Azure-Monitor under en tidsperiod. 
- Mått som lagras i 30 dagar. 
- Aktiviteten loggposter som lagras i 90 dagar. 
- Diagnostik loggar lagras inte alls. 

Om du vill toostore data längre än hello tidsperioder som anges ovan, kan du använda någon Azure-lagring. Övervakningsdata sparas i din lagring för baserat på en bevarandeprincip som du anger. Du har toopay för hello utrymme hello data tar upp i Azure-lagring. 

Några sätt toouse informationen:

- Du kan ha andra verktyg inom eller utanför Azure läsa den och bearbetar den väl har skrivits.
- Du hämtar hello data lokalt för ett lokalt arkiv eller ändra din bevarandeprincip i hello tookeep molndata för längre tid.  
- Du kan lämna hello data i Azure-lagring på obestämd tid för. 

### <a name="query"></a>Fråga
Du kan använda hello Azure övervakaren REST API, plattformsoberoende kommandoradsgränssnittet (CLI)-kommandon, PowerShell-cmdlets eller hello .NET SDK tooaccess hello data i hello system eller Azure storage

Exempel:

* Hämta data för ett anpassat övervakning program som du har skrivit
* Skapa egna frågor och skicka data tooa tredjeparts-programmet.

### <a name="visualize"></a>Visualisera
Visualisera dina övervakningsdata i grafik och diagram hjälper dig att hitta trender snabbare än att slå via hello data.  

Några visualiseringen metoderna är:

* Använd hello Azure-portalen
* Vidarebefordra data tooAzure Application Insights
* Vidarebefordra data tooMicrosoft PowerBI
* Väg hello data tooa från tredje part visualiseringen verktyget med hjälp av antingen live streaming eller genom att läsa hello verktyget från ett Arkiv i Azure-lagring


### <a name="automate"></a>Automatisera
Du kan använda data tootrigger övervakningsaviseringar eller hela processer. Exempel:

* Använd data tooautoscale compute-instanser uppåt eller nedåt utifrån belastningen för programmet.
* Skicka e-postmeddelanden när ett mått korsar ett förutbestämt tröskelvärde.
* Anropa en webbtjänst-URL (webhook) tooexecute en åtgärd i ett system utanför Azure
* Starta en runbook i Azure automation tooperform en rad olika uppgifter

## <a name="methods-of-accessing-azure-monitor"></a>Metoder för att komma åt Azure-Monitor
I allmänhet kan du ändra dataspårning, Routning och hämtning med någon av följande metoder hello. Inte alla metoder är tillgängliga för alla åtgärder eller -datatyper.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Plattformsoberoende kommandoradsgränssnittet (CLI)](insights-cli-samples.md)
* [REST-API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Nästa steg
Lär dig mer om
- En videogenomgång av bara Azure-Monitor finns på  
[Kom igång med Azure-Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). En ytterligare video som förklarar ett scenario där du kan använda Azure-Monitor finns på [utforska Microsoft Azure-övervakning och diagnostik](https://channel9.msdn.com/events/Ignite/2016/BRK2234) och [Azure Övervakare i ett videoklipp från Ignite 2016](https://myignite.microsoft.com/videos/4977)
- Kör genom hello Azure-Monitor gränssnitt i [komma igång med Azure-Monitor](monitoring-get-started.md)
- Ställ in hello [Azure Diagnostics tillägg](../azure-diagnostics.md) om du försöker toodiagnose problem i din molntjänst, virtuell dator, virtuella skala anger eller Service Fabric-programmet.
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) om du försöker toodiagnostic problem i din App Service Web app.
- [Felsöka Azure Storage](../storage/common/storage-e2e-troubleshooting.md) när du använder Storage-Blobbar, tabeller eller köer
- [Logga Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) och hello [Operations Management Suite](https://www.microsoft.com/oms/)
