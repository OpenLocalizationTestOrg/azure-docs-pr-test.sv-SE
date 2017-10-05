---
title: "Distribuera StorSimple enheten Manager-tjänsten i Azure | Microsoft Docs"
description: "Beskriver hur du skapar och tar bort StorSimple enheten Manager-tjänsten i Azure-portalen och beskriver hur du hanterar nyckeln för tjänstregistrering."
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
ms.openlocfilehash: 22bb4a32f006d7e49356743c2a87eb622a61d18e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>Distribuera StorSimple enheten Manager-tjänsten för StorSimple 8000-serien enheter

## <a name="overview"></a>Översikt

StorSimple Device Manager-tjänsten körs i Microsoft Azure och ansluter till flera StorSimple-enheter. När du har skapat tjänsten kan den använda och hantera alla enheter som är anslutna till StorSimple enheten Manager-tjänsten från en enda central plats, vilket minimerar administrativa belastningen.

Den här självstudiekursen beskriver de steg som krävs för att skapa, borttagning, migrering av tjänsten och hantering av nyckel för tjänstregistrering. Informationen i den här artikeln gäller endast StorSimple 8000-serien enheter. Mer information om virtuella StorSimple-matriser, gå till [distribuera StorSimple enheten Manager-tjänsten för din virtuella StorSimple-matrisen](storsimple-virtual-array-manage-service.md).

## <a name="create-a-service"></a>Skapa en tjänst
Om du vill skapa en StorSimple Device Manager-tjänst måste du ha:

* En prenumeration med ett Enterprise-avtal
* Ett aktivt Microsoft Azure storage-konto
* Faktureringsinformation som används för hantering

Endast prenumerationer med ett Enterprise-avtal är tillåtna. Microsoft Sponsorship prenumerationer som tilläts i den klassiska Azure portalen stöds inte i Azure-portalen. Följande meddelande visas när du använder en prenumeration som inte stöds:

![Prenumerationen är inte giltigt](./media/storsimple-8000-manage-service/subscription-not-valid.jpg)

Du kan också välja att generera en standardkontot för lagring när du skapar tjänsten.

En enskild tjänst kan hantera flera enheter. Men en enhet kan sträcka sig över flera tjänster. Stora företag kan ha flera instanser av tjänsten ska fungera med olika prenumerationer, organisationer eller även distribution platser. 

> [!NOTE]
> Du behöver separata instanser av Enhetshanteraren för StorSimple-tjänsten för att hantera StorSimple 8000-serien enheter och virtuella StorSimple-matriser.

Utför följande steg för att skapa en tjänst.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


För varje tjänst StorSimple Enhetshanteraren finns följande attribut:

* **Namnet** – namn som har tilldelats till din StorSimple Device Manager-tjänsten när den skapades. **Tjänstens namn kan inte ändras efter att tjänsten har skapats. Detta gäller även för andra entiteter, till exempel enheter, volymer, volymbehållare och principer för säkerhetskopiering som inte kan i Azure-portalen.**
* **Status för** – status för tjänsten, som kan vara **Active**, **skapa**, eller **Online**.
* **Plats** – den geografiska plats där StorSimple-enheten ska distribueras.
* **Prenumerationen** – faktureringsadministratör prenumerationen som är associerad med din tjänst.

## <a name="move-a-service-to-azure-portal"></a>Flytta en tjänst till Azure-portalen
StorSimple 8000-serien kan nu hanteras i Azure-portalen. Om du har en befintlig tjänst för att hantera StorSimple-enheter, rekommenderar vi att du flyttar din tjänst till Azure-portalen. Den klassiska Azure-portalen för StorSimple Manager-tjänsten är inte tillgänglig efter den 30 September 2017.

