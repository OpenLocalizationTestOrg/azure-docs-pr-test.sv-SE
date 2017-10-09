---
title: "aaaCreate export jobb för Azure Import/Export | Microsoft Docs"
description: "Lär dig hur toocreate export jobbet för hello Microsoft Azure Import/Export service."
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
ms.openlocfilehash: 4a10b42cc86dbf3bcea3a515bc065e2259228ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-export-job-for-hello-azure-importexport-service"></a><span data-ttu-id="fb4b7-103">Skapa ett exportjobb för hello Azure Import/Export service</span><span class="sxs-lookup"><span data-stu-id="fb4b7-103">Creating an export job for hello Azure Import/Export service</span></span>
<span data-ttu-id="fb4b7-104">Skapa ett exportjobb för hello Microsoft Azure Import/Export-tjänsten använder hello REST API omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fb4b7-104">Creating an export job for hello Microsoft Azure Import/Export service using hello REST API involves hello following steps:</span></span>

-   <span data-ttu-id="fb4b7-105">Att välja hello-blobbar tooexport.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-105">Selecting hello blobs tooexport.</span></span>

-   <span data-ttu-id="fb4b7-106">Hämta en leveransplats.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-106">Obtaining a shipping location.</span></span>

-   <span data-ttu-id="fb4b7-107">Skapa hello exportjobb.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-107">Creating hello export job.</span></span>

-   <span data-ttu-id="fb4b7-108">Leverera din tomma enheter tooMicrosoft via en stöds operatör-tjänst.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-108">Shipping your empty drives tooMicrosoft via a supported carrier service.</span></span>

-   <span data-ttu-id="fb4b7-109">Uppdaterar hello exportjobb med hello Paketinformation.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-109">Updating hello export job with hello package information.</span></span>

