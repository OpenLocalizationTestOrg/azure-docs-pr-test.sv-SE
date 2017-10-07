---
title: "aaaGet igång med Azure-Monitor | Microsoft Docs"
description: "Komma igång med Azure-Monitor toogain inblick i hello driften av dina resurser och vidta åtgärder som är baserade på data."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: johnkem
ms.openlocfilehash: 49e7105621a9e60c9fa14c4911c4fe45e0d197a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-monitor"></a>Kom igång med Azure Monitor
Azure övervakaren är hello platform-tjänst som tillhandahåller en enda källa för övervakning av Azure-resurser. Med Azure-Monitor kan du visualisera, fråga, vidarebefordra, arkivera och vidta åtgärder för hello mått och loggar som kommer från resurser i Azure. Du kan arbeta med dessa data med hello övervakaren portalbladet och [övervakaren PowerShell-Cmdlets](insights-powershell-samples.md), [plattformsoberoende CLI](insights-cli-samples.md), eller [Azure övervakaren REST API: er](https://msdn.microsoft.com/library/dn931943.aspx). I den här artikeln vi att gå igenom några av hello huvudkomponenterna i Azure-Monitor med hello portal för att demonstrera.

1. I hello portal, navigerar för**fler tjänster** och hitta hello **övervakaren** alternativet. Klicka på hello stjärnikonen tooadd alternativet tooyour favoritlistan så att det alltid är lättillgängligt från hello vänstra navigeringsfältet.
   
    ![Övervaka i hello Tjänstlista](./media/monitoring-get-started/monitor-more-services.png)
2. Klicka på hello **övervakaren** alternativet tooopen in hello **övervakaren** bladet. På det här bladet sammanförs alla dina övervakningsinställningar och -data till en sammanslagen vy. Öppnas toohello **aktivitetsloggen** avsnitt.
   
    ![Navigera i Monitor-bladet](./media/monitoring-get-started/monitor-blade-nav.png)
   
    Azure övervakaren har tre grundläggande kategorier av övervakningsdata: hello **aktivitetsloggen**, **mått**, och **diagnostikloggar**.
3. Klicka på **aktivitetsloggen** tooensure som hello aktivitet loggen avsnitt visas.
   
    ![Bladet Aktivitetslogg](./media/monitoring-get-started/monitor-act-log-blade.png)
   
    Hej [ **aktivitetsloggen** ](monitoring-overview-activity-logs.md) beskriver alla åtgärder som utförts på resurser i din prenumeration. Använder hello aktivitetsloggen, kan du bestämma hello ' vad som, och när ' för alla skapa, uppdatera eller ta bort åtgärder på resurser i din prenumeration. Hello aktivitetsloggen anger du när ett webbprogram har stoppats och som hindrade den. Aktiviteten logghändelser lagras i hello plattform och tillgängliga tooquery i 90 dagar.
   
    Du kan skapa och spara frågor för vanliga filter och PIN-kod hello viktigaste frågor tooa portalens instrumentpanel så att du vet alltid om händelser som uppfyller dina kriterier har inträffat.
4. Filtrera hello visa tooa just den resursgruppen över hello föregående vecka och klicka sedan på hello **spara** knappen.
   
    ![Spara aktivitetsloggfråga](./media/monitoring-get-started/monitor-act-log-save.png)
5. Klicka på hello **PIN-kod** knappen.
   
    ![Klicka på Fäst för aktivitetslogg](./media/monitoring-get-started/monitor-act-log-pin.png)
   
    De flesta hello vyerna i den här genomgången kan vara Fäst tooa instrumentpanelen. På så sätt kan du skapa en enskild informationskälla för användningsdata i dina tjänster. 
6. Returnera tooyour instrumentpanelen. Du kan nu se att hello-fråga (och antalet resultat) visas i instrumentpanelen. Detta är användbart om du vill se tooquickly några åtgärder för hög-profil som har genomförts nyligen i din prenumeration, t.ex. att en ny roll tilldelades eller att en virtuell dator raderades.
   
    ![Aktiviteten loggen Fäst toodashboard](./media/monitoring-get-started/monitor-act-log-db.png)
7. Returnera toohello **övervakaren** panelen och klickar på hello **mått** avsnitt. Du måste först tooselect en resurs genom filtrering och välja med hello nedrullningsbara alternativ hello överst i hello-bladet.
   
    ![Filtrera resurs för mått](./media/monitoring-get-started/monitor-met-filter.png)
   
    Alla Azure-resurser genererar [**mätvärden**](monitoring-overview-metrics.md). I den här vyn samlas alla mått på en och samma plats så att du lätt kan förstå hur dina resurser arbetar.
8. När du har valt en resurs, visas alla tillgängliga mått på hello vänster sida av hello-bladet. Du kan skapa diagram över flera mått på en gång genom att välja mått och ändra hello diagrammet typ och ett tidsintervall. Du kan också visa alla måttaviseringar på den här resursen.
   
    ![Bladet Mått](./media/monitoring-get-started/monitor-metric-blade.png)
   
   > [!NOTE]
   > Vissa mått är bara tillgängliga om du aktiverar [Application Insights](../application-insights/app-insights-overview.md) och/eller Windows eller Linux Azure-diagnostik på din resurs.
   > 
   > 
9. När du är nöjd med ditt diagram, kan du använda hello **PIN-kod** knappen toopin den tooyour instrumentpanelen.
10. Returnera toohello **övervakaren** bladet och klicka på **diagnostikloggar**.
    
    ![Bladet Diagnostikloggar](./media/monitoring-get-started/monitor-diaglogs-blade.png)
    
    [**Diagnostikloggar** ](monitoring-overview-of-diagnostic-logs.md) är loggar som orsakat *av* en resurs som innehåller data om hello driften av just den resursen. Till exempel är loggarna Network Security Group Rule Counters (regelräknare för nätverkssäkerhetsgrupp) och logikappsarbetsflöde båda typer av diagnostikloggar. Dessa loggar kan lagras i ett lagringskonto, direktuppspelat tooan Event Hub eller skickas för[logganalys](../log-analytics/log-analytics-overview.md). Log Analytics är Microsofts produkt för driftsinformation för avancerad sökning och avisering.
    
    Du kan visa och filtrera en lista över alla resurser i din prenumeration tooidentify om de har diagnostikloggar aktiverad i hello-portalen.
11. Klicka på en resurs i hello diagnostikloggar bladet. Om diagnostikloggar lagras på ett lagringskonto visas en lista med timloggar som du kan ladda ned direkt.
    
    ![Diagnostikloggar för en resurs](./media/monitoring-get-started/monitor-diaglogs-detail.png)
    
    Du kan också klicka på **diagnostikinställningar**, vilket kan du tooset in eller ändra inställningar för arkivering tooa lagringskontot strömning tooEvent nav eller för att skicka tooa logganalys-arbetsytan.
    
    ![Aktivera diagnostikloggar](./media/monitoring-get-started/monitor-diaglogs-enable.png)
    
    Om du har ställt in diagnostikloggar tooLog Analytics, kan du söka dem i hello **loggen Sök** på hello portal.
12. Navigera toohello **aviseringar** avsnitt i hello övervakaren bladet.
    
    ![offentligt aviseringsblad](./media/monitoring-get-started/monitor-alerts-nopp.png)
    
    Här kan du hantera alla [**aviseringar**](monitoring-overview-alerts.md) på dina Azure-resurser. Detta inkluderar aviseringar på mått, aktivitet logghändelser, webbtester med Application Insights (platser) och proaktiv diagnostik i Application Insights. Aviseringar kan utlösa en e-toobe skickas eller en HTTP POST tooa Webhooksadressen.
13. Klicka på **Lägg till mått avisering** toocreate en avisering.
    
    ![lägg till måttavisering](./media/monitoring-get-started/monitor-alerts-add.png)
    
    Du kan sedan PIN-kod en avisering tooyour instrumentpanelen tooeasily finns tillståndet när som helst.
14. hello övervakaren avsnittet innehåller också länkar för[Programinsikter](../application-insights/app-insights-overview.md) program och [logganalys](../log-analytics/log-analytics-overview.md) lösningar för hantering. Dessa andra Microsoft-produkter har djupgående integrering med Azure Monitor.
15. Om du inte använder Application Insights eller Log Analytics finns en risk för att Azure Monitor har ett samarbete med dina aktuella produkter för övervakning, loggning och aviseringar. Se vår [partners sidan](monitoring-partners.md) för en fullständig lista över och instruktioner för hur toointegrate.

Genom att följa dessa steg och fästa alla relevanta paneler tooa instrumentpanelen kan skapa du omfattande vyer för programmet och infrastruktur som det här:

![Azure Monitor-instrumentpanel](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Nästa steg
* Läs hello [översikt av Azure-Monitor](monitoring-overview.md)

