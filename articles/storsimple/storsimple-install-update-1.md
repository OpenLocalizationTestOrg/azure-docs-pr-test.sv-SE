---
title: "Installera uppdatering 1.2 på din StorSimple-enhet | Microsoft Docs"
description: "Beskriver hur du installerar StorSimple 8000 Series uppdatering 1.2 på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 7a513923-eb77-4078-b0ab-f8e90183796a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 80ff35cc47dfc38089f4c392ef4c90baf9ccc03e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-12-on-your-storsimple-8000-series-device"></a>Installera uppdatering 1.2 på enheten StorSimple 8000-serien
## <a name="overview"></a>Översikt
Den här självstudiekursen beskriver hur du installerar uppdateringen 1.2 på en StorSimple-enhet som kör en programvaruversion före uppdatering 1. Kursen omfattar också ytterligare steg som krävs för uppdateringen när en gateway har konfigurerats för ett nätverksgränssnitt än DATA 0 av StorSimple-enhet.

1.2 uppdateringen enheten programuppdateringar, LSI drivrutinsuppdateringar och uppdateringar av inbyggd disk. Programvara och LSI drivrutinsuppdateringar uppdateras utan avbrott och kan tillämpas via den klassiska Azure-portalen. Programvara disk störande uppdateringar och kan endast användas via Windows PowerShell-gränssnittet för enheten.

Beroende på vilken version av din enhet är igång och att du kan fastställa om uppdateringen 1.2 kommer att användas. Du kan kontrollera programvaruversionen på enheten genom att navigera till den **snabböversikten** avsnitt i enheten **instrumentpanelen**.

</br>

| Om du kör programvaruversion... | Vad som händer i portalen? |
| --- | --- |
| Version – GA |Om du kör versionen (GA) kan du inte installera denna uppdatering. Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) uppdatera din enhet. |
| Uppdatering 0.1 |Portalen gäller Update 1.2. |
| Uppdatering 0.2 |Portalen gäller Update 1.2. |
| Uppdatering 0.3 |Portalen gäller Update 1.2. |
| Uppdatering 1 |Den här uppdateringen är inte tillgänglig. |
| Uppdatera 1.1 |Den här uppdateringen är inte tillgänglig. |

</br>

> [!IMPORTANT]
> * Du kan inte se Update 1.2 direkt, eftersom vi gör en stegvis distribution av uppdateringar. Söker efter uppdateringar igen några dagar som den här uppdateringen blir tillgänglig snart.
> * Uppdateringen innehåller en uppsättning manuella och automatiska före kontrollerar hälsotillståndet för enheten vad gäller maskinvara tillstånd och nätverksanslutningen. Kontrollerna före utförs endast om du installerar uppdateringarna från den klassiska Azure-portalen.
> * Vi rekommenderar att du installerar uppdateringar för programvara eller drivrutiner via den klassiska Azure-portalen. Du bör bara gå till Windows PowerShell-gränssnittet för enheten (för att installera uppdateringar) om den före uppdateringen gateway inte i portalen. Uppdateringarna kan ta 5-10 timmar att installera (inklusive Windows-uppdateringar). Underhåll läge uppdateringar måste installeras via Windows PowerShell-gränssnittet för enheten. Eftersom Underhåll läge uppdateringar är störande uppdateringar, kommer dessa nertid för din enhet.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Installera uppdatering 1.2 via den klassiska Azure-portalen
Utför följande steg om du vill uppdatera enheten till [uppdatera 1.2](storsimple-update1-release-notes.md). Använd den här proceduren om du har en gateway som konfigurerats på DATA 0-nätverksgränssnittet på din enhet.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Kontrollera att enheten kör **StorSimple 8000 Series uppdatering 1.2 (6.3.9600.17584)**. Den **senast uppdaterad datum** bör också ändras. Du får också se att Underhåll läge uppdateringar är tillgängliga (det här meddelandet kan fortsätta att visas för upp till 24 timmar efter installation av uppdateringar).
   
   Underhåll läge uppdateringar är störande uppdateringar som leda till enheten driftstopp och kan endast användas via Windows PowerShell-gränssnittet på enheten.
   
   ![Underhållssidan](./media/storsimple-install-update-1/InstallUpdate12_10M.png "sidan Underhåll")
