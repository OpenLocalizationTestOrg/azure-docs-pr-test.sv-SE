---
title: "aaaInstall uppdateringar på virtuella StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hur toouse hello virtuella StorSimple-matris web UI tooapply uppdateringar med hjälp av hello Azure portal och snabbkorrigeringen metod"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: 6165b305fb0d404b404cf3a11dd0ade2f2f13b2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a>Installera uppdateringen 0,4 på din virtuella StorSimple-matris

## <a name="overview"></a>Översikt

Den här artikeln beskriver hello steg krävs tooinstall uppdatering 0,4 på din StorSimple virtuell matrisen via hello lokala webbgränssnittet och via hello Azure-portalen. Du måste tooapply programvara uppdateringar eller snabbkorrigeringar tookeep din virtuella StorSimple-matris uppdaterade. 

Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten. Anges att hello virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp. 

Innan du installerar en uppdatering rekommenderar vi att du vidta hello volymer eller resurser offline på hello värd först och sedan hello enhet. Detta minimerar möjlighet att data skadas.

> [!IMPORTANT]
> Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda hello snabbkorrigeringen metoden via hello lokala web UI tooinstall uppdatering 0.3. Om du kör uppdatering 0,2 eller senare, rekommenderar vi att installerar du hello uppdateringar via hello Azure-portalen.
 

## <a name="use-hello-local-web-ui"></a>Använd hello lokala webbgränssnittet

Det finns två steg när du använder hello lokala webbgränssnittet:

* Hämta hello uppdatering eller snabbkorrigering för hello
* Installera hello uppdatering eller snabbkorrigering för hello

### <a name="download-hello-update-or-hello-hotfix"></a>Hämta hello uppdatering eller snabbkorrigering för hello

Utför följande steg toodownload hello programuppdateringen från hello Microsoft Update-katalogen hello.

#### <a name="toodownload-hello-update-or-hello-hotfix"></a>toodownload hello uppdatering eller hello snabbkorrigering

1. Starta Internet Explorer och navigera för[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Om det här är första gången du använder hello Microsoft Update-katalogen på den här datorn klickar du på **installera** när begärd tooinstall hello tillägg för Microsoft Update-katalogen.

3. Ange hello Knowledge Base (KB) nummer i sökrutan för hello Microsoft Update-katalogen hello hello snabbkorrigering som du vill toodownload. Ange **3216577** för Update 0,4 och klicka sedan på **Sök**.
   
    hello snabbkorrigeringen visas, till exempel **StorSimple virtuell matris uppdatering 0,4**.
   
    ![Sökkatalog](./media/storsimple-virtual-array-install-update-04/download1.png)

4. Klicka på **Lägg till**. hello uppdateringen läggs toohello korg.

5. Klicka på **View Basket** (Visa korg).

6. Klicka på **Hämta**. Ange eller **Bläddra** tooa lokal plats där du vill hello hämtar tooappear. hello uppdateringarna laddas toohello angav plats och placeras i en undermapp med samma namn som hello uppdatering hello. hello-mappen kan också vara kopierade tooa nätverksresurs som kan nås från hello enhet.

7. Öppna hello kopierade mappen, bör du se en paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`. Den här filen är används tooinstall hello uppdatering eller snabbkorrigering.

### <a name="install-hello-update-or-hello-hotfix"></a>Installera hello uppdatering eller snabbkorrigering för hello

Tidigare toohello uppdatering eller snabbkorrigering installation, kontrollera att du har hello uppdatering eller hello snabbkorrigering hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs. 

Använd den här metoden tooinstall uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner. Den här proceduren tar mindre än 2 minuter toocomplete. Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a>tooinstall hello uppdatering eller snabbkorrigering hello

1. Hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**.
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update1m.png)

2. I **uppdatering filsökväg**, ange hello filnamn för hello uppdatering eller hello snabbkorrigering. Du kan även bläddra toohello uppdatering eller snabbkorrigering installationsfilen om placeras på en nätverksresurs. Klicka på **Använd**.
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update2m.png)

3. En varning visas. Angivna detta är en enskild nod-enhet när hello uppdateringen har genomförts, hello enheten startas om och det finns en avbrottstid. Klicka på kryssikonen hello.
   
   ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update3m.png)

4. hello uppdateringen startar. När hello enheten har uppdaterats, startas om. hello är lokala Användargränssnittet inte tillgänglig i den här tiden.
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update5m.png)

5. När hello omstarten är klar, tas du toohello **inloggning** sidan. tooverify som hello enhetens programvara har uppdaterats i hello lokala webbgränssnittet, gå för**Underhåll** > **programuppdateringen**. hello visas programvaruversionen ska vara **10.0.0.0.0.10289.0** för uppdatering 0,4.
   
   > [!NOTE]
   > Vi rapporterar hello programvaruversioner på något annat sätt i hello lokala webbgränssnittet och hello Azure-portalen. Till exempel hello lokala UI-webbrapporter **10.0.0.0.0.10289** och hello Azure portal rapporter **10.0.10289.0** för hello samma version.
   
    ![uppdatera enhet](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-hello-azure-portal"></a>Använd hello Azure-portalen

Om du kör uppdatering 0,2 och senare, rekommenderar vi att du installerar uppdateringar via hello Azure-portalen. hello portal proceduren kräver hello användaren tooscan, hämta och installera hello uppdateringar. Den här proceduren tar cirka 7 minuter toocomplete. Utföra hello följande steg tooinstall hello uppdatering eller snabbkorrigering.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Efter hello är-installationen klar (som visas jobbets status till 100%), gå tooyour StorSimple enheten Manager-tjänsten. Välj **enheter** och markera och på hello enheten tooupdate hello listan över enheter anslutna toothis service. I hello **inställningar** bladet går för**hantera** avsnittet och väljer **enhetsuppdateringar**. hello visas programvaruversionen ska vara **10.0.10289.0**.


## <a name="next-steps"></a>Nästa steg

Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

