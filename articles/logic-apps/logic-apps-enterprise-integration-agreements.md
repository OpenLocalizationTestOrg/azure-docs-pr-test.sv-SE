---
title: "Avtal för B2B - kommunikation i Azure Logic Apps | Microsoft Docs"
description: "Skapa avtal så att partners kan kommunicera i B2B-scenarier för Azure Logic Apps och Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: LADocs
ms.openlocfilehash: 7ce0860272901f3b4e4cf3d63f7361d539f64741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a><span data-ttu-id="2be7e-103">Partner avtal för B2B-kommunikation med Azure Logikappar och Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="2be7e-103">Partner agreements for B2B communication with Azure Logic Apps and Enterprise Integration Pack</span></span>

<span data-ttu-id="2be7e-104">Avtal kan kommunicera sömlöst med standardprotokollen affärsobjekt och är hörnstenarna för business-to-business (B2B)-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2be7e-104">Agreements let business entities communicate seamlessly using industry standard protocols and are the cornerstone for business-to-business (B2B) communication.</span></span> <span data-ttu-id="2be7e-105">När du aktiverar B2B-scenarier för logic apps med Enterprise-Integrationspaket är ett avtal en kommunikation mellan B2B handelspartner.</span><span class="sxs-lookup"><span data-stu-id="2be7e-105">When enabling B2B scenarios for logic apps with the Enterprise Integration Pack, an agreement is a communications arrangement between B2B trading partners.</span></span> <span data-ttu-id="2be7e-106">Det här avtalet är baserat på de meddelanden som partners vill upprätta och är protokoll eller transport-specifika.</span><span class="sxs-lookup"><span data-stu-id="2be7e-106">This agreement is based on the communications that the partners want to establish and is protocol or transport-specific.</span></span>

<span data-ttu-id="2be7e-107">Enterprise integration stöder dessa protokoll eller transport standarder:</span><span class="sxs-lookup"><span data-stu-id="2be7e-107">Enterprise integration supports these protocol or transport standards:</span></span>

* [<span data-ttu-id="2be7e-108">AS2</span><span class="sxs-lookup"><span data-stu-id="2be7e-108">AS2</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="2be7e-109">X12</span><span class="sxs-lookup"><span data-stu-id="2be7e-109">X12</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="2be7e-110">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="2be7e-110">EDIFACT</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a><span data-ttu-id="2be7e-111">Varför använda avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-111">Why use agreements</span></span>

<span data-ttu-id="2be7e-112">Här är några vanliga fördelar när du använder avtal:</span><span class="sxs-lookup"><span data-stu-id="2be7e-112">Here are some common benefits when using agreements:</span></span>

* <span data-ttu-id="2be7e-113">Aktiverar olika organisationer och företag att utbyta information i ett välkänt format.</span><span class="sxs-lookup"><span data-stu-id="2be7e-113">Enables different organizations and businesses to exchange information in a well-known format.</span></span>
* <span data-ttu-id="2be7e-114">Förbättrar effektiviteten vid B2B-transaktioner</span><span class="sxs-lookup"><span data-stu-id="2be7e-114">Improves efficiency when conducting B2B transactions</span></span>
* <span data-ttu-id="2be7e-115">Enkelt att skapa, hantera och använda när du skapar enterprise integrationsappar</span><span class="sxs-lookup"><span data-stu-id="2be7e-115">Easy to create, manage, and use when creating enterprise integration apps</span></span>

## <a name="how-to-create-agreements"></a><span data-ttu-id="2be7e-116">Så här skapar du avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-116">How to create agreements</span></span>

* [<span data-ttu-id="2be7e-117">Skapa ett AS2-avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-117">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="2be7e-118">Skapa en X12 avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-118">Create an X12 agreement</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="2be7e-119">Skapa ett EDIFACT-avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-119">Create an EDIFACT agreement</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="how-to-use-an-agreement"></a><span data-ttu-id="2be7e-120">Hur du använder ett avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-120">How to use an agreement</span></span>

<span data-ttu-id="2be7e-121">Du kan skapa [logikappar](logic-apps-what-are-logic-apps.md "Lär dig mer om Logic apps") med B2B-funktioner med hjälp av ett avtal som du skapade.</span><span class="sxs-lookup"><span data-stu-id="2be7e-121">You can create [logic apps](logic-apps-what-are-logic-apps.md "Learn about Logic apps") with B2B capabilities by using an agreement that you created.</span></span>

## <a name="how-to-edit-an-agreement"></a><span data-ttu-id="2be7e-122">Så här redigerar du ett avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-122">How to edit an agreement</span></span>

<span data-ttu-id="2be7e-123">Du kan redigera avtal genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="2be7e-123">You can edit any agreement by following these steps:</span></span>

1. <span data-ttu-id="2be7e-124">Välj det integration-konto som har det avtal som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="2be7e-124">Select the integration account that has the agreement you want to update.</span></span>

2. <span data-ttu-id="2be7e-125">Välj den **avtal** panelen.</span><span class="sxs-lookup"><span data-stu-id="2be7e-125">Choose the **Agreements** tile.</span></span>

3. <span data-ttu-id="2be7e-126">På den **avtal** bladet välj avtalet.</span><span class="sxs-lookup"><span data-stu-id="2be7e-126">On the **Agreements** blade, select the agreement.</span></span>

4. <span data-ttu-id="2be7e-127">Välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="2be7e-127">Choose **Edit**.</span></span> <span data-ttu-id="2be7e-128">Gör önskade ändringar.</span><span class="sxs-lookup"><span data-stu-id="2be7e-128">Make your changes.</span></span>

5. <span data-ttu-id="2be7e-129">Spara dina ändringar genom att välja **OK**.</span><span class="sxs-lookup"><span data-stu-id="2be7e-129">To save your changes, choose **OK**.</span></span>

## <a name="how-to-delete-an-agreement"></a><span data-ttu-id="2be7e-130">Hur du tar bort ett avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-130">How to delete an agreement</span></span>

<span data-ttu-id="2be7e-131">Du kan ta bort ett avtal på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2be7e-131">You can delete any agreement by following these steps:</span></span>

1. <span data-ttu-id="2be7e-132">Välj det integration-konto som har det avtal som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="2be7e-132">Select the integration account that has the agreement you want to delete.</span></span>
2. <span data-ttu-id="2be7e-133">Välj den **avtal** panelen.</span><span class="sxs-lookup"><span data-stu-id="2be7e-133">Choose the **Agreements** tile.</span></span>
3. <span data-ttu-id="2be7e-134">På den **avtal** bladet välj avtalet.</span><span class="sxs-lookup"><span data-stu-id="2be7e-134">On the **Agreements** blade, select the agreement.</span></span>
4. <span data-ttu-id="2be7e-135">Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="2be7e-135">Choose **Delete**.</span></span>
5. <span data-ttu-id="2be7e-136">Bekräfta att du vill ta bort det valda avtalet.</span><span class="sxs-lookup"><span data-stu-id="2be7e-136">Confirm that you want to delete the selected agreement.</span></span>

    <span data-ttu-id="2be7e-137">Bladet avtal visas inte längre borttagna avtalet.</span><span class="sxs-lookup"><span data-stu-id="2be7e-137">The Agreements blade no longer shows the deleted agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2be7e-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2be7e-138">Next steps</span></span>
* [<span data-ttu-id="2be7e-139">Skapa ett AS2-avtal</span><span class="sxs-lookup"><span data-stu-id="2be7e-139">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
