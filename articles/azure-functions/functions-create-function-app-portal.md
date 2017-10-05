---
title: "Skapa en funktionsapp från Azure Portal | Microsoft Docs"
description: "Skapa en ny funktionsapp i Azure App Service från portalen."
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
ms.openlocfilehash: 85a88c537415cd6f2b6bc005cc18e3baaa29e9a4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a><span data-ttu-id="b1b96-103">Skapa en funktionsapp från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b1b96-103">Create a function app from the Azure portal</span></span>

<span data-ttu-id="b1b96-104">Azure funktionen appar använder Azure App Service-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="b1b96-104">Azure Function Apps uses the Azure App Service infrastructure.</span></span> <span data-ttu-id="b1b96-105">Det här avsnittet visar hur du skapar en funktion i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b1b96-105">This topic shows you how to create a function app in the Azure portal.</span></span> <span data-ttu-id="b1b96-106">En funktionsapp är en behållare som är värd för körningen av enskilda funktioner.</span><span class="sxs-lookup"><span data-stu-id="b1b96-106">A function app is the container that hosts the execution of individual functions.</span></span> <span data-ttu-id="b1b96-107">När du skapar en funktionsapp i Apptjänst som är värd för planen kan appen funktionen använda alla funktioner i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b1b96-107">When you create a function app in the App Service hosting plan, your function app can use all the features of App Service.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="b1b96-108">Skapa en funktionsapp</span><span class="sxs-lookup"><span data-stu-id="b1b96-108">Create a function app</span></span>

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="b1b96-109">När du skapar en funktionsapp, ange en giltig **appnamn**, som kan innehålla endast bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="b1b96-109">When you create a function app, supply a valid **App name**, which can contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="b1b96-110">Understreck (**_**) är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="b1b96-110">Underscore (**_**) is not an allowed character.</span></span>

<span data-ttu-id="b1b96-111">Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.</span><span class="sxs-lookup"><span data-stu-id="b1b96-111">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="b1b96-112">Namnet på ditt lagringskonto måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="b1b96-112">Your storage account name must be unique within Azure.</span></span> 

<span data-ttu-id="b1b96-113">När funktionen appen har skapats kan skapa du enskilda funktioner i en eller flera olika språk.</span><span class="sxs-lookup"><span data-stu-id="b1b96-113">After the function app is created, you can create individual functions in one or more different languages.</span></span> <span data-ttu-id="b1b96-114">Skapa funktioner [med hjälp av portalen](functions-create-first-azure-function.md#create-function), [kontinuerlig distribution](functions-continuous-deployment.md), eller av [Överför med FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span><span class="sxs-lookup"><span data-stu-id="b1b96-114">Create functions [by using the portal](functions-create-first-azure-function.md#create-function), [continuous deployment](functions-continuous-deployment.md), or by [uploading with FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span></span>

## <a name="service-plans"></a><span data-ttu-id="b1b96-115">Service-planer</span><span class="sxs-lookup"><span data-stu-id="b1b96-115">Service plans</span></span>

<span data-ttu-id="b1b96-116">Azure Functions har två olika serviceplaner: förbrukning planerings- och App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="b1b96-116">Azure Functions has two different service plans: Consumption plan and App Service plan.</span></span> <span data-ttu-id="b1b96-117">Plan för förbrukning tilldelar automatiskt datorkraft när koden körs, att skala ut som behövs för att hantera belastningen och sedan skalor in när koden inte körs.</span><span class="sxs-lookup"><span data-stu-id="b1b96-117">The Consumption plan automatically allocates compute power when your code is running, scales-out as necessary to handle load, and then scales-in when code is not running.</span></span> <span data-ttu-id="b1b96-118">Programtjänstplanen ger din funktion appåtkomst till alla funktioner i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b1b96-118">The App Service plan gives your function app access to all the facilities of App Service.</span></span> <span data-ttu-id="b1b96-119">Du måste välja serviceplanen när funktionen appen skapas och den för närvarande inte kan ändras.</span><span class="sxs-lookup"><span data-stu-id="b1b96-119">You must choose your service plan when your function app is created, and it cannot currently be changed.</span></span> <span data-ttu-id="b1b96-120">Mer information finns i [väljer du ett Azure-funktioner som värd för planen](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="b1b96-120">For more information, see [Choose an Azure Functions hosting plan](functions-scale.md).</span></span>

<span data-ttu-id="b1b96-121">Om du planerar att köra JavaScript-funktioner på en apptjänstplan, bör du välja en plan med färre kärnor.</span><span class="sxs-lookup"><span data-stu-id="b1b96-121">If you are planning to run JavaScript functions on an App Service plan, you should choose a plan with fewer cores.</span></span> <span data-ttu-id="b1b96-122">Mer information finns i [JavaScript-referens för funktioner](functions-reference-node.md#choose-single-core-app-service-plans).</span><span class="sxs-lookup"><span data-stu-id="b1b96-122">For more information, see the [JavaScript reference for Functions](functions-reference-node.md#choose-single-core-app-service-plans).</span></span>

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a><span data-ttu-id="b1b96-123">Lagringskraven för kontot</span><span class="sxs-lookup"><span data-stu-id="b1b96-123">Storage account requirements</span></span>

<span data-ttu-id="b1b96-124">När du skapar en funktionsapp i App Service, måste du skapa eller länka till en generell Azure Storage-konto som har stöd för lagring av Blob, köer och tabellen.</span><span class="sxs-lookup"><span data-stu-id="b1b96-124">When creating a function app in App Service, you must create or link to a general-purpose Azure Storage account that supports Blob, Queue, and Table storage.</span></span> <span data-ttu-id="b1b96-125">Internt använder funktioner lagring för åtgärder som hanterar utlösare och loggning funktionen körningar.</span><span class="sxs-lookup"><span data-stu-id="b1b96-125">Internally, Functions uses Storage for operations such as managing triggers and logging function executions.</span></span> <span data-ttu-id="b1b96-126">Vissa storage-konton stöder inte köer och tabeller som endast blob storage-konton, Azure Premium-lagring och allmänna lagringskonton med ZRS replikering.</span><span class="sxs-lookup"><span data-stu-id="b1b96-126">Some storage accounts do not support queues and tables, such as blob-only storage accounts, Azure Premium Storage, and general-purpose storage accounts with ZRS replication.</span></span> <span data-ttu-id="b1b96-127">Dessa konton är filtrerade av i bladet Storage-konto när du skapar en funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="b1b96-127">These accounts are filtered out of from the Storage Account blade when creating a function app.</span></span>

>[!NOTE]
><span data-ttu-id="b1b96-128">När du använder förbrukningen värd plan lagras konfigurationsfilerna funktionen kod och bindningen i Azure File storage i huvudsakliga storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b1b96-128">When using the Consumption hosting plan, your function code and binding configuration files are stored in Azure File storage in the main storage account.</span></span> <span data-ttu-id="b1b96-129">När du tar bort huvudsakliga storage-konto raderas innehållet och kan inte återställas.</span><span class="sxs-lookup"><span data-stu-id="b1b96-129">When you delete the main storage account, this content is deleted and cannot be recovered.</span></span>

<span data-ttu-id="b1b96-130">Läs mer om lagringskontotyper i [introduktion till Azure Storage-tjänster](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span><span class="sxs-lookup"><span data-stu-id="b1b96-130">To learn more about storage account types, see [Introducing the Azure Storage Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b1b96-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1b96-131">Next steps</span></span>

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



