---
title: aaaCreate ett Azure Media Services-konto med hello Azure-portalen | Microsoft Docs
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa ett Azure Media Services-konto med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: juliako
ms.openlocfilehash: fdad93d5d470fc08380670ec0f6c2d33468b1492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-media-services-account-using-hello-azure-portal"></a><span data-ttu-id="b962b-103">Skapa ett Azure Media Services-konto med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b962b-103">Create an Azure Media Services account using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b962b-104">Portal</span><span class="sxs-lookup"><span data-stu-id="b962b-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="b962b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b962b-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="b962b-106">REST</span><span class="sxs-lookup"><span data-stu-id="b962b-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="b962b-107">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="b962b-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b962b-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="b962b-109">hello Azure-portalen kan tooquickly skapa ett Azure Media Services (AMS)-konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-109">hello Azure portal provides a way tooquickly create an Azure Media Services (AMS) account.</span></span> <span data-ttu-id="b962b-110">Du kan använda ditt konto tooaccess Media Services som aktiverar du toostore, kryptera, koda, hantera och strömma medieinnehåll i Azure.</span><span class="sxs-lookup"><span data-stu-id="b962b-110">You can use your account tooaccess Media Services that enable you toostore, encrypt, encode, manage, and stream media content in Azure.</span></span> <span data-ttu-id="b962b-111">Samtidigt hello du skapar ett Media Services-konto du också skapa ett associerat lagringskonto (eller använda ett befintligt) i hello samma geografiska region som hello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-111">At hello time you create a Media Services account, you also create an associated storage account (or use an existing one) in hello same geographic region as hello Media Services account.</span></span>

<span data-ttu-id="b962b-112">Den här artikeln beskriver några vanliga begrepp och visar hur toocreate ett Media Services-konto med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b962b-112">This article explains some common concepts and shows how toocreate a Media Services account with hello Azure portal.</span></span>

## <a name="concepts"></a><span data-ttu-id="b962b-113">Koncept</span><span class="sxs-lookup"><span data-stu-id="b962b-113">Concepts</span></span>
<span data-ttu-id="b962b-114">Åtkomst till Media Services kräver två associerade konton:</span><span class="sxs-lookup"><span data-stu-id="b962b-114">Accessing Media Services requires two associated accounts:</span></span>

* <span data-ttu-id="b962b-115">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-115">A Media Services account.</span></span> <span data-ttu-id="b962b-116">De ger dig åtkomst tooa uppsättning molnbaserade Media Services som är tillgängliga i Azure.</span><span class="sxs-lookup"><span data-stu-id="b962b-116">Your account gives you access tooa set of cloud-based Media Services that are available in Azure.</span></span> <span data-ttu-id="b962b-117">På ett Media Services-konto lagras inget faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="b962b-117">A Media Services account does not store actual media content.</span></span> <span data-ttu-id="b962b-118">I stället lagras metadata om hello medieinnehåll och mediebearbetningsjobb på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-118">Instead it stores metadata about hello media content and media processing jobs in your account.</span></span> <span data-ttu-id="b962b-119">För närvarande hello du skapar hello konto, Välj en tillgänglig Media Services-region.</span><span class="sxs-lookup"><span data-stu-id="b962b-119">At hello time you create hello account, you select an available Media Services region.</span></span> <span data-ttu-id="b962b-120">hello-region som du väljer är ett datacenter som lagrar hello metadataposterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-120">hello region you select is a data center that stores hello metadata records for your account.</span></span>
  
* <span data-ttu-id="b962b-121">Ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b962b-121">An Azure storage account.</span></span> <span data-ttu-id="b962b-122">Storage-konton måste finnas i hello samma geografiska region som hello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-122">Storage accounts must be located in hello same geographic region as hello Media Services account.</span></span> <span data-ttu-id="b962b-123">När du skapar ett Media Services-konto kan du antingen välja ett befintligt lagringskonto i hello samma region eller skapa ett nytt lagringskonto i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="b962b-123">When you create a Media Services account, you can either choose an existing storage account in hello same region, or you can create a new storage account in hello same region.</span></span> <span data-ttu-id="b962b-124">Om du tar bort ett Media Services-konto raderas inte hello blobbar på ditt relaterade lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b962b-124">If you delete a Media Services account, hello blobs in your related storage account are not deleted.</span></span>

