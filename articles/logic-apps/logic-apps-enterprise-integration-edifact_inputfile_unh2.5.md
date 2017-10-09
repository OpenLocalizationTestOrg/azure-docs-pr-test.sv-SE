---
title: "aaaLogic appar B2B edifact avkoda lösa UNH2.5 - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 6d85242d0f828fa52cdc9689938f3ba1e51b1183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toohandle-edifact-documents-having-unh25-segment"></a><span data-ttu-id="ea96c-103">Hur toohandle EDIFACT-dokument med UNH2.5 segment</span><span class="sxs-lookup"><span data-stu-id="ea96c-103">How toohandle EDIFACT documents having UNH2.5 segment</span></span>
<span data-ttu-id="ea96c-104">Om UNH2.5 finns i hello EDIFACT dokumentet används för schemat sökning.</span><span class="sxs-lookup"><span data-stu-id="ea96c-104">When UNH2.5 is present in hello EDIFACT document, it is being used for schema lookup.</span></span> 

<span data-ttu-id="ea96c-105">Exempel: hello UNH fältet är **EAN008** i hello EDIFACT-meddelande</span><span class="sxs-lookup"><span data-stu-id="ea96c-105">Example: hello UNH field is **EAN008** in hello EDIFACT message</span></span>  
<span data-ttu-id="ea96c-106">UNH + SSDD1 + ORDER: D: 03B: FN:**EAN008**'</span><span class="sxs-lookup"><span data-stu-id="ea96c-106">UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'</span></span>  

<span data-ttu-id="ea96c-107">Steg toofollow toohandle hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="ea96c-107">Steps toofollow toohandle hello message</span></span> 
1. <span data-ttu-id="ea96c-108">Uppdatera hello schema</span><span class="sxs-lookup"><span data-stu-id="ea96c-108">Update hello schema</span></span>
2. <span data-ttu-id="ea96c-109">Kontrollera inställningarna för hello-avtal</span><span class="sxs-lookup"><span data-stu-id="ea96c-109">Check hello agreement settings</span></span>  

## <a name="update-hello-schema"></a><span data-ttu-id="ea96c-110">Uppdatera hello schema</span><span class="sxs-lookup"><span data-stu-id="ea96c-110">Update hello schema</span></span>
<span data-ttu-id="ea96c-111">tooprocess hello-meddelande måste toodeploy ett schema med hello UNH2.5 rotnodens namn.</span><span class="sxs-lookup"><span data-stu-id="ea96c-111">tooprocess hello message, you need toodeploy a schema with hello UNH2.5 root node name.</span></span>  <span data-ttu-id="ea96c-112">För ett exempel hello schemanamnet rot skulle vara **EFACT_D03B_ORDERS_EAN008**</span><span class="sxs-lookup"><span data-stu-id="ea96c-112">For given an example, hello schema root name would be **EFACT_D03B_ORDERS_EAN008**</span></span>  

<span data-ttu-id="ea96c-113">Du skulle ha en enskild schemat toodeploy för varje D03B_ORDERS med ett annat UNH2.5 segment.</span><span class="sxs-lookup"><span data-stu-id="ea96c-113">For each D03B_ORDERS with a different UNH2.5 segment, you would have toodeploy an individual schema.</span></span>  

## <a name="add-schema-toohello-edifact-agreement"></a><span data-ttu-id="ea96c-114">Lägga till schemat toohello EDIFACT-avtal</span><span class="sxs-lookup"><span data-stu-id="ea96c-114">Add schema toohello EDIFACT agreement</span></span>
### <a name="edifact-decode"></a><span data-ttu-id="ea96c-115">EDIFACT avkoda</span><span class="sxs-lookup"><span data-stu-id="ea96c-115">EDIFACT Decode</span></span>
<span data-ttu-id="ea96c-116">tooDecode Hej inkommande meddelande, konfigurera hello schemat i hello EDIFACT-avtal får inställningar</span><span class="sxs-lookup"><span data-stu-id="ea96c-116">tooDecode hello incoming message, configure hello schema in hello EDIFACT agreement receive settings</span></span>
1. <span data-ttu-id="ea96c-117">Lägg till hello schemat toohello integrering konto</span><span class="sxs-lookup"><span data-stu-id="ea96c-117">Add hello schema toohello integration account</span></span>    
2. <span data-ttu-id="ea96c-118">Konfigurera hello schemat i hello EDIFACT-avtal får inställningar.</span><span class="sxs-lookup"><span data-stu-id="ea96c-118">Configure hello schema in hello EDIFACT agreement receive settings.</span></span> 
3. <span data-ttu-id="ea96c-119">Välj EDIFACT-avtal och klickar på **redigera som JSON**.</span><span class="sxs-lookup"><span data-stu-id="ea96c-119">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="ea96c-120">Lägg till UNH2.5 värde i hello Receive avtal **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ea96c-120">Add UNH2.5 value in hello Receive Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png)</span></span>

### <a name="edifact-encode"></a><span data-ttu-id="ea96c-121">EDIFACT koda</span><span class="sxs-lookup"><span data-stu-id="ea96c-121">EDIFACT Encode</span></span>
<span data-ttu-id="ea96c-122">tooEncode Hej inkommande meddelande, konfigurera hello schemat i hello EDIFACT-avtal skicka inställningar</span><span class="sxs-lookup"><span data-stu-id="ea96c-122">tooEncode hello incoming message, configure hello schema in hello EDIFACT agreement send settings</span></span>
1. <span data-ttu-id="ea96c-123">Lägg till hello schemat toohello integrering konto</span><span class="sxs-lookup"><span data-stu-id="ea96c-123">Add hello schema toohello integration account</span></span>    
2. <span data-ttu-id="ea96c-124">Konfigurera hello schemat i hello EDIFACT-avtal skicka inställningar.</span><span class="sxs-lookup"><span data-stu-id="ea96c-124">Configure hello schema in hello EDIFACT agreement send settings.</span></span> 
3. <span data-ttu-id="ea96c-125">Välj EDIFACT-avtal och klickar på **redigera som JSON**.</span><span class="sxs-lookup"><span data-stu-id="ea96c-125">Select EDIFACT agreement and click **Edit as JSON**.</span></span>  <span data-ttu-id="ea96c-126">Lägg till UNH2.5 värde i hello skicka avtal **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span><span class="sxs-lookup"><span data-stu-id="ea96c-126">Add UNH2.5 value in hello Send Agreement **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea96c-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea96c-127">Next Steps</span></span>
* [<span data-ttu-id="ea96c-128">Mer information om integration konto avtal</span><span class="sxs-lookup"><span data-stu-id="ea96c-128">Learn more about integration account agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  