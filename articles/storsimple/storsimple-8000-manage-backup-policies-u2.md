---
title: "aaaManage StorSimple 8000-serien säkerhetskopieringsprinciper | Microsoft Docs"
description: "Beskriver hur du kan använda hello StorSimple Enhetshanteraren service toocreate och hantera manuella säkerhetskopieringar, scheman för säkerhetskopiering och lagring av säkerhetskopior på en enhet för StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 7c56365abb6ba69d02008829ca6ae703d4632705
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-toomanage-backup-policies"></a>Använd hello StorSimple enheten Manager-tjänsten i Azure portal toomanage principer för säkerhetskopiering


## <a name="overview"></a>Översikt

Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service **säkerhetskopiera princip** bladet toocontrol säkerhetskopiering processer och lagring av säkerhetskopior för din StorSimple-volymer. Här beskrivs också hur toocomplete en manuell säkerhetskopiering.

När du säkerhetskopierar en volym kan du välja toocreate lokal ögonblicksbild eller en ögonblicksbild i molnet. Om du säkerhetskopierar en lokalt Fäst volym rekommenderar vi att du anger en ögonblicksbild i molnet. Med ett stort antal lokala ögonblicksbilder av en lokalt Fäst volym tillsammans med en datauppsättning som har mycket omsättning resulterar i en situation där du snabbt kan köra out-of-lokalt utrymme. Om du väljer tootake lokala ögonblicksbilder, rekommenderar vi att du tar färre dagliga ögonblicksbilder tooback hello senaste tillstånd behålla dem i en dag och ta bort dem.

När du tar en ögonblicksbild i molnet för en lokalt Fäst volym måste kopiera du endast hello ändras data toohello moln, där det är deduplicerad och komprimeras.

## <a name="hello-backup-policy-blade"></a>Hej principbladet för säkerhetskopiering

Hej **säkerhetskopiera princip** bladet för din StorSimple-enhet kan du toomanage principer för säkerhetskopiering och schema för lokala och molnögonblicksbilder. Principer för säkerhetskopiering är används tooconfigure scheman för säkerhetskopiering och lagring av säkerhetskopior för en samling av volymer. Principer för säkerhetskopiering kan du tootake en ögonblicksbild av flera volymer samtidigt. Det innebär att hello säkerhetskopior som har skapats av en princip för säkerhetskopiering är kraschkonsekvent kopior.

hello säkerhetskopieringsprinciper tabular lista kan du toofilter hello befintliga principer för säkerhetskopiering av en eller flera av hello följande fält:

* **Principnamn** – hello namnet som associeras med hello princip. hello olika typer av principer är:

  * Schemalagda principer som skapas av hello användaren explicit.
  * Importera principer som ursprungligen skapades i hello StorSimple Snapshot Manager. Dessa har en tagg som beskriver hello StorSimple Snapshot Manager värden som hello principer har importerats från.

  > [!NOTE]
  > Automatisk eller standard principer för säkerhetskopiering är inte längre aktiverat när hello volymer skapas.

* **Senaste lyckade säkerhetskopiering** – hello datum och tid för hello senaste lyckad säkerhetskopia som togs med den här principen.

* **Nästa säkerhetskopiering** – hello datum och tid för hello nästa schemalagda säkerhetskopieringen som initieras av den här principen.

* **Volymer** – hello volymer som är associerade med hello principen. Alla hello volymer som är associerade med en princip för säkerhetskopiering är grupperade när säkerhetskopieringar skapas.

* **Scheman** – hello antalet scheman som är associerade med hello säkerhetskopieringsprincip.

hello vanliga åtgärder som du kan utföra för principer för säkerhetskopiering är:

* Lägga till en säkerhetskopieringspolicy
* Lägg till eller ändra ett schema
* Lägg till eller ta bort en volym
* Ta bort en princip för säkerhetskopiering
* Gör en manuell säkerhetskopia

## <a name="add-a-backup-policy"></a>Lägga till en säkerhetskopieringspolicy

Lägg till en princip för säkerhetskopiering tooautomatically schema säkerhetskopiorna. När du skapar en volym, det finns ingen standardprincip för säkerhetskopiering som är kopplad till volymen. Du behöver tooadd och tilldela en säkerhetskopieringsprincip tooprotect volymdata.

Utföra hello följa stegen i hello Azure portal tooadd en säkerhetskopieringsprincip för din StorSimple-enhet. När du har lagt till hello princip kan du definiera ett schema (se [Lägg till eller ändra ett schema](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a>Lägg till eller ändra ett schema

Du kan lägga till eller ändra ett schema som är bifogade tooan säkerhetskopieringsprincip på StorSimple-enheten. Utför följande steg i hello Azure portal tooadd hello eller ändra ett schema.

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a>Lägg till eller ta bort en volym

Du kan lägga till eller ta bort en volym som tilldelats tooa princip för säkerhetskopiering på StorSimple-enheten. Utför följande steg i hello Azure portal tooadd hello eller ta bort en volym.

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a>Ta bort en princip för säkerhetskopiering

Utföra hello följa stegen i hello Azure portal toodelete en princip för säkerhetskopiering på StorSimple-enheten.

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Gör en manuell säkerhetskopia

Utföra hello följa stegen i hello Azure portal toocreate en på-begäran (manuell) säkerhetskopiering för en enskild volym.

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

