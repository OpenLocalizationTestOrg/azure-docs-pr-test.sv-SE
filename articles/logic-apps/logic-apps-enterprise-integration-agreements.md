---
title: "aaaAgreements för B2B - kommunikation i Azure Logic Apps | Microsoft Docs"
description: "Skapa avtal så att partners kan kommunicera i B2B-scenarier för Azure Logic Apps och hello Enterprise-Integrationspaket"
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
ms.openlocfilehash: 499edcbab1cd67fbc169e393c3cad7b81658a250
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a><span data-ttu-id="b2c8a-103">Partner avtal för B2B-kommunikation med Azure Logikappar och Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="b2c8a-103">Partner agreements for B2B communication with Azure Logic Apps and Enterprise Integration Pack</span></span>

<span data-ttu-id="b2c8a-104">Avtal kan kommunicera sömlöst med standardprotokollen affärsobjekt och är hello hörnstenarna för business-to-business (B2B)-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-104">Agreements let business entities communicate seamlessly using industry standard protocols and are hello cornerstone for business-to-business (B2B) communication.</span></span> <span data-ttu-id="b2c8a-105">När du aktiverar B2B-scenarier för logic apps med hello Enterprise-Integrationspaket är ett avtal en kommunikation mellan B2B handelspartner.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-105">When enabling B2B scenarios for logic apps with hello Enterprise Integration Pack, an agreement is a communications arrangement between B2B trading partners.</span></span> <span data-ttu-id="b2c8a-106">Det här avtalet baseras på hello kommunikation som hello partners vill för upprätta och är protokoll- eller transport-specifika.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-106">This agreement is based on hello communications that hello partners want too establish and is protocol or transport-specific.</span></span>

<span data-ttu-id="b2c8a-107">Enterprise integration stöder dessa protokoll eller transport standarder:</span><span class="sxs-lookup"><span data-stu-id="b2c8a-107">Enterprise integration supports these protocol or transport standards:</span></span>

* [<span data-ttu-id="b2c8a-108">AS2</span><span class="sxs-lookup"><span data-stu-id="b2c8a-108">AS2</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="b2c8a-109">X12</span><span class="sxs-lookup"><span data-stu-id="b2c8a-109">X12</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="b2c8a-110">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="b2c8a-110">EDIFACT</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a><span data-ttu-id="b2c8a-111">Varför använda avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-111">Why use agreements</span></span>

<span data-ttu-id="b2c8a-112">Här är några vanliga fördelar när du använder avtal:</span><span class="sxs-lookup"><span data-stu-id="b2c8a-112">Here are some common benefits when using agreements:</span></span>

* <span data-ttu-id="b2c8a-113">Aktiverar olika organisationer och företag tooexchange information i ett välkänt format.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-113">Enables different organizations and businesses tooexchange information in a well-known format.</span></span>
* <span data-ttu-id="b2c8a-114">Förbättrar effektiviteten vid B2B-transaktioner</span><span class="sxs-lookup"><span data-stu-id="b2c8a-114">Improves efficiency when conducting B2B transactions</span></span>
* <span data-ttu-id="b2c8a-115">Enkelt toocreate, hantera och använda när du skapar enterprise integrationsappar</span><span class="sxs-lookup"><span data-stu-id="b2c8a-115">Easy toocreate, manage, and use when creating enterprise integration apps</span></span>

## <a name="how-toocreate-agreements"></a><span data-ttu-id="b2c8a-116">Hur toocreate avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-116">How toocreate agreements</span></span>

* [<span data-ttu-id="b2c8a-117">Skapa ett AS2-avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-117">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
* [<span data-ttu-id="b2c8a-118">Skapa en X12 avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-118">Create an X12 agreement</span></span>](logic-apps-enterprise-integration-x12.md)
* [<span data-ttu-id="b2c8a-119">Skapa ett EDIFACT-avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-119">Create an EDIFACT agreement</span></span>](logic-apps-enterprise-integration-edifact.md)

## <a name="how-toouse-an-agreement"></a><span data-ttu-id="b2c8a-120">Hur toouse ett avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-120">How toouse an agreement</span></span>

<span data-ttu-id="b2c8a-121">Du kan skapa [logikappar](logic-apps-what-are-logic-apps.md "Lär dig mer om Logic apps") med B2B-funktioner med hjälp av ett avtal som du skapade.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-121">You can create [logic apps](logic-apps-what-are-logic-apps.md "Learn about Logic apps") with B2B capabilities by using an agreement that you created.</span></span>

## <a name="how-tooedit-an-agreement"></a><span data-ttu-id="b2c8a-122">Hur tooedit ett avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-122">How tooedit an agreement</span></span>

<span data-ttu-id="b2c8a-123">Du kan redigera avtal genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="b2c8a-123">You can edit any agreement by following these steps:</span></span>

1. <span data-ttu-id="b2c8a-124">Välj hello integration konto som har hello avtal som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-124">Select hello integration account that has hello agreement you want tooupdate.</span></span>

2. <span data-ttu-id="b2c8a-125">Välj hello **avtal** panelen.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-125">Choose hello **Agreements** tile.</span></span>

3. <span data-ttu-id="b2c8a-126">På hello **avtal** bladet, Välj hello avtal.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-126">On hello **Agreements** blade, select hello agreement.</span></span>

4. <span data-ttu-id="b2c8a-127">Välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-127">Choose **Edit**.</span></span> <span data-ttu-id="b2c8a-128">Gör önskade ändringar.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-128">Make your changes.</span></span>

5. <span data-ttu-id="b2c8a-129">toosave ändringarna, Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-129">toosave your changes, choose **OK**.</span></span>

## <a name="how-toodelete-an-agreement"></a><span data-ttu-id="b2c8a-130">Hur toodelete ett avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-130">How toodelete an agreement</span></span>

<span data-ttu-id="b2c8a-131">Du kan ta bort ett avtal på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2c8a-131">You can delete any agreement by following these steps:</span></span>

1. <span data-ttu-id="b2c8a-132">Välj hello integration konto som har hello avtal som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-132">Select hello integration account that has hello agreement you want toodelete.</span></span>
2. <span data-ttu-id="b2c8a-133">Välj hello **avtal** panelen.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-133">Choose hello **Agreements** tile.</span></span>
3. <span data-ttu-id="b2c8a-134">På hello **avtal** bladet, Välj hello avtal.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-134">On hello **Agreements** blade, select hello agreement.</span></span>
4. <span data-ttu-id="b2c8a-135">Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-135">Choose **Delete**.</span></span>
5. <span data-ttu-id="b2c8a-136">Bekräfta att du vill toodelete hello valt avtal.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-136">Confirm that you want toodelete hello selected agreement.</span></span>

    <span data-ttu-id="b2c8a-137">hello avtal bladet visar inte längre hello bort avtal.</span><span class="sxs-lookup"><span data-stu-id="b2c8a-137">hello Agreements blade no longer shows hello deleted agreement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2c8a-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2c8a-138">Next steps</span></span>
* [<span data-ttu-id="b2c8a-139">Skapa ett AS2-avtal</span><span class="sxs-lookup"><span data-stu-id="b2c8a-139">Create an AS2 agreement</span></span>](logic-apps-enterprise-integration-as2.md)
