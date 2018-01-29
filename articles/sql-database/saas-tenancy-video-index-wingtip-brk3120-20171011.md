---
title: Video indexerade Azure SaaS SQL app | Microsoft Docs
description: "Den här artikeln indexerar olika tidpunkter på vår 81 minuter video om SaaS DB appdesign innehavare, från Ignite konferens hålls 11 oktober 2017. Du kan gå vidare till delen som intresserar dig. Minst 3 mönster beskrivs. Azure-funktioner som underlättar utveckling och hantering beskrivs."
ms.custom: 
ms.date: 12/06/2017
ms.prod: sql-server
ms.reviewer: billgib
ms.suite: 
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: article
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: db47d1dd0a6114ebca0715da179cc0629b26781a
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/07/2017
---
# <a name="video-indexed-and-annotated-for-mulit-tenant-saas-app-using-azure-sql-database"></a>Video indexerade och kommenterade för mulit klient SaaS-program med Azure SQL Database

Den här artikeln är ett kommenterade index i tid platserna för en 81 minuter lång video om SaaS innehavare modeller eller mönster. Den här artikeln kan du hoppa bakåt eller framåt i videon till vilken del intresserar dig. Videon förklarar viktiga designalternativen för ett databasprogram med flera innehavare på Azure SQL Database. Videon innehåller demonstrationer genomgångar av kod för hantering och ibland detalj informera upplevelse än vad som kan vara i vår skriftlig dokumentation.

Videon förstärks informationen i vår skriftlig dokumentation finns på: 
- *Konceptuell:* [innehavare mönster för flera innehavare SaaS-databas][saas-concept-design-patterns-563e]
- *Självstudier:* [Wingtip biljetter SaaS-program][saas-how-welcome-wingtip-app-679t]

Videon och artiklarna beskriver många faser för att skapa ett program med flera innehavare i Azure SQL Database i molnet. Särskilda funktioner i Azure SQL Database gör det enklare att utveckla och implementera flera innehavare appar som är enklare att hantera och tillförlitligt performant.

Vi uppdaterar regelbundet vår skriftlig dokumentation. Videon redigeras eller uppdaterades inte, så slutligen flera av dess detaljer som kan bli inaktuell.



## <a name="sequence-of-38-time-indexed-screenshots"></a>Sekvens med 38 tid-indexerat skärmdumpar

Det här avsnittet indexerar tid platsen för 38 diskussioner i hela det 81 minuter lång video. Varje gång index används med en skärmbild från videon och ibland med ytterligare information.

Varje gång indexet är i formatet *hh: mm:*. Till exempel andra indexerade tid plats med etiketten **Session mål**, startar vid ungefärliga tid för **0:03:11**.


### <a name="compact-links-to-video-indexed-time-locations"></a>Compact länkar till video indexerade tid platser

Följande titlar är länkar till sina motsvarande kommenterade avsnitt längre fram i den här artikeln:

