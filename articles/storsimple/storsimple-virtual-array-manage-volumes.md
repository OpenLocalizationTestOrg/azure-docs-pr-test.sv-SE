---
title: "aaaManage volymer på virtuella StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hello StorSimple Enhetshanteraren och förklarar hur toouse den toomanage volymer på din virtuella StorSimple-matrisen."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 46aa6d7508b3e62f75a3b78ed73302b88320a0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-service-toomanage-volumes-on-hello-storsimple-virtual-array"></a>Använd StorSimple Enhetshanteraren service toomanage volymer på hello virtuella StorSimple-matris

## <a name="overview"></a>Översikt

Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera volymer på din virtuella StorSimple-matrisen.

Hej StorSimple enheten Manager-tjänsten är ett tillägg i hello Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt. I tillägg toomanaging filresurser och volymer kan du använda hello StorSimple Enhetshanteraren service tooview och hantera enheter, Visa aviseringar, och visa och hantera principer för säkerhetskopiering och hello säkerhetskopieringskatalogen.

## <a name="volume-types"></a>Volymtyper

StorSimple-volymer kan vara:

* **Lokalt Fäst**: Data på dessa volymer förblir på hello matrisen och inte läcker över toohello moln.
* **Nivåer**: Data i dessa volymer kan läcker över toohello moln. När du skapar en nivåindelad volym cirka 10% av hello utrymme har etablerats på hello lokala nivån och 90% av hello utrymme har etablerats i hello molnet. Till exempel om du har etablerat en volym 1 TB, 100 GB skulle finns i hello lokalt utrymme och 900 GB ska användas i hello molnet när hello datanivåer. I sin tur innebär detta att du slut alla hello lokalt utrymme på hello enheten inte kan etablera en nivåindelad volym (eftersom hello 10% krävs på hello lokala nivå inte är tillgänglig).

### <a name="provisioned-capacity"></a>Etablerad kapacitet
Läs toohello i den följande tabellen för maximal etablerad kapacitet för varje volym.

| **Gränsen identifierare**                                       | **Gränsen**     |
|------------------------------------------------------------|---------------|
| Minsta storlek på en nivåindelad volym                            | 500 GB        |
| Maximal storlek för en nivåindelad volym                            | 5 TB          |
| Minsta storleken för en lokalt Fäst volym                    | 50 GB         |
| Maximal storlek för en lokalt Fäst volym                    | 500 GB        |

## <a name="hello-volumes-blade"></a>hello volymer bladet
Hej **volymer** menyn på din StorSimple-tjänsten sammanfattning bladet visar hello lista över lagringsvolymer på en viss StorSimple-matris och gör att du toomanage dem.

