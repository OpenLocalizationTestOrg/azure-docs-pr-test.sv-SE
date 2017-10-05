---
title: Validera XML - Azure Logikappar | Microsoft Docs
description: "Validera XML med scheman för Logikappar i Azure och B2B-scenarier med hjälp av Enterprise-Integrationspaket"
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
ms.openlocfilehash: 8558efffa354cc4bb93820c837077ee997924c95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="validate-xml-for-enterprise-integration"></a><span data-ttu-id="2e025-103">Validera XML för företagsintegration</span><span class="sxs-lookup"><span data-stu-id="2e025-103">Validate XML for enterprise integration</span></span>

<span data-ttu-id="2e025-104">Ofta i B2B-scenarier måste partners i ett avtal se till att de meddelanden som är giltiga för databearbetning ska kunna starta.</span><span class="sxs-lookup"><span data-stu-id="2e025-104">Often in B2B scenarios, the partners in an agreement must make sure that the messages they exchange are valid before data processing can start.</span></span> <span data-ttu-id="2e025-105">Du kan validera dokument mot ett fördefinierat schema genom att använda Använd XML-verifiering kopplingen i Enterprise-Integrationspaket.</span><span class="sxs-lookup"><span data-stu-id="2e025-105">You can validate documents against a predefined schema by using the use the XML Validation connector in the Enterprise Integration Pack.</span></span>

## <a name="validate-a-document-with-the-xml-validation-connector"></a><span data-ttu-id="2e025-106">Validera ett dokument med XML-verifiering connector</span><span class="sxs-lookup"><span data-stu-id="2e025-106">Validate a document with the XML Validation connector</span></span>

1. <span data-ttu-id="2e025-107">Skapa en logikapp och [koppla appen till kontot integration](../logic-apps/logic-apps-enterprise-integration-accounts.md "Lär dig hur du länkar ett integration konto till en logikapp") som har det schema som du vill använda för att validera XML-data.</span><span class="sxs-lookup"><span data-stu-id="2e025-107">Create a logic app, and [link the app to the integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn to link an integration account to a Logic app") that has the schema you want to use for validating XML data.</span></span>

2. <span data-ttu-id="2e025-108">Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="2e025-108">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. <span data-ttu-id="2e025-109">Att lägga till den **XML-verifiering** åtgärd, Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="2e025-109">To add the **XML Validation** action, choose **Add an action**.</span></span>

4. <span data-ttu-id="2e025-110">Om du vill filtrera alla åtgärder som du vill ange *xml* i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="2e025-110">To filter all the actions to the one that you want, enter *xml* in the search box.</span></span> <span data-ttu-id="2e025-111">Välj **XML-verifiering**.</span><span class="sxs-lookup"><span data-stu-id="2e025-111">Choose **XML Validation**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. <span data-ttu-id="2e025-112">Välj för att ange XML-innehåll som du vill validera **innehåll**.</span><span class="sxs-lookup"><span data-stu-id="2e025-112">To specify the XML content that you want to validate, select **CONTENT**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. <span data-ttu-id="2e025-113">Välj body-tagg som det innehåll som du vill validera.</span><span class="sxs-lookup"><span data-stu-id="2e025-113">Select the body tag as the content that you want to validate.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. <span data-ttu-id="2e025-114">Ange schemat som du vill använda för att validera den tidigare *innehåll* indata, Välj **SCHEMANAMNET**.</span><span class="sxs-lookup"><span data-stu-id="2e025-114">To specify the schema you want to use for validating the previous *content* input, choose **SCHEMA NAME**.</span></span>

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. <span data-ttu-id="2e025-115">Spara ditt arbete</span><span class="sxs-lookup"><span data-stu-id="2e025-115">Save your work</span></span>  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

<span data-ttu-id="2e025-116">Du är nu klar med att konfigurera validering-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2e025-116">You are now done with setting up your validation connector.</span></span> <span data-ttu-id="2e025-117">I ett verkligt program kanske du vill lagra validerade data i en line-of-business (LOB)-app som SalesForce.</span><span class="sxs-lookup"><span data-stu-id="2e025-117">In a real world application, you might want to store the validated data in a line-of-business (LOB) app like SalesForce.</span></span> <span data-ttu-id="2e025-118">Lägg till en åtgärd för att skicka verifierade utdata till Salesforce.</span><span class="sxs-lookup"><span data-stu-id="2e025-118">To send the validated output to Salesforce, add an action.</span></span>

<span data-ttu-id="2e025-119">Gör en begäran om du vill testa åtgärden verifiering för HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="2e025-119">To test your validation action, make a request to the HTTP endpoint.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e025-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e025-120">Next steps</span></span>
[<span data-ttu-id="2e025-121">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="2e025-121">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")   