-   <span data-ttu-id="fb4b7-110">Ta emot hello enheter tillbaka från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-110">Receiving hello drives back from Microsoft.</span></span>

 <span data-ttu-id="fb4b7-111">Se [med hello Windows Azure Import/Export service tooTransfer Data tooBlob lagring](storage-import-export-service.md) för en översikt över hello Import/Export service och en genomgång som visar hur toouse hello [Azure-portalen](https://portal.azure.com/) toocreate och hantera importera och exportera jobben.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-111">See [Using hello Windows Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello [Azure portal](https://portal.azure.com/) toocreate and manage import and export jobs.</span></span>

## <a name="selecting-blobs-tooexport"></a><span data-ttu-id="fb4b7-112">Att välja blobbar tooexport</span><span class="sxs-lookup"><span data-stu-id="fb4b7-112">Selecting blobs tooexport</span></span>
 <span data-ttu-id="fb4b7-113">toocreate ett exportjobb måste tooprovide en lista över blobbar som du vill tooexport från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-113">toocreate an export job, you will need tooprovide a list of blobs that you want tooexport from your storage account.</span></span> <span data-ttu-id="fb4b7-114">Det finns ett par sätt tooselect blobbar toobe exporteras:</span><span class="sxs-lookup"><span data-stu-id="fb4b7-114">There are a few ways tooselect blobs toobe exported:</span></span>

-   <span data-ttu-id="fb4b7-115">Du kan använda en relativ blob sökvägen tooselect en enda blob och alla dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-115">You can use a relative blob path tooselect a single blob and all of its snapshots.</span></span>

-   <span data-ttu-id="fb4b7-116">Du kan använda en relativ blob sökvägen tooselect en enda blob undantag av dess ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-116">You can use a relative blob path tooselect a single blob excluding its snapshots.</span></span>

-   <span data-ttu-id="fb4b7-117">Du kan använda en relativ blob-sökväg och en ögonblicksbild tid tooselect en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-117">You can use a relative blob path and a snapshot time tooselect a single snapshot.</span></span>

-   <span data-ttu-id="fb4b7-118">Du kan använda en blob-prefixet tooselect alla blobbar och ögonblicksbilder med hello angivna prefix.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-118">You can use a blob prefix tooselect all blobs and snapshots with hello given prefix.</span></span>

-   <span data-ttu-id="fb4b7-119">Du kan exportera alla blobbar och ögonblicksbilder i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-119">You can export all blobs and snapshots in hello storage account.</span></span>

 <span data-ttu-id="fb4b7-120">Mer information om att ange-blobbar tooexport, finns hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-120">For more information about specifying blobs tooexport, see hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="fb4b7-121">Hämta din leverans plats</span><span class="sxs-lookup"><span data-stu-id="fb4b7-121">Obtaining your shipping location</span></span>
<span data-ttu-id="fb4b7-122">Innan du skapar ett exportjobb du behöver tooobtain en leverans namn och adress genom att anropa hello [få plats](https://portal.azure.com) eller [listan platser](/rest/api/storageimportexport/listlocations) igen.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-122">Before creating an export job, you need tooobtain a shipping location name and address by calling hello [Get Location](https://portal.azure.com) or [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="fb4b7-123">`List Locations`Returnerar en lista över platser och deras e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-123">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="fb4b7-124">Du kan välja en plats från hello returnerade lista och leverera din hårddiskar toothat adress.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-124">You can select a location from hello returned list and ship your hard drives toothat address.</span></span> <span data-ttu-id="fb4b7-125">Du kan också använda hello `Get Location` åtgärden tooobtain hello leveransadress för en specifik plats direkt.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-125">You can also use hello `Get Location` operation tooobtain hello shipping address for a specific location directly.</span></span>

<span data-ttu-id="fb4b7-126">Så hello under tooobtain hello leverans platsen:</span><span class="sxs-lookup"><span data-stu-id="fb4b7-126">Follow hello steps below tooobtain hello shipping location:</span></span>

-   <span data-ttu-id="fb4b7-127">Identifiera hello namn hello platsen för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-127">Identify hello name of hello location of your storage account.</span></span> <span data-ttu-id="fb4b7-128">Det här värdet kan hittas under hello **plats** på hello lagringskontots **instrumentpanelen** i hello klassisk portal eller efterfrågade för med hello service management API-åtgärd [hämta Lagringskontoegenskaperna](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="fb4b7-128">This value can be found under hello **Location** field on hello storage account's **Dashboard** in hello classic portal or queried for by using hello service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="fb4b7-129">Hämta hello platsen som är tillgängliga tooprocess det här lagringskontot genom att anropa hello `Get Location` igen.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-129">Retrieve hello location that are available tooprocess this storage account by calling hello `Get Location` operation.</span></span>

-   <span data-ttu-id="fb4b7-130">Om hello `AlternateLocations` egenskapen hello plats innehåller själva hello platsen så är det bra toouse den här platsen.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-130">If hello `AlternateLocations` property of hello location contains hello location itself, then it is okay toouse this location.</span></span> <span data-ttu-id="fb4b7-131">Annars kan anropa hello `Get Location` igen med en av hello alternativa platser.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-131">Otherwise, call hello `Get Location` operation again with one of hello alternate locations.</span></span> <span data-ttu-id="fb4b7-132">hello ursprungsplatsen kan tillfälligt stängt för underhåll.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-132">hello original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-hello-export-job"></a><span data-ttu-id="fb4b7-133">Skapa hello exportjobb</span><span class="sxs-lookup"><span data-stu-id="fb4b7-133">Creating hello export job</span></span>
 <span data-ttu-id="fb4b7-134">toocreate hello exportjobb, anrop hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-134">toocreate hello export job, call hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="fb4b7-135">Du behöver tooprovide hello följande information:</span><span class="sxs-lookup"><span data-stu-id="fb4b7-135">You will need tooprovide hello following information:</span></span>

-   <span data-ttu-id="fb4b7-136">Ett namn för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-136">A name for hello job.</span></span>

-   <span data-ttu-id="fb4b7-137">Hej lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-137">hello storage account name.</span></span>

-   <span data-ttu-id="fb4b7-138">hello leverans platsnamn, fick i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-138">hello shipping location name, obtained in hello previous step.</span></span>

-   <span data-ttu-id="fb4b7-139">En jobbtyp (exportera).</span><span class="sxs-lookup"><span data-stu-id="fb4b7-139">A job type (Export).</span></span>

-   <span data-ttu-id="fb4b7-140">hello avsändaradress där hello enheter som ska skickas när hello exportjobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-140">hello return address where hello drives should be sent after hello export job has completed.</span></span>

-   <span data-ttu-id="fb4b7-141">hello lista över BLOB (eller blob-prefix) toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-141">hello list of blobs (or blob prefixes) toobe exported.</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="fb4b7-142">Leverera dina enheter</span><span class="sxs-lookup"><span data-stu-id="fb4b7-142">Shipping your drives</span></span>
 <span data-ttu-id="fb4b7-143">Sedan använder hello Azure Import/Export verktyget toodetermine hello antalet enheter som du behöver toosend, baserat på hello blob som du har valt toobe exporteras och hello enhetsstorlek.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-143">Next, use hello Azure Import/Export Tool toodetermine hello number of drives you need toosend, based on hello blobs you have selected toobe exported and hello drive size.</span></span> <span data-ttu-id="fb4b7-144">Se hello [referens för Azure Import/Export verktyg](storage-import-export-tool-how-to-v1.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-144">See hello [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) for details.</span></span>

 <span data-ttu-id="fb4b7-145">Paketet hello enheter i en enda paketera och leverera dem toohello adress som du fått i hello tidigare steg.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-145">Package hello drives in a single package and ship them toohello address obtained in hello earlier step.</span></span> <span data-ttu-id="fb4b7-146">Observera hello spåra antalet paketet för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-146">Note hello tracking number of your package for hello next step.</span></span>

> [!NOTE]
>  <span data-ttu-id="fb4b7-147">Du måste leverera enheter via en stöds operatör-tjänst som tillhandahåller ett spårningsnummer för paketet.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-147">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-hello-export-job-with-your-package-information"></a><span data-ttu-id="fb4b7-148">Uppdaterar hello exportjobb med Paketinformation</span><span class="sxs-lookup"><span data-stu-id="fb4b7-148">Updating hello export job with your package information</span></span>
 <span data-ttu-id="fb4b7-149">När du har Spårningsnumret till din kan anropa hello [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) operatör åtgärdsnamn tooupdated hello och spårnings-ID för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-149">After you have your tracking number, call hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation tooupdated hello carrier name and tracking number for hello job.</span></span> <span data-ttu-id="fb4b7-150">Alternativt kan du ange hello antalet enheter, hello avsändaradress och hello leverans samt datum.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-150">You can optionally specify hello number of drives, hello return address, and hello shipping date as well.</span></span>

## <a name="receiving-hello-package"></a><span data-ttu-id="fb4b7-151">Mottagande hello-paket</span><span class="sxs-lookup"><span data-stu-id="fb4b7-151">Receiving hello package</span></span>
 <span data-ttu-id="fb4b7-152">När din exportjobb har bearbetats returnerade enheter tooyou med krypterade data.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-152">After your export job has been processed, your drives will be returned tooyou with your encrypted data.</span></span> <span data-ttu-id="fb4b7-153">Du kan hämta hello BitLocker-nyckel för varje hello enheter genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-153">You can retrieve hello BitLocker key for each of hello drives by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="fb4b7-154">Sedan kan du låsa upp hello-enhet med hjälp av hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-154">You can then unlock hello drive using hello key.</span></span> <span data-ttu-id="fb4b7-155">hello enhet manifestfilen på varje enhet innehåller hello lista över filer på hello enhet samt hello ursprungliga blob-adressen för varje fil.</span><span class="sxs-lookup"><span data-stu-id="fb4b7-155">hello drive manifest file on each drive contains hello list of files on hello drive, as well as hello original blob address for each file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb4b7-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb4b7-156">Next steps</span></span>

* [<span data-ttu-id="fb4b7-157">Med hjälp av hello Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="fb4b7-157">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
