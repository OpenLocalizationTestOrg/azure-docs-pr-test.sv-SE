---
title: "Vad hände med min molntjänstprojekt? | Microsoft Docs"
description: "Beskriver vad som händer i ett projekt för cloud services när du ansluter till ett Azure storage-konto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4e0d4864c2fad624fbde39080146dc62ebebff09
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="f652d-104">Vad hände med min cloud services-projekt (Visual Studio Azure Storage ansluten service)?</span><span class="sxs-lookup"><span data-stu-id="f652d-104">What happened to my cloud services project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="f652d-105">Referenser som lagts till</span><span class="sxs-lookup"><span data-stu-id="f652d-105">References added</span></span>
<span data-ttu-id="f652d-106">Azure Storage NuGet-paketet har lagts till Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="f652d-106">The Azure Storage NuGet package was added to your Visual Studio project.</span></span>  
<span data-ttu-id="f652d-107">Det här paketet lägger du till följande .NET referenser:</span><span class="sxs-lookup"><span data-stu-id="f652d-107">This package adds the following .NET references:</span></span>

* <span data-ttu-id="f652d-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="f652d-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="f652d-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="f652d-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="f652d-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="f652d-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="f652d-111">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="f652d-111">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="f652d-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="f652d-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="f652d-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="f652d-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="f652d-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="f652d-114">**System.Data**</span></span>
* <span data-ttu-id="f652d-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="f652d-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="f652d-116">Anslutningssträngen för Azure Storage som lagts till</span><span class="sxs-lookup"><span data-stu-id="f652d-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="f652d-117">Element har skapats med anslutningssträngen och nyckeln för det valda lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f652d-117">Elements were created with the selected storage account's connection string and key.</span></span> <span data-ttu-id="f652d-118">Ändringar har gjorts till följande filer:</span><span class="sxs-lookup"><span data-stu-id="f652d-118">Modifications were made to the following files:</span></span>

* <span data-ttu-id="f652d-119">**ServiceDefinition.csdef**</span><span class="sxs-lookup"><span data-stu-id="f652d-119">**ServiceDefinition.csdef**</span></span>
* <span data-ttu-id="f652d-120">**ServiceConfiguration.Cloud.cscfg**</span><span class="sxs-lookup"><span data-stu-id="f652d-120">**ServiceConfiguration.Cloud.cscfg**</span></span>
* <span data-ttu-id="f652d-121">**ServiceConfiguration.Local.cscfg**</span><span class="sxs-lookup"><span data-stu-id="f652d-121">**ServiceConfiguration.Local.cscfg**</span></span>

