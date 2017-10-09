---
title: "aaaManaging Redis-cache med hjälp av hello Azure Explorer för Eclipse | Microsoft Docs"
description: "Lär dig hur toomanage din Azure redis cachelagrar med hello Azure Explorer för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/14/2017
ms.author: robmcm
ms.openlocfilehash: aa0c38862bda7919a3fc6c53c2fdaf555dd64bff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="99c47-103">Hantera Redis-cache med hjälp av hello Azure Explorer för Eclipse</span><span class="sxs-lookup"><span data-stu-id="99c47-103">Managing Redis Caches using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="99c47-104">hello Azure Explorer, som är en del av hello Azure Toolkit för Eclipse, tillhandahåller Java-utvecklare med en enkel att använda lösning för hantering av redis-cache i Azure kontot från inuti hello Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="99c47-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside hello Eclipse IDE.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-eclipse"></a><span data-ttu-id="99c47-105">Skapa ett Redis-Cache med hjälp av Eclipse</span><span class="sxs-lookup"><span data-stu-id="99c47-105">Create a Redis Cache by using Eclipse</span></span>

<span data-ttu-id="99c47-106">hello följande steg vägleder dig genom hello steg toocreate ett redis-cache med hjälp av hello Azure Explorer.</span><span class="sxs-lookup"><span data-stu-id="99c47-106">hello following steps walk you through hello steps toocreate a redis cache using hello Azure Explorer.</span></span>

1. <span data-ttu-id="99c47-107">Logga in tooyour Azure-konto med hello steg i hello [logga i instruktioner för hello Azure Toolkit för Eclipse] artikel.</span><span class="sxs-lookup"><span data-stu-id="99c47-107">Sign in tooyour Azure account using hello steps in hello [Sign In Instructions for hello Azure Toolkit for Eclipse] article.</span></span>

1. <span data-ttu-id="99c47-108">I hello **Azure Explorer** verktyget fönster, expandera hello **Azure** noden, högerklickar du på **Redis-cache**, och klicka sedan på **skapa Redis-Cache**.</span><span class="sxs-lookup"><span data-stu-id="99c47-108">In hello **Azure Explorer** tool window, expand hello **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Skapa Redis-Cache-menyn][CR01]

1. <span data-ttu-id="99c47-110">När hello **nytt Redis-Cache** i dialogrutan Ange hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="99c47-110">When hello **New Redis Cache** dialog box appears, specify hello following options:</span></span>

   ![Skapa ny dialogruta för Redis-Cache][CR02]

   <span data-ttu-id="99c47-112">a.</span><span class="sxs-lookup"><span data-stu-id="99c47-112">a.</span></span> <span data-ttu-id="99c47-113">**DNS-namnet**: Anger hello DNS-underdomänen för hello nya redis-cache som föregås för ”. redis.cache.windows .net”, till exempel: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="99c47-113">**DNS Name**: Specifies hello DNS subdomain for hello new redis cache, which is prepended too".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="99c47-114">b.</span><span class="sxs-lookup"><span data-stu-id="99c47-114">b.</span></span> <span data-ttu-id="99c47-115">**Prenumerationen**: Anger hello Azure-prenumeration du vill använda toouse för hello nya redis-cache.</span><span class="sxs-lookup"><span data-stu-id="99c47-115">**Subscription**: Specifies hello Azure subscription you want toouse for hello new redis cache.</span></span>

   <span data-ttu-id="99c47-116">c.</span><span class="sxs-lookup"><span data-stu-id="99c47-116">c.</span></span> <span data-ttu-id="99c47-117">**Resursgruppen**: Anger hello resursgruppen för redis-cache, behöver du toochoose något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="99c47-117">**Resource Group**: Specifies hello resource group for your redis cache; you need toochoose one of hello following options:</span></span>
      * <span data-ttu-id="99c47-118">**Skapa ny**: Anger att du vill toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="99c47-118">**Create New**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="99c47-119">**Använd befintlig**: Anger att du ska välja från en lista över resursgrupper som är associerade med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="99c47-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="99c47-120">d.</span><span class="sxs-lookup"><span data-stu-id="99c47-120">d.</span></span> <span data-ttu-id="99c47-121">**Plats**: Anger hello plats där redis-cache skapas, till exempel *västra USA*.</span><span class="sxs-lookup"><span data-stu-id="99c47-121">**Location**: Specifies hello location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="99c47-122">e.</span><span class="sxs-lookup"><span data-stu-id="99c47-122">e.</span></span> <span data-ttu-id="99c47-123">**Prisnivån**: Anger vilken prisnivå redis-cache använder; den här inställningen avgör hello antalet klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="99c47-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines hello number of client connections.</span></span> <span data-ttu-id="99c47-124">(Mer information finns i [Redis-Cache priser].)</span><span class="sxs-lookup"><span data-stu-id="99c47-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="99c47-125">f.</span><span class="sxs-lookup"><span data-stu-id="99c47-125">f.</span></span> <span data-ttu-id="99c47-126">**Icke-SSL-porten**: Anger om redis-cache kan icke-SSL-anslutningar; SSL-anslutningar tillåts som standard.</span><span class="sxs-lookup"><span data-stu-id="99c47-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="99c47-127">När du har angett alla redis cache-inställningar, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="99c47-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="99c47-128">När redis-cache har skapats visas den i hello Azure Explorer.</span><span class="sxs-lookup"><span data-stu-id="99c47-128">After your redis cache has been created, it will be displayed in hello Azure Explorer.</span></span>

   ![Redis-Cache i Azure Explorer][CR03]

