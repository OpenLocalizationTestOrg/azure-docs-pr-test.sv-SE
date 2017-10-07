---
title: "aaaLog supportärende för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig hur toocreate stöd för en begäran och starta en session stöd på din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e1a3aa3c56e036c782c4fb502c477dc0feaa0ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>Kontakta Microsofts Support för din StorSimple
Om det uppstår problem med Microsoft Azure StorSimple-lösning kan du skapa en tjänstbegäran för teknisk support. Du kanske också måste toostart en supportsession på din StorSimple-enhet i en online session med supportpersonalen. Den här artikeln vägleder dig genom:

* Hur toocreate stöd för en begäran.
* Hur hello toostart en session stöd i Windows PowerShell-gränssnittet för din StorSimple-enhet.

Granska hello [StorSimple 8000-serien stöd SLA och information](https://msdn.microsoft.com/library/mt433077.aspx) innan du skapar en supportbegäran.

## <a name="create-a-support-request"></a>Skapa en supportförfrågan
Utför följande steg toocreate en supportbegäran hello:

#### <a name="toocreate-a-support-request"></a>toocreate en supportbegäran
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com/)i hello övre högra hörnet, klickar du på namnet på ditt konto och klicka sedan på **kontakta Microsoft Support**.
   
    ![Kontakta MS supporten via ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. Du kommer att omdirigerade toohello nya Azure-portalen (portal.azure.com). Klicka på hello **ny supportbegäran** panelen.
   
    ![Kontakta supporten för MS via nya portalen](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Hello höger på hello-skärmen, hello **ny supportbegäran** visas. 
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. I hello **grunderna** dialogrutan, fullständig hello följande:                                
   
   1. Från hello **utfärda typ** listrutan, Välj **tekniska**.
   2. Välj en **prenumeration** hello nedrullningsbara listan.
   3. Från hello **Service** listrutan, Välj **StorSimple**. 
   4. Välj en **supportavtal** hello nedrullningsbara listan. Du behöver en betald stöd plan tooenable teknisk Support.
4. Klicka på **Nästa**. Hej **problemet** dialogrutan visas.
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. I hello **problemet** dialogrutan, fullständig hello följande:
   
   1. Välj en **allvarlighetsgrad** nivå hello nedrullningsbara listan.
   2. Välj en **problemtyp** hello nedrullningsbara listan.
   3. Välj en **kategori** hello nedrullningsbara listan. 
   4. I hello **information** rutan, en kort beskrivning av problemet.
   5. I hello **tidsram** rutan, ange hello datum, tid och tidszon som motsvarar toohello senaste förekomst av problemet.
   6. Under **filuppladdning**, klickar du på hello mappen ikonen toobrowse tooyour stöd för paketet.
   7. Välj hello **dela diagnostikinformation** kryssrutan.
6. Klicka på **Nästa**. Hej **kontaktinformation** dialogrutan visas.
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. Ange din kontaktinformation och välj en kontaktmetod (telefon eller e-post). 
8. Välj hello **spara kontaktändringar för framtida supportförfrågningar** kryssrutan.
9. Klicka på **Skapa**.

När du har skickat en begäran om en supporttekniker kommer att kontakta dig så snart som möjligt tooproceed med din begäran.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Starta en session med stöd i Windows PowerShell för StorSimple
tootroubleshoot eventuella problem som du kan uppleva med hello StorSimple-enhet, måste tooengage med hello Microsoft Support-teamet. Microsoft Support kan behöva toouse en stöd session toolog på tooyour enhet. 

Utföra hello följande steg toostart en supportsession:

#### <a name="toostart-a-support-session"></a>toostart en supportsession
1. Hello enhet direkt via seriekonsolen hello eller via en telnet-session från en fjärrdator. toodo, följ hello här i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Hello-sessionen som öppnas, trycker du på hello **RETUR** viktiga tooget en kommandotolk.
3. Välj alternativ 1, i menyn för seriekonsolen av hello **logga in med fullständig åtkomst**.
4. I hello kommandotolk, skriver du hello följande lösenord: 
   
    `Password1`
5. Skriv hello följande kommando i Kommandotolken hello:
   
    `Enable-HcsSupportAccess`
6. En krypterad sträng visas tooyou. Kopiera den här strängen i en textredigerare, till exempel Anteckningar.
7. Spara den här strängen och skicka den i ett e-postmeddelande tooMicrosoft Support. 

> [!IMPORTANT]
> Du kan inaktivera stöd åtkomst genom att köra `Disable-HcsSupportAccess`. Hej StorSimple-enheten försöker också toodisable stöd åtkomst 8 timmar efter hello-sessionen har startats. Det är en bästa praxis toochange StorSimple-enheten autentiseringsuppgifter när du initierar en supportsession.
> 
> 

