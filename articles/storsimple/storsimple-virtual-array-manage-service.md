---
title: "aaaDeploy StorSimple enheten Manager-tjänsten | Microsoft Docs"
description: "Förklarar hur toocreate och ta bort hello StorSimple enheten Manager-tjänsten i hello Azure-portalen och beskriver hur toomanage hello nyckel för tjänstregistrering."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 9064053addc7b3dfcce08b47e81b38c2e0e1b559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-virtual-array"></a>Distribuera hello StorSimple Enhetshanteraren tjänst för virtuell StorSimple-matris
## <a name="overview"></a>Översikt

Hej StorSimple enheten Manager-tjänsten körs i Microsoft Azure och ansluter toomultiple StorSimple-enheter. När du har skapat hello service kan du använda den toomanage hello enheter från hello Microsoft Azure portal körs i en webbläsare. Detta gör att du toomonitor alla hello-enheter som är anslutna toohello StorSimple Enhetshanteraren tjänsten från en enda central plats, vilket minimerar administrativa belastningen.

hello vanliga uppgifter relaterade tooa StorSimple enheten Manager-tjänsten är:

* Skapa en tjänst
* Ta bort en tjänst
* Hämta hello nyckel för tjänstregistrering
* Återskapa hello nyckel för tjänstregistrering

Den här självstudiekursen beskrivs hur tooperform av hello föregående aktiviteter. hello informationen i den här artikeln är tillämpliga endast tooStorSimple virtuella matriser. Mer information om StorSimple 8000-serien finns för[distribuera StorSimple Manager-tjänsten](storsimple-manage-service.md).

## <a name="create-a-service"></a>Skapa en tjänst

toocreate en tjänst behöver du toohave:

* En prenumeration med ett Enterprise-avtal
* Ett aktivt Microsoft Azure storage-konto
* Hej faktureringsinformation som används för hantering

Du kan också välja toogenerate storage-konto när du skapar hello-tjänsten.

En enskild tjänst kan hantera flera enheter. Men en enhet kan sträcka sig över flera tjänster. Stora företag kan ha flera instanser av tjänsten toowork med olika prenumerationer, organisationer eller även distribution platser.

> [!NOTE]
> Du behöver separata instanser av StorSimple Enhetshanteraren service toomanage StorSimple 8000-serien enheter och virtuella StorSimple-matriser.


Utföra hello följande steg toocreate en tjänst.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Ta bort en tjänst

Innan du tar bort en tjänst måste du kontrollera att inga anslutna enheter använder den. Om tjänsten hello används, inaktivera hello anslutna enheter. hello inaktivera åtgärden hello anslutning mellan hello enhets- och hello-servern, men bevara hello enhetsdata i hello molnet.

> [!IMPORTANT]
> När en tjänst har tagits bort, kan hello-åtgärden inte ångras. Alla enheter som använder hello tjänsten behöver toobe fabriksåterställning innan den kan användas med en annan tjänst. I det här scenariot hello lokala data på hello enhet samt hello konfiguration kommer att förloras.
 

Utföra hello följande steg toodelete en tjänst.

#### <a name="toodelete-a-service"></a>toodelete en tjänst

1. Gå för**alla resurser**. Sök efter StorSimple Device Manager-tjänsten. Välj hello-tjänsten som du vill toodelete.
   
    ![Välj tjänsten toodelete](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Gå tooyour service instrumentpanelen tooensure det finns inga enheter anslutna toohello service. Om det finns inga enheter som registrerats med den här tjänsten kan visas du dessutom en banderoll visas toohello effekt. Klicka på **Ta bort**.
   
    ![Ta bort tjänsten](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. När du uppmanas att bekräfta, klickar du på **Ja** i hello meddelande om bekräftelse. 
   
    ![Bekräfta borttagning av tjänsten](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Det kan ta några minuter för hello service toobe tas bort. Efter hello service är har tagits bort, du får ett meddelande.
   
    ![Lyckad tjänsten tas bort](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

hello lista över tjänster kommer att uppdateras.

 ![Uppdaterade listan över tjänster](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-hello-service-registration-key"></a>Hämta hello nyckel för tjänstregistrering
När du har skapat en tjänst måste tooregister din StorSimple-enhet med hello-tjänsten. tooregister din första StorSimple-enhet du behöver hello nyckel för tjänstregistrering. tooregister ytterligare enheter med en befintlig StorSimple-tjänst, måste både hello registreringsnyckel och hello krypteringsnyckel för tjänstdata (som genereras på hello första enheten under registreringen). Läs mer om hello krypteringsnyckel för tjänstdata [StorSimple-säkerhet](storsimple-security.md). Du kan hämta hello registreringsnyckel genom att öppna hello **nycklar** bladet för din tjänst.

Utför följande steg tooget hello tjänstens registreringsnyckel hello.

#### <a name="tooget-hello-service-registration-key"></a>tooget hello nyckel för tjänstregistrering
1. I hello **StorSimple Enhetshanteraren** bladet går för**Management &gt;**  **nycklar**.
   
   ![Bladet Nycklar](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. I hello **nycklar** bladet en nyckel för tjänstregistrering visas. Kopiera hello registreringsnyckel med hello kopiera-ikonen. 

Behåll hello nyckel för tjänstregistrering på en säker plats. Du behöver den här nyckeln, samt hello krypteringsnyckel för tjänstdata, tooregister ytterligare enheter med den här tjänsten. När du har fått hello nyckel för tjänstregistrering måste tooconfigure enheten via hello Windows PowerShell för StorSimple-gränssnittet.

## <a name="regenerate-hello-service-registration-key"></a>Återskapa hello nyckel för tjänstregistrering
Du behöver tooregenerate en nyckel för tjänstregistrering om du är obligatoriska tooperform viktiga rotation eller om hello listan över tjänstadministratörer har ändrats. När du återskapar nyckeln hello används hello nya nyckeln bara för att registrera följande enheter. hello-enheter som redan har registrerat påverkas inte av den här processen.

Utför följande steg tooregenerate en nyckel för tjänstregistrering hello.

#### <a name="tooregenerate-hello-service-registration-key"></a>tooregenerate hello nyckel för tjänstregistrering
1. I hello **StorSimple Enhetshanteraren** bladet går för**Management &gt;**  **nycklar**.
   
   ![Bladet Nycklar](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. I hello **nycklar** bladet, klickar du på **återskapa**.
   
   ![Klicka på Återskapa](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. I hello **Generera nyckel för tjänstregistrering** bladet granska hello åtgärd krävs när hello nycklar genereras om. Alla efterföljande hello-enheter som registreras med tjänsten ska använda hello ny registreringsnyckel. Klicka på **återskapa** tooconfirm. Du meddelas när hello registreringen är slutförd.
   
   ![Bekräfta Generera nyckel](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. En ny nyckel för tjänstregistrering visas.
   
    ![Bekräfta Generera nyckel](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Kopiera den här nyckeln och spara den för att registrera nya enheter med den här tjänsten.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[Kom igång](storsimple-virtual-array-deploy1-portal-prep.md) med en virtuell StorSimple-matris.
* Lär dig hur för[administrera din StorSimple-enhet](storsimple-ova-web-ui-admin.md).

