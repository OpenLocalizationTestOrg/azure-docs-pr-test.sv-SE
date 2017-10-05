---
title: "Hantera din StorSimple-volymbehållare | Microsoft Docs"
description: "Beskriver hur du kan använda StorSimple Manager volym behållare sidan för att lägga till, ändra eller ta bort en volymbehållare."
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
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a><span data-ttu-id="1e68e-103">Använda StorSimple Manager-tjänsten för att hantera behållare för StorSimple-volym</span><span class="sxs-lookup"><span data-stu-id="1e68e-103">Use the StorSimple Manager service to manage StorSimple volume containers</span></span>
## <a name="overview"></a><span data-ttu-id="1e68e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1e68e-104">Overview</span></span>
<span data-ttu-id="1e68e-105">Den här självstudiekursen beskrivs hur du använda StorSimple Manager-tjänsten för att skapa och hantera behållare för StorSimple-volym.</span><span class="sxs-lookup"><span data-stu-id="1e68e-105">This tutorial explains how to use the StorSimple Manager service to create and manage StorSimple volume containers.</span></span>

<span data-ttu-id="1e68e-106">En volymbehållare i en Microsoft Azure StorSimple-enhet innehåller en eller flera volymer som har lagringskonto, kryptering och förbrukning av bandbreddsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="1e68e-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="1e68e-107">En enhet kan ha flera volymbehållare för alla volymer.</span><span class="sxs-lookup"><span data-stu-id="1e68e-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="1e68e-108">En volymbehållare har följande attribut:</span><span class="sxs-lookup"><span data-stu-id="1e68e-108">A volume container has the following attributes:</span></span>

* <span data-ttu-id="1e68e-109">**Volymer** – nivåer eller lokalt Fäst StorSimple-volymer som ingår i volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="1e68e-109">**Volumes** – The tiered or locally pinned StorSimple volumes that are contained within the volume container.</span></span> <span data-ttu-id="1e68e-110">En volymbehållare kan innehålla upp till 256 StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="1e68e-110">A volume container may contain up to 256 StorSimple volumes.</span></span>
* <span data-ttu-id="1e68e-111">**Kryptering** – en krypteringsnyckel som kan definieras för varje volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="1e68e-111">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="1e68e-112">Den här nyckeln används för att kryptera data som skickas från din StorSimple-enhet till molnet.</span><span class="sxs-lookup"><span data-stu-id="1e68e-112">This key is used for encrypting the data that is sent from your StorSimple device to the cloud.</span></span> <span data-ttu-id="1e68e-113">En militära klass AES 256 bitarsnyckel används med nyckeln anges av användaren.</span><span class="sxs-lookup"><span data-stu-id="1e68e-113">A military-grade AES-256 bit key is used with the user-entered key.</span></span> <span data-ttu-id="1e68e-114">Om du vill skydda dina data rekommenderar vi att du alltid aktiverar molnet lagringskryptering.</span><span class="sxs-lookup"><span data-stu-id="1e68e-114">To secure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="1e68e-115">**Storage-konto** – storage-konto som är länkad till din molntjänstleverantör för lagring.</span><span class="sxs-lookup"><span data-stu-id="1e68e-115">**Storage account** – The storage account that is linked to your cloud storage service provider.</span></span> <span data-ttu-id="1e68e-116">Alla volymer som finns i en volymbehållare dela det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="1e68e-116">All the volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="1e68e-117">Du kan välja ett lagringskonto från en befintlig lista eller skapa ett nytt konto när du skapar volymbehållaren och anger sedan autentiseringsuppgifterna för det kontot.</span><span class="sxs-lookup"><span data-stu-id="1e68e-117">You can choose a storage account from an existing list, or create a new account when you create the volume container and then specify the access credentials for that account.</span></span>
* <span data-ttu-id="1e68e-118">**Molnet bandbredd** – bandbredden som används av enheten när data från enheten skickas till molnet.</span><span class="sxs-lookup"><span data-stu-id="1e68e-118">**Cloud bandwidth** – The bandwidth consumed by the device when the data from the device is being sent to the cloud.</span></span> <span data-ttu-id="1e68e-119">Du kan tillämpa en bandbreddskontroll genom att ange ett värde mellan 1 och 1000 Mbps när du definierar den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="1e68e-119">You can enforce a bandwidth control by specifying a value between 1 and 1000 Mbps when you define this container.</span></span> <span data-ttu-id="1e68e-120">Om du vill att enheten använder all tillgänglig bandbredd, kan du ange det här fältet till obegränsad.</span><span class="sxs-lookup"><span data-stu-id="1e68e-120">If you want the device to consume all available bandwidth, set this field to Unlimited.</span></span> <span data-ttu-id="1e68e-121">Du kan också skapa och tillämpa en bandbreddsmall för att allokera bandbredd enligt schemat.</span><span class="sxs-lookup"><span data-stu-id="1e68e-121">You can also create and apply a bandwidth template to allocate bandwidth based on schedule.</span></span>

![Volymen behållare sida](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

<span data-ttu-id="1e68e-123">Den här följande procedurer beskriver hur du använder den virtuella StorSimple **volymbehållare** sidan för att slutföra följande vanliga åtgärder:</span><span class="sxs-lookup"><span data-stu-id="1e68e-123">This following procedures explain how to use the StorSimple **Volume containers** page to complete the following common operations:</span></span>

* <span data-ttu-id="1e68e-124">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="1e68e-124">Add a volume container</span></span> 
* <span data-ttu-id="1e68e-125">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="1e68e-125">Modify a volume container</span></span> 
* <span data-ttu-id="1e68e-126">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="1e68e-126">Delete a volume container</span></span> 

## <a name="add-a-volume-container"></a><span data-ttu-id="1e68e-127">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="1e68e-127">Add a volume container</span></span>
<span data-ttu-id="1e68e-128">Utför följande steg för att lägga till en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="1e68e-128">Perform the following steps to add a volume container.</span></span>

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="1e68e-129">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="1e68e-129">Modify a volume container</span></span>
<span data-ttu-id="1e68e-130">Utför följande steg om du vill ändra en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="1e68e-130">Perform the following steps to modify a volume container.</span></span>

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="1e68e-131">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="1e68e-131">Delete a volume container</span></span>
<span data-ttu-id="1e68e-132">En volymbehållare har volymer i den.</span><span class="sxs-lookup"><span data-stu-id="1e68e-132">A volume container has volumes within it.</span></span> <span data-ttu-id="1e68e-133">Det kan endast tas bort om de volymer som finns i den först tas bort.</span><span class="sxs-lookup"><span data-stu-id="1e68e-133">It can be deleted only if all the volumes contained in it are first deleted.</span></span> <span data-ttu-id="1e68e-134">Utför följande steg för att ta bort en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="1e68e-134">Perform the following steps to delete a volume container.</span></span>

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="1e68e-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e68e-135">Next steps</span></span>
* <span data-ttu-id="1e68e-136">Lär dig mer om [hantera StorSimple-volymer](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="1e68e-136">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span> 
* <span data-ttu-id="1e68e-137">Lär dig mer om [använda StorSimple Manager-tjänsten för att administrera din StorSimple-enhet](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="1e68e-137">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

