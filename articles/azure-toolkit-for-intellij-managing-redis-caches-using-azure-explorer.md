---
title: "Hantera Redis-cache med hjälp av Azure-Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig mer om att hantera Azure redis-cache med hjälp av Azure-Explorer för IntelliJ."
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
ms.openlocfilehash: 9ab8ae17ee2a92b5b16d2210366c00b5b8023fa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="b8698-103">Hantera Redis-cache med hjälp av Azure-Explorer för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="b8698-103">Managing Redis Caches using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="b8698-104">Azure-Explorer, som är en del av Azure-verktygen för IntelliJ ger Java-utvecklare med ett enkelt att använda lösning för hantering av redis-cache i Azure kontot från inuti IntelliJ IDE.</span><span class="sxs-lookup"><span data-stu-id="b8698-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing redis caches in their Azure account from inside the IntelliJ IDE.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a><span data-ttu-id="b8698-105">Skapa ett Redis-Cache med hjälp av IntelliJ</span><span class="sxs-lookup"><span data-stu-id="b8698-105">Create a Redis Cache by using IntelliJ</span></span>

<span data-ttu-id="b8698-106">Följande steg vägleder dig genom stegen för att skapa ett redis-cache med hjälp av Azure-Explorer.</span><span class="sxs-lookup"><span data-stu-id="b8698-106">The following steps walk you through the steps to create a redis cache using the Azure Explorer.</span></span>

1. <span data-ttu-id="b8698-107">Logga in på ditt Azure-konto med hjälp av stegen i den [logga i instruktioner för Azure-verktygen för IntelliJ] artikel.</span><span class="sxs-lookup"><span data-stu-id="b8698-107">Sign in to your Azure account using the steps in the [Sign In Instructions for the Azure Toolkit for IntelliJ] article.</span></span>

1. <span data-ttu-id="b8698-108">I den **Azure Explorer** verktyget fönster, expandera den **Azure** noden, högerklickar du på **Redis-cache**, och klicka sedan på **skapa Redis-Cache**.</span><span class="sxs-lookup"><span data-stu-id="b8698-108">In the **Azure Explorer** tool window, expand the **Azure** node, right-click **Redis Caches**, and then click **Create Redis Cache**.</span></span>

   ![Skapa Redis-Cache-menyn][CR01]

1. <span data-ttu-id="b8698-110">När den **nytt Redis-Cache** i dialogrutan anger du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b8698-110">When the **New Redis Cache** dialog box appears, specify the following options:</span></span>

   ![Skapa nytt Redis-Cache-dialogruta][CR02]

   <span data-ttu-id="b8698-112">a.</span><span class="sxs-lookup"><span data-stu-id="b8698-112">a.</span></span> <span data-ttu-id="b8698-113">**DNS-namnet**: Anger DNS-underdomänen för den nya redis-cache, vilket till före ”. redis.cache.windows.net”, till exempel: *wingtiptoys.redis.cache.windows.net*.</span><span class="sxs-lookup"><span data-stu-id="b8698-113">**DNS Name**: Specifies the DNS subdomain for the new redis cache, which are prepended to ".redis.cache.windows.net"; for example: *wingtiptoys.redis.cache.windows.net*.</span></span>

   <span data-ttu-id="b8698-114">b.</span><span class="sxs-lookup"><span data-stu-id="b8698-114">b.</span></span> <span data-ttu-id="b8698-115">**Prenumerationen**: Anger den Azure-prenumeration du vill använda för nya redis-cache.</span><span class="sxs-lookup"><span data-stu-id="b8698-115">**Subscription**: Specifies the Azure subscription you want to use for the new redis cache.</span></span>

   <span data-ttu-id="b8698-116">c.</span><span class="sxs-lookup"><span data-stu-id="b8698-116">c.</span></span> <span data-ttu-id="b8698-117">**Resursgruppen**: Anger resursgruppen för redis-cache, måste du välja något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b8698-117">**Resource Group**: Specifies the resource group for your redis cache; you need to choose one of the following options:</span></span>
      * <span data-ttu-id="b8698-118">**Skapa ny**: Anger att du vill skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b8698-118">**Create New**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="b8698-119">**Använd befintlig**: Anger att du ska välja från en lista över resursgrupper som är associerade med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b8698-119">**Use Existing**: Specifies that you will choose from a list of resource groups associated with your Azure account.</span></span>

   <span data-ttu-id="b8698-120">d.</span><span class="sxs-lookup"><span data-stu-id="b8698-120">d.</span></span> <span data-ttu-id="b8698-121">**Plats**: Anger platsen där redis-cache skapas, till exempel *västra USA*.</span><span class="sxs-lookup"><span data-stu-id="b8698-121">**Location**: Specifies the location where your redis cache is created; for example, *West US*.</span></span>

   <span data-ttu-id="b8698-122">e.</span><span class="sxs-lookup"><span data-stu-id="b8698-122">e.</span></span> <span data-ttu-id="b8698-123">**Prisnivån**: Anger vilken prisnivå redis-cache använder; den här inställningen avgör antalet klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="b8698-123">**Pricing Tier**: Specifies which pricing tier your redis cache uses; this setting determines the number of client connections.</span></span> <span data-ttu-id="b8698-124">(Mer information finns i [Redis-Cache priser].)</span><span class="sxs-lookup"><span data-stu-id="b8698-124">(For more information, see [Redis Cache Pricing].)</span></span>

   <span data-ttu-id="b8698-125">f.</span><span class="sxs-lookup"><span data-stu-id="b8698-125">f.</span></span> <span data-ttu-id="b8698-126">**Icke-SSL-porten**: Anger om redis-cache kan icke-SSL-anslutningar; SSL-anslutningar tillåts som standard.</span><span class="sxs-lookup"><span data-stu-id="b8698-126">**Non-SSL port**: Specifies whether your redis cache allows non-SSL connections; by default, only SSL connections are allowed.</span></span>

