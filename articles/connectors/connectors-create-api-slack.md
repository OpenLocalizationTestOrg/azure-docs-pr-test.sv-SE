---
title: "Använd Slack-kopplingen i dina Azure logic apps | Microsoft Docs"
description: Anslut till Slack i dina logic apps
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: fc5fc128efe01bd0727e3ff30d8938918e89ac3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-slack-connector"></a><span data-ttu-id="3d3c0-103">Kom igång med Slack-koppling</span><span class="sxs-lookup"><span data-stu-id="3d3c0-103">Get started with the Slack connector</span></span>
<span data-ttu-id="3d3c0-104">Slack är ett team kommunikationsverktyg som sammanför alla meddelanden i ett team placera, direkt sökbara och tillgänglig överallt.</span><span class="sxs-lookup"><span data-stu-id="3d3c0-104">Slack is a team communication tool, that brings together all of your team communications in one place, instantly searchable and available wherever you go.</span></span> 

<span data-ttu-id="3d3c0-105">Kom igång genom att skapa en logikapp nu. Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3d3c0-105">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-slack"></a><span data-ttu-id="3d3c0-106">Skapa en anslutning till Slack</span><span class="sxs-lookup"><span data-stu-id="3d3c0-106">Create a connection to Slack</span></span>
<span data-ttu-id="3d3c0-107">Om du vill använda Slack-anslutningstjänsten måste du först skapa en **anslutning** ange detaljer för dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3d3c0-107">To use the Slack connector, you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="3d3c0-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3d3c0-108">Property</span></span> | <span data-ttu-id="3d3c0-109">Krävs</span><span class="sxs-lookup"><span data-stu-id="3d3c0-109">Required</span></span> | <span data-ttu-id="3d3c0-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3d3c0-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3d3c0-111">Token</span><span class="sxs-lookup"><span data-stu-id="3d3c0-111">Token</span></span> |<span data-ttu-id="3d3c0-112">Ja</span><span class="sxs-lookup"><span data-stu-id="3d3c0-112">Yes</span></span> |<span data-ttu-id="3d3c0-113">Ange Slack autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="3d3c0-113">Provide Slack Credentials</span></span> |

<span data-ttu-id="3d3c0-114">Följ dessa steg för att logga in till Slack och slutför konfigurationen av slacket **anslutning** i din logikapp:</span><span class="sxs-lookup"><span data-stu-id="3d3c0-114">Follow these steps to sign into Slack, and complete the configuration of the Slack **connection** in your logic app:</span></span>

1. <span data-ttu-id="3d3c0-115">Välj **upprepning**</span><span class="sxs-lookup"><span data-stu-id="3d3c0-115">Select **Recurrence**</span></span>
2. <span data-ttu-id="3d3c0-116">Välj en **frekvens** och ange en **intervall**</span><span class="sxs-lookup"><span data-stu-id="3d3c0-116">Select a **Frequency** and enter an **Interval**</span></span>
3. <span data-ttu-id="3d3c0-117">Välj **lägga till en åtgärd**</span><span class="sxs-lookup"><span data-stu-id="3d3c0-117">Select **Add an action**</span></span>  
   <span data-ttu-id="3d3c0-118">![Konfigurera Slack][1]</span><span class="sxs-lookup"><span data-stu-id="3d3c0-118">![Configure Slack][1]</span></span>  
4. <span data-ttu-id="3d3c0-119">Ange Slack i sökrutan och vänta tills sökning för att returnera alla poster med Slack i namnet</span><span class="sxs-lookup"><span data-stu-id="3d3c0-119">Enter Slack in the search box and wait for the search to return all entries with Slack in the name</span></span>
5. <span data-ttu-id="3d3c0-120">Välj **Slack - skicka meddelandet**</span><span class="sxs-lookup"><span data-stu-id="3d3c0-120">Select **Slack - Post message**</span></span>
6. <span data-ttu-id="3d3c0-121">Välj **logga in till Slack**:</span><span class="sxs-lookup"><span data-stu-id="3d3c0-121">Select **Sign in to Slack**:</span></span>  
   <span data-ttu-id="3d3c0-122">![Konfigurera Slack][2]</span><span class="sxs-lookup"><span data-stu-id="3d3c0-122">![Configure Slack][2]</span></span>
7. <span data-ttu-id="3d3c0-123">Ange din Slack autentiseringsuppgifter för att logga in att godkänna programmet</span><span class="sxs-lookup"><span data-stu-id="3d3c0-123">Provide your Slack credentials to sign in to authorize the  application</span></span>    
   ![Konfigurera Slack][3]  
8. <span data-ttu-id="3d3c0-125">Du ska omdirigeras till inloggningssidan för din organisation.</span><span class="sxs-lookup"><span data-stu-id="3d3c0-125">You'll be redirected to your organization's Log in page.</span></span> <span data-ttu-id="3d3c0-126">**Auktorisera** Slack interagerar med din logikapp:</span><span class="sxs-lookup"><span data-stu-id="3d3c0-126">**Authorize** Slack to interact with your logic app:</span></span>      
   <span data-ttu-id="3d3c0-127">![Konfigurera Slack][5]</span><span class="sxs-lookup"><span data-stu-id="3d3c0-127">![Configure Slack][5]</span></span> 
9. <span data-ttu-id="3d3c0-128">När auktoriseringen är klar ska du omdirigeras till din logikapp för att slutföra den genom att konfigurera den **Slack - hämta alla meddelanden** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3d3c0-128">After the authorization completes you'll be redirected to your logic app to complete it by configuring the **Slack - Get all messages** section.</span></span> <span data-ttu-id="3d3c0-129">Lägg till andra utlösare och åtgärder som du behöver.</span><span class="sxs-lookup"><span data-stu-id="3d3c0-129">Add other triggers and actions that you need.</span></span>  
   <span data-ttu-id="3d3c0-130">![Konfigurera Slack][6]</span><span class="sxs-lookup"><span data-stu-id="3d3c0-130">![Configure Slack][6]</span></span>
10. <span data-ttu-id="3d3c0-131">Spara ditt arbete genom att välja **spara** på menyraden ovan.</span><span class="sxs-lookup"><span data-stu-id="3d3c0-131">Save your work by selecting **Save** on the menu bar above.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="3d3c0-132">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="3d3c0-132">Connector-specific details</span></span>

<span data-ttu-id="3d3c0-133">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/slack/).</span><span class="sxs-lookup"><span data-stu-id="3d3c0-133">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/slack/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="3d3c0-134">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="3d3c0-134">More connectors</span></span>
<span data-ttu-id="3d3c0-135">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="3d3c0-135">Go back to the [APIs list](apis-list.md).</span></span>

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
