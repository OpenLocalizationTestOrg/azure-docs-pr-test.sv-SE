---
title: "aaaManage StorSimple-lagringskonto autentiseringsuppgifter för Microsoft Azure StorSimple 8000-serien enheter | Microsoft Docs"
description: "Beskriver hur du kan använda hello Enhetshanteraren StorSimple Konfigurera sidan tooadd, redigera, ta bort eller rotera hello säkerhetsnycklar för ett lagringskonto."
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
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 132ee46509b39db4d1b97b0f1077800a253e8da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-storage-account-credentials"></a>Använd hello StorSimple Enhetshanteraren service toomanage autentiseringsuppgifterna för ditt lagringskonto

## <a name="overview"></a>Översikt

Hej **Configuration** avsnittet hello StorSimple Enhetshanteraren service-bladet visar hello globala parametrar för tjänsten som kan skapas i hello StorSimple enheten Manager-tjänsten. De här parametrarna kan vara tillämpade tooall hello enheter anslutna toohello tjänsten och inkluderar:

* Autentiseringsuppgifter för lagringskonto
* Bandbredd-mallar 
* Access control-poster 

Den här självstudiekursen beskrivs hur tooadd, redigera, ta bort lagringskontouppgifter eller rotera hello säkerhetsnycklar för ett lagringskonto.

 ![Lista över autentiseringsuppgifter för lagringskonton](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Storage-konton innehåller hello-autentiseringsuppgifter som hello StorSimple-enhet använder tooaccess ditt lagringskonto med din molntjänstleverantör. För Microsoft Azure storage-konton är dessa autentiseringsuppgifter, till exempel hello kontonamn och hello primärnyckeln. 

På hello **lagringskontouppgifter** bladet, alla lagringsutrymmen konton som har skapats för hello fakturering prenumeration visas i tabellformat som innehåller hello följande information:

* **Namnet** – hello unikt namn som tilldelats toohello konto när den skapades.
* **SSL aktiverat** – om hello SSL är aktiverat och enhet till moln kommunikation över hello säker kanal.
* **Används av** – hello antalet volymer med hello storage-konto.

hello vanligaste uppgifter relaterade toostorage konton som du kan utföra är:

* Lägg till ett lagringskonto 
* Redigera ett lagringskonto 
* Ta bort ett lagringskonto 
* Viktiga rotation av storage-konton 

## <a name="types-of-storage-accounts"></a>Typer av lagringskonton

Det finns tre typer av lagringskonton som kan användas med din StorSimple-enhet.

* **Automatiskt genererade lagringskonton** – som hello namnet den här typen av lagringskonto genereras automatiskt när hello tjänst skapas först. toolearn mer information om hur det här lagringskontot har skapats, se [steg 1: skapa en ny tjänst](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) i [distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md). 
* **Storage-konton i hello tjänstprenumeration** – dessa är hello Azure storage-konton som är associerade med hello samma prenumeration som hello-tjänsten. toolearn mer information om hur du skapar dessa storage-konton, se [om Azure Lagringskonton](../storage/common/storage-create-storage-account.md). 
* **Storage-konton utanför hello tjänstprenumeration** – dessa är hello Azure storage-konton som inte är associerade med din tjänst och förmodligen sparats innan hello tjänsten skapades.

## <a name="add-a-storage-account"></a>Lägg till ett lagringskonto

Du kan lägga till ett lagringskonto genom att ange ett unikt eget namn och autentiseringsuppgifter som är länkade toohello storage-konto (med hello angivna molnet service provider). Du kan också ha hello alternativet för att aktivera hello secure sockets layer (SSL)-läge toocreate en säker kanal för nätverkskommunikation mellan din enhet och hello moln.

Du kan skapa flera konton för en viss molntjänstleverantören. Tänk dock på att du inte kan ändra hello molntjänstleverantören efter ett lagringskonto skapas.

Medan hello storage-kontot sparas försöker hello-tjänsten toocommunicate med din molntjänstleverantör. hello autentiseringsuppgifter och hello åtkomst material som du har angett autentiseras just nu. Ett lagringskonto skapas endast om hello autentiseringen lyckas. Om hello autentiseringen misslyckas visas ett felmeddelande visas.

Använd följande procedurer tooadd Azure storage-kontouppgifter hello:

* tooadd storage-konto autentiseringsuppgifter som har hello samma Azure-prenumeration som hello Enhetshanteraren tjänst
* tooadd en autentiseringsuppgift för Azure storage-konto som ligger utanför hello Enhetshanteraren prenumeration på tjänsten

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="tooadd-an-azure-storage-account-credential-outside-of-hello-storsimple-device-manager-service-subscription"></a>tooadd ett Azure storage-kontoautentisering utanför hello StorSimple Enhetshanteraren prenumeration på tjänsten

1. Navigera tooyour StorSimple Device Manager-tjänsten, väljer och dubbelklicka på den. Då öppnas hello **översikt** bladet.
2. Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt. Här visas alla befintliga lagringskontouppgifter som är associerade med hello StorSimple enheten Manager-tjänsten.
3. Klicka på **Lägg till**.
4. I hello **lägga till en lagring kontoautentisering** bladet hello följande:
   
    1. För **prenumeration**väljer **andra**.
   
    2. Ange hello namnet för dina autentiseringsuppgifter för Azure storage-konto.
   
    3. I hello **åtkomstnyckeln för Lagringskontot** textrutan Ange hello primära åtkomstnyckeln för dina autentiseringsuppgifter för Azure storage-konto. tooget det här viktiga går toohello Azure Storage-tjänst, Välj lagring-kontoautentisering och på **hantera nycklar**. Nu kan du kopiera hello primärnyckeln.
   
    4. tooenable SSL, klicka på hello **aktivera** knappen toocreate en säker kanal för nätverkskommunikation mellan din StorSimple Device Manager-tjänsten och hello moln. Klicka på hello **inaktivera** knappen bara om du arbetar inom ett privat moln.
   
    5. Klicka på **Lägg till**. Du meddelas när hello lagring kontoautentisering har skapats.

5. hello nyskapad lagring kontoautentisering visas på hello StorSimple Konfigurera Enhetshanteraren service bladet under **lagringskontouppgifter**.
   


## <a name="edit-a-storage-account"></a>Redigera ett lagringskonto

Du kan redigera ett lagringskonto som används av en volymbehållare. Om du redigerar ett lagringskonto som används för närvarande är hello endast toomodify som är tillgängliga i fältet hello åtkomstnyckel för hello storage-konto. Du kan ange hello nya lagringsåtkomstnyckel och spara inställningarna för hello uppdateras.

#### <a name="tooedit-a-storage-account"></a>tooedit ett lagringskonto

1. Gå tooyour StorSimple enheten Manager-tjänsten. I hello **Configuration** klickar du på **lagringskontouppgifter**.

    ![Autentiseringsuppgifter för lagringskonto](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. I hello **lagringskontouppgifter** bladet hello listan med lagringskontouppgifter, väljer och klickar på hello en du vill tooedit. 

3. Du kan ändra hello **aktivera SSL** val. Du kan också klicka på **mer...**  och välj sedan **Sync access key toorotate** åtkomstnycklar för lagring konto. Gå för[nyckeln rotation av lagringskonton](#key-rotation-of-storage-accounts) mer information om hur tooperform nyckeln rotation. När du har ändrat hello inställningar, klickar du på **spara**. 

    ![Spara autentiseringsuppgifterna redigerade lagringskonto](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. 

    ![Bekräfta ändringar](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

hello inställningarna uppdateras och sparas för ditt lagringskonto. 

## <a name="delete-a-storage-account"></a>Ta bort ett lagringskonto

> [!IMPORTANT]
> Du kan ta bort ett lagringskonto endast om den inte används av en volymbehållare. Om ett lagringskonto används av en volymbehållare kan du först ta bort hello volymbehållare och ta sedan bort hello associerade storage-konto.

#### <a name="toodelete-a-storage-account"></a>toodelete ett lagringskonto

1. Gå tooyour StorSimple enheten Manager-tjänsten. I hello **Configuration** klickar du på **lagringskontouppgifter**.

2. Hovra över hello-konto som du vill toodelete i hello tabular lista med lagringskonton. Högerklicka på snabbmenyn för tooinvoke hello och på **ta bort**.

    ![Ta bort lagringskontouppgifter](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. När du uppmanas att bekräfta, klickar du på **Ja** toocontinue med hello borttagning. hello tabular lista kommer att vara uppdaterade tooreflect hello ändringar.

    ![Bekräfta borttagning](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Viktiga rotation av storage-konton

Av säkerhetsskäl är viktiga rotation ofta ett krav i datacenter. Varje Microsoft Azure-prenumeration kan ha en eller flera associerade storage-konton. Hej toothese åtkomstkonton styrs av hello prenumeration och snabbtangenterna för varje lagringskonto. 

När du skapar ett lagringskonto genererar Microsoft Azure två 512-bitars lagring åtkomstnycklar som används för autentisering när lagringskontot hello används. Om du har två åtkomstnycklar för lagring kan du tooregenerate hello nycklarna utan avbrott tooyour lagringstjänsten eller toothat-tjänsten för dataåtkomst. hello nyckel som används för närvarande är hello *primära* nyckel och hello reservnyckel är refererad tooas hello *sekundära* nyckel. En av dessa två nycklar måste anges när Microsoft Azure StorSimple-enheten ansluter till din molntjänstleverantör för lagring.

## <a name="what-is-key-rotation"></a>Vad är viktiga rotation?

Normalt använder program endast en av hello nycklar tooaccess dina data. Du kan ha dina program växla över toousing hello andra nyckeln efter en viss tidsperiod. När du har växlat sekundärnyckeln ditt program toohello, kan du dra tillbaka hello första nyckeln och sedan skapa en ny nyckel. Med hjälp av hello två nycklar det här sättet kan dina program åtkomst toohello data utan att det medför några driftavbrott.

Hej lagringskontonycklar måste alltid lagras i hello tjänsten i krypterad form. Dessa kan dock återställas via hello StorSimple enheten Manager-tjänsten. hello kan hämta hello primärnyckeln och sekundärnyckeln för alla hello storage-konton i hello samma prenumeration, inklusive konton som har skapats i hello lagringstjänsten samt hello standardkontona för lagring genereras hello när StorSimple Device Manager tjänsten skapades. Hej StorSimple enheten Manager-tjänsten kommer alltid hämta nycklarna från hello klassiska Azure-portalen och lagra dem på ett sätt som är krypterade.

## <a name="rotation-workflow"></a>Rotation arbetsflöde

En Microsoft Azure-administratör kan återskapa eller ändra hello primära och sekundära nycklarna genom direkt åtkomst till hello storage-konto (via hello Microsoft Azure Storage service). Hej StorSimple enheten Manager-tjänsten kan inte se ändringen automatiskt.

tooinform hello StorSimple Enhetshanteraren tjänsten hello förändring, behöver du tooaccess hello StorSimple Enhetshanteraren tjänsten åtkomst hello lagringskontot och sedan synkronisera hello primära och sekundära nycklarna (beroende på vilket som ändrats). hello-tjänsten sedan hämtar hello senaste nyckeln krypterar hello nycklar och skickar hello krypterade nyckeln toohello enhet.

#### <a name="toosynchronize-keys-for-storage-accounts-in-hello-same-subscription-as-hello-service"></a>toosynchronize nycklar för lagringskonton hello samma prenumeration som hello-tjänst 
1. Gå tooyour StorSimple enheten Manager-tjänsten. I hello **Configuration** klickar du på **lagringskontouppgifter**.
2. Klicka på hello något som du vill toomodify hello tabular lista av lagringskonton. 

    ![Synkronisera nycklar](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Klicka på **... Flera** och välj sedan **Sync access key toorotate**.   

    ![Synkronisera nycklar](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. I hello StorSimple Device Manager service behöver du tooupdate hello nyckel som tidigare har ändrats i hello Microsoft Azure Storage-tjänst. Om hello primärnyckeln har ändrats (återskapats), väljer **primära** nyckel. Om hello sekundärnyckeln har ändrats, väljer **sekundära** nyckel. Klicka på **Sync nyckeln**.
      
      ![Synkronisera nycklar](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

Du meddelas när hello nyckeln är har sycnhronized.

#### <a name="toosynchronize-keys-for-storage-accounts-outside-of-hello-service-subscription"></a>toosynchronize nycklar för lagringskonton utanför hello prenumeration på tjänsten
1. På hello **Services** klickar du på hello **konfigurera** fliken.
2. Klicka på **lägga till eller redigera Lagringskonton**.
3. I dialogrutan för hello hello följande:
   
   1. Välj hello storage-konto med hello åtkomstnyckel som du vill tooupdate.
   2. Du behöver tooupdate hello lagringsåtkomstnyckel i hello StorSimple enheten Manager-tjänsten. I det här fallet kan du se hello lagringsåtkomstnyckel. Ange hello ny nyckel i hello **åtkomstnyckeln för Lagringskontot** rutan. 
   3. Spara ändringarna. Din lagringsåtkomstnyckel för kontot ska nu uppdateras.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [StorSimple-säkerhet](storsimple-8000-security.md).
* Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

