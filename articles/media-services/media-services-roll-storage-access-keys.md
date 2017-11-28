---
title: "Uppdatera Media Services när du återställer åtkomstnycklar för lagring | Microsoft Docs"
description: "Den här artikeln får du vägledning om hur du uppdaterar Media Services efter rullande åtkomstnycklar för lagring."
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
ms.openlocfilehash: 304e72e0d2d4a7e95df513e6d5481def9eae3f68
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a><span data-ttu-id="e1d3d-103">Uppdatera Media Services när du återställer åtkomstnycklar för lagring</span><span class="sxs-lookup"><span data-stu-id="e1d3d-103">Update Media Services after rolling storage access keys</span></span>

<span data-ttu-id="e1d3d-104">Du uppmanas också att välja ett Azure Storage-konto som används för att lagra media innehållet när du skapar ett nytt konto i Azure Media Services (AMS).</span><span class="sxs-lookup"><span data-stu-id="e1d3d-104">When you create a new Azure Media Services (AMS) account, you are also asked to select an Azure Storage account that is used to store your media content.</span></span> <span data-ttu-id="e1d3d-105">Du kan lägga till flera lagringskonton Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-105">You can add more than one storage accounts to your Media Services account.</span></span> <span data-ttu-id="e1d3d-106">Det här avsnittet visar hur du rotera lagringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-106">This topic shows how to rotate storage keys.</span></span> <span data-ttu-id="e1d3d-107">Den visar även hur du lägger till storage-konton till ett konto för media.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-107">It also shows how to add storage accounts to a media account.</span></span> 

