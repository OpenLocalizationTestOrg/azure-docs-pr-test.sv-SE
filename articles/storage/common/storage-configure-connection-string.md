---
title: "aaaConfigure en anslutningssträng för Azure Storage | Microsoft Docs"
description: "Konfigurera en anslutningssträng för ett Azure storage-konto. En anslutningssträng innehåller hello information behövs tooauthenticate åtkomst tooa lagringskontot från ditt program vid körning."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: marsma
ms.openlocfilehash: ac1d7d9bf11fa6f44243cda0c40d8faee12e513b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-storage-connection-strings"></a><span data-ttu-id="fad35-104">Konfigurera Azure Storage-anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="fad35-104">Configure Azure Storage connection strings</span></span>

<span data-ttu-id="fad35-105">En anslutningssträng innehåller hello autentiseringsinformation som krävs för dina programdata tooaccess i ett Azure Storage-konto vid körning.</span><span class="sxs-lookup"><span data-stu-id="fad35-105">A connection string includes hello authentication information required for your application tooaccess data in an Azure Storage account at runtime.</span></span> <span data-ttu-id="fad35-106">Du kan konfigurera anslutningssträngar för att:</span><span class="sxs-lookup"><span data-stu-id="fad35-106">You can configure connection strings to:</span></span>

* <span data-ttu-id="fad35-107">Ansluta toohello Azure storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="fad35-107">Connect toohello Azure storage emulator.</span></span>
* <span data-ttu-id="fad35-108">Åtkomst till ett lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="fad35-108">Access a storage account in Azure.</span></span>
* <span data-ttu-id="fad35-109">Åtkomst till angivna resurser i Azure via en signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="fad35-109">Access specified resources in Azure via a shared access signature (SAS).</span></span>

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a><span data-ttu-id="fad35-110">Lagra anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="fad35-110">Storing your connection string</span></span>
<span data-ttu-id="fad35-111">Programmet måste tooaccess hello anslutningssträngen vid körning tooauthenticate begäranden tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="fad35-111">Your application needs tooaccess hello connection string at runtime tooauthenticate requests made tooAzure Storage.</span></span> <span data-ttu-id="fad35-112">Du har flera alternativ för att lagra anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="fad35-112">You have several options for storing your connection string:</span></span>

