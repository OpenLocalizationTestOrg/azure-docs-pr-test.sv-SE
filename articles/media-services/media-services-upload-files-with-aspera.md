---
title: aaaUpload filer till ett Azure Media Services-konto med Aspera | Microsoft Docs
description: "Den här självstudiekursen vägleder dig genom hello stegen för att överföra filer till ett lagringskonto som är kopplat till ett Media Services-konto med hello ** Aspera Server på begäran **-tjänsten på Azure."
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 7213f016cc1b7f262b14db7b39b478a03970d1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-aspera-server-on-demand-service-on-azure"></a><span data-ttu-id="8f7ab-103">Överföra filer till ett Media Services-konto med hello Aspera Server på begäran-tjänsten på Azure</span><span class="sxs-lookup"><span data-stu-id="8f7ab-103">Upload files into a Media Services account using hello Aspera Server On Demand service on Azure</span></span>

## <a name="overview"></a><span data-ttu-id="8f7ab-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="8f7ab-104">Overview</span></span>

<span data-ttu-id="8f7ab-105">**Aspera** är en programvara för filöverföring i hög hastighet.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-105">**Aspera** is a high-speed file transfer software.</span></span> <span data-ttu-id="8f7ab-106">**Aspera Server On Demand** för Azure möjliggör snabba överföringar och hämtning av stora filer direkt i Azure-blobobjektlagringen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-106">**Aspera Server On Demand** for Azure enables high-speed upload and download of large files directly into Azure Blob object storage.</span></span> <span data-ttu-id="8f7ab-107">Information om **Aspera på begäran**, se hello [Aspera moln](http://cloud.asperasoft.com/) plats.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-107">For information about **Aspera On Demand**, see hello [Aspera Cloud](http://cloud.asperasoft.com/) site.</span></span> 
  
<span data-ttu-id="8f7ab-108">**Aspera Server på begäran** för Azure kan köpas från hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-108">**Aspera Server On Demand** for Azure is available for purchase from hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/).</span></span> <span data-ttu-id="8f7ab-109">Ordna toocomplete köp av **Aspera Server på begäran** på Azure, loggar du in Azure Marketplace med ditt Windows Live ID.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-109">In order toocomplete a purchase of **Aspera Server On Demand** for Azure, please log into Azure Marketplace with your Windows Live ID.</span></span>

<span data-ttu-id="8f7ab-110">Den här självstudiekursen vägleder dig genom hello stegen för att överföra filer till ett lagringskonto som är kopplat till ett Media Services-konto med hello **Aspera Server på begäran** på Azure.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-110">This tutorial walks you through hello steps of uploading files into a storage account that is associated with a Media Services account using hello **Aspera Server On Demand** service on Azure.</span></span> 

<span data-ttu-id="8f7ab-111">Du hittar ett exempel som visar hur toouse Azure fungerar med Aspera och Media Services [här](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-111">You can find an example that shows how toouse Azure functions with Aspera and Media Services [here](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span></span>

>[!NOTE]
><span data-ttu-id="8f7ab-112">Det finns en gräns toohello maximal filstorlek som stöds för bearbetning med Azure Media Services-medieprocessorer (MP).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-112">There is a limit toohello maximum file size supported for processing with Azure Media Services media processors (MPs).</span></span> <span data-ttu-id="8f7ab-113">Se [detta](media-services-quotas-and-limitations.md) avsnittet för information om hello filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="8f7ab-114">Krav</span><span class="sxs-lookup"><span data-stu-id="8f7ab-114">Prerequisites</span></span> 

<span data-ttu-id="8f7ab-115">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="8f7ab-115">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="8f7ab-116">Ett Windows Live ID</span><span class="sxs-lookup"><span data-stu-id="8f7ab-116">A Windows Live ID</span></span>
* <span data-ttu-id="8f7ab-117">Ett [Azure-konto](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-117">An [Azure account](https://azure.microsoft.com).</span></span> <span data-ttu-id="8f7ab-118">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-118">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="8f7ab-119">Ett [Azure Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-119">An [Azure Media Services account](media-services-portal-create-account.md).</span></span>

## <a name="purchase-aspera-on-demand-for-azure"></a><span data-ttu-id="8f7ab-120">Köpa Aspera On Demand för Azure</span><span class="sxs-lookup"><span data-stu-id="8f7ab-120">Purchase Aspera On Demand for Azure</span></span>

<span data-ttu-id="8f7ab-121">När du har loggat in på Azure Marketplace, följer du dessa grundläggande steg toocomplete köpet Aspera på begäran för Azure.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-121">Once you have logged into Azure Marketplace,  follow these basic steps toocomplete your purchase of Aspera On Demand for Azure.</span></span>

1. <span data-ttu-id="8f7ab-122">Sök efter Aspera och välj Server On Demand.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-122">Search for Aspera and select 'Server On Demand'.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. <span data-ttu-id="8f7ab-124">Granska hello-prenumerationsavtal och klicka på Registrera dig</span><span class="sxs-lookup"><span data-stu-id="8f7ab-124">Review hello subscription plans and click on 'Sign Up'</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. <span data-ttu-id="8f7ab-126">Fyll i hello programvarukrav för din Server på begäran-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-126">Fill in hello specifics for your Server on Demand subscription.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. <span data-ttu-id="8f7ab-128">Klicka på hello **prisnivån** och välj önskad månatliga volymen hello sub Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-128">Click on hello **Pricing Tier** and select your desired monthly volume in hello sub panel.</span></span> <span data-ttu-id="8f7ab-129">I hello **planera** fönstret väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-129">In hello **Plan details** panel, select **OK**.</span></span> <span data-ttu-id="8f7ab-130">Sedan hello **väljer din prisnivån** klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-130">Then, in hello **Choose your Pricing Tier** panel, click **Select**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. <span data-ttu-id="8f7ab-132">Klicka på **juridiska villkor** tooview och godkänner hello juridiska villkoren hello sub Kontrollpanelen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-132">Click on **Legal terms** tooview and accept hello legal terms in hello sub panel.</span></span> <span data-ttu-id="8f7ab-133">När du har granskat hello juridiska villkor, klickar du på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-133">Once you have reviewed hello legal terms, click **Purchase**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. <span data-ttu-id="8f7ab-135">Fullständigt hello köp genom att klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-135">Complete hello purchase by clicking **Create**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. <span data-ttu-id="8f7ab-137">hello Azure instrumentpanelen kommer att meddela att den etablerar hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-137">hello Azure dashboard will announce that it is provisioning hello service.</span></span>  <span data-ttu-id="8f7ab-138">När den är klar med att etablera hittar hello prenumeration genom att söka i dina resurser för hello namnet på hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-138">Once it is completed with provisioning, you can find hello new subscription by searching in your resources for hello name of hello service.</span></span> <span data-ttu-id="8f7ab-139">När du har hittat hello-tjänsten, dubbel-Klicka på det toolaunch hello service management portal.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-139">Once you have found hello service, double click on it toolaunch hello service management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. <span data-ttu-id="8f7ab-141">Starta hello Aspera-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-141">Launch hello Aspera management portal.</span></span> <span data-ttu-id="8f7ab-142">När du har hittat nya Aspera tjänsten får du åtkomst toohello management portal genom att klicka på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-142">Once you have found your new Aspera service, you can gain access toohello management portal, by clicking on hello service.</span></span>  <span data-ttu-id="8f7ab-143">En ny panel öppnas.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-143">A new panel will be launched.</span></span> <span data-ttu-id="8f7ab-144">Från inom den nya panelen måste tooclick på hello **resursnamnet** av den nya tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-144">From within that new panel, you need tooclick on hello **Resource Name** of your new service.</span></span>  <span data-ttu-id="8f7ab-145">I följande skärmbild hello, är hello resursnamnet 'AsperaTransferDemo'.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-145">In hello following screenshot, hello resource name is 'AsperaTransferDemo'.</span></span> <span data-ttu-id="8f7ab-146">När du klickar på hello resursnamnet startas en annan panel.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-146">Once you click on hello resource name, another panel is launched.</span></span> <span data-ttu-id="8f7ab-147">Länken Manage (Hantera) visas på den nyöppnade panelen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-147">In that newly launched panel, you will see a 'Manage' link.</span></span> <span data-ttu-id="8f7ab-148">Klicka på hello ”hantera” länken toolaunch hello Aspera-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-148">Click on hello 'Manage' link toolaunch hello Aspera management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. <span data-ttu-id="8f7ab-150">Hantera länken genom att klicka på hello, du får toohello registreringssidan, vilket krävs tooaccess hello service.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-150">By clicking on hello manage link, you will get toohello registration page, which is required tooaccess hello service.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. <span data-ttu-id="8f7ab-152">Du bör nu ha åtkomst toohello Aspera service management portal där du kan skapa snabbtangenter, hämta Aspera klienter och licenser, visa syntax och lär dig mer om hello API: er.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-152">At this point, you should have access toohello Aspera service management portal, where you can create access keys, download Aspera clients and licenses, view usage and learn about hello APIs.</span></span>

    <span data-ttu-id="8f7ab-153">hello visar följande skärmbild hello åtkomst skapas.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-153">hello following screenshot shows hello access creation.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    <span data-ttu-id="8f7ab-155">hello visar följande skärmbild hello användning reporting gränssnitt i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-155">hello following screenshot shows hello usage reporting interfaces in hello portal.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a><span data-ttu-id="8f7ab-157">Överföra filer med Aspera</span><span class="sxs-lookup"><span data-stu-id="8f7ab-157">Upload files with Aspera</span></span>

1. <span data-ttu-id="8f7ab-158">Hämta och installera klientprogramvaran för hello Aspera:</span><span class="sxs-lookup"><span data-stu-id="8f7ab-158">Download and install hello Aspera client software:</span></span>
    
    * [<span data-ttu-id="8f7ab-159">Plugin-program för webbläsare</span><span class="sxs-lookup"><span data-stu-id="8f7ab-159">Browser plugin</span></span>](http://downloads.asperasoft.com/connect2/)
    * [<span data-ttu-id="8f7ab-160">Rich-klient</span><span class="sxs-lookup"><span data-stu-id="8f7ab-160">Rich client</span></span>](http://downloads.asperasoft.com/en/downloads/2)

2. <span data-ttu-id="8f7ab-161">Gör din första överföring.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-161">Make your first transfer.</span></span> <span data-ttu-id="8f7ab-162">I ordning toouse hello Aspera klienten tootransfer med hello tjänst för överföring av Aspera behöver du toocomplete hello följande:</span><span class="sxs-lookup"><span data-stu-id="8f7ab-162">In order toouse hello Aspera client tootransfer with hello Aspera transfer service, you need toocomplete hello following:</span></span> 

    1. <span data-ttu-id="8f7ab-163">Skapa en snabbtangent hello Aspera portalen.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-163">Create an access key, using hello Aspera portal.</span></span>  
    2. <span data-ttu-id="8f7ab-164">Hämta och installera licens hello Aspera klienten (programvara finns i hello Aspera portal).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-164">Download, install, and license hello Aspera client (software can be found in hello Aspera portal).</span></span>  

    >[!NOTE]
    ><span data-ttu-id="8f7ab-165">Läs hello Aspera klienten guide för konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-165">Please read hello Aspera client guide for configuration information.</span></span>
    
    3. <span data-ttu-id="8f7ab-166">Hämta viss information för ditt lagringskonto som är kopplat till ditt Azure Media-konto med hjälp av hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-166">Retrieve some information of your storage account that is associated with your Azure Media Account using hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="8f7ab-167">Mer specifikt namn och nyckel, och hello storage blob behållare i toowhich du tooplace ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-167">Specifically, name and key, and hello storage blob container name in toowhich you want tooplace your content.</span></span> 

        * <span data-ttu-id="8f7ab-168">tooget hello lagring information från hello portal: hitta ditt lagringskonto, klickar du på hello snabbtangenter och kopiera hello namn och hello nyckeln för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-168">tooget hello storage info from hello portal: find your storage account, click on hello Access keys and copy hello name and hello key of your account.</span></span>
        * <span data-ttu-id="8f7ab-169">tooget hello behållarnamn: hitta ditt lagringskonto, Välj **Blobbar**väljer hello namnet på hello-behållare som du vill tooupload hello innehåll i.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-169">tooget hello container name: find your storage account, select **Blobs**, select hello name of hello container you want tooupload hello content into.</span></span> 

    <span data-ttu-id="8f7ab-170">Nedan visas hello Skärmbild av hello Aspera klienten **Connection Manager** där du måste ange hello ”Azure” lagringstyp och autentiseringsuppgifter som hello blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-170">Below is hello screenshot of hello Aspera client **Connection Manager** where you must specify hello 'Azure' storage type and credentials as well as hello blob container.</span></span>

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a><span data-ttu-id="8f7ab-172">Resurser</span><span class="sxs-lookup"><span data-stu-id="8f7ab-172">Resources</span></span>

<span data-ttu-id="8f7ab-173">hello följande resurser har nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8f7ab-173">hello following resources were mentioned in this article.</span></span> 

* [<span data-ttu-id="8f7ab-174">Ansluta plugin-program för webbläsare</span><span class="sxs-lookup"><span data-stu-id="8f7ab-174">Connect Browser Plugin</span></span>](http://downloads.asperasoft.com/connect2/)
* [<span data-ttu-id="8f7ab-175">Anslutningsguide</span><span class="sxs-lookup"><span data-stu-id="8f7ab-175">Connect Guide</span></span>](http://downloads.asperasoft.com/en/documentation/8)
* [<span data-ttu-id="8f7ab-176">Aspera-klient</span><span class="sxs-lookup"><span data-stu-id="8f7ab-176">Aspera Client</span></span>](http://downloads.asperasoft.com/en/downloads/2)
* [<span data-ttu-id="8f7ab-177">Klientguide</span><span class="sxs-lookup"><span data-stu-id="8f7ab-177">Client Guide</span></span>](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a><span data-ttu-id="8f7ab-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f7ab-178">Next steps</span></span>

<span data-ttu-id="8f7ab-179">Du kan nu [kopiera blobar från ett lagringskonto till ett AMS-konto](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span><span class="sxs-lookup"><span data-stu-id="8f7ab-179">You can now [copy blobs from a storage account into an AMS account ](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="8f7ab-180">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="8f7ab-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="8f7ab-181">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="8f7ab-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

