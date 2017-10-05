---
title: "Logic Apps B2B edifact avkoda lösa UNH2.5 - Azure Logic Apps | Microsoft Docs"
description: "Azure Logic Apps B2B edifact avkoda lösa UNH2.5"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 62ad8183cc6e9f56255b2729a04ee7710d00a21a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-handle-edifact-documents-having-unh25-segment"></a><span data-ttu-id="68f56-103">Hantera EDIFACT-dokument med UNH2.5 segment</span><span class="sxs-lookup"><span data-stu-id="68f56-103">How to handle EDIFACT documents having UNH2.5 segment</span></span>
<span data-ttu-id="68f56-104">Om UNH2.5 finns i dokumentet EDIFACT används för schemat sökning.</span><span class="sxs-lookup"><span data-stu-id="68f56-104">When UNH2.5 is present in the EDIFACT document, it is being used for schema lookup.</span></span> 

<span data-ttu-id="68f56-105">Exempel: Fältet UNH är **EAN008** i EDIFACT-meddelande</span><span class="sxs-lookup"><span data-stu-id="68f56-105">Example: The UNH field is **EAN008** in the EDIFACT message</span></span>  
<span data-ttu-id="68f56-106">UNH + SSDD1 + ORDER: D: 03B: FN:**EAN008**'</span><span class="sxs-lookup"><span data-stu-id="68f56-106">UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'</span></span>  

<span data-ttu-id="68f56-107">Åtgärder för att hantera meddelandet</span><span class="sxs-lookup"><span data-stu-id="68f56-107">Steps to follow to handle the message</span></span> 
1. <span data-ttu-id="68f56-108">Uppdatera schemat</span><span class="sxs-lookup"><span data-stu-id="68f56-108">Update the schema</span></span>
2. <span data-ttu-id="68f56-109">Kontrollera inställningarna för avtal</span><span class="sxs-lookup"><span data-stu-id="68f56-109">Check the agreement settings</span></span>  

## <a name="update-the-schema"></a><span data-ttu-id="68f56-110">Uppdatera schemat</span><span class="sxs-lookup"><span data-stu-id="68f56-110">Update the schema</span></span>
<span data-ttu-id="68f56-111">Du behöver distribuera ett schema med UNH2.5 rotnodens namn för att bearbeta meddelandet.</span><span class="sxs-lookup"><span data-stu-id="68f56-111">To process the message, you need to deploy a schema with the UNH2.5 root node name.</span></span>  <span data-ttu-id="68f56-112">För ett exempel är roten schemanamnet **EFACT_D03B_ORDERS_EAN008**</span><span class="sxs-lookup"><span data-stu-id="68f56-112">For given an example, the schema root name would be **EFACT_D03B_ORDERS_EAN008**</span></span>  

<span data-ttu-id="68f56-113">För varje D03B_ORDERS med ett annat UNH2.5 segment, skulle du behöva distribuera en enskild schemat.</span><span class="sxs-lookup"><span data-stu-id="68f56-113">For each D03B_ORDERS with a different UNH2.5 segment, you would have to deploy an individual schema.</span></span>  

## <a name="add-schema-to-the-edifact-agreement"></a><span data-ttu-id="68f56-114">Lägga till schemat i EDIFACT-avtal</span><span class="sxs-lookup"><span data-stu-id="68f56-114">Add schema to the EDIFACT agreement</span></span>
### <a name="edifact-decode"></a><span data-ttu-id="68f56-115">EDIFACT avkoda</span><span class="sxs-lookup"><span data-stu-id="68f56-115">EDIFACT Decode</span></span>
<span data-ttu-id="68f56-116">Konfigurera schemat för att avkoda det inkommande meddelandet i EDIFACT avtalet tar emot inställningar</span><span class="sxs-lookup"><span data-stu-id="68f56-116">To Decode the incoming message, configure the schema in the EDIFACT agreement receive settings</span></span>
1. <span data-ttu-id="68f56-117">Lägga till schemat integration-konto</span><span class="sxs-lookup"><span data-stu-id="68f56-117">Add the schema to the integration account</span></span>    
2. <span data-ttu-id="68f56-118">Konfigurera schemat i EDIFACT avtal får inställningar.</span><span class="sxs-lookup"><span data-stu-id="68f56-118">Configure the schema in the EDIFACT agreement receive settings.</span></span> 
3. <span data-ttu-id="68f56-119">Välj EDIFACT-avtal och klickar på **redigera som JSON**.</span><span class="sxs-lookup"><span data-stu-id="68f56-119">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="68f56-120">Lägg till UNH2.5 värde i avtalet får **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68f56-120">Add UNH2.5 value in the Receive Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span></span>

### <a name="edifact-encode"></a><span data-ttu-id="68f56-121">EDIFACT koda</span><span class="sxs-lookup"><span data-stu-id="68f56-121">EDIFACT Encode</span></span>
<span data-ttu-id="68f56-122">Konfigurera schemat för att koda det inkommande meddelandet i EDIFACT-avtal skicka inställningar</span><span class="sxs-lookup"><span data-stu-id="68f56-122">To Encode the incoming message, configure the schema in the EDIFACT agreement send settings</span></span>
1. <span data-ttu-id="68f56-123">Lägga till schemat integration-konto</span><span class="sxs-lookup"><span data-stu-id="68f56-123">Add the schema to the integration account</span></span>    
2. <span data-ttu-id="68f56-124">Konfigurera schemat i EDIFACT-avtal skicka inställningar.</span><span class="sxs-lookup"><span data-stu-id="68f56-124">Configure the schema in the EDIFACT agreement send settings.</span></span> 
3. <span data-ttu-id="68f56-125">Välj EDIFACT-avtal och klickar på **redigera som JSON**.</span><span class="sxs-lookup"><span data-stu-id="68f56-125">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="68f56-126">Lägg till UNH2.5 värde i avtalet skicka **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span><span class="sxs-lookup"><span data-stu-id="68f56-126">Add UNH2.5 value in the Send Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="68f56-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68f56-127">Next Steps</span></span>
* [<span data-ttu-id="68f56-128">Mer information om integration konto avtal</span><span class="sxs-lookup"><span data-stu-id="68f56-128">Learn more about integration account agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  