---
title: "aaaAzure användargränssnitt för Mobile Engagement - startsida"
description: "Lär dig hur toomanage ditt befintliga program och projekt med hjälp av Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: aff578d2-40f6-43e4-b0ea-7d2674cb28a1
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 727dad188c5876d09db84634a17e10b280039c49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-your-existing-application-and-projects"></a>Hur toomanage ditt befintliga program och projekt
Den här artikeln beskriver hello **Start** sidan hello **Mobile Engagement** portal. Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar. Observera att toostart med hello-portalen måste du först toocreate en **Azure Mobile Engagement** konto. 

startsidan för tooget toohello, klickar du på **hem** på hello upp till vänster på sidan hello. Den innehåller hello lista över alla program som ingår i hello valt samling. På den här sidan se du bara en snabb överblick över dina program.

hello-startsidan innehåller även alla projekt som kan innehålla alla program som finns i ditt konto. Observera att alla kan komma åt hello startsida hello Användargränssnittet genom att skapa ett konto, men du behöver toogrant behörighet tooother användare för att de toohave åtkomst tooyour anpassade program i **Mina projekt**.

Du kan också visa hello jämförelsetabell för hello valt program. Eller välj tooview hello jämförelsetabell för valda program i ett projekt.

![Home1][0]

## <a name="my-applications"></a>Mina program
hello snabb överblick över dina program kan du tooselect vilket program som du vill att tooopen tooview hello detaljerad menyfliksområdet alternativ. Du kan klicka hello namnet på programmet tooreturn toohello senast besökta menyfliksområdet plats i ditt program, eller klicka hello växeln ikonen toogo direkt toohello ”inställningar” sidan i ditt program. Du kan söka, filtrera eller sortera hello information som visas på hello program tabeller. Du kan också dra och släpp hello huvuden toochange hello kolumnordning.

Hello översikt över dina program innehåller bland annat:

* **Ny användare trend**: utvecklingen av nya användare över hello senaste två veckorna.
* **Aktiva användare**: antal aktiva användare under hello senaste 30 dagarna.
* **Aktiva användare trend**: utvecklingen av aktiva användare under hello senaste två veckorna.
* **Sessioner**: en session är användningen av hello-program som utförs av en användare, från hello tid hello användaren börjar använda, tills hello användaren slutar.
* **Sessionen trender**: utvecklingen av sessioner över hello senaste två veckorna.

När du klickar på ett program kan starta du övervakning och hantering av dina appar via hello Användargränssnittet. Exempel:    

* [Övervaka realtidsdata för appen](mobile-engagement-user-interface-monitor.md)
* [Analysera historiska data för appen](mobile-engagement-user-interface-analytics.md)
* [Skapa och hantera segment av användare tooidentify användningsmönster](mobile-engagement-user-interface-segments.md)
* [Nå ut toohello användare av ditt program med push-meddelanden](mobile-engagement-user-interface-reach.md)

## <a name="my-projects"></a>Mina projekt
Du kan använda projekt toogroup dina program och ge behörighet tooother användare tooaccess dina program. Du ger behörighet tooother användare genom att ange e-postadress. Hej **nytt projekt** knappen kan du toocreate ett nytt projekt genom att bara ange ”namn” och ”beskrivning” det nya projektet. När projektet har skapats kan du klicka på hello projekt namnet tooedit hello namn och en beskrivning av din produkt och tooselect alla hello-program som du vill använda toosee i det här projektet.

![Home6][60]

Roller är:

* **Visningsprogrammet**: A Viewer är en användare kan bara visa hello program associerade tooa projekt. Ett visningsprogram kan komma åt analytics och övervaka data och titta på Reach resultat. Ett visningsprogram kan inte ändra någon information eller hantera program eller användare. Ett visningsprogram kan inte skapa eller ändra Reach kampanjstatus.
* **Utvecklare**: A utvecklare är en användare som kan göra allt ett visningsprogram kan göra, samt hantera program. En utvecklare kan aktivera och inaktivera program, ändra programinformationen (till exempel och signaturmappar) och skapa Reach-kampanjer. En utvecklare kan inte hantera användare.
* **Administratören**: en administratör är en användare som kan göra allt en utvecklare kan göra, samt hantera användare. En administratör kan bjuda in användare toojoin ett projekt kan ändra användarroller och ändra projektets information. Behörighet på användarnivå kan också anges i ”inställningar”.

Klicka på ett projekt tooview alla hello-program som ingår i det här projektet. hello följande bild visar hello jämförelsetabell för hello valt program.

![(Bostad2)][3]

## <a name="see-also"></a>Se även
* [Begrepp][Link 6]
* [Felsöka Guide Service][Link 24]

<!--Image references-->
[0]: ./media/mobile-engagement-user-interface-home/home0.png
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[60]: ./media/mobile-engagement-user-interface-home/home6.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