1. <span data-ttu-id="b8698-127">När du har angett alla redis cache-inställningar, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8698-127">When you have specified all your redis cache settings, click **OK**.</span></span>

<span data-ttu-id="b8698-128">När redis-cache har skapats visas den i Azure-Explorer.</span><span class="sxs-lookup"><span data-stu-id="b8698-128">After your redis cache has been created, it will be displayed in the Azure Explorer.</span></span>

   ![Redis-Cache i Azure Explorer][CR03]

> [!NOTE]
>
> <span data-ttu-id="b8698-130">Mer information om hur du konfigurerar din Azure redis-cache-inställningar, se [hur du konfigurerar Azure Redis-Cache].</span><span class="sxs-lookup"><span data-stu-id="b8698-130">For more information about configuring your Azure redis cache settings, see [How to configure Azure Redis Cache].</span></span>
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a><span data-ttu-id="b8698-131">Visa egenskaperna för din Redis-Cache i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="b8698-131">Display the properties for your Redis Cache in IntelliJ</span></span>

1. <span data-ttu-id="b8698-132">Högerklicka på redis-cache i Azure-Explorer och klicka på **visa egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="b8698-132">In the Azure Explorer, right-click your redis cache and click **Show properties**.</span></span>

   ![Azure Explorer snabbmenyn att visa egenskaper för ett redis-cache][SP01]

1. <span data-ttu-id="b8698-134">Azure-Explorer visar egenskaper för redis-cache.</span><span class="sxs-lookup"><span data-stu-id="b8698-134">The Azure Explorer displays the properties for your redis cache.</span></span>

   ![Egenskaper för Redis-cache][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a><span data-ttu-id="b8698-136">Ta bort Redis-Cache med IntelliJ</span><span class="sxs-lookup"><span data-stu-id="b8698-136">Delete your Redis Cache by using IntelliJ</span></span>

1. <span data-ttu-id="b8698-137">Högerklicka på redis-cache i Azure-Explorer och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b8698-137">In the Azure Explorer, right-click your redis cache and click **Delete**.</span></span>

   ![Azure Explorer snabbmenyn att ta bort ett redis-cache][DE01]

1. <span data-ttu-id="b8698-139">Klicka på **Ja** när du uppmanas att ta bort redis-cache.</span><span class="sxs-lookup"><span data-stu-id="b8698-139">Click **Yes** when prompted to delete your redis cache.</span></span>

   ![Ta bort redis-cache-fråga][DE02]

## <a name="next-steps"></a><span data-ttu-id="b8698-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8698-141">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<span data-ttu-id="b8698-142">Mer information om Azure redis-cache, konfigurationsinställningar och priser finns i följande länkar:</span><span class="sxs-lookup"><span data-stu-id="b8698-142">For more information about Azure redis caches, configuration settings and pricing, see the following links:</span></span>

* <span data-ttu-id="b8698-143">[Azure Redis Cache]</span><span class="sxs-lookup"><span data-stu-id="b8698-143">[Azure Redis Cache]</span></span>
* <span data-ttu-id="b8698-144">[Redis-Cache-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="b8698-144">[Redis Cache Documentation]</span></span>
* <span data-ttu-id="b8698-145">[Redis-Cache priser]</span><span class="sxs-lookup"><span data-stu-id="b8698-145">[Redis Cache Pricing]</span></span>
* <span data-ttu-id="b8698-146">[hur du konfigurerar Azure Redis-Cache]</span><span class="sxs-lookup"><span data-stu-id="b8698-146">[How to configure Azure Redis Cache]</span></span>

<!-- URL List -->

<span data-ttu-id="b8698-147">[Redis-Cache priser]: https://azure.microsoft.com/pricing/details/cache/</span><span class="sxs-lookup"><span data-stu-id="b8698-147">[Redis Cache Pricing]: https://azure.microsoft.com/pricing/details/cache/</span></span>
<span data-ttu-id="b8698-148">[Azure Redis Cache]: https://azure.microsoft.com/services/cache/</span><span class="sxs-lookup"><span data-stu-id="b8698-148">[Azure Redis Cache]: https://azure.microsoft.com/services/cache/</span></span>
<span data-ttu-id="b8698-149">[Redis-Cache-dokumentation]: ./redis-cache/index.md</span><span class="sxs-lookup"><span data-stu-id="b8698-149">[Redis Cache Documentation]: ./redis-cache/index.md</span></span>
<span data-ttu-id="b8698-150">[hur du konfigurerar Azure Redis-Cache]: ./redis-cache/cache-configure.md</span><span class="sxs-lookup"><span data-stu-id="b8698-150">[How to configure Azure Redis Cache]: ./redis-cache/cache-configure.md</span></span>
<span data-ttu-id="b8698-151">[logga i instruktioner för Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="b8698-151">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
