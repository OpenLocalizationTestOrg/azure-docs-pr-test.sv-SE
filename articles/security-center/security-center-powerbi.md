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
# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a><span data-ttu-id="fedba-103">Kunskap genom statistik från Azure Security Center med Power BI</span><span class="sxs-lookup"><span data-stu-id="fedba-103">Get insights from Azure Security Center data with Power BI</span></span>
<span data-ttu-id="fedba-104">Hej [Power BI-instrumentpanel](http://aka.ms/azure-security-center-power-bi) för Azure Security Center kan toovisualize, analysera och filtrera rekommendationer och säkerhetsaviseringar från var som helst, inklusive din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="fedba-104">hello [Power BI Dashboard](http://aka.ms/azure-security-center-power-bi) for Azure Security Center enables you toovisualize, analyze, and filter recommendations and security alerts from anywhere, including your mobile device.</span></span> <span data-ttu-id="fedba-105">Använd hello Power BI dashboard tooreveal trender och angreppsmönster, se säkerhetsaviseringar efter resurs eller käll-IP-adress och oåtgärdade säkerhetsrisker efter resurs eller ålder.</span><span class="sxs-lookup"><span data-stu-id="fedba-105">Use hello Power BI dashboard tooreveal trends and attack patterns - view security alerts by resource or source IP address and unaddressed security risks by resource or age.</span></span>

<span data-ttu-id="fedba-106">Du kan också kombinera Security Centers rekommendationer och säkerhetsaviseringar med andra data på intressanta sätt, till exempel använda data från [Azure-granskningsloggarna](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) och [Azure SQL Database Auditing](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/).</span><span class="sxs-lookup"><span data-stu-id="fedba-106">You can also mash up Security Center recommendations and security alerts with other data in interesting ways, for example using data from [Azure Audit Logs](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) and [Azure SQL Database Auditing](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/).</span></span> <span data-ttu-id="fedba-107">Båda har Power BI-instrumentpaneler och du kan också exportera den här informationen tooExcel för enkel rapportering om hello säkerhetsläget för molnresurser.</span><span class="sxs-lookup"><span data-stu-id="fedba-107">Both offer Power BI Dashboards, and you can also export this data tooExcel for easy reporting on hello security state of your cloud resources.</span></span>

## <a name="using-azure-security-center-dashboard-tooaccess-power-bi"></a><span data-ttu-id="fedba-108">Med hjälp av Azure Security Center instrumentpanelen tooaccess Power BI</span><span class="sxs-lookup"><span data-stu-id="fedba-108">Using Azure Security Center dashboard tooaccess Power BI</span></span>
<span data-ttu-id="fedba-109">Du kan också använda hello Azure Security Center instrumentpanelen tooaccess Power BI-rapporter.</span><span class="sxs-lookup"><span data-stu-id="fedba-109">You can also use hello Azure Security Center dashboard tooaccess Power BI reports.</span></span> <span data-ttu-id="fedba-110">Följ hello steg tooperform aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="fedba-110">Follow hello steps tooperform this task:</span></span>

1. <span data-ttu-id="fedba-111">I hello **Azure Security Center** instrumentpanelen, klickar du på **Power BI** knappen.</span><span class="sxs-lookup"><span data-stu-id="fedba-111">In hello **Azure Security Center** dashboard, click **Power BI** button.</span></span>

    ![Ansluta tooAzure Security Center med Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-1-newUI-2017.png)
2. <span data-ttu-id="fedba-113">Hej **Power BI** blad öppnas hello höger enligt hello följande skärm:</span><span class="sxs-lookup"><span data-stu-id="fedba-113">hello **Power BI** blade opens on hello right side as shown in hello following screen:</span></span>

    ![Ansluta tooAzure Security Center med Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new11-2017.png)
3. <span data-ttu-id="fedba-115">Om du skapar hello Power BI-instrumentpanel för hello första gången, kan du välja en av hello följande alternativ i hello **utforska i Power BI** bladet:</span><span class="sxs-lookup"><span data-stu-id="fedba-115">If you are creating hello Power BI dashboard for hello first time, you can choose one of hello following options in hello **Explore in Power BI** blade:</span></span>

   * <span data-ttu-id="fedba-116">**Instrumentpanel för säkerhetsinformation**: Välj det här alternativet om du vill toocreate en instrumentpanel med säkerhetsstatus, trådar och identifieringar.</span><span class="sxs-lookup"><span data-stu-id="fedba-116">**Security insights dashboard**: choose this option if you want toocreate a dashboard that includes security status, threads, and detections.</span></span> <span data-ttu-id="fedba-117">Det här alternativet väljs ofta av utvecklare som har ansvar för analys av skyddsstatus och hittade varningar inom prenumerationerna.</span><span class="sxs-lookup"><span data-stu-id="fedba-117">This option is a more common for DevOps role that is responsible for analyzing their protection status and detected alerts across subscriptions.</span></span>
   * <span data-ttu-id="fedba-118">**Policy management dashboard**: Välj det här alternativet om du vill tooexplore hantering och säkerhetsrutiner principen.</span><span class="sxs-lookup"><span data-stu-id="fedba-118">**Policy management dashboard**: choose this option if you want tooexplore management and enforcement policy.</span></span>  <span data-ttu-id="fedba-119">Det här alternativet väljs ofta av centralt IT-ansvariga som mest arbetar med styrning och administration.</span><span class="sxs-lookup"><span data-stu-id="fedba-119">This option is a more common for Central IT who are more focused on governance.</span></span> <span data-ttu-id="fedba-120">De kan använda den här instrumentpanelen toogain synlighet och insikter om säkerhetsrutinerna följs i organisationen.</span><span class="sxs-lookup"><span data-stu-id="fedba-120">They can use this dashboard toogain visibility and insights on security policy adherence across their organization.</span></span>
   * <span data-ttu-id="fedba-121">Om du redan har en Power BI-instrumentpanelen, klickar du på **gå tooyour aktuella Power BI-instrumentpanel**.</span><span class="sxs-lookup"><span data-stu-id="fedba-121">If you already have a Power BI dashboard, click **Go tooyour current Power BI dashboard**.</span></span>
4. <span data-ttu-id="fedba-122">Klicka på alternativet **Instrumentpanel för säkerhetsinformation** för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="fedba-122">For this example, click **Security insights dashboard** option.</span></span> <span data-ttu-id="fedba-123">Om det är hello första gången skapar du en Power BI-instrumentpanel för Security Center du uppmanas tooinstall hello-Innehållspaketet.</span><span class="sxs-lookup"><span data-stu-id="fedba-123">If this is hello first time, you are creating a Power BI dashboard for Security Center you are prompted tooinstall hello content pack.</span></span> <span data-ttu-id="fedba-124">Klicka på **hämta** knapp i hello **Content Pack för Power BI** fönster som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="fedba-124">Click **Get** button in hello **Content packs for Power BI** window as shown in hello following screen:</span></span>

    ![Azure Security Center, instrumentpanel för säkerhetsinformation](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)
5. <span data-ttu-id="fedba-126">Hej **ansluta tooAzure Security Center Säkerhetsinsikter** fönster visas.</span><span class="sxs-lookup"><span data-stu-id="fedba-126">hello **Connect tooAzure Security Center Security Insights** window appear.</span></span> <span data-ttu-id="fedba-127">Se till att hello **autentisering** metoden är **oAuth2** enligt nedan och klickar på **inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="fedba-127">Make sure hello **Authentication** method is **oAuth2** as shown below and click **Sign in** button.</span></span>

    ![Autentisering](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)
6. <span data-ttu-id="fedba-129">Du kan bli ombedd tooauthenticate igen med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="fedba-129">You may be asked tooauthenticate again with your Azure credentials.</span></span> <span data-ttu-id="fedba-130">Efter autentiseringen skapas instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="fedba-130">After authenticating your dashboard will be created.</span></span> <span data-ttu-id="fedba-131">När hello instrumentpanelen har skapats visas en rapport med hello liknar enligt hello följande skärm:</span><span class="sxs-lookup"><span data-stu-id="fedba-131">Once hello dashboard is created you see a report with hello similar structure as shown in hello following screen:</span></span>

    ![Power BI-instrumentpanel](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)

> [!NOTE]
> <span data-ttu-id="fedba-133">En uppdatering av hello rapporten är schemalagda tootake dagligen.</span><span class="sxs-lookup"><span data-stu-id="fedba-133">A refresh of hello report is scheduled tootake place on a daily basis.</span></span> <span data-ttu-id="fedba-134">Om det uppstår ett fel på den här uppdateringen kan du läsa [möjliga uppdateringsproblem med hello Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), mer information om hur tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="fedba-134">In case you experience a failure on this refresh, read [Potential Refresh Issues with hello Azure Security Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), for more information on how tootroubleshoot.</span></span>
>
>

<span data-ttu-id="fedba-135">Här kan du se hello många säkerhetsaviseringar och rekommendationer samt hello antalet virtuella datorer, Azure SQL-databaser och nätverksresurser som övervakas av Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="fedba-135">Here you can see hello number of security alerts and recommendations, as well as hello number of VMs, Azure SQL databases, and network resources being monitored by Azure Security Center.</span></span>

<span data-ttu-id="fedba-136">En länk tooAzure Security Center omdirigerar toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fedba-136">A link tooAzure Security Center redirects you toohello Azure portal.</span></span> <span data-ttu-id="fedba-137">hello diagram gör det enkelt toovisualize information om säkerhetsrekommendationer och aviseringar, inklusive:</span><span class="sxs-lookup"><span data-stu-id="fedba-137">hello charts make it easy toovisualize information about security recommendations and alerts, including:</span></span>

* <span data-ttu-id="fedba-138">Resurssäkerhetsstatus</span><span class="sxs-lookup"><span data-stu-id="fedba-138">Resource Security State</span></span>
* <span data-ttu-id="fedba-139">Outförda rekommendationer</span><span class="sxs-lookup"><span data-stu-id="fedba-139">Pending Recommendations</span></span>
* <span data-ttu-id="fedba-140">Rekommendationer för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="fedba-140">VM Recommendations</span></span>
* <span data-ttu-id="fedba-141">Aviseringar över tid</span><span class="sxs-lookup"><span data-stu-id="fedba-141">Alerts over Time</span></span>
* <span data-ttu-id="fedba-142">Angripna resurser</span><span class="sxs-lookup"><span data-stu-id="fedba-142">Attacked Resources</span></span>
* <span data-ttu-id="fedba-143">Angripna IP-adresser</span><span class="sxs-lookup"><span data-stu-id="fedba-143">Attacked IPs</span></span>

<span data-ttu-id="fedba-144">Bakom de olika diagrammen finns ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="fedba-144">Behind each chart, there are additional insights.</span></span> <span data-ttu-id="fedba-145">Välj en sida vid sida-toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="fedba-145">Select a tile toosee more information.</span></span> <span data-ttu-id="fedba-146">Till exempel hello **resurs säkerhetstillstånd** innehåller ytterligare information om outförda rekommendationer av resurser som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="fedba-146">For example, hello **Resource Security State** tile shows you additional details about pending recommendations by resources as shown in hello following screen:</span></span>

![Rekommendationer](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

<span data-ttu-id="fedba-148">Om du klickar på någon av staplarna i diagrammet kommer hello andra toogray ut och du fokus på hello som du valt.</span><span class="sxs-lookup"><span data-stu-id="fedba-148">If you click on any line of this graph, hello others are going toogray out and you focus only on hello one you selected.</span></span> <span data-ttu-id="fedba-149">tooreturn toohello instrumentpanelen, klickar du på **Azure Security Center** under hello **instrumentpaneler** alternativet hello vänster på sidan.</span><span class="sxs-lookup"><span data-stu-id="fedba-149">tooreturn toohello dashboard, click **Azure Security Center** under hello **Dashboards** option on hello left pane of this page.</span></span>

> [!NOTE]
> <span data-ttu-id="fedba-150">Om du vill att toocustomize dina rapporter genom att lägga till extra fält eller ändra befintliga illustrationer, kan du redigera hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="fedba-150">If you’d like toocustomize your reports by adding additional fields or changing existing visuals, you can edit hello report.</span></span> <span data-ttu-id="fedba-151">Läs artikeln om att [interagera med rapporter i redigeringsvyn i Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="fedba-151">Read [Interact with a report in Editing View in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) for more information.</span></span>
>
>

<span data-ttu-id="fedba-152">Hej **aviseringar över tid, angripna resurser** och **angripare IP-adresser** brickor har hello liknande utdata när du klickar på var och en av den.</span><span class="sxs-lookup"><span data-stu-id="fedba-152">hello **Alerts over Time, Attacked Resources** and **Attacker IPs** tiles will have hello similar output when you click on each one of it.</span></span> <span data-ttu-id="fedba-153">Detta inträffar eftersom hello rapporten samlar in information om alla de här tre variablerna och anropar den **resurser angripet** enligt hello följande skärm:</span><span class="sxs-lookup"><span data-stu-id="fedba-153">This happens because hello report aggregates information regarding all those three variables and calls it **Resources under Attack** as shown in hello following screen:</span></span>

![Resurser som är angripna](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

<span data-ttu-id="fedba-155">Då du kan också spara en kopia av den här rapporten, skriva ut den eller publicera den på hello-webbplatsen med hjälp av hello alternativen i hello **filen** menyn.</span><span class="sxs-lookup"><span data-stu-id="fedba-155">At this point you can also save a copy of this report, print it or publish it on hello web by using hello options available in hello **File** menu.</span></span>

![Arkivmenyn](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a><span data-ttu-id="fedba-157">Utforska dina data i Azure Security Center med Power BI-tjänsterna</span><span class="sxs-lookup"><span data-stu-id="fedba-157">Exploring your Azure Security Center data with Power BI services</span></span>
<span data-ttu-id="fedba-158">Ansluta toohello [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) i Power BI och köra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fedba-158">Connect toohello [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) in Power BI and execute hello following steps:</span></span>

1. <span data-ttu-id="fedba-159">I hello **Innehållspaketet för Power BI** ser du två alternativ som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="fedba-159">In hello **Content Pack for Power BI** window you will see two options as shown below.</span></span>

    ![Innehållspaket för Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

   > [!NOTE]
   > <span data-ttu-id="fedba-161">Om du redan utförts hello första delen av den här artikeln visas bara ett alternativ, Azure Security Center Policy Management.</span><span class="sxs-lookup"><span data-stu-id="fedba-161">If already executed hello first part of this article you will see only one option, which is Azure Security Center Policy Management.</span></span>
   >
   >
2. <span data-ttu-id="fedba-162">Hello syftet med det här exemplet, klickar du på **hämta** i hello **Azure Security Center Policy Management** panelen.</span><span class="sxs-lookup"><span data-stu-id="fedba-162">For hello purpose of this example, click **Get** in hello **Azure Security Center Policy Management** tile.</span></span>
3. <span data-ttu-id="fedba-163">I hello **ansluta tooAzure Security Center Policy Management** fönster, se till att tooselect **oAuth2** under **autentiseringsmetod** listrutan som visas nedan och klickar på **Inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="fedba-163">In hello **Connect tooAzure Security Center Policy Management** window, make sure tooselect **oAuth2** under **Authentication Method** drop down as shown below and click **Sign in** button.</span></span>

    ![Principhanteringsfönstret](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)
4. <span data-ttu-id="fedba-165">Du kommer att omdirigerade tooan autentiseringssida där du anger hello autentiseringsuppgifter för att du använder tooconnect tooAzure Security Center.</span><span class="sxs-lookup"><span data-stu-id="fedba-165">You will be redirected tooan authentication page where you should type hello credentials that you are using tooconnect tooAzure Security Center.</span></span> <span data-ttu-id="fedba-166">När hello-autentiseringen är klar, startar Power BI importerar data toobuild dina rapporter.</span><span class="sxs-lookup"><span data-stu-id="fedba-166">After hello authentication process is complete, Power BI will start importing data toobuild your reports.</span></span> <span data-ttu-id="fedba-167">Under tiden kan du se hello efter meddelande i hello högra hörnet i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="fedba-167">During this time you may see hello following message on hello right corner of your browser:</span></span>

    ![Ansluta tooAzure Security Center med Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

   > [!NOTE]
   > <span data-ttu-id="fedba-169">När hello instrumentpanelen skapas för hello första gången kan det ta längre tid än vanligt, främst för scenarier där du har flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="fedba-169">when hello dashboard is being created for hello first time it can take longer than usual, mainly for scenarios where you have multiple subscriptions.</span></span>
   >
   >
5. <span data-ttu-id="fedba-170">När hello processen är klar läses din Azure Security Center Power BI-instrumentpanel med hello **principhantering** rapportera liknande toohello som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="fedba-170">Once hello process is finished, your Azure Security Center Power BI dashboard will load with hello **Policy Management** report similar toohello one shown below:</span></span>

    ![Instrumentpanelen för principhantering](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a><span data-ttu-id="fedba-172">Se även</span><span class="sxs-lookup"><span data-stu-id="fedba-172">See also</span></span>
<span data-ttu-id="fedba-173">I det här dokumentet du lärt dig hur toouse Power BI i Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="fedba-173">In this document, you learned how toouse Power BI in Azure Security Center.</span></span> <span data-ttu-id="fedba-174">toolearn mer om Azure Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="fedba-174">toolearn more about Azure Security Center, see hello following:</span></span>

* <span data-ttu-id="fedba-175">[Planera för Azure Security Center och handboken](security-center-planning-and-operations-guide.md) – Lär dig hur tooplan införandet av Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="fedba-175">[Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) — Learn how tooplan Azure Security Center adoption.</span></span>
* <span data-ttu-id="fedba-176">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsinställningar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="fedba-176">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security settings in Azure Security Center</span></span>
* <span data-ttu-id="fedba-177">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar</span><span class="sxs-lookup"><span data-stu-id="fedba-177">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts</span></span>
* <span data-ttu-id="fedba-178">[Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="fedba-178">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service</span></span>
* <span data-ttu-id="fedba-179">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure</span><span class="sxs-lookup"><span data-stu-id="fedba-179">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) — Find blog posts about Azure security and compliance</span></span>
