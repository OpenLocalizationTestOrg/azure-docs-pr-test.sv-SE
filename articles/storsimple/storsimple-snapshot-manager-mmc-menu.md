---
title: "aaaStorSimple Snapshot Manager MMC menyn Åtgärder | Microsoft Docs"
description: "Beskriver hur toouse hello standard Microsoft Management Console (MMC) menyn åtgärder i StorSimple Snapshot Manager."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 78ef81af-0d3a-4802-be54-ad192f9ac8a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: e7ba42cf2086c552bcc06b528abdead8fe4534d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-mmc-menu-actions-in-storsimple-snapshot-manager"></a>Använda hello MMC menyn åtgärder i StorSimple Snapshot Manager

## <a name="overview"></a>Översikt
StorSimple Snapshot Manager visas följande åtgärder visas på alla Åtgärdsmenyer och alla variationer av hello hello **åtgärder** fönstret.

* Visa
* Nytt fönster härifrån 
* Uppdatera 
* Exportera lista 
* Hjälp 

Dessa åtgärder är en del av hello Microsoft Management Console (MMC) och är inte specifik tooStorSimple Snapshot Manager. Den här självstudiekursen beskriver dessa åtgärder och förklarar hur toouse av dem i StorSimple Snapshot Manager.

## <a name="view"></a>Visa
Du kan använda hello **visa** alternativet toochange hello **resultat** vy och toochange hello fönstret konsolvyn. 

#### <a name="toochange-hello-results-pane-view"></a>vy för toochange hello resultat
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** , högerklicka på en nod eller expandera hello nod och högerklickar du i hello **resultat** rutan och klicka sedan på hello **visa** alternativet. 
3. tooadd eller ta bort hello-kolumner som visas i hello **resultat** rutan klickar du på **Lägg till/ta bort kolumner**. Hej **Lägg till/ta bort kolumner** dialogrutan visas.
   
    ![Lägg till eller ta bort kolumner från resultatfönstret](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Add_remove_columns.png) 
4. Fylla hello på följande sätt:
   
   * Välj objekt från hello **tillgänglig** kolumner och klicka på **Lägg till** tooadd dem toohello **visas kolumner** lista. 
   * Klicka på objekten i hello **visas kolumner** och på **ta bort** tooremove dem hello-listan. 
   * Markera ett objekt i hello **visas** kolumner och klicka på **Flytta upp** eller **Flytta ned** toomove hello objekt uppåt eller nedåt i listan hello. 
   * Klicka på **Återställ standardvärden** tooreturn toohello standard **resultat** fönstret konfiguration. 
5. När du är klar med dina val klickar du på **OK**. 

#### <a name="toochange-hello-console-window-view"></a>toochange hello konsolvyn fönster
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** fönstret, högerklicka på en nod, klicka på **visa**, och klicka sedan på **anpassa**. Hej **anpassa** dialogrutan visas.
   
    ![Anpassa hello konsolfönster](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Customize.png) 
3. Markera eller avmarkera hello kryssrutorna tooshow eller döljs i hello konsolfönstret. När du är klar med dina val klickar du på **OK**.

## <a name="new-window-from-here"></a>Nytt fönster härifrån
Du kan använda hello **nytt fönster härifrån** alternativet tooopen ett nytt konsolfönster.

#### <a name="tooopen-a-new-console-window"></a>tooopen ett nytt konsolfönster
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** , högerklicka på en nod, och klickar sedan på **nytt fönster härifrån**. 
   
    Ett nytt fönster visas med endast hello omfång som du har valt. Om du högerklickar på hello exempelvis **Säkerhetskopieringsprinciper** noden hello nytt fönster visas endast hello **principer för säkerhetskopiering** nod i hello **Scope** rutan och en lista över definierade Säkerhetskopiera principer i hello **resultat** fönstret. Se följande exempel hello.
   
    ![Nytt fönster härifrån](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_NewWindow.png) 

## <a name="refresh"></a>Uppdatera
Du kan använda hello **uppdatera** åtgärd tooupdate hello konsolfönstret.

#### <a name="tooupdate-hello-console-window"></a>tooupdate hello konsolfönster
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** , högerklicka på en nod eller expandera hello nod och högerklickar du i hello **resultat** rutan och klicka sedan på **uppdatera**. 

## <a name="export-list"></a>Exportera lista
Du kan använda hello **exportera listan** åtgärd toosave en lista i en fil med kommaavgränsade värden (CSV). Du kan till exempel exportera hello listan med principer för säkerhetskopiering eller hello säkerhetskopieringskatalogen. Du kan sedan importera hello CSV-fil i ett kalkylbladsprogram för analys.

#### <a name="toosave-a-list-in-a-comma-separated-value-csv-file"></a>toosave en lista i en fil med kommaavgränsade värden (CSV)
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager. 
2. I hello **omfång** , högerklicka på en nod eller expandera hello nod och högerklickar du i hello **resultat** rutan och klicka sedan på **exportera listan**. 
3. Hej **exportera listan** dialogrutan visas. Fylla hello på följande sätt: 
   
   1. I hello **filnamn** , Skriv ett namn för hello CSV-fil eller klicka på hello pilen tooselect hello nedrullningsbara listan.
   2. I hello **filformat** rutan, klickar du på pilen hello och välj en filtyp hello nedrullningsbara listan.
   3. toosave endast valda objekt, Välj hello rader och klicka sedan på hello **Spara endast markerade rader** kryssrutan. toosave alla exporterade listor, rensa hello **Spara endast markerade rader** kryssrutan.
   4. Klicka på **Spara**.
      
      ![Exportera listan som en CSV-fil](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Export_List.png) 

## <a name="help"></a>Hjälp
Du kan använda hello **hjälp** menyn tooview tillgängliga onlinehjälpen för StorSimple Snapshot Manager och hello MMC.

#### <a name="tooview-available-online-help"></a>hjälp för tooview tillgänglig online
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** , högerklicka på en nod eller expandera hello nod och högerklickar du i hello **resultat** rutan och klicka sedan på **hjälp**. 

## <a name="next-steps"></a>Nästa steg
* Mer information om hello [StorSimple Snapshot Manager användargränssnittet](storsimple-use-snapshot-manager.md).
* Lär dig mer om [med hjälp av StorSimple Snapshot Manager tooadminister StorSimple-lösningen](storsimple-snapshot-manager-admin.md).

