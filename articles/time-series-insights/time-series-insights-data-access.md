---
title: "aaaData åtkomstprinciper i Azure tid serien insikter | Microsoft Docs"
description: "I kursen får du lära dig toomanage principerna dataåtkomst i tid serien insikter"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: f286d26c8c5c851c523e9384760dc4b10711fa3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="grant-data-access-tooa-time-series-insights-environment-using-azure-portal"></a><span data-ttu-id="5f912-103">Bevilja åtkomst tooa tid serien insikter datamiljö med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="5f912-103">Grant data access tooa Time Series Insights environment using Azure portal</span></span>

<span data-ttu-id="5f912-104">Time Series Insights-miljöer har två oberoende typer av principer för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5f912-104">Time Series Insights environments have two independent types of access policies:</span></span>

* <span data-ttu-id="5f912-105">Hantera åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="5f912-105">Management access policies</span></span>
* <span data-ttu-id="5f912-106">Dataåtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="5f912-106">Data access policies</span></span>

<span data-ttu-id="5f912-107">Båda principerna beviljar Azure Active Directory-huvudkonton (användare och appar) olika behörigheter i en viss miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-107">Both policies grant Azure Active Directory principals (users and apps) various permissions on a particular environment.</span></span> <span data-ttu-id="5f912-108">hello säkerhetsobjekt (användare och appar) måste tillhöra toohello active directory (eller ”Azure-klient”) som associeras med hello som innehåller hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-108">hello principals (users and apps) must belong toohello active directory (or “Azure tenant”) associated with hello subscription containing hello environment.</span></span>

<span data-ttu-id="5f912-109">Principer för hantering av åtkomst bevilja som behörigheter relaterade toohello konfigurationen av hello-miljö</span><span class="sxs-lookup"><span data-stu-id="5f912-109">Management access policies grant permissions related toohello configuration of hello environment, such as</span></span>
*   <span data-ttu-id="5f912-110">Skapa och ta bort hello miljö, händelsekällor referera till datamängder och</span><span class="sxs-lookup"><span data-stu-id="5f912-110">Creation and deletion of hello environment, event sources, reference data sets, and</span></span>
*   <span data-ttu-id="5f912-111">Hantering av hello principerna dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="5f912-111">Management of hello data access policies.</span></span>

<span data-ttu-id="5f912-112">Principerna dataåtkomst bevilja behörighet tooissue datafrågor manipulera referensdata i hello miljö och dela sparade frågor och perspektiv som är associerade med hello-miljön.</span><span class="sxs-lookup"><span data-stu-id="5f912-112">Data access policies grant permissions tooissue data queries, manipulate reference data in hello environment, and share saved queries and perspectives associated with hello environment.</span></span>

<span data-ttu-id="5f912-113">hello två typer av principer kan tydlig uppdelning mellan toohello åtkomsthantering av hello miljö och åtkomst till toohello data i hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-113">hello two kinds of policies allow clear separation between access toohello management of hello environment and access toohello data inside hello environment.</span></span> <span data-ttu-id="5f912-114">Det är exempelvis möjligt toosetup en miljö så att hello ägare/skapare av hello miljö tas bort från hello dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="5f912-114">For example, it is possible toosetup an environment such that hello owner/creator of hello environment is removed from hello data access.</span></span> <span data-ttu-id="5f912-115">Samt användare och tjänster som är tillåtna tooread data från hello miljö beviljas ingen åtkomst toohello konfiguration av hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-115">As well as users and services that are allowed tooread data from hello environment may be granted no access toohello configuration of hello environment.</span></span>

## <a name="grant-data-access"></a><span data-ttu-id="5f912-116">Bevilja åtkomst till data</span><span class="sxs-lookup"><span data-stu-id="5f912-116">Grant data access</span></span>
<span data-ttu-id="5f912-117">hello följande steg visar hur toogrant dataåtkomst för en UPN:</span><span class="sxs-lookup"><span data-stu-id="5f912-117">hello following steps show how toogrant data access for a user principal:</span></span>

