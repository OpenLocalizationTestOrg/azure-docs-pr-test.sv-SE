---
title: "Vad hände med min molntjänstprojekt? | Microsoft Docs"
description: "Beskriver vad som händer i ett projekt för cloud services när du ansluter till ett Azure storage-konto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 4c9de56f6daf07097c0f593db37d14dce3bce05f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="9f719-104">Vad hände med min cloud services-projekt (Visual Studio Azure Storage ansluten service)?</span><span class="sxs-lookup"><span data-stu-id="9f719-104">What happened to my cloud services project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="9f719-105">Referenser som lagts till</span><span class="sxs-lookup"><span data-stu-id="9f719-105">References added</span></span>
<span data-ttu-id="9f719-106">Azure Storage NuGet-paketet har lagts till Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="9f719-106">The Azure Storage NuGet package was added to your Visual Studio project.</span></span>  
<span data-ttu-id="9f719-107">Det här paketet lägger du till följande .NET referenser:</span><span class="sxs-lookup"><span data-stu-id="9f719-107">This package adds the following .NET references:</span></span>

* <span data-ttu-id="9f719-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="9f719-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="9f719-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="9f719-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="9f719-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="9f719-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="9f719-111">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="9f719-111">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="9f719-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="9f719-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="9f719-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="9f719-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="9f719-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="9f719-114">**System.Data**</span></span>
* <span data-ttu-id="9f719-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="9f719-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="9f719-116">Anslutningssträngen för Azure Storage som lagts till</span><span class="sxs-lookup"><span data-stu-id="9f719-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="9f719-117">Element har skapats med anslutningssträngen och nyckeln för det valda lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="9f719-117">Elements were created with the selected storage account's connection string and key.</span></span> <span data-ttu-id="9f719-118">Ändringar har gjorts till följande filer:</span><span class="sxs-lookup"><span data-stu-id="9f719-118">Modifications were made to the following files:</span></span>

* <span data-ttu-id="9f719-119">**ServiceDefinition.csdef**</span><span class="sxs-lookup"><span data-stu-id="9f719-119">**ServiceDefinition.csdef**</span></span>
* <span data-ttu-id="9f719-120">**ServiceConfiguration.Cloud.cscfg**</span><span class="sxs-lookup"><span data-stu-id="9f719-120">**ServiceConfiguration.Cloud.cscfg**</span></span>
* <span data-ttu-id="9f719-121">**ServiceConfiguration.Local.cscfg**</span><span class="sxs-lookup"><span data-stu-id="9f719-121">**ServiceConfiguration.Local.cscfg**</span></span>

