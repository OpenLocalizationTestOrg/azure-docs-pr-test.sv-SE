---
title: "Vad hände med min 5 för ASP.NET-projekt (Visual Studio ansluten tjänster) | Microsoft Docs"
description: "Beskriver vad som händer när du ansluter till ett Azure storage-konto i Visual Studio ASP.NET 5-projekt med Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4390993772eaf35516e48ad7adcdcec5f1df8d71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a><span data-ttu-id="6ab8a-103">Vad hände med min 5 för ASP.NET-projekt (Visual Studio Azure Storage ansluten services)?</span><span class="sxs-lookup"><span data-stu-id="6ab8a-103">What happened to my ASP.NET 5 project (Visual Studio Azure Storage connected services)?</span></span>
## <a name="references-added"></a><span data-ttu-id="6ab8a-104">Referenser som lagts till</span><span class="sxs-lookup"><span data-stu-id="6ab8a-104">References added</span></span>
<span data-ttu-id="6ab8a-105">Azure Storage NuGet-paketet har lagts till Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="6ab8a-105">The Azure Storage NuGet package was added to your Visual Studio project.</span></span>  
<span data-ttu-id="6ab8a-106">Det här paketet lägger du till följande .NET referenser:</span><span class="sxs-lookup"><span data-stu-id="6ab8a-106">This package adds the following .NET references:</span></span>

* <span data-ttu-id="6ab8a-107">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-107">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="6ab8a-108">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-108">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="6ab8a-109">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-109">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="6ab8a-110">**Microsoft.WindowsAzure.Configuration**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-110">**Microsoft.WindowsAzure.Configuration**</span></span>
* <span data-ttu-id="6ab8a-111">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-111">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="6ab8a-112">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-112">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="6ab8a-113">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-113">**System.Data**</span></span>
* <span data-ttu-id="6ab8a-114">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="6ab8a-114">**System.Spatial**</span></span>

<span data-ttu-id="6ab8a-115">Dessutom NuGet-paketet **Microsoft.Framework.Configuration.Json** har lagts till.</span><span class="sxs-lookup"><span data-stu-id="6ab8a-115">Also, the NuGet package **Microsoft.Framework.Configuration.Json** was added.</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="6ab8a-116">Anslutningssträngen för Azure Storage som lagts till</span><span class="sxs-lookup"><span data-stu-id="6ab8a-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="6ab8a-117">Ett element har skapats med det valda lagringskontot anslutningssträngen och nyckel i filen config.json för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="6ab8a-117">In the config.json file of your project, an element was created with the selected storage account's connection string and key.</span></span>

<span data-ttu-id="6ab8a-118">Mer information finns i [ASP.NET 5](http://www.asp.net/vnext).</span><span class="sxs-lookup"><span data-stu-id="6ab8a-118">For more information, see [ASP.NET 5](http://www.asp.net/vnext).</span></span>