1.  <span data-ttu-id="5f912-118">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5f912-118">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="5f912-119">Klicka på ”alla resurser” hello menyn hello vänster på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5f912-119">Click “All resources” in hello menu on hello left side of hello Azure portal.</span></span>
3.  <span data-ttu-id="5f912-120">Välj Time Series Insights-miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-120">Select your Time Series Insights environment.</span></span>

  ![Hantera hello tid serien insikter source - miljö](media/data-access/getstarted-grant-data-access1.png)

4.  <span data-ttu-id="5f912-122">Välj ”Dataplansåtkomst” och klicka på ”Lägg till”</span><span class="sxs-lookup"><span data-stu-id="5f912-122">Select “Data Plane Access”, click “Add”</span></span>

  ![Hantera hello tid serien insikter källa – Lägg till](media/data-access/getstarted-grant-data-access2.png)

5.  <span data-ttu-id="5f912-124">Klicka på ”Välj användare”.</span><span class="sxs-lookup"><span data-stu-id="5f912-124">Click “Select user”.</span></span>
6.  <span data-ttu-id="5f912-125">Söka och välja användare via hello e-post.</span><span class="sxs-lookup"><span data-stu-id="5f912-125">Search and select user by hello email.</span></span>
7.  <span data-ttu-id="5f912-126">Klicka på ”Välj” i bladet ”Välj användare”.</span><span class="sxs-lookup"><span data-stu-id="5f912-126">Click “Select” in “Select User” blade.</span></span>

  ![Hantera hello tid serien insikter source - Välj användare](media/data-access/getstarted-grant-data-access3.png)

8.  <span data-ttu-id="5f912-128">Klicka på ”Välj roll”.</span><span class="sxs-lookup"><span data-stu-id="5f912-128">Click “Select role”.</span></span>
9.  <span data-ttu-id="5f912-129">Välj ”bidragsgivare” om du vill tooallow användardata toochange referens och dela sparade frågor och perspektiv med andra användare av hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-129">Select “Contributor” if you want tooallow user toochange reference data and share saved queries and perspectives with other users of hello environment.</span></span> <span data-ttu-id="5f912-130">Annars väljer ”Reader” tooallow frågan användardata i hello miljö och spara personliga (inte delade) frågor i hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="5f912-130">Otherwise select “Reader” tooallow user query data in hello environment and save personal (not shared) queries in hello environment.</span></span>
10. <span data-ttu-id="5f912-131">Klicka på ”Ok” hello ”Välj roll”-bladet.</span><span class="sxs-lookup"><span data-stu-id="5f912-131">Click “Ok” in hello “Select Role” blade.</span></span>

  ![Hantera hello tid serien insikter source - Välj rolltjänster](media/data-access/getstarted-grant-data-access4.png)

11. <span data-ttu-id="5f912-133">Klicka på ”Ok” hello ”Välj användarroll”-bladet.</span><span class="sxs-lookup"><span data-stu-id="5f912-133">Click “Ok” in hello “Select User Role” blade.</span></span>
12. <span data-ttu-id="5f912-134">Du bör se:</span><span class="sxs-lookup"><span data-stu-id="5f912-134">You should see:</span></span>

  ![Hantera hello tid serien insikter source - resultat](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a><span data-ttu-id="5f912-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f912-136">Next steps</span></span>

* [<span data-ttu-id="5f912-137">Skapa en händelsekälla</span><span class="sxs-lookup"><span data-stu-id="5f912-137">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="5f912-138">[Skicka händelser](time-series-insights-send-events.md) toohello händelsekälla</span><span class="sxs-lookup"><span data-stu-id="5f912-138">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
* <span data-ttu-id="5f912-139">Visa din miljö i [Time Series Insights-portalen](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="5f912-139">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
