---
title: "aaaAzure Data Factory för utvecklare"
description: "Lär dig mer om olika sätt toocreate, övervaka och hantera Azure datafabriker"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: dc752fa1-a6c3-4753-904e-9f32d0a940b7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: spelluru
redirect_url: https://azure.microsoft.com/services/data-factory/
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: 5384f2209ff0f788527542ddd400c1d163d587c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory-developer-reference"></a><span data-ttu-id="ed25a-103">Azure Data Factory-utvecklarreferens</span><span class="sxs-lookup"><span data-stu-id="ed25a-103">Azure Data Factory Developer Reference</span></span>
<span data-ttu-id="ed25a-104">Du kan skapa, övervaka och hantera hello fabriker med hjälp av Azure-portalen, Azure PowerShell, klassbiblioteket .NET eller REST API.</span><span class="sxs-lookup"><span data-stu-id="ed25a-104">You can create, monitor, and manage hello factories using either Azure portal, Azure PowerShell, .NET Class Library, or REST API.</span></span>

| <span data-ttu-id="ed25a-105">Metod</span><span class="sxs-lookup"><span data-stu-id="ed25a-105">Method</span></span> | <span data-ttu-id="ed25a-106">Resursplats</span><span class="sxs-lookup"><span data-stu-id="ed25a-106">Resource Location</span></span> | <span data-ttu-id="ed25a-107">Referenser för utvecklare</span><span class="sxs-lookup"><span data-stu-id="ed25a-107">Developer References</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ed25a-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ed25a-108">Azure portal</span></span> |[<span data-ttu-id="ed25a-109">https://Portal.Azure.com/</span><span class="sxs-lookup"><span data-stu-id="ed25a-109">https://portal.azure.com/</span></span>](https://portal.azure.com) |[<span data-ttu-id="ed25a-110">Kom igång med Azure Data Factory (Azure portal)</span><span class="sxs-lookup"><span data-stu-id="ed25a-110">Get started with Azure Data Factory (Azure portal)</span></span>](data-factory-build-your-first-pipeline-using-editor.md) |
| <span data-ttu-id="ed25a-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ed25a-111">Azure PowerShell</span></span> |<span data-ttu-id="ed25a-112">Hämta senaste hello [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="ed25a-112">Download hello latest [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409)</span></span> |[<span data-ttu-id="ed25a-113">Cmdlet-referens</span><span class="sxs-lookup"><span data-stu-id="ed25a-113">Cmdlet reference</span></span>](https://msdn.microsoft.com/library/dn820234.aspx) |
| <span data-ttu-id="ed25a-114">.NET-klassbibliotek</span><span class="sxs-lookup"><span data-stu-id="ed25a-114">.NET Class Library</span></span> |<span data-ttu-id="ed25a-115">hello Azure Data Factory .NET SDK aktiverar toocreate, övervaka, och hantera Azure datafabriker och utöka Data Factory med hjälp av en .NET-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ed25a-115">hello Azure Data Factory .NET SDK enables you toocreate, monitor, and manage Azure data factories and extend Data Factory using a .NET activity.</span></span> <span data-ttu-id="ed25a-116">Se [använda anpassade aktiviteter i ett Azure Data Factory-pipelinen](data-factory-use-custom-activities.md) och [skapa, övervaka och hantera Azure datafabriker med Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) artiklar toohelp dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="ed25a-116">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) and [Create, monitor, and manage Azure data factories using Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) articles toohelp you get started.</span></span><br/><br/><span data-ttu-id="ed25a-117"><b>Hämta hello senaste Nuget</b></span><span class="sxs-lookup"><span data-stu-id="ed25a-117"><b>Downloading hello latest Nuget</b></span></span><br/><span data-ttu-id="ed25a-118">Du kan hämta hello senaste Azure Data Factory hantering av bibliotek Nuget-paketet från: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span><span class="sxs-lookup"><span data-stu-id="ed25a-118">You can download hello latest Azure Data Factory Management Library Nuget package from: [https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/)</span></span><br/><br/><span data-ttu-id="ed25a-119">**Med hjälp av Package Manager-konsolen i Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="ed25a-119">**Using Package Manager Console in Visual Studio**</span></span><br/><span data-ttu-id="ed25a-120">Du kan köra hello följande kommando i Visual Studio Package Manager-konsolen tooget hello senaste Azure Data Factory-hanteringsmodul</span><span class="sxs-lookup"><span data-stu-id="ed25a-120">You can run hello following command in Visual Studio’s Package Manager Console tooget hello latest Azure Data Factory Management Library</span></span><br/><br/><span data-ttu-id="ed25a-121">Install-Package Microsoft.Azure.Management.DataFactories</span><span class="sxs-lookup"><span data-stu-id="ed25a-121">Install-Package Microsoft.Azure.Management.DataFactories</span></span> |[<span data-ttu-id="ed25a-122">Referens för .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ed25a-122">.NET SDK Reference</span></span>](https://msdn.microsoft.com/library/mt415893.aspx) |
| <span data-ttu-id="ed25a-123">REST API</span><span class="sxs-lookup"><span data-stu-id="ed25a-123">REST API</span></span> |<span data-ttu-id="ed25a-124">Du kan använda hello Data Factory REST API: et toocreate, övervaka och hantera Azure datafabriker.</span><span class="sxs-lookup"><span data-stu-id="ed25a-124">You can use hello Data Factory REST API toocreate, monitor, and manage Azure data factories.</span></span> |[<span data-ttu-id="ed25a-125">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="ed25a-125">REST API Reference</span></span>](https://msdn.microsoft.com/library/dn906738.aspx) |

