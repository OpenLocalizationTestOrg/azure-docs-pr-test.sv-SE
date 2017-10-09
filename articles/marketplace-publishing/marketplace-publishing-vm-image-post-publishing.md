---
title: aaaManaging som den virtuella datorn bild i hello Azure Marketplace | Microsoft Docs
description: "Detaljerad vägledning om hur toomanage din virtuella dator bild i hello Azure Marketplace efter första publicering"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: 47a082e686e1248ceacb844d3c0f2f5c33133dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="post-production-guide-for-virtual-machine-offers-in-hello-azure-marketplace"></a>Släppts guiden för den virtuella datorn är i hello Azure Marketplace
Den här artikeln förklarar hur du kan uppdatera en Direktmigrering av virtuell dator erbjudandet i hello Azure Marketplace. Det hjälper dig att hello processen att lägga till en eller flera nya SKU: er tooan befintliga erbjudandet. Dessutom hjälper dig att hello processen för att ta bort ett erbjudande för Direktmigrering av virtuell dator eller SKU från hello Marketplace.

När ett erbjudande/SKU mellanlagras i hello [Azure-portalen](http://portal.azure.com), du kan inte ändra hello följande rutor:

* **Erbjuder identifierare**: hello publicering portal, gå för**virtuella datorer** och välj erbjudandet. Klicka på **VM-AVBILDNINGAR** > **erbjuder identifierare**.
* **SKU-ID**: hello publicering portal, gå för**virtuella datorer** och välj erbjudandet. Klicka på **SKU: er** > **lägga till en SKU**.
* **Publisher Namespace**: hello publicering portal, gå för**virtuella datorer** > **genomgången** > **berätta för oss om ditt företag** (finns under ”steg 2 registrera företagsnamn”) > **Publisher Namespace** > **Namespace**.

När hello erbjudandet/SKU har listats i hello [Marketplace](http://azure.microsoft.com/marketplace), du kan inte ändra hello följande rutor:

* **Erbjuder identifierare**: hello publicering portal, gå för**virtuella datorer** och välj erbjudandet. Klicka på **VM-AVBILDNINGAR** > **erbjuder identifierare**.
* **SKU-ID**: hello publicering portal, gå för**virtuella datorer** och välj erbjudandet. Klicka på **SKU: er** > **lägga till en SKU**.
* **Publisher Namespace**: hello publicering portal, gå för**virtuella datorer** > **genomgången** > **berätta för oss om ditt företag** (finns under ”steg 2 registrera”) **Publisher Namespace** > **Namespace**.
* **Portar**: hello publicering portal, gå för**virtuella datorer** och välj erbjudandet. Klicka på **VM-AVBILDNINGAR** > **öppna portar**.
* **Priser för ändring av listan SKU(s)**
* **Fakturering modellen ändring av listan SKU(s)**
* **Borttagning av fakturering områden i listan SKU(s)**
* **Ändra hello data disk antal listade SKU(s)**

## <a name="update-hello-technical-details-of-a-sku"></a>Uppdatera hello teknisk information om en SKU
tooadd en ny version toohello visas SKU och publicera erbjudandet, gör du följande:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **VM-AVBILDNINGAR** fliken.
4. I hello **SKU: er** avsnittet, leta upp hello SKU som du vill tooupdate.
5. Lägg till ett nytt versionsnummer för hello SKU: N och klickar på hello  **+**  knappen. nya hello-versionen måste vara i ett X.Y.Z format, där X, Y och Z är heltal. Bara ska version ändringar inkrementell.
6. I hello **OS VHD URL** anger signatur för delad åtkomst hello URI som skapats för hello operativsystemet VHD och spara hello ändringar.

   > [!IMPORTANT]
   > Du kan inte öka/minska hello datadiskar för en listade SKU: N. Du måste toocreate nya SKU i det här fallet. Detaljerad information finns i avsnittet toohello [lägga till en ny SKU under listan erbjudande](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![VM-avbildningar](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-hello-nontechnical-details-of-an-offer-or-a-sku"></a>Uppdatera hello sökassistent information om ett erbjudande eller en SKU
Du kan uppdatera hello sökassistent (marknadsföring, juridiska, support, kategorier) information om din live erbjudande eller SKU i hello Marketplace.

### <a name="update-hello-offer-description-and-logos"></a>Uppdatera hello erbjudande beskrivning och logotyper
tooupdate hello erbjudandeinformationen och publicera erbjudandet, gör du följande:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **MARKNADSFÖRING** fliken.
4. Klicka på **engelska (USA)**.
5. Klicka på hello **information** fliken. I hello **beskrivning** avsnitt, uppdatering hello erbjudande **rubrik**, **sammanfattning**, och **LÅNG sammanfattning** och spara hello ändringar.

   > [!NOTE]
   > När du uppdaterar hello SKU information vara medveten om dessa begränsningar: 
   * Ange inte dubbla text för hello erbjudande beskrivning och hello SKU beskrivning.
   * Ange inte dubbla text för hello SKU rubrik och hello erbjudande lång sammanfattning. 
   * Ange inte dubbla text för hello SKU rubrik och hello erbjudande sammanfattning.
   >
   >

    ![Information](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. I hello **logotyper** avsnitt i hello **information** uppdaterar hello logotyper. Se till att hello logotyper följer hello [riktlinjer för Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > Ikon för hjälte är valfritt. Du kan välja inte tooupload en hjälte ikon. När en ikon i hjälte har överförts, det finns dock ingen etablera toodelete från hello publicering av portalen. Följ hello [hjälte ikonen riktlinjer](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Logotyper](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-hello-sku-description"></a>Uppdatera hello SKU beskrivning
tooupdate hello SKU information och publicera erbjudandet, Följ dessa steg:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **MARKNADSFÖRING** fliken.
4. Klicka på **engelska (USA)**.
5. Klicka på hello **planer** fliken. I hello **SKU: er** avsnittet, uppdatera hello SKU **rubrik**, **sammanfattning**, och **beskrivning** och spara hello ändringar.

   > [!NOTE]
   > När du uppdaterar hello SKU information vara medveten om dessa begränsningar: 
   * Ange inte dubbla text för hello erbjudande beskrivning och hello SKU beskrivning. 
   * Ange inte dubbla text för hello SKU rubrik och hello erbjudande lång sammanfattning. 
   * Ange inte dubbla text för hello SKU rubrik och hello erbjudande sammanfattning.
   >
   >
6. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Planer](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Ändra befintliga länkar eller lägga till nya länkar
toochange befintliga länkar eller lägga till nya länkar och publicera erbjudandet, gör du följande:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **MARKNADSFÖRING** fliken.
4. Klicka på **engelska (USA)**.
5. Klicka på hello **länkar** fliken.
6. tooadd en ny länk i hello **länkar** klickar du på **+ Lägg till länk**. I hello **lägga till länken** dialogrutan Ange hello länken **rubrik** och **URL** och spara hello ändringar. Du kan ange en länk som innehåller information som kan hjälpa kunderna.
7. tooupdate eller ta bort en befintlig länk väljer hello länk och klickar på hello **redigera** knapp eller hello **ta bort** knappen.

   > [!NOTE]
   > Se till att hello länkar som du har angett i det här avsnittet fungerar korrekt, eftersom dessa länkar hämta verifieras under din begäran produktionsprocess.
   >
   >
8. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Länkar](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Lägga till en länk](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Ändra en befintlig exempelbild eller lägga till en ny exempelbild
toochange ett befintliga exemplet bild- eller lägga till nya exempel bilder och publicera erbjudandet, gör du följande:

> [!NOTE]
> Endast en exempelbild visas i hello [Azure-portalen](https://portal.azure.com).
>
>

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **MARKNADSFÖRING** fliken.
4. Klicka på **engelska (USA)**.
5. Klicka på hello **exempel bilder** fliken.
6. tooadd en ny exempelbild i hello **exempel bilder** klickar du på **ladda upp en ny avbildning** och spara hello ändringar.

   > [!NOTE]
   > Inklusive en exempelbild är valfritt.
   >
7. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Exempel bilder](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-hello-legal-content"></a>Uppdatera hello juridiska innehåll
tooupdate Hej juridiska innehåll och publicera erbjudandet, gör du följande:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **MARKNADSFÖRING** fliken.
4. Klicka på **engelska (USA)**.
5. Klicka på hello **JURIDISKA** fliken. I hello **juridiska** avsnittet, uppdatera principer/villkor för användning. Skriv eller klistra in hello principer/villkor i hello **ANVÄNDNINGSVILLKOREN** och spara hello ändringar.
6. hello tecken för hello juridiska villkor för användning är 1 miljon tecken.
7. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Juridisk information](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-hello-support-information"></a>Uppdatera hello supportinformation
tooupdate hello supportinformation och publicera erbjudandet, gör du följande:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **stöd** fliken.
4. I hello **Engineering Kontakta** avsnittet update hello engineering kontaktinformation. Denna information används för intern kommunikation mellan hello partner och Microsoft endast.
5. I hello **kundsupport** och uppdatera hello supportinformation och hello **stöder URL**. Denna information används för intern kommunikation mellan hello partner och Microsoft endast.

   > [!NOTE]
   > Om du vill tooprovide endast e-postsupport, ange ett dummy telefonnummer i hello **kundsupport** avsnitt. I det här fallet hello e-post som används i stället.
   >
   >
6. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Support](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-hello-categories"></a>Uppdatera hello kategorier
tooupdate hello kategorier avsnittet för erbjudandet och publicera erbjudandet, Följ dessa steg:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **kategorier** fliken.
4. I hello **kategorier** avsnittet, uppdatera hello kategorier för erbjudandet och spara hello ändringar. Du kan välja toofive kategorier för hello Azure Marketplace-galleriet.
5. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![Kategorier](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Lägg till en ny SKU under listan erbjudande
tooadd en ny SKU i erbjudandet live så här:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **SKU: er** fliken. Klicka på **lägga till en SKU**. 
4. Ange hello dialogrutan en **SKU-ID** med små bokstäver. Välj hello **ta egen licens (BYOL) faktureringsmodellen** kryssrutan om du vill toopublish hello nya SKU med en BYOL faktureringsmodell som tillämpas. Annars avmarkerar du kryssrutan för hello. Klicka på hello skalstreck markering toocreate nya SKU. Om du inte valde hello BYOL faktureringsmodell som tillämpas anges automatiskt toohourly hello faktureringsmodell som tillämpas. Om du vill hello 30-dagars utvärderingsversion för hello timvis fakturering modellen väljer **en månad** för **finns en kostnadsfri utvärderingsversion?** Annars väljer **Nej utvärderingsversion**. (**Finns en kostnadsfri utvärderingsversion?**  visas bara om du inte har valt BYOL medan skapar hello nya SKU.)

   > [!IMPORTANT]
   > **Dölj den här SKU från hello Marketplace eftersom alltid ska köpas via en lösningsmall** ska vara **Ja** *endast* om du har godkänt för att publicera en lösningsmall. Annars kan det här alternativet ska alltid vara **nr**.
   >
   >
4. Hello hello vänster klickar du på menyn hello **VM-AVBILDNINGAR** fliken och ta reda på hello nya SKU som du har skapat.
5. tooset upp Hej nya SKU: N, se [erhålla certifikat för VM-avbildning](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) anvisningar.
6. tooadd marknadsföringsmaterial för Hej nya SKU: N, se [ange Marketplace marknadsföring innehåll](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. tooadd prisinformation för Hej nya SKU: N, se [ange dina priser](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

    ![SKU: er](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Lägg till en SKU](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-hello-data-disk-count-for-a-listed-sku"></a>Ändra hello datadiskar listade SKU
Du kan inte öka/minska hello datadiskar för en listade SKU: N. Du måste toocreate nya SKU i det här fallet. Detaljerad information finns i avsnittet toohello [lägga till en ny SKU under listan erbjudande](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-hello-marketplace"></a>Ta bort ett listade erbjudande från hello Marketplace
Olika aspekter måste toobe tagit hand om hello gäller en tooremove ett live-erbjudande för begäran. tooget vägledning från hello support-teamet tooremove ett listade erbjudande från hello Marketplace, Följ dessa steg:

1. Generera ett supportärende på hello [skapa en incident](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) sidan.

2. Välj **problemtyp** som **hantera erbjudanden**, och välj **kategori** som **ändra ett erbjudande och/eller SKU redan i produktion**.
3. Förfrågan hello.

hello-teamet hjälper dig att hello erbjudandet/SKU borttagningsprocessen.

> [!NOTE]
> Du kan alltid ta bort hello erbjudande när den är i statusen utkast (men inte mellanlagring eller produktion). På hello **HISTORIK** klickar du på **KASTA utkast**.
>
>

## <a name="delete-a-listed-sku-from-hello-marketplace"></a>Ta bort en listade SKU från hello Marketplace
toodelete en listade SKU från hello Marketplace, Följ dessa steg:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).

2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. I hello hello vänster klickar hello **SKU: er** fliken.
4. Välj hello SKU som du vill toodelete och på hello **ta bort** knappen.
5. Gå toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish hello erbjudandet i hello Marketplace.
6. När hello erbjudande publiceras i hello Marketplace tas hello SKU bort från hello Marketplace och hello Azure-portalen.

## <a name="delete-hello-current-version-of-a-listed-sku-from-hello-marketplace"></a>Ta bort hello nuvarande version av listan SKU från hello Marketplace
toodelete hello nuvarande version av en listade SKU från hello Marketplace, Följ dessa steg: 

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).

2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **VM-AVBILDNINGAR** fliken.
4. Välj hello SKU vars aktuell version du vill toodelete och på hello **ta bort** knappen.
5. Gå toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish hello erbjudandet i hello Marketplace.
6. När hello erbjudande hämtar publiceras i hello Marketplace hello nuvarande version av hello listade SKU tas bort från hello Marketplace och hello Azure-portalen. hello SKU sedan återställs tooits tidigare version.

## <a name="revert-hello-listing-price-tooproduction-values"></a>Återställa hello visar pris tooproduction värden
toorevert hello lista pris tooproduction värden, Följ dessa steg:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).
2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **priser** fliken.
4. Välj en region vars priser som du vill tooreset.

    ![Priser regioner](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Återställa hello priser för alla hello kärnor för SKU: er med en timme faktureringsmodell som tillämpas, som i produktion för hello valda regionen. Se för SKU: er med en BYOL fakturering modell hello SKU som är tillgängliga i hello region genom att välja hello kryssrutan för hello SKU i hello **EXTERNALLY-LICENSED (BYOL) SKU tillgänglighet** avsnitt.

    ![Prissättningsmodeller](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klicka på **AUTOPRICE andra marknader utifrån priser i USA**.

   > [!NOTE]
   > hello knappens etikett kan vara olika beroende på hello regionen som du väljer. Eftersom vi valde USA hello knappen heter **AUTOPRICE andra marknader baserat på priser i UNITED STATES**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. På sidan 1 hello Autoprice guiden, Välj hello grundläggande marknaden och klickar på hello **pilen** knappen.

    ![Grundläggande marknaden](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. På sidan 2, väljer serviceplaner och mätare (kärnor) och klicka på hello **pilen** knappen.

    ![Tjänsten planer och mätare](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Klicka på sidan 3 **växla alla** tooselect alla regioner. Du kan manuellt ange kryssrutorna för vissa regioner. Klicka på hello **pilen** knappen.

    ![Välj marknader](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. På sidan 4, granska hello exchange priser och klickar på **Slutför**. hello återställs hello priser bl.a tooyour val.

11. På hello **priser** klickar du på **visa sammanfattning och ändringar**.
    För **Visa Version**väljer **utkast**, och för **Jämför med**väljer **produktion**. Om du ser inga prisnivå skillnaden återställts hello priser toohello produktion värden.

    ![Visa sammanfattning och ändringar](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

## <a name="revert-hello-billing-model-tooproduction-values"></a>Återställa hello fakturering modellen tooproduction värden
toorevert hello fakturering modellen tooproduction värden, Följ dessa steg:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).

2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **SKU: er** fliken.
4. Klicka på hello **redigera** knappen toorevert hello faktureringsmodell som tillämpas. I hello fönster som öppnas, markera eller avmarkera hello **debitering och licensiering görs externt från Azure (aka Bring Your Own License)** kryssrutan.

    ![Redigera fakturering](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Åtgärderna hello i ”Återställ hello visar pris tooproduction värden” i den här artikeln.
6. Gå toohello **publicera** och på **PUSH tooSTAGING**. Detaljerad information om hur du testar erbjudandet i hello mellanlagring miljö finns [testa VM erbjudandet för hello Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. När du har testat erbjudandet i Förproduktion går toohello **publicera** fliken i hello publicering av portalen. Klicka på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

## <a name="revert-hello-visibility-setting-of-a-listed-sku-toohello-production-value"></a>Återställa hello inställningar för listan SKU toohello produktionsvärdet
toorevert hello inställningar för listan SKU toohello produktionsvärde, Följ dessa steg:

1. Logga in toohello [publicering portal](https://publish.windowsazure.com).

2. Gå toohello **virtuella datorer** och välj erbjudandet.
3. Hello hello vänster klickar du på menyn hello **SKU: er** fliken.
4. Välj din SKU och återställa hello inställningar för hello SKU toohello produktionsvärde.

    ![Synlighet](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. När du är klar med hello ändringar klickar du på **begäran om godkännande av tooPUSH tooPRODUCTION** toorepublish erbjudandet i hello Marketplace.

## <a name="see-also"></a>Se även
* [Komma igång: Publicera ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)
* [Förstå beloppet reporting](marketplace-publishing-report-payout.md)
* [Ändra din leverantör av Molnlösningar återförsäljare morot](marketplace-publishing-csp-incentive.md)
* [Felsöka vanliga problem i hello Marketplace](marketplace-publishing-support-common-issues.md)
* [Få support som en utgivare](marketplace-publishing-get-publisher-support.md)
* [Skapa en VM-avbildning lokalt](marketplace-publishing-vm-image-creation-on-premise.md)
* [Skapa en virtuell dator som kör Windows i hello Azure preview portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
