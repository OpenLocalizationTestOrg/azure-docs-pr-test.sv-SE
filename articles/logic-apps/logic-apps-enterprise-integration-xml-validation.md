---
title: aaaValidate XML - Azure Logic Apps | Microsoft Docs
description: "Validera XML med scheman för Logikappar i Azure och B2B-scenarier med hjälp av hello Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 81f662d0ddf908657b54de8af0a75fff55782ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-for-enterprise-integration"></a><span data-ttu-id="062a6-103">Validera XML för företagsintegration</span><span class="sxs-lookup"><span data-stu-id="062a6-103">Validate XML for enterprise integration</span></span>

<span data-ttu-id="062a6-104">Ofta i B2B-scenarier måste hello partners i ett avtal se till att hello meddelanden som är giltiga för databearbetning ska kunna starta.</span><span class="sxs-lookup"><span data-stu-id="062a6-104">Often in B2B scenarios, hello partners in an agreement must make sure that hello messages they exchange are valid before data processing can start.</span></span> <span data-ttu-id="062a6-105">Du kan validera dokument mot ett fördefinierat schema genom att använda hello Använd hello XML-verifiering connector i hello Enterprise-Integrationspaket.</span><span class="sxs-lookup"><span data-stu-id="062a6-105">You can validate documents against a predefined schema by using hello use hello XML Validation connector in hello Enterprise Integration Pack.</span></span>

## <a name="validate-a-document-with-hello-xml-validation-connector"></a><span data-ttu-id="062a6-106">Validera ett dokument med hello XML-verifiering connector</span><span class="sxs-lookup"><span data-stu-id="062a6-106">Validate a document with hello XML Validation connector</span></span>

1. <span data-ttu-id="062a6-107">Skapa en logikapp och [länka hello app toohello integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Läs toolink en logikapp integrering konto tooa") som har hello-schema som du vill använda toouse vid verifiering av XML-data.</span><span class="sxs-lookup"><span data-stu-id="062a6-107">Create a logic app, and [link hello app toohello integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app") that has hello schema you want toouse for validating XML data.</span></span>

2. <span data-ttu-id="062a6-108">Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="062a6-108">Add a **Request - When an HTTP request is received** trigger tooyour logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. <span data-ttu-id="062a6-109">tooadd hello **XML-verifiering** åtgärd, Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="062a6-109">tooadd hello **XML Validation** action, choose **Add an action**.</span></span>

4. <span data-ttu-id="062a6-110">toofilter alla hello toohello för åtgärder som du vill ange *xml* i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="062a6-110">toofilter all hello actions toohello one that you want, enter *xml* in hello search box.</span></span> <span data-ttu-id="062a6-111">Välj **XML-verifiering**.</span><span class="sxs-lookup"><span data-stu-id="062a6-111">Choose **XML Validation**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. <span data-ttu-id="062a6-112">toospecify hello XML-innehåll som du vill toovalidate, Välj **innehåll**.</span><span class="sxs-lookup"><span data-stu-id="062a6-112">toospecify hello XML content that you want toovalidate, select **CONTENT**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. <span data-ttu-id="062a6-113">Välj hello body-tagg som hello innehåll som du vill toovalidate.</span><span class="sxs-lookup"><span data-stu-id="062a6-113">Select hello body tag as hello content that you want toovalidate.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. <span data-ttu-id="062a6-114">toospecify hello schema som du vill använda toouse för att validera hello tidigare *innehåll* indata, Välj **SCHEMANAMNET**.</span><span class="sxs-lookup"><span data-stu-id="062a6-114">toospecify hello schema you want toouse for validating hello previous *content* input, choose **SCHEMA NAME**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. <span data-ttu-id="062a6-115">Spara ditt arbete</span><span class="sxs-lookup"><span data-stu-id="062a6-115">Save your work</span></span>  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

<span data-ttu-id="062a6-116">Du är nu klar med att konfigurera validering-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="062a6-116">You are now done with setting up your validation connector.</span></span> <span data-ttu-id="062a6-117">Du kanske vill toostore hello verifiera data i en line-of-business (LOB)-app som SalesForce i ett verkligt program.</span><span class="sxs-lookup"><span data-stu-id="062a6-117">In a real world application, you might want toostore hello validated data in a line-of-business (LOB) app like SalesForce.</span></span> <span data-ttu-id="062a6-118">toosend Hej verifierade utdata tooSalesforce, lägga till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="062a6-118">toosend hello validated output tooSalesforce, add an action.</span></span>

<span data-ttu-id="062a6-119">tootest åtgärden verifiering, gör en begäran toohello HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="062a6-119">tootest your validation action, make a request toohello HTTP endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="062a6-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="062a6-120">Next steps</span></span>
[<span data-ttu-id="062a6-121">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="062a6-121">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")   

