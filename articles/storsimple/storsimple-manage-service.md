---
title: "aaaDeploy hello StorSimple Manager-tjänsten | Microsoft Docs"
description: "Förklarar hur toocreate och ta bort hello StorSimple Manager-tjänsten i hello klassiska Azure-portalen och beskriver hur toomanage hello nyckel för tjänstregistrering."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f49b647d91b03bb89ebd0e5cce196e50e3c00296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-manager-service-in-hello-azure-classic-portal"></a>Distribuera hello StorSimple Manager-tjänsten i hello klassiska Azure-portalen

## <a name="overview"></a>Översikt
Hej StorSimple Manager-tjänsten körs i Microsoft Azure och ansluter toomultiple StorSimple-enheter. När du har skapat hello service kan du använda den toomanage hello enheter från hello Microsoft Azure klassiska portal körs i en webbläsare. Detta gör att du toomonitor alla hello-enheter som är anslutna toohello StorSimple Manager-tjänst från en enda central plats, vilket minimerar administrativa belastningen.

Landningssida för hello StorSimple Manager visar alla hello StorSimple Manager-tjänster som du kan använda toomanage din StorSimple-lagringsenheter. För varje StorSimple Manager-tjänsten visas hello följande information på hello StorSimple Manager-sidan:

* **Namnet** – hello-namn som har tilldelats tooyour StorSimple Manager-tjänsten när den skapades. **hello tjänstens namn kan inte ändras när hello-tjänsten har skapats. Detta gäller även för andra entiteter, till exempel enheter, volymer, volymbehållare och principer för säkerhetskopiering som inte kan i hello klassiska Azure-portalen.**
* **Status för** – hello status för hello-tjänsten, som kan vara **Active**, **skapa**, eller **Online**.
* **Plats** – hello geografisk plats i vilka hello StorSimple enheten kommer att distribueras.
* **Prenumerationen** – hello fakturering prenumeration som är associerad med din tjänst.

hello vanliga uppgifter som kan utföras via hello StorSimple Manager-sidan är:

* Skapa en tjänst
* Ta bort en tjänst
* Hämta hello nyckel för tjänstregistrering
* Återskapa hello nyckel för tjänstregistrering

Den här självstudiekursen beskriver hur tooperform av dessa uppgifter.

## <a name="create-a-service"></a>Skapa en tjänst
Använd hello **Snabbregistrering** alternativet toocreate StorSimple Manager-tjänsten om du vill toodeploy din StorSimple-enhet. toocreate en tjänst behöver du toohave:

* En prenumeration med ett Enterprise-avtal
* Ett aktivt Microsoft Azure storage-konto
* Hej faktureringsinformation som används för hantering

Du kan också välja toogenerate standardkontot för lagring när du skapar hello-tjänsten.

En enskild tjänst kan hantera flera enheter. Men en enhet kan sträcka sig över flera tjänster. Stora företag kan ha flera instanser av tjänsten toowork med olika prenumerationer, organisationer eller även distribution platser. Observera att du behöver separata instanser av StorSimple Manager service toomanage StorSimple 8000-serien enheter och virtuella StorSimple-matriser.

> [!IMPORTANT] 
> Om du har en oanvända tjänst (ingen enhet åtgärder som utförts på denna resurs) skapade tidigare tooAugust 2016, kan inte hanteras via Azure-portalen eller klassiska Azure-portalen. Vi rekommenderar att du skapar en ny tjänst i hello Azure-portalen.

Utföra hello följande steg toocreate en tjänst.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Ta bort en tjänst
Innan du tar bort en tjänst måste du kontrollera att inga anslutna enheter använder den. Om tjänsten hello används, inaktivera hello anslutna enheter. hello inaktivera åtgärden hello anslutning mellan hello enhets- och hello-servern, men bevara hello enhetsdata i hello molnet.

> [!IMPORTANT] 
> När en tjänst har tagits bort, kan hello-åtgärden inte ångras. Alla enheter som använder hello tjänsten behöver toobe fabriksåterställning innan den kan användas med en annan tjänst. I det här scenariot hello lokala data på hello enhet samt hello konfiguration kommer att förloras.

Utföra hello följande steg toodelete en tjänst.

### <a name="toodelete-a-service"></a>toodelete en tjänst
1. På hello **StorSimple Manager-tjänsten** sidan Välj hello service toodelete gärna.
2. Klicka på **ta bort** på hello hello sidans nederkant.
3. Klicka på **Ja** i hello meddelande om bekräftelse. Det kan ta några minuter för hello service toobe tas bort.

## <a name="get-hello-service-registration-key"></a>Hämta hello nyckel för tjänstregistrering
När du har skapat en tjänst måste tooregister din StorSimple-enhet med hello-tjänsten. tooregister din första StorSimple-enhet du behöver hello nyckel för tjänstregistrering. tooregister ytterligare enheter med en befintlig StorSimple-tjänst, måste både hello registreringsnyckel och hello krypteringsnyckel för tjänstdata (som genereras på hello första enheten under registreringen). Läs mer om hello krypteringsnyckel för tjänstdata [StorSimple-säkerhet](storsimple-security.md). Du kan hämta hello registreringsnyckel genom att öppna **registreringsnyckel** på hello **Services** sidan.

Utför följande steg tooget hello tjänstens registreringsnyckel hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Behåll hello nyckel för tjänstregistrering på en säker plats. Du behöver den här nyckeln, samt hello krypteringsnyckel för tjänstdata, tooregister ytterligare enheter med den här tjänsten. När du har fått hello nyckel för tjänstregistrering måste tooconfigure enheten via hello Windows PowerShell för StorSimple-gränssnittet.

Mer information om hur toouse den här nyckeln finns registrering [steg3: konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-hello-service-registration-key"></a>Återskapa hello nyckel för tjänstregistrering
Du behöver tooregenerate en nyckel för tjänstregistrering om du är obligatoriska tooperform viktiga rotation eller om hello listan över tjänstadministratörer har ändrats. När du återskapar nyckeln hello används hello nya nyckeln bara för att registrera följande enheter. hello-enheter som redan har registrerat påverkas inte av den här processen.

Utför följande steg tooregenerate en nyckel för tjänstregistrering hello.

### <a name="tooregenerate-hello-service-registration-key"></a>tooregenerate hello nyckel för tjänstregistrering
1. På hello **StorSimple Manager-tjänsten** klickar du på **registreringsnyckel**.
2. I hello **nyckel för tjänstregistrering** dialogrutan klickar du på **återskapa**.
3. Visas ett bekräftelsemeddelande. Klicka på **OK** toocontinue med hello återskapas.
4. En ny nyckel för tjänstregistrering visas.
5. Kopiera den här nyckeln och spara den för att registrera nya enheter med den här tjänsten.
6. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-service/HCS_CheckIcon.png) tooclose den här dialogrutan.

## <a name="next-steps"></a>Nästa steg
* Mer information om hello [StorSimple distributionsprocessen](storsimple-deployment-walkthrough-u2.md).
* Lär dig mer om [hantera ditt lagringskonto i StorSimple](storsimple-manage-storage-accounts.md).
* Läs mer om hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).
