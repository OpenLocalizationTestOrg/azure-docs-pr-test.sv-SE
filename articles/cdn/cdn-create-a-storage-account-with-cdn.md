---
title: aaaIntegrate ett Azure storage-konto med Azure CDN | Microsoft Docs
description: "Lär dig hur toouse hello Azure innehåll innehållsleveransnätverk (CDN) toodeliver hög bandbredd innehåll genom cachelagring av BLOB-objekt från Azure Storage."
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
ms.openlocfilehash: e44716969d6a784265cc4b1da34f0d021a17b38d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a><span data-ttu-id="517e4-103">Integrera Azure storage-konto med Azure CDN</span><span class="sxs-lookup"><span data-stu-id="517e4-103">Integrate an Azure storage account with Azure CDN</span></span>
<span data-ttu-id="517e4-104">CDN kan vara aktiverad toocache innehåll från ditt Azure storage.</span><span class="sxs-lookup"><span data-stu-id="517e4-104">CDN can be enabled toocache content from your Azure storage.</span></span> <span data-ttu-id="517e4-105">Den erbjuder utvecklare en global lösning för att leverera innehåll med hög bandbredd genom cachelagring av blobbar och compute-instanser på fysiska noder i hello USA, Europa, Asien, Australien och Sydamerika statiskt innehåll.</span><span class="sxs-lookup"><span data-stu-id="517e4-105">It offers developers a global solution for delivering high-bandwidth content by caching blobs and static content of compute instances at physical nodes in hello United States, Europe, Asia, Australia and South America.</span></span>

