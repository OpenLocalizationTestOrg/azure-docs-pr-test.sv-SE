---
title: "aaaGet igång med Azure Schemaläggaren i Azure-portalen | Microsoft Docs"
description: "Komma igång med Azure Scheduler på Azure-portalen"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 58255c0ad19da65932f8b1d36cb8fef1ff6e651b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Komma igång med Azure Scheduler på Azure-portalen
Det är enkelt toocreate schemalagda jobb i Schemaläggaren på Azure. I den här kursen lär du dig hur toocreate ett jobb. Du lär dig också om övervaknings- och hanteringsfunktionerna i Scheduler.

## <a name="create-a-job"></a>Skapa ett jobb
1. Logga in för[Azure-portalen](https://portal.azure.com/).  
2. Klicka på **+ ny** > typen *Scheduler* i sökrutan hello > Välj **Scheduler** i resultaten > klickar du på **skapa**.
   
    ![][marketplace-create]
3. Vi ska skapa ett jobb som bara skickar en GET-begäran mot http://www.microsoft.com/. I hello **jobb i Schemaläggaren** anger hello följande information:
   
   1. **Namn:** `getmicrosoft`  
   2. **Prenumeration:** Din Azure-prenumeration   
   3. **Jobbsamling:** Markera en befintlig jobbsamling eller klicka på **Skapa ny** > ange ett namn.
4. Nästa i **åtgärdsinställningar**, definiera hello följande värden:
   
   1. **Åtgärdstyp:** ` HTTP`  
   2. **Metod:** `GET`  
   3. **URL:** ` http://www.microsoft.com`  
      
      ![][action-settings]
5. Till sist ska vi definiera ett schema. hello jobb kan definieras som ett enstaka jobb, men väljer vi ett återkommande schema:
   
   1. **Återkommande**: `Recurring`
   2. **Start**: Dagens datum
   3. **Detta ska upprepas varje**: `12 Hours`
   4. **Sluta**: Två dagar från dagens datum  
      
      ![][recurrence-schedule]
6. Klicka på **Skapa**

## <a name="manage-and-monitor-jobs"></a>Hantera och övervaka jobb
När ett jobb har skapats visas den i hello Azure huvudinstrumentpanelen. Klicka på hello jobbet och en ny öppnas med hello följande flikar:

1. Egenskaper  
2. Åtgärdsinställningar  
3. Schema  
4. Historik
5. Användare
   
   ![][job-overview]

### <a name="properties"></a>Egenskaper
Dessa skrivskyddade egenskaper beskrivs hello management metadata för hello jobb i Schemaläggaren.

   ![][job-properties]

### <a name="action-settings"></a>Åtgärdsinställningar
Klicka på ett jobb i hello **jobb** sidan kan du tooconfigure jobbet. På så sätt kan du konfigurera avancerade inställningar, om du inte konfigurerar dem i hello-Snabbregistrering guiden.

Du kan ändra hello återförsöksprincip och hello felåtgärd för alla åtgärdstyper av.

Du kan ändra hello metoden tooany tillåten HTTP-verb för HTTP och HTTPS Jobbåtgärden typer. Du kan också lägga till, ta bort eller ändra hello sidhuvuden och information om grundläggande autentisering.

Du kan ändra hello lagringskonto, könamn, SAS-token och brödtext för kön åtgärd lagringstyper.

Du kan ändra hello namnområdet, avsnittet/kösökvägen, autentiseringsinställningar, transporttyp, meddelandeegenskaper och meddelandetexten för åtgärdstyper för service bus.

   ![][job-action-settings]

### <a name="schedule"></a>Schema
På så sätt kan du konfigurera om hello schema, om du vill att toochange hello schema som du skapade i hello-Snabbregistrering guiden.

Detta är en möjlighet toobuild [komplexa scheman och avancerade upprepning i jobbet](scheduler-advanced-complexity.md)

Du kan ändra hello startdatum och tid, återkommande schema och hello slutdatum och tid (om hello jobbet är återkommande.)

   ![][job-schedule]

### <a name="history"></a>Historik
Hej **historik** fliken visas valda måtten för varje jobbkörningen i hello system för hello valda jobbet. De här måtten ange realtid värden om hello hälsotillståndet för din Schemaläggaren:

1. Status  
2. Detaljer  
3. Antal återförsök
4. Förekomst: första, andra, tredje osv.
5. Starttid för körning  
6. Sluttid för körning
   
   ![][job-history]

Du kan klicka på Kör tooview dess **historikinformation**, inklusive hello hela svaret för varje körning. Den här dialogrutan kan du också toocopy hello svar toohello Urklipp.

   ![][job-history-details]

### <a name="users"></a>Användare
Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure Scheduler. toolearn hur toouse hello användare på fliken finns för[rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md)

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Scheduler](scheduler-concepts-terms.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren](scheduler-advanced-complexity.md)

 [REST API-referens för Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Scheduler](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
