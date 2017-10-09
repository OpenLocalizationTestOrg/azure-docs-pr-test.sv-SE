---
title: "aaaDeploy hello StorSimple Enhetshanteraren tjänst i Azure | Microsoft Docs"
description: "Förklarar hur toocreate och ta bort hello StorSimple enheten Manager-tjänsten i hello Azure-portalen och beskriver hur toomanage hello nyckel för tjänstregistrering."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: alkohli
ms.openlocfilehash: b84a907d6b735c8fee7bdc51f9c0074857297d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>Distribuera hello StorSimple Enhetshanteraren tjänst för StorSimple 8000-serien enheter

## <a name="overview"></a>Översikt

Hej StorSimple enheten Manager-tjänsten körs i Microsoft Azure och ansluter toomultiple StorSimple-enheter. När du har skapat hello service kan du använda den toomanage alla hello-enheter som är anslutna toohello StorSimple Enhetshanteraren tjänsten från en enda central plats, vilket minimerar administrativa belastningen.

Den här självstudiekursen beskriver hello steg som krävs för hello skapande, borttagning, migrering av hello-tjänsten och hello hantering av hello nyckel för tjänstregistrering. hello informationen i den här artikeln är tillämpliga endast tooStorSimple 8000-serien enheter. Mer information om virtuella StorSimple-matriser gå för[distribuera StorSimple enheten Manager-tjänsten för din virtuella StorSimple-matrisen](storsimple-virtual-array-manage-service.md).

## <a name="create-a-service"></a>Skapa en tjänst
toocreate en StorSimple Enhetshanteraren tjänst behöver du toohave:

* En prenumeration med ett Enterprise-avtal
* Ett aktivt Microsoft Azure storage-konto
* Hej faktureringsinformation som används för hantering

Endast hello prenumerationer med ett Enterprise-avtal är tillåtna. Microsoft Sponsorship prenumerationer som tilläts i hello klassiska Azure-portalen stöds inte i hello Azure-portalen. Hello efter meddelande när du använder en prenumeration som inte stöds visas:

![Prenumerationen är inte giltigt](./media/storsimple-8000-manage-service/subscription-not-valid.jpg)

Du kan också välja toogenerate standardkontot för lagring när du skapar hello-tjänsten.

En enskild tjänst kan hantera flera enheter. Men en enhet kan sträcka sig över flera tjänster. Stora företag kan ha flera instanser av tjänsten toowork med olika prenumerationer, organisationer eller även distribution platser. 

> [!NOTE]
> Du behöver separata instanser av StorSimple Enhetshanteraren service toomanage StorSimple 8000-serien enheter och virtuella StorSimple-matriser.

Utföra hello följande steg toocreate en tjänst.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


För varje StorSimple Device Manager-tjänst finns hello följande attribut:

* **Namnet** – hello-namn som har tilldelats tooyour StorSimple enheten Manager-tjänsten när den skapades. **hello tjänstens namn kan inte ändras när hello-tjänsten har skapats. Detta gäller även för andra entiteter, till exempel enheter, volymer, volymbehållare och principer för säkerhetskopiering som inte kan i hello Azure-portalen.**
* **Status för** – hello status för hello-tjänsten, som kan vara **Active**, **skapa**, eller **Online**.
* **Plats** – hello geografisk plats i vilka hello StorSimple enheten kommer att distribueras.
* **Prenumerationen** – hello fakturering prenumeration som är associerad med din tjänst.

## <a name="move-a-service-tooazure-portal"></a>Flytta en tooAzure leverantörer
StorSimple 8000-serien kan nu hanteras hello Azure-portalen. Om du har en befintlig service toomanage hello StorSimple-enheter, rekommenderar vi att du flyttar din tjänst toohello Azure-portalen. hello Azure klassiska portal för hello StorSimple Manager-tjänsten är inte tillgänglig efter den 30 September 2017.

