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
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-volume-containers"></a><span data-ttu-id="0133a-103">Använd hello StorSimple Manager-tjänsten toomanage StorSimple-volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-103">Use hello StorSimple Manager service toomanage StorSimple volume containers</span></span>
## <a name="overview"></a><span data-ttu-id="0133a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0133a-104">Overview</span></span>
<span data-ttu-id="0133a-105">Den här självstudiekursen beskrivs hur toouse hello toocreate för StorSimple Manager-tjänsten och hantera behållare för StorSimple-volym.</span><span class="sxs-lookup"><span data-stu-id="0133a-105">This tutorial explains how toouse hello StorSimple Manager service toocreate and manage StorSimple volume containers.</span></span>

<span data-ttu-id="0133a-106">En volymbehållare i en Microsoft Azure StorSimple-enhet innehåller en eller flera volymer som har lagringskonto, kryptering och förbrukning av bandbreddsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="0133a-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="0133a-107">En enhet kan ha flera volymbehållare för alla volymer.</span><span class="sxs-lookup"><span data-stu-id="0133a-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="0133a-108">En volymbehållare har hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="0133a-108">A volume container has hello following attributes:</span></span>

* <span data-ttu-id="0133a-109">**Volymer** – hello nivåer eller lokalt Fäst StorSimple-volymer som ingår i hello volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="0133a-109">**Volumes** – hello tiered or locally pinned StorSimple volumes that are contained within hello volume container.</span></span> <span data-ttu-id="0133a-110">En volymbehållare kan innehålla in too256 StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="0133a-110">A volume container may contain up too256 StorSimple volumes.</span></span>
* <span data-ttu-id="0133a-111">**Kryptering** – en krypteringsnyckel som kan definieras för varje volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="0133a-111">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="0133a-112">Den här nyckeln används för att kryptera hello data som skickas från din StorSimple-enhet toohello moln.</span><span class="sxs-lookup"><span data-stu-id="0133a-112">This key is used for encrypting hello data that is sent from your StorSimple device toohello cloud.</span></span> <span data-ttu-id="0133a-113">En militära klass AES 256 bitarsnyckel används med hello användarens nyckel.</span><span class="sxs-lookup"><span data-stu-id="0133a-113">A military-grade AES-256 bit key is used with hello user-entered key.</span></span> <span data-ttu-id="0133a-114">toosecure dina data rekommenderar vi att du alltid aktiverar lagringskryptering i molnet.</span><span class="sxs-lookup"><span data-stu-id="0133a-114">toosecure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="0133a-115">**Storage-konto** – hello storage-konto som är länkade tooyour molntjänstleverantören för lagring.</span><span class="sxs-lookup"><span data-stu-id="0133a-115">**Storage account** – hello storage account that is linked tooyour cloud storage service provider.</span></span> <span data-ttu-id="0133a-116">Alla hello volymer som finns i en volymbehållare dela det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="0133a-116">All hello volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="0133a-117">Du kan välja ett lagringskonto från en befintlig lista eller skapa ett nytt konto när du skapar hello volymbehållare och sedan ange hello autentiseringsuppgifter för det kontot.</span><span class="sxs-lookup"><span data-stu-id="0133a-117">You can choose a storage account from an existing list, or create a new account when you create hello volume container and then specify hello access credentials for that account.</span></span>
* <span data-ttu-id="0133a-118">**Molnet bandbredd** – hello bandbredd som används av hello enhet när hello data från hello enhet skickas toohello moln.</span><span class="sxs-lookup"><span data-stu-id="0133a-118">**Cloud bandwidth** – hello bandwidth consumed by hello device when hello data from hello device is being sent toohello cloud.</span></span> <span data-ttu-id="0133a-119">Du kan tillämpa en bandbreddskontroll genom att ange ett värde mellan 1 och 1000 Mbps när du definierar den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="0133a-119">You can enforce a bandwidth control by specifying a value between 1 and 1000 Mbps when you define this container.</span></span> <span data-ttu-id="0133a-120">Om du vill hello enheten tooconsume all tillgänglig bandbredd, anger du det här fältet tooUnlimited.</span><span class="sxs-lookup"><span data-stu-id="0133a-120">If you want hello device tooconsume all available bandwidth, set this field tooUnlimited.</span></span> <span data-ttu-id="0133a-121">Du kan också skapa och använda en bandbredd mallen tooallocate bandbredd baserat på schemat.</span><span class="sxs-lookup"><span data-stu-id="0133a-121">You can also create and apply a bandwidth template tooallocate bandwidth based on schedule.</span></span>

![Volymen behållare sida](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

<span data-ttu-id="0133a-123">Den här nedan beskrivs hur toouse hello StorSimple **volymbehållare** sidan toocomplete hello följande vanliga åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0133a-123">This following procedures explain how toouse hello StorSimple **Volume containers** page toocomplete hello following common operations:</span></span>

* <span data-ttu-id="0133a-124">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-124">Add a volume container</span></span> 
* <span data-ttu-id="0133a-125">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-125">Modify a volume container</span></span> 
* <span data-ttu-id="0133a-126">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-126">Delete a volume container</span></span> 

## <a name="add-a-volume-container"></a><span data-ttu-id="0133a-127">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-127">Add a volume container</span></span>
<span data-ttu-id="0133a-128">Utför följande steg tooadd en volymbehållare hello.</span><span class="sxs-lookup"><span data-stu-id="0133a-128">Perform hello following steps tooadd a volume container.</span></span>

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="0133a-129">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-129">Modify a volume container</span></span>
<span data-ttu-id="0133a-130">Utför följande steg toomodify en volymbehållare hello.</span><span class="sxs-lookup"><span data-stu-id="0133a-130">Perform hello following steps toomodify a volume container.</span></span>

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="0133a-131">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="0133a-131">Delete a volume container</span></span>
<span data-ttu-id="0133a-132">En volymbehållare har volymer i den.</span><span class="sxs-lookup"><span data-stu-id="0133a-132">A volume container has volumes within it.</span></span> <span data-ttu-id="0133a-133">Det kan endast tas bort om alla hello volymer finns i den först tas bort.</span><span class="sxs-lookup"><span data-stu-id="0133a-133">It can be deleted only if all hello volumes contained in it are first deleted.</span></span> <span data-ttu-id="0133a-134">Utför följande steg toodelete en volymbehållare hello.</span><span class="sxs-lookup"><span data-stu-id="0133a-134">Perform hello following steps toodelete a volume container.</span></span>

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="0133a-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0133a-135">Next steps</span></span>
* <span data-ttu-id="0133a-136">Lär dig mer om [hantera StorSimple-volymer](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="0133a-136">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span> 
* <span data-ttu-id="0133a-137">Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="0133a-137">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

