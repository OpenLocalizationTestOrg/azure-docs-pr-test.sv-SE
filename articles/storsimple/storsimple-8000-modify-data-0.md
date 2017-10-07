---
title: "aaaModify DATA 0 på StorSimple 8000-serieenhet | Microsoft Docs"
description: "Lär dig hur toouse Windows PowerShell för StorSimple tooreconfigure hello DATA 0-nätverksgränssnittet på din StorSimple-enhet."
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
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 2d3bdd3675017be86def45a81d94b6c03fc61da6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a>Ändra hello DATA 0 gränssnitt för nätverksinställningar på enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt

Microsoft Azure StorSimple-enheten har sex nätverksgränssnitt från DATA 0 tooDATA 5. hello DATA 0 gränssnittet är alltid konfigurerad via Windows PowerShell-gränssnittet för hello eller hello seriekonsolen och är automatiskt moln-aktiverat. Observera att du kan konfigurera DATA 0-nätverksgränssnittet via hello Azure-portalen.

hello DATA 0 gränssnittet konfigureras först via hello installationsguiden under första distributionen av hello StorSimple-enhet. När hello enhet är i ett arbetsläge kan du behöva tooreconfigure DATA 0 inställningar. Den här kursen erbjuder två metoder toomodify DATA 0 nätverksinställningar, både via Windows PowerShell för StorSimple.

När du har läst den här självstudiekursen kommer du att kunna:

* Ändra DATA 0 nätverk via hello installationsguiden
* Ändra DATA 0 nätverksinställningar via hello `Set-HcsNetInterface` cmdlet

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Ändra DATA 0 nätverksinställningar via installationsguiden
Du kan konfigurera om DATA 0 nätverksinställningar genom att ansluta toohello Windows PowerShell-gränssnittet av StorSimple-enheten och starta en session för installationsprogrammet guiden. Utför följande steg toomodify DATA 0 hello inställningar:

#### <a name="toomodify-data-0-network-settings-through-setup-wizard"></a>toomodify DATA 0 nätverksinställningar via installationsguiden
1. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**. När du uppmanas ange hello **enhetens administratörslösenord**. hello standardlösenordet är `Password1`.
2. I hello kommandotolk, skriver du:
   
    `Invoke-HcsSetupWizard`
3. En installationsguide visas toohelp konfigurera hello DATA 0-gränssnittet på enheten. Ange nya värden för hello IP-adress, -gateway och nätmask.

> [!NOTE]
> hello fast domänkontrollanter IP-adresser måste konfigureras via hello toobe **nätverksinställningar** bladet för hello StorSimple-enheten i hello Azure-portalen. Mer information finns för[ändra nätverksgränssnitt](storsimple-8000-modify-device-config.md#modify-network-interfaces).

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Ändra DATA 0 nätverksinställningar via cmdlet Set-HcsNetInterface
Ett annat sätt tooreconfigure DATA 0-nätverksgränssnittet är via hello hello `Set-HcsNetInterface` cmdlet. hello cmdlet körs från hello Windows PowerShell-gränssnittet för din StorSimple-enhet. När du använder den här proceduren kan hello styrenhets-fästa IP-adresser även konfigureras här. Utför följande steg toomodify hello DATA 0 hello inställningar: 

#### <a name="toomodify-data-0-network-settings-through-hello-set-hcsnetinterface-cmdlet"></a>toomodify DATA 0 nätverksinställningar via hello Set-HcsNetInterface cmdlet
1. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**. När du uppmanas ange hello enhetens administratörslösenord. hello standardlösenordet är `Password1`.
2. I hello kommandotolk, skriver du:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Skriv följande värden för DATA 0 hello i hello vinkel hakparenteser:
   
   * IPv4-adress
   * IPv4-gateway
   * IPv4-nätmask
   * Fast IPv4-adressen för styrenhet 0
   * Fast IPv4-adressen för styrenhet 1
     
     Mer information om hello använder denna cmdlet finns för[Windows PowerShell för StorSimple cmdlet-referens](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Nästa steg
* tooconfigure nätverksgränssnitt än DATA 0, kan du använda hello [konfigurerar nätverksinställningar i hello Azure-portalen](storsimple-8000-modify-device-config.md). 
* Om du får problem när du konfigurerar din nätverksgränssnitt finns för[felsöka distributionsproblem](storsimple-troubleshoot-deployment.md).

