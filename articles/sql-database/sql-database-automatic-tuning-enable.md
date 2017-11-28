---
title: "aaaEnable automatisk inställning för Azure SQL Database | Microsoft Docs"
description: "Du kan aktivera automatisk justering på Azure SQL Database enkelt."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.openlocfilehash: af9da161eabc0f8c4cb100c050288f234efb8093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-automatic-tuning"></a><span data-ttu-id="fcf36-103">Aktivera automatisk inställning</span><span class="sxs-lookup"><span data-stu-id="fcf36-103">Enable automatic tuning</span></span>

<span data-ttu-id="fcf36-104">Azure SQL Database är en automatiskt hanterade datatjänst som ständigt övervakar dina frågor och identifierar hello åtgärd som du kan utföra tooimprove prestandan för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="fcf36-104">Azure SQL Database is an automatically managed data service that constantly monitors your queries and identifies hello action that you can perform tooimprove performance of your workload.</span></span> <span data-ttu-id="fcf36-105">Du kan granska rekommendationer och manuellt koppla dem, eller låta Azure SQL Database automatiskt tillämpa korrigerande åtgärder – den här funktionen kallas **automatiskt prestandajustering läge**.</span><span class="sxs-lookup"><span data-stu-id="fcf36-105">You can review recommendations and manually apply them, or let Azure SQL Database automatically apply corrective actions - this is known as **automatic tuning mode**.</span></span> <span data-ttu-id="fcf36-106">Automatisk justering kan aktiveras på hello server eller hello i databasen.</span><span class="sxs-lookup"><span data-stu-id="fcf36-106">Automatic tuning can be enabled at hello server or hello database level.</span></span>

## <a name="enable-automatic-tuning-on-server"></a><span data-ttu-id="fcf36-107">Aktivera automatisk justering på servern</span><span class="sxs-lookup"><span data-stu-id="fcf36-107">Enable automatic tuning on server</span></span>

<span data-ttu-id="fcf36-108">tooenable automatisk justering på Azure SQL Database-server, navigera toohello server i Azure portal och välj sedan **automatisk justering** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="fcf36-108">tooenable automatic tuning on Azure SQL Database server, navigate toohello server in Azure portal and then select **Automatic tuning** in hello menu.</span></span> <span data-ttu-id="fcf36-109">Välj hello alternativ för automatisk justering tooenable och markera **Verkställ**:</span><span class="sxs-lookup"><span data-stu-id="fcf36-109">Select hello automatic tuning options you want tooenable and select **Apply**:</span></span>

![Server](./media/sql-database-automatic-tuning-enable/server.png)

<span data-ttu-id="fcf36-111">Automatisk justering alternativ på servern är tillämpade tooall databaser på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="fcf36-111">Automatic tuning options on server are applied tooall databases on hello server.</span></span> <span data-ttu-id="fcf36-112">Som standard alla databaser ärver hello konfiguration från sina överordnade servern, men detta kan åsidosättas och anges separat för varje databas.</span><span class="sxs-lookup"><span data-stu-id="fcf36-112">By default, all databases inherit hello configuration from their parent server, but this can be overridden and specified for each database individually.</span></span>

## <a name="configure-automatic-tuning-on-database"></a><span data-ttu-id="fcf36-113">Konfigurera automatisk inställning för databasen</span><span class="sxs-lookup"><span data-stu-id="fcf36-113">Configure automatic tuning on database</span></span>

<span data-ttu-id="fcf36-114">hello Azure portal aktiverar du tooindividually ange hello automatisk justering konfiguration på varje databas.</span><span class="sxs-lookup"><span data-stu-id="fcf36-114">hello Azure portal enables you tooindividually specify hello automatic tuning configuration on each database.</span></span>

> [!NOTE]
> <span data-ttu-id="fcf36-115">hello allmänna rekommendationer är toomanage hello automatisk justering konfigurationen på servernivå så hello samma inställningar kan tillämpas på varje databas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fcf36-115">hello general recommendation is toomanage hello automatic tuning configuration at server level so hello same configuration settings can be applied on every database automatically.</span></span> <span data-ttu-id="fcf36-116">Konfigurera automatisk inställning på en individuell databas om hello databasen skiljer sig att hello andra på samma server.</span><span class="sxs-lookup"><span data-stu-id="fcf36-116">Configure automatic tuning on an individual database if hello database is different that others on hello same server.</span></span>
>

<span data-ttu-id="fcf36-117">automatisk inställning på en enskild databas tooenable navigera toohello databasen i hello Azure-portalen och sedan väljer **automatisk justering**.</span><span class="sxs-lookup"><span data-stu-id="fcf36-117">tooenable automatic tuning on a single database, navigate toohello database in hello Azure portal and then and select **Automatic tuning**.</span></span> <span data-ttu-id="fcf36-118">Du kan konfigurera en enskild databas tooinherit hello inställningar från hello databasen genom att markera kryssrutan hello eller du kan ange hello konfiguration för en databas individuellt.</span><span class="sxs-lookup"><span data-stu-id="fcf36-118">You can configure a single database tooinherit hello settings from hello database by selecting hello checkbox or you can specify hello configuration for a database individually.</span></span>

![Databas](./media/sql-database-automatic-tuning-enable/database.png)

<span data-ttu-id="fcf36-120">När du har valt rätt konfiguration, klickar du på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="fcf36-120">Once you have selected appropriate configuration, click **Apply**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcf36-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fcf36-121">Next steps</span></span>
* <span data-ttu-id="fcf36-122">Läs hello [automatisk justering artikel](sql-database-automatic-tuning.md) toolearn mer om automatisk justering och hur det kan hjälpa dig att förbättra prestandan.</span><span class="sxs-lookup"><span data-stu-id="fcf36-122">Read hello [Automatic tuning article](sql-database-automatic-tuning.md) toolearn more about automatic tuning and how it can help you improve your performance.</span></span>
* <span data-ttu-id="fcf36-123">Se [rekommendationer](sql-database-advisor.md) en översikt över Azure SQL Database prestanda rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="fcf36-123">See [Performance recommendations](sql-database-advisor.md) for an overview of Azure SQL Database performance recommendations.</span></span>
* <span data-ttu-id="fcf36-124">Se [insikter i frågeprestanda](sql-database-query-performance.md) toolearn om hur du visar hello prestandapåverkan top-frågor.</span><span class="sxs-lookup"><span data-stu-id="fcf36-124">See [Query Performance Insights](sql-database-query-performance.md) toolearn about viewing hello performance impact of your top queries.</span></span>
