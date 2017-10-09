---
title: "aaaCreate ett importjobb för Azure Import/Export | Microsoft Docs"
description: "Lär dig hur toocreate en import för hello Microsoft Azure Import/Export service."
author: muralikk
manager: syadav
editor: syadav
services: storage
documentationcenter: 
ms.assetid: 8b886e83-6148-4149-9d0f-5d48ec822475
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: da974c33a3688bb5e2412c8bfcbeca704096c2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-import-job-for-hello-azure-importexport-service"></a><span data-ttu-id="e364e-103">Skapa ett importjobb för hello Azure Import/Export service</span><span class="sxs-lookup"><span data-stu-id="e364e-103">Creating an import job for hello Azure Import/Export service</span></span>

<span data-ttu-id="e364e-104">Skapa ett importjobb för hello Microsoft Azure Import/Export-tjänsten använder hello REST API omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e364e-104">Creating an import job for hello Microsoft Azure Import/Export service using hello REST API involves hello following steps:</span></span>

-   <span data-ttu-id="e364e-105">Förbereda enheter med hello Azure Import/Export-verktyget.</span><span class="sxs-lookup"><span data-stu-id="e364e-105">Preparing drives with hello Azure Import/Export Tool.</span></span>

-   <span data-ttu-id="e364e-106">Hämta hello plats toowhich tooship hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e364e-106">Obtaining hello location toowhich tooship hello drive.</span></span>

-   <span data-ttu-id="e364e-107">Skapa hello importjobbet.</span><span class="sxs-lookup"><span data-stu-id="e364e-107">Creating hello import job.</span></span>

-   <span data-ttu-id="e364e-108">Leverera hello enheter tooMicrosoft via en stöds operatör-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e364e-108">Shipping hello drives tooMicrosoft via a supported carrier service.</span></span>

-   <span data-ttu-id="e364e-109">Uppdaterar hello importjobbet med hello leveransinformation.</span><span class="sxs-lookup"><span data-stu-id="e364e-109">Updating hello import job with hello shipping details.</span></span>

 <span data-ttu-id="e364e-110">Se [med hello Microsoft Azure Import/Export service tooTransfer Data tooBlob lagring](storage-import-export-service.md) för en översikt över hello Import/Export service och en genomgång som visar hur toouse hello [Azure-portalen](https://portal.azure.com/) toocreate och hantera importera och exportera jobben.</span><span class="sxs-lookup"><span data-stu-id="e364e-110">See [Using hello Microsoft Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello [Azure  portal](https://portal.azure.com/) toocreate and manage import and export jobs.</span></span>

## <a name="preparing-drives-with-hello-azure-importexport-tool"></a><span data-ttu-id="e364e-111">Förbereda enheter med hello Azure Import/Export-verktyget</span><span class="sxs-lookup"><span data-stu-id="e364e-111">Preparing drives with hello Azure Import/Export Tool</span></span>

<span data-ttu-id="e364e-112">hello steg tooprepare enheter för importen är hello samma om du skapar hello jobvia hello portalen eller via hello REST API.</span><span class="sxs-lookup"><span data-stu-id="e364e-112">hello steps tooprepare drives for an import job are hello same whether you create hello jobvia hello portal or via hello REST API.</span></span>

<span data-ttu-id="e364e-113">Nedan finns en kort översikt över förberedelse av enheten.</span><span class="sxs-lookup"><span data-stu-id="e364e-113">Below is a brief overview of drive preparation.</span></span> <span data-ttu-id="e364e-114">Se toohello [Azure Import ExportTool referens](storage-import-export-tool-how-to-v1.md) fullständiga instruktioner.</span><span class="sxs-lookup"><span data-stu-id="e364e-114">Refer toohello [Azure Import-ExportTool Reference](storage-import-export-tool-how-to-v1.md) for complete instructions.</span></span> <span data-ttu-id="e364e-115">Du kan hämta hello Azure Import/Export verktyget [här](http://go.microsoft.com/fwlink/?LinkID=301900).</span><span class="sxs-lookup"><span data-stu-id="e364e-115">You can download hello Azure Import/Export Tool [here](http://go.microsoft.com/fwlink/?LinkID=301900).</span></span>

<span data-ttu-id="e364e-116">Förbereda enheten omfattar:</span><span class="sxs-lookup"><span data-stu-id="e364e-116">Preparing your drive involves:</span></span>

-   <span data-ttu-id="e364e-117">Identifiera hello data toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="e364e-117">Identifying hello data toobe imported.</span></span>

-   <span data-ttu-id="e364e-118">Identifiera hello mål blobbar i Windows Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e364e-118">Identifying hello destination blobs in Windows Azure Storage.</span></span>

-   <span data-ttu-id="e364e-119">Använder hello Azure Import/Export verktyget toocopy data-tooone eller hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e364e-119">Using hello Azure Import/Export Tool toocopy your data tooone or more hard drives.</span></span>

 <span data-ttu-id="e364e-120">hello Azure Import/Export verktyget skapar också en manifestfil för varje hello enheter som den är redo.</span><span class="sxs-lookup"><span data-stu-id="e364e-120">hello Azure Import/Export Tool will also generate a manifest file for each of hello drives as it is prepared.</span></span> <span data-ttu-id="e364e-121">En manifestfil innehåller:</span><span class="sxs-lookup"><span data-stu-id="e364e-121">A manifest file contains:</span></span>

-   <span data-ttu-id="e364e-122">En uppräkning av alla hello-filer som är avsedd för överföringen och hello mappningar från dessa filer tooblobs.</span><span class="sxs-lookup"><span data-stu-id="e364e-122">An enumeration of all hello files intended for upload and hello mappings from these files tooblobs.</span></span>

-   <span data-ttu-id="e364e-123">Kontrollsummor för hello segment för varje fil.</span><span class="sxs-lookup"><span data-stu-id="e364e-123">Checksums of hello segments of each file.</span></span>

-   <span data-ttu-id="e364e-124">Information om hello metadata och egenskaper tooassociate med varje blob.</span><span class="sxs-lookup"><span data-stu-id="e364e-124">Information about hello metadata and properties tooassociate with each blob.</span></span>

-   <span data-ttu-id="e364e-125">En lista över hello åtgärd tootake om en blob som överförs har hello samma namn som en befintlig blob i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="e364e-125">A listing of hello action tootake if a blob that is being uploaded has hello same name as an existing blob in hello container.</span></span> <span data-ttu-id="e364e-126">Möjliga alternativ är: a) över hello blob med hello-fil, b) skydda hello befintlig blob och hoppa över överföring hello-fil, c) lägga till ett suffix toohello namn så att den inte står i konflikt med andra filer.</span><span class="sxs-lookup"><span data-stu-id="e364e-126">Possible options are: a) overwrite hello blob with hello file, b) keep hello existing blob and skip uploading hello file, c) append a suffix toohello name so that it does not conflict with other files.</span></span>

