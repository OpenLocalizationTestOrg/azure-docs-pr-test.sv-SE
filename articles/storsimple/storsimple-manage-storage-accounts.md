---
title: aaaManage StorSimple-lagringskonto | Microsoft Docs
description: "Beskriver hur du kan använda hello StorSimple Manager Konfigurera sidan tooadd, redigera, ta bort eller rotera hello säkerhetsnycklar för ett lagringskonto."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/29/2016
ms.author: v-sharos
ms.openlocfilehash: 78f408818ee8532dfaac445200048145547c987c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-storage-account"></a>Använd hello StorSimple Manager-tjänsten toomanage storage-konto
## <a name="overview"></a>Översikt
Hej **konfigurera** visar hello globala parametrar för tjänsten som kan skapas i hello StorSimple Manager-tjänsten. De här parametrarna kan vara tillämpade tooall hello enheter anslutna toohello tjänsten och inkluderar:

* Lagringskonton 
* Bandbredd-mallar 
* Access control-poster 

Den här självstudiekursen beskrivs hur du kan använda hello **konfigurera** sidan tooadd, redigera eller ta bort storage-konton eller rotera hello säkerhetsnycklar för ett lagringskonto.

 ![Konfigurera](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Storage-konton innehåller hello-autentiseringsuppgifter som hello enheten använder tooaccess ditt lagringskonto med din molntjänstleverantör. För Microsoft Azure storage-konton är dessa autentiseringsuppgifter, till exempel hello kontonamn och hello primärnyckeln. 

På hello **konfigurera** sidan alla lagringsutrymmen konton som har skapats för hello fakturering prenumeration visas i tabellformat som innehåller hello följande information:

* **Namnet** – hello unikt namn som tilldelats toohello konto när den skapades.
* **SSL aktiverat** – om hello SSL är aktiverat och enhet till moln kommunikation över hello säker kanal.
* **Används av** – hello antalet volymer med hello storage-konto.

hello vanligaste uppgifterna relaterade toostorage konton som kan utföras på hello **konfigurera** sidan är:

* Lägg till ett lagringskonto 
* Redigera ett lagringskonto 
* Ta bort ett lagringskonto 
* Viktiga rotation av storage-konton 

## <a name="types-of-storage-accounts"></a>Typer av lagringskonton
Det finns tre typer av lagringskonton som kan användas med din StorSimple-enhet.

* **Automatiskt genererade lagringskonton** – som hello namnet den här typen av lagringskonto genereras automatiskt när hello tjänst skapas först. toolearn mer information om hur det här lagringskontot har skapats, se [steg 1: skapa en ny tjänst](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) i [distribuera din lokala StorSimple-enhet](storsimple-deployment-walkthrough.md). 
* **Storage-konton i hello tjänstprenumeration** – dessa är hello Azure storage-konton som är associerade med hello samma prenumeration som hello-tjänsten. toolearn mer information om hur du skapar dessa storage-konton, se [om Azure Lagringskonton](../storage/common/storage-create-storage-account.md). 
* **Storage-konton utanför hello tjänstprenumeration** – dessa är hello Azure storage-konton som inte är associerade med din tjänst och förmodligen sparats innan hello tjänsten skapades.

## <a name="add-a-storage-account"></a>Lägg till ett lagringskonto
Du kan lägga till ett lagringskonto genom att ange ett unikt eget namn och autentiseringsuppgifter som är länkade toohello storage-konto (med hello angivna molnet service provider). Du kan också ha hello alternativet för att aktivera hello secure sockets layer (SSL)-läge toocreate en säker kanal för nätverkskommunikation mellan din enhet och hello moln.

Du kan skapa flera konton för en viss molntjänstleverantören. Tänk dock på att du inte kan ändra hello molntjänstleverantören efter ett lagringskonto skapas.

Medan hello storage-kontot sparas försöker hello-tjänsten toocommunicate med din molntjänstleverantör. hello autentiseringsuppgifter och hello åtkomst material som du har angett autentiseras just nu. Ett lagringskonto skapas endast om hello autentiseringen lyckas. Om hello autentiseringen misslyckas visas ett felmeddelande visas.

Resource Manager-lagringskonton som skapats i Azure portal även stöds med StorSimple. hello Resource Manager storage-konton inte visas i hello listrutan för val när du försöker toocreate en volymbehållare hello endast storage-konton som skapats i hello klassiska Azure-portalen kommer att visas. Hanteraren för filserverresurser storage-konton måste toobe som lagts till med hello proceduren tooadd ett lagringskonto som beskrivs nedan.

> [!NOTE]
> hello proceduren för att lägga till ett lagringskonto skiljer sig beroende på hello StorSimple programvaruversion du använder. Vara säker på att toofollow hello steg för din StorSimple-version.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Redigera ett lagringskonto
Du kan redigera ett lagringskonto som används av en volymbehållare. Om du redigerar ett lagringskonto som används för närvarande är hello endast toomodify som är tillgängliga i fältet hello åtkomstnyckel för hello storage-konto. Du kan ange hello nya lagringsåtkomstnyckel och spara inställningarna för hello uppdateras.

#### <a name="tooedit-a-storage-account"></a>tooedit ett lagringskonto
1. På hello Landningssida för tjänsten, Välj tjänsten dubbelklicka hello tjänstnamn, och klicka sedan på **konfigurera**.
2. Klicka på **lägga till eller redigera Lagringskonton**.
3. I hello **Lägg till/redigera Lagringskonton** dialogrutan:
   
   1. Hello nedrullningsbara listan med **Lagringskonton**, välja ett befintligt konto som du vill att toomodify. Detta kan också innefatta hello storage-konton som har genererats automatiskt när hello tjänsten skapades.
   2. Om nödvändigt, kan du ändra hello **aktivera SSL-läge** val.
   3. Du kan välja toorotate åtkomstnycklar för lagring konto. Se [nyckeln rotation av lagringskonton](#key-rotation-of-storage-accounts) för mer information om hur tooperform nyckeln rotation.
   4. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) toosave hello inställningar. hello inställningar kommer att uppdateras vid hello **konfigurera** sidan. Klicka på **spara** toosave hello nyligen uppdaterade inställningar.
      
      ![Redigera ett lagringskonto](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Ta bort ett lagringskonto
> [!IMPORTANT]
> Du kan ta bort ett lagringskonto endast om den inte används av en volymbehållare. Om ett lagringskonto används av en volymbehållare kan du först ta bort hello volymbehållare och ta sedan bort hello associerade storage-konto.
> 
> 

#### <a name="toodelete-a-storage-account"></a>toodelete ett lagringskonto
1. På hello Landningssida för StorSimple Manager-tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på **konfigurera**.
2. Hovra över hello-konto som du vill toodelete i hello tabular lista med lagringskonton.
3. En borttagningsikonen (**x**) visas i hello extrema högra kolumnen för det lagringskontot. Klicka på hello **x** ikonen toodelete hello autentiseringsuppgifter.
4. När du uppmanas att bekräfta, klickar du på **Ja** toocontinue med hello borttagning. hello tabular lista kommer att vara uppdaterade tooreflect hello ändringar.

## <a name="key-rotation-of-storage-accounts"></a>Viktiga rotation av storage-konton
Av säkerhetsskäl är viktiga rotation ofta ett krav i datacenter. 

> [!NOTE]
> hello följer viktiga rotation information och hello rotation proceduren gäller endast tooMicrosoft Azure storage-konton. Om du använder en annan molntjänstleverantör kan du hantera nycklar för lagringskonto via instrumentpanelen för den providern.
> 
> 

Varje Microsoft Azure-prenumeration kan ha en eller flera associerade storage-konton. Hej toothese åtkomstkonton styrs av hello prenumeration och snabbtangenterna för varje lagringskonto. 

När du skapar ett lagringskonto genererar Microsoft Azure två 512-bitars lagring åtkomstnycklar som används för autentisering när lagringskontot hello används. Om du har två åtkomstnycklar för lagring kan du tooregenerate hello nycklarna utan avbrott tooyour lagringstjänsten eller toothat-tjänsten för dataåtkomst. hello nyckel som används för närvarande är hello *primära* nyckel och hello reservnyckel är refererad tooas hello *sekundära* nyckel. En av dessa två nycklar måste anges när Microsoft Azure StorSimple-enheten ansluter till din molntjänstleverantör för lagring.

## <a name="what-is-key-rotation"></a>Vad är viktiga rotation?
Normalt använder program endast en av hello nycklar tooaccess dina data. Du kan ha dina program växla över toousing hello andra nyckeln efter en viss tidsperiod. När du har växlat sekundärnyckeln ditt program toohello, kan du dra tillbaka hello första nyckeln och sedan skapa en ny nyckel. Med hjälp av hello två nycklar det här sättet kan dina program åtkomst toohello data utan att det medför några driftavbrott.

Hej lagringskontonycklar måste alltid lagras i hello tjänsten i krypterad form. Dessa kan dock återställas via hello StorSimple Manager-tjänsten. hello kan hämta hello primärnyckeln och sekundärnyckeln för alla hello storage-konton i hello samma prenumeration, inklusive konton som har skapats i hello lagringstjänsten samt hello standardkontona för lagring genereras hello när StorSimple Manager-tjänsten tjänsten skapades. Hej StorSimple Manager-tjänsten kommer alltid hämta nycklarna från hello klassiska Azure-portalen och lagra dem på ett sätt som är krypterade.

## <a name="rotation-workflow"></a>Rotation arbetsflöde
En Microsoft Azure-administratör kan återskapa eller ändra hello primära och sekundära nycklarna genom direkt åtkomst till hello storage-konto (via hello Microsoft Azure Storage service). Hej StorSimple Manager-tjänsten kan inte se ändringen automatiskt.

tooinform hello StorSimple Manager-tjänsten hello förändring, behöver du tooaccess hello StorSimple Manager-tjänsten, komma åt hello storage-konto och sedan synkronisera hello primära och sekundära nycklarna (beroende på vilket som ändrats). hello-tjänsten sedan hämtar hello senaste nyckeln krypterar hello nycklar och skickar hello krypterade nyckeln toohello enhet.

#### <a name="toosynchronize-keys-for-storage-accounts-in-hello-same-subscription-as-hello-service-azure-only"></a>toosynchronize nycklar för lagringskonton hello samma prenumeration som hello-tjänst (Azure)
1. På hello **Services** klickar du på hello **konfigurera** fliken.
2. Klicka på **lägga till eller redigera Lagringskonton**.
3. I dialogrutan för hello hello följande:
   
   1. Välj hello storage-konto med hello nyckel som du vill toosynchronize. Hej lagringskontonycklar krypteras när de visas.
   2. I hello StorSimple Manager-tjänsten behöver du tooupdate hello nyckel som tidigare har ändrats i hello Microsoft Azure Storage-tjänst. Om hello primärnyckeln har ändrats (återskapats), klickar du på **synkronisera primärnyckeln**. Om hello sekundärnyckeln har ändrats, klickar du på **synkronisera sekundärnyckeln**.
      
      ![Synkronisera nycklar](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="toosynchronize-keys-for-storage-accounts-outside-of-hello-service-subscription"></a>toosynchronize nycklar för lagringskonton utanför hello prenumeration på tjänsten
1. På hello **Services** klickar du på hello **konfigurera** fliken.
2. Klicka på **lägga till eller redigera Lagringskonton**.
3. I dialogrutan för hello hello följande:
   
   1. Välj hello storage-konto med hello åtkomstnyckel som du vill tooupdate.
   2. Du behöver tooupdate hello lagringsåtkomstnyckel i hello StorSimple Manager-tjänsten. I det här fallet kan du se hello lagringsåtkomstnyckel. Ange hello ny nyckel i hello **åtkomstnyckeln för Lagringskontot**y-rutan. 
   3. Spara ändringarna. Din lagringsåtkomstnyckel för kontot ska nu uppdateras.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [StorSimple-säkerhet](storsimple-security.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

