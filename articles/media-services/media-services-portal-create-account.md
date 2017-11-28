---
title: Skapa ett Azure Media Services-konto med Azure-portalen | Microsoft Docs
description: "Den här självstudien går igenom hur du skapar ett Azure Media Services-konto med Azure-portalen."
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
ms.openlocfilehash: 919a0b2f390bab353d24423d7f216e7031581aba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a><span data-ttu-id="121ec-103">Skapa ett Media Services-konto med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="121ec-103">Create an Azure Media Services account using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="121ec-104">Portalen</span><span class="sxs-lookup"><span data-stu-id="121ec-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="121ec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="121ec-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="121ec-106">REST</span><span class="sxs-lookup"><span data-stu-id="121ec-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="121ec-107">Du behöver ett Azure-konto för att slutföra den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="121ec-107">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="121ec-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="121ec-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="121ec-109">Azure-portalen är ett sätt att snabbt skapa ett Azure Media Services-konto (AMS).</span><span class="sxs-lookup"><span data-stu-id="121ec-109">The Azure portal provides a way to quickly create an Azure Media Services (AMS) account.</span></span> <span data-ttu-id="121ec-110">Du kan använda ditt konto för att få åtkomst till Media Services för att lagra, kryptera, koda, hantera och strömma medieinnehåll i Azure.</span><span class="sxs-lookup"><span data-stu-id="121ec-110">You can use your account to access Media Services that enable you to store, encrypt, encode, manage, and stream media content in Azure.</span></span> <span data-ttu-id="121ec-111">När du skapar ett Media Services-konto skapar du också ett associerat lagringskonto (eller använder ett befintligt) i samma geografiska område som Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="121ec-111">At the time you create a Media Services account, you also create an associated storage account (or use an existing one) in the same geographic region as the Media Services account.</span></span>

<span data-ttu-id="121ec-112">Den här artikeln beskriver några vanliga begrepp och visar hur du skapar ett Media Services-konto med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="121ec-112">This article explains some common concepts and shows how to create a Media Services account with the Azure portal.</span></span>

## <a name="concepts"></a><span data-ttu-id="121ec-113">Koncept</span><span class="sxs-lookup"><span data-stu-id="121ec-113">Concepts</span></span>
<span data-ttu-id="121ec-114">Åtkomst till Media Services kräver två associerade konton:</span><span class="sxs-lookup"><span data-stu-id="121ec-114">Accessing Media Services requires two associated accounts:</span></span>

* <span data-ttu-id="121ec-115">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="121ec-115">A Media Services account.</span></span> <span data-ttu-id="121ec-116">Med ditt konto får du åtkomst till en uppsättning molnbaserade Media Services-tjänster som är tillgängliga i Azure.</span><span class="sxs-lookup"><span data-stu-id="121ec-116">Your account gives you access to a set of cloud-based Media Services that are available in Azure.</span></span> <span data-ttu-id="121ec-117">På ett Media Services-konto lagras inget faktiskt medieinnehåll.</span><span class="sxs-lookup"><span data-stu-id="121ec-117">A Media Services account does not store actual media content.</span></span> <span data-ttu-id="121ec-118">I stället lagras metadata om medieinnehåll och mediebearbetningsjobb på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="121ec-118">Instead it stores metadata about the media content and media processing jobs in your account.</span></span> <span data-ttu-id="121ec-119">När du skapar kontot väljer du en tillgänglig Media Services-region.</span><span class="sxs-lookup"><span data-stu-id="121ec-119">At the time you create the account, you select an available Media Services region.</span></span> <span data-ttu-id="121ec-120">Den regionen som du väljer är ett datacenter som lagrar metadataposterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="121ec-120">The region you select is a data center that stores the metadata records for your account.</span></span>
  
* <span data-ttu-id="121ec-121">Ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="121ec-121">An Azure storage account.</span></span> <span data-ttu-id="121ec-122">Lagringskonton måste finnas i samma geografiska område som Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="121ec-122">Storage accounts must be located in the same geographic region as the Media Services account.</span></span> <span data-ttu-id="121ec-123">När du skapar ett Media Services-konto kan du antingen välja ett befintligt lagringskonto i samma region eller skapa ett nytt lagringskonto i samma region.</span><span class="sxs-lookup"><span data-stu-id="121ec-123">When you create a Media Services account, you can either choose an existing storage account in the same region, or you can create a new storage account in the same region.</span></span> <span data-ttu-id="121ec-124">Om du tar bort ett Media Services-konto raderas inte blobbarna på ditt relaterade lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="121ec-124">If you delete a Media Services account, the blobs in your related storage account are not deleted.</span></span>

