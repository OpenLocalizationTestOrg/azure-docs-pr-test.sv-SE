---
title: "Felsökning av RemoteApp - aaaAzure programfel för att starta och anslutningen | Microsoft Docs"
description: "Lär dig hur tootroubleshoot problem med start- och ansluter tooapplications i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e5cf7171-d1c2-4053-a38b-5af7821305e1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e51d480c9d3fa1f2076f95b63c7a8cd2f6956a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Felsöka Azure RemoteApp - programfel för att starta och anslutning
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Finns i Azure RemoteApp-programmen inte toolaunch för flera olika skäl. Den här artikeln beskrivs olika anledningar och felmeddelanden som användarna kan få när försök toolaunch program. Det är också pratar om anslutningsfel. (Men den här artikeln beskriver inte problem när du loggar in hello Azure RemoteApp-klienten.)  

Läs vidare för information om vanliga felmeddelanden på grund av tooapp start- och fel.

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Vi håller på att ställa in... Försök igen om 10 minuter.
Det här felet betyder Azure RemoteApp skala upp toomeet hello kapacitetsbehovet för dina användare. I bakgrunden hello skapas mer Azure RemoteApp-VM-instanser toohandle hello kapacitetsbehov för dina användare. Vanligtvis detta tar cirka fem minuter men kan ta upp too10 minuter. Ibland är det inträffar inte tillräckligt snabbt och resurser krävs omedelbart. Till exempel 9: 00 scenario där många användare behöver toouse din app i Azure RemoteAppn på hello samtidigt. Om detta händer tooyou vi aktivera **kapacitet läge** på hello serverdel. toodo detta öppna ett Azure supportärende. Att vissa tooinclude ditt prenumerations-ID i hello-begäran.  

![Vi att konfigurera](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-tooyour-applications-please-re-launch-your-application"></a>Det gick inte att återansluta automatiskt tooyour program, starta om programmet
Det här meddelandet visas ofta om du använder Azure RemoteApp och placera din PC toosleep som är längre än 4 timmar och sedan aktiverades av din dator och hello Azure RemoteApp-klienten försök tooauto återansluta och tidsgränsen har överskridits.  Instruera användarna toonavigate tillbaka toohello programmet och försök tooopen från hello Azure RemoteApp-klienten.

![Det gick inte att återansluta automatiskt tooyour program](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-hello-temp-profile"></a>Problem med hello tillfällig profil
Felet uppstår när användarprofilen (användarprofil-Disk) inte kunde toomount och hello användare tar emot en tillfälliga profil.  Administratörer bör gå toohello samling i hello Azure-portalen och gå sedan toohello **sessioner** fliken och försök för**logga ut** hello användare. Detta kommer att tvinga en fullständig logga ut från hello användarsession - sedan har hello användaren försök toolaunch en app igen. Om kontakta Azure-supporten om problemet kvarstår.

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp har slutat att fungera
Det här felmeddelandet innebär hello Azure RemoteApp-klienten har ett problem och måste toobe startas om. Instruera användarna tooclose: Välj **Stäng program** och starta sedan om hello Azure RemoteApp-klienten.  Om hello problemet kvarstår öppen och supportärende i Azure.

![Azure RemoteApp har slutat att fungera](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-hello-connection-or-contact-your-system-administrator"></a>Ett fel uppstod när fjärrskrivbordsanslutningen försökte få åtkomst till den här resursen. Försök hello anslutning eller kontakta din systemadministratör
Detta är ett allmänt felmeddelande - kontakta Azure-supporten så kan vi undersöka. 

![Allmänt Azure RemoteApp-meddelande](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

