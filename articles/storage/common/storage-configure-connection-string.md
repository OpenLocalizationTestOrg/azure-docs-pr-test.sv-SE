---
title: "Konfigurera en anslutningssträng för Azure Storage | Microsoft Docs"
description: "Konfigurera en anslutningssträng för ett Azure storage-konto. En anslutningssträng innehåller den information som krävs för att autentisera åtkomst till ett lagringskonto från ditt program vid körning."
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
ms.openlocfilehash: 4b21e75fde55f195362809ce486a2615954ff93c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-storage-connection-strings"></a><span data-ttu-id="aa0b0-104">Konfigurera Azure Storage-anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="aa0b0-104">Configure Azure Storage connection strings</span></span>

<span data-ttu-id="aa0b0-105">En anslutningssträng innehåller autentiseringsinformation som krävs för ditt program för att komma åt data i Azure Storage-konto vid körning.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-105">A connection string includes the authentication information required for your application to access data in an Azure Storage account at runtime.</span></span> <span data-ttu-id="aa0b0-106">Du kan konfigurera anslutningssträngar för att:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-106">You can configure connection strings to:</span></span>

* <span data-ttu-id="aa0b0-107">Ansluta till Azure storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-107">Connect to the Azure storage emulator.</span></span>
* <span data-ttu-id="aa0b0-108">Åtkomst till ett lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-108">Access a storage account in Azure.</span></span>
* <span data-ttu-id="aa0b0-109">Åtkomst till angivna resurser i Azure via en signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="aa0b0-109">Access specified resources in Azure via a shared access signature (SAS).</span></span>

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a><span data-ttu-id="aa0b0-110">Lagra anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="aa0b0-110">Storing your connection string</span></span>
<span data-ttu-id="aa0b0-111">Ditt program behöver åtkomst till anslutningssträngen vid körningstillfället att autentisera begäranden till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-111">Your application needs to access the connection string at runtime to authenticate requests made to Azure Storage.</span></span> <span data-ttu-id="aa0b0-112">Du har flera alternativ för att lagra anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-112">You have several options for storing your connection string:</span></span>

