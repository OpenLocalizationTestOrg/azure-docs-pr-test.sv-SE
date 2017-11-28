---
title: "Skapa en export jobb för Azure Import/Export | Microsoft Docs"
description: "Lär dig hur du skapar ett exportjobb för tjänsten Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: bdeac373aa8270bd9de8f135ec7166d744fd83ae
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a><span data-ttu-id="2cb46-103">Skapa ett exportjobb för tjänsten Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="2cb46-103">Creating an export job for the Azure Import/Export service</span></span>
<span data-ttu-id="2cb46-104">Skapa ett exportjobb för tjänsten Microsoft Azure Import/Export med hjälp av REST API omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="2cb46-104">Creating an export job for the Microsoft Azure Import/Export service using the REST API involves the following steps:</span></span>

-   <span data-ttu-id="2cb46-105">Om du väljer att exportera blobbarna.</span><span class="sxs-lookup"><span data-stu-id="2cb46-105">Selecting the blobs to export.</span></span>

-   <span data-ttu-id="2cb46-106">Hämta en leveransplats.</span><span class="sxs-lookup"><span data-stu-id="2cb46-106">Obtaining a shipping location.</span></span>

-   <span data-ttu-id="2cb46-107">Skapa exportjobbet.</span><span class="sxs-lookup"><span data-stu-id="2cb46-107">Creating the export job.</span></span>

-   <span data-ttu-id="2cb46-108">Leverera tomt enheter till Microsoft via en stöds operatör-tjänst.</span><span class="sxs-lookup"><span data-stu-id="2cb46-108">Shipping your empty drives to Microsoft via a supported carrier service.</span></span>

-   <span data-ttu-id="2cb46-109">Uppdaterar exportjobbet med Paketinformation om.</span><span class="sxs-lookup"><span data-stu-id="2cb46-109">Updating the export job with the package information.</span></span>