> [!NOTE]
> <span data-ttu-id="b962b-125">Information om tillgänglighet för Azure Media Services-funktioner i olika regioner finns i [tillgänglighet för AMS-funktioner i datacenter](scenarios-and-availability.md#availability).</span><span class="sxs-lookup"><span data-stu-id="b962b-125">For information about availability of Azure Media Services features in different regions, see [availability of AMS features across datacenters](scenarios-and-availability.md#availability).</span></span>

## <a name="create-an-ams-account"></a><span data-ttu-id="b962b-126">Skapa ett AMS-konto</span><span class="sxs-lookup"><span data-stu-id="b962b-126">Create an AMS account</span></span>
<span data-ttu-id="b962b-127">hello stegen i det här avsnittet visar hur toocreate en AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="b962b-127">hello steps in this section show how toocreate an AMS account.</span></span>

1. <span data-ttu-id="b962b-128">Logga in på hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b962b-128">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b962b-129">Klicka på **+Ny** > **Webb + mobilt** > **Media Services**.</span><span class="sxs-lookup"><span data-stu-id="b962b-129">Click **+New** > **Web + Mobile** > **Media Services**.</span></span>
   
    ![Skapa Media Services](./media/media-services-create-account/media-services-new1.png)
3. <span data-ttu-id="b962b-131">Ange de erfordrade värdena i **SKAPA MEDIA SERVICES-KONTO**.</span><span class="sxs-lookup"><span data-stu-id="b962b-131">In **CREATE MEDIA SERVICES ACCOUNT** enter required values.</span></span>
   
    ![Skapa Media Services](./media/media-services-create-account/media-services-new3.png)
   
   1. <span data-ttu-id="b962b-133">I **kontonamn**, ange hello namnet på hello nya AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="b962b-133">In **Account Name**, enter hello name of hello new AMS account.</span></span> <span data-ttu-id="b962b-134">Namnet på ett Media Services är alla gemena bokstäver eller siffror utan blanksteg och 3 too24 tecken.</span><span class="sxs-lookup"><span data-stu-id="b962b-134">A Media Services account name is all lowercase letters or numbers with no spaces, and is 3 too24 characters in length.</span></span>
   2. <span data-ttu-id="b962b-135">Vid prenumeration väljer du bland hello olika Azure-prenumerationer som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="b962b-135">In Subscription, select among hello different Azure subscriptions that you have access to.</span></span>
   3. <span data-ttu-id="b962b-136">I **resursgruppen**, Välj hello ny eller befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="b962b-136">In **Resource Group**, select hello new or existing resource.</span></span>  <span data-ttu-id="b962b-137">En resursgrupp är en samling resurser som delar livscykel, behörigheter och principer.</span><span class="sxs-lookup"><span data-stu-id="b962b-137">A resource group is a collection of resources that share lifecycle, permissions, and policies.</span></span> <span data-ttu-id="b962b-138">Lär dig mer [här](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="b962b-138">Learn more [here](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
   4. <span data-ttu-id="b962b-139">I **plats**, Välj hello geografiska region som kommer att använda toostore hello media och metadataposter poster för Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="b962b-139">In **Location**,  select hello geographic region that will be used toostore hello media and metadata records for your Media Services account.</span></span> <span data-ttu-id="b962b-140">Den här regionen kommer att använda tooprocess och strömma dina media.</span><span class="sxs-lookup"><span data-stu-id="b962b-140">This  region will be used tooprocess and stream your media.</span></span> <span data-ttu-id="b962b-141">Endast hello tillgängliga Media Services-regionerna visas i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="b962b-141">Only hello available Media Services regions appear in hello drop-down list box.</span></span> 
   5. <span data-ttu-id="b962b-142">I **Lagringskonto**, Välj en storage-konto tooprovide blob storage för hello medieinnehållet från ditt Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="b962b-142">In **Storage Account**, select a storage account tooprovide blob storage of hello media content from your Media Services account.</span></span> <span data-ttu-id="b962b-143">Du kan välja ett befintligt lagringskonto i hello samma geografiska region som ditt Media Services-konto, eller om du kan skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b962b-143">You can select an existing storage account in hello same geographic region as your Media Services account, or you can create a storage account.</span></span> <span data-ttu-id="b962b-144">Ett nytt lagringskonto skapas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="b962b-144">A new storage account is created in hello same region.</span></span> <span data-ttu-id="b962b-145">hello regler för lagringskontot namn är hello samma som för Media Services-konton.</span><span class="sxs-lookup"><span data-stu-id="b962b-145">hello rules for storage account names are hello same as for Media Services accounts.</span></span>
      
       <span data-ttu-id="b962b-146">Mer information om lagring finns [här](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b962b-146">Learn more about storage [here](../storage/common/storage-introduction.md).</span></span>
   6. <span data-ttu-id="b962b-147">Välj **PIN-kod toodashboard** toosee hello förloppet för kontodistributionen hello.</span><span class="sxs-lookup"><span data-stu-id="b962b-147">Select **Pin toodashboard** toosee hello progress of hello account deployment.</span></span>
4. <span data-ttu-id="b962b-148">Klicka på **skapa** längst hello hello formuläret.</span><span class="sxs-lookup"><span data-stu-id="b962b-148">Click **Create** at hello bottom of hello form.</span></span>
   
    <span data-ttu-id="b962b-149">När hello kontot har skapats, översikt översiktssidan läses in.</span><span class="sxs-lookup"><span data-stu-id="b962b-149">Once hello account is successfully created, overview page loads.</span></span> <span data-ttu-id="b962b-150">I hello strömmande slutpunkten tabell hello konto har ett standardvärde strömmande slutpunkten i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b962b-150">In hello streaming endpoint table hello account will have a default streaming endpoint in hello **Stopped** state.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="b962b-151">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b962b-151">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="b962b-152">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b962b-152">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
   
## <a name="toomanage-your-ams-account"></a><span data-ttu-id="b962b-153">toomanage AMS-kontot</span><span class="sxs-lookup"><span data-stu-id="b962b-153">toomanage your AMS account</span></span>

<span data-ttu-id="b962b-154">toomanage AMS-kontot (till exempel ansluta toohello AMS API programmässigt, överföra videor, koda tillgångar, konfigurera innehållsskydd, övervaka jobbförlopp) Välj **inställningar** på vänster sida av hello portal hello.</span><span class="sxs-lookup"><span data-stu-id="b962b-154">toomanage your AMS account (for example, connect toohello AMS API programmatically, upload videos, encode assets, configure content protection, monitor job progress) select **Settings** on hello left side of hello portal.</span></span> <span data-ttu-id="b962b-155">Från hello **inställningar**, navigera tooone hello tillgängliga blad (till exempel: **API-åtkomst**, **tillgångar**, **jobb**, **Content protection**).</span><span class="sxs-lookup"><span data-stu-id="b962b-155">From hello **Settings**, navigate tooone of hello available blades (for example: **API access**, **Assets**, **Jobs**, **Content protection**).</span></span>


## <a name="next-steps"></a><span data-ttu-id="b962b-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b962b-156">Next steps</span></span>

<span data-ttu-id="b962b-157">Du kan nu överföra filer till AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="b962b-157">You can now upload files into your AMS account.</span></span> <span data-ttu-id="b962b-158">Mer information finns i [Överföra filer](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="b962b-158">For more information, see [Upload files](media-services-portal-upload-files.md).</span></span>

<span data-ttu-id="b962b-159">Om du planerar tooaccess AMS API programmässigt Se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="b962b-159">If you plan tooaccess AMS API programmatically, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="b962b-160">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="b962b-160">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b962b-161">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="b962b-161">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