2. Hämta uppdateringar för underhåll-läge med hjälp av stegen i [att hämta snabbkorrigeringar](#to-download-hotfixes) att söka efter och hämta KB3063416, vilka installerar disk firmware-uppdateringar (andra uppdateringar ska vara installerad nu).
3. Följ stegen i [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes) för att installera underhållsläge uppdateringar.
4. I den klassiska Azure-portalen går du till den **Underhåll** och längst ned på sidan klickar du på **Sök efter uppdateringar** att söka efter alla Windows-uppdateringar och klicka sedan på **installera uppdateringar**. Du är klar när alla uppdateringar har installerats.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Installera uppdatering 1.2 på en enhet som har en gateway som konfigurerats för en icke-DATA 0-nätverksgränssnittet
Du bör använda den här proceduren endast om du inte gateway-kontroll vid försök att installera uppdateringar via den klassiska Azure-portalen. Det går inte att kontrollen som du har en gateway som tilldelats till en icke-DATA 0-nätverksgränssnittet och din enhet med en programvaruversion före uppdatering 1. Om enheten inte har en gateway på en icke-DATA 0-nätverksgränssnittet kan du uppdatera din enhet direkt från den klassiska Azure-portalen. Se [uppdatering 1.2 via den klassiska Azure-portalen](#install-update-1.2-via-the-azure-classic-portal).

Programvaruversioner som kan uppgraderas med den här metoden är uppdatering 0.1, uppdatering 0,2 och uppdatering 0.3.

> [!IMPORTANT]
> * Om enheten kör versionen (GA) version, kontakta [Microsoft-supporten](storsimple-contact-microsoft-support.md) som hjälper dig med uppdateringen.
> * Den här proceduren behöver bara utföras en gång för att tillämpa uppdateringen 1.2. Du kan använda den klassiska Azure-portalen för att tillämpa efterföljande uppdateringar.
> 
> 

Om enheten körs före uppdateringen 1 programvara och har en gateway som angetts för ett nätverksgränssnitt än DATA 0, kan du använda Update 1.2 på följande två sätt:

* **Alternativ 1**: ladda ned uppdateringen och använda den med hjälp av `Start-HcsHotfix` cmdlet från Windows PowerShell-gränssnittet för enheten. Det här är den rekommenderade metoden. **Använd inte den här metoden för att tillämpa uppdateringen 1.2 om enheten kör uppdatering 1.0 eller uppdatera 1.1.**
* **Alternativ 2**: ta bort gateway-konfigurationen och installera uppdateringen direkt från den klassiska Azure-portalen.

Detaljerade anvisningar för var och en av dessa finns i följande avsnitt.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Alternativ 1: Använd Windows PowerShell för StorSimple att tillämpa uppdateringen 1.2 som en snabbkorrigering
Du bör använda den här proceduren om du kör uppdatering 0.1, 0,2 0,3 och om din gateway-check misslyckades vid försök att installera uppdateringar från den klassiska Azure-portalen. Om du kör versionen (GA) programvara du [Microsoft-supporten](storsimple-contact-microsoft-support.md) uppdatera din enhet.

Om du vill installera en snabbkorrigering Update 1.2, måste du hämta och installera följande snabbkorrigeringar:

| Ordning | kB | Beskrivning | Typ av uppdatering |
| --- | --- | --- | --- |
| 1 |KB3063418 |Programuppdatering |Vanliga |
| 2 |KB3043005 |LSI SAS-styrenhet uppdatering |Vanliga |
| 3 |KB3063416 |Disk inbyggd programvara |Underhåll |

Innan du använder den här proceduren för att installera uppdateringen, kontrollera att:

* Båda styrenheter är online.

Utför följande steg för att tillämpa uppdateringen 1.2. **Uppdateringarna kan ta cirka 2 timmar för att slutföra (cirka 30 minuter för programvara, 30 minuter för drivrutin, 45 minuter för disk inbyggd programvara).**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Alternativ 2: Använda den klassiska Azure-portalen för att tillämpa uppdateringen 1.2 efter borttagning av gateway-konfiguration
Den här proceduren gäller bara för StorSimple-enheter som använder en programvaruversion före uppdatering 1 och har en gateway som angetts för ett nätverksgränssnitt än DATA 0. Du måste ta bort gateway-inställningen innan du kan uppdatera.

Uppdateringen kan ta några timmar att slutföra. Om dina värdar finns i olika undernät, kan borttagning av gateway-konfiguration på iSCSI-gränssnitt leda till driftstopp. Vi rekommenderar att du konfigurerar DATA 0 för iSCSI-trafik för att minska avbrottstiden.

Utför följande steg för att inaktivera nätverksgränssnitt med gatewayen och sedan installera uppdateringen.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nästa steg
Lär dig mer om den [Update 1.2 versionen](storsimple-update1-release-notes.md).

