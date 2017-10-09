---
title: "aaaChange hello StorSimple Enhetsläge | Microsoft Docs"
description: "Beskriver hello StorSimple enheten lägen och förklarar hur toouse Windows PowerShell för StorSimple toochange hello enhet-läge."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 299fd380a83bcd06780c97937f4064f0791b440d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-device-mode-on-your-storsimple-device"></a>Ändra hello Enhetsläge på din StorSimple-enhet
Den här artikeln innehåller en kort beskrivning av hello respektive läge där din StorSimple-enhet fungerar. Din StorSimple-enhet kan fungera i tre lägen: normal, underhåll och återställning. 

När du har läst den här artikeln, vet du:

* Hej StorSimple enheten lägen är
* Hur toofigure i vilket läge hello StorSimple-enheten är i
* Hur toochange från normala toomaintenance läge och *tvärtom*

hello ovan hanteringsuppgifter kan endast utföras via Windows PowerShell-gränssnittet för hello av StorSimple-enheten.

## <a name="about-storsimple-device-modes"></a>Om lägen för StorSimple-enhet
Din StorSimple-enhet kan köras i läget normal, underhåll eller återställning. Var och en av dessa lägen kortfattat beskrivs nedan.

### <a name="normal-mode"></a>Normalt läge
Detta definieras som hello operativa normalläge för en helt konfigurerade StorSimple-enhet. Som standard måste enheten vara i normalläge.

### <a name="maintenance-mode"></a>Underhållsläge
Ibland behöver hello StorSimple-enhet toobe placeras i underhållsläge. Det här läget kan du tooperform Underhåll på hello enhet och installera störande uppdateringar som de relaterade toodisk inbyggd programvara.

Du kan placera hello system i underhållsläge endast via hello Windows PowerShell för StorSimple. Alla i/o-begäranden är pausade i det här läget. Tjänster, till exempel beständiga RAM-minne (NVRAM) eller hello klustertjänsten också har stoppats. Båda hello domänkontrollanter startas om när du anger eller avsluta det här läget. När du avslutar hello underhållsläge återupptas alla hello-tjänster och bör vara felfritt. Det kan ta några minuter.

> [!NOTE]
> **Underhållsläge stöds bara på en korrekt fungerande enhet. Den stöds inte på en enhet där en eller båda av hello domänkontrollanter inte fungerar.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Återställningsläge
Återställningsläge kan beskrivas som ”Windows felsäkert läge med nätverksstöd”. Återställningsläge snabbt tillkallar hello Microsoft Support-teamet och låter dem tooperform diagnostik på hello system. hello primära målet för återställningsläge är tooretrieve hello systemloggar.

Om datorn försätts i återställningsläge, bör du kontakta Microsoft Support för nästa steg. Mer information finns för[kontakta Microsoft Support](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **Du kan inte placera hello enheten i återställningsläget. Om hello enheten är i ett dåligt tillstånd, försöker återställningsläge tooget hello enhet i ett tillstånd där Microsoft Support-personal undersöka den.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Identifiera läge för StorSimple-enhet
#### <a name="toodetermine-hello-current-device-mode"></a>toodetermine hello aktuell enhet-läge
1. Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Titta på hello Banderollmeddelandet i menyn för seriekonsolen hello av hello enhet. Det här meddelandet anger uttryckligen om hello enheten är i läget underhåll eller återställning. Om hello-meddelande inte innehåller någon särskild information som rör toohello system läge, är hello-enheten i normalläge.

## <a name="change-hello-storsimple-device-mode"></a>Ändra läge för hello StorSimple-enhet
Du kan placera hello StorSimple-enhet i underhållsläge läge (från normalläge) tooperform underhåll eller installera uppdateringar för underhåll lägen. Utför följande procedurer tooenter eller avsluta underhållsläget hello.

> [!IMPORTANT]
> Innan du anger underhållsläge, kontrollera att båda styrenheter är felfri genom att öppna hello **maskinvarustatus** på hello **Underhåll** sida i hello klassiska Azure-portalen. Om en eller båda hello domänkontrollanter inte är felfri, kan du kontakta Microsofts Support för hello nästa steg. Mer information finns för[kontakta Microsoft Support](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="tooenter-maintenance-mode"></a>tooenter underhållsläge
1. Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**. När du uppmanas att ange hello **enhetens administratörslösenord**. hello standardlösenordet är: `Password1`.
3. I hello kommandotolk, skriver du: 
   
    `Enter-HcsMaintenanceMode`
4. Visas ett varningsmeddelande som talar om att underhållsläge ska störa alla i/o-begäranden och sever hello anslutning toohello klassiska Azure-portalen och du uppmanas att bekräfta. Typen **Y** tooenter underhållsläge.
5. Både domänkontrollanter startas om. När hello omstarten är klar, visas ett annat meddelande som anger att hello-enheten är i underhållsläge.

#### <a name="tooexit-maintenance-mode"></a>tooexit underhållsläge
1. Logga in toohello enhetens seriekonsol. Kontrollera från hello Banderollmeddelandet som enheten är i underhållsläge.
2. I hello kommandotolk, skriver du:
   
    `Exit-HcsMaintenanceMode`
3. Ett varningsmeddelande och ett meddelande visas. Typen **Y** tooexit underhållsläge.
4. Både domänkontrollanter startas om. När hello omstarten är klar, visas ett annat meddelande som anger att hello-enheten är i normalläge.

## <a name="next-steps"></a>Nästa steg
Lär dig hur för[tillämpa uppdateringar för normal och underhåll läge](storsimple-update-device.md) på StorSimple-enheten.

