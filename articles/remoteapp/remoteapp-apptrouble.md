---
title: "Felsökning för Azure RemoteApp - programfel för att starta och anslutningen | Microsoft Docs"
description: "Lär dig hur du felsöker problem med att starta och ansluter till program i Azure RemoteApp."
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
ms.openlocfilehash: fc9d538991adce7fc13e9654b9a7c6d113d03fde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Felsöka Azure RemoteApp - programfel för att starta och anslutning
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Program som finns i Azure RemoteApp kan inte starta för flera olika skäl. Den här artikeln beskrivs olika anledningar och felmeddelanden som användare kan visas vid försök att starta program. Det är också pratar om anslutningsfel. (Men den här artikeln beskriver inte problem när du loggar in på Azure RemoteApp-klienten.)  

Läs vidare för information om vanliga felmeddelanden på grund av fel för att starta och anslutning av app.

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Vi håller på att ställa in... Försök igen om 10 minuter.
Det här felet betyder Azure RemoteApp skala upp så att de uppfyller kapacitetsbehovet för dina användare. I bakgrunden som mer Azure RemoteApp-VM-instanser skapas för att hantera kapacitetsbehov för dina användare. Vanligtvis detta tar cirka fem minuter men kan ta upp till 10 minuter. Ibland är det inträffar inte tillräckligt snabbt och resurser krävs omedelbart. Till exempel en 9: 00 scenario där många användare behöver använda din app i Azure RemoteAppn på samma gång. Om detta händer du vi aktivera **kapacitet läge** på serverdelen. Om du vill göra detta för att öppna ett supportärende i Azure. Vara säker på att inkludera ditt prenumerations-ID i begäran.  

![Vi att konfigurera](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Det gick inte att återansluta automatiskt till programmen, starta om programmet
Det här meddelandet visas ofta om du använder Azure RemoteApp och sedan försätta datorn i viloläge längre än 4 timmar och sedan aktiverades av din dator och Azure RemoteApp-klienten försöker automatiskt återansluter och tidsgränsen har överskridits.  Instruera användarna att gå tillbaka till programmet och försök att öppna från Azure RemoteApp-klienten.

![Det gick inte att återansluta automatiskt till ditt program](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problem med tillfällig profil
Det här felet uppstår om användarprofilen (användarprofil-Disk) inte gick att montera och användaren får en tillfälliga profil.  Administratörer bör navigera till samlingen i Azure-portalen och gå sedan till den **sessioner** fliken och försök att **logga ut** användaren. Detta kommer att tvinga en fullständig logga ut från sessionen - sedan användaren försöker starta en app igen. Om kontakta Azure-supporten om problemet kvarstår.

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp har slutat att fungera
Det här felmeddelandet innebär Azure RemoteApp-klienten har ett problem och måste startas om. Instruera användarna att stänga: Välj **Stäng program** och starta sedan om Azure RemoteApp-klienten.  Om problemet kvarstår öppen och supportärende i Azure.

![Azure RemoteApp har slutat att fungera](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Ett fel uppstod när fjärrskrivbordsanslutningen försökte få åtkomst till den här resursen. Försöka ansluta igen eller kontakta din systemadministratör
Detta är ett allmänt felmeddelande - kontakta Azure-supporten så kan vi undersöka. 

![Allmänt Azure RemoteApp-meddelande](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

