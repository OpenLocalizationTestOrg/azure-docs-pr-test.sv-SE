---
title: aaaManage access control-poster i StorSimple | Microsoft Docs
description: "Beskriver hur toouse åtkomstkontroll poster (ACRs) toodetermine vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet."
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
ms.date: 05/31/2017
ms.author: alkohli
ms.openlocfilehash: cf532206e2c0bc49da853663ba34ae993ec2981d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a>Använd hello StorSimple Manager service toomanage access control-poster

## <a name="overview"></a>Översikt
Access control-poster (ACRs) kan du toospecify vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet. ACRs anges tooa specifika volymen och innehålla hello iSCSI kvalificerade namn (kvalificerade) hello-värdar. När en värd försöker tooconnect tooa volym, enhetskontroller hello hello ACR som är kopplad till volymen för hello IQN namn och om det finns en matchning och sedan hello anslutningen har upprättats. Hej access control-poster i hello **Configuration** avsnitt i din StorSimple Enhetshanteraren service bladet visas alla hello access control-poster med hello motsvarande kvalificerade hello värdar.

Den här kursen förklaras hello följande vanliga ACR-relaterade uppgifter:

* Lägg till en åtkomstkontrollpost
* Redigera en åtkomstkontrollpost
* Ta bort en åtkomstkontrollpost

> [!IMPORTANT]
> * När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.
> * När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.

## <a name="get-hello-iqn"></a>Hämta hello IQN

Utför följande steg tooget hello IQN för en Windows-värd som kör Windows Server 2012 hello.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]


## <a name="add-an-access-control-record"></a>Lägg till en åtkomstkontrollpost
Du använder hello **Configuration** hello StorSimple Enhetshanteraren service bladet tooadd ACRs avsnitt. Normalt kopplar du en ACR en volym.

Utför följande steg tooadd en ACR hello.

#### <a name="tooadd-an-acr"></a>tooadd en ACR

1. Gå tooyour StorSimple enheten Manager-tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.
2. I hello **Access control-poster** bladet, klickar du på **+ Lägg till ACR**.

    ![Klicka på Lägg till ACR](./media/storsimple-8000-manage-acrs/createacr1.png)

3. I hello **lägga till ACR** bladet hello följande steg:

    1. Ange ett namn för din ACR.
    
    2. Ange hello IQN namnet på din Windows Server-värden under **iSCSI Initiator namn (IQN)**.

    3. Klicka på **Lägg till** toocreate hello ACR.

        ![Klicka på Lägg till ACR](./media/storsimple-8000-manage-acrs/createacr2.png)

4.  hello tillagda nyligen ACR visas i hello tabular lista över ACRs.

    ![Klicka på Lägg till ACR](./media/storsimple-8000-manage-acrs/createacr5.png)


## <a name="edit-an-access-control-record"></a>Redigera en åtkomstkontrollpost
Du använder hello **Configuration** hello StorSimple Enhetshanteraren service bladet tooedit ACRs avsnitt.

> [!NOTE]
> Vi rekommenderar att du ändrar dessa ACRs som för närvarande inte används. tooedit en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.

Utför följande steg tooedit en ACR hello.

#### <a name="tooedit-an-access-control-record"></a>tooedit en åtkomstkontrollpost
1.  Gå tooyour StorSimple enheten Manager-tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Klicka i hello tabular lista över hello access control-poster, och välj hello ACR toomodify gärna.

    ![Redigera access control-poster](./media/storsimple-8000-manage-acrs/editacr1.png)

3. I hello **redigera åtkomstkontrollpost** bladet, ange en annan IQN motsvarande tooanother värd.

    ![Redigera access control-poster](./media/storsimple-8000-manage-acrs/editacr2.png)

4. Klicka på **Spara**. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. 

    ![Redigera access control-poster](./media/storsimple-8000-manage-acrs/editacr3.png)

5. Du meddelas när hello ACR uppdateras. hello tabular lista uppdateras också tooreflect hello ändringen.

   
## <a name="delete-an-access-control-record"></a>Ta bort en åtkomstkontrollpost
Du använder hello **Configuration** hello StorSimple Enhetshanteraren service bladet toodelete ACRs avsnitt.

> [!NOTE]
> Du kan ta bort dessa ACRs som för närvarande inte används. toodelete en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.

Utför följande steg toodelete en åtkomstkontrollpost hello.

#### <a name="toodelete-an-access-control-record"></a>toodelete en åtkomstkontrollpost
1.  Gå tooyour StorSimple enheten Manager-tjänsten genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/createacr1.png)

2. Klicka i hello tabular lista över hello access control-poster, och välj hello ACR toodelete gärna.

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr1.png)

3. Högerklicka på tooinvoke hello snabbmenyn och välj **ta bort**.

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr2.png)

4. När du uppmanas att bekräfta, granska hello information och klickar sedan på **ta bort**.

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr3.png)

5. Du meddelas när hello borttagningen har slutförts. hello tabular lista är uppdaterade tooreflect hello borttagning.

    ![Gå tooaccess styra poster](./media/storsimple-8000-manage-acrs/deleteacr5.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [hantera StorSimple-volymer](storsimple-8000-manage-volumes-u2.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

