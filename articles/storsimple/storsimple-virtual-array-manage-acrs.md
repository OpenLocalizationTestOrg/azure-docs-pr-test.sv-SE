---
title: "aaaManage access control-poster för virtuell StorSimple-matrisen | Microsoft Docs"
description: "Beskriver hur toomanage åtkomstkontroll poster (ACRs) toodetermine vilka värdar som kan ansluta tooa volym på hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e154bb4f-faab-4d92-a593-900c3ddc9595
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 608fdf72413761ce3c9c4bf297a748489c415685
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-access-control-records-for-storsimple-virtual-array"></a>Använd StorSimple Enhetshanteraren toomanage kontroll åtkomstregister för virtuell StorSimple-matris

## <a name="overview"></a>Översikt

Access control-poster (ACRs) kan du toospecify vilka värdar som kan ansluta tooa volym på hello virtuella StorSimple-matris (även kallat hello lokala virtuella enheten StorSimple). ACRs anges tooa specifika volymen och innehålla hello iSCSI kvalificerade namn (kvalificerade) hello-värdar. När en värd försöker tooconnect tooa volym, hello enhetskontroller hello ACR som är kopplad till volymen för hello IQN namn och om det finns en matchning, upprättas hello-anslutning. Hej **Access control-poster** bladet inom hello **Configuration** avsnitt i Device Manager-tjänsten visar alla hello access control-poster med hello motsvarande kvalificerade hello värdar.

![Hantera access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-manage-acrs.png)

Den här kursen förklaras hello följande vanliga ACR-relaterade uppgifter:

* Hämta hello IQN
* Lägg till en åtkomstkontrollpost
* Redigera en åtkomstkontrollpost
* Ta bort en åtkomstkontrollpost

> [!IMPORTANT]
> 
> * När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.
> * När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.


## <a name="get-hello-iqn"></a>Hämta hello IQN

Utför följande steg tooget hello IQN för en Windows-värd som kör Windows Server 2012 hello.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Lägg till en ACR

Du använder **Access control-poster** bladet inom hello **Configuration** avsnitt i din StorSimple Enhetshanteraren service tooadd ACRs. Normalt kan du associera en ACR med en volym.

Information om hur du kopplar en ACR med en volym finns för[lägga till en volym](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym.


Utför följande steg tooadd en ACR hello.

#### <a name="tooadd-an-acr"></a>tooadd en ACR

1. Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** klickar du på **Access control-poster**.
2. I hello **Access control-poster** bladet, klickar du på **Lägg till**.
3. I hello **lägga till ACR** bladet hello följande:
   
    1. Ange ett **namn** för din ACR.
    
    2. Under **iSCSI-Initierarnamn**, ange hello IQN namnet på din Windows-värd. tooget hello IQN för Windows Server-värd, hello följande:
   
    3. Starta hello Microsoft iSCSI-initieraren på din Windows-värd. I hello iSCSI-initieraren fönstret Egenskaper på hello **Configuration** , markera och kopiera hello sträng från hello **Initierarnamn** fältet.
    Klistra in den här strängen i hello **IQN** i hello **lägga till ACR** bladet.
   
    6. Klicka på **Lägg till** tooadd hello ACR.  
   
        ![Lägg till access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-add-acrs.png)
4. Hej tabular lista är uppdaterade tooreflect det här tillägget.

## <a name="edit-an-acr"></a>Redigera en ACR

Du använder hello **Access control-poster** bladet inom hello **Configuration** avsnitt i Enhetshanteraren tjänsten i hello Azure portal tooedit ACRs.

> [!NOTE]
> Du bör inte ändra en ACR som används för närvarande. tooedit en ACR som är kopplade till en volym som används för närvarande, bör du först se hello volymen offline.


Utför följande steg tooedit en ACR hello.

#### <a name="tooedit-an-acr"></a>tooedit en ACR

1. Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** avsnittet **Access control-poster**.
2. I hello **Access control-poster** bladet från hello tabular lista över hello access control-poster, dubbelklicka på hello ACR toomodify gärna.
3. I hello **access control poster** bladet hello följande:
   
    1. Ange hello IQN för hello ACR.
   
    2. Klicka på **spara** hello överst i hello bladet toosave hello ändrade ACR. Du ser hello följande bekräftelsemeddelande:
   
        ![Redigera access control-poster](./media/storsimple-virtual-array-manage-acrs/ova-edit-acrs.png)

## <a name="delete-an-access-control-record"></a>Ta bort en åtkomstkontrollpost

Du använder hello **Configuration** sida i hello Azure portal toodelete ACRs.

> [!NOTE]
> 
> * Du bör inte ta bort en ACR som används för närvarande. toodelete en ACR som är kopplade till en volym som används för närvarande, bör du först se hello volymen offline.
> * När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.


Utför följande steg toodelete en åtkomstkontrollpost hello.

#### <a name="toodelete-an-access-control-record"></a>toodelete en åtkomstkontrollpost

1. Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Configuration** avsnittet **Access control-poster**.

2. I hello **Access control-poster** bladet från hello tabular lista över hello access control-poster, dubbelklicka på hello ACR toodelete gärna.

3. I hello redigera access control poster-bladet klickar du på **ta bort**.
   
    ![Ta bort ACRS](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs.png)

4. När du uppmanas att bekräfta, klickar du på **ta bort** toocontinue med hello borttagning. hello tabular lista är uppdaterade tooreflect hello borttagning.
   
   ![Varningsmeddelande](./media/storsimple-virtual-array-manage-acrs/ova-del-acrs-warning.png)

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [lägga till volymer och konfigurera ACRs](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume).