* <span data-ttu-id="fad35-113">Ett program som körs på skrivbordet hello eller på en enhet kan lagra hello anslutningssträngen i en **app.config** eller **web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="fad35-113">An application running on hello desktop or on a device can store hello connection string in an **app.config** or **web.config** file.</span></span> <span data-ttu-id="fad35-114">Lägg till hello anslutning sträng toohello **AppSettings** avsnitt i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="fad35-114">Add hello connection string toohello **AppSettings** section in these files.</span></span>
* <span data-ttu-id="fad35-115">Ett program som körs i en Azure-molntjänst kan lagra hello anslutningssträngen i hello [Azure-tjänstkonfigurationsfil schema (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="fad35-115">An application running in an Azure cloud service can store hello connection string in hello [Azure service configuration schema (.cscfg) file](https://msdn.microsoft.com/library/ee758710.aspx).</span></span> <span data-ttu-id="fad35-116">Lägg till hello anslutning sträng toohello **ConfigurationSettings** i hello service konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="fad35-116">Add hello connection string toohello **ConfigurationSettings** section of hello service configuration file.</span></span>
* <span data-ttu-id="fad35-117">Du kan använda anslutningssträngen direkt i din kod.</span><span class="sxs-lookup"><span data-stu-id="fad35-117">You can use your connection string directly in your code.</span></span> <span data-ttu-id="fad35-118">Vi rekommenderar dock att du lagrar anslutningssträngen i en konfigurationsfil i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="fad35-118">However, we recommend that you store your connection string in a configuration file in most scenarios.</span></span>

<span data-ttu-id="fad35-119">Spara anslutningssträngen i en konfigurationsfil gör det enkelt tooupdate hello anslutning sträng tooswitch mellan hello storage-emulatorn och ett Azure storage-konto i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="fad35-119">Storing your connection string in a configuration file makes it easy tooupdate hello connection string tooswitch between hello storage emulator and an Azure storage account in hello cloud.</span></span> <span data-ttu-id="fad35-120">Du behöver bara tooedit hello anslutning sträng toopoint tooyour målmiljön.</span><span class="sxs-lookup"><span data-stu-id="fad35-120">You only need tooedit hello connection string toopoint tooyour target environment.</span></span>

<span data-ttu-id="fad35-121">Du kan använda hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess anslutningssträngen vid körningstillfället oavsett där programmet körs.</span><span class="sxs-lookup"><span data-stu-id="fad35-121">You can use hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess your connection string at runtime regardless of where your application is running.</span></span>

## <a name="create-a-connection-string-for-hello-storage-emulator"></a><span data-ttu-id="fad35-122">Skapa en anslutningssträng för hello storage-emulatorn</span><span class="sxs-lookup"><span data-stu-id="fad35-122">Create a connection string for hello storage emulator</span></span>
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

<span data-ttu-id="fad35-123">Läs mer om hello storage-emulatorn [Använd hello Azure storage-emulatorn för utveckling och testning](storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="fad35-123">For more information about hello storage emulator, see [Use hello Azure storage emulator for development and testing](storage-use-emulator.md).</span></span>

## <a name="create-a-connection-string-for-an-azure-storage-account"></a><span data-ttu-id="fad35-124">Skapa en anslutningssträng för ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="fad35-124">Create a connection string for an Azure storage account</span></span>
<span data-ttu-id="fad35-125">toocreate en anslutningssträng för Azure storage-konto, Använd hello följande format.</span><span class="sxs-lookup"><span data-stu-id="fad35-125">toocreate a connection string for your Azure storage account, use hello following format.</span></span> <span data-ttu-id="fad35-126">Ange om du vill tooconnect toohello storage-konto via HTTPS (rekommenderas) eller HTTP ersätter `myAccountName` med hello namnet på ditt lagringskonto och Ersätt `myAccountKey` med din åtkomstnyckel:</span><span class="sxs-lookup"><span data-stu-id="fad35-126">Indicate whether you want tooconnect toohello storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with hello name of your storage account, and replace `myAccountKey` with your account access key:</span></span>

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

<span data-ttu-id="fad35-127">Anslutningssträngen kan till exempel se ut:</span><span class="sxs-lookup"><span data-stu-id="fad35-127">For example, your connection string might look similar to:</span></span>

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

<span data-ttu-id="fad35-128">Azure Storage har stöd för både HTTP och HTTPS i en anslutningssträng *HTTPS rekommenderas*.</span><span class="sxs-lookup"><span data-stu-id="fad35-128">Although Azure Storage supports both HTTP and HTTPS in a connection string, *HTTPS is highly recommended*.</span></span>

> [!TIP]
> <span data-ttu-id="fad35-129">Du hittar ditt lagringskonto anslutningssträngar i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fad35-129">You can find your storage account's connection strings in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="fad35-130">Navigera för**inställningar** > **åtkomstnycklar** i ditt lagringskonto menyn bladet toosee-anslutningssträngar för båda primära och sekundära nycklarna.</span><span class="sxs-lookup"><span data-stu-id="fad35-130">Navigate too**SETTINGS** > **Access keys** in your storage account's menu blade toosee connection strings for both primary and secondary access keys.</span></span>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a><span data-ttu-id="fad35-131">Skapa en anslutningssträng med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="fad35-131">Create a connection string using a shared access signature</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a><span data-ttu-id="fad35-132">Skapa en anslutningssträng för en explicit lagring-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="fad35-132">Create a connection string for an explicit storage endpoint</span></span>
<span data-ttu-id="fad35-133">Du kan ange Tjänsteslutpunkter explicit i anslutningssträngen i stället för hello slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="fad35-133">You can specify explicit service endpoints in your connection string instead of using hello default endpoints.</span></span> <span data-ttu-id="fad35-134">toocreate en anslutningssträng som anger en slutpunkt som explicit ange hello fullständig tjänstslutpunkten för varje tjänst, inklusive hello protokollspecifikation (HTTPS (rekommenderas) eller HTTP) i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="fad35-134">toocreate a connection string that specifies an explicit endpoint, specify hello complete service endpoint for each service, including hello protocol specification (HTTPS (recommended) or HTTP), in hello following format:</span></span>

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

<span data-ttu-id="fad35-135">Ett scenario där du kanske vill toospecify en explicit slutpunkt är när du har kopplat till ditt Blob storage endpoint tooa [domänen](../blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="fad35-135">One scenario where you might wish toospecify an explicit endpoint is when you've mapped your Blob storage endpoint tooa [custom domain](../blobs/storage-custom-domain-name.md).</span></span> <span data-ttu-id="fad35-136">I så fall kan du ange din anpassade slutpunkt för Blob storage i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="fad35-136">In that case, you can specify your custom endpoint for Blob storage in your connection string.</span></span> <span data-ttu-id="fad35-137">Du kan också ange hello standardslutpunkterna för hello andra tjänster om programmet använder dem.</span><span class="sxs-lookup"><span data-stu-id="fad35-137">You can optionally specify hello default endpoints for hello other services if your application uses them.</span></span>

<span data-ttu-id="fad35-138">Här är ett exempel på en anslutningssträng som anger en explicit slutpunkt för hello Blob-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="fad35-138">Here is an example of a connection string that specifies an explicit endpoint for hello Blob service:</span></span>

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="fad35-139">Det här exemplet anger explicit slutpunkter för alla tjänster, inklusive en anpassad domän för hello Blob-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="fad35-139">This example specifies explicit endpoints for all services, including a custom domain for hello Blob service:</span></span>

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="fad35-140">hello endpoint värden i en anslutningssträng är används tooconstruct hello begäran URI: er toohello storage-tjänster och styr hello form av alla URI: er som returneras tooyour kod.</span><span class="sxs-lookup"><span data-stu-id="fad35-140">hello endpoint values in a connection string are used tooconstruct hello request URIs toohello storage services, and dictate hello form of any URIs that are returned tooyour code.</span></span>

<span data-ttu-id="fad35-141">Om du har kopplat en anpassad domän för lagring endpoint tooa och utesluter den slutpunkten från en anslutningssträng och du inte kommer att kunna toouse strängen anslutningen tooaccess data i tjänsten från din kod.</span><span class="sxs-lookup"><span data-stu-id="fad35-141">If you've mapped a storage endpoint tooa custom domain and omit that endpoint from a connection string, then you will not be able toouse that connection string tooaccess data in that service from your code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fad35-142">Tjänsten endpoint värdena i anslutningssträngarna måste vara giltig URI: er, inklusive `https://` (rekommenderas) eller `http://`.</span><span class="sxs-lookup"><span data-stu-id="fad35-142">Service endpoint values in your connection strings must be well-formed URIs, including `https://` (recommended) or `http://`.</span></span> <span data-ttu-id="fad35-143">Eftersom Azure Storage inte stöder ännu HTTPS för anpassade domäner du *måste* ange `http://` för valfri slutpunkt URI som pekar tooa anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="fad35-143">Because Azure Storage does not yet support HTTPS for custom domains, you *must* specify `http://` for any endpoint URI that points tooa custom domain.</span></span>
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a><span data-ttu-id="fad35-144">Skapa en anslutningssträng med en slutpunkt-suffix</span><span class="sxs-lookup"><span data-stu-id="fad35-144">Create a connection string with an endpoint suffix</span></span>
<span data-ttu-id="fad35-145">toocreate en anslutningssträngen för en lagringstjänst i regioner eller instanser med olika endpoint-suffix som för Azure Kina eller Azure Government, Använd hello följande connection-strängformat.</span><span class="sxs-lookup"><span data-stu-id="fad35-145">toocreate a connection string for a storage service in regions or instances with different endpoint suffixes, such as for Azure China or Azure Government, use hello following connection string format.</span></span> <span data-ttu-id="fad35-146">Ange om du vill tooconnect toohello storage-konto via HTTPS (rekommenderas) eller HTTP ersätter `myAccountName` med hello namn för ditt lagringskonto, Ersätt `myAccountKey` med din åtkomstnyckel och Ersätt `mySuffix` med hello URI suffix:</span><span class="sxs-lookup"><span data-stu-id="fad35-146">Indicate whether you want tooconnect toohello storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with hello name of your storage account, replace `myAccountKey` with your account access key, and replace `mySuffix` with hello URI suffix:</span></span>

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

<span data-ttu-id="fad35-147">Här är en exempel-anslutningssträng för storage-tjänster i Azure Kina:</span><span class="sxs-lookup"><span data-stu-id="fad35-147">Here's an example connection string for storage services in Azure China:</span></span>

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a><span data-ttu-id="fad35-148">Parsning av en anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="fad35-148">Parsing a connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a><span data-ttu-id="fad35-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fad35-149">Next steps</span></span>
* [<span data-ttu-id="fad35-150">Använd hello Azure storage-emulatorn för utveckling och testning</span><span class="sxs-lookup"><span data-stu-id="fad35-150">Use hello Azure storage emulator for development and testing</span></span>](storage-use-emulator.md)
* [<span data-ttu-id="fad35-151">Azure lagringsutforskare</span><span class="sxs-lookup"><span data-stu-id="fad35-151">Azure Storage explorers</span></span>](storage-explorers.md)
* [<span data-ttu-id="fad35-152">Använda signaturer för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="fad35-152">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)