![Volymer bladet](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

En volym som består av en serie med attribut:

* **Volymnamn** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello volym.
* **Status för** – kan vara online eller offline. Om en volym är offline, men det är inte synliga tooinitiators (servrar) som tillåts åtkomst toouse hello volym.
* **Typen** – anger om hello volymen är **skiktindelade** (hello standard) eller **lokalt Fäst**.
* **Kapacitet** – anger hello mängden data som används som jämfört med toohello totala mängden data som kan lagras av hello initierare (server).
* **Säkerhetskopiering** – om av hello virtuella StorSimple-matris, aktiveras automatiskt alla volymer för säkerhetskopiering.
* **Ansluten värdar** – anger hello initierarna (servrarna) som tillåts åtkomst toothis volym.

![Information om volymer](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Använd hello instruktioner i den här självstudiekursen tooperform hello följande uppgifter:

* Lägg till en volym
* Ändra en volym
* Koppla från en volym
* Ta bort en volym

## <a name="add-a-volume"></a>Lägg till en volym

1. Från hello StorSimple-tjänsten sammanfattning-bladet, klickar du på **+ Lägg till volymen** från hello kommandofältet. Detta öppnar hello **Lägg till volymen** bladet.
   
    ![Lägg till volym](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. I hello **Lägg till volymen** bladet hello följande:
   
   * I hello **volymnamn** , ange ett unikt namn för volymen. hello namn måste vara en sträng som innehåller 3 too127 tecken.
   * I hello **typen** listrutan anger om toocreate en **skiktindelade** eller **lokalt Fäst** volym. Arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, Välj **lokalt Fäst volym**. Alla andra data, Välj **skiktindelade** volym.
   * I hello **kapacitet** anger hello storleken på hello volym. En nivåindelad volym måste vara mellan 500 GB och 5 TB och en lokalt Fäst volym måste vara mellan 50 och 500 GB.
   * * Klicka på **anslutna värdar**, Välj en access control post (ACR) motsvarande toohello iSCSI-initierare du vill tooconnect toothis volymen och klickar sedan på **Välj**.
3. tooadd en ny anslutna värddator klickar du på **Lägg till ny**, ange ett namn för hello värden och dess iSCSI-kvalificerade namn (IQN) och klicka sedan på **Lägg till**.
   
    ![Lägg till volym](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. När du har konfigurerat din volym klickar du på **skapa**. En volym skapas med hello angetts inställningar och du visas ett meddelande på hello har skapats hello samma. Som standard aktiveras säkerhetskopiering för hello volym.
5. tooconfirm som hello volymen har har skapats, gå toohello **volymer** bladet. Du bör se hello volym som visas.
   
    ![Volymen skapa lyckades](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Ändra en volym

Ändra en volym när du behöver toochange hello värdar som har åtkomst till hello volym. hello kan andra attribut för en volym inte ändras när hello volymen har skapats.

#### <a name="toomodify-a-volume"></a>toomodify en volym

1. Från hello **volymer** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris vilka hello volymen som du vill toomodify finns.
2. **Välj** hello volymen och klickar på **anslutna värdar** tooview hello anslutna värden och ändra den tooa annan server.
   
    ![Redigera volym](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Spara dina ändringar genom att klicka på hello **spara** kommandofältet. Inställningarna för angivna tillämpas och ett meddelande visas.

## <a name="take-a-volume-offline"></a>Koppla från en volym

Du kanske måste tootake en volym offline när du planerar toomodify den eller ta bort den. När en volym är offline, är den inte tillgänglig för skrivskyddad åtkomst. Du behöver tootake hello volymen offline på värden för hello samt på hello enhet.

#### <a name="tootake-a-volume-offline"></a>tootake en volym som är offline

1. Kontrollera att hello volymen i fråga inte används innan den tas offline.
2. Ta hello volymen offline på värden för hello första. Detta tar bort alla potentiella risken för korrupta data på hello volym. Specifika anvisningar finns i toohello instruktioner för operativsystemet för värden.
3. När hello volymen på hello värden är offline kan du ta hello volymen på hello matrisen offline genom att utföra följande steg hello:
   
   * Från hello **volymer** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris vilka hello volymen som du vill tootake offline finns.
   * **Välj** hello volymen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta offline**.
     
        ![Offline volym](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Studera hello hello **ta offline** bladet och bekräfta ditt godkännande av hello igen. Klicka på **ta offline** tootake hello volymen offline. Visas ett meddelande om hello-åtgärd pågår.
   * tooconfirm hello volymen har sparats offline, gå toohello **volymer** bladet. Du bör se hello status för hello volym som offline.
     
       ![Offline volym bekräftelse](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Ta bort en volym

> [!IMPORTANT]
> Du kan ta bort en volym endast om den är offline.
> 
> 

Slutför följande steg toodelete en volym hello.

#### <a name="toodelete-a-volume"></a>toodelete en volym

1. Från hello **volymer** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris vilka hello volymen som du vill toodelete finns.
2. **Välj** hello volymen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta bort**.
   
    ![Ta bort volym](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Kontrollera status för hello hello volym som du vill toodelete. Om hello volymen som du vill toodelete inte är offline kan försättas offline första, följande hello steg [kopplar från en volym](#take-a-volume-offline).
4. När du uppmanas att bekräfta i hello **ta bort** bladet acceptera hello bekräftelse och klicka på **ta bort**. hello volymen tas bort och hello **volymer** bladet visar hello uppdaterad lista över volymer inom hello virtuella matris.

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[klona en StorSimple-volym](storsimple-virtual-array-clone.md).

