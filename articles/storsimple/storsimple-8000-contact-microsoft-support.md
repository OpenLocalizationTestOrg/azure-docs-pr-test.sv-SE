---
title: "aaaCreate stöder biljett eller fall för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig hur toolog stöder begäran och starta en session med stöd på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: alkohli;
ms.openlocfilehash: 832e3ac739dafa6dd8c24aaea38aef9c6f1b27b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support"></a>Kontakta Microsofts support

Hej StorSimple Enhetshanteraren ger hello funktion för**logga en ny supportförfrågan** inom hello service sammanfattning bladet. Om det uppstår problem med din StorSimple-lösning kan du skapa en tjänstbegäran för teknisk support. Du kanske också måste toostart en supportsession på din StorSimple-enhet i en online session med supportpersonalen. Den här artikeln vägleder dig genom:

* Hur toocreate stöd för en begäran.
* Hur begäran toomanage en livscykel från hello-portalen.
* Hur hello toostart en session stöd i Windows PowerShell-gränssnittet för din StorSimple-enhet.

Granska hello [StorSimple 8000-serien stöd SLA och information](https://msdn.microsoft.com/library/mt433077.aspx) innan du skapar en supportbegäran.

## <a name="create-a-support-request"></a>Skapa en supportförfrågan

Beroende på din [supportavtal](https://azure.microsoft.com/support/plans/), du kan skapa supportärenden på ett problem på din StorSimple-enhet direkt från hello StorSimple Enhetshanteraren service sammanfattning bladet. Utför följande steg toocreate en supportbegäran hello:

#### <a name="toocreate-a-support-request"></a>toocreate en supportbegäran

1. Gå tooyour StorSimple enheten Manager-tjänsten. Hello sammanfattning bladet inställningar för tjänsten, gå för**stöd + felsökning** avsnittet och klicka sedan på **ny supportbegäran**.
     
    ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. I hello **ny supportbegäran** bladet väljer **grunderna**. I hello **grunderna** bladet hello följande steg:
   1. Från hello **utfärda typ** listrutan, Välj **tekniska**.
   2. hello aktuella **prenumeration**, **Service** typ och hello **resurs** (StorSimple Device Manager service) väljs automatiskt. 
   3. Välj en **supportavtal** hello nedrullningsbara listan om du har flera planer som är associerad med din prenumeration. Du behöver en betald stöd plan tooenable teknisk Support.
   4. Klicka på **Nästa**.

       ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. I hello **ny supportbegäran** bladet väljer **steg 2 problemet**. I hello **problemet** bladet hello följande steg:
    
    1. Välj hello **allvarlighetsgrad**.
    2. Ange om hello problemet är relaterade toohello enhet eller hello StorSimple enheten Manager-tjänsten.
    3. Välj en **kategori** för den här utfärda och ge fler **information** om hello problemet.
    4. Ange hello startdatum och tidpunkt för hello problem.
    5. I hello **filuppladdning**, klickar du på hello mappen ikonen toobrowse tooyour stöd för paketet.
    6. Kontrollera **dela diagnostikinformation**.
    7. Klicka på **Nästa**.

       ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. I hello **ny supportbegäran** bladet, klickar du på **steg 3 kontaktinformation**. I hello **kontaktinformation** bladet hello följande steg:

    1. I hello **Kontaktalternativ**anger önskad kontaktmetod (telefon eller e-post) och hello språk. svarstid för hello väljs automatiskt baserat på din prenumeration planen.
    2. Ange ditt namn, e-post, valfritt kontakt, land i hello kontaktinformation. Välj hello **spara kontaktändringar för framtida supportförfrågningar** kryssrutan.
    3. Klicka på **Skapa**.
   
        ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    Microsoft Support använder denna information tooreach ut tooyou för ytterligare information, diagnostik och upplösning.
När du har skickat en begäran om en supporttekniker kommer att kontakta dig så snart som möjligt tooproceed med din begäran.

## <a name="manage-a-support-request"></a>Hantera en supportbegäran

När du har skapat ett supportärende kan du hantera hello livscykeln för hello-biljett från hello-portalen.

#### <a name="toomanage-your-support-requests"></a>toomanage din support begär

1. tooget toohello hjälp och supportsida, navigera för**Bläddra > hjälp + support**.

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. En tabell lista över alla hello support begär visas i hello **hjälp + support** bladet.

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. Välj och klicka på en supportbegäran. Du kan visa hello status och hello information för denna begäran. Klicka på **+ nytt meddelande** om du vill att toofollow på denna begäran.

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

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


## <a name="next-steps"></a>Nästa steg

Lär dig hur för[diagnostisera och lösa problem relaterade tooyour StorSimple 8000-serieenhet](storsimple-troubleshoot-deployment.md)