## <a name="step-1-create-a-storage-account"></a><span data-ttu-id="517e4-106">Steg 1: Skapa ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="517e4-106">Step 1: Create a storage account</span></span>
<span data-ttu-id="517e4-107">Använd följande procedur toocreate ett nytt lagringskonto för en Azure-prenumeration hello.</span><span class="sxs-lookup"><span data-stu-id="517e4-107">Use hello following procedure toocreate a new storage account for a Azure subscription.</span></span> <span data-ttu-id="517e4-108">Ett lagringskonto ger åtkomst till Azure storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="517e4-108">A storage account gives access to Azure storage services.</span></span> <span data-ttu-id="517e4-109">Hej lagringskonto representerar hello högsta nivå av hello namnområdet för åtkomst till var och en av hello Azure storage tjänstkomponenter: Blob-tjänster, Queue-tjänster och tabellen tjänster.</span><span class="sxs-lookup"><span data-stu-id="517e4-109">hello storage account represents hello highest level of hello namespace for accessing each of hello Azure storage service components: Blob services, Queue services, and Table services.</span></span> <span data-ttu-id="517e4-110">Mer information finns i toohello [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="517e4-110">For more information, refer toohello [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="517e4-111">toocreate ett lagringskonto, du måste vara hello tjänstadministratör eller en medadministratör för hello associerade prenumeration.</span><span class="sxs-lookup"><span data-stu-id="517e4-111">toocreate a storage account, you must be either hello service administrator or a co-administrator for hello associated subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="517e4-112">Det finns flera metoder du kan använda toocreate ett lagringskonto, inklusive hello Azure Portal och Powershell.</span><span class="sxs-lookup"><span data-stu-id="517e4-112">There are several methods you can use toocreate a storage account, including hello Azure Portal and Powershell.</span></span>  <span data-ttu-id="517e4-113">För den här självstudiekursen kommer vi att använda hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="517e4-113">For this tutorial, we'll be using hello Azure Portal.</span></span>  
> 
> 

<span data-ttu-id="517e4-114">**toocreate ett lagringskonto för en Azure-prenumeration**</span><span class="sxs-lookup"><span data-stu-id="517e4-114">**toocreate a storage account for an Azure subscription**</span></span>

1. <span data-ttu-id="517e4-115">Logga in toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="517e4-115">Sign in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="517e4-116">I hello övre vänstra hörnet, Välj **ny**.</span><span class="sxs-lookup"><span data-stu-id="517e4-116">In hello upper left corner, select **New**.</span></span> <span data-ttu-id="517e4-117">I hello **ny** väljer **Data + lagring**, klicka på **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="517e4-117">In hello **New** Dialog, select **Data  + Storage**, then click **Storage account**.</span></span>
    
    <span data-ttu-id="517e4-118">Hej **skapa lagringskonto** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="517e4-118">hello **Create storage account** blade appears.</span></span>   

    ![Skapa Lagringskonto][create-new-storage-account]  

3. <span data-ttu-id="517e4-120">I hello **namnet** skriver du ett underdomännamn.</span><span class="sxs-lookup"><span data-stu-id="517e4-120">In hello **Name** field, type a subdomain name.</span></span> <span data-ttu-id="517e4-121">Den här posten kan innehålla 3 till 24 gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="517e4-121">This entry can contain 3-24 lowercase letters and numbers.</span></span>
   
    <span data-ttu-id="517e4-122">Det här värdet blir hello värdnamnet inom hello URI som används för att adressera Blob, kö eller tabell resurser för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="517e4-122">This value becomes hello host name within hello URI that is used to address Blob, Queue, or Table resources for hello subscription.</span></span> <span data-ttu-id="517e4-123">För att lösa en behållare resurs i hello Blob-tjänsten har du använder en URI i hello följande format, där  *&lt;StorageAccountLabel&gt;*  refererar toohello värdet du angav i **ange en URL**:</span><span class="sxs-lookup"><span data-stu-id="517e4-123">To address a container resource in hello Blob service, you would use a URI in hello following format, where *&lt;StorageAccountLabel&gt;* refers toohello value you typed in **Enter a URL**:</span></span>
   
    <span data-ttu-id="517e4-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;minbehållare&gt;*</span><span class="sxs-lookup"><span data-stu-id="517e4-124">http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*</span></span>
   
    <span data-ttu-id="517e4-125">**Viktigt:** hello URL etikett formulär hello underdomän hello lagringskonto URI och måste vara unika bland alla värdbaserade tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="517e4-125">**Important:** hello URL label forms hello subdomain of hello storage  account URI and must be unique among all hosted services in  Azure.</span></span>
   
    <span data-ttu-id="517e4-126">Det här värdet används också som hello namnet på det här lagringskontot i hello-portalen eller vid åtkomst till det här kontot programmässigt.</span><span class="sxs-lookup"><span data-stu-id="517e4-126">This value is also used as hello name of this storage account in hello portal, or when accessing this account programmatically.</span></span>
4. <span data-ttu-id="517e4-127">Lämna hello standardvärden för **distributionsmodellen**, **konto kind**, **prestanda**, och **replikering**.</span><span class="sxs-lookup"><span data-stu-id="517e4-127">Leave hello defaults for **Deployment model**, **Account kind**, **Performance**, and **Replication**.</span></span> 
5. <span data-ttu-id="517e4-128">Välj hello **prenumeration** att hello storage-konto kommer att användas med.</span><span class="sxs-lookup"><span data-stu-id="517e4-128">Select hello **Subscription** that hello storage account will be used with.</span></span>
6. <span data-ttu-id="517e4-129">Välj eller skapa en **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="517e4-129">Select or create a **Resource Group**.</span></span>  <span data-ttu-id="517e4-130">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).</span><span class="sxs-lookup"><span data-stu-id="517e4-130">For more information on Resource Groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups).</span></span>
7. <span data-ttu-id="517e4-131">Välj en plats för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="517e4-131">Select a location for your storage account.</span></span>
8. <span data-ttu-id="517e4-132">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="517e4-132">Click **Create**.</span></span> <span data-ttu-id="517e4-133">hello processen att skapa hello storage-konto kan ta flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="517e4-133">hello process of creating hello storage account might take several minutes toocomplete.</span></span>

## <a name="step-2-enable-cdn-for-hello-storage-account"></a><span data-ttu-id="517e4-134">Steg 2: Aktivera CDN för hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="517e4-134">Step 2: Enable CDN for hello storage account</span></span>

<span data-ttu-id="517e4-135">Med senaste hello-integration kan du nu aktivera CDN för ditt lagringskonto utan att lämna lagring-portaltillägg.</span><span class="sxs-lookup"><span data-stu-id="517e4-135">With hello newest integration, you can now enable CDN for your storage account without leaving your storage portal extension.</span></span> 

