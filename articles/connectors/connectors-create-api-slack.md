---
title: aaaUse hello Slack-anslutningen i dina Azure logic apps | Microsoft Docs
description: Ansluta tooSlack i dina logic apps
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
ms.openlocfilehash: 6599d7b69d2147425c9fab978c5d0f93e5605f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-slack-connector"></a><span data-ttu-id="1bc7e-103">Kom igång med hello Slack-koppling</span><span class="sxs-lookup"><span data-stu-id="1bc7e-103">Get started with hello Slack connector</span></span>
<span data-ttu-id="1bc7e-104">Slack är ett team kommunikationsverktyg som sammanför alla meddelanden i ett team placera, direkt sökbara och tillgänglig överallt.</span><span class="sxs-lookup"><span data-stu-id="1bc7e-104">Slack is a team communication tool, that brings together all of your team communications in one place, instantly searchable and available wherever you go.</span></span> 

<span data-ttu-id="1bc7e-105">Kom igång genom att skapa en logikapp nu. Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1bc7e-105">Get started by creating a logic app now; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-tooslack"></a><span data-ttu-id="1bc7e-106">Skapa en anslutning tooSlack</span><span class="sxs-lookup"><span data-stu-id="1bc7e-106">Create a connection tooSlack</span></span>
<span data-ttu-id="1bc7e-107">toouse hello Slack anslutningen kan du först skapa en **anslutning** ange hello information för dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="1bc7e-107">toouse hello Slack connector, you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="1bc7e-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="1bc7e-108">Property</span></span> | <span data-ttu-id="1bc7e-109">Krävs</span><span class="sxs-lookup"><span data-stu-id="1bc7e-109">Required</span></span> | <span data-ttu-id="1bc7e-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="1bc7e-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1bc7e-111">Token</span><span class="sxs-lookup"><span data-stu-id="1bc7e-111">Token</span></span> |<span data-ttu-id="1bc7e-112">Ja</span><span class="sxs-lookup"><span data-stu-id="1bc7e-112">Yes</span></span> |<span data-ttu-id="1bc7e-113">Ange Slack autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1bc7e-113">Provide Slack Credentials</span></span> |

<span data-ttu-id="1bc7e-114">Följ dessa steg toosign till Slack och fullständig hello konfiguration av hello Slack **anslutning** i din logikapp:</span><span class="sxs-lookup"><span data-stu-id="1bc7e-114">Follow these steps toosign into Slack, and complete hello configuration of hello Slack **connection** in your logic app:</span></span>

1. <span data-ttu-id="1bc7e-115">Välj **upprepning**</span><span class="sxs-lookup"><span data-stu-id="1bc7e-115">Select **Recurrence**</span></span>
2. <span data-ttu-id="1bc7e-116">Välj en **frekvens** och ange en **intervall**</span><span class="sxs-lookup"><span data-stu-id="1bc7e-116">Select a **Frequency** and enter an **Interval**</span></span>
3. <span data-ttu-id="1bc7e-117">Välj **lägga till en åtgärd**</span><span class="sxs-lookup"><span data-stu-id="1bc7e-117">Select **Add an action**</span></span>  
   <span data-ttu-id="1bc7e-118">![Konfigurera Slack][1]</span><span class="sxs-lookup"><span data-stu-id="1bc7e-118">![Configure Slack][1]</span></span>  
4. <span data-ttu-id="1bc7e-119">Ange Slack i sökrutan hello och vänta tills hello Sök tooreturn alla poster med Slack i hello namn</span><span class="sxs-lookup"><span data-stu-id="1bc7e-119">Enter Slack in hello search box and wait for hello search tooreturn all entries with Slack in hello name</span></span>
5. <span data-ttu-id="1bc7e-120">Välj **Slack - skicka meddelandet**</span><span class="sxs-lookup"><span data-stu-id="1bc7e-120">Select **Slack - Post message**</span></span>
6. <span data-ttu-id="1bc7e-121">Välj **logga in tooSlack**:</span><span class="sxs-lookup"><span data-stu-id="1bc7e-121">Select **Sign in tooSlack**:</span></span>  
   <span data-ttu-id="1bc7e-122">![Konfigurera Slack][2]</span><span class="sxs-lookup"><span data-stu-id="1bc7e-122">![Configure Slack][2]</span></span>
7. <span data-ttu-id="1bc7e-123">Ange dina autentiseringsuppgifter för Slack toosign i tooauthorize hello program</span><span class="sxs-lookup"><span data-stu-id="1bc7e-123">Provide your Slack credentials toosign in tooauthorize hello  application</span></span>    
   ![Konfigurera Slack][3]  
8. <span data-ttu-id="1bc7e-125">Du kommer att omdirigerade tooyour organisation inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="1bc7e-125">You'll be redirected tooyour organization's Log in page.</span></span> <span data-ttu-id="1bc7e-126">**Auktorisera** Slack toointeract med din logikapp:</span><span class="sxs-lookup"><span data-stu-id="1bc7e-126">**Authorize** Slack toointeract with your logic app:</span></span>      
   <span data-ttu-id="1bc7e-127">![Konfigurera Slack][5]</span><span class="sxs-lookup"><span data-stu-id="1bc7e-127">![Configure Slack][5]</span></span> 
9. <span data-ttu-id="1bc7e-128">När hello auktorisering har slutförts kommer du att omdirigerade tooyour logik app toocomplete den genom att konfigurera hello **Slack - hämta alla meddelanden** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1bc7e-128">After hello authorization completes you'll be redirected tooyour logic app toocomplete it by configuring hello **Slack - Get all messages** section.</span></span> <span data-ttu-id="1bc7e-129">Lägg till andra utlösare och åtgärder som du behöver.</span><span class="sxs-lookup"><span data-stu-id="1bc7e-129">Add other triggers and actions that you need.</span></span>  
   <span data-ttu-id="1bc7e-130">![Konfigurera Slack][6]</span><span class="sxs-lookup"><span data-stu-id="1bc7e-130">![Configure Slack][6]</span></span>
10. <span data-ttu-id="1bc7e-131">Spara ditt arbete genom att välja **spara** hello menyraden ovan.</span><span class="sxs-lookup"><span data-stu-id="1bc7e-131">Save your work by selecting **Save** on hello menu bar above.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="1bc7e-132">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="1bc7e-132">Connector-specific details</span></span>

<span data-ttu-id="1bc7e-133">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/slack/).</span><span class="sxs-lookup"><span data-stu-id="1bc7e-133">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/slack/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="1bc7e-134">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="1bc7e-134">More connectors</span></span>
<span data-ttu-id="1bc7e-135">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1bc7e-135">Go back toohello [APIs list](apis-list.md).</span></span>

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
