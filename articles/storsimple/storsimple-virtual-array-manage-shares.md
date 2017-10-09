---
title: aaaManage virtuella StorSimple-matris delar | Microsoft Docs
description: "Beskriver hello StorSimple Enhetshanteraren och förklarar hur toouse den toomanage resurser på din virtuella StorSimple-matrisen."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9b57d7ec7c0b7de5a22e1b816daa8852d0f32a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-shares-on-hello-storsimple-virtual-array"></a>Använda hello StorSimple Enhetshanteraren service toomanage resurser på hello virtuella StorSimple-matris

## <a name="overview"></a>Översikt

Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera resurser på din virtuella StorSimple-matrisen.

Hej StorSimple enheten Manager-tjänsten är ett tillägg i hello Azure-portalen där du kan hantera din StorSimple-lösning från ett enda webbgränssnitt. I tillägg toomanaging filresurser och volymer kan du använda hello StorSimple Enhetshanteraren service tooview och hantera enheter, Visa aviseringar, hantera principer för säkerhetskopiering och hantera hello säkerhetskopieringskatalogen.

## <a name="share-types"></a>Dela typer

StorSimple resurser kan vara:

* **Lokalt Fäst**: Data i resurserna ligger på hello matrisen vid alla tidpunkter och inte läcker över toohello moln.
* **Nivåer**: Data i resurserna kan läcker över toohello moln. När du skapar en nivåindelad resurs cirka 10% av hello utrymme har etablerats på hello lokala nivån och 90% av hello utrymme har etablerats i hello molnet. Till exempel om du har etablerat en resurs för 1 TB, 100 GB skulle finns i hello lokalt utrymme och 900 GB ska användas i hello molnet när hello datanivåer. I sin tur innebär detta att du slut alla hello lokalt utrymme på hello enheten inte kan etablera en skiktad resurs (eftersom hello 10% krävs på hello lokala nivå inte är tillgänglig).

### <a name="provisioned-capacity"></a>Etablerad kapacitet

Mer information finns i den följande tabellen för maximal kapacitet för varje resurstyp av som etablerade toohello.

| **Gränsen identifierare** | **Gränsen** |
| --- | --- |
| Minsta storlek på en skiktad resurs |500 GB |
| Maximal storlek för en skiktad resurs |20 TB |
| Minsta storleken för en lokalt Fäst resurs |50 GB |
| Maximal storlek för en lokalt Fäst resurs |2 TB |

## <a name="hello-shares-blade"></a>hello resurser bladet

Hej **resurser** menyn på din StorSimple-tjänsten sammanfattning bladet visar hello listan över resurser för lagring på en viss StorSimple-matris och gör att du toomanage dem.