1. <span data-ttu-id="517e4-136">Välj hello lagringskonto, söka ”CDN” eller rulla nedåt från hello vänstra navigeringsmenyn och klicka på ”Azure CDN”.</span><span class="sxs-lookup"><span data-stu-id="517e4-136">Select hello storage account, search "CDN" or scroll down from hello left navigation menu, then click "Azure CDN".</span></span>
    
    <span data-ttu-id="517e4-137">Hej **Azure CDN** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="517e4-137">hello **Azure CDN** blade appears.</span></span>

    ![CDN aktivera navigering][cdn-enable-navigation]
    
2. <span data-ttu-id="517e4-139">Skapa en ny slutpunkt genom att ange hello krävs information</span><span class="sxs-lookup"><span data-stu-id="517e4-139">Create a new endpoint by entering hello required information</span></span>
    - <span data-ttu-id="517e4-140">**CDN-profilen**: du kan skapa en ny eller använda en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="517e4-140">**CDN Profile**: You can create a new or use an existing profile.</span></span>
    - <span data-ttu-id="517e4-141">**Prisnivån**: behöver du bara tooselect en prisnivå om du skapar en ny CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="517e4-141">**Pricing tier**: You only need tooselect a pricing tier if you create a new CDN profile.</span></span>
    - <span data-ttu-id="517e4-142">**Namnet på slutpunkten CDN**: Ange en slutpunktsnamn per valet.</span><span class="sxs-lookup"><span data-stu-id="517e4-142">**CDN endpoint name**: Enter an endpoint name per your choice.</span></span>

    > [!TIP]
    > <span data-ttu-id="517e4-143">hello skapa CDN-slutpunkten använder hello värdnamnet för ditt lagringskonto som ursprung som standard.</span><span class="sxs-lookup"><span data-stu-id="517e4-143">hello created CDN endpoint uses hello hostname of your storage account as origin by default.</span></span>

    <span data-ttu-id="517e4-144">! [cdn-slutpunkten skapandet av ny] [cdn--slutpunkt-skapa nya]</span><span class="sxs-lookup"><span data-stu-id="517e4-144">![cdn new endpoint creation][cdn-new-endpoint-creation]</span></span>

3. <span data-ttu-id="517e4-145">När den har skapats visas hello ny slutpunkt i hello endpoint listan ovan.</span><span class="sxs-lookup"><span data-stu-id="517e4-145">After creation, hello new endpoint will show up in hello endpoint list above.</span></span>

    ![CDN-slutpunkten ny lagring][cdn-storage-new-endpoint]

