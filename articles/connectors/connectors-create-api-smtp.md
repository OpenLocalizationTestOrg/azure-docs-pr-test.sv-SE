---
title: SMTP-anslutaren i Azure Logic Apps | Microsoft Docs
description: Skapa logikappar med Azure App service. Anslut till SMTP om du vill skicka e-post.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-smtp-connector"></a><span data-ttu-id="94a55-104">Kom igång med SMTP-koppling</span><span class="sxs-lookup"><span data-stu-id="94a55-104">Get started with the SMTP connector</span></span>
<span data-ttu-id="94a55-105">Anslut till SMTP om du vill skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="94a55-105">Connect to SMTP to send email.</span></span>

<span data-ttu-id="94a55-106">Att använda [alla anslutningar](apis-list.md), måste du först skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="94a55-106">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="94a55-107">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="94a55-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-smtp"></a><span data-ttu-id="94a55-108">Ansluta till SMTP</span><span class="sxs-lookup"><span data-stu-id="94a55-108">Connect to SMTP</span></span>
<span data-ttu-id="94a55-109">Innan din logikapp kan komma åt någon tjänst, måste du först skapa en *anslutning* till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="94a55-109">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="94a55-110">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="94a55-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="94a55-111">Till exempel för att ansluta till SMTP måste du först en SMTP *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="94a55-111">For example, to connect to SMTP, you first need an SMTP *connection*.</span></span> <span data-ttu-id="94a55-112">Ange de autentiseringsuppgifter som du vanligtvis använder för att få åtkomst till tjänsten som du ansluter till om du vill skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="94a55-112">To create a connection, enter the credentials you normally use to access the service you connect to.</span></span> <span data-ttu-id="94a55-113">Så i SMTP-exemplet anger du autentiseringsuppgifterna till Anslutningsnamn, SMTP-serveradressen och inloggningsinformation för användare att skapa anslutning till SMTP.</span><span class="sxs-lookup"><span data-stu-id="94a55-113">So, in the SMTP example, enter the credentials to your connection name, SMTP server address, and user login information to create the connection to SMTP.</span></span>  

### <a name="create-a-connection-to-smtp"></a><span data-ttu-id="94a55-114">Skapa en anslutning till SMTP</span><span class="sxs-lookup"><span data-stu-id="94a55-114">Create a connection to SMTP</span></span>
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a><span data-ttu-id="94a55-115">Använda en SMTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="94a55-115">Use an SMTP trigger</span></span>
<span data-ttu-id="94a55-116">En utlösare är en händelse som kan användas för att starta arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="94a55-116">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="94a55-117">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="94a55-117">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="94a55-118">I det här exemplet SMTP har inte en utlösare egna, använder vi den **Salesforce - när ett objekt skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="94a55-118">In this example, because SMTP does not have a trigger of its own, we'll use the **Salesforce - When an object is created** trigger.</span></span> <span data-ttu-id="94a55-119">Den här utlösaren aktiveras när ett nytt objekt skapas i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="94a55-119">This trigger activates when a new object is created in Salesforce.</span></span> <span data-ttu-id="94a55-120">I vårt exempel vi ska konfigurera den så att varje gång en ny lead skapas i Salesforce, en *skicka e-post* sker via SMTP-anslutningen med ett meddelande för ny lead håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="94a55-120">For our example, we'll set it up such that every time a new lead is created in Salesforce, a *send email* action occurs via the SMTP connector with a notification of the new lead being created.</span></span>

1. <span data-ttu-id="94a55-121">Ange *salesforce* i sökrutan på logic apps designer väljer den **Salesforce - när ett objekt skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="94a55-121">Enter *salesforce* in the search box on the logic apps designer then select the **Salesforce - When an object is created** trigger.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. <span data-ttu-id="94a55-122">Den **när ett objekt skapas** kontrollen visas.</span><span class="sxs-lookup"><span data-stu-id="94a55-122">The **When an object is created** control is displayed.</span></span>
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. <span data-ttu-id="94a55-123">Välj den **objekttyp** Välj *leda* från listan över objekt.</span><span class="sxs-lookup"><span data-stu-id="94a55-123">Select the **Object Type** then select *Lead* from the list of objects.</span></span> <span data-ttu-id="94a55-124">I det här steget anger du att du skapar en utlösare som meddelar logikappen när en ny lead skapas i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="94a55-124">In this step you are indicating that you are creating a trigger that will notify your logic app whenever a new lead is created in Salesforce.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. <span data-ttu-id="94a55-125">Utlösaren har skapats.</span><span class="sxs-lookup"><span data-stu-id="94a55-125">The trigger has been created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a><span data-ttu-id="94a55-126">Använda en SMTP-åtgärden</span><span class="sxs-lookup"><span data-stu-id="94a55-126">Use an SMTP action</span></span>
<span data-ttu-id="94a55-127">En åtgärd är en åtgärd som utförs av arbetsflödet som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="94a55-127">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="94a55-128">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="94a55-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="94a55-129">Nu när utlösaren har lagts till, Följ dessa steg för att lägga till en SMTP-åtgärden som utförs när en ny lead skapas i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="94a55-129">Now that the trigger has been added, follow these steps to add an SMTP action that will occur when a new lead is created in Salesforce.</span></span>

1. <span data-ttu-id="94a55-130">Välj **+ nytt steg** att lägga till den åtgärd som ska vidtas när en ny lead skapas.</span><span class="sxs-lookup"><span data-stu-id="94a55-130">Select **+ New Step** to add the action you would like to take when a new lead is created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. <span data-ttu-id="94a55-131">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="94a55-131">Select **Add an action**.</span></span> <span data-ttu-id="94a55-132">Den här öppnas sökrutan där du kan söka efter något du vill göra.</span><span class="sxs-lookup"><span data-stu-id="94a55-132">This opens the search box where you can search for any action you would like to take.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. <span data-ttu-id="94a55-133">Ange *smtp* att söka efter åtgärder som rör SMTP.</span><span class="sxs-lookup"><span data-stu-id="94a55-133">Enter *smtp* to search for actions related to SMTP.</span></span>  
4. <span data-ttu-id="94a55-134">Välj **SMTP - skicka e-post** som åtgärden som ska vidtas när ny lead skapas.</span><span class="sxs-lookup"><span data-stu-id="94a55-134">Select **SMTP - Send Email** as the action to take when the new lead is created.</span></span> <span data-ttu-id="94a55-135">Åtgärden kontrollen block öppnas.</span><span class="sxs-lookup"><span data-stu-id="94a55-135">The action control block opens.</span></span> <span data-ttu-id="94a55-136">Du måste upprätta en smtp-anslutning i designern blocket om du inte har gjort det tidigare.</span><span class="sxs-lookup"><span data-stu-id="94a55-136">You will have to establish your smtp connection in the designer block if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. <span data-ttu-id="94a55-137">Ange önskad e-information på den **SMTP - skicka e-post** block.</span><span class="sxs-lookup"><span data-stu-id="94a55-137">Input your desired email information in the **SMTP - Send Email** block.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. <span data-ttu-id="94a55-138">Spara arbetet för att aktivera arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="94a55-138">Save your work in order to activate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="94a55-139">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="94a55-139">Connector-specific details</span></span>

<span data-ttu-id="94a55-140">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/smtpconnector/).</span><span class="sxs-lookup"><span data-stu-id="94a55-140">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/smtpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="94a55-141">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="94a55-141">More connectors</span></span>
<span data-ttu-id="94a55-142">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="94a55-142">Go back to the [APIs list](apis-list.md).</span></span>