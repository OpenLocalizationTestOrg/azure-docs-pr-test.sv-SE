---
title: 'aaaAzure Mobile Engagement Troubleshooting Guide - API: er'
description: "Felsökning för Azure Mobile Engagement - API: er"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3efc8a52-2b74-4917-b887-815ae8277474
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: piyushjo
ms.openlocfilehash: 5656b6f0f1aaf3e496a168c7cf09b307b9ab2a4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-api-issues"></a>Felsökningsguide för API-problem
hello följande är möjliga problem som kan uppstå med hur administratörer interagerar med Azure Mobile Engagement via hello API: er.

## <a name="syntax-issues"></a>Syntaxen problem
### <a name="issue"></a>Problem
* Syntaxfel med hjälp av hello API (eller oväntade resultat).

### <a name="causes"></a>Orsaker
* Problem med syntax:
  * Se till att toocheck hello syntaxen för hello specifika API: et du använder tooconfirm som hello alternativet är tillgängligt.
  * Ett vanligt problem med API-användning är tooconfuse hello Reach-API och hello Push-API (de flesta uppgifter ska utföras med hello nå API i stället för hello Push-API). 
  * Ett annat vanliga problem med SDK-integration och API-användning är tooconfuse hello SDK-nyckeln och hello API-nyckel.
  * Skript som ansluter toohello API: er måste toosend data minst var 10: e minut eller hello anslutning gör timeout (särskilt vanligt i övervakaren API-skript som lyssnar efter data). tooprevent timeout har ditt skript skicka en XMPP ping var 10 minuter tookeep hello sessions alive med hello-servern.

### <a name="see-also"></a>Se även
* [API-dokumentationen][Link 4]
* [XMPP protokollet Info](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-toouse-hello-api-tooperform-hello-same-action-available-in-hello-azure-mobile-engagement-ui"></a>Det går inte toouse hello API tooperform hello samma åtgärd som är tillgängliga i hello Azure Mobile Engagement UI
### <a name="issue"></a>Problem
* En åtgärd som inte fungerar hello Azure Mobile Engagement-Gränssnittet inte fungerar från hello relaterade Azure Mobile Engagement-API.

### <a name="causes"></a>Orsaker
* Bekräfta att du kan utföra samma åtgärd från hello Azure Mobile Engagement Användargränssnittet visar du har korrekt integrerat funktionen Azure Mobile Engagement med hello hello SDK.

### <a name="see-also"></a>Se även
* [UI-dokumentation][Link 1]

## <a name="error-messages"></a>Felmeddelanden
### <a name="issue"></a>Problem
* Felkoder med hello-API som visas vid körning eller i loggarna.

### <a name="causes"></a>Orsaker
* Här är en sammansatt lista över vanliga API status koder för referens och preliminär Felsökning:
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from hello current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in hello response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). hello parameters provided toohello API or service are invalid. In this case, hello HTTP response will embed more details. Make sure tootest for hello MIME type of hello response as hello payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check hello AppID and SDK key).
        402        Billing lock. hello application is either off its quotas or is currently in a bad billing state.
        403        hello application is not enabled or hello specific API is disabled for this application.
        403        Unauthorized access toohello project or application, invalid access key (hello key must match hello one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), hello suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and hello campaign’s property named, manual Push must be set tootrue.
        403        hello email address is already associated tooanother account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        hello email address is not associated with an account. Please create or update hello account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying tooedit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for hello first time.
        409        Name already associated tooa different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or hello period is too large toobe displayed (hello server didn’t retrieve hello analytics because hello user request is for a period that is too large).
        503        Analytics not available yet (hello requested information is not computed yet for an application).
        504        hello server was not able toohandle your request in a reasonable time (if you make multiple calls tooan API very quickly, try toomake one call at a time and spread hello calls out over time).

### <a name="see-also"></a>Se även
* [API-dokumentationen – för detaljerade fel på varje specifika API: et][Link 4]

## <a name="silent-failures"></a>Tyst fel
### <a name="issue"></a>Problem
* API-åtgärd misslyckas inget felmeddelande visas vid körning eller i loggarna.

### <a name="causes"></a>Orsaker
* Många objekt som kommer att inaktiveras i hello Azure Mobile Engagement UI om de inte är integrerad korrekt, men misslyckas tyst från hello API, så Kom ihåg tootest hello samma funktioner från hello UI toosee om det fungerar.
* Azure Mobile Engagement och många avancerade funktioner i Azure Mobile Engagement som du försöker toouse måste toobe individuellt integrerade i din app med hello SDK som separata steg innan du kan använda dem.

### <a name="see-also"></a>Se även
* [Felsökningsguide för - SDK][Link 25]

<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