> [!NOTE]
> <span data-ttu-id="517e4-147">Du kan även gå tooAzure CDN tillägget tooenable CDN. [Kursen](#Tutorial-cdn-create-profile).</span><span class="sxs-lookup"><span data-stu-id="517e4-147">You can also go tooAzure CDN extension tooenable CDN.[Tutorial](#Tutorial-cdn-create-profile).</span></span>
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a><span data-ttu-id="517e4-148">Steg 3: Aktivera ytterligare CDN-funktioner</span><span class="sxs-lookup"><span data-stu-id="517e4-148">Step 3: Enable additional CDN features</span></span>

<span data-ttu-id="517e4-149">Klicka på hello CDN-slutpunkten från hello listan tooopen CDN configuration bladet storage-konto ”Azure CDN” bladet.</span><span class="sxs-lookup"><span data-stu-id="517e4-149">From storage account "Azure CDN" blade, click hello CDN endpoint from hello list tooopen CDN configuration blade.</span></span> <span data-ttu-id="517e4-150">Du kan aktivera ytterligare CDN-funktioner för leveransen, till exempel komprimering, frågesträngen geo filtrering.</span><span class="sxs-lookup"><span data-stu-id="517e4-150">You can enable additional CDN features for your delivery, such as compression, query string, geo filtering.</span></span> <span data-ttu-id="517e4-151">Du kan också lägga till anpassade domäner mappning tooyour CDN-slutpunkten och aktivera anpassad domän för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="517e4-151">You can also add custom domain mapping tooyour CDN endpoint and enable custom domain HTTPS.</span></span>
    
![cdn-CDN lagringskonfigurationen][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a><span data-ttu-id="517e4-153">Steg 4: Åtkomst CDN innehåll</span><span class="sxs-lookup"><span data-stu-id="517e4-153">Step 4: Access CDN content</span></span>
<span data-ttu-id="517e4-154">tooaccess cachelagrat innehåll på hello CDN, Använd hello CDN-URL som angetts i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="517e4-154">tooaccess cached content on hello CDN, use hello CDN URL provided in hello portal.</span></span> <span data-ttu-id="517e4-155">hello-adress för en cachelagrade blob är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="517e4-155">hello address for a cached blob will be similar toohello following:</span></span>

<span data-ttu-id="517e4-156">http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\></span><span class="sxs-lookup"><span data-stu-id="517e4-156">http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\></span></span>

> [!NOTE]
> <span data-ttu-id="517e4-157">När du aktiverar CDN åtkomst tooa storage-konto, är alla offentligt tillgängliga objekt berättigade för cachelagring av CDN kant.</span><span class="sxs-lookup"><span data-stu-id="517e4-157">Once you enable CDN access tooa storage account, all publicly available objects are eligible for CDN edge caching.</span></span> <span data-ttu-id="517e4-158">Om du ändrar ett objekt som för tillfället är cachelagrade i hello CDN hello nytt innehåll inte tillgängliga via hello CDN förrän hello CDN uppdateras dess innehåll när hello cachelagrat innehåll time to live-period har löpt ut.</span><span class="sxs-lookup"><span data-stu-id="517e4-158">If you modify an object that is currently cached in hello CDN, hello new content will not be available via hello CDN until hello CDN refreshes its content when hello cached content time-to-live period expires.</span></span>
> 
> 

## <a name="step-5-remove-content-from-hello-cdn"></a><span data-ttu-id="517e4-159">Steg 5: Ta bort innehåll från hello CDN</span><span class="sxs-lookup"><span data-stu-id="517e4-159">Step 5: Remove content from hello CDN</span></span>
<span data-ttu-id="517e4-160">Om du inte längre vill toocache ett objekt i hello Azure Content Delivery Network (CDN) måste göra du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="517e4-160">If you no longer wish toocache an object in hello Azure Content Delivery Network (CDN), you can take one of hello following steps:</span></span>

* <span data-ttu-id="517e4-161">Du kan göra hello behållaren privat i stället för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="517e4-161">You can make hello container private instead of public.</span></span> <span data-ttu-id="517e4-162">Se [Hantera anonym läsbehörighet toocontainers och blobbar](../storage/blobs/storage-manage-access-to-resources.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="517e4-162">See [Manage anonymous read access toocontainers and blobs](../storage/blobs/storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="517e4-163">Du kan inaktivera eller ta bort hello CDN-slutpunkten med hello-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="517e4-163">You can disable or delete hello CDN endpoint using hello Management Portal.</span></span>
* <span data-ttu-id="517e4-164">Du kan ändra din värdbaserade tjänsten toono längre svara toorequests för hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="517e4-164">You can modify your hosted service toono longer respond toorequests for hello object.</span></span>

<span data-ttu-id="517e4-165">Ett objekt som redan har cachelagrats i hello CDN förblir cachelagrade tills hello time to live för hello objektet ut eller hello endpoint tas bort.</span><span class="sxs-lookup"><span data-stu-id="517e4-165">An object already cached in hello CDN will remain cached until hello time-to-live period for hello object expires or until hello endpoint is purged.</span></span> <span data-ttu-id="517e4-166">När hello time to live period har löpt ut kontrollerar hello CDN toosee om hello CDN-slutpunkten fortfarande är giltigt och hello objekt anonymt tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="517e4-166">When hello time-to-live period expires, hello CDN will check toosee whether hello CDN endpoint is still valid and hello object still anonymously accessible.</span></span> <span data-ttu-id="517e4-167">Om det inte sedan cachelagras hello objektet inte längre.</span><span class="sxs-lookup"><span data-stu-id="517e4-167">If it is not, then hello object will no longer be cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="517e4-168">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="517e4-168">Additional resources</span></span>
* [<span data-ttu-id="517e4-169">Hur tooMap innehåll till CDN tooa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="517e4-169">How tooMap CDN Content tooa Custom Domain</span></span>](cdn-map-content-to-custom-domain.md)
* [<span data-ttu-id="517e4-170">Aktivera HTTPS för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="517e4-170">Enable HTTPS for your custom domain</span></span>](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
