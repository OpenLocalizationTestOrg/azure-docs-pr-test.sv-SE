---
title: "Vad hände med Webbjobb projektet (Visual Studio Azure Storage ansluten service)? | Microsoft Docs"
description: "Beskriver vad som hände i ett projekt för Azure Webjobs när anslutning till ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 3b28ddeadc87937941d60b16fae817e59a220b22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a><span data-ttu-id="49c6a-104">Vad hände med Webbjobb projektet (Visual Studio Azure Storage ansluten service)?</span><span class="sxs-lookup"><span data-stu-id="49c6a-104">What happened to my WebJob project (Visual Studio Azure Storage connected service)?</span></span>
## <a name="references-added"></a><span data-ttu-id="49c6a-105">Referenser som lagts till</span><span class="sxs-lookup"><span data-stu-id="49c6a-105">References Added</span></span>
<span data-ttu-id="49c6a-106">Azure Storage NuGet-paketet har lagts till eller uppdateras i Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="49c6a-106">The Azure Storage NuGet package was added to or updated in your Visual Studio project.</span></span>  
<span data-ttu-id="49c6a-107">Det här paketet lägger du till följande .NET referenser:</span><span class="sxs-lookup"><span data-stu-id="49c6a-107">This package adds the following .NET references:</span></span>

* <span data-ttu-id="49c6a-108">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="49c6a-108">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="49c6a-109">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="49c6a-109">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="49c6a-110">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="49c6a-110">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="49c6a-111">**Microsoft.WindowsAzure.ConfigurationManager**</span><span class="sxs-lookup"><span data-stu-id="49c6a-111">**Microsoft.WindowsAzure.ConfigurationManager**</span></span>
* <span data-ttu-id="49c6a-112">**Microsoft.WindowsAzure.Storage**</span><span class="sxs-lookup"><span data-stu-id="49c6a-112">**Microsoft.WindowsAzure.Storage**</span></span>
* <span data-ttu-id="49c6a-113">**Newtonsoft.Json**</span><span class="sxs-lookup"><span data-stu-id="49c6a-113">**Newtonsoft.Json**</span></span>
* <span data-ttu-id="49c6a-114">**System.Data**</span><span class="sxs-lookup"><span data-stu-id="49c6a-114">**System.Data**</span></span>
* <span data-ttu-id="49c6a-115">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="49c6a-115">**System.Spatial**</span></span>

## <a name="connection-string-for-azure-storage-added"></a><span data-ttu-id="49c6a-116">Anslutningssträngen för Azure Storage som lagts till</span><span class="sxs-lookup"><span data-stu-id="49c6a-116">Connection string for Azure Storage added</span></span>
<span data-ttu-id="49c6a-117">I filen App.config i ditt projekt i **AzureWebJobsStorage** och **AzureWebJobsDashboard** poster har uppdaterats med anslutningssträngen och nyckeln för det valda lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="49c6a-117">In the App.config file of your project, the **AzureWebJobsStorage** and **AzureWebJobsDashboard** entries were updated with the selected storage account's connection string and key.</span></span>

<span data-ttu-id="49c6a-118">Mer information finns i [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="49c6a-118">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

