---
title: "Runtime översikt över Azure Functions | Microsoft Docs"
description: "Översikt över förhandsversionen av Azure Functions-Runtime"
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
ms.openlocfilehash: cb98d5f2aaa526555820c15ba5a32fb7e78ffc5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-runtime-overview"></a><span data-ttu-id="8fbe5-103">Översikt över Azure Functions-Runtime</span><span class="sxs-lookup"><span data-stu-id="8fbe5-103">Azure Functions Runtime Overview</span></span>

<span data-ttu-id="8fbe5-104">Azure Functions-Runtime är en ny metod att dra nytta av enkelheten och flexibilitet i Azure Functions programming modellen på lokalt.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-104">The Azure Functions Runtime provides a new way for you to take advantage of the simplicity and flexibility of the Azure Functions programming model on-premises.</span></span> <span data-ttu-id="8fbe5-105">Azure Functions-Runtime är bygger på öppen källkod samma rot som Azure Functions distribuerade lokalt att ge en nästan identisk utvecklingsarbetet som Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-105">Built on the same open source roots as Azure Functions, Azure Functions Runtime is deployed on-premises to provide a nearly identical development experience as the cloud service.</span></span>

![Azure Functions-Runtime Preview-portalen][1]

<span data-ttu-id="8fbe5-107">Azure Functions-Runtime kan du uppleva Azure Functions innan du genomför till molnet.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-107">The Azure Functions Runtime provides a way for you to experience Azure Functions before committing to the cloud.</span></span> <span data-ttu-id="8fbe5-108">På så sätt kan kan kod tillgångarna som du skapar sedan tas med dig till molnet när du migrerar.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-108">In this way, the code assets you build can then be taken with you to the cloud when you migrate.</span></span>  <span data-ttu-id="8fbe5-109">Körningen öppnas också nya alternativ för dig, som att använda ledig datorkraft lokala datorer för att köra batchprocesser över natten.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-109">The runtime also opens up new options for you, such as using the spare compute power of your on-premises computers to run batch processes overnight.</span></span> <span data-ttu-id="8fbe5-110">Du kan också använda enheter inom organisationen villkorligt skicka data till andra system, både lokalt och i molnet.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-110">You can also use devices within your organization to conditionally send data to other systems, both on-premises and in the cloud.</span></span>

<span data-ttu-id="8fbe5-111">Azure Functions-Runtime består av två delar:</span><span class="sxs-lookup"><span data-stu-id="8fbe5-111">The Azure Functions Runtime consists of two pieces:</span></span>
* <span data-ttu-id="8fbe5-112">Azure Functions-Runtime Ledningsroll</span><span class="sxs-lookup"><span data-stu-id="8fbe5-112">Azure Functions Runtime Management Role</span></span>
* <span data-ttu-id="8fbe5-113">Azure Functions-Runtime Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="8fbe5-113">Azure Functions Runtime Worker Role</span></span>

## <a name="azure-functions-management-role"></a><span data-ttu-id="8fbe5-114">Azure Functions roll</span><span class="sxs-lookup"><span data-stu-id="8fbe5-114">Azure Functions Management Role</span></span>

<span data-ttu-id="8fbe5-115">Azure Functions Management rollen ger en värd för hanteringen av dina funktioner lokala.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-115">The Azure Functions Management Role provides a host for the management of your Functions on-premises.</span></span> <span data-ttu-id="8fbe5-116">Den här rollen utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8fbe5-116">This role performs the following tasks:</span></span>

* <span data-ttu-id="8fbe5-117">Att vara värd för Azure Functions hanteringsportalen, vilket är det samma som visas i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8fbe5-117">Hosting of the Azure Functions Management Portal, which is the the same one you see in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8fbe5-118">På så sätt kan du utveckla dina funktioner på samma sätt som i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-118">This lets you develop your functions in the same way as you would in the Azure portal.</span></span>
* <span data-ttu-id="8fbe5-119">Distribuera funktioner på flera funktioner arbetare.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-119">Distributing functions across multiple Functions workers.</span></span>
* <span data-ttu-id="8fbe5-120">Tillhandahåller en publishing slutpunkt så att du kan publicera dina funktioner direkt från Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-120">Providing a publishing endpoint so that you can publish your functions direct from Microsoft Visual Studio.</span></span>

## <a name="azure-functions-worker-role"></a><span data-ttu-id="8fbe5-121">Azure Functions Worker-rollen</span><span class="sxs-lookup"><span data-stu-id="8fbe5-121">Azure Functions Worker Role</span></span>

<span data-ttu-id="8fbe5-122">Azure Functions-arbetsroller distribueras i Windows-behållare och det är där Funktionskoden körs.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-122">The Azure Functions Worker Roles are deployed in Windows Containers and this is where your function code executes.</span></span>  <span data-ttu-id="8fbe5-123">Du kan distribuera flera arbetsroller i organisationen och är viktiga sätt där kunder kan göra att använda ledig datorkraft.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-123">You can deploy multiple Worker Roles throughout your organization and is a key way in which customers can make use of spare compute power.</span></span>

## <a name="minimum-requirements"></a><span data-ttu-id="8fbe5-124">Minimikrav</span><span class="sxs-lookup"><span data-stu-id="8fbe5-124">Minimum Requirements</span></span>

<span data-ttu-id="8fbe5-125">Du måste ha en dator med att komma igång med Azure Functions-Runtime **Windows Server 2016 eller Windows 10 skapare Update** med åtkomst till en **SQL Server** instans.</span><span class="sxs-lookup"><span data-stu-id="8fbe5-125">To get started with the Azure Functions Runtime you must have a machine with **Windows Server 2016 or Windows 10 Creators Update** with access to a **SQL Server** instance.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8fbe5-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fbe5-126">Next Steps</span></span>

<span data-ttu-id="8fbe5-127">Installera den [Azure Functions-Runtime preview](https://aka.ms/azafr)</span><span class="sxs-lookup"><span data-stu-id="8fbe5-127">Install the [Azure Functions Runtime preview](https://aka.ms/azafr)</span></span>

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