## <a name="obtaining-your-shipping-location"></a><span data-ttu-id="e364e-127">Hämta din leverans plats</span><span class="sxs-lookup"><span data-stu-id="e364e-127">Obtaining your shipping location</span></span>

<span data-ttu-id="e364e-128">Innan du skapar ett importjobb du behöver tooobtain en leverans namn och adress genom att anropa hello [listan platser](/rest/api/storageimportexport/listlocations) igen.</span><span class="sxs-lookup"><span data-stu-id="e364e-128">Before creating an import job, you need tooobtain a shipping location name and address by calling hello [List Locations](/rest/api/storageimportexport/listlocations) operation.</span></span> <span data-ttu-id="e364e-129">`List Locations`Returnerar en lista över platser och deras e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="e364e-129">`List Locations` will return a list of locations and their mailing addresses.</span></span> <span data-ttu-id="e364e-130">Du kan välja en plats från hello returnerade lista och leverera din hårddiskar toothat adress.</span><span class="sxs-lookup"><span data-stu-id="e364e-130">You can select a location from hello returned list and ship your hard drives toothat address.</span></span> <span data-ttu-id="e364e-131">Du kan också använda hello `Get Location` åtgärden tooobtain hello leveransadress för en specifik plats direkt.</span><span class="sxs-lookup"><span data-stu-id="e364e-131">You can also use hello `Get Location` operation tooobtain hello shipping address for a specific location directly.</span></span>

 <span data-ttu-id="e364e-132">Så hello under tooobtain hello leverans platsen:</span><span class="sxs-lookup"><span data-stu-id="e364e-132">Follow hello steps below tooobtain hello shipping location:</span></span>

-   <span data-ttu-id="e364e-133">Identifiera hello namn hello platsen för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e364e-133">Identify hello name of hello location of your storage account.</span></span> <span data-ttu-id="e364e-134">Det här värdet kan hittas under hello **plats** på hello lagringskontots **instrumentpanelen** i hello Azure portal eller efterfrågade för med hello service management API-åtgärd [hämta lagring Kontot egenskaper](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span><span class="sxs-lookup"><span data-stu-id="e364e-134">This value can be found under hello **Location** field on hello storage account's **Dashboard** in hello Azure portal or queried for by using hello service management API operation [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties).</span></span>

-   <span data-ttu-id="e364e-135">Hämta hello-plats som är tillgängliga tooprocess det här lagringskontot genom att anropa hello `Get Location` igen.</span><span class="sxs-lookup"><span data-stu-id="e364e-135">Retrieve hello location that is available tooprocess this storage account by calling hello `Get Location` operation.</span></span>

-   <span data-ttu-id="e364e-136">Om hello `AlternateLocations` egenskapen hello plats innehåller själva hello platsen så är det bra toouse den här platsen.</span><span class="sxs-lookup"><span data-stu-id="e364e-136">If hello `AlternateLocations` property of hello location contains hello location itself, then it is okay toouse this location.</span></span> <span data-ttu-id="e364e-137">Annars kan anropa hello `Get Location` igen med en av hello alternativa platser.</span><span class="sxs-lookup"><span data-stu-id="e364e-137">Otherwise, call hello `Get Location` operation again with one of hello alternate locations.</span></span> <span data-ttu-id="e364e-138">hello ursprungsplatsen kan tillfälligt stängt för underhåll.</span><span class="sxs-lookup"><span data-stu-id="e364e-138">hello original location might be temporarily closed for maintenance.</span></span>

