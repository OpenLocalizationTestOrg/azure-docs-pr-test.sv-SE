---
title: aaaManage access control-poster i StorSimple | Microsoft Docs
description: "Beskriver hur toouse åtkomstkontroll poster (ACRs) toodetermine vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: a1e718c2679301b34221a233557a1eaae869a94f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-access-control-records"></a>Använd hello StorSimple Manager service toomanage access control-poster
## <a name="overview"></a>Översikt
Access control-poster (ACRs) kan du toospecify vilka värdar som kan ansluta tooa volymen på hello StorSimple-enhet. ACRs anges tooa specifika volymen och innehålla hello iSCSI kvalificerade namn (kvalificerade) hello-värdar. När en värd försöker tooconnect tooa volym, enhetskontroller hello hello ACR som är kopplad till volymen för hello IQN namn och om det finns en matchning och sedan hello anslutningen har upprättats. hello åtkomstkontroll innehåller avsnitt om hello **konfigurera** sidan visas alla hello access control-poster med hello motsvarande kvalificerade hello värdar.

Den här kursen förklaras hello följande vanliga ACR-relaterade uppgifter:

* Lägg till en åtkomstkontrollpost 
* Redigera en åtkomstkontrollpost 
* Ta bort en åtkomstkontrollpost 

> [!IMPORTANT]
> * När du tilldelar en ACR tooa volym Se till att hello inte är samtidigt komma åt volymen av mer än en icke-klustrad värd eftersom detta kan skada hello volym. 
> * När du tar bort en ACR från en volym, kontrollera hello motsvarande värden inte kommer åt hello volymen eftersom hello borttagning kan resultera i en skrivskyddad avbrott.
> 
> 

## <a name="add-an-access-control-record"></a>Lägg till en åtkomstkontrollpost
Du använder hello StorSimple Manager-tjänsten **konfigurera** sidan tooadd ACRs. Normalt kopplar du en ACR en volym.

Utför följande steg tooadd en ACR hello.

#### <a name="tooadd-an-access-control-record"></a>tooadd en åtkomstkontrollpost
1. På hello Landningssida för tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på hello **konfigurera** fliken.
2. I tabellform lista hello **Access control-poster**, varor en **namn** för din ACR.
3. Ange hello IQN namn för din Windows-värd under **iSCSI-Initierarnamn**. tooget hello IQN för Windows Server-värd, hello följande:
   
   * Starta hello Microsoft iSCSI-initieraren på din Windows-värd.
   * I hello **iSCSI-Initieraregenskaper** fönstret på hello **Configuration** , markera och kopiera hello sträng från hello **Initierarnamn** fältet.
   * Klistra in den här strängen i hello **iSCSI-Initierarnamn** på hello ACRs tabellen i hello klassiska Azure-portalen.
4. Klicka på **spara** toosave hello nyskapad ACR. hello tabular lista kommer vara uppdaterade tooreflect det här tillägget.

## <a name="edit-an-access-control-record"></a>Redigera en åtkomstkontrollpost
Du använder hello **konfigurera** sida i hello Azure klassiska portal tooedit ACRs. 

> [!NOTE]
> Du kan ändra de ACRs som för närvarande inte används. tooedit en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.
> 
> 

Utför följande steg tooedit en ACR hello.

#### <a name="tooedit-an-access-control-record"></a>tooedit en åtkomstkontrollpost
1. På hello Landningssida för tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på hello **konfigurera** fliken.
2. Hovra över hello ACR toomodify gärna i hello tabular lista av hello access control-poster.
3. Ange ett nytt namn och/eller IQN för hello ACR.
4. Klicka på **spara** toosave hello ändrade ACR. hello tabular lista kommer vara uppdaterade tooreflect ändringen.

## <a name="delete-an-access-control-record"></a>Ta bort en åtkomstkontrollpost
Du använder hello **konfigurera** sida i hello Azure klassiska portal toodelete ACRs. 

> [!NOTE]
> Du kan ta bort dessa ACRs som för närvarande inte används. toodelete en ACR som är kopplade till en volym som används, måste du först göra hello volymen offline.
> 
> 

Utför följande steg toodelete en åtkomstkontrollpost hello.

#### <a name="toodelete-an-access-control-record"></a>toodelete en åtkomstkontrollpost
1. På hello Landningssida för tjänsten, väljer du din tjänst, dubbelklicka på hello tjänstens namn och klicka sedan på hello **konfigurera** fliken.
2. Hovra över hello ACR toodelete gärna i hello tabular lista av hello access control-poster (ACRs).
3. En borttagningsikonen (**x**) visas i hello extrema högra kolumnen för hello ACR som du väljer. Klicka på hello **x** ikonen toodelete hello ACR.
4. När du uppmanas att bekräfta, klickar du på **Ja** toocontinue med hello borttagning. hello tabular lista kommer att vara uppdaterade tooreflect hello borttagning.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [hantera StorSimple-volymer](storsimple-manage-volumes.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

