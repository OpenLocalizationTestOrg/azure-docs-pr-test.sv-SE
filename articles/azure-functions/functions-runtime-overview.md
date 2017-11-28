---
title: "aaaAzure översikt över Functions-Runtime | Microsoft Docs"
description: "Översikt över hello Azure Functions Runtime Preview"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 8ce3e5037731d499c330b395c89c90109d18d65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-runtime-overview"></a><span data-ttu-id="34a43-103">Översikt över Azure Functions-Runtime</span><span class="sxs-lookup"><span data-stu-id="34a43-103">Azure Functions Runtime Overview</span></span>

<span data-ttu-id="34a43-104">hello Azure Functions-Runtime är en ny metod för du tootake utnyttja hello enkelhet och flexibilitet i hello Azure Functions programming modellen på lokalt.</span><span class="sxs-lookup"><span data-stu-id="34a43-104">hello Azure Functions Runtime provides a new way for you tootake advantage of hello simplicity and flexibility of hello Azure Functions programming model on-premises.</span></span> <span data-ttu-id="34a43-105">Bygger på hello öppna samma källa rötter eftersom Azure Functions, Azure Functions-Runtime är distribuerade lokalt tooprovide nästan identisk development upplevelse som hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="34a43-105">Built on hello same open source roots as Azure Functions, Azure Functions Runtime is deployed on-premises tooprovide a nearly identical development experience as hello cloud service.</span></span>

![Azure Functions-Runtime Preview-portalen][1]

<span data-ttu-id="34a43-107">hello Azure Functions-Runtime kan du tooexperience Azure Functions innan du genomför toohello moln.</span><span class="sxs-lookup"><span data-stu-id="34a43-107">hello Azure Functions Runtime provides a way for you tooexperience Azure Functions before committing toohello cloud.</span></span> <span data-ttu-id="34a43-108">På så sätt kan kan hello kod tillgångar som du skapar sedan tas toohello moln när du migrerar.</span><span class="sxs-lookup"><span data-stu-id="34a43-108">In this way, hello code assets you build can then be taken with you toohello cloud when you migrate.</span></span>  <span data-ttu-id="34a43-109">hello runtime öppnas också nya alternativ för dig, till exempel med hjälp av ledig datorkraft som hello lokala datorer toorun batch processer över natten.</span><span class="sxs-lookup"><span data-stu-id="34a43-109">hello runtime also opens up new options for you, such as using hello spare compute power of your on-premises computers toorun batch processes overnight.</span></span> <span data-ttu-id="34a43-110">Du kan också använda enheter inom din organisation tooconditionally skicka data tooother system, både lokalt och i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="34a43-110">You can also use devices within your organization tooconditionally send data tooother systems, both on-premises and in hello cloud.</span></span>

<span data-ttu-id="34a43-111">hello Azure Functions-Runtime består av två delar:</span><span class="sxs-lookup"><span data-stu-id="34a43-111">hello Azure Functions Runtime consists of two pieces:</span></span>
* <span data-ttu-id="34a43-112">Azure Functions-Runtime Ledningsroll</span><span class="sxs-lookup"><span data-stu-id="34a43-112">Azure Functions Runtime Management Role</span></span>
* <span data-ttu-id="34a43-113">Azure Functions-Runtime Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="34a43-113">Azure Functions Runtime Worker Role</span></span>

## <a name="azure-functions-management-role"></a><span data-ttu-id="34a43-114">Azure Functions roll</span><span class="sxs-lookup"><span data-stu-id="34a43-114">Azure Functions Management Role</span></span>

<span data-ttu-id="34a43-115">hello Azure Functions Ledningsroll innehåller en värd för hello hanteringen av dina funktioner lokala.</span><span class="sxs-lookup"><span data-stu-id="34a43-115">hello Azure Functions Management Role provides a host for hello management of your Functions on-premises.</span></span> <span data-ttu-id="34a43-116">Den här rollen utför hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="34a43-116">This role performs hello following tasks:</span></span>

* <span data-ttu-id="34a43-117">Att vara värd för hello Azure Functions-hanteringsportalen som är hello hello samma som visas i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="34a43-117">Hosting of hello Azure Functions Management Portal, which is hello hello same one you see in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="34a43-118">Det här kan du utveckla dina funktioner i hello samma sätt som i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="34a43-118">This lets you develop your functions in hello same way as you would in hello Azure portal.</span></span>
* <span data-ttu-id="34a43-119">Distribuera funktioner på flera funktioner arbetare.</span><span class="sxs-lookup"><span data-stu-id="34a43-119">Distributing functions across multiple Functions workers.</span></span>
* <span data-ttu-id="34a43-120">Tillhandahåller en publishing slutpunkt så att du kan publicera dina funktioner direkt från Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34a43-120">Providing a publishing endpoint so that you can publish your functions direct from Microsoft Visual Studio.</span></span>

## <a name="azure-functions-worker-role"></a><span data-ttu-id="34a43-121">Azure Functions Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="34a43-121">Azure Functions Worker Role</span></span>

<span data-ttu-id="34a43-122">hello Azure Functions arbetsroller distribueras i Windows-behållare och det är där Funktionskoden körs.</span><span class="sxs-lookup"><span data-stu-id="34a43-122">hello Azure Functions Worker Roles are deployed in Windows Containers and this is where your function code executes.</span></span>  <span data-ttu-id="34a43-123">Du kan distribuera flera arbetsroller i organisationen och är viktiga sätt där kunder kan göra att använda ledig datorkraft.</span><span class="sxs-lookup"><span data-stu-id="34a43-123">You can deploy multiple Worker Roles throughout your organization and is a key way in which customers can make use of spare compute power.</span></span>

## <a name="minimum-requirements"></a><span data-ttu-id="34a43-124">Minimikrav</span><span class="sxs-lookup"><span data-stu-id="34a43-124">Minimum Requirements</span></span>

<span data-ttu-id="34a43-125">tooget igång med hello Azure Functions-Runtime måste du ha en dator med **Windows Server 2016 eller Windows 10 skapare Update** med åtkomst tooa **SQL Server** instans.</span><span class="sxs-lookup"><span data-stu-id="34a43-125">tooget started with hello Azure Functions Runtime you must have a machine with **Windows Server 2016 or Windows 10 Creators Update** with access tooa **SQL Server** instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34a43-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34a43-126">Next Steps</span></span>

<span data-ttu-id="34a43-127">Installera hello [Azure Functions-Runtime preview](https://aka.ms/azafr)</span><span class="sxs-lookup"><span data-stu-id="34a43-127">Install hello [Azure Functions Runtime preview](https://aka.ms/azafr)</span></span>

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