> [!NOTE]
>
> <span data-ttu-id="99c47-130">Mer information om hur du konfigurerar din Azure redis-cache-inställningar, se [hur tooconfigure Azure Redis-Cache].</span><span class="sxs-lookup"><span data-stu-id="99c47-130">For more information about configuring your Azure redis cache settings, see [How tooconfigure Azure Redis Cache].</span></span>
>

## <a name="display-hello-properties-for-your-redis-cache-in-eclipse"></a><span data-ttu-id="99c47-131">Visa hello egenskaper för din Redis-Cache i Eclipse</span><span class="sxs-lookup"><span data-stu-id="99c47-131">Display hello properties for your Redis Cache in Eclipse</span></span>

1. <span data-ttu-id="99c47-132">I hello Azure Explorer, högerklicka på redis-cache och klickar på **visa egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="99c47-132">In hello Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Azure Explorer kontexten menyn toodisplay egenskaper för ett redis-cache][SP01]

1. <span data-ttu-id="99c47-134">hello Azure Explorer visar hello egenskaper för redis-cache.</span><span class="sxs-lookup"><span data-stu-id="99c47-134">hello Azure Explorer displays hello properties for your redis cache.</span></span>

   ![Egenskaper för Redis-cache][SP02]

## <a name="delete-your-redis-cache-by-using-eclipse"></a><span data-ttu-id="99c47-136">Ta bort Redis-Cache med Eclipse</span><span class="sxs-lookup"><span data-stu-id="99c47-136">Delete your Redis Cache by using Eclipse</span></span>

1. <span data-ttu-id="99c47-137">I hello Azure Explorer, högerklicka på redis-cache och klickar på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="99c47-137">In hello Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Azure Explorer kontexten menyn toodelete ett redis-cache][DE01]

1. <span data-ttu-id="99c47-139">Klicka på **OK** när du uppmanas till detta toodelete redis-cache.</span><span class="sxs-lookup"><span data-stu-id="99c47-139">Click **OK** when prompted toodelete your redis cache.</span></span>

   ![Ta bort redis-cache-fråga][DE02]

## <a name="next-steps"></a><span data-ttu-id="99c47-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99c47-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="99c47-142">Mer information om Azure redis-cache, konfigurationsinställningar och priser finns i hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="99c47-142">For more information about Azure redis caches, configuration settings and pricing, see hello following links:</span></span>

* <span data-ttu-id="99c47-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="99c47-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="99c47-144">[Redis-Cache-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="99c47-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="99c47-145">[Redis-Cache priser]</span><span class="sxs-lookup"><span data-stu-id="99c47-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="99c47-146">[hur tooconfigure Azure Redis-Cache]</span><span class="sxs-lookup"><span data-stu-id="99c47-146">[How tooconfigure Azure Redis Cache]</span></span>

<!-- URL List -->

[Redis-Cache priser]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis-Cache-dokumentation]: ./redis-cache/index.md
[hur tooconfigure Azure Redis-Cache]: ./redis-cache/cache-configure.md
[logga i instruktioner för hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE02.png
