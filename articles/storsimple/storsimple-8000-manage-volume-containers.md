---
title: "aaaManage din StorSimple-volymbehållare på hello StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur du kan använda hello Enhetshanteraren StorSimple-volym behållaren sidan tooadd, ändra eller ta bort en volymbehållare."
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
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 7374d4ab9aecd6280ae1d93a29f17d12d28c9362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-volume-containers"></a>Använd hello StorSimple Enhetshanteraren service toomanage StorSimple-volymbehållare

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera behållare för StorSimple-volym.

En volymbehållare i en Microsoft Azure StorSimple-enhet innehåller en eller flera volymer som har lagringskonto, kryptering och förbrukning av bandbreddsinställningarna. En enhet kan ha flera volymbehållare för alla volymer. 

En volymbehållare har hello följande attribut:

* **Volymer** – hello nivåer eller lokalt Fäst StorSimple-volymer som ingår i hello volymbehållare. 
* **Kryptering** – en krypteringsnyckel som kan definieras för varje volymbehållare. Den här nyckeln används för att kryptera hello data som skickas från din StorSimple-enhet toohello moln. En militära klass AES 256 bitarsnyckel används med hello användarens nyckel. toosecure dina data rekommenderar vi att du alltid aktiverar lagringskryptering i molnet.
* **Storage-konto** – hello Azure storage-konto som används toostore hello data. Alla hello volymer som finns i en volymbehållare dela det här lagringskontot. Du kan välja ett lagringskonto från en befintlig lista eller skapa ett nytt konto när du skapar hello volymbehållare och sedan ange hello autentiseringsuppgifter för det kontot.
* **Molnet bandbredd** – hello bandbredd som används av hello enhet när hello data från hello enhet skickas toohello moln. Du kan tillämpa en bandbreddskontroll genom att ange ett värde mellan 1 och 1 000 Mbps när du skapar den här behållaren. Om du vill hello enheten tooconsume all tillgänglig bandbredd, anger du det här fältet för**obegränsad**. Du kan också skapa och använda en bandbredd mallen tooallocate bandbredd baserat på schemat.

hello följande procedurer beskriver hur toouse hello StorSimple **volymbehållare** bladet toocomplete hello följande vanliga åtgärder:

* Lägg till volymbehållare
* Ändra en volymbehållare
* Ta bort en volymbehållare

## <a name="add-a-volume-container"></a>Lägg till volymbehållare
Utför följande steg tooadd en volymbehållare hello.

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a>Ändra en volymbehållare
Utför följande steg toomodify en volymbehållare hello.

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Ta bort en volymbehållare
En volymbehållare har volymer i den. Det kan endast tas bort om alla hello volymer finns i den först tas bort. Utför följande steg toodelete en volymbehållare hello.

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [hantera StorSimple-volymer](storsimple-8000-manage-volumes-u2.md). 
* Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