## <a name="creating-hello-import-job"></a><span data-ttu-id="e364e-139">Skapa hello-importjobb</span><span class="sxs-lookup"><span data-stu-id="e364e-139">Creating hello import job</span></span>
<span data-ttu-id="e364e-140">toocreate hello importjobbet, anrop hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) igen.</span><span class="sxs-lookup"><span data-stu-id="e364e-140">toocreate hello import job, call hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation.</span></span> <span data-ttu-id="e364e-141">Du behöver tooprovide hello följande information:</span><span class="sxs-lookup"><span data-stu-id="e364e-141">You will need tooprovide hello following information:</span></span>

-   <span data-ttu-id="e364e-142">Ett namn för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="e364e-142">A name for hello job.</span></span>

-   <span data-ttu-id="e364e-143">Hej lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="e364e-143">hello storage account name.</span></span>

-   <span data-ttu-id="e364e-144">hello leverans platsnamn, hämtas från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e364e-144">hello shipping location name, obtained from hello previous step.</span></span>

-   <span data-ttu-id="e364e-145">En jobbtyp (importera).</span><span class="sxs-lookup"><span data-stu-id="e364e-145">A job type (Import).</span></span>

-   <span data-ttu-id="e364e-146">hello avsändaradress där hello enheter som ska skickas när hello importjobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e364e-146">hello return address where hello drives should be sent after hello import job has completed.</span></span>

-   <span data-ttu-id="e364e-147">hello lista över enheter i hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="e364e-147">hello list of drives in hello job.</span></span> <span data-ttu-id="e364e-148">För varje enhet, måste du inkludera hello följande information som erhölls vid förberedelsen för hello enhet:</span><span class="sxs-lookup"><span data-stu-id="e364e-148">For each drive, you must include hello following information that was obtained during hello drive preparation step:</span></span>

    -   <span data-ttu-id="e364e-149">hello enhet Id</span><span class="sxs-lookup"><span data-stu-id="e364e-149">hello drive Id</span></span>

    -   <span data-ttu-id="e364e-150">hello BitLocker-nyckel</span><span class="sxs-lookup"><span data-stu-id="e364e-150">hello BitLocker key</span></span>

    -   <span data-ttu-id="e364e-151">hello manifestfilen relativ sökväg på hello hårddisken</span><span class="sxs-lookup"><span data-stu-id="e364e-151">hello manifest file relative path on hello hard drive</span></span>

    -   <span data-ttu-id="e364e-152">Hej Base16 kodad manifestfilen MD5-hash</span><span class="sxs-lookup"><span data-stu-id="e364e-152">hello Base16 encoded manifest file MD5 hash</span></span>

## <a name="shipping-your-drives"></a><span data-ttu-id="e364e-153">Leverera dina enheter</span><span class="sxs-lookup"><span data-stu-id="e364e-153">Shipping your drives</span></span>
<span data-ttu-id="e364e-154">Du måste leverera din enheter toohello adress som du fick från hello föregående steg och du måste ange hello Import/Export service med hello spåra antalet hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="e364e-154">You must ship your drives toohello address that you obtained from hello previous step, and you must provide hello Import/Export service with hello tracking number of hello package.</span></span>

> [!NOTE]
>  <span data-ttu-id="e364e-155">Du måste leverera enheter via en stöds operatör-tjänst som tillhandahåller ett spårningsnummer för paketet.</span><span class="sxs-lookup"><span data-stu-id="e364e-155">You must ship your drives via a supported carrier service, which will provide a tracking number for your package.</span></span>

## <a name="updating-hello-import-job-with-your-shipping-information"></a><span data-ttu-id="e364e-156">Uppdaterar hello importjobbet med leveransinformation</span><span class="sxs-lookup"><span data-stu-id="e364e-156">Updating hello import job with your shipping information</span></span>
<span data-ttu-id="e364e-157">När du har Spårningsnumret till din kan anropa hello [uppdatering jobbegenskaper](/api/storageimportexport/jobs#Jobs_Update) åtgärden tooupdate hello leverans operatör namn, hello Spårningsnumret för hello jobbet och hello operatör kontonummer för returnerade leverans.</span><span class="sxs-lookup"><span data-stu-id="e364e-157">After you have your tracking number, call hello [Update Job Properties](/api/storageimportexport/jobs#Jobs_Update) operation tooupdate hello shipping carrier name, hello tracking number for hello job, and hello carrier account number for return shipping.</span></span> <span data-ttu-id="e364e-158">Du kan alternativt ange hello antalet enheter och hello leverans samt datum.</span><span class="sxs-lookup"><span data-stu-id="e364e-158">You can optionally specify hello number of drives and hello shipping date as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e364e-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e364e-159">Next steps</span></span>

* [<span data-ttu-id="e364e-160">Med hjälp av hello Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="e364e-160">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