hello alternativet toomigrate toohello Azure-portalen är tillgänglig i faser. Om du inte ser en alternativet toomigrate tooAzure portal, men du vill använda toomove och har granskat hello konsekvenserna av migrering, enligt beskrivningen i hello [överväganden för övergången](#considerations-for-transition), kan du [skicka en begäran om](https://aka.ms/ss8000-cx-signup).

### <a name="considerations-for-transition"></a>Överväganden för övergång

Granska hello effekten av migreringen toohello nya Azure-portalen innan du flyttar hello-tjänsten.

#### <a name="before-you-transition"></a>Innan du övergår

* Enheten är Kör uppdatering 3.0 eller senare. Om enheten kör en äldre version, installera hello senaste uppdateringarna. Mer information finns för[installera uppdatering 4](storsimple-8000-install-update-4.md). Om du använder en StorSimple moln installation (8010/8020), kan du skapa en ny installation av moln med uppdatering 4.0. 

* När du är övergick toohello nya Azure-portalen kan använda du inte hello Azure klassiska portal toomanage din StorSimple-enhet.

* hello övergång utan avbrott och det finns inget driftstopp för hello enhet.

* Alla hello StorSimple enheten chefer under hello angetts prenumerationen överförs.

#### <a name="during-hello-transition"></a>Under hello övergång

* Du kan inte hantera din enhet från hello-portalen.
* Åtgärder som till exempel lagringsnivåer och schemalagda säkerhetskopieringar fortsätta toooccur.
* Ta inte bort hello gamla StorSimple-enhetshanterare medan hello övergången pågår.

#### <a name="after-hello-transition"></a>Efter övergången hello

* Du kan inte längre hantera dina enheter från hello klassiska portalen.

* hello befintliga Azure Service Management (ASM) PowerShell-cmdlets stöds inte. Uppdatera hello skript toomanage dina enheter via hello Azure Resource Manager.

* Konfigurationen av tjänsten och bevaras. Alla volymer och säkerhetskopior är också övergick toohello Azure-portalen.

### <a name="begin-transition"></a>Börja övergång

Utför följande steg tootransition hello din tjänst toohello Azure-portalen.

1. Gå tooyour befintlig StorSimple Manager-tjänsten i hello klassiska portalen.

2. Du ser ett meddelande som informerar dig om att hello StorSimple enheten Manager-tjänsten är tillgänglig i hello Azure-portalen. Observera att hello service i hello Azure-portalen är refererad tooas StorSimple enheten Manager-tjänsten.

    ![Meddelande för migrering](./media/storsimple-8000-manage-service/service-transition1.jpg)

    1. Se till att du har granskat hello full effekt av migrering.
    2. Granska hello lista över StorSimple-enhetshanterare som kommer att flyttas från hello klassiska portalen.

3. Klicka på **migrera**. hello övergång börjar och tar några minuter toocomplete.

När hello övergången är klar kan hantera du dina enheter via hello StorSimple enheten Manager-tjänsten i hello Azure-portalen.

I hello Azure-portalen, endast hello StorSimple-enheter som kör uppdatering 3.0 och senare stöds. hello-enheter som kör äldre versioner har begränsat stöd. hello efter tabellen summrizes vilka åtgärder som stöds på hello-enhet som kör versios tidigare tooUpdate 3.0, när du har migrerat från hello klassiska toohello Azure-portalen.

| Åtgärd                                                                                                                       | Stöds      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Registrera en enhet                                                                                                               | Ja            |
| Konfigurera inställningar för enheter, till exempel Allmänt, nätverk och säkerhet                                                                | Ja            |
| Skanna, hämta och installera uppdateringar                                                                                             | Ja            |
| Inaktivera enhet                                                                                                               | Ja            |
| Ta bort enheten                                                                                                                   | Ja            |
| Skapa, ändra och ta bort en volymbehållare                                                                                   | Nej             |
| Skapa, ändra och ta bort en volym                                                                                             | Nej             |
| Skapa, ändra och ta bort en princip för säkerhetskopiering                                                                                      | Nej             |
| Gör en manuell säkerhetskopia                                                                                                            | Nej             |
| Ta en schemalagd säkerhetskopiering                                                                                                         | Inte tillämpligt |
| Återställa från en säkerhetskopieuppsättningen                                                                                                        | Nej             |
| Klona tooa enhet som kör uppdatering 3.0 och senare <br> hello källenheten körs tidigare tooUpdate för version 3.0.                                | Ja            |
| Klona tooa enhet som kör versioner tidigare tooUpdate 3.0                                                                          | Nej             |
| Redundans som källenhet <br> (från en enhet som kör version tidigare tooUpdate 3.0 tooa enhet som kör uppdatering 3.0 och senare)                                                               | Ja            |
| Redundans som målenhet <br> (tooa enhet som kör tidigare tooUpdate för programvara version 3.0)                                                                                   | Nej             |
| Ta bort en avisering                                                                                                                  | Ja            |
| Visa principer för säkerhetskopiering, säkerhetskopieringskatalogen, volymer, volymbehållare, övervakning diagram, jobb och aviseringar som skapats i klassiska portalen | Ja            |
| Aktivera och inaktivera styrenheter                                                                                              | Ja            |


## <a name="delete-a-service"></a>Ta bort en tjänst

Innan du tar bort en tjänst måste du kontrollera att inga anslutna enheter använder den. Om tjänsten hello används, inaktivera hello anslutna enheter. hello inaktivera åtgärden hello anslutning mellan hello enhets- och hello-servern, men bevara hello enhetsdata i hello molnet.

> [!IMPORTANT]
> När en tjänst har tagits bort, kan hello-åtgärden inte ångras. Alla enheter som använder hello service måste toobe Återställ toofactory standardvärden innan den kan användas med en annan tjänst. I det här scenariot hello lokala data på hello enhet samt hello konfigurationen förloras.

Utföra hello följande steg toodelete en tjänst.

### <a name="toodelete-a-service"></a>toodelete en tjänst

1. Sök Hej tjänst du vill ha toodelete. Klicka på **resurser** ikon och sedan indata hello toosearch på lämpligt sätt. Klicka på hello-tjänster som du vill toodelete i hello sökresultat.

    ![Sök tjänsten toodelete](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Då kommer du toohello StorSimple Enhetshanteraren service bladet. Klicka på **Ta bort**.

    ![Ta bort tjänsten](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klicka på **Ja** i hello meddelande om bekräftelse. Det kan ta några minuter för hello service toobe tas bort.

    ![Bekräfta borttagning](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-hello-service-registration-key"></a>Hämta hello nyckel för tjänstregistrering

När du har skapat en tjänst måste tooregister din StorSimple-enhet med hello-tjänsten. tooregister din första StorSimple-enhet du behöver hello nyckel för tjänstregistrering. tooregister ytterligare enheter med en befintlig StorSimple-tjänst, behöver du både hello registreringsnyckel och hello krypteringsnyckel för tjänstdata (som genereras på hello första enheten under registreringen). Läs mer om hello krypteringsnyckel för tjänstdata [StorSimple-säkerhet](storsimple-8000-security.md). Du kan hämta hello registreringsnyckel genom att öppna **nycklar** på StorSimple Enhetshanteraren-bladet.

Utför följande steg tooget hello tjänstens registreringsnyckel hello.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Behåll hello nyckel för tjänstregistrering på en säker plats. Du behöver den här nyckeln, samt hello krypteringsnyckel för tjänstdata, tooregister ytterligare enheter med den här tjänsten. När du har fått hello nyckel för tjänstregistrering, måste du konfigurera din enhet via hello Windows PowerShell för StorSimple-gränssnittet.

Mer information om hur toouse den här nyckeln finns registrering [steg3: konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-hello-service-registration-key"></a>Återskapa hello nyckel för tjänstregistrering
Du behöver tooregenerate en nyckel för tjänstregistrering om du är obligatoriska tooperform viktiga rotation eller om hello listan över tjänstadministratörer har ändrats. När du återskapar nyckeln hello används hello nya nyckeln bara för att registrera följande enheter. hello-enheter som redan har registrerat påverkas inte av den här processen.

Utför följande steg tooregenerate en nyckel för tjänstregistrering hello.

### <a name="tooregenerate-hello-service-registration-key"></a>tooregenerate hello nyckel för tjänstregistrering
1. I hello **StorSimple Enhetshanteraren** bladet går för**Management &gt;**  **nycklar**.
    
    ![Bladet Nycklar](./media/storsimple-8000-manage-service/regenregkey2.png)

2. I hello **nycklar** bladet, klickar du på **återskapa**.

    ![Klicka på Återskapa](./media/storsimple-8000-manage-service/regenregkey3.png)
3. I hello **Generera nyckel för tjänstregistrering** bladet granska hello åtgärd krävs när hello nycklar genereras om. Alla efterföljande hello-enheter som registreras med den här tjänsten använder hello ny registreringsnyckel. Klicka på **återskapa** tooconfirm. Du meddelas när hello återställningen är slutförd.

    ![Bekräfta Generera](./media/storsimple-8000-manage-service/regenregkey4.png)

4. En ny nyckel för tjänstregistrering visas.

5. Kopiera den här nyckeln och spara den för att registrera nya enheter med den här tjänsten.



## <a name="change-hello-service-data-encryption-key"></a>Ändra hello krypteringsnyckel för tjänstdata
Tjänsten datakrypteringsnycklar är används tooencrypt konfidentiell kundinformation, till exempel lagringskontouppgifter som skickas från din StorSimple Manager service toohello StorSimple-enhet. Du behöver toochange nycklarna regelbundet om organisationen har en princip för viktiga rotation på hello lagringsenheter. hello process viktiga förändringar kan skilja sig något beroende på om det finns en enstaka enhet eller flera enheter som hanteras av hello StorSimple Manager-tjänsten. Mer information finns för[StorSimple säkerhet och dataskydd](storsimple-8000-security.md).

Krypteringsnyckel för tjänstdata ändra hello är en process i steg 3:

1. Med Windows PowerShell-skript för Azure Resource Manager, auktorisera en enhet toochange hello krypteringsnyckel för tjänstdata.
2. Använder Windows PowerShell för StorSimple, initiera förändring för nyckel hello-service-kryptering.
3. Om du har mer än en StorSimple-enhet kan du uppdatera hello krypteringsnyckel för tjänstdata på hello andra enheter.

### <a name="step-1-use-windows-powershell-script-tooauthorize-a-device-toochange-hello-service-data-encryption-key"></a>Steg 1: Använd Windows PowerShell-skript tooAuthorize en enhet toochange hello krypteringsnyckel för tjänstdata
Vanligtvis begär hello enhetsadministratör hello service administratören godkänna en enhet toochange service datakrypteringsnycklar. Hej administratör sedan godkänner hello enhetsnyckel toochange hello.

Det här steget utförs med hjälp av hello Azure Resource Manager-baserat skript. Hej administratör kan välja en enhet som är berättigade toobe behörighet. hello enhet är sedan behöriga toostart hello krypteringsnyckel för tjänstdata ändra processen. 

Mer information om hur du använder skriptet hello gå för[auktorisera ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-toochange-service-data-encryption-keys"></a>Vilka enheter kan auktoriseras toochange service datakrypteringsnycklar?
En enhet måste uppfylla följande villkor innan det kan behöriga tooinitiate service encryption key dataändringar hello:

* hello enheten måste vara online toobe som är berättigade till auktoriseringen av tjänsten data kryptering ändringen.
* Du kan tillåta hello samma enhet igen efter 30 minuter ändrar hello nyckeln inte har initierats.
* Du kan tillåta en annan enhet, förutsatt att hello ändringen inte har initierats av hello tidigare godkänd enhet. När hello ny enhet har auktoriserats kan inte hello gamla enheten initiera hello ändringen.
* Du kan inte godkänna en enhet när hello förnyelse av krypteringsnyckel för tjänstdata hello pågår.
* Du kan godkänna en enhet när några av hello enheter som har registrerats med hello-tjänsten har flyttats över hello kryptering medan andra inte har. 

### <a name="step-2-use-windows-powershell-for-storsimple-tooinitiate-hello-service-data-encryption-key-change"></a>Steg 2: Använda Windows PowerShell för StorSimple tooinitiate hello tjänsten data encryption key ändras
Det här steget utförs i hello Windows PowerShell för StorSimple-gränssnittet på hello behörighet StorSimple-enhet.

> [!NOTE]
> Inga åtgärder kan utföras i hello Azure-portalen för din StorSimple Manager-tjänsten tills hello nyckelförnyelse har slutförts.
> 
> 

Om du använder hello enhetens seriekonsol tooconnect toohello Windows PowerShell-gränssnittet, utföra hello följande steg.

#### <a name="tooinitiate-hello-service-data-encryption-key-change"></a>Ändra tooinitiate hello krypteringsnyckel för tjänstdata
1. Välj alternativ 1 toolog med fullständig åtkomst.
2. I hello kommandotolk, skriver du:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. När hello cmdleten har slutförts får du en ny krypteringsnyckel för tjänstdata. Kopiera och spara den här nyckeln för användning i steg 3 i den här processen. Den här nyckeln kommer att användas tooupdate alla hello återstående enheter som har registrerats med hello StorSimple Manager-tjänsten.
   
   > [!NOTE]
   > Den här processen måste initieras inom fyra timmar för att auktorisera en StorSimple-enhet.
   > 
   > 
   
   Den här nya nyckeln skickas toohello service toobe pushas tooall hello enheter som är registrerade med hello-tjänsten. En avisering visas sedan på instrumentpanelen för hello-tjänsten. hello-tjänsten kommer att inaktivera alla hello åtgärder på hello registrerade enheter och hello enhetsadministratör måste sedan tooupdate hello krypteringsnyckel för tjänstdata på hello andra enheter. Dock kommer hello I/o (värdar skickar data toohello moln) inte att avbrytas.
   
   Om du har en enda enhet registrerats tooyour service, hello förnyelse processen är slutförd och du kan hoppa över hello nästa steg. Om du har flera enheter registrerade tooyour tjänsten fortsätta toostep 3.

### <a name="step-3-update-hello-service-data-encryption-key-on-other-storsimple-devices"></a>Steg 3: Uppdatera hello krypteringsnyckel för tjänstdata på andra StorSimple-enheter
Dessa steg måste utföras i hello Windows PowerShell-gränssnittet för din StorSimple-enhet om du har flera enheter registrerade tooyour StorSimple Manager-tjänsten. hello-nyckel som du fick i steg 2 måste vara används tooupdate alla hello återstående StorSimple-enheten registrerad med hello StorSimple Manager-tjänsten.

Utföra hello följande steg tooupdate hello service datakryptering på enheten.

#### <a name="tooupdate-hello-service-data-encryption-key"></a>tooupdate hello krypteringsnyckel för tjänstdata
1. Använda Windows PowerShell för StorSimple tooconnect toohello-konsolen. Välj alternativ 1 toolog med fullständig åtkomst.
2. I hello kommandotolk, skriver du:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Ange hello krypteringsnyckel för tjänstdata som du fick i [steg 2: använda Windows PowerShell för StorSimple tooinitiate hello tjänsten data encryption key ändringen](#to-initiate-the-service-data-encryption-key-change).


## <a name="next-steps"></a>Nästa steg
* Mer information om hello [StorSimple distributionsprocessen](storsimple-8000-deployment-walkthrough-u2.md).
* Lär dig mer om [hantera ditt lagringskonto i StorSimple](storsimple-8000-manage-storage-accounts.md).
* Läs mer om hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).
