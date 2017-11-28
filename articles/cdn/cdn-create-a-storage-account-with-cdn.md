---
title: Integrera Azure storage-konto med Azure CDN | Microsoft Docs
description: "Lär dig hur du använder i Azure innehåll innehållsleveransnätverk (CDN) för att tillhandahålla hög bandbredd innehåll genom cachelagring blobbar från Azure Storage."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 511076935d06ed0908341044e37069e74530be49
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a><span data-ttu-id="330d5-103">Integrera Azure storage-konto med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="330d5-103">Integrate an Azure storage account with Azure CDN</span></span>
<span data-ttu-id="330d5-104">CDN kan aktiveras till att cachelagra innehåll från ditt Azure storage.</span><span class="sxs-lookup"><span data-stu-id="330d5-104">CDN can be enabled to cache content from your Azure storage.</span></span> <span data-ttu-id="330d5-105">Den erbjuder utvecklare en global lösning för att leverera innehåll med hög bandbredd genom cachelagring av blobbar och compute-instanser på fysiska noder i USA, Europa, Asien, Australien och Sydamerika statiskt innehåll.</span><span class="sxs-lookup"><span data-stu-id="330d5-105">It offers developers a global solution for delivering high-bandwidth content by caching blobs and static content of compute instances at physical nodes in the United States, Europe, Asia, Australia and South America.</span></span>

