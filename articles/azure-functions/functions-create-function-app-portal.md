---
title: "aaaCreate en funktionsapp från hello Azure Portal | Microsoft Docs"
description: "Skapa en ny funktionsapp i Azure App Service från hello-portalen."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c531fc71c798edf22e25a5f4b79c15413809dc86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-from-hello-azure-portal"></a><span data-ttu-id="0a2b6-103">Skapa en funktionsapp från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0a2b6-103">Create a function app from hello Azure portal</span></span>

<span data-ttu-id="0a2b6-104">Azure funktionen appar använder hello Azure App Service-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-104">Azure Function Apps uses hello Azure App Service infrastructure.</span></span> <span data-ttu-id="0a2b6-105">Det här avsnittet beskrivs hur du toocreate en funktionsapp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-105">This topic shows you how toocreate a function app in hello Azure portal.</span></span> <span data-ttu-id="0a2b6-106">En funktionsapp är hello-behållare som är värd för hello körningen av enskilda funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-106">A function app is hello container that hosts hello execution of individual functions.</span></span> <span data-ttu-id="0a2b6-107">När du skapar en funktionsapp i hello värd programtjänstplanen kan appen funktionen använda alla hello-funktioner i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-107">When you create a function app in hello App Service hosting plan, your function app can use all hello features of App Service.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="0a2b6-108">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="0a2b6-108">Create a function app</span></span>

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="0a2b6-109">När du skapar en funktionsapp, ange en giltig **appnamn**, som kan innehålla endast bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-109">When you create a function app, supply a valid **App name**, which can contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="0a2b6-110">Understreck (**_**) är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-110">Underscore (**_**) is not an allowed character.</span></span>

<span data-ttu-id="0a2b6-111">Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-111">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="0a2b6-112">Namnet på ditt lagringskonto måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-112">Your storage account name must be unique within Azure.</span></span> 

<span data-ttu-id="0a2b6-113">När hello funktionsapp har skapats kan skapa du enskilda funktioner i en eller flera olika språk.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-113">After hello function app is created, you can create individual functions in one or more different languages.</span></span> <span data-ttu-id="0a2b6-114">Skapa funktioner [med hjälp av hello portal](functions-create-first-azure-function.md#create-function), [kontinuerlig distribution](functions-continuous-deployment.md), eller av [Överför med FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span><span class="sxs-lookup"><span data-stu-id="0a2b6-114">Create functions [by using hello portal](functions-create-first-azure-function.md#create-function), [continuous deployment](functions-continuous-deployment.md), or by [uploading with FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span></span>

## <a name="service-plans"></a><span data-ttu-id="0a2b6-115">Service-planer</span><span class="sxs-lookup"><span data-stu-id="0a2b6-115">Service plans</span></span>

<span data-ttu-id="0a2b6-116">Azure Functions har två olika serviceplaner: förbrukning planerings- och App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-116">Azure Functions has two different service plans: Consumption plan and App Service plan.</span></span> <span data-ttu-id="0a2b6-117">hello förbrukning plan tilldelar automatiskt datorkraft när koden körs, skala ut som behövs toohandle belastning och skalar sedan i när koden inte körs.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-117">hello Consumption plan automatically allocates compute power when your code is running, scales-out as necessary toohandle load, and then scales-in when code is not running.</span></span> <span data-ttu-id="0a2b6-118">Hej programtjänstplanen ger din funktion app tooall hello lokaler av App Service.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-118">hello App Service plan gives your function app access tooall hello facilities of App Service.</span></span> <span data-ttu-id="0a2b6-119">Du måste välja serviceplanen när funktionen appen skapas och den för närvarande inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-119">You must choose your service plan when your function app is created, and it cannot currently be changed.</span></span> <span data-ttu-id="0a2b6-120">Mer information finns i [väljer du ett Azure-funktioner som värd för planen](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="0a2b6-120">For more information, see [Choose an Azure Functions hosting plan](functions-scale.md).</span></span>

<span data-ttu-id="0a2b6-121">Om du planerar toorun JavaScript-funktioner på en apptjänstplan, bör du välja en plan med färre kärnor.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-121">If you are planning toorun JavaScript functions on an App Service plan, you should choose a plan with fewer cores.</span></span> <span data-ttu-id="0a2b6-122">Mer information finns i hello [JavaScript-referens för funktioner](functions-reference-node.md#choose-single-core-app-service-plans).</span><span class="sxs-lookup"><span data-stu-id="0a2b6-122">For more information, see hello [JavaScript reference for Functions](functions-reference-node.md#choose-single-core-app-service-plans).</span></span>

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a><span data-ttu-id="0a2b6-123">Lagringskraven för kontot</span><span class="sxs-lookup"><span data-stu-id="0a2b6-123">Storage account requirements</span></span>

<span data-ttu-id="0a2b6-124">När du skapar en funktionsapp i App Service, måste du skapa eller länka tooa allmänna Azure Storage-konto som har stöd för lagring av Blob, köer och tabellen.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-124">When creating a function app in App Service, you must create or link tooa general-purpose Azure Storage account that supports Blob, Queue, and Table storage.</span></span> <span data-ttu-id="0a2b6-125">Internt använder funktioner lagring för åtgärder som hanterar utlösare och loggning funktionen körningar.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-125">Internally, Functions uses Storage for operations such as managing triggers and logging function executions.</span></span> <span data-ttu-id="0a2b6-126">Vissa storage-konton stöder inte köer och tabeller som endast blob storage-konton, Azure Premium-lagring och allmänna lagringskonton med ZRS replikering.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-126">Some storage accounts do not support queues and tables, such as blob-only storage accounts, Azure Premium Storage, and general-purpose storage accounts with ZRS replication.</span></span> <span data-ttu-id="0a2b6-127">Dessa konton filtreras av från hello-bladet Storage-konto när du skapar en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-127">These accounts are filtered out of from hello Storage Account blade when creating a function app.</span></span>

>[!NOTE]
><span data-ttu-id="0a2b6-128">När du använder hello förbrukning värd plan lagras konfigurationsfilerna funktionen kod och bindningen i Azure File storage i hello huvudsakliga storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-128">When using hello Consumption hosting plan, your function code and binding configuration files are stored in Azure File storage in hello main storage account.</span></span> <span data-ttu-id="0a2b6-129">När du tar bort hello huvudsakliga storage-konto raderas innehållet och kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="0a2b6-129">When you delete hello main storage account, this content is deleted and cannot be recovered.</span></span>

<span data-ttu-id="0a2b6-130">toolearn mer om lagringskontotyper finns [introduktion till hello Azure Storage-tjänster](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span><span class="sxs-lookup"><span data-stu-id="0a2b6-130">toolearn more about storage account types, see [Introducing hello Azure Storage Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0a2b6-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a2b6-131">Next steps</span></span>

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



