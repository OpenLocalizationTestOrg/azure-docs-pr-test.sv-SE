---
title: "aaaTroubleshoot BizTalk-tjänster med hjälp av åtgärdsloggar | Microsoft Docs"
description: "Felsöka BizTalk-tjänster med hjälp av åtgärdsloggar. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 102779ed6e29784f190c28e4102a7d9670614914
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk-tjänst: Felsökning med åtgärdsloggar

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-hello-operation-logs"></a>Vad är hello Åtgärdsloggar
Åtgärdsloggar är en funktion för tjänster som är tillgängliga i hello klassiska Azure-portalen där du tooview historiska loggar av åtgärder som utförs på Azure-tjänster, inklusive BizTalk-tjänst. Detta gör att du tooview historiska data toomanagement åtgärder på prenumerationen BizTalk Service så långt tillbaka 180 dagar.

> [!NOTE]
> Den här funktionen endast samlar in loggar för hanteringsåtgärder på BizTalk-tjänster, till exempel när hello-tjänsten har startats backas upp, och så vidare. Dessa åtgärder spåras oavsett om de utförs hello klassiska Azure-portalen eller med hjälp av hello [BizTalk Service REST API: er](http://msdn.microsoft.com/library/azure/dn232347.aspx). En fullständig lista över åtgärder som spåras använder tjänster finns [Operations spårade med hjälp av Azure hanteringstjänster](#bizops).<br/><br/>
> Detta in inte hello loggar för aktiviteter relaterade tooBizTalk körtid (t.ex (meddelande bearbetas av bryggor osv.). tooview dessa loggar, Använd hello spårning vyn från hello BizTalk-Services-portalen. Mer information finns i [spårning av meddelanden](http://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>Visa BizTalk Services Åtgärdsloggar
1. Välj i hello klassiska Azure-portalen, **hanteringstjänster**, och välj sedan hello **Åtgärdsloggar** fliken.
2. Du kan filtrera hello loggar baserat på olika parametrar som prenumeration, datumintervall, service-typen (t.ex. BizTalk-tjänst), tjänstnamn eller status för hello-åtgärd (lyckades, misslyckades).
3. Välj hello markering tooview hello filtrerad lista. hello följande bild visar aktiviteter relaterade tootestbiztalkservice: ![visa åtgärdsloggar][ViewLogs] 
4. tooview mer information om en specifik åtgärd, Välj hello raden och klicka på **information** i hello Aktivitetsfältet längst ned hello.

## <a name="bizops"></a>Åtgärder som spåras med hjälp av Azure hanteringstjänster
hello visas följande tabell hello-åtgärder som spåras med hello Azure Management Services:

| Åtgärdsnamn | Aktivitet |
| --- | --- |
| CreateBizTalkService |Åtgärden toocreate en ny BizTalk Service |
| DeleteBizTalkService |Åtgärden toodelete en BizTalk Service |
| RestartBizTalkService |Åtgärden toorestart en BizTalk Service |
| StartBizTalkService |Åtgärden toostart en BizTalk Service |
| StopBizTalkService |Åtgärden toostop en BizTalk Service |
| DisableBizTalkService |Åtgärden toodisable en BizTalk Service |
| EnableBizTalkService |Åtgärden tooenable en BizTalk Service |
| BackupBizTalkService |Åtgärden tooback upp en BizTalk Service |
| RestoreBizTalkService |Åtgärden toorestore en BizTalk Service från angivna säkerhetskopia |
| SuspendBizTalkService |Åtgärden toosuspend en BizTalk Service |
| ResumeBizTalkService |Åtgärden tooresume en BizTalk Service |
| ScaleBizTalkService |Åtgärden tooscale en BizTalk Service uppåt eller nedåt |
| ConfigUpdateBizTalkService |Åtgärden tooupdate hello konfigurationen av en BizTalk Service |
| ServiceUpdateBizTalkService |Åtgärden tooupgrade eller nedgradering av en annan version för BizTalk Service-tooa |
| PurgeBackupBizTalkService |Åtgärden toopurge säkerhetskopior av hello BizTalk Service utanför hello kvarhållningsperioden |

## <a name="see-also"></a>Se även
* [Säkerhetskopiera BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Återställa BizTalk-tjänst från en säkerhetskopia](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Etablering av statusdiagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Begränsning](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Utfärdarens namn och nyckel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