-   <span data-ttu-id="2cb46-110">Ta emot enheterna tillbaka från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cb46-110">Receiving the drives back from Microsoft.</span></span>

 <span data-ttu-id="2cb46-111">Se [med hjälp av tjänsten Windows Azure Import/Export för att överföra Data till Blob Storage](storage-import-export-service.md) för en översikt över Import/Export-tjänsten och en genomgång som visar hur du använder den [Azure-portalen](https://portal.azure.com/) att skapa och Hantera importera och exportera jobben.</span><span class="sxs-lookup"><span data-stu-id="2cb46-111">See [Using the Windows Azure Import/Export service to Transfer Data to Blob Storage](storage-import-export-service.md) for an overview of the Import/Export service and a tutorial that demonstrates how to use the [Azure portal](https://portal.azure.com/) to create and manage import and export jobs.</span></span>

## <a name="selecting-blobs-to-export"></a><span data-ttu-id="2cb46-112">Välja att exportera blobbarna</span><span class="sxs-lookup"><span data-stu-id="2cb46-112">Selecting blobs to export</span></span>
 <span data-ttu-id="2cb46-113">Om du vill skapa ett exportjobb, behöver du ange en lista över blobbar som du vill exportera från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2cb46-113">To create an export job, you will need to provide a list of blobs that you want to export from your storage account.</span></span> <span data-ttu-id="2cb46-114">Det finns några sätt att välja blobbar som ska exporteras:</span><span class="sxs-lookup"><span data-stu-id="2cb46-114">There are a few ways to select blobs to be exported:</span></span>

-   <span data-ttu-id="2cb46-115">Du kan använda en relativ blob-sökväg för att välja en enda blob och alla dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="2cb46-115">You can use a relative blob path to select a single blob and all of its snapshots.</span></span>

-   <span data-ttu-id="2cb46-116">Du kan använda en relativ blob-sökväg för att välja en enda blob undantag av dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="2cb46-116">You can use a relative blob path to select a single blob excluding its snapshots.</span></span>

-   <span data-ttu-id="2cb46-117">Du kan använda en relativ blob-sökväg och ett ögonblicksbild för att välja en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="2cb46-117">You can use a relative blob path and a snapshot time to select a single snapshot.</span></span>

-   <span data-ttu-id="2cb46-118">Du kan använda ett blob-prefix för att markera alla blobbar och ögonblicksbilder med det angivna prefixet.</span><span class="sxs-lookup"><span data-stu-id="2cb46-118">You can use a blob prefix to select all blobs and snapshots with the given prefix.</span></span>

-   <span data-ttu-id="2cb46-119">Du kan exportera alla blobbar och ögonblicksbilder i storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2cb46-119">You can export all blobs and snapshots in the storage account.</span></span>

 <span data-ttu-id="2cb46-120">Läs mer om hur du anger BLOB att exportera den [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-120">For more information about specifying blobs to export, see the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="2cb46-121">Hämta din leverans plats</span><span class="sxs-lookup"><span data-stu-id="2cb46-121">Obtaining your shipping location</span></span>
<span data-ttu-id="2cb46-122">Innan du skapar ett exportjobb, måste du skaffa ett leverans platsnamn och adress genom att anropa den [få plats](https://portal.azure.com) eller [listan platser](/rest/api/storageimportexport/listlocations) igen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-122">Before creating an export job, you need to obtain a shipping location name and address by calling the [Get Location](https://portal.azure.com) or [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="2cb46-123">`List Locations`Returnerar en lista över platser och deras e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="2cb46-123">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="2cb46-124">Du kan välja en plats från listan över returnerade och leverera din hårddiskar till adressen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-124">You can select a location from the returned list and ship your hard drives to that address.</span></span> <span data-ttu-id="2cb46-125">Du kan också använda den `Get Location` åtgärden att direkt hämta leveransadress för en viss plats.</span><span class="sxs-lookup"><span data-stu-id="2cb46-125">You can also use the `Get Location` operation to obtain the shipping address for a specific location directly.</span></span>

<span data-ttu-id="2cb46-126">Följ stegen nedan för att hämta leveransplatsen för:</span><span class="sxs-lookup"><span data-stu-id="2cb46-126">Follow the steps below to obtain the shipping location:</span></span>

-   <span data-ttu-id="2cb46-127">Identifiera namnet på platsen för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="2cb46-127">Identify the name of the location of your storage account.</span></span> <span data-ttu-id="2cb46-128">Det här värdet kan hittas den **plats** på storage-konto **instrumentpanelen** i klassiskt portalen eller efterfrågade för med hjälp av service management API-åtgärd [hämta lagring Kontot egenskaper](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="2cb46-128">This value can be found under the **Location** field on the storage account's **Dashboard** in the classic portal or queried for by using the service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="2cb46-129">Hämta platsen för att utföra det här lagringskontot genom att anropa den `Get Location` igen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-129">Retrieve the location that are available to process this storage account by calling the `Get Location` operation.</span></span>

-   <span data-ttu-id="2cb46-130">Om den `AlternateLocations` -egenskapen för platsen innehåller själva platsen och sedan går bra att använda den här platsen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-130">If the `AlternateLocations` property of the location contains the location itself, then it is okay to use this location.</span></span> <span data-ttu-id="2cb46-131">Annars kan anropa den `Get Location` igen med en av de alternativa platserna.</span><span class="sxs-lookup"><span data-stu-id="2cb46-131">Otherwise, call the `Get Location` operation again with one of the alternate locations.</span></span> <span data-ttu-id="2cb46-132">Den ursprungliga platsen kan tillfälligt stängt för underhåll.</span><span class="sxs-lookup"><span data-stu-id="2cb46-132">The original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-the-export-job"></a><span data-ttu-id="2cb46-133">Skapa exportjobbet</span><span class="sxs-lookup"><span data-stu-id="2cb46-133">Creating the export job</span></span>
 <span data-ttu-id="2cb46-134">Om du vill skapa exportjobbet anropa den [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-134">To create the export job, call the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="2cb46-135">Du måste ange följande information:</span><span class="sxs-lookup"><span data-stu-id="2cb46-135">You will need to provide the following information:</span></span>

-   <span data-ttu-id="2cb46-136">Ett namn för jobbet.</span><span class="sxs-lookup"><span data-stu-id="2cb46-136">A name for the job.</span></span>

-   <span data-ttu-id="2cb46-137">Namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="2cb46-137">The storage account name.</span></span>

-   <span data-ttu-id="2cb46-138">Leverans platsnamnet, hämtades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2cb46-138">The shipping location name, obtained in the previous step.</span></span>

-   <span data-ttu-id="2cb46-139">En jobbtyp (exportera).</span><span class="sxs-lookup"><span data-stu-id="2cb46-139">A job type (Export).</span></span>

-   <span data-ttu-id="2cb46-140">Returadressen där enheterna som ska skickas när exportjobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2cb46-140">The return address where the drives should be sent after the export job has completed.</span></span>

-   <span data-ttu-id="2cb46-141">Lista över blobbar (eller blob-prefix) som ska exporteras.</span><span class="sxs-lookup"><span data-stu-id="2cb46-141">The list of blobs (or blob prefixes) to be exported.</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="2cb46-142">Leverera dina enheter</span><span class="sxs-lookup"><span data-stu-id="2cb46-142">Shipping your drives</span></span>
 <span data-ttu-id="2cb46-143">Använd sedan verktyget Azure Import/Export för att avgöra hur många enheter som du måste skicka, baserat på blobbar som du har valt exporteras och storleken på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="2cb46-143">Next, use the Azure Import/Export Tool to determine the number of drives you need to send, based on the blobs you have selected to be exported and the drive size.</span></span> <span data-ttu-id="2cb46-144">Finns det [referens för Azure Import/Export verktyg](storage-import-export-tool-how-to-v1.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="2cb46-144">See the [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) for details.</span></span>

 <span data-ttu-id="2cb46-145">Paketera enheter i ett paket och skicka dem till den adress som du fått i tidigare steg.</span><span class="sxs-lookup"><span data-stu-id="2cb46-145">Package the drives in a single package and ship them to the address obtained in the earlier step.</span></span> <span data-ttu-id="2cb46-146">Observera att spåra antalet paketet för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="2cb46-146">Note the tracking number of your package for the next step.</span></span>

> [!NOTE]
>  <span data-ttu-id="2cb46-147">Du måste leverera enheter via en stöds operatör-tjänst som tillhandahåller ett spårningsnummer för paketet.</span><span class="sxs-lookup"><span data-stu-id="2cb46-147">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-the-export-job-with-your-package-information"></a><span data-ttu-id="2cb46-148">Uppdaterar exportjobbet med Paketinformation</span><span class="sxs-lookup"><span data-stu-id="2cb46-148">Updating the export job with your package information</span></span>
 <span data-ttu-id="2cb46-149">När du har Spårningsnumret till din kan anropa den [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) åtgärden Uppdatera namnet på operatör och spårnings-ID för jobbet.</span><span class="sxs-lookup"><span data-stu-id="2cb46-149">After you have your tracking number, call the [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation to updated the carrier name and tracking number for the job.</span></span> <span data-ttu-id="2cb46-150">Alternativt kan du ange antalet enheter, returadressen och leveransdatum samt.</span><span class="sxs-lookup"><span data-stu-id="2cb46-150">You can optionally specify the number of drives, the return address, and the shipping date as well.</span></span>

## <a name="receiving-the-package"></a><span data-ttu-id="2cb46-151">Tar emot paketet</span><span class="sxs-lookup"><span data-stu-id="2cb46-151">Receiving the package</span></span>
 <span data-ttu-id="2cb46-152">När din exportjobb har bearbetats returneras enheter till dig med krypterade data.</span><span class="sxs-lookup"><span data-stu-id="2cb46-152">After your export job has been processed, your drives will be returned to you with your encrypted data.</span></span> <span data-ttu-id="2cb46-153">Du kan hämta BitLocker-nyckel för var och en av enheterna genom att anropa den [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="2cb46-153">You can retrieve the BitLocker key for each of the drives by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="2cb46-154">Sedan kan du låsa upp enheten med nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2cb46-154">You can then unlock the drive using the key.</span></span> <span data-ttu-id="2cb46-155">Enheten manifestfilen på varje enhet innehåller listan med filer på enheten som den ursprungliga blob-adressen för varje fil.</span><span class="sxs-lookup"><span data-stu-id="2cb46-155">The drive manifest file on each drive contains the list of files on the drive, as well as the original blob address for each file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2cb46-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2cb46-156">Next steps</span></span>

* [<span data-ttu-id="2cb46-157">Med hjälp av tjänsten Import/Export REST API</span><span class="sxs-lookup"><span data-stu-id="2cb46-157">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
