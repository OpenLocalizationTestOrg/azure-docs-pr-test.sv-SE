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
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-volume-containers"></a><span data-ttu-id="463d6-103">Använd hello StorSimple Enhetshanteraren service toomanage StorSimple-volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-103">Use hello StorSimple Device Manager service toomanage StorSimple volume containers</span></span>

## <a name="overview"></a><span data-ttu-id="463d6-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="463d6-104">Overview</span></span>
<span data-ttu-id="463d6-105">Den här självstudiekursen beskrivs hur toouse hello StorSimple Enhetshanteraren service toocreate och hantera behållare för StorSimple-volym.</span><span class="sxs-lookup"><span data-stu-id="463d6-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage StorSimple volume containers.</span></span>

<span data-ttu-id="463d6-106">En volymbehållare i en Microsoft Azure StorSimple-enhet innehåller en eller flera volymer som har lagringskonto, kryptering och förbrukning av bandbreddsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="463d6-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="463d6-107">En enhet kan ha flera volymbehållare för alla volymer.</span><span class="sxs-lookup"><span data-stu-id="463d6-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="463d6-108">En volymbehållare har hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="463d6-108">A volume container has hello following attributes:</span></span>

* <span data-ttu-id="463d6-109">**Volymer** – hello nivåer eller lokalt Fäst StorSimple-volymer som ingår i hello volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="463d6-109">**Volumes** – hello tiered or locally pinned StorSimple volumes that are contained within hello volume container.</span></span> 
* <span data-ttu-id="463d6-110">**Kryptering** – en krypteringsnyckel som kan definieras för varje volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="463d6-110">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="463d6-111">Den här nyckeln används för att kryptera hello data som skickas från din StorSimple-enhet toohello moln.</span><span class="sxs-lookup"><span data-stu-id="463d6-111">This key is used for encrypting hello data that is sent from your StorSimple device toohello cloud.</span></span> <span data-ttu-id="463d6-112">En militära klass AES 256 bitarsnyckel används med hello användarens nyckel.</span><span class="sxs-lookup"><span data-stu-id="463d6-112">A military-grade AES-256 bit key is used with hello user-entered key.</span></span> <span data-ttu-id="463d6-113">toosecure dina data rekommenderar vi att du alltid aktiverar lagringskryptering i molnet.</span><span class="sxs-lookup"><span data-stu-id="463d6-113">toosecure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="463d6-114">**Storage-konto** – hello Azure storage-konto som används toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="463d6-114">**Storage account** – hello Azure storage account that is used toostore hello data.</span></span> <span data-ttu-id="463d6-115">Alla hello volymer som finns i en volymbehållare dela det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="463d6-115">All hello volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="463d6-116">Du kan välja ett lagringskonto från en befintlig lista eller skapa ett nytt konto när du skapar hello volymbehållare och sedan ange hello autentiseringsuppgifter för det kontot.</span><span class="sxs-lookup"><span data-stu-id="463d6-116">You can choose a storage account from an existing list, or create a new account when you create hello volume container and then specify hello access credentials for that account.</span></span>
* <span data-ttu-id="463d6-117">**Molnet bandbredd** – hello bandbredd som används av hello enhet när hello data från hello enhet skickas toohello moln.</span><span class="sxs-lookup"><span data-stu-id="463d6-117">**Cloud bandwidth** – hello bandwidth consumed by hello device when hello data from hello device is being sent toohello cloud.</span></span> <span data-ttu-id="463d6-118">Du kan tillämpa en bandbreddskontroll genom att ange ett värde mellan 1 och 1 000 Mbps när du skapar den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="463d6-118">You can enforce a bandwidth control by specifying a value between 1 Mbps and 1,000 Mbps when you create this container.</span></span> <span data-ttu-id="463d6-119">Om du vill hello enheten tooconsume all tillgänglig bandbredd, anger du det här fältet för**obegränsad**.</span><span class="sxs-lookup"><span data-stu-id="463d6-119">If you want hello device tooconsume all available bandwidth, set this field too**Unlimited**.</span></span> <span data-ttu-id="463d6-120">Du kan också skapa och använda en bandbredd mallen tooallocate bandbredd baserat på schemat.</span><span class="sxs-lookup"><span data-stu-id="463d6-120">You can also create and apply a bandwidth template tooallocate bandwidth based on schedule.</span></span>

<span data-ttu-id="463d6-121">hello följande procedurer beskriver hur toouse hello StorSimple **volymbehållare** bladet toocomplete hello följande vanliga åtgärder:</span><span class="sxs-lookup"><span data-stu-id="463d6-121">hello following procedures explain how toouse hello StorSimple **Volume containers** blade toocomplete hello following common operations:</span></span>

* <span data-ttu-id="463d6-122">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-122">Add a volume container</span></span>
* <span data-ttu-id="463d6-123">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-123">Modify a volume container</span></span>
* <span data-ttu-id="463d6-124">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-124">Delete a volume container</span></span>

## <a name="add-a-volume-container"></a><span data-ttu-id="463d6-125">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-125">Add a volume container</span></span>
<span data-ttu-id="463d6-126">Utför följande steg tooadd en volymbehållare hello.</span><span class="sxs-lookup"><span data-stu-id="463d6-126">Perform hello following steps tooadd a volume container.</span></span>

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="463d6-127">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-127">Modify a volume container</span></span>
<span data-ttu-id="463d6-128">Utför följande steg toomodify en volymbehållare hello.</span><span class="sxs-lookup"><span data-stu-id="463d6-128">Perform hello following steps toomodify a volume container.</span></span>

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="463d6-129">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="463d6-129">Delete a volume container</span></span>
<span data-ttu-id="463d6-130">En volymbehållare har volymer i den.</span><span class="sxs-lookup"><span data-stu-id="463d6-130">A volume container has volumes within it.</span></span> <span data-ttu-id="463d6-131">Det kan endast tas bort om alla hello volymer finns i den först tas bort.</span><span class="sxs-lookup"><span data-stu-id="463d6-131">It can be deleted only if all hello volumes contained in it are first deleted.</span></span> <span data-ttu-id="463d6-132">Utför följande steg toodelete en volymbehållare hello.</span><span class="sxs-lookup"><span data-stu-id="463d6-132">Perform hello following steps toodelete a volume container.</span></span>

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="463d6-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="463d6-133">Next steps</span></span>
* <span data-ttu-id="463d6-134">Lär dig mer om [hantera StorSimple-volymer](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="463d6-134">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span> 
* <span data-ttu-id="463d6-135">Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="463d6-135">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