- [1. **(Start)**  Välkommen bild, 0:00:03](#anchor-image-wtip-min00001)
- [2. Sessionen mål, 0:03:11](#anchor-image-wtip-min00311)
- [3. Ärende 0:04:17](#anchor-image-wtip-min00417)
- [4. Webbprogram med flera innehavare, 0:05:05](#anchor-image-wtip-min00505)
- [5. Appen webbformulär i åtgärden, 0:05:55](#anchor-image-wtip-min00555)
- [6. Kostnad (skala, isolering, återställning), 0 per klient: 09:31](#anchor-image-wtip-min00931)
- [7. Databasen modeller för flera innehavare:- och nackdelar 0:11:59](#anchor-image-wtip-min01159)
- [8. Hybrid modellen blandar fördelarna med Huvudmålservern/ST, 0:13:01](#anchor-image-wtip-min01301)
- [9. Stöd för en innehavare jämfört med flera innehavare:- och nackdelar 0:16:44](#anchor-image-wtip-min01644)
- [10. Pooler är kostnadseffektiv för oväntade arbetsbelastningar, 0:19:36](#anchor-image-wtip-min01936)
- [11. Demonstration av databasen per klient och hybrid ST/Huvudmålservern 0:20:08](#anchor-image-wtip-min02008)
- [12. Live appen från visar Dojo 0:20:29](#anchor-image-wtip-min02029)
- [13. MYOB och inte en DBA synliga och 0:28:54](#anchor-image-wtip-min02854)
- [14. MYOB elastisk pool användning exempelvis 0:29:40](#anchor-image-wtip-min02940)
- [15. Lär dig från MYOB och andra ISV: er, 0:31:36](#anchor-image-wtip-min03136)
- [16. Mönster skriver till E2E SaaS scenariot 0:43:15](#anchor-image-wtip-min04315)
- [17. Kanoniskt hybrid flera innehavare SaaS appen, 0:47:33](#anchor-image-wtip-min04733)
- [18. Wingtip SaaS exempelappen 0:48:10](#anchor-image-wtip-min04810)
- [19. Scenarier och mönster utforskade i självstudier, 0:49:10](#anchor-image-wtip-min04910)
- [20. Demonstration av självstudier och Github-lagringsplatsen, 0:50:18](#anchor-image-wtip-min05018)
- [21. Github-repo Microsoft/WingtipSaaS 0:50:38](#anchor-image-wtip-min05038)
- [22. Utforska mönster, 0:56:20](#anchor-image-wtip-min05620)
- [23. Etablering av klienter och onboarding 0:57:44](#anchor-image-wtip-min05744)
- [24. Etablering av klienter och program anslutning, 0:58:58](#anchor-image-wtip-min05858)
- [25. Demonstration av hantering av skript etablering av en enskild klient 0:59:43](#anchor-image-wtip-min05943)
- [26. PowerShell för att etablera och katalogen, 02:00:1](#anchor-image-wtip-min10002)
- [27. T-SQL SELECT * från TenantsExtended 1:03:30](#anchor-image-wtip-min10330)
- [28. Hantera oförutsägbart klienternas arbetsbelastningar, 1:04:36](#anchor-image-wtip-min10436)
- [29. Övervakning, elastisk pool 1:06:39](#anchor-image-wtip-min10639)
- [30. Läsa in generation och prestandaövervakning, 1:09:42](#anchor-image-wtip-min10942)
- [31. Schemahantering av i skala, 1:10:33](#anchor-image-wtip-min11033)
- [32. Distribuerade frågor över klient databaser, 1:12:21](#anchor-image-wtip-min11221)
- [33. Demonstration av biljett generation, 1:12:32](#anchor-image-wtip-min11232)
- [34. SSMS ad hoc analytics, 1:12:46](#anchor-image-wtip-min11246)
- [35. Extrahera klientdata till SQL DW 1:16:32](#anchor-image-wtip-min11632)
- [36. Diagram över dagliga försäljning distribution, 1:16:48](#anchor-image-wtip-min11648)
- [37. Sammanfattning och anrop till åtgärd, 1:19:52](#anchor-image-wtip-min11952)
- [38. Mer information, 42:20:1](#anchor-image-wtip-min12042)


&nbsp;

### <a name="annotated-index-time-locations-in-the-video"></a>Kommenterade index tid platser i videon

Klicka på någon skärmbild bild går du till den exakta tidpunkt platsen i videon.


&nbsp; <a name="anchor-image-wtip-min00001"/>
#### <a name="1-start-welcome-slide-00001"></a>1. *(Start)*  Välkommen bild, 0:00:01

*Lär dig från MYOB: designmönster för SaaS-program på Azure SQL Database - BRK3120*

[![Välkommen till bild][image-wtip-min00003-brk3120-whole-welcome]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1)

- Rubrik: Learning från MYOB: designmönster för SaaS-program på Azure SQL Database
- Bill.Gibson@microsoft.com
- Högsta programansvarig, Azure SQL-databas
- Microsoft Ignite session BRK3120, Orlando, FL USA oktober/11 2017


&nbsp; <a name="anchor-image-wtip-min00311"/>
#### <a name="2-session-objectives-00153"></a>2. Sessionen mål, 0:01:53
[![Sessionen mål][image-wtip-min00311-session]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=113)

- Alternativa modeller för flera innehavare appar med för- och nackdelar.
- SaaS-mönster för att minska kostnaderna för utveckling, hantering och resurs.
- En exempelapp + skript.
- Funktioner för PaaS + SaaS mönster gör du SQL-databasen en mycket skalbar, kostnadseffektiv dataplattform för flera innehavare SaaS.


&nbsp; <a name="anchor-image-wtip-min00417"/>
#### <a name="3-agenda-00409"></a>3. Ärende 0:04:09
[![Ärende][image-wtip-min00417-agenda]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=249)


&nbsp; <a name="anchor-image-wtip-min00505"/>
#### <a name="4-multi-tenant-web-app-00500"></a>4. Webbprogram med flera innehavare, 0:05:00
[![Wingtip SaaS app: flera innehavare för webbprogram][image-wtip-min00505-web-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=300)


&nbsp; <a name="anchor-image-wtip-min00555"/>
#### <a name="5-app-web-form-in-action-00539"></a>5. Appen webbformulär i åtgärden, 0:05:39
[![Appen webbformulär i praktiken][image-wtip-min00555-app-web-form]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=339)


&nbsp; <a name="anchor-image-wtip-min00931"/>
#### <a name="6-per-tenant-cost-scale-isolation-recovery-00658"></a>6. Kostnad (skala, isolering, återställning), 0 per klient: 06:58
[![Per klientorganisation kostnad, skala, isolering, återställning][image-wtip-min00931-per-tenant-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=418)


&nbsp; <a name="anchor-image-wtip-min01159"/>
#### <a name="7-database-models-for-multi-tenant-pros-and-cons-00952"></a>7. Databasen modeller för flera innehavare:- och nackdelar 0:09:52
[![Databasen modeller för flera innehavare:- och nackdelar][image-wtip-min01159-db-models-pros-cons]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=592)


&nbsp; <a name="anchor-image-wtip-min01301"/>
#### <a name="8-hybrid-model-blends-benefits-of-mtst-01229"></a>8. Hybrid modellen blandar fördelarna med Huvudmålservern/ST, 0:12:29
[![Hybrid modellen blandar fördelarna med Huvudmålservern/ST][image-wtip-min01301-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=749)


&nbsp; <a name="anchor-image-wtip-min01644"/>
#### <a name="9-single-tenant-vs-multi-tenant-pros-and-cons-01311"></a>9. Stöd för en innehavare jämfört med flera innehavare:- och nackdelar 0:13:11
[![Stöd för en innehavare jämfört med flera innehavare:- och nackdelar][image-wtip-min01644-st-vs-mt]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=791)


&nbsp; <a name="anchor-image-wtip-min01936"/>
#### <a name="10-pools-are-cost-effective-for-unpredictable-workloads-01749"></a>10. Pooler är kostnadseffektiv för oväntade arbetsbelastningar, 0:17:49
[![Pooler är kostnadseffektiv för oväntade arbetsbelastningar][image-wtip-min01936-pools-cost]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1069)


&nbsp; <a name="anchor-image-wtip-min02008"/>
#### <a name="11-demo-of-database-per-tenant-and-hybrid-stmt-01959"></a>11. Demonstration av databasen per klient och hybrid ST/Huvudmålservern 0:19:59
[![Demonstration av databasen per klient och hybrid ST/Huvudmålservern][image-wtip-min02008-demo-st-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1199)


&nbsp; <a name="anchor-image-wtip-min02029"/>
#### <a name="12-live-app-form-showing-dojo-02010"></a>12. Live appen från visar Dojo 0:20:10
[![Aktiva app formuläret visar Dojo][image-wtip-min02029-live-app-form-dojo]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1210)

&nbsp; <a name="anchor-image-wtip-min02854"/>
#### <a name="13-myob-and-not-a-dba-in-sight-02506"></a>13. MYOB och inte en DBA synliga och 0:25:06
[![MYOB och inte en DBA i kalkylbladet][image-wtip-min02854-myob-no-dba]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1506)


&nbsp; <a name="anchor-image-wtip-min02940"/>
#### <a name="14-myob-elastic-pool-usage-example-02930"></a>14. MYOB elastisk pool användning exempelvis 0:29:30
[![MYOB elastisk pool syntax exempel][image-wtip-min02940-myob-elastic]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1770)


&nbsp; <a name="anchor-image-wtip-min03136"/>
#### <a name="15-learning-from-myob-and-other-isvs-03125"></a>15. Lär dig från MYOB och andra ISV: er, 0:31:25
[![Learning från MYOB och andra ISV: er][image-wtip-min03136-learning-isvs]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1885)


&nbsp; <a name="anchor-image-wtip-min04315"/>
#### <a name="16-patterns-compose-into-e2e-saas-scenario-03142"></a>16. Mönster skriver till E2E SaaS scenariot 0:31:42
[![Mönster utgöra i E2E SaaS-scenario][image-wtip-min04315-patterns-compose]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1902)


&nbsp; <a name="anchor-image-wtip-min04733"/>
#### <a name="17-canonical-hybrid-multi-tenant-saas-app-04604"></a>17. Kanoniskt hybrid flera innehavare SaaS appen, 0:46:04
[![Kanoniskt hybrid flera innehavare SaaS-app][image-wtip-min04733-canonical-hybrid]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2764)


&nbsp; <a name="anchor-image-wtip-min04810"/>
#### <a name="18-wingtip-saas-sample-app-04801"></a>18. Wingtip SaaS exempelappen 0:48:01
[![Wingtip SaaS sample-appen][image-wtip-min04810-wingtip-saas-app]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2881)


&nbsp; <a name="anchor-image-wtip-min04910"/>
#### <a name="19-scenarios-and-patterns-explored-in-the-tutorials-04900"></a>19. Scenarier och mönster utforskade i självstudier, 0:49:00
[![Scenarier och mönster utforskade i självstudierna][image-wtip-min04910-scenarios-tutorials]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=2940)


&nbsp; <a name="anchor-image-wtip-min05018"/>
#### <a name="20-demo-of-tutorials-and-github-repository-05012"></a>20. Demonstration av självstudier och Github-lagringsplatsen, 0:50:12
[![Demo självstudier och Github-repo][image-wtip-min05018-demo-tutorials-github]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3012)


&nbsp; <a name="anchor-image-wtip-min05038"/>
#### <a name="21-github-repo-microsoftwingtipsaas-05032"></a>21. Github-repo Microsoft/WingtipSaaS 0:50:32
[![Github-repo Microsoft/WingtipSaaS][image-wtip-min05038-github-wingtipsaas]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3032)


&nbsp; <a name="anchor-image-wtip-min05620"/>
#### <a name="22-exploring-the-patterns-05615"></a>22. Utforska mönster, 0:56:15
[![Utforska mönster][image-wtip-min05620-exploring-patterns]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3375)


&nbsp; <a name="anchor-image-wtip-min05744"/>
#### <a name="23-provisioning-tenants-and-onboarding-05619"></a>23. Etablering av klienter och onboarding 0:56:19
[![Etablering av klienter och onboarding][image-wtip-min05744-provisioning-tenants-onboarding-1]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3379)


&nbsp; <a name="anchor-image-wtip-min05858"/>
#### <a name="24-provisioning-tenants-and-application-connection-05752"></a>24. Etablering av klienter och program anslutning, 0:57:52
[![Etablering av klienter och program anslutning][image-wtip-min05858-provisioning-tenants-app-connection-2]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3472)


&nbsp; <a name="anchor-image-wtip-min05943"/>
#### <a name="25-demo-of-management-scripts-provisioning-a-single-tenant-05936"></a>25. Demonstration av hantering av skript etablering av en enskild klient 0:59:36
[![Demonstration av hanteringsskript etablering av en enskild klient][image-wtip-min05943-demo-management-scripts-st]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3576)


&nbsp; <a name="anchor-image-wtip-min10002"/>
#### <a name="26-powershell-to-provision-and-catalog-05956"></a>26. PowerShell för att etablera och katalogen, 0:59:56
[![PowerShell för att etablera och katalogen][image-wtip-min10002-powershell-provision-catalog]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3596)


&nbsp; <a name="anchor-image-wtip-min10330"/>
#### <a name="27-t-sql-select--from-tenantsextended-10325"></a>27. T-SQL SELECT * från TenantsExtended 1:03:25
[![T-SQL SELECT * från TenantsExtended][image-wtip-min10330-sql-select-tenantsextended]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3805)


&nbsp; <a name="anchor-image-wtip-min10436"/>
#### <a name="28-managing-unpredictable-tenant-workloads-10334"></a>28. Hantera oförutsägbart klienternas arbetsbelastningar, 1:03:34
[![Hantera oförutsägbart klienternas arbetsbelastningar][image-wtip-min10436-managing-unpredictable-workloads]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3814)


&nbsp; <a name="anchor-image-wtip-min10639"/>
#### <a name="29-elastic-pool-monitoring-10632"></a>29. Övervakning, elastisk pool 1:06:32
[![Övervakning av elastisk pool][image-wtip-min10639-elastic-pool-monitoring]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=3992)


&nbsp; <a name="anchor-image-wtip-min10942"/>
#### <a name="30-load-generation-and-performance-monitoring-10937"></a>30. Läsa in generation och prestandaövervakning, 1:09:37
[![Läs in generation och prestandaövervakning][image-wtip-min10942-load-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4117)


&nbsp; <a name="anchor-image-wtip-min11033"/>
#### <a name="31-schema-management-at-scale-10940"></a>31. Schemahantering av i skala, 1:09:40
[![Schemahantering i större skala][image-wtip-min11033-schema-management-scale]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=34120)


&nbsp; <a name="anchor-image-wtip-min11221"/>
#### <a name="32-distributed-query-across-tenant-databases-11118"></a>32. Distribuerade frågor över klient databaser, 1:11:18
[![Distribuerade frågor över klient databaser][image-wtip-min11221-distributed-query]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4278)


&nbsp; <a name="anchor-image-wtip-min11232"/>
#### <a name="33-demo-of-ticket-generation-11228"></a>33. Demonstration av biljett generation, 1:12:28
[![Demonstration av biljett generation][image-wtip-min11232-demo-ticket-generation]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4348)


&nbsp; <a name="anchor-image-wtip-min11246"/>
#### <a name="34-ssms-adhoc-analytics-11235"></a>34. SSMS ad hoc analytics, 1:12:35
[![SSMS ad hoc analytics][image-wtip-min11246-ssms-adhoc-analytics]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4355)


&nbsp; <a name="anchor-image-wtip-min11632"/>
#### <a name="35-extract-tenant-data-into-sql-dw-11546"></a>35. Extrahera klientdata till SQL DW 1:15:46
[![Extrahera klientdata till SQL DW][image-wtip-min11632-extract-tenant-data-sql-dw]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4546)


&nbsp; <a name="anchor-image-wtip-min11648"/>
#### <a name="36-graph-of-daily-sale-distribution-11638"></a>36. Diagram över dagliga försäljning distribution, 1:16:38
[![Diagram över dagliga försäljning distribution][image-wtip-min11648-graph-daily-sale-distribution]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4598)


&nbsp; <a name="anchor-image-wtip-min11952"/>
#### <a name="37-wrap-up-and-call-to-action-11743"></a>37. Sammanfattning och anrop till åtgärd, 1:17:43
[![Sammanfattning och anrop till åtgärd][image-wtip-min11952-wrap-up-call-action]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4663)


&nbsp; <a name="anchor-image-wtip-min12042"/>
#### <a name="38-resources-for-more-information-12035"></a>38. Mer information, 1:20:35
[![Mer information][image-wtip-min12042-resources-more-info]](https://www.youtube.com/watch?v=jjNmcKBVjrc&t=4835)

- [Blogginlägget 22 kan 2017][resource-blog-saas-patterns-app-dev-sql-db-768h]

- *Konceptuell:* [innehavare mönster för flera innehavare SaaS-databas][saas-concept-design-patterns-563e]

- *Självstudier:* [Wingtip biljetter SaaS-program][saas-how-welcome-wingtip-app-679t]

- Github-lagringsplatser för varianter av Wingtip biljetter SaaS innehavare-program:
    - [Github-repo-för - fristående programmodell][github-wingtip-standaloneapp].
    - [Github-repo-för - DB Per klient modellen][github-wingtip-dbpertenant].
    - [Github-repo-för - modell för flera innehavare DB][github-wingtip-multitenantdb].





## <a name="next-steps"></a>Nästa steg

- [Första kursen artikel][saas-how-welcome-wingtip-app-679t]




<!-- Image link reference IDs. -->

[image-wtip-min00003-brk3120-whole-welcome]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00003-brk3120-welcome-myob-design-saas-app-sql-db.png "Välkommen till bild"

[image-wtip-min00311-session]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00311-session-objectives-takeaway.png "Sessionen mål."

[image-wtip-min00417-agenda]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00417-agenda-app-management-models-patterns.png "Ärende."

[image-wtip-min00505-web-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00505-wingtip-saas-app-mt-web.png "Wingtip SaaS app: flera innehavare för webbprogram"

[image-wtip-min00555-app-web-form]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00555-app-form-contoso-concert-hall-night-opera.png "Appen webbformulär i praktiken"

[image-wtip-min00931-per-tenant-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min00931-saas-data-management-concerns.png "Per klientorganisation kostnad, skala, isolering, återställning"

[image-wtip-min01159-db-models-pros-cons]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01159-db-models-multi-tenant-saas-apps.png "Databasen modeller för flera innehavare:- och nackdelar"

[image-wtip-min01301-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01301-hybrib-model-blends-benefits-mt-st.png "Hybrid modellen blandar fördelarna med Huvudmålservern/ST"

[image-wtip-min01644-st-vs-mt]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01644-st-mt-pros-cons.png "Stöd för en innehavare jämfört med flera innehavare:- och nackdelar"

[image-wtip-min01936-pools-cost]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min01936-pools-cost-effective-unpredictable-workloads.png "Pooler är kostnadseffektiv för oväntade arbetsbelastningar"

[image-wtip-min02008-demo-st-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02008-demo-st-hybrid.png "Demonstration av databasen per klient och hybrid ST/Huvudmålservern"

[image-wtip-min02029-live-app-form-dojo]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02029-app-form-dogwwod-dojo.png "Aktiva app formuläret visar Dojo"

[image-wtip-min02854-myob-no-dba]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02854-myob-no-dba.png "MYOB och inte en DBA i kalkylbladet"

[image-wtip-min02940-myob-elastic]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min02940-myob-elastic-pool-usage-example.png "MYOB elastisk pool syntax exempel"

[image-wtip-min03136-learning-isvs]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min03136-myob-isv-saas-patterns-design-scale.png "Learning från MYOB och andra ISV: er"

[image-wtip-min04315-patterns-compose]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04315-patterns-compose-into-e2e-saas-scenario-st-mt.png "Mönster utgöra i E2E SaaS-scenario"

[image-wtip-min04733-canonical-hybrid]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04733-canonical-hybrid-mt-saas-app.png "Kanoniskt hybrid flera innehavare SaaS-app"

[image-wtip-min04810-wingtip-saas-app]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04810-saas-sample-app-descr-of-modules-links.png "Wingtip SaaS sample-appen"

[image-wtip-min04910-scenarios-tutorials]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min04910-scenarios-patterns-explored-tutorials.png "Scenarier och mönster utforskade i självstudierna"

[image-wtip-min05018-demo-tutorials-github]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05018-demo-saas-tutorials-github-repo.png "Demonstration av självstudier och Github-repo"

[image-wtip-min05038-github-wingtipsaas]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05038-github-repo-wingtipsaas.png "Github-repo Microsoft/WingtipSaaS"

[image-wtip-min05620-exploring-patterns]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05620-exploring-patterns-tutorials.png "Utforska mönster"

[image-wtip-min05744-provisioning-tenants-onboarding-1]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05744-provisioning-tenants-connecting-run-time-1.png "Etablering av klienter och onboarding"

[image-wtip-min05858-provisioning-tenants-app-connection-2]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05858-provisioning-tenants-connecting-run-time-2.png "Etablering av klienter och program anslutning"

[image-wtip-min05943-demo-management-scripts-st]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min05943-demo-management-scripts-provisioning-st.png "Demonstration av hanteringsskript etablering av en enskild klient"

[image-wtip-min10002-powershell-provision-catalog]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10002-powershell-code.png "PowerShell för att etablera och katalogen"

[image-wtip-min10330-sql-select-tenantsextended]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10330-ssms-tenantcatalog.png "T-SQL SELECT * från TenantsExtended"

[image-wtip-min10436-managing-unpredictable-workloads]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10436-managing-unpredictable-tenant-workloads.png "Hantera oförutsägbart klienternas arbetsbelastningar"

[image-wtip-min10639-elastic-pool-monitoring]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10639-elastic-pool-monitoring.png "Övervakning av elastisk pool"

[image-wtip-min10942-load-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min10942-schema-management-scale.png "Läs in generation och prestandaövervakning"

[image-wtip-min11033-schema-management-scale]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11033-schema-manage-1000s-dbs-one.png "Schemahantering i större skala"

[image-wtip-min11221-distributed-query]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11221-distributed-query-all-tenants-asif-single-db.png "Distribuerade frågor över klient databaser"

[image-wtip-min11232-demo-ticket-generation]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11232-demo-ticket-generation-distributed-query.png "Demonstration av biljett generation"

[image-wtip-min11246-ssms-adhoc-analytics]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11246-tsql-adhoc-analystics-db-elastic-query.png "SSMS ad hoc analytics"

[image-wtip-min11632-extract-tenant-data-sql-dw]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11632-extract-tenant-data-analytics-db-dw.png "Extrahera klientdata till SQL DW"

[image-wtip-min11648-graph-daily-sale-distribution]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11648-graph-daily-sale-contoso-concert-hall.png "Diagram över dagliga försäljning distribution"

[image-wtip-min11952-wrap-up-call-action]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min11952-wrap-call-action-saasfeedback.png "Sammanfattning och anrop till åtgärd"

[image-wtip-min12042-resources-more-info]: media/saas-tenancy-video-index-wingtip-brk3120-20171011/wingtip-20171011-min12042-resources-blog-github-tutorials-get-started.png "Mer information"




<!-- Article link reference IDs. -->

[saas-concept-design-patterns-563e]: saas-tenancy-app-design-patterns.md

[saas-how-welcome-wingtip-app-679t]: saas-tenancy-welcome-wingtip-tickets-app.md


[video-on-youtube-com-478y]: https://www.youtube.com/watch?v=jjNmcKBVjrc&t=1

[video-on-channel9-479c]: https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3120


[resource-blog-saas-patterns-app-dev-sql-db-768h]: https://azure.microsoft.com/blog/saas-patterns-accelerate-saas-application-development-on-sql-database/


[github-wingtip-standaloneapp]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp/

[github-wingtip-dbpertenant]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/

[github-wingtip-multitenantdb]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/

