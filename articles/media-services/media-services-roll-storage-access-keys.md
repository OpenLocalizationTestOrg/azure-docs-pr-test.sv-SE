---
title: "aaaUpdate Media Services efter rullande lagring åtkomstnycklar | Microsoft Docs"
description: "Den här artikeln får du vägledning om hur tooupdate Media Services efter rullande lagring åtkomstnycklar."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 26fa7a75a73397842aaebda59516a00f68ab97f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a><span data-ttu-id="821c0-103">Uppdatera Media Services när du återställer åtkomstnycklar för lagring</span><span class="sxs-lookup"><span data-stu-id="821c0-103">Update Media Services after rolling storage access keys</span></span>

<span data-ttu-id="821c0-104">När du skapar ett nytt konto i Azure Media Services (AMS) kan uppmanas du också tooselect ett Azure Storage-konto som används för toostore media-innehåll.</span><span class="sxs-lookup"><span data-stu-id="821c0-104">When you create a new Azure Media Services (AMS) account, you are also asked tooselect an Azure Storage account that is used toostore your media content.</span></span> <span data-ttu-id="821c0-105">Du kan lägga till fler än en storage-konton tooyour Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="821c0-105">You can add more than one storage accounts tooyour Media Services account.</span></span> <span data-ttu-id="821c0-106">Det här avsnittet visar hur toorotate lagringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="821c0-106">This topic shows how toorotate storage keys.</span></span> <span data-ttu-id="821c0-107">Den visar även hur tooadd lagringskonton tooa media-konto.</span><span class="sxs-lookup"><span data-stu-id="821c0-107">It also shows how tooadd storage accounts tooa media account.</span></span> 

