---
title: "Installera uppdateringar på en virtuell StorSimple-matris | Microsoft Docs"
description: "Beskriver hur du använder virtuella StorSimple-matris webbgränssnittet för att tillämpa uppdateringar med hjälp av metoden portal och snabbkorrigering"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7f1b1e7d-04d0-4ca2-9dbb-77077ff19bb9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/07/2016
ms.author: alkohli
ms.openlocfilehash: bccb0d49c1959a690d513961c32d946763385a87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array"></a>Installera uppdateringar på din virtuella StorSimple-matris
## <a name="overview"></a>Översikt
Den här artikeln beskriver de steg som krävs för att installera uppdateringar på din StorSimple virtuell matrisen via lokala webbgränssnittet och via den klassiska Azure-portalen. Du måste tillämpa uppdateringar eller snabbkorrigeringar för att hålla det uppdaterat din virtuella StorSimple-matris. 

Tänk på att installera en uppdatering eller snabbkorrigering startar om enheten. Med hänsyn till att den virtuella StorSimple-matrisen är en enskild nod-enhet, avbryts alla i/o pågår och din enhet upplever driftstopp. 

Innan du installerar en uppdatering, rekommenderar vi att du vidtar volymer eller resurser offline på värden först och sedan enheten. Detta minimerar möjlighet att data skadas.

> [!IMPORTANT]
> Om du kör uppdatering 0.1 eller GA programvaruversioner, måste du använda metoden snabbkorrigeringen via lokala webbgränssnittet för att installera uppdatering 0.3. Om du kör uppdatering 0,2 rekommenderar vi att du installerar uppdateringar via den klassiska Azure-portalen.
> 
> 

## <a name="use-the-local-web-ui"></a>Använda lokala webbgränssnittet
Det finns två steg när du använder lokala webbgränssnittet:

* Hämta uppdateringen eller snabbkorrigeringen
* Installera uppdateringen eller snabbkorrigeringen

### <a name="download-the-update-or-the-hotfix"></a>Hämta uppdateringen eller snabbkorrigeringen
Utför följande steg för att hämta programuppdateringen från Microsoft Update Catalog.

#### <a name="to-download-the-update-or-the-hotfix"></a>Hämta uppdateringen eller snabbkorrigeringen
1. Starta Internet Explorer och gå till [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Om det här är första gången du använder Microsoft Update Catalog på den här datorn klickar du på **Installera** när du uppmanas att installera tillägget för Microsoft Update Catalog.
3. Ange antalet den snabbkorrigering som du vill hämta Knowledge Base (KB) i sökrutan i Microsoft Update-katalogen. Ange **3182061** uppdatering 0.3 för och klicka sedan på **Sök**.
   
    Snabbkorrigeringen listan visas till exempel **StorSimple virtuell matris uppdatering 0.3**.
   
    ![Sökkatalog](./media/storsimple-ova-install-update-01/download1.png)
4. Klicka på **Lägg till**. Uppdateringen läggs till i korgen.
5. Klicka på **View Basket** (Visa korg).
6. Klicka på **Hämta**. Ange eller **Bläddra** till en lokal plats där du vill att nedladdningarna ska läggas. Uppdateringarna hämtas till den angivna platsen och placeras i en undermapp med samma namn som uppdateringen. Mappen kan också kopieras till en nätverksresurs som kan nås från enheten.
7. Öppna mappen kopierade, bör du se en paketfil för Microsoft Update fristående `WindowsTH-KB3011067-x64`. Den här filen används för att installera uppdatering eller snabbkorrigering.

### <a name="install-the-update-or-the-hotfix"></a>Installera uppdateringen eller snabbkorrigeringen
Kontrollera att du har uppdateringen eller snabbkorrigeringen hämtas antingen lokalt på värden eller tillgängligt via en nätverksresurs före installationen uppdatering eller snabbkorrigering. 

Använd den här metoden för att installera uppdateringar på en enhet som kör GA eller uppdatera 0,1 programvaruversioner. Den här proceduren tar mindre än 2 minuter att slutföra. Utför följande steg för att installera uppdatering eller snabbkorrigering.

#### <a name="to-install-the-update-or-the-hotfix"></a>Så här installerar du uppdateringen eller snabbkorrigeringen
1. Gå till i det lokala webbgränssnittet **Underhåll** > **programuppdateringen**.
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update1m.png)
2. I **uppdatering filsökväg**, ange filnamnet för uppdateringen eller snabbkorrigeringen. Du kan även bläddra till installationsfilen uppdatering eller snabbkorrigering om placeras på en nätverksresurs. Klicka på **Använd**.
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update2m.png)
3. En varning visas. Angivna detta är en enskild nod-enhet när uppdateringen har genomförts, enheten startas om och det finns en avbrottstid. Klicka på kryssikonen.
   
   ![uppdatera enhet](./media/storsimple-ova-install-update-01/update3m.png)
4. Uppdateringen startar. När enheten har uppdaterats, startas om. Lokala Användargränssnittet är inte tillgängligt i den här tiden.
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update5m.png)
5. När omstarten är klar, kommer du till den **inloggning** sidan. Kontrollera att enhetens programvara har uppdaterats i lokala webbgränssnittet, gå till **Underhåll** > **programuppdateringen**. Programvaruversionen visas ska vara **10.0.0.0.0.10288.0** för uppdatering 0.3.
   
   > [!NOTE]
   > Vi rapporterar programvaruversioner i ett lite annorlunda sätt i lokala webbgränssnittet och den klassiska Azure-portalen. Till exempel lokala webbgränssnittet rapporterar **10.0.0.0.0.10288** och Azure klassiska portal rapporter **10.0.10288.0** för samma version. 
   > 
   > 
   
    ![uppdatera enhet](./media/storsimple-ova-install-update-01/update6m.png)

## <a name="use-the-azure-classic-portal"></a>Använd den klassiska Azure-portalen
Om du kör uppdatering 0,2, rekommenderar vi att du installerar uppdateringar via den klassiska Azure-portalen. Portalen proceduren kräver att användaren skanna, hämta och installera uppdateringarna. Den här proceduren tar cirka 7 minuter för att slutföra. Utför följande steg för att installera uppdatering eller snabbkorrigering.

[!INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

När installationen är klar (som visas jobbets status till 100%), går du till **enheter > Underhåll > programuppdateringar**. Den visa programvaruversionen ska 10.0.10288.0.

![uppdatera enhet](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