> [!NOTE]
> <span data-ttu-id="121ec-125">Information om tillgänglighet för Azure Media Services-funktioner i olika regioner finns i [tillgänglighet för AMS-funktioner i datacenter](scenarios-and-availability.md#availability).</span><span class="sxs-lookup"><span data-stu-id="121ec-125">For information about availability of Azure Media Services features in different regions, see [availability of AMS features across datacenters](scenarios-and-availability.md#availability).</span></span>

## <a name="create-an-ams-account"></a><span data-ttu-id="121ec-126">Skapa ett AMS-konto</span><span class="sxs-lookup"><span data-stu-id="121ec-126">Create an AMS account</span></span>
<span data-ttu-id="121ec-127">Stegen i det här avsnittet visar hur du skapar ett AMS-konto.</span><span class="sxs-lookup"><span data-stu-id="121ec-127">The steps in this section show how to create an AMS account.</span></span>

1. <span data-ttu-id="121ec-128">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="121ec-128">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="121ec-129">Klicka på **+Ny** > **Webb + mobilt** > **Media Services**.</span><span class="sxs-lookup"><span data-stu-id="121ec-129">Click **+New** > **Web + Mobile** > **Media Services**.</span></span>
   
    ![Skapa Media Services](./media/media-services-create-account/media-services-new1.png)
3. <span data-ttu-id="121ec-131">Ange de erfordrade värdena i **SKAPA MEDIA SERVICES-KONTO**.</span><span class="sxs-lookup"><span data-stu-id="121ec-131">In **CREATE MEDIA SERVICES ACCOUNT** enter required values.</span></span>
   
    ![Skapa Media Services](./media/media-services-create-account/media-services-new3.png)
   
   1. <span data-ttu-id="121ec-133">Ange namnet på det nya AMS-kontot vid **Kontonamn**.</span><span class="sxs-lookup"><span data-stu-id="121ec-133">In **Account Name**, enter the name of the new AMS account.</span></span> <span data-ttu-id="121ec-134">Namnet på ett Media Services-konto består av gemena bokstäver eller siffror utan blanksteg och 3 till 24 tecken.</span><span class="sxs-lookup"><span data-stu-id="121ec-134">A Media Services account name is all lowercase letters or numbers with no spaces, and is 3 to 24 characters in length.</span></span>
   2. <span data-ttu-id="121ec-135">Vid Prenumeration väljer du mellan de olika Azure-prenumerationer som du har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="121ec-135">In Subscription, select among the different Azure subscriptions that you have access to.</span></span>
   3. <span data-ttu-id="121ec-136">I **Resursgrupp** väljer du ny eller befintlig resurs.</span><span class="sxs-lookup"><span data-stu-id="121ec-136">In **Resource Group**, select the new or existing resource.</span></span>  <span data-ttu-id="121ec-137">En resursgrupp är en samling resurser som delar livscykel, behörigheter och principer.</span><span class="sxs-lookup"><span data-stu-id="121ec-137">A resource group is a collection of resources that share lifecycle, permissions, and policies.</span></span> <span data-ttu-id="121ec-138">Lär dig mer [här](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="121ec-138">Learn more [here](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
   4. <span data-ttu-id="121ec-139">För **Plats** väljer du den geografiska region som ska användas för att lagra media och metadataposter för ditt Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="121ec-139">In **Location**,  select the geographic region that will be used to store the media and metadata records for your Media Services account.</span></span> <span data-ttu-id="121ec-140">Den här regionen används för att bearbeta och strömma dina media.</span><span class="sxs-lookup"><span data-stu-id="121ec-140">This  region will be used to process and stream your media.</span></span> <span data-ttu-id="121ec-141">Endast de tillgängliga Media Services-regionerna visas i listrutan.</span><span class="sxs-lookup"><span data-stu-id="121ec-141">Only the available Media Services regions appear in the drop-down list box.</span></span> 
   5. <span data-ttu-id="121ec-142">Vid **Storage-konto** väljer du ett lagringskonto för att tillhandahålla Blob Storage av medieinnehållet från ditt Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="121ec-142">In **Storage Account**, select a storage account to provide blob storage of the media content from your Media Services account.</span></span> <span data-ttu-id="121ec-143">Du kan välja ett befintligt lagringskonto i samma geografiska region som ditt Media Services-konto eller skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="121ec-143">You can select an existing storage account in the same geographic region as your Media Services account, or you can create a storage account.</span></span> <span data-ttu-id="121ec-144">Ett nytt lagringskonto skapas i samma region.</span><span class="sxs-lookup"><span data-stu-id="121ec-144">A new storage account is created in the same region.</span></span> <span data-ttu-id="121ec-145">Reglerna för namn på lagringskonton är desamma som för Media Services-konton.</span><span class="sxs-lookup"><span data-stu-id="121ec-145">The rules for storage account names are the same as for Media Services accounts.</span></span>
      
       <span data-ttu-id="121ec-146">Mer information om lagring finns [här](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="121ec-146">Learn more about storage [here](../storage/common/storage-introduction.md).</span></span>
   6. <span data-ttu-id="121ec-147">Välj **PIN-kod för instrumentpanelen** för att se förloppet för kontodistributionen.</span><span class="sxs-lookup"><span data-stu-id="121ec-147">Select **Pin to dashboard** to see the progress of the account deployment.</span></span>
4. <span data-ttu-id="121ec-148">Klicka på **Skapa** längst ned i formuläret.</span><span class="sxs-lookup"><span data-stu-id="121ec-148">Click **Create** at the bottom of the form.</span></span>
   
    <span data-ttu-id="121ec-149">När kontot har skapats läses översiktssidan in.</span><span class="sxs-lookup"><span data-stu-id="121ec-149">Once the account is successfully created, overview page loads.</span></span> <span data-ttu-id="121ec-150">I tabellen med slutpunkter för direktuppspelning har kontot en standardslutpunkt för direktuppspelning med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="121ec-150">In the streaming endpoint table the account will have a default streaming endpoint in the **Stopped** state.</span></span> 

    >[!NOTE]
    ><span data-ttu-id="121ec-151">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="121ec-151">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="121ec-152">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning som du vill spela upp innehåll från ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="121ec-152">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
   
## <a name="to-manage-your-ams-account"></a><span data-ttu-id="121ec-153">Hantera AMS-kontot</span><span class="sxs-lookup"><span data-stu-id="121ec-153">To manage your AMS account</span></span>

<span data-ttu-id="121ec-154">Om du vill hantera ditt AMS-konto (till exempel ansluta till AMS-API:et via programmering, överföra videoklipp, koda tillgångar, konfigurera innehållsskydd och övervaka jobbförlopp) väljer du **Inställningar** på vänster sida av portalen.</span><span class="sxs-lookup"><span data-stu-id="121ec-154">To manage your AMS account (for example, connect to the AMS API programmatically, upload videos, encode assets, configure content protection, monitor job progress) select **Settings** on the left side of the portal.</span></span> <span data-ttu-id="121ec-155">I **Inställningar** navigerar du till något av de tillgängliga bladen (till exempel **API-åtkomst**, **Tillgångar**, **Jobb** eller **Innehållsskydd**).</span><span class="sxs-lookup"><span data-stu-id="121ec-155">From the **Settings**, navigate to one of the available blades (for example: **API access**, **Assets**, **Jobs**, **Content protection**).</span></span>


## <a name="next-steps"></a><span data-ttu-id="121ec-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="121ec-156">Next steps</span></span>

<span data-ttu-id="121ec-157">Du kan nu överföra filer till AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="121ec-157">You can now upload files into your AMS account.</span></span> <span data-ttu-id="121ec-158">Mer information finns i [Överföra filer](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="121ec-158">For more information, see [Upload files](media-services-portal-upload-files.md).</span></span>

<span data-ttu-id="121ec-159">Om du tänker ansluta till AMS-API:et via programmering kan du läsa [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md) (Ansluta API:et för Azure Media Services med Azure AD-autentisering).</span><span class="sxs-lookup"><span data-stu-id="121ec-159">If you plan to access AMS API programmatically, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="121ec-160">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="121ec-160">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="121ec-161">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="121ec-161">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