## <a name="step-1-create-a-storage-account"></a><span data-ttu-id="330d5-106">Steg 1: Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="330d5-106">Step 1: Create a storage account</span></span>
<span data-ttu-id="330d5-107">Använd följande procedur för att skapa ett nytt lagringskonto för en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="330d5-107">Use the following procedure to create a new storage account for a Azure subscription.</span></span> <span data-ttu-id="330d5-108">Ett lagringskonto ger åtkomst till Azure storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="330d5-108">A storage account gives access to Azure storage services.</span></span> <span data-ttu-id="330d5-109">Lagringskontot representerar den högsta nivån av namnområdet för åtkomst till alla komponenter för Azure storage-tjänsten: Blob-tjänster, Queue-tjänster och tabellen tjänster.</span><span class="sxs-lookup"><span data-stu-id="330d5-109">The storage account represents the highest level of the namespace for accessing each of the Azure storage service components: Blob services, Queue services, and Table services.</span></span> <span data-ttu-id="330d5-110">Mer information finns i den [introduktion till Microsoft Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="330d5-110">For more information, refer to the [Introduction to Microsoft Azure Storage](../storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="330d5-111">Du måste vara tjänstadministratör eller en medadministratör för den associera prenumerationen om du vill skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="330d5-111">To create a storage account, you must be either the service administrator or a co-administrator for the associated subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="330d5-112">Det finns flera metoder du kan använda för att skapa ett lagringskonto, inklusive Azure Portal och Powershell.</span><span class="sxs-lookup"><span data-stu-id="330d5-112">There are several methods you can use to create a storage account, including the Azure Portal and Powershell.</span></span>  <span data-ttu-id="330d5-113">Den här självstudiekursen kommer ska vi använda Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="330d5-113">For this tutorial, we'll be using the Azure Portal.</span></span>  
> 
> 

<span data-ttu-id="330d5-114">**Skapa ett lagringskonto för en Azure-prenumeration**</span><span class="sxs-lookup"><span data-stu-id="330d5-114">**To create a storage account for an Azure subscription**</span></span>

1. <span data-ttu-id="330d5-115">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="330d5-115">Sign in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="330d5-116">I det övre vänstra hörnet väljer **ny**.</span><span class="sxs-lookup"><span data-stu-id="330d5-116">In the upper left corner, select **New**.</span></span> <span data-ttu-id="330d5-117">I den **ny** väljer **Data + lagring**, klicka på **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="330d5-117">In the **New** Dialog, select **Data  + Storage**, then click **Storage account**.</span></span>
    
    <span data-ttu-id="330d5-118">Den **skapa lagringskonto** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="330d5-118">The **Create storage account** blade appears.</span></span>   

    ![Skapa Lagringskonto][create-new-storage-account]  

3. <span data-ttu-id="330d5-120">I den **namnet** skriver du ett underdomännamn.</span><span class="sxs-lookup"><span data-stu-id="330d5-120">In the **Name** field, type a subdomain name.</span></span> <span data-ttu-id="330d5-121">Den här posten kan innehålla 3 till 24 gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="330d5-121">This entry can contain 3-24 lowercase letters and numbers.</span></span>
   
    <span data-ttu-id="330d5-122">Det här värdet blir värdnamnet inom URI: N som används för att adressera Blob, kö eller tabell resurser för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="330d5-122">This value becomes the host name within the URI that is used to address Blob, Queue, or Table resources for the subscription.</span></span> <span data-ttu-id="330d5-123">För att lösa en behållare resurs i Blob-tjänsten har du använder en URI i följande format, där  *&lt;StorageAccountLabel&gt;*  refererar till värdet du angav i **ange en URL**:</span><span class="sxs-lookup"><span data-stu-id="330d5-123">To address a container resource in the Blob service, you would use a URI in the following format, where *&lt;StorageAccountLabel&gt;* refers to the value you typed in **Enter a URL**:</span></span>
   
    <span data-ttu-id="330d5-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;minbehållare&gt;*</span><span class="sxs-lookup"><span data-stu-id="330d5-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span></span>
   
    <span data-ttu-id="330d5-125">**Viktigt:** URL: en etikett formulär underdomänen lagringskontot URI och måste vara unika bland alla värdbaserade tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="330d5-125">**Important:** The URL label forms the subdomain of the storage  account URI and must be unique among all hosted services in  Azure.</span></span>
   
    <span data-ttu-id="330d5-126">Det här värdet används också som namn på det här lagringskontot i portalen eller vid åtkomst till det här kontot programmässigt.</span><span class="sxs-lookup"><span data-stu-id="330d5-126">This value is also used as the name of this storage account in the portal, or when accessing this account programmatically.</span></span>
4. <span data-ttu-id="330d5-127">Lämnar du standardinställningarna för **distributionsmodellen**, **konto kind**, **prestanda**, och **replikering**.</span><span class="sxs-lookup"><span data-stu-id="330d5-127">Leave the defaults for **Deployment model**, **Account kind**, **Performance**, and **Replication**.</span></span> 
5. <span data-ttu-id="330d5-128">Välj den **prenumeration** som lagringskontot kommer att användas med.</span><span class="sxs-lookup"><span data-stu-id="330d5-128">Select the **Subscription** that the storage account will be used with.</span></span>
6. <span data-ttu-id="330d5-129">Välj eller skapa en **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="330d5-129">Select or create a **Resource Group**.</span></span>  <span data-ttu-id="330d5-130">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="330d5-130">For more information on Resource Groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
7. <span data-ttu-id="330d5-131">Välj en plats för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="330d5-131">Select a location for your storage account.</span></span>
8. <span data-ttu-id="330d5-132">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="330d5-132">Click **Create**.</span></span> <span data-ttu-id="330d5-133">Processen att skapa lagringskontot kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="330d5-133">The process of creating the storage account might take several minutes to complete.</span></span>

## <a name="step-2-enable-cdn-for-the-storage-account"></a><span data-ttu-id="330d5-134">Steg 2: Aktivera CDN för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="330d5-134">Step 2: Enable CDN for the storage account</span></span>

<span data-ttu-id="330d5-135">Med den senaste integrationen kan du nu aktivera CDN för ditt lagringskonto utan att lämna lagring-portaltillägg.</span><span class="sxs-lookup"><span data-stu-id="330d5-135">With the newest integration, you can now enable CDN for your storage account without leaving your storage portal extension.</span></span> 

1. <span data-ttu-id="330d5-136">Välj lagringskonto, söka efter ”CDN” eller bläddra nedåt från den vänstra navigeringsmenyn och klicka på ”Azure CDN”.</span><span class="sxs-lookup"><span data-stu-id="330d5-136">Select the storage account, search "CDN" or scroll down from the left navigation menu, then click "Azure CDN".</span></span>
    
    <span data-ttu-id="330d5-137">Den **Azure CDN** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="330d5-137">The **Azure CDN** blade appears.</span></span>

    ![CDN aktivera navigering][cdn-enable-navigation]
    
2. <span data-ttu-id="330d5-139">Skapa en ny slutpunkt genom att ange informationen som krävs</span><span class="sxs-lookup"><span data-stu-id="330d5-139">Create a new endpoint by entering the required information</span></span>
    - <span data-ttu-id="330d5-140">**CDN-profilen**: du kan skapa en ny eller använda en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="330d5-140">**CDN Profile**: You can create a new or use an existing profile.</span></span>
    - <span data-ttu-id="330d5-141">**Prisnivån**: du behöver bara välja en prisnivå om du skapar en ny CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="330d5-141">**Pricing tier**: You only need to select a pricing tier if you create a new CDN profile.</span></span>
    - <span data-ttu-id="330d5-142">**Namnet på slutpunkten CDN**: Ange en slutpunktsnamn per valet.</span><span class="sxs-lookup"><span data-stu-id="330d5-142">**CDN endpoint name**: Enter an endpoint name per your choice.</span></span>

    > [!TIP]
    > <span data-ttu-id="330d5-143">Skapade CDN-slutpunkten använder värdnamnet för ditt lagringskonto som ursprung som standard.</span><span class="sxs-lookup"><span data-stu-id="330d5-143">The created CDN endpoint uses the hostname of your storage account as origin by default.</span></span>

    <span data-ttu-id="330d5-144">! [cdn-slutpunkten skapandet av ny] [cdn--slutpunkt-skapa nya]</span><span class="sxs-lookup"><span data-stu-id="330d5-144">![cdn new endpoint creation][cdn-new-endpoint-creation]</span></span>

3. <span data-ttu-id="330d5-145">När den har skapats, ny slutpunkt kommer att visas i listan slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="330d5-145">After creation, the new endpoint will show up in the endpoint list above.</span></span>

    ![CDN-slutpunkten ny lagring][cdn-storage-new-endpoint]

> [!NOTE]
> <span data-ttu-id="330d5-147">Du kan även gå till Azure CDN-tillägg för att aktivera CDN. [Kursen](#Tutorial-cdn-create-profile).</span><span class="sxs-lookup"><span data-stu-id="330d5-147">You can also go to Azure CDN extension to enable CDN.[Tutorial](#Tutorial-cdn-create-profile).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a><span data-ttu-id="330d5-148">Steg 3: Aktivera ytterligare CDN-funktioner</span><span class="sxs-lookup"><span data-stu-id="330d5-148">Step 3: Enable additional CDN features</span></span>

<span data-ttu-id="330d5-149">Storage-konto ”Azure CDN” bladet Klicka CDN-slutpunkten i listan för att öppna bladet för CDN-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="330d5-149">From storage account "Azure CDN" blade, click the CDN endpoint from the list to open CDN configuration blade.</span></span> <span data-ttu-id="330d5-150">Du kan aktivera ytterligare CDN-funktioner för leveransen, till exempel komprimering, frågesträngen geo filtrering.</span><span class="sxs-lookup"><span data-stu-id="330d5-150">You can enable additional CDN features for your delivery, such as compression, query string, geo filtering.</span></span> <span data-ttu-id="330d5-151">Du kan också lägga till anpassade domänmappning till CDN-slutpunkten och aktivera anpassad domän för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="330d5-151">You can also add custom domain mapping to your CDN endpoint and enable custom domain HTTPS.</span></span>
    
![cdn-CDN lagringskonfigurationen][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a><span data-ttu-id="330d5-153">Steg 4: Åtkomst CDN innehåll</span><span class="sxs-lookup"><span data-stu-id="330d5-153">Step 4: Access CDN content</span></span>
<span data-ttu-id="330d5-154">Använd CDN-URL som anges i portalen för att komma åt cachelagrade innehåll på CDN.</span><span class="sxs-lookup"><span data-stu-id="330d5-154">To access cached content on the CDN, use the CDN URL provided in the portal.</span></span> <span data-ttu-id="330d5-155">Adressen för en cachelagrade blob ska vara liknar följande:</span><span class="sxs-lookup"><span data-stu-id="330d5-155">The address for a cached blob will be similar to the following:</span></span>

<span data-ttu-id="330d5-156">http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\></span><span class="sxs-lookup"><span data-stu-id="330d5-156">http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\></span></span>

> [!NOTE]
> <span data-ttu-id="330d5-157">När du aktiverar CDN åtkomst till ett lagringskonto, är alla offentligt tillgängliga objekt berättigade för cachelagring av CDN kant.</span><span class="sxs-lookup"><span data-stu-id="330d5-157">Once you enable CDN access to a storage account, all publicly available objects are eligible for CDN edge caching.</span></span> <span data-ttu-id="330d5-158">Om du ändrar ett objekt som för tillfället är cachelagrade i CDN nytt innehåll inte tillgängliga via CDN förrän CDN uppdateras dess innehåll när den cachelagrade innehållet time to live-perioden har löpt ut.</span><span class="sxs-lookup"><span data-stu-id="330d5-158">If you modify an object that is currently cached in the CDN, the new content will not be available via the CDN until the CDN refreshes its content when the cached content time-to-live period expires.</span></span>
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a><span data-ttu-id="330d5-159">Steg 5: Ta bort innehåll från CDN</span><span class="sxs-lookup"><span data-stu-id="330d5-159">Step 5: Remove content from the CDN</span></span>
<span data-ttu-id="330d5-160">Om du inte längre vill cachelagra ett objekt i Azure Content Delivery innehållsleveransnätverk (CDN) kan du göra något av följande steg:</span><span class="sxs-lookup"><span data-stu-id="330d5-160">If you no longer wish to cache an object in the Azure Content Delivery Network (CDN), you can take one of the following steps:</span></span>

* <span data-ttu-id="330d5-161">Du kan göra behållaren privat i stället för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="330d5-161">You can make the container private instead of public.</span></span> <span data-ttu-id="330d5-162">Mer information finns i [Hantera anonym läsbehörighet till behållare och blobbar](../storage/blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="330d5-162">See [Manage anonymous read access to containers and blobs](../storage/blobs/storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="330d5-163">Du kan inaktivera eller ta bort CDN-slutpunkten med hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="330d5-163">You can disable or delete the CDN endpoint using the Management Portal.</span></span>
* <span data-ttu-id="330d5-164">Du kan ändra din värdbaserade tjänst för att inte längre svarar på begäranden för objektet.</span><span class="sxs-lookup"><span data-stu-id="330d5-164">You can modify your hosted service to no longer respond to requests for the object.</span></span>

<span data-ttu-id="330d5-165">Ett objekt som redan har cachelagrats i CDN förblir cachelagrade tills time to live-period för objektet har löpt ut eller slutpunkten tas bort.</span><span class="sxs-lookup"><span data-stu-id="330d5-165">An object already cached in the CDN will remain cached until the time-to-live period for the object expires or until the endpoint is purged.</span></span> <span data-ttu-id="330d5-166">När time to live-period har löpt ut ska CDN kontrollera om CDN-slutpunkten är fortfarande giltigt och anonymt fortfarande komma åt objektet.</span><span class="sxs-lookup"><span data-stu-id="330d5-166">When the time-to-live period expires, the CDN will check to see whether the CDN endpoint is still valid and the object still anonymously accessible.</span></span> <span data-ttu-id="330d5-167">Om det inte är det, kommer inte längre objektet cachelagras.</span><span class="sxs-lookup"><span data-stu-id="330d5-167">If it is not, then the object will no longer be cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="330d5-168">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="330d5-168">Additional resources</span></span>
* [<span data-ttu-id="330d5-169">Mappa CDN-innehåll till en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="330d5-169">How to Map CDN Content to a Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="330d5-170">Aktivera HTTPS för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="330d5-170">Enable HTTPS for your custom domain</span></span>](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
