---
title: "aaaGet insikter från Azure Security Center med Power BI | Microsoft Docs"
description: "hello Azure Security Center Power BI-innehållspaket gör det enkelt toofind säkerhetsaviseringar, rekommendationer, angripna resurser och trender, baserat på en datamängd som har skapats för dina rapporter."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 0ded6bc7-52e8-43b4-8940-0bee137526e3
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: yurid
ms.openlocfilehash: af68aef626034fe03d793c36b515a3f26619e5f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Kunskap genom statistik från Azure Security Center med Power BI
Hej [Power BI-instrumentpanel](http://aka.ms/azure-security-center-power-bi) för Azure Security Center kan toovisualize, analysera och filtrera rekommendationer och säkerhetsaviseringar från var som helst, inklusive din mobila enhet. Använd hello Power BI dashboard tooreveal trender och angreppsmönster, se säkerhetsaviseringar efter resurs eller käll-IP-adress och oåtgärdade säkerhetsrisker efter resurs eller ålder.

Du kan också kombinera Security Centers rekommendationer och säkerhetsaviseringar med andra data på intressanta sätt, till exempel använda data från [Azure-granskningsloggarna](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) och [Azure SQL Database Auditing](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Båda har Power BI-instrumentpaneler och du kan också exportera den här informationen tooExcel för enkel rapportering om hello säkerhetsläget för molnresurser.

## <a name="using-azure-security-center-dashboard-tooaccess-power-bi"></a>Med hjälp av Azure Security Center instrumentpanelen tooaccess Power BI
Du kan också använda hello Azure Security Center instrumentpanelen tooaccess Power BI-rapporter. Följ hello steg tooperform aktiviteten:

1. I hello **Azure Security Center** instrumentpanelen, klickar du på **Power BI** knappen.

    ![Ansluta tooAzure Security Center med Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-1-newUI-2017.png)
2. Hej **Power BI** blad öppnas hello höger enligt hello följande skärm:

    ![Ansluta tooAzure Security Center med Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new11-2017.png)
3. Om du skapar hello Power BI-instrumentpanel för hello första gången, kan du välja en av hello följande alternativ i hello **utforska i Power BI** bladet:

   * **Instrumentpanel för säkerhetsinformation**: Välj det här alternativet om du vill toocreate en instrumentpanel med säkerhetsstatus, trådar och identifieringar. Det här alternativet väljs ofta av utvecklare som har ansvar för analys av skyddsstatus och hittade varningar inom prenumerationerna.
   * **Policy management dashboard**: Välj det här alternativet om du vill tooexplore hantering och säkerhetsrutiner principen.  Det här alternativet väljs ofta av centralt IT-ansvariga som mest arbetar med styrning och administration. De kan använda den här instrumentpanelen toogain synlighet och insikter om säkerhetsrutinerna följs i organisationen.
   * Om du redan har en Power BI-instrumentpanelen, klickar du på **gå tooyour aktuella Power BI-instrumentpanel**.
4. Klicka på alternativet **Instrumentpanel för säkerhetsinformation** för det här exemplet. Om det är hello första gången skapar du en Power BI-instrumentpanel för Security Center du uppmanas tooinstall hello-Innehållspaketet. Klicka på **hämta** knapp i hello **Content Pack för Power BI** fönster som visas i följande skärmbild hello:

    ![Azure Security Center, instrumentpanel för säkerhetsinformation](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)
5. Hej **ansluta tooAzure Security Center Säkerhetsinsikter** fönster visas. Se till att hello **autentisering** metoden är **oAuth2** enligt nedan och klickar på **inloggning** knappen.

    ![Autentisering](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)
6. Du kan bli ombedd tooauthenticate igen med dina autentiseringsuppgifter för Azure. Efter autentiseringen skapas instrumentpanelen. När hello instrumentpanelen har skapats visas en rapport med hello liknar enligt hello följande skärm:

    ![Power BI-instrumentpanel](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)

> [!NOTE]
> En uppdatering av hello rapporten är schemalagda tootake dagligen. Om det uppstår ett fel på den här uppdateringen kan du läsa [möjliga uppdateringsproblem med hello Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), mer information om hur tootroubleshoot.
>
>

Här kan du se hello många säkerhetsaviseringar och rekommendationer samt hello antalet virtuella datorer, Azure SQL-databaser och nätverksresurser som övervakas av Azure Security Center.

En länk tooAzure Security Center omdirigerar toohello Azure-portalen. hello diagram gör det enkelt toovisualize information om säkerhetsrekommendationer och aviseringar, inklusive:

* Resurssäkerhetsstatus
* Outförda rekommendationer
* Rekommendationer för virtuella datorer
* Aviseringar över tid
* Angripna resurser
* Angripna IP-adresser

Bakom de olika diagrammen finns ytterligare information. Välj en sida vid sida-toosee mer information. Till exempel hello **resurs säkerhetstillstånd** innehåller ytterligare information om outförda rekommendationer av resurser som visas i följande skärmbild hello:

![Rekommendationer](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Om du klickar på någon av staplarna i diagrammet kommer hello andra toogray ut och du fokus på hello som du valt. tooreturn toohello instrumentpanelen, klickar du på **Azure Security Center** under hello **instrumentpaneler** alternativet hello vänster på sidan.

> [!NOTE]
> Om du vill att toocustomize dina rapporter genom att lägga till extra fält eller ändra befintliga illustrationer, kan du redigera hello rapporten. Läs artikeln om att [interagera med rapporter i redigeringsvyn i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) för mer information.
>
>

Hej **aviseringar över tid, angripna resurser** och **angripare IP-adresser** brickor har hello liknande utdata när du klickar på var och en av den. Detta inträffar eftersom hello rapporten samlar in information om alla de här tre variablerna och anropar den **resurser angripet** enligt hello följande skärm:

![Resurser som är angripna](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Då du kan också spara en kopia av den här rapporten, skriva ut den eller publicera den på hello-webbplatsen med hjälp av hello alternativen i hello **filen** menyn.

![Arkivmenyn](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Utforska dina data i Azure Security Center med Power BI-tjänsterna
Ansluta toohello [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) i Power BI och köra hello följande steg:

1. I hello **Innehållspaketet för Power BI** ser du två alternativ som visas nedan.

    ![Innehållspaket för Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

   > [!NOTE]
   > Om du redan utförts hello första delen av den här artikeln visas bara ett alternativ, Azure Security Center Policy Management.
   >
   >
2. Hello syftet med det här exemplet, klickar du på **hämta** i hello **Azure Security Center Policy Management** panelen.
3. I hello **ansluta tooAzure Security Center Policy Management** fönster, se till att tooselect **oAuth2** under **autentiseringsmetod** listrutan som visas nedan och klickar på **Inloggning** knappen.

    ![Principhanteringsfönstret](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)
4. Du kommer att omdirigerade tooan autentiseringssida där du anger hello autentiseringsuppgifter för att du använder tooconnect tooAzure Security Center. När hello-autentiseringen är klar, startar Power BI importerar data toobuild dina rapporter. Under tiden kan du se hello efter meddelande i hello högra hörnet i webbläsaren:

    ![Ansluta tooAzure Security Center med Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

   > [!NOTE]
   > När hello instrumentpanelen skapas för hello första gången kan det ta längre tid än vanligt, främst för scenarier där du har flera prenumerationer.
   >
   >
5. När hello processen är klar läses din Azure Security Center Power BI-instrumentpanel med hello **principhantering** rapportera liknande toohello som visas nedan:

    ![Instrumentpanelen för principhantering](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur toouse Power BI i Azure Security Center. toolearn mer om Azure Security Center finns hello följande:

* [Planera för Azure Security Center och handboken](security-center-planning-and-operations-guide.md) – Lär dig hur tooplan införandet av Azure Security Center.
* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsinställningar i Azure Security Center
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure
