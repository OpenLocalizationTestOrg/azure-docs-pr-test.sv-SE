---
title: "aaaManage din StorSimple-volymbehållare | Microsoft Docs"
description: "Beskriver hur du kan använda hello StorSimple Manager-volym behållaren sidan tooadd, ändra eller ta bort en volymbehållare."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 9b29536e0072306e53ac92bacca78a13d932c2b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-volume-containers"></a>Använd hello StorSimple Manager-tjänsten toomanage StorSimple-volymbehållare
## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur toouse hello toocreate för StorSimple Manager-tjänsten och hantera behållare för StorSimple-volym.

En volymbehållare i en Microsoft Azure StorSimple-enhet innehåller en eller flera volymer som har lagringskonto, kryptering och förbrukning av bandbreddsinställningarna. En enhet kan ha flera volymbehållare för alla volymer. 

En volymbehållare har hello följande attribut:

* **Volymer** – hello nivåer eller lokalt Fäst StorSimple-volymer som ingår i hello volymbehållare. En volymbehållare kan innehålla in too256 StorSimple-volymer.
* **Kryptering** – en krypteringsnyckel som kan definieras för varje volymbehållare. Den här nyckeln används för att kryptera hello data som skickas från din StorSimple-enhet toohello moln. En militära klass AES 256 bitarsnyckel används med hello användarens nyckel. toosecure dina data rekommenderar vi att du alltid aktiverar lagringskryptering i molnet.
* **Storage-konto** – hello storage-konto som är länkade tooyour molntjänstleverantören för lagring. Alla hello volymer som finns i en volymbehållare dela det här lagringskontot. Du kan välja ett lagringskonto från en befintlig lista eller skapa ett nytt konto när du skapar hello volymbehållare och sedan ange hello autentiseringsuppgifter för det kontot.
* **Molnet bandbredd** – hello bandbredd som används av hello enhet när hello data från hello enhet skickas toohello moln. Du kan tillämpa en bandbreddskontroll genom att ange ett värde mellan 1 och 1000 Mbps när du definierar den här behållaren. Om du vill hello enheten tooconsume all tillgänglig bandbredd, anger du det här fältet tooUnlimited. Du kan också skapa och använda en bandbredd mallen tooallocate bandbredd baserat på schemat.

![Volymen behållare sida](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Den här nedan beskrivs hur toouse hello StorSimple **volymbehållare** sidan toocomplete hello följande vanliga åtgärder:

* Lägg till volymbehållare 
* Ändra en volymbehållare 
* Ta bort en volymbehållare 

## <a name="add-a-volume-container"></a>Lägg till volymbehållare
Utför följande steg tooadd en volymbehållare hello.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Ändra en volymbehållare
Utför följande steg toomodify en volymbehållare hello.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Ta bort en volymbehållare
En volymbehållare har volymer i den. Det kan endast tas bort om alla hello volymer finns i den först tas bort. Utför följande steg toodelete en volymbehållare hello.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [hantera StorSimple-volymer](storsimple-manage-volumes.md). 
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

