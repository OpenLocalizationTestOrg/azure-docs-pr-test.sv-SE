---
title: "läge för aaaChange StorSimple-enhet | Microsoft Docs"
description: "Beskriver hello StorSimple enheten lägen och förklarar hur toouse Windows PowerShell för StorSimple toochange hello enhet-läge."
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
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 058ca6cc38954bce3679cc21b39d341b10cb4dfb
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


### <a name="recovery-mode"></a>Återställningsläge

Återställningsläge kan beskrivas som ”Windows felsäkert läge med nätverksstöd”. Återställningsläge snabbt tillkallar hello Microsoft Support-teamet och låter dem tooperform diagnostik på hello system. hello primära målet för återställningsläge är tooretrieve hello systemloggar.

Om datorn försätts i återställningsläge, bör du kontakta Microsoft Support för nästa steg. Mer information finns för[kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md).

> [!NOTE]
> **Du kan inte placera hello enheten i återställningsläget. Om hello enheten är i ett dåligt tillstånd, försöker återställningsläge tooget hello enhet i ett tillstånd där Microsoft Support-personal undersöka den.**

## <a name="determine-storsimple-device-mode"></a>Identifiera läge för StorSimple-enhet

#### <a name="toodetermine-hello-current-device-mode"></a>toodetermine hello aktuell enhet-läge

1. Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. Titta på hello Banderollmeddelandet i menyn för seriekonsolen hello av hello enhet. Det här meddelandet anger uttryckligen om hello enheten är i läget underhåll eller återställning. Om hello-meddelande inte innehåller någon särskild information som rör toohello system läge, är hello-enheten i normalläge.

## <a name="change-hello-storsimple-device-mode"></a>Ändra läge för hello StorSimple-enhet

Du kan placera hello StorSimple-enhet i underhållsläge läge (från normalläge) tooperform underhåll eller installera uppdateringar för underhåll lägen. Utför följande procedurer tooenter eller avsluta underhållsläget hello.

> [!IMPORTANT]
> Innan du anger underhållsläge, kontrollera att båda styrenheter är felfri genom att öppna hello **Enhetsinställningar > maskinvara hälsa** för din enhet i hello Azure-portalen. Om en eller båda hello domänkontrollanter inte är felfri, kan du kontakta Microsofts Support för hello nästa steg. Mer information finns för[kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md).
 

#### <a name="tooenter-maintenance-mode"></a>tooenter underhållsläge

1. Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
2. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**. När du uppmanas att ange hello **enhetens administratörslösenord**. hello standardlösenordet är: `Password1`.
3. I hello kommandotolk, skriver du: 
   
    `Enter-HcsMaintenanceMode`
4. Visas ett varningsmeddelande som talar om att underhållsläge ska störa alla i/o-begäranden och sever hello anslutning toohello Azure-portalen och du uppmanas att bekräfta. Typen **Y** tooenter underhållsläge.
5. Både domänkontrollanter startas om. När hello omstarten är klar, visar hello seriekonsolen banderoll hello enheten är i underhållsläge. Ett exempel på utdata visas nedan.

```
    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Controller0>Enter-HcsMaintenanceMode
    Checking device state...

    In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Passive
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>

```

#### <a name="tooexit-maintenance-mode"></a>tooexit underhållsläge

1. Logga in toohello enhetens seriekonsol. Kontrollera från hello Banderollmeddelandet som enheten är i underhållsläge.
2. I hello kommandotolk, skriver du:
   
    `Exit-HcsMaintenanceMode`
3. Ett varningsmeddelande och ett meddelande visas. Typen **Y** tooexit underhållsläge.
4. Både domänkontrollanter startas om. När hello omstarten är klar, visar hello seriekonsolen banderoll hello enheten är i normalläge. Ett exempel på utdata visas nedan.

```
    -----------------------MAINTENANCE MODE------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0
    ---------------------------------------------------------------

    Controller0>Exit-HcsMaintenanceMode
    Checking device state...

    Before exiting maintenance mode, ensure that any updates that are required on both controllers have been applied. Failure tooinstall on each controller could result in data corruption. Exiting maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooexit maintenance mode?
    [Y] Yes [N] No (Default is "Y"): Y

    <BOTH CONTROLLERS RESTART>

    ---------------------------------------------------------------
    Microsoft Azure StorSimple Appliance Model 8100
    Name: 8100-SHX0991003G44MT
    Software Version: 6.3.9600.17820
    Copyright (C) 2014 Microsoft Corporation. All rights reserved.
    You are connected tooController0 - Active
    ---------------------------------------------------------------

    Serial Console Menu
    [1] Log in with full access
    [2] Log into peer controller with full access
    [3] Connect with limited access
    [4] Change language
    Please enter your choice>
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[tillämpa uppdateringar för normal och underhåll läge](storsimple-update-device.md) på StorSimple-enheten.

