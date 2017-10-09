---
title: "aaaManage din StorSimple-säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda hello StorSimple Manager-tjänsten toocreate och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior.."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d1834bc8-d520-4463-82ae-3b32fabd021c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 710cbe54d14031b4de43e9da292ed169085d5af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies"></a>Använd hello StorSimple Manager-tjänsten toomanage principer för säkerhetskopiering
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur toouse hello StorSimple Manager-tjänsten **Säkerhetskopieringsprinciper** toocontrol processer för säkerhetskopiering och lagring av säkerhetskopior för din StorSimple-volymer. Här beskrivs också hur toocomplete en manuell säkerhetskopiering.

Hej **Säkerhetskopieringsprinciper** sidan kan du toomanage principer för säkerhetskopiering och schema för lokala och molnögonblicksbilder. (Principer för säkerhetskopiering är används tooconfigure scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer). Principer för säkerhetskopiering kan du tootake en ögonblicksbild av flera volymer samtidigt. Det innebär att hello säkerhetskopior som har skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior. Den här sidan visar hello principer för säkerhetskopiering, typer, hello associerade volymer, hello antal säkerhetskopior bevaras och hello alternativet tooenable dessa principer.

Hej **Säkerhetskopieringsprinciper** sidan kan du också toofilter hello befintliga principer för säkerhetskopiering av en eller flera av hello följande fält:

* **Principnamn** – hello namnet som associeras med hello princip. hello olika typer av principer är:
  
  * Schemalagda principer som skapas av hello användaren explicit.
  * Automatisk principer som skapas när hello standard säkerhetskopiering för det här alternativet om volymen har aktiverat när hello volymer skapas. Dessa principer är namngivna som VolumeName_Default där volymnamn refererar toohello namnet på hello StorSimple-volym som konfigurerats av hello användare i hello klassiska Azure-portalen. hello automatisk principer resultera i dagliga molnögonblicksbilder början på 22:30 tid.
  * Importera principer som ursprungligen skapades i hello StorSimple Snapshot Manager. Dessa har en tagg som beskriver hello StorSimple Snapshot Manager värden som hello principer har importerats från.
* **Volymer** – hello volymer som är associerade med hello principen. Alla hello volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.
* **Senaste lyckade säkerhetskopiering** – hello datum och tid för hello senaste lyckad säkerhetskopia som togs med den här principen.
* **Nästa säkerhetskopiering** – hello datum och tid för hello nästa schemalagda säkerhetskopieringen som initieras av den här principen.
* **Scheman** – hello antalet scheman som är associerade med hello säkerhetskopieringsprincip.

hello vanliga åtgärder som du kan utföra den här sidan är:

* Lägga till en säkerhetskopieringspolicy 
* Lägg till eller ändra ett schema 
* Ta bort en princip för säkerhetskopiering 
* Gör en manuell säkerhetskopia 
* Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman 

## <a name="add-a-backup-policy"></a>Lägga till en säkerhetskopieringspolicy
Lägg till en princip för säkerhetskopiering tooautomatically schema säkerhetskopiorna. Utföra hello följa stegen i hello Azure klassiska portal tooadd en säkerhetskopieringsprincip för din StorSimple-enhet. När du har lagt till hello princip kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Video tillgänglig](./media/storsimple-manage-backup-policies/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur toocreate en lokal eller molnet säkerhetskopiera princip, klickar du på [här](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Lägg till eller ändra ett schema
Du kan lägga till eller ändra ett schema som är bifogade tooan säkerhetskopieringsprincip på StorSimple-enheten. Utför följande steg i hello Azure klassiska portal tooadd hello eller ändra ett schema.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Ta bort en princip för säkerhetskopiering
Utföra hello följa stegen i hello Azure klassiska portal toodelete en princip för säkerhetskopiering på StorSimple-enheten.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Gör en manuell säkerhetskopia
Utföra hello följa stegen i hello Azure klassiska portal toocreate en på-begäran (manuell) säkerhetskopiering för en enskild volym.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Skapa en anpassad princip för säkerhetskopiering med flera volymer och scheman
Utföra hello följa stegen i hello Azure klassiska portal toocreate en anpassad princip för säkerhetskopiering som har flera volymer och scheman.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