* <span data-ttu-id="aa0b0-113">Ett program som körs på skrivbordet eller på en enhet kan lagra anslutningssträngen i en **app.config** eller **web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-113">An application running on the desktop or on a device can store the connection string in an **app.config** or **web.config** file.</span></span> <span data-ttu-id="aa0b0-114">Lägg till anslutningssträngen till den **AppSettings** avsnitt i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-114">Add the connection string to the **AppSettings** section in these files.</span></span>
* <span data-ttu-id="aa0b0-115">Ett program som körs i en Azure-molntjänst kan lagra anslutningssträngen i den [Azure-tjänstkonfigurationsfil schema (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa0b0-115">An application running in an Azure cloud service can store the connection string in the [Azure service configuration schema (.cscfg) file](https://msdn.microsoft.com/library/ee758710.aspx).</span></span> <span data-ttu-id="aa0b0-116">Lägg till anslutningssträngen till den **ConfigurationSettings** i tjänstkonfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-116">Add the connection string to the **ConfigurationSettings** section of the service configuration file.</span></span>
* <span data-ttu-id="aa0b0-117">Du kan använda anslutningssträngen direkt i din kod.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-117">You can use your connection string directly in your code.</span></span> <span data-ttu-id="aa0b0-118">Vi rekommenderar dock att du lagrar anslutningssträngen i en konfigurationsfil i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-118">However, we recommend that you store your connection string in a configuration file in most scenarios.</span></span>

<span data-ttu-id="aa0b0-119">Spara anslutningssträngen i en konfigurationsfil gör det enkelt att uppdatera anslutningssträngen att växla mellan storage-emulatorn och ett Azure storage-konto i molnet.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-119">Storing your connection string in a configuration file makes it easy to update the connection string to switch between the storage emulator and an Azure storage account in the cloud.</span></span> <span data-ttu-id="aa0b0-120">Behöver du bara redigera anslutningssträng att peka målmiljön.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-120">You only need to edit the connection string to point to your target environment.</span></span>

<span data-ttu-id="aa0b0-121">Du kan använda den [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) att komma åt anslutningssträngen vid körningstillfället oavsett där programmet körs.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-121">You can use the [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) to access your connection string at runtime regardless of where your application is running.</span></span>

## <a name="create-a-connection-string-for-the-storage-emulator"></a><span data-ttu-id="aa0b0-122">Skapa en anslutningssträng för storage-emulatorn</span><span class="sxs-lookup"><span data-stu-id="aa0b0-122">Create a connection string for the storage emulator</span></span>
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

<span data-ttu-id="aa0b0-123">Mer information om storage-emulatorn finns [Använd Azure storage-emulatorn för utveckling och testning](storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="aa0b0-123">For more information about the storage emulator, see [Use the Azure storage emulator for development and testing](storage-use-emulator.md).</span></span>

## <a name="create-a-connection-string-for-an-azure-storage-account"></a><span data-ttu-id="aa0b0-124">Skapa en anslutningssträng för ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="aa0b0-124">Create a connection string for an Azure storage account</span></span>
<span data-ttu-id="aa0b0-125">Använd följande format för att skapa en anslutningssträng för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-125">To create a connection string for your Azure storage account, use the following format.</span></span> <span data-ttu-id="aa0b0-126">Ange om du vill ansluta till storage-konto via HTTPS (rekommenderas) eller HTTP ersätter `myAccountName` med namnet på ditt lagringskonto och Ersätt `myAccountKey` med din åtkomstnyckel:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-126">Indicate whether you want to connect to the storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with the name of your storage account, and replace `myAccountKey` with your account access key:</span></span>

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

<span data-ttu-id="aa0b0-127">Anslutningssträngen kan till exempel se ut:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-127">For example, your connection string might look similar to:</span></span>

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

<span data-ttu-id="aa0b0-128">Azure Storage har stöd för både HTTP och HTTPS i en anslutningssträng *HTTPS rekommenderas*.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-128">Although Azure Storage supports both HTTP and HTTPS in a connection string, *HTTPS is highly recommended*.</span></span>

> [!TIP]
> <span data-ttu-id="aa0b0-129">Du kan hitta ditt lagringskonto anslutningssträngar i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa0b0-129">You can find your storage account's connection strings in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="aa0b0-130">Gå till **inställningar** > **åtkomstnycklar** i ditt lagringskonto menyn bladet för att visa anslutningssträngar för båda primära och sekundära nycklarna.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-130">Navigate to **SETTINGS** > **Access keys** in your storage account's menu blade to see connection strings for both primary and secondary access keys.</span></span>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a><span data-ttu-id="aa0b0-131">Skapa en anslutningssträng med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="aa0b0-131">Create a connection string using a shared access signature</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a><span data-ttu-id="aa0b0-132">Skapa en anslutningssträng för en explicit lagring-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="aa0b0-132">Create a connection string for an explicit storage endpoint</span></span>
<span data-ttu-id="aa0b0-133">Du kan ange Tjänsteslutpunkter explicit i anslutningssträngen i stället för standard-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-133">You can specify explicit service endpoints in your connection string instead of using the default endpoints.</span></span> <span data-ttu-id="aa0b0-134">Ange den fullständiga tjänstslutpunkten för varje tjänst, inklusive protokollspecifikation (HTTPS (rekommenderas) eller HTTP) i följande format för att skapa en anslutningssträng som anger en explicit slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-134">To create a connection string that specifies an explicit endpoint, specify the complete service endpoint for each service, including the protocol specification (HTTPS (recommended) or HTTP), in the following format:</span></span>

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

<span data-ttu-id="aa0b0-135">Ett scenario där du kanske vill ange en explicit slutpunkt är när du har anslutit din slutpunkt för Blob-lagring till en [domänen](../blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="aa0b0-135">One scenario where you might wish to specify an explicit endpoint is when you've mapped your Blob storage endpoint to a [custom domain](../blobs/storage-custom-domain-name.md).</span></span> <span data-ttu-id="aa0b0-136">I så fall kan du ange din anpassade slutpunkt för Blob storage i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-136">In that case, you can specify your custom endpoint for Blob storage in your connection string.</span></span> <span data-ttu-id="aa0b0-137">Du kan också ange standardslutpunkterna för andra tjänster om programmet använder dem.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-137">You can optionally specify the default endpoints for the other services if your application uses them.</span></span>

<span data-ttu-id="aa0b0-138">Här är ett exempel på en anslutningssträng som anger en explicit slutpunkt för Blob-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-138">Here is an example of a connection string that specifies an explicit endpoint for the Blob service:</span></span>

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

<span data-ttu-id="aa0b0-139">Det här exemplet anger explicit slutpunkter för alla tjänster, inklusive en anpassad domän för Blob-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-139">This example specifies explicit endpoints for all services, including a custom domain for the Blob service:</span></span>

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

<span data-ttu-id="aa0b0-140">Slutpunkten värdena i en anslutningssträng används för att konstruera begäran URI: er till storage-tjänster och styr form av alla URI: er som returneras till din kod.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-140">The endpoint values in a connection string are used to construct the request URIs to the storage services, and dictate the form of any URIs that are returned to your code.</span></span>

<span data-ttu-id="aa0b0-141">Om du har anslutit en slutpunkt för lagring till en anpassad domän och utesluter den slutpunkten från en anslutningssträng kommer sedan du inte att kunna använda den anslutningssträngen att komma åt data i tjänsten från din kod.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-141">If you've mapped a storage endpoint to a custom domain and omit that endpoint from a connection string, then you will not be able to use that connection string to access data in that service from your code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa0b0-142">Tjänsten endpoint värdena i anslutningssträngarna måste vara giltig URI: er, inklusive `https://` (rekommenderas) eller `http://`.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-142">Service endpoint values in your connection strings must be well-formed URIs, including `https://` (recommended) or `http://`.</span></span> <span data-ttu-id="aa0b0-143">Eftersom Azure Storage inte stöder ännu HTTPS för anpassade domäner du *måste* ange `http://` för valfri slutpunkt URI som pekar på en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-143">Because Azure Storage does not yet support HTTPS for custom domains, you *must* specify `http://` for any endpoint URI that points to a custom domain.</span></span>
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a><span data-ttu-id="aa0b0-144">Skapa en anslutningssträng med en slutpunkt-suffix</span><span class="sxs-lookup"><span data-stu-id="aa0b0-144">Create a connection string with an endpoint suffix</span></span>
<span data-ttu-id="aa0b0-145">Om du vill skapa en anslutningssträng för en lagringstjänst i regioner eller instanser med olika endpoint suffix, till exempel för Azure Kina eller Azure Government använder du följande connection-strängformat.</span><span class="sxs-lookup"><span data-stu-id="aa0b0-145">To create a connection string for a storage service in regions or instances with different endpoint suffixes, such as for Azure China or Azure Government, use the following connection string format.</span></span> <span data-ttu-id="aa0b0-146">Ange om du vill ansluta till storage-konto via HTTPS (rekommenderas) eller HTTP ersätter `myAccountName` med namnet på ditt lagringskonto, Ersätt `myAccountKey` med din åtkomstnyckel och Ersätt `mySuffix` med suffixet URI:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-146">Indicate whether you want to connect to the storage account through HTTPS (recommended) or HTTP, replace `myAccountName` with the name of your storage account, replace `myAccountKey` with your account access key, and replace `mySuffix` with the URI suffix:</span></span>

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

<span data-ttu-id="aa0b0-147">Här är en exempel-anslutningssträng för storage-tjänster i Azure Kina:</span><span class="sxs-lookup"><span data-stu-id="aa0b0-147">Here's an example connection string for storage services in Azure China:</span></span>

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a><span data-ttu-id="aa0b0-148">Parsning av en anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="aa0b0-148">Parsing a connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a><span data-ttu-id="aa0b0-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa0b0-149">Next steps</span></span>
* [<span data-ttu-id="aa0b0-150">Använd Azure storage-emulatorn för utveckling och testning</span><span class="sxs-lookup"><span data-stu-id="aa0b0-150">Use the Azure storage emulator for development and testing</span></span>](storage-use-emulator.md)
* [<span data-ttu-id="aa0b0-151">Azure lagringsutforskare</span><span class="sxs-lookup"><span data-stu-id="aa0b0-151">Azure Storage explorers</span></span>](storage-explorers.md)
* [<span data-ttu-id="aa0b0-152">Använda signaturer för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="aa0b0-152">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)