![Resurser-bladet](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

En resurs består av en serie med attribut:

* **Resursnamn** – ett beskrivande namn som måste vara unikt och hjälper till att identifiera hello resursen.
* **Status för** – kan vara online eller offline. Om en resurs som är offline, användare hello-resursen inte kan tooaccess den.
* **Typen** – anger om hello resursen är **skiktindelade** (hello standard) eller **lokalt Fäst**.
* **Kapacitet** – anger hello mängden data som används som jämfört med toohello totala mängden data som kan lagras på hello-resurs.
* **Beskrivning** – en valfri inställning som hjälper till att beskriva hello resursen.
* **Behörigheter** -hello NTFS-behörigheter toohello resurs som kan hanteras via Utforskaren.
* **Säkerhetskopiering** – om av hello virtuella StorSimple-matris, aktiveras automatiskt alla resurser för säkerhetskopiering.

![Delar information](./media/storsimple-virtual-array-manage-shares/share-details.png)

Använd hello instruktioner i den här självstudiekursen tooperform hello följande uppgifter:

* Lägg till en resurs
* Ändra en resurs
* Koppla från en resurs
* Ta bort en resurs

## <a name="add-a-share"></a>Lägg till en resurs

1. Från hello StorSimple-tjänsten sammanfattning-bladet, klickar du på **+ Lägg till resursen** från hello kommandofältet. Detta öppnar hello **Lägg till resursen** bladet.

    ![Lägg till resurs](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. I hello **Lägg till resursen** bladet hello följande:
   
    1. I hello **resursnamn** , ange ett unikt namn för din resurs. hello namn måste vara en sträng som innehåller 3 too127 tecken.

    2. En valfri **beskrivning** för hello resurs. hello beskrivning hjälper dig att identifiera hello resurs-ägare.

    3. I hello **typen** listrutan anger om toocreate en **skiktindelade** eller **lokalt Fäst** delar. Arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, Välj **lokalt Fäst resursen**. Alla andra data, Välj **skiktindelade** delar.

    4. I hello **kapacitet** anger hello storlek hello-resursen. En skiktad resursen måste vara mellan 500 GB och 20 TB och en lokalt Fäst resursen måste vara mellan 50 GB och 2 TB.

    5. I hello **Ange standard fullständig behörighet** fältet, tilldela hello behörigheter toohello användare eller hello-grupp som har åtkomst till resursen. Ange hello namn hello användaren eller användargruppen för hello i  _john@contoso.com_  format. Vi rekommenderar att du använder en användare grupp (i stället för en enskild användare) tooallow admin privilegier tooaccess dessa resurser. När du har tilldelat hello behörigheter här kan kan du sedan använda Utforskaren toomodify dessa behörigheter.
3. När du har konfigurerat din resurs klickar du på **skapa**. En resurs med hello som angetts kommer att skapas inställningar och du ser ett meddelande. Som standard ska säkerhetskopiering aktiveras för hello resursen.
4. tooconfirm som hello resursen har har skapats, gå toohello **resurser** bladet. Du bör se hello dela listan.
   
    ![Dela skapa lyckades](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Ändra en resurs

Ändra en resurs när du behöver toochange hello beskrivning av hello resursen. Inga andra resursegenskaper kan inte ändras när hello resursen har skapats.

#### <a name="toomodify-a-share"></a>toomodify en resurs

1. Från hello **resurser** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matrisen på vilka hello resursen som du vill toomodify finns.
2. **Välj** hello resursen tooview hello aktuella beskrivning och ändra den.
3. Spara dina ändringar genom att klicka på hello **spara** kommandofältet. Inställningarna för angivna tillämpas och ett meddelande visas.
   
    ![ Redigera resurs](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Koppla från en resurs

Du kanske måste tootake en resurs offline när du planerar toomodify den eller ta bort den. När en resurs är offline, är den inte tillgänglig för skrivskyddad åtkomst. Du behöver tootake hello resursen offline på värden för hello samt på hello enhet.

#### <a name="tootake-a-share-offline"></a>tootake en resurs som är offline

1. Kontrollera att den aktuella hello resursen inte används innan den tas offline.
2. Ta hello resursen på hello matrisen offline genom att utföra hello följande steg:
   
    1. Från hello **resurser** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matrisen på vilka hello resursen som du vill tootake offline finns.

    2. **Välj** hello resursen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta offline**.
     
        ![Frånkopplad resurs](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Studera hello hello **ta offline** bladet och bekräfta ditt godkännande av hello igen. Klicka på **ta offline** tootake hello resursen offline. Visas ett meddelande om hello-åtgärd pågår.

    4. tooconfirm som hello resursen skapades har offline, gå toohello **resurser** bladet. Du bör se hello statusen som offline hello-resursen.

## <a name="delete-a-share"></a>Ta bort en resurs

> [!IMPORTANT]
> Du kan ta bort en resurs bara om den är offline.


Slutför följande steg toodelete en resurs hello.

#### <a name="toodelete-a-share"></a>toodelete en resurs

1. Från hello **resurser** inställningen på hello StorSimple-tjänsten sammanfattning bladet välj hello virtuella matris som du vill toodelete finns på vilken hello-resursen.
2. **Välj** hello resursen och klickar på **...**  (alternativt Högerklicka på den här raden) och hello snabbmenyn, Välj **ta bort**.
   
    ![Ta bort resursen](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Kontrollera status för hello hello resursen som du vill toodelete. Om hello resursen som du vill toodelete inte är offline, koppla från den först. Gör så hello i [koppla en resurs från](#take-a-share-offline).
4. När du uppmanas att bekräfta i hello **ta bort** bladet acceptera hello bekräftelse och klicka på **ta bort**. hello resursen tas bort och hello **resurser** bladet visar hello uppdatera listan över resurser inom hello virtuella matris.

## <a name="next-steps"></a>Nästa steg
Lär dig hur för[klona en StorSimple-resurs](storsimple-virtual-array-clone.md).