Alternativet att migrera till Azure Portal är tillgängligt i faser. Om du inte ser något alternativ för att migrera till Azure Portal men vill flytta och har läst igenom [informationen om saker att tänka på vid migrering ](#considerations-for-transition) kan du [skicka en förfrågan](https://aka.ms/ss8000-cx-signup).

### <a name="considerations-for-transition"></a>Överväganden för övergång

Granska effekten av migreringen till den nya Azure-portalen innan du flyttar tjänsten.

#### <a name="before-you-transition"></a>Innan du övergår

* Enheten är Kör uppdatering 3.0 eller senare. Om enheten kör en äldre version kan du installera de senaste uppdateringarna. Mer information finns på [installera uppdatering 4](storsimple-8000-install-update-4.md). Om du använder en StorSimple moln installation (8010/8020), kan du skapa en ny installation av moln med uppdatering 4.0. 

* När du har gått över till den nya Azure-portalen, kan du inte använda den klassiska Azure-portalen för att hantera din StorSimple-enhet.

* Övergången utan avbrott och det finns inget driftstopp för enheten.

* Alla StorSimple-enhet chefer under den angivna prenumerationen överförs.

#### <a name="during-the-transition"></a>Under övergången

* Du kan inte hantera din enhet från portalen.
* Åtgärder som till exempel lagringsnivåer och schemalagda säkerhetskopieringar fortsätta ska ske.
* Ta inte bort de gamla StorSimple enheten chefer medan övergången pågår.

#### <a name="after-the-transition"></a>Efter övergången

* Du kan inte längre hantera dina enheter från den klassiska portalen.

* De befintliga Azure Service Management (ASM) PowerShell-cmdletarna stöds inte. Uppdatera skript för att hantera dina enheter via Azure Resource Manager.

* Konfigurationen av tjänsten och bevaras. Alla volymer och säkerhetskopieringar överförs till Azure-portalen.

### <a name="begin-transition"></a>Börja övergång

Utför följande steg om du vill överföra din tjänst till Azure-portalen.

1. Gå till befintlig StorSimple Manager-tjänsten i den klassiska portalen.

2. Du ser ett meddelande som informerar dig om att StorSimple enheten Manager-tjänsten är tillgänglig i Azure-portalen. Observera att i Azure-portalen tjänsten kallas StorSimple enheten Manager-tjänsten.

    ![Meddelande för migrering](./media/storsimple-8000-manage-service/service-transition1.jpg)

    1. Se till att du har granskat fullständig effekten av migreringen.
    2. Granska listan över StorSimple enhetshanterare som kommer att flyttas från den klassiska portalen.

3. Klicka på **migrera**. Övergången börjar och tar några minuter att slutföra.

När övergången är klar kan hantera du dina enheter via tjänsten StorSimple Enhetshanteraren i Azure-portalen.

I Azure portal stöds bara på StorSimple-enheter som kör uppdatering 3.0 och senare. De enheter som kör äldre versioner har begränsat stöd. Följande tabell summrizes vilka åtgärder som stöds på den enhet som kör versios före uppdateringen 3.0 när du har migrerat från klassiskt Azure-portalen.

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
| Klona till en enhet som kör uppdatering 3.0 och senare <br> På källenheten kör version före uppdateringen 3.0.                                | Ja            |
| Klona till en enhet som kör versioner före uppdateringen 3.0                                                                          | Nej             |
| Redundans som källenhet <br> (från en enhet som kör version före uppdateringen 3.0 till en enhet som kör uppdatering 3.0 och senare)                                                               | Ja            |
| Redundans som målenhet <br> (för att en enhet som kör programvaruversion före uppdateringen 3.0)                                                                                   | Nej             |
| Ta bort en avisering                                                                                                                  | Ja            |
| Visa principer för säkerhetskopiering, säkerhetskopieringskatalogen, volymer, volymbehållare, övervakning diagram, jobb och aviseringar som skapats i klassiska portalen | Ja            |
| Aktivera och inaktivera styrenheter                                                                                              | Ja            |


## <a name="delete-a-service"></a>Ta bort en tjänst

Innan du tar bort en tjänst måste du kontrollera att inga anslutna enheter använder den. Om tjänsten inte används, inaktivera anslutna enheter. Åtgärden inaktivera sever anslutningen mellan enheten och tjänsten, men behålla enhetsdata i molnet.

> [!IMPORTANT]
> När en tjänst har tagits bort, kan åtgärden inte ångras. Alla enheter som använder tjänsten måste återställas till fabriksinställningarna innan den kan användas med en annan tjänst. I det här scenariot förloras lokala data på enheten, samt konfigurationen.

Utför följande steg för att ta bort en tjänst.

### <a name="to-delete-a-service"></a>Ta bort en tjänst

1. Sök efter tjänsten som du vill ta bort. Klicka på **resurser** ikon och sedan ange lämpliga villkor för att söka. I sökresultaten klickar du på tjänsten som du vill ta bort.

    ![Söktjänsten att ta bort](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Då kommer du till bladet StorSimple Device Manager-tjänsten. Klicka på **Ta bort**.

    ![Ta bort tjänsten](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klicka på **Ja** i meddelande om bekräftelse. Det kan ta några minuter för tjänsten som ska tas bort.

    ![Bekräfta borttagning](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>Hämta nyckel för tjänstregistrering

När du har skapat en tjänst måste registrera din StorSimple-enhet med tjänsten. Om du vill registrera din första StorSimple-enhet, måste nyckeln för tjänstregistrering. Om du vill registrera ytterligare enheter med en befintlig StorSimple-tjänst, behöver du både Registreringsnyckeln och den krypteringsnyckel för tjänstdata (som genereras på den första enheten under registreringen). Läs mer om krypteringsnyckeln för tjänstdata [StorSimple-säkerhet](storsimple-8000-security.md). Du kan hämta nyckel för tjänstregistrering genom att öppna **nycklar** på StorSimple Enhetshanteraren-bladet.

Utför följande steg för att hämta nyckel för tjänstregistrering.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Behåll nyckel för tjänstregistrering på en säker plats. Du behöver den här nyckeln, samt krypteringsnyckeln för tjänstdata, att registrera ytterligare enheter med den här tjänsten. När du har fått nyckeln för tjänstregistrering, måste du konfigurera din enhet via Windows PowerShell för StorSimple-gränssnittet.

Mer information om hur du använder den här nyckeln för tjänstregistrering finns [steg3: konfigurera och registrera enheten via Windows PowerShell för StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Återskapa nyckeln för tjänstregistrering
Du måste återskapa en nyckel för tjänstregistrering om du behöver utföra viktiga rotation eller om listan över tjänstadministratörer har ändrats. När du återskapar nyckeln används den nya nyckeln bara för att registrera följande enheter. De enheter som redan har registrerats påverkas inte av den här processen.

Utför följande steg för att återskapa en nyckel för tjänstregistrering.

### <a name="to-regenerate-the-service-registration-key"></a>Återskapa nyckeln för tjänstregistrering
1. I den **StorSimple Enhetshanteraren** gå till bladet **Management &gt;**  **nycklar**.
    
    ![Bladet Nycklar](./media/storsimple-8000-manage-service/regenregkey2.png)

2. I den **nycklar** bladet, klickar du på **återskapa**.

    ![Klicka på Återskapa](./media/storsimple-8000-manage-service/regenregkey3.png)
3. I den **Generera nyckel för tjänstregistrering** bladet, granska åtgärden krävs när nycklar genereras om. Alla efterföljande enheter som registreras med den här tjänsten använder den nya Registreringsnyckeln. Klicka på **återskapa** att bekräfta. Du meddelas när omgenerering är klar.

    ![Bekräfta Generera](./media/storsimple-8000-manage-service/regenregkey4.png)

4. En ny nyckel för tjänstregistrering visas.

5. Kopiera den här nyckeln och spara den för att registrera nya enheter med den här tjänsten.



## <a name="change-the-service-data-encryption-key"></a>Ändra krypteringsnyckeln för tjänstdata
Tjänsten datakrypteringsnycklar används för att kryptera konfidentiell kundinformation, till exempel lagringskontouppgifter som skickas från din StorSimple Manager-tjänsten till StorSimple-enhet. Du behöver ändra dessa nycklar regelbundet om organisationen har en key rotation princip på lagringsenheter. Process för viktiga förändringar kan variera beroende på om det finns en enstaka enhet eller flera enheter som hanteras av StorSimple Manager-tjänsten. Mer information finns på [StorSimple säkerhet och dataskydd](storsimple-8000-security.md).

Krypteringsnyckel för tjänstdata är en process i steg 3:

1. Med Windows PowerShell-skript för Azure Resource Manager, auktorisera en enhet för att ändra krypteringsnyckeln för tjänstdata.
2. Använder Windows PowerShell för StorSimple, starta tjänsten data encryption key ändras.
3. Om du har mer än en StorSimple-enhet kan du uppdatera krypteringsnyckeln för tjänstdata på andra enheter.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Steg 1: Använd Windows PowerShell-skript för att auktorisera en enhet för att ändra krypteringsnyckeln för tjänstdata
Vanligtvis begär enhetsadministratören att administratören godkänna en enhet för att ändra datakrypteringsnycklar för tjänsten. Tjänstadministratören godkänner sedan enheten för att ändra nyckeln.

Det här steget utförs med hjälp av Azure Resource Manager-baserat skript. Administratören kan välja en enhet som kan godkännas. Enheten har sedan behörighet att starta tjänsten data krypteringsprocessen ändringen. 

Mer information om hur du använder skriptet går du till [auktorisera ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Vilka enheter kan ha behörighet att ändra tjänsten datakrypteringsnycklar?
En enhet måste uppfylla följande kriterier innan den kan ha behörighet att starta tjänsten kryptering viktiga ändringar:

* Enheten måste vara online för att vara kvalificerad för auktoriseringen av tjänsten data kryptering ändringen.
* Du kan godkänna på samma enhet igen efter 30 minuter om viktiga ändringen inte har initierats.
* Du kan godkänna en annan enhet, under förutsättning att ändringen inte har initierats av tidigare godkänd enheten. När den nya enheten har auktoriserats, kan inte gamla enheten initiera ändringen.
* Du kan inte godkänna en enhet när förnyelsen av datakrypteringsnyckeln för tjänsten pågår.
* Du kan godkänna en enhet när några av de enheter som registrerats för tjänsten har återställts via kryptering medan andra inte har. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Steg 2: Använda Windows PowerShell för StorSimple att initiera tjänsten data encryption key ändras
Det här steget utförs i Windows PowerShell för StorSimple-gränssnittet på behöriga StorSimple-enheten.

> [!NOTE]
> Inga åtgärder kan utföras i Azure-portalen för din StorSimple Manager-tjänsten tills nyckelförnyelse har slutförts.
> 
> 

Utför följande steg om du använder enhetens seriekonsol för att ansluta till Windows PowerShell-gränssnittet.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Att starta tjänsten data encryption key ändras
1. Välj alternativ 1 för att logga in med fullständig åtkomst.
2. Skriv följande vid kommandotolken:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. När cmdleten har slutförts får du en ny krypteringsnyckel för tjänstdata. Kopiera och spara den här nyckeln för användning i steg 3 i den här processen. Den här nyckeln används för att uppdatera alla enheter som registrerats med StorSimple Manager-tjänsten.
   
   > [!NOTE]
   > Den här processen måste initieras inom fyra timmar för att auktorisera en StorSimple-enhet.
   > 
   > 
   
   Den här nya nyckeln skickas sedan till tjänsten ska kunna skickas till alla enheter som registreras med tjänsten. En avisering visas sedan på instrumentpanelen för tjänsten. Tjänsten kommer att inaktivera alla åtgärder på registrerade enheter och enhetsadministratören sedan måste du uppdatera krypteringsnyckeln för tjänstdata på andra enheter. I/o (värdar skicka data till molnet) kommer inte att avbrytas.
   
   Om du har en enda enhet som registrerats till tjänsten förnyelse processen är slutförd och du kan hoppa över nästa steg. Om du har flera enheter som registrerats till tjänsten kan du gå vidare till steg 3.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Steg 3: Uppdatera krypteringsnyckeln för tjänstdata på andra StorSimple-enheter
Dessa steg måste utföras i Windows PowerShell-gränssnittet för din StorSimple-enhet om du har flera enheter som registrerats med StorSimple Manager-tjänsten. Den nyckel som du fick i steg 2 måste användas för att uppdatera alla återstående StorSimple-enheten registrerad med StorSimple Manager-tjänsten.

Utför följande steg för att uppdatera tjänsten datakryptering på enheten.

#### <a name="to-update-the-service-data-encryption-key"></a>Uppdatera krypteringsnyckeln för tjänstdata
1. Använda Windows PowerShell för StorSimple för att ansluta till konsolen. Välj alternativ 1 för att logga in med fullständig åtkomst.
2. Skriv följande vid kommandotolken:
   
    `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Ange den krypteringsnyckel för tjänstdata som du fick i [steg 2: använda Windows PowerShell för StorSimple att initiera tjänsten data encryption key ändras](#to-initiate-the-service-data-encryption-key-change).


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om den [StorSimple distributionsprocessen](storsimple-8000-deployment-walkthrough-u2.md).
* Lär dig mer om [hantera ditt lagringskonto i StorSimple](storsimple-8000-manage-storage-accounts.md).
* Mer information om hur du [använda Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).
