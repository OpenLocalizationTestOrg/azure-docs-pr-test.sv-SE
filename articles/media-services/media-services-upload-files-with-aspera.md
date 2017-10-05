---
title: "Överföra filer till ett Azure Media Services-konto med Aspera | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för att överföra filer till ett lagringskonto som är associerad med ett Media Services-konto med den ** Aspera Server på begäran **-tjänsten på Azure."
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
ms.openlocfilehash: e3090da9b2c5b8f99545a1f7f9601bfd8d5221f1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-the-aspera-server-on-demand-service-on-azure"></a><span data-ttu-id="4c6c5-103">Överföra filer till ett Media Services-konto med hjälp av tjänsten Aspera Server On Demand på Azure</span><span class="sxs-lookup"><span data-stu-id="4c6c5-103">Upload files into a Media Services account using the Aspera Server On Demand service on Azure</span></span>

## <a name="overview"></a><span data-ttu-id="4c6c5-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="4c6c5-104">Overview</span></span>

<span data-ttu-id="4c6c5-105">**Aspera** är en programvara för filöverföring i hög hastighet.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-105">**Aspera** is a high-speed file transfer software.</span></span> <span data-ttu-id="4c6c5-106">**Aspera Server On Demand** för Azure möjliggör snabba överföringar och hämtning av stora filer direkt i Azure-blobobjektlagringen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-106">**Aspera Server On Demand** for Azure enables high-speed upload and download of large files directly into Azure Blob object storage.</span></span> <span data-ttu-id="4c6c5-107">Information om **Aspera On Demand** finns på webbplatsen [Aspera Cloud](http://cloud.asperasoft.com/).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-107">For information about **Aspera On Demand**, see the [Aspera Cloud](http://cloud.asperasoft.com/) site.</span></span> 
  
<span data-ttu-id="4c6c5-108">**Aspera Server On Demand** för Azure kan köpas på [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-108">**Aspera Server On Demand** for Azure is available for purchase from the [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/).</span></span> <span data-ttu-id="4c6c5-109">Du måste vara inloggad på Azure Marketplace med ditt Windows Live ID för att kunna köpa **Aspera Server On Demand** för Azure.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-109">In order to complete a purchase of **Aspera Server On Demand** for Azure, please log into Azure Marketplace with your Windows Live ID.</span></span>

<span data-ttu-id="4c6c5-110">Den här kursen visar hur du överför filer till ett lagringskonto som är associerat med ett Media Services-konto med tjänsten **Aspera Server On Demand** på Azure.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-110">This tutorial walks you through the steps of uploading files into a storage account that is associated with a Media Services account using the **Aspera Server On Demand** service on Azure.</span></span> 

<span data-ttu-id="4c6c5-111">Ett exempel som visar hur du använder Azure-funktioner med Aspera och Media Services finns [här](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-111">You can find an example that shows how to use Azure functions with Aspera and Media Services [here](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).</span></span>

>[!NOTE]
><span data-ttu-id="4c6c5-112">Det finns en gräns för maximal filstorlek för bearbetning med Azure Media Services-mediebearbetare (MP:er).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-112">There is a limit to the maximum file size supported for processing with Azure Media Services media processors (MPs).</span></span> <span data-ttu-id="4c6c5-113">Information om filstorleksbegränsningen finns i [det här](media-services-quotas-and-limitations.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about the file size limitation.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="4c6c5-114">Krav</span><span class="sxs-lookup"><span data-stu-id="4c6c5-114">Prerequisites</span></span> 

<span data-ttu-id="4c6c5-115">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="4c6c5-115">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="4c6c5-116">Ett Windows Live ID</span><span class="sxs-lookup"><span data-stu-id="4c6c5-116">A Windows Live ID</span></span>
* <span data-ttu-id="4c6c5-117">Ett [Azure-konto](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-117">An [Azure account](https://azure.microsoft.com).</span></span> <span data-ttu-id="4c6c5-118">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-118">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="4c6c5-119">Ett [Azure Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-119">An [Azure Media Services account](media-services-portal-create-account.md).</span></span>

## <a name="purchase-aspera-on-demand-for-azure"></a><span data-ttu-id="4c6c5-120">Köpa Aspera On Demand för Azure</span><span class="sxs-lookup"><span data-stu-id="4c6c5-120">Purchase Aspera On Demand for Azure</span></span>

<span data-ttu-id="4c6c5-121">När du är inloggad på Azure Marketplace kan du köpa Aspera On Demand för Azure genom att följande dessa enkla steg.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-121">Once you have logged into Azure Marketplace,  follow these basic steps to complete your purchase of Aspera On Demand for Azure.</span></span>

1. <span data-ttu-id="4c6c5-122">Sök efter Aspera och välj Server On Demand.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-122">Search for Aspera and select 'Server On Demand'.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. <span data-ttu-id="4c6c5-124">Granska prenumerationsavtalen och klicka på Sign Up (Registrera dig)</span><span class="sxs-lookup"><span data-stu-id="4c6c5-124">Review the subscription plans and click on 'Sign Up'</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. <span data-ttu-id="4c6c5-126">Fyll i informationen för din för din prenumeration på Server On Demand.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-126">Fill in the specifics for your Server on Demand subscription.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. <span data-ttu-id="4c6c5-128">Klicka på **Pricing Tier** (Prisnivå) och välj önskad månatlig volym på delpanelen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-128">Click on the **Pricing Tier** and select your desired monthly volume in the sub panel.</span></span> <span data-ttu-id="4c6c5-129">Välj **OK** på panelen **Plan details** (Avtalsinformation).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-129">In the **Plan details** panel, select **OK**.</span></span> <span data-ttu-id="4c6c5-130">Klicka sedan på **Select** (Välj) på panelen **Choose your Pricing Tier** (Välj din prisnivå).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-130">Then, in the **Choose your Pricing Tier** panel, click **Select**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. <span data-ttu-id="4c6c5-132">Klicka på **Legal terms** (Juridiska villkor) för att läsa och acceptera de juridiska villkoren på delpanelen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-132">Click on **Legal terms** to view and accept the legal terms in the sub panel.</span></span> <span data-ttu-id="4c6c5-133">När du har granskat de juridiska villkoren klickar du på **Purchase** (Köp).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-133">Once you have reviewed the legal terms, click **Purchase**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. <span data-ttu-id="4c6c5-135">Slutför köpet genom att klicka på **Create** (Skapa).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-135">Complete the purchase by clicking **Create**.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. <span data-ttu-id="4c6c5-137">På Azure-instrumentpanelen anges att tjänsten etableras.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-137">The Azure dashboard will announce that it is provisioning the service.</span></span>  <span data-ttu-id="4c6c5-138">När etableringen är klar kan du se den nya prenumerationen genom att söka efter namnet på tjänsten bland dina resurser.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-138">Once it is completed with provisioning, you can find the new subscription by searching in your resources for the name of the service.</span></span> <span data-ttu-id="4c6c5-139">När du har hittat tjänsten dubbelklickar du på den för att starta hanteringsportalen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-139">Once you have found the service, double click on it to launch the service management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. <span data-ttu-id="4c6c5-141">Starta Aspera-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-141">Launch the Aspera management portal.</span></span> <span data-ttu-id="4c6c5-142">När du har hittat din nya Aspera-tjänst kan du öppna hanteringsportalen genom att klicka på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-142">Once you have found your new Aspera service, you can gain access to the management portal, by clicking on the service.</span></span>  <span data-ttu-id="4c6c5-143">En ny panel öppnas.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-143">A new panel will be launched.</span></span> <span data-ttu-id="4c6c5-144">På den nya panelen klickar du på **resursnamnet** för den nya tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-144">From within that new panel, you need to click on the **Resource Name** of your new service.</span></span>  <span data-ttu-id="4c6c5-145">På följande skärmbild är resursens namn AsperaTransferDemo.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-145">In the following screenshot, the resource name is 'AsperaTransferDemo'.</span></span> <span data-ttu-id="4c6c5-146">När du klickar på resursnamnet öppnas en ny panel.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-146">Once you click on the resource name, another panel is launched.</span></span> <span data-ttu-id="4c6c5-147">Länken Manage (Hantera) visas på den nyöppnade panelen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-147">In that newly launched panel, you will see a 'Manage' link.</span></span> <span data-ttu-id="4c6c5-148">Klicka på länken för att öppna Aspera-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-148">Click on the 'Manage' link to launch the Aspera management portal.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. <span data-ttu-id="4c6c5-150">När du klickar på hanteringslänken kommer du till registreringssidan (registrering krävs för att få åtkomst till tjänsten).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-150">By clicking on the manage link, you will get to the registration page, which is required to access the service.</span></span>

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. <span data-ttu-id="4c6c5-152">Nu ska du ha åtkomst till Aspera-hanteringsportalen där du kan skapa åtkomstnycklar, hämta Aspera-klienter och licenser, visa användning och läsa om API:erna.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-152">At this point, you should have access to the Aspera service management portal, where you can create access keys, download Aspera clients and licenses, view usage and learn about the APIs.</span></span>

    <span data-ttu-id="4c6c5-153">Skärmbilden nedan visar hur åtkomst skapas.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-153">The following screenshot shows the access creation.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    <span data-ttu-id="4c6c5-155">Skärmbilden nedan visar gränssnitt för användningsrapportering i portalen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-155">The following screenshot shows the usage reporting interfaces in the portal.</span></span> 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a><span data-ttu-id="4c6c5-157">Överföra filer med Aspera</span><span class="sxs-lookup"><span data-stu-id="4c6c5-157">Upload files with Aspera</span></span>

1. <span data-ttu-id="4c6c5-158">Hämta och installera Aspera-klientprogrammet:</span><span class="sxs-lookup"><span data-stu-id="4c6c5-158">Download and install the Aspera client software:</span></span>
    
    * [<span data-ttu-id="4c6c5-159">Plugin-program för webbläsare</span><span class="sxs-lookup"><span data-stu-id="4c6c5-159">Browser plugin</span></span>](http://downloads.asperasoft.com/connect2/)
    * [<span data-ttu-id="4c6c5-160">Rich-klient</span><span class="sxs-lookup"><span data-stu-id="4c6c5-160">Rich client</span></span>](http://downloads.asperasoft.com/en/downloads/2)

2. <span data-ttu-id="4c6c5-161">Gör din första överföring.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-161">Make your first transfer.</span></span> <span data-ttu-id="4c6c5-162">Innan du kan använda Aspera-klienten med Aspera-överföringstjänsten måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="4c6c5-162">In order to use the Aspera client to transfer with the Aspera transfer service, you need to complete the following:</span></span> 

    1. <span data-ttu-id="4c6c5-163">Skapa en åtkomstnyckel med Aspera-portalen.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-163">Create an access key, using the Aspera portal.</span></span>  
    2. <span data-ttu-id="4c6c5-164">Hämta, installera och licensiera Aspera-klienten (programmet finns på Aspera-portalen).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-164">Download, install, and license the Aspera client (software can be found in the Aspera portal).</span></span>  

    >[!NOTE]
    ><span data-ttu-id="4c6c5-165">Läs konfigurationsinformationen i handboken för Aspera-klienten.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-165">Please read the Aspera client guide for configuration information.</span></span>
    
    3. <span data-ttu-id="4c6c5-166">Hämta information om lagringskontot som är associerat med ditt Azure Media-konto via [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-166">Retrieve some information of your storage account that is associated with your Azure Media Account using the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="4c6c5-167">Du behöver namn och nyckel samt namnet på lagringsblobbehållaren som du vill placera innehållet i.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-167">Specifically, name and key, and the storage blob container name in to which you want to place your content.</span></span> 

        * <span data-ttu-id="4c6c5-168">Så här hämtar du lagringsinformation från portalen: Leta reda på lagringskontot, klicka på Åtkomstnycklar och kopiera namnet och nyckeln för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-168">To get the storage info from the portal: find your storage account, click on the Access keys and copy the name and the key of your account.</span></span>
        * <span data-ttu-id="4c6c5-169">Så här hämtar du behållarnamnet: Leta reda på lagringskontot, välj **Blobar** och välj namnet på den behållare som du vill överföra innehållet till.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-169">To get the container name: find your storage account, select **Blobs**, select the name of the container you want to upload the content into.</span></span> 

    <span data-ttu-id="4c6c5-170">Nedan visas en skärmbild på **Connection Manager** för Aspera-klienten. Där måste du ange Azure-lagringstyp, autentiseringsuppgifter och blobbehållaren.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-170">Below is the screenshot of the Aspera client **Connection Manager** where you must specify the 'Azure' storage type and credentials as well as the blob container.</span></span>

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a><span data-ttu-id="4c6c5-172">Resurser</span><span class="sxs-lookup"><span data-stu-id="4c6c5-172">Resources</span></span>

<span data-ttu-id="4c6c5-173">Följande resurser har nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4c6c5-173">The following resources were mentioned in this article.</span></span> 

* [<span data-ttu-id="4c6c5-174">Ansluta plugin-program för webbläsare</span><span class="sxs-lookup"><span data-stu-id="4c6c5-174">Connect Browser Plugin</span></span>](http://downloads.asperasoft.com/connect2/)
* [<span data-ttu-id="4c6c5-175">Anslutningsguide</span><span class="sxs-lookup"><span data-stu-id="4c6c5-175">Connect Guide</span></span>](http://downloads.asperasoft.com/en/documentation/8)
* [<span data-ttu-id="4c6c5-176">Aspera-klient</span><span class="sxs-lookup"><span data-stu-id="4c6c5-176">Aspera Client</span></span>](http://downloads.asperasoft.com/en/downloads/2)
* [<span data-ttu-id="4c6c5-177">Klientguide</span><span class="sxs-lookup"><span data-stu-id="4c6c5-177">Client Guide</span></span>](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a><span data-ttu-id="4c6c5-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c6c5-178">Next steps</span></span>

<span data-ttu-id="4c6c5-179">Du kan nu [kopiera blobar från ett lagringskonto till ett AMS-konto](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span><span class="sxs-lookup"><span data-stu-id="4c6c5-179">You can now [copy blobs from a storage account into an AMS account ](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="4c6c5-180">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="4c6c5-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4c6c5-181">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="4c6c5-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

