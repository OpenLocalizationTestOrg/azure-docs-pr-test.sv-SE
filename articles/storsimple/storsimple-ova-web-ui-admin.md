---
title: aaaStorSimple virtuella matris web UI administration | Microsoft Docs
description: "Beskriver hur tooperform grundläggande enhet administrativa uppgifter via hello virtuella StorSimple-matris webbgränssnittet."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 31a20a587c4302231f027fcf772a50df33b23407
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-web-ui-tooadminister-your-storsimple-virtual-array"></a>Använd hello Webbgränssnittet tooadminister din virtuella StorSimple-matris
![processflöde för installationen](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Översikt
hello självstudiekurser i den här artikeln gäller toohello Microsoft Azure StorSimple virtuell matris (även kallat hello lokala virtuella enheten StorSimple) körs mars 2016 allmän tillgänglighet (GA) versionen. Den här artikeln beskrivs några av hello komplexa arbetsflöden och administrativa uppgifter som kan utföras på hello virtuella StorSimple-matris. Du kan hantera hello virtuella StorSimple-matris med hello StorSimple Manager-tjänsten Användargränssnittet (refererad tooas hello portalens användargränssnitt) och hello lokala webbgränssnittet för hello enhet. Den här artikeln fokuserar på hello aktiviteter som du kan utföra med hello webbgränssnittet.

Den här artikeln innehåller hello följande kurser:

* Hämta krypteringsnyckel för tjänstdata hello
* Felsöka web UI installationen
* Generera en logg-paket
* Stänga av eller starta om enheten

## <a name="get-hello-service-data-encryption-key"></a>Hämta krypteringsnyckel för tjänstdata hello
En krypteringsnyckel för tjänstdata ska genereras när du registrerar din första enhet med hello StorSimple Manager-tjänsten. Den här nyckeln är sedan krävs med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple Manager-tjänsten.

Om du har tappat bort din krypteringsnyckel för tjänstdata och behöver tooretrieve, utföra hello följande steg i hello lokala webbgränssnittet för hello enheten registreras med din tjänst.

#### <a name="tooget-hello-service-data-encryption-key"></a>tooget hello krypteringsnyckel för tjänstdata
1. Ansluta toohello lokala webbgränssnittet. Gå för**Configuration** > **Molninställningar**.
2. Hello längst hello-sidan, klickar du på **hämta krypteringsnyckel för tjänstdata**. En nyckel visas. Kopiera och spara den här nyckeln.
   
    ![Hämta krypteringsnyckel för tjänstdata 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Felsöka web UI installationen
I vissa fall när du konfigurerar hello enheten via hello lokala webbgränssnittet, kanske du stöter på fel. toodiagnose och felsökning av dessa fel kan du köra hello diagnostiktest.

#### <a name="toorun-hello-diagnostic-tests"></a>toorun hello diagnostiktest
1. Hello lokala webbgränssnittet, gå för**felsökning** > **diagnostiska testerna**.
   
    ![Kör diagnostik 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. Hello längst hello-sidan, klickar du på **kör diagnostiktest**. Detta initierar testerna toodiagnose möjliga problem med ditt nätverk, enhet, webbproxy, tid eller molninställningar. Du meddelas att hello-enheten kör test.
3. Hello resultat visas när hello testerna har slutförts. hello visar följande exempel hello resultaten av diagnostiska test. Observera att hello webbproxyinställningarna inte har konfigurerats på den här enheten och därför hello web proxy testet kördes inte. Alla hello andra tester för nätverksinställningar, DNS-server och tidsinställningar lyckades.
   
    ![Kör diagnostik 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Generera en logg-paket
En logg paketet består av alla relevanta hello-loggar som kan hjälpa Microsoft-supporten att felsöka eventuella enhetsproblem. I den här versionen kan en logg-paketet skapas via hello lokala webbgränssnittet.

#### <a name="toogenerate-hello-log-package"></a>toogenerate hello loggen paketet
1. Hello lokala webbgränssnittet, gå för**felsökning** > **systemloggar**.
   
    ![Generera loggen paketet 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. Hello längst hello-sidan, klickar du på **skapa loggen paketet**. Ett paket med hello systemloggar kommer att skapas. Det kan ta några minuter.
   
    ![Generera loggen paketet 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    Du meddelas när hello paketet har skapats och hello sidan uppdateras tooindicate hello datum och tid då hello paketet skapades.
   
    ![Generera loggen paketet 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. Klicka på **loggen hämtningspaketet**. Komprimerade paketet kommer att hämtas i systemet.
   
    ![Generera loggen paketet 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. Du kan packa hello hämtade loggen paketet och visa hello systemloggfilerna.

## <a name="shut-down-and-restart-your-device"></a>Stäng och starta om enheten
Du kan stänga av eller starta om din virtuella enhet använder hello lokala webbgränssnittet. Vi rekommenderar att innan du startar om ta hello volymer eller resurser offline på hello värden och hello enhet. Detta minimerar risken för varje möjlighet att data skadas. 

#### <a name="tooshut-down-your-virtual-device"></a>tooshut ned din virtuella enhet
1. Hello lokala webbgränssnittet, gå för**Underhåll** > **energiinställningar**.
2. Hello längst hello-sidan, klickar du på **avstängning**.
   
    ![enheten avstängning 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. En varning visas om att stängas av hello enheten kommer att avbryta alla i/o som pågår, vilket resulterar i ett driftstopp. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![enheten avstängning varning](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Du meddelas att hello avsluta har initierats.
   
    ![enheten avstängning igång](./media/storsimple-ova-web-ui-admin/image38.png)
   
    hello enheten stängs nu. Om du vill toostart enheten behöver toodo som via hello Hyper-V Manager.

#### <a name="toorestart-your-virtual-device"></a>toorestart din virtuella enhet
1. Hello lokala webbgränssnittet, gå för**Underhåll** > **energiinställningar**.
2. Hello längst hello-sidan, klickar du på **starta om**.
   
    ![omstart av enheten](./media/storsimple-ova-web-ui-admin/image36.png)
3. En varning visas om att starta om hello enheten kommer att avbryta alla IOs som pågår, vilket resulterar i ett driftstopp. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Starta om varning](./media/storsimple-ova-web-ui-admin/image37.png)
   
    Du meddelas att hello omstart har initierats.
   
    ![Starta om initieras](./media/storsimple-ova-web-ui-admin/image39.png)
   
    När hello omstart pågår förlorar hello anslutning toohello Användargränssnittet. Du kan övervaka hello omstart genom att uppdatera hello användargränssnitt med jämna mellanrum. Du kan också övervaka hello omstart Enhetsstatus via hello Hyper-V Manager.

## <a name="next-steps"></a>Nästa steg
Lär dig hur för[Använd hello StorSimple Manager service toomanage enheten](storsimple-virtual-array-manager-service-administration.md).

