---
title: "Hantera din StorSimple-volymbehållare på StorSimple 8000-serien enheten | Microsoft Docs"
description: "Beskriver hur du kan använda StorSimple Enhetshanteraren volym behållare sidan för att lägga till, ändra eller ta bort en volymbehållare."
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
ms.openlocfilehash: 0f8e00d6d07224f56625482f339e612e68914be2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-volume-containers"></a><span data-ttu-id="8dc13-103">Använda Enhetshanteraren för StorSimple-tjänsten för att hantera behållare för StorSimple-volym</span><span class="sxs-lookup"><span data-stu-id="8dc13-103">Use the StorSimple Device Manager service to manage StorSimple volume containers</span></span>

## <a name="overview"></a><span data-ttu-id="8dc13-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8dc13-104">Overview</span></span>
<span data-ttu-id="8dc13-105">Den här självstudiekursen beskrivs hur du använda Enhetshanteraren för StorSimple-tjänsten för att skapa och hantera behållare för StorSimple-volym.</span><span class="sxs-lookup"><span data-stu-id="8dc13-105">This tutorial explains how to use the StorSimple Device Manager service to create and manage StorSimple volume containers.</span></span>

<span data-ttu-id="8dc13-106">En volymbehållare i en Microsoft Azure StorSimple-enhet innehåller en eller flera volymer som har lagringskonto, kryptering och förbrukning av bandbreddsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="8dc13-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="8dc13-107">En enhet kan ha flera volymbehållare för alla volymer.</span><span class="sxs-lookup"><span data-stu-id="8dc13-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="8dc13-108">En volymbehållare har följande attribut:</span><span class="sxs-lookup"><span data-stu-id="8dc13-108">A volume container has the following attributes:</span></span>

* <span data-ttu-id="8dc13-109">**Volymer** – nivåer eller lokalt Fäst StorSimple-volymer som ingår i volymbehållaren.</span><span class="sxs-lookup"><span data-stu-id="8dc13-109">**Volumes** – The tiered or locally pinned StorSimple volumes that are contained within the volume container.</span></span> 
* <span data-ttu-id="8dc13-110">**Kryptering** – en krypteringsnyckel som kan definieras för varje volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="8dc13-110">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="8dc13-111">Den här nyckeln används för att kryptera data som skickas från din StorSimple-enhet till molnet.</span><span class="sxs-lookup"><span data-stu-id="8dc13-111">This key is used for encrypting the data that is sent from your StorSimple device to the cloud.</span></span> <span data-ttu-id="8dc13-112">En militära klass AES 256 bitarsnyckel används med nyckeln anges av användaren.</span><span class="sxs-lookup"><span data-stu-id="8dc13-112">A military-grade AES-256 bit key is used with the user-entered key.</span></span> <span data-ttu-id="8dc13-113">Om du vill skydda dina data rekommenderar vi att du alltid aktiverar molnet lagringskryptering.</span><span class="sxs-lookup"><span data-stu-id="8dc13-113">To secure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="8dc13-114">**Storage-konto** – Azure storage-konto som används för att lagra data.</span><span class="sxs-lookup"><span data-stu-id="8dc13-114">**Storage account** – The Azure storage account that is used to store the data.</span></span> <span data-ttu-id="8dc13-115">Alla volymer som finns i en volymbehållare dela det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="8dc13-115">All the volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="8dc13-116">Du kan välja ett lagringskonto från en befintlig lista eller skapa ett nytt konto när du skapar volymbehållaren och anger sedan autentiseringsuppgifterna för det kontot.</span><span class="sxs-lookup"><span data-stu-id="8dc13-116">You can choose a storage account from an existing list, or create a new account when you create the volume container and then specify the access credentials for that account.</span></span>
* <span data-ttu-id="8dc13-117">**Molnet bandbredd** – bandbredden som används av enheten när data från enheten skickas till molnet.</span><span class="sxs-lookup"><span data-stu-id="8dc13-117">**Cloud bandwidth** – The bandwidth consumed by the device when the data from the device is being sent to the cloud.</span></span> <span data-ttu-id="8dc13-118">Du kan tillämpa en bandbreddskontroll genom att ange ett värde mellan 1 och 1 000 Mbps när du skapar den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="8dc13-118">You can enforce a bandwidth control by specifying a value between 1 Mbps and 1,000 Mbps when you create this container.</span></span> <span data-ttu-id="8dc13-119">Om du vill att enheten använder all tillgänglig bandbredd, ange fältet till **obegränsad**.</span><span class="sxs-lookup"><span data-stu-id="8dc13-119">If you want the device to consume all available bandwidth, set this field to **Unlimited**.</span></span> <span data-ttu-id="8dc13-120">Du kan också skapa och tillämpa en bandbreddsmall för att allokera bandbredd enligt schemat.</span><span class="sxs-lookup"><span data-stu-id="8dc13-120">You can also create and apply a bandwidth template to allocate bandwidth based on schedule.</span></span>

<span data-ttu-id="8dc13-121">Följande procedurer beskriver hur du använder den virtuella StorSimple **volymbehållare** bladet för att slutföra följande vanliga åtgärder:</span><span class="sxs-lookup"><span data-stu-id="8dc13-121">The following procedures explain how to use the StorSimple **Volume containers** blade to complete the following common operations:</span></span>

* <span data-ttu-id="8dc13-122">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="8dc13-122">Add a volume container</span></span>
* <span data-ttu-id="8dc13-123">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="8dc13-123">Modify a volume container</span></span>
* <span data-ttu-id="8dc13-124">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="8dc13-124">Delete a volume container</span></span>

## <a name="add-a-volume-container"></a><span data-ttu-id="8dc13-125">Lägg till volymbehållare</span><span class="sxs-lookup"><span data-stu-id="8dc13-125">Add a volume container</span></span>
<span data-ttu-id="8dc13-126">Utför följande steg för att lägga till en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="8dc13-126">Perform the following steps to add a volume container.</span></span>

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="8dc13-127">Ändra en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="8dc13-127">Modify a volume container</span></span>
<span data-ttu-id="8dc13-128">Utför följande steg om du vill ändra en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="8dc13-128">Perform the following steps to modify a volume container.</span></span>

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="8dc13-129">Ta bort en volymbehållare</span><span class="sxs-lookup"><span data-stu-id="8dc13-129">Delete a volume container</span></span>
<span data-ttu-id="8dc13-130">En volymbehållare har volymer i den.</span><span class="sxs-lookup"><span data-stu-id="8dc13-130">A volume container has volumes within it.</span></span> <span data-ttu-id="8dc13-131">Det kan endast tas bort om de volymer som finns i den först tas bort.</span><span class="sxs-lookup"><span data-stu-id="8dc13-131">It can be deleted only if all the volumes contained in it are first deleted.</span></span> <span data-ttu-id="8dc13-132">Utför följande steg för att ta bort en volymbehållare.</span><span class="sxs-lookup"><span data-stu-id="8dc13-132">Perform the following steps to delete a volume container.</span></span>

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="8dc13-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8dc13-133">Next steps</span></span>
* <span data-ttu-id="8dc13-134">Lär dig mer om [hantera StorSimple-volymer](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="8dc13-134">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span> 
* <span data-ttu-id="8dc13-135">Lär dig mer om [använder Enhetshanteraren för StorSimple-tjänsten för att administrera din StorSimple-enhet](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="8dc13-135">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

