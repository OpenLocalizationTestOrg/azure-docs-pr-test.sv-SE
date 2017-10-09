---
title: aaaManage virtuella StorSimple-matris lagringskontouppgifter | Microsoft Docs
description: "Beskriver hur du kan använda hello Enhetshanteraren StorSimple Konfigurera sidan tooadd, redigera, ta bort eller rotera hello säkerhetsnycklar för lagringskontouppgifter som är associerade med hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: 22a0341eae0b89020065be4dbfaae77999f8be0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-storage-account-credentials-for-storsimple-virtual-array"></a>Använd StorSimple Enhetshanteraren toomanage lagringskontouppgifter för virtuell StorSimple-matris

## <a name="overview"></a>Översikt
Hej **Configuration** avsnitt i hello StorSimple Enhetshanteraren service bladet för din virtuella StorSimple-matris visar hello tjänsten för global parametrar som kan skapas i hello StorSimple Manager-tjänsten. De här parametrarna kan vara tillämpade tooall hello enheter anslutna toohello tjänsten och inkluderar:

* Autentiseringsuppgifter för lagringskonto
* Access control-poster
  
  ![Enhetshanteraren Service instrumentpanelen](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Den här självstudiekursen beskrivs hur du kan lägga till, redigera eller ta bort lagringskontouppgifter för din virtuella StorSimple-matrisen. hello information i den här kursen gäller bara toohello virtuella StorSimple-matris. Mer information om hur toomanage lagringskonton i 8000-serien finns [Använd hello StorSimple Manager service toomanage ditt lagringskonto](storsimple-manage-storage-accounts.md).

Storage-konto autentiseringsuppgifter innehåller hello-autentiseringsuppgifter som hello enheten använder tooaccess storage-konto med din molntjänstleverantör. För Microsoft Azure storage-konton är dessa autentiseringsuppgifter, till exempel hello kontonamn och hello primärnyckeln.

På hello **lagringskontouppgifter** bladet, alla lagringskonto autentiseringsuppgifter som har skapats för hello fakturering prenumeration visas i tabellformat som innehåller hello följande information:

* **Namnet** – hello unikt namn som tilldelats toohello konto när den skapades.
* **SSL aktiverat** – om hello SSL är aktiverat och enhet till moln kommunikation över hello säker kanal.
  
  ![Konfigurationsavsnittet](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

hello vanligaste uppgifterna relaterade toostorage kontoautentiseringsuppgifter som kan utföras på hello **lagringskontouppgifter** bladet är:

* Lägg till autentiseringsuppgift för lagringskonto
* Redigera en autentiseringsuppgift för storage-konto
* Ta bort en autentiseringsuppgift för storage-konto

## <a name="types-of-storage-account-credentials"></a>Typer av autentiseringsuppgifterna för lagringskonto
Det finns tre typer av lagring kontoautentiseringsuppgifter som kan användas med din StorSimple-enhet.

* **Automatiskt genererade lagringskontouppgifter** – som hello namnet den här typen av lagring kontoautentisering genereras automatiskt när hello tjänst skapas först. toolearn mer information om hur dessa autentiseringsuppgifter för storage-konto har skapats, se [skapa en ny tjänst](storsimple-virtual-array-manage-service.md#create-a-service).
* **lagringskontouppgifter i hello tjänstprenumeration** – dessa är hello Azure storage-konto autentiseringsuppgifter som är associerade med hello samma prenumeration som hello-tjänsten. toolearn mer information om hur du skapar dessa lagringskontouppgifter, se [om Azure Lagringskonton](../storage/common/storage-create-storage-account.md).
* **lagringskontouppgifter utanför hello tjänstprenumeration** – dessa är hello Azure storage autentiseringsuppgifter som inte är associerade med din tjänst och förmodligen sparats innan hello tjänsten skapades.

## <a name="add-a-storage-account-credential"></a>Lägg till autentiseringsuppgift för lagringskonto
Du kan lägga till ett konto autentiseringsuppgifter tooyour StorSimple Enhetshanteraren service lagringskonfiguration genom att ange ett unikt eget namn och autentiseringsuppgifter som är länkade toohello storage-konto. Du kan också ha hello alternativet för att aktivera hello secure sockets layer (SSL)-läge toocreate en säker kanal för nätverkskommunikation mellan din enhet och hello moln.

Du kan skapa flera konton för en viss molntjänstleverantören. Medan hello lagring kontoautentisering sparas försöker hello-tjänsten toocommunicate med din molntjänstleverantör. hello autentiseringsuppgifter och hello åtkomst material som du har angett autentiseras just nu. Storage-konto autentiseringsuppgifter skapas endast om hello autentiseringen lyckas. Om hello autentiseringen misslyckas visas ett felmeddelande visas.

Använd följande procedurer tooadd Azure storage-kontouppgifter hello:

* tooadd storage-konto autentiseringsuppgifter som har hello samma Azure-prenumeration som hello Enhetshanteraren tjänst
* tooadd en autentiseringsuppgift för Azure storage-konto som ligger utanför hello Enhetshanteraren prenumeration på tjänsten

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a>tooadd storage-konto autentiseringsuppgifter som har hello samma Azure-prenumeration som hello Enhetshanteraren tjänst

1. Navigera tooyour Device Manager-tjänsten, väljer och dubbelklicka på den. Då öppnas hello **översikt** bladet.
2. Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt.
3. Klicka på **Lägg till**.
4. I hello **Lägg till ett lagringskonto** bladet hello följande:
   
    1. För **prenumeration**väljer **aktuella**.
    2. Ange hello namnet på ditt Azure storage-konto.
    3. Välj **aktivera** toocreate en säker kanal för nätverkskommunikation mellan din StorSimple-enhet och hello moln. Välj **inaktivera** endast om du arbetar inom ett privat moln.
    4. Klicka på **Lägg till**. Du meddelas när hello storage-konto har skapats.<br></br>
   
        ![Lägg till en befintlig lagring konto autentiseringsuppgift](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="tooadd-an-azure-storage-account-credential-that-is-outside-of-hello-device-manager-service-subscription"></a>tooadd en autentiseringsuppgift för Azure storage-konto som ligger utanför hello Enhetshanteraren prenumeration på tjänsten

1. Navigera tooyour Device Manager-tjänsten, väljer och dubbelklicka på den. Då öppnas hello **översikt** bladet.
2. Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt. Här visas alla befintliga lagringskontouppgifter som är associerade med hello StorSimple enheten Manager-tjänsten.
3. Klicka på **Lägg till**.
4. I hello **Lägg till ett lagringskonto** bladet hello följande:
   
    1. För **prenumeration**väljer **andra**.
   
    2. Ange hello namnet för dina autentiseringsuppgifter för Azure storage-konto.
   
    3. I hello **åtkomstnyckeln för Lagringskontot** textrutan Ange hello primära åtkomstnyckeln för dina autentiseringsuppgifter för Azure storage-konto. tooget det här viktiga går toohello Azure Storage-tjänst, Välj lagring-kontoautentisering och på **hantera nycklar**. Nu kan du kopiera hello primärnyckeln.
   
    4. tooenable SSL, klicka på hello **aktivera** knappen toocreate en säker kanal för nätverkskommunikation mellan din StorSimple Device Manager-tjänsten och hello moln. Klicka på hello **inaktivera** knappen bara om du arbetar inom ett privat moln.
   
    5. Klicka på **Lägg till**. Du meddelas när hello lagring kontoautentisering har skapats.

5. hello nyskapad lagring kontoautentisering visas på hello StorSimple Konfigurera Enhetshanteraren service bladet under **lagringskontouppgifter**.
   
    ![Lägg till en lagring kontoautentisering utanför hello Enhetshanteraren prenumeration på tjänsten](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Redigera en autentiseringsuppgift för storage-konto
Du kan redigera en storage-konto autentiseringsuppgifter som används av enheten. Om du redigerar en autentiseringsuppgift för lagring kontot som används för närvarande är hello fält tillgängliga toomodify hello åtkomstnyckel och hello SSL-läge för hello storage-konto autentiseringsuppgifter. Du kan ange hello nya lagringsåtkomstnyckel eller ändra hello **aktivera SSL-läge** val och spara hello uppdaterade inställningar.

#### <a name="tooedit-a-storage-account-credential"></a>tooedit storage-konto autentiseringsuppgifter
1. Navigera tooyour Device Manager-tjänsten, väljer och dubbelklicka på den. Då öppnas hello **översikt** bladet.
2. Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt. Här visas alla befintliga lagringskontouppgifter som är associerade med hello StorSimple enheten Manager-tjänsten.
3. Välj i hello tabular lista över lagringskontouppgifter, och dubbelklicka på hello-konto som du vill toomodify.
4. I hello lagring kontoautentisering **egenskaper** bladet hello följande:
   
   1. Om nödvändigt, kan du ändra hello **aktivera SSL** läge val.
   2. Du kan välja tooregenerate åtkomstnycklar för lagring konto autentiseringsuppgifter. Mer information finns i [återskapa hello lagringskontonycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys). Ange hello nya autentiseringsuppgifter lagringskontonyckel. Detta är hello primärnyckeln för ett Azure storage-konto.
   3. Klicka på **spara** hello överst i hello **egenskaper** bladet toosave hello inställningar. hello inställningarna uppdateras på hello **lagringskontouppgifter** bladet.
      
      ![Redigera en autentiseringsuppgift för storage-konto](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Ta bort en autentiseringsuppgift för storage-konto
> [!IMPORTANT]
> Du kan ta bort en autentiseringsuppgift för storage-konto endast om den inte används. Om en autentiseringsuppgift för storage-konto används, visas ett meddelande.
> 
> 

#### <a name="toodelete-a-storage-account-credential"></a>toodelete storage-konto autentiseringsuppgifter
1. Navigera tooyour Device Manager-tjänsten, väljer och dubbelklicka på den. Då öppnas hello **översikt** bladet.
2. Välj **lagringskontouppgifter** inom hello **Configuration** avsnitt. Här visas alla befintliga lagringskontouppgifter som är associerade med hello StorSimple enheten Manager-tjänsten.
3. Välj i hello tabular lista över lagringskontouppgifter, och dubbelklicka på hello-konto som du vill toodelete.
4. I hello lagring kontoautentisering **egenskaper** bladet hello följande:
   
   1. Klicka på **ta bort** toodelete hello autentiseringsuppgifter.
   2. När du uppmanas att bekräfta, klickar du på **Ja** toocontinue med hello borttagning. hello tabular lista är uppdaterade tooreflect hello ändringarna.
      
      ![Ta bort en autentiseringsuppgift för storage-konto](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Synkroniserar lagringskontonycklar för autentiseringsuppgifter
Av säkerhetsskäl är viktiga rotation ofta ett krav i datacenter. En Microsoft Azure-administratör kan återskapa eller ändra hello primära och sekundära nycklarna genom direkt åtkomst till hello lagring kontoautentisering (via hello Microsoft Azure Storage service). Hej StorSimple enheten Manager-tjänsten kan inte se ändringen automatiskt.

tooinform hello StorSimple Enhetshanteraren tjänsten hello förändring, behöver du tooaccess hello StorSimple Enhetshanteraren tjänst, komma åt hello lagringsutrymme konto autentiseringsuppgifter och sedan synkronisera hello primära och sekundära nycklarna (beroende på vilket som ändrats). hello-tjänsten sedan hämtar hello senaste nyckeln krypterar hello nycklar och skickar hello krypterade nyckeln toohello enhet.

#### <a name="toosynchronize-keys-for-storage-account-credentials-in-hello-same-subscription-as-hello-service-azure-only"></a>toosynchronize nycklar för lagringskontouppgifter i hello samma prenumeration som hello-tjänst (Azure)
1. Hello service landning bladet Välj tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **lagringskontouppgifter**.
2. På hello **lagringskontouppgifter** bladet hello väljer i lista med lagringskontouppgifter, hello lagring kontoautentisering vars nycklar som du vill toosynchronize.
3. I hello **egenskaper** bladet för hello valt kontoautentisering för lagring, hello följande:
   
    1. Klicka på **mer**, och klicka sedan på **Sync åtkomstnyckel**.
   
    2. När du uppmanas att bekräfta, klickar du på **Sync nyckeln** toocomplete hello synkronisering.
    
4. I hello StorSimple Device Manager service behöver du tooupdate hello nyckel som tidigare har ändrats i hello Microsoft Azure Storage-tjänst. I hello **synkronisera lagringskontonyckel** bladet hello primärnyckeln har ändrats (återskapats), klicka på primär och klicka sedan på **Sync nyckeln**. Om hello sekundärnyckeln har ändrats, klickar du på **sekundära**, och klicka sedan på **Sync nyckeln**.
   
    ![Åtkomstnyckeln för synkronisering](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-acess-key.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

