---
title: aaaScale instansen antal manuellt eller med Autoskala med Azure Portal | Microsoft Docs
description: "Lär dig hur tooscale dina Azure-tjänster."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: 8cb78f18416bd3caecce52702a8d630aa434d776
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-instance-count-manually-or-automatically"></a>Skala instansantalet manuellt eller automatiskt
I hello [Azure Portal](https://portal.azure.com/), du kan ange hello instanser av tjänsten manuellt eller, du kan ange parametrar som toohave skala automatiskt baserat på begäran. Detta är vanligtvis enligt tooas *skala ut* eller *skala i*.

Innan skalning baserat på instansantal, bör du tänka skalning påverkas av **prisnivå** i tillägget tooinstance antal. Olika prisnivåer kan ha olika antal kärnor och minne, och så de får bättre prestanda för hello samma antal instanser (vilket är *skala upp* eller *skala*). Den här artikeln innehåller *skala i* och *ut*.

Du kan skala i hello portal och du kan också använda hello [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) eller [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooadjust skala manuellt eller automatiskt.

> [!NOTE]
> Den här artikeln beskriver hur toocreate en autoskalningsinställning i hello-portalen på [http://portal.azure.com](http://portal.azure.com). Autoskala inställningar skapas i den här portalen måste redigeras den klassiska portalen för hello ([http://manage.windowsazure.com](http://manage.windowsazure.com)).
> 
> 

## <a name="scaling-manually"></a>Skalning manuellt
1. I hello [Azure Portal](https://portal.azure.com/), klickar du på **Bläddra**, navigera sedan toohello resurs som du vill tooscale, som en **programtjänstplanen**.
2. Klicka på **Inställningar > skala ut (apptjänstplan).**
3. Hello överst i hello **skala** bladet som du kan se en historik över Autoskala åtgärder av hello-tjänsten.
   
    ![Skala bladet](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > Åtgärder som utförs av Autoskala kommer att visas i det här diagrammet. Om du justera hello instansantalet återspeglas hello ändras inte i det här diagrammet.
   > 
   > 
4. Du kan justera hello nummer **instanser** med skjutreglaget.
5. Klicka på hello **spara** kommandot och du kommer att skalas toothat antal instanser för nästan omedelbart.

## <a name="scaling-based-on-a-pre-set-metric"></a>Skalning utifrån en förinställd mått
Om du vill hello antalet instanser, justera tooautomatically baserat på ett mått, Välj hello mått som du vill använda i hello **skala genom** listrutan. Till exempel en **programtjänstplanen** du kan skala genom **CPU-procent**.

1. När du väljer ett mått får du ett skjutreglage, eller text rutorna tooenter hello antalet instanser som du vill tooscale mellan:
   
    ![Skala bladet med CPU-procent](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Autoskala tar aldrig din tjänst under eller över hello gränser som du anger, oavsett dina belastningen.
2. Andra kan välja du hello målområdet för hello mått. Om du har valt exempelvis **CPU-procent**, du kan ange ett mål för hello Genomsnittlig CPU för alla instanser av hello i din tjänst. En skalbar sker när hello Genomsnittlig CPU överskrider hello maximalt definierar du, på samma sätt kan en skala i sker när hello Genomsnittlig CPU sjunker under hello minsta.
3. Klicka på hello **spara** kommando. Autoskala kontrollerar varje några minuter toomake till att du är i intervallet för hello-instans och mål för dina mått. När tjänsten tar emot ytterligare trafik, får du fler instanser utan att göra något.

## <a name="scale-based-on-other-metrics"></a>Skala baserat på andra mått
Du kan skala baserat på mått än hello förinställda som visas i hello **skala genom** listrutan och kan även ha en komplex uppsättning skala ut och skala i regler.

### <a name="adding-or-changing-a-rule"></a>Lägga till eller ändra en regel
1. Välj hello **schema och prestanda regler** i hello **skala genom** listrutan: ![Prestandaregler](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Om du tidigare hade Autoskala visas på en överblick över exakt hello-regler som du hade.
3. tooscale baserat på ett annat mått Klicka hello **Lägg till regel** rad. Du kan också klicka på någon av hello befintliga rader toochange från hello mått du tidigare hade toohello mått som du vill tooscale av.
   ![Lägg till regel](./media/insights-how-to-scale/Insights_AddRule.png)
4. Du måste nu tooselect vilka mått som du vill använda tooscale av. När du väljer det ett mått är ett par saker tooconsider:
   
   * Hej *resurs* hello mått kommer från. Vanligtvis ska detta vara hello samma som hello resurs du skalning. Om du vill tooscale av hello djupet för en kö för lagring är hello resurs hello kö som du vill tooscale av.
   * Hej *måttnamnet* sig själv.
   * Hej *tid aggregering* av hello mått. Detta är hur hello data kombinera över hello *varaktighet*.
5. När du har valt dina mått du hello tröskel för hello mått och hello-operator. Anta exempelvis att du kunde **större än** **80%**.
6. Välj sedan hello åtgärd som du vill tootake. Det finns ett par olika typer av åtgärder:
   
   * Öka eller minska med – det här lägga till eller ta bort hello **värdet** antal instanser som du definierar
   * Öka eller minska procent - hello instansantalet ändras med en procent. Du kan till exempel placera 25 i hello **värdet** fält, och om du har för närvarande 8 instanser, 2 skulle läggas till.
   * Öka eller minska för – detta anger hello instans antal toohello **värdet** du definierar.
7. Slutligen kan du välja lågfrekvent ned - hur länge den här regeln ska vänta efter hello tidigare skala åtgärd tooscale igen.
8. När du har konfigurerat din regel nådde **OK**.
9. När du har konfigurerat alla hello regler som du vill vara säker på att toohit hello **spara** kommando.

### <a name="scaling-with-multiple-steps"></a>Skalning med flera steg
hello exemplen ovan är relativt enkla. Men om du vill toobe mer aggressivt om att skala upp (eller ned), du kan även lägga till flera skala regler för hello samma mått. Du kan till exempel definiera två skalningsregler på CPU-procent:

1. Skala ut med 1 instans om CPU-procent är över 60%
2. Skala ut 3 instanser om CPU-procent är över 85%

![Flera skalningsregler](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Med regeln ytterligare om din belastningen överskrider 85% innan en skalningsåtgärd visas två ytterligare instanser i stället för en.

## <a name="scale-based-on-a-schedule"></a>Skala enligt ett schema
Som standard när du skapar en regel för skala gäller alltid. Du kan se som när du klickar på hello profil huvud:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Men kanske du vill toohave mer aggressivt skalning under hello dag eller hello vecka än på hello helg. Du kan även stänga av tjänsten helt av arbetstid.

1. toodo, på hello-profil som du har väljer **återkommande** i stället för **alltid,** och välj hello gånger som du vill hello profil tooapply.
2. Till exempel en profil som används under hello vecka i hello toohave **dagar** listrutan avmarkera **lördag** och **söndag**.
3. toohave en profil som används under hello dagtid, Ställ in hello **starttid** toohello tid på dagen som du vill toostart på.
   
    ![Standard upprepning](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. Klicka på **OK**.
5. Därefter måste tooadd hello profil som du vill tooapply andra tider. Klicka på hello **lägga till profilen** rad.
    ![Inaktivera arbete](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Namnge din nya sekund, profil, du kan till exempel kalla den **av arbete**.
7. Välj sedan **återkommande** igen och väljer hello instans antal intervall som du vill att under denna tid.
8. Som med hello standardprofil, Välj hello **dagar** du vill använda den här profilen tooapply till och hello **starttid** under hello dag.
   
   > [!NOTE]
   > Autoskala använder hello reglerna för sommartid för **tidszon** du väljer. Dock under sommartid hello UTC-förskjutningen visar hello grundläggande Tidszonförskjutning, inte hello Daylight besparingar UTC-förskjutningen.
   > 
   > 
9. Klicka på **OK**.
10. Nu behöver du tooadd oavsett regler som du vill ha tooapply under andra profilen. Klicka på **Lägg till regel**, och du kan sedan skapa hello samma regel du har under hello standardprofil.
    
    ![Lägg till regel toooff arbete](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Vara att toocreate både en regel för att skala ut och skala, eller under hello kommer profil hello instansantalet endast växa (eller minska).
12. Klicka slutligen på **spara**.

## <a name="next-steps"></a>Nästa steg
* [Övervaka tjänsten](insights-how-to-customize-monitoring.md) toomake att tjänsten är tillgänglig och svarstid.
* [Aktivera övervakning och diagnostik](insights-how-to-use-diagnostics.md) toocollect detaljerad hög frekvens mått på din tjänst.
* [Få aviseringar](insights-receive-alert-notifications.md) när drifthändelser inträffar eller när mätvärden överskrider ett tröskelvärde.
* [Övervaka programprestanda](../application-insights/app-insights-azure-web-apps.md) om du vill toounderstand exakt hur din kod genomför i hello molnet.
* [Visa händelser och aktivitetsloggen](insights-debugging-with-events.md) toolearn allt som har skett i din tjänst.
* [Övervaka tillgänglighet och svarstider på en webbsida](../application-insights/app-insights-monitor-web-app-availability.md) med Application Insights så att du kan läsa mer om sidan inte är tillgänglig.