<span data-ttu-id="821c0-108">tooperform hello åtgärder som beskrivs i det här avsnittet, bör du använda [ARM-API: er](https://docs.microsoft.com/rest/api/media/mediaservice) och [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span><span class="sxs-lookup"><span data-stu-id="821c0-108">tooperform hello actions described in this topic, you should be using [ARM APIs](https://docs.microsoft.com/rest/api/media/mediaservice) and [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span></span>  <span data-ttu-id="821c0-109">Mer information finns i [hur toomanage Azure resurser med PowerShell och Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="821c0-109">For more information, see [How toomanage Azure resources with PowerShell and Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

## <a name="overview"></a><span data-ttu-id="821c0-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="821c0-110">Overview</span></span>

<span data-ttu-id="821c0-111">När ett nytt lagringskonto har skapats, genererar Azure två 512-bitars åtkomstnycklar för lagring, som används tooauthenticate åtkomst tooyour lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="821c0-111">When a new storage account is created, Azure generates two 512-bit storage access keys, which are used tooauthenticate access tooyour storage account.</span></span> <span data-ttu-id="821c0-112">tookeep lagring anslutningarna säkrare bör tooperiodically återskapa och rotera din lagringsåtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="821c0-112">tookeep your storage connections more secure, it is recommended tooperiodically regenerate and rotate your storage access key.</span></span> <span data-ttu-id="821c0-113">Två åtkomstnycklar (primär eller sekundär) tillhandahålls i ordning tooenable du toomaintain anslutningar toohello storage-konto med en snabbtangent medan du återskapar hello andra snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="821c0-113">Two access keys (primary and secondary) are provided in order tooenable you toomaintain connections toohello storage account using one access key while you regenerate hello other access key.</span></span> <span data-ttu-id="821c0-114">Den här proceduren är en förkortning av ”rullande snabbtangenter”.</span><span class="sxs-lookup"><span data-stu-id="821c0-114">This procedure is also called "rolling access keys".</span></span>

<span data-ttu-id="821c0-115">Media Services är beroende av en nyckel för säkerhetslagring tillhandahålls tooit.</span><span class="sxs-lookup"><span data-stu-id="821c0-115">Media Services depends on a storage key provided tooit.</span></span> <span data-ttu-id="821c0-116">Mer specifikt beroende hello-positionerare som används toostream eller hämta dina tillgångar hello angivna lagringsåtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="821c0-116">Specifically, hello locators that are used toostream or download your assets depend on hello specified storage access key.</span></span> <span data-ttu-id="821c0-117">När en AMS-kontot skapas det tar ett beroende på hello primära lagringsåtkomstnyckel som standard men som en användare kan du uppdatera hello lagringsnyckel AMS.</span><span class="sxs-lookup"><span data-stu-id="821c0-117">When an AMS account is created it takes a dependency on hello primary storage access key by default but as a user you can update hello storage key that AMS has.</span></span> <span data-ttu-id="821c0-118">Du måste kontrollera toolet Media Services veta vilka viktiga toouse genom att följa stegen som beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="821c0-118">You must make sure toolet Media Services know which key toouse by following steps described in this topic.</span></span>  

>[!NOTE]
> <span data-ttu-id="821c0-119">Om du har flera lagringskonton, utför den här proceduren med varje lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="821c0-119">If you have multiple storage accounts, you would perform this procedure with each storage account.</span></span> <span data-ttu-id="821c0-120">hello ordning i vilken du roterar lagringsnycklar är inte fast.</span><span class="sxs-lookup"><span data-stu-id="821c0-120">hello order in which you rotate storage keys is not fixed.</span></span> <span data-ttu-id="821c0-121">Du kan rotera hello sekundärnyckeln först och hello primär nyckel eller tvärtom.</span><span class="sxs-lookup"><span data-stu-id="821c0-121">You can rotate hello secondary key first and then hello primary key or vice versa.</span></span>
>
> <span data-ttu-id="821c0-122">Innan du kör stegen beskrivs i det här avsnittet på ett Produktionskonto för, se till att tootest dem på ett konto för Förproduktion.</span><span class="sxs-lookup"><span data-stu-id="821c0-122">Before executing steps described in this topic on a production account, make sure tootest them on a pre-production account.</span></span>
>

## <a name="steps-toorotate-storage-keys"></a><span data-ttu-id="821c0-123">Steg toorotate lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="821c0-123">Steps toorotate storage keys</span></span> 
 
 1. <span data-ttu-id="821c0-124">Ändra hello primära lagringskontonyckel via hello powershell-cmdleten eller [Azure](https://portal.azure.com/) portal.</span><span class="sxs-lookup"><span data-stu-id="821c0-124">Change hello storage account Primary key through hello powershell cmdlet or [Azure](https://portal.azure.com/) portal.</span></span>
 2. <span data-ttu-id="821c0-125">Anropa synkronisering AzureRmMediaServiceStorageKeys cmdleten med lämpliga parametrar tooforce media konto toopick in lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="821c0-125">Call Sync-AzureRmMediaServiceStorageKeys cmdlet with appropriate params tooforce media account toopick up storage account keys</span></span>
 
    <span data-ttu-id="821c0-126">hello som följande exempel visar hur toosync nycklar toostorage konton.</span><span class="sxs-lookup"><span data-stu-id="821c0-126">hello following example shows how toosync keys toostorage accounts.</span></span>
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. <span data-ttu-id="821c0-127">Vänta en timme.</span><span class="sxs-lookup"><span data-stu-id="821c0-127">Wait an hour or so.</span></span> <span data-ttu-id="821c0-128">Kontrollera hello strömmande scenarier fungerar.</span><span class="sxs-lookup"><span data-stu-id="821c0-128">Verify hello streaming scenarios are working.</span></span>
 4. <span data-ttu-id="821c0-129">Ändra lagringskontots sekundära åtkomstnyckel via hello powershell-cmdlet eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="821c0-129">Change storage account secondary key through hello powershell cmdlet or Azure portal.</span></span>
 5. <span data-ttu-id="821c0-130">Anropa synkronisering AzureRmMediaServiceStorageKeys powershell med lämpliga parametrar tooforce media konto toopick in nya lagringskontonycklarna genereras.</span><span class="sxs-lookup"><span data-stu-id="821c0-130">Call Sync-AzureRmMediaServiceStorageKeys powershell with appropriate params tooforce media account toopick up new storage account keys.</span></span> 
 6. <span data-ttu-id="821c0-131">Vänta en timme.</span><span class="sxs-lookup"><span data-stu-id="821c0-131">Wait an hour or so.</span></span> <span data-ttu-id="821c0-132">Kontrollera hello strömmande scenarier fungerar.</span><span class="sxs-lookup"><span data-stu-id="821c0-132">Verify hello streaming scenarios are working.</span></span>
 
### <a name="a-powershell-cmdlet-example"></a><span data-ttu-id="821c0-133">Ett powershell-cmdlet-exempel</span><span class="sxs-lookup"><span data-stu-id="821c0-133">A powershell cmdlet example</span></span> 

<span data-ttu-id="821c0-134">hello visar exemplet nedan hur tooget hello storage-konto och synkroniseras med hello AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="821c0-134">hello following example demonstrates how tooget hello storage account and sync it with hello AMS account.</span></span>

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-tooadd-storage-accounts-tooyour-ams-account"></a><span data-ttu-id="821c0-135">Steg tooadd lagringskonton tooyour AMS-konto</span><span class="sxs-lookup"><span data-stu-id="821c0-135">Steps tooadd storage accounts tooyour AMS account</span></span>

<span data-ttu-id="821c0-136">hello följande avsnitt visar hur tooadd lagringskonton tooyour AMS-konto: [bifoga flera storage-konton tooa Media Services-konto](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="821c0-136">hello following topic shows how tooadd storage accounts tooyour AMS account: [Attach multiple storage accounts tooa Media Services account](meda-services-managing-multiple-storage-accounts.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="821c0-137">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="821c0-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="821c0-138">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="821c0-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="821c0-139">Bekräftelser</span><span class="sxs-lookup"><span data-stu-id="821c0-139">Acknowledgments</span></span>
<span data-ttu-id="821c0-140">Vi vill gärna tooacknowledge hello följande personer som har bidragit till att skapa det här dokumentet: Cenk Dingiloglu, Milano Gada Seva Titov.</span><span class="sxs-lookup"><span data-stu-id="821c0-140">We would like tooacknowledge hello following people who contributed towards creating this document: Cenk Dingiloglu, Milan Gada, Seva Titov.</span></span>