<span data-ttu-id="e1d3d-108">Om du vill utföra de åtgärder som beskrivs i det här avsnittet bör du använda [ARM-API: er](https://docs.microsoft.com/rest/api/media/mediaservice) och [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span><span class="sxs-lookup"><span data-stu-id="e1d3d-108">To perform the actions described in this topic, you should be using [ARM APIs](https://docs.microsoft.com/rest/api/media/mediaservice) and [Powershell](https://docs.microsoft.com/powershell/resourcemanager/azurerm.media/v0.3.2/azurerm.media).</span></span>  <span data-ttu-id="e1d3d-109">Mer information finns i [hantera Azure-resurser med PowerShell och Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="e1d3d-109">For more information, see [How to manage Azure resources with PowerShell and Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

## <a name="overview"></a><span data-ttu-id="e1d3d-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="e1d3d-110">Overview</span></span>

<span data-ttu-id="e1d3d-111">När ett nytt lagringskonto har skapats, genererar Azure två 512-bitars åtkomstnycklar för lagring, som används för att autentisera åtkomst till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-111">When a new storage account is created, Azure generates two 512-bit storage access keys, which are used to authenticate access to your storage account.</span></span> <span data-ttu-id="e1d3d-112">För att skydda din lagringsanslutningar, rekommenderas att återskapa och rotera din lagringsåtkomstnyckel med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-112">To keep your storage connections more secure, it is recommended to periodically regenerate and rotate your storage access key.</span></span> <span data-ttu-id="e1d3d-113">Två åtkomstnycklar (primära och sekundära) har angetts för att aktivera att upprätthålla anslutningar till lagringskontot som den ena åtkomstnyckeln medan du återskapar den andra åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-113">Two access keys (primary and secondary) are provided in order to enable you to maintain connections to the storage account using one access key while you regenerate the other access key.</span></span> <span data-ttu-id="e1d3d-114">Den här proceduren är en förkortning av ”rullande snabbtangenter”.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-114">This procedure is also called "rolling access keys".</span></span>

<span data-ttu-id="e1d3d-115">Media Services är beroende av en nyckel för säkerhetslagring angivna.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-115">Media Services depends on a storage key provided to it.</span></span> <span data-ttu-id="e1d3d-116">Mer specifikt beror positionerare som används för att strömma eller hämta dina tillgångar på den angivna lagringsåtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-116">Specifically, the locators that are used to stream or download your assets depend on the specified storage access key.</span></span> <span data-ttu-id="e1d3d-117">När en AMS-kontot skapas det tar ett beroende på den primära lagringsåtkomstnyckel som standard men som en användare kan du uppdatera lagringsnyckeln som AMS har.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-117">When an AMS account is created it takes a dependency on the primary storage access key by default but as a user you can update the storage key that AMS has.</span></span> <span data-ttu-id="e1d3d-118">Du måste se till att låta Media Services vet vilken nyckel ska användas genom att följa stegen som beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-118">You must make sure to let Media Services know which key to use by following steps described in this topic.</span></span>  

>[!NOTE]
> <span data-ttu-id="e1d3d-119">Om du har flera lagringskonton, utför den här proceduren med varje lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="e1d3d-119">If you have multiple storage accounts, you would perform this procedure with each storage account.</span></span> <span data-ttu-id="e1d3d-120">Den ordning i vilken du roterar lagringsnycklar är inte fast.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-120">The order in which you rotate storage keys is not fixed.</span></span> <span data-ttu-id="e1d3d-121">Du kan rotera den sekundära nyckeln först och den primära nyckeln eller tvärtom.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-121">You can rotate the secondary key first and then the primary key or vice versa.</span></span>
>
> <span data-ttu-id="e1d3d-122">Se till att testa dem på ett konto i Förproduktion innan du kör stegen som beskrivs i det här avsnittet på ett Produktionskonto.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-122">Before executing steps described in this topic on a production account, make sure to test them on a pre-production account.</span></span>
>

## <a name="steps-to-rotate-storage-keys"></a><span data-ttu-id="e1d3d-123">Steg för att rotera lagringsnycklar</span><span class="sxs-lookup"><span data-stu-id="e1d3d-123">Steps to rotate storage keys</span></span> 
 
 1. <span data-ttu-id="e1d3d-124">Ändra lagringskontots primära åtkomstnyckel via powershell-cmdleten eller [Azure](https://portal.azure.com/) portal.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-124">Change the storage account Primary key through the powershell cmdlet or [Azure](https://portal.azure.com/) portal.</span></span>
 2. <span data-ttu-id="e1d3d-125">Anropa synkronisering AzureRmMediaServiceStorageKeys cmdleten med lämpliga parametrar för att tvinga media-konto för att hämta nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="e1d3d-125">Call Sync-AzureRmMediaServiceStorageKeys cmdlet with appropriate params to force media account to pick up storage account keys</span></span>
 
    <span data-ttu-id="e1d3d-126">I följande exempel visas hur du synkroniserar nycklar till storage-konton.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-126">The following example shows how to sync keys to storage accounts.</span></span>
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. <span data-ttu-id="e1d3d-127">Vänta en timme.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-127">Wait an hour or so.</span></span> <span data-ttu-id="e1d3d-128">Kontrollera strömmande scenarier fungerar.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-128">Verify the streaming scenarios are working.</span></span>
 4. <span data-ttu-id="e1d3d-129">Ändra lagringskontots sekundära åtkomstnyckel via powershell-cmdlet eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-129">Change storage account secondary key through the powershell cmdlet or Azure portal.</span></span>
 5. <span data-ttu-id="e1d3d-130">Anropa synkronisering AzureRmMediaServiceStorageKeys powershell med lämpliga parametrar för att tvinga media-konto för att hämta nya lagringskontonycklarna genereras.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-130">Call Sync-AzureRmMediaServiceStorageKeys powershell with appropriate params to force media account to pick up new storage account keys.</span></span> 
 6. <span data-ttu-id="e1d3d-131">Vänta en timme.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-131">Wait an hour or so.</span></span> <span data-ttu-id="e1d3d-132">Kontrollera strömmande scenarier fungerar.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-132">Verify the streaming scenarios are working.</span></span>
 
### <a name="a-powershell-cmdlet-example"></a><span data-ttu-id="e1d3d-133">Ett powershell-cmdlet-exempel</span><span class="sxs-lookup"><span data-stu-id="e1d3d-133">A powershell cmdlet example</span></span> 

<span data-ttu-id="e1d3d-134">Exemplet nedan visar hur du hämtar storage-konto och synkroniseras med AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-134">The following example demonstrates how to get the storage account and sync it with the AMS account.</span></span>

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a><span data-ttu-id="e1d3d-135">Steg för att lägga till storage-konton i AMS-kontot</span><span class="sxs-lookup"><span data-stu-id="e1d3d-135">Steps to add storage accounts to your AMS account</span></span>

<span data-ttu-id="e1d3d-136">Följande avsnitt beskriver hur du lägger till storage-konton AMS-kontot: [koppla flera lagringskonton till ett Media Services-konto](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="e1d3d-136">The following topic shows how to add storage accounts to your AMS account: [Attach multiple storage accounts to a Media Services account](meda-services-managing-multiple-storage-accounts.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="e1d3d-137">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="e1d3d-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e1d3d-138">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="e1d3d-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="e1d3d-139">Bekräftelser</span><span class="sxs-lookup"><span data-stu-id="e1d3d-139">Acknowledgments</span></span>
<span data-ttu-id="e1d3d-140">Vi vill bekräfta följande personer som har bidragit till att skapa det här dokumentet: Cenk Dingiloglu, Milano Gada Seva Titov.</span><span class="sxs-lookup"><span data-stu-id="e1d3d-140">We would like to acknowledge the following people who contributed towards creating this document: Cenk Dingiloglu, Milan Gada, Seva Titov.</span></span>
