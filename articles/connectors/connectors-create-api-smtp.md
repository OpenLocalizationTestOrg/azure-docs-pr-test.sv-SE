---
title: aaaSMTP connector i Azure Logic Apps | Microsoft Docs
description: Skapa logikappar med Azure App service. Ansluta tooSMTP toosend e-post.
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
ms.openlocfilehash: 36bb836851014d24f2e069fda8376ad7a08c943b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-smtp-connector"></a><span data-ttu-id="55e1e-104">Kom igång med hello SMTP-anslutaren</span><span class="sxs-lookup"><span data-stu-id="55e1e-104">Get started with hello SMTP connector</span></span>
<span data-ttu-id="55e1e-105">Ansluta tooSMTP toosend e-post.</span><span class="sxs-lookup"><span data-stu-id="55e1e-105">Connect tooSMTP toosend email.</span></span>

<span data-ttu-id="55e1e-106">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="55e1e-106">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="55e1e-107">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="55e1e-107">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toosmtp"></a><span data-ttu-id="55e1e-108">Ansluta tooSMTP</span><span class="sxs-lookup"><span data-stu-id="55e1e-108">Connect tooSMTP</span></span>
<span data-ttu-id="55e1e-109">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="55e1e-109">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="55e1e-110">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="55e1e-110">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="55e1e-111">Till exempel tooconnect tooSMTP, måste du först en SMTP *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="55e1e-111">For example, tooconnect tooSMTP, you first need an SMTP *connection*.</span></span> <span data-ttu-id="55e1e-112">toocreate en anslutning, ange hello autentiseringsuppgifter som du vanligtvis använder tooaccess hello-tjänsten som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="55e1e-112">toocreate a connection, enter hello credentials you normally use tooaccess hello service you connect to.</span></span> <span data-ttu-id="55e1e-113">Så i hello SMTP exemplet anger du hello autentiseringsuppgifter tooyour anslutningens namn, SMTP-serveradressen och användaren logga in information toocreate hello anslutning tooSMTP.</span><span class="sxs-lookup"><span data-stu-id="55e1e-113">So, in hello SMTP example, enter hello credentials tooyour connection name, SMTP server address, and user login information toocreate hello connection tooSMTP.</span></span>  

### <a name="create-a-connection-toosmtp"></a><span data-ttu-id="55e1e-114">Skapa en anslutning tooSMTP</span><span class="sxs-lookup"><span data-stu-id="55e1e-114">Create a connection tooSMTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooSMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a><span data-ttu-id="55e1e-115">Använda en SMTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="55e1e-115">Use an SMTP trigger</span></span>
<span data-ttu-id="55e1e-116">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="55e1e-116">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="55e1e-117">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="55e1e-117">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="55e1e-118">I det här exemplet SMTP har inte en utlösare egna, använder vi hello **Salesforce - när ett objekt skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="55e1e-118">In this example, because SMTP does not have a trigger of its own, we'll use hello **Salesforce - When an object is created** trigger.</span></span> <span data-ttu-id="55e1e-119">Den här utlösaren aktiveras när ett nytt objekt skapas i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="55e1e-119">This trigger activates when a new object is created in Salesforce.</span></span> <span data-ttu-id="55e1e-120">I vårt exempel vi ska konfigurera den så att varje gång en ny lead skapas i Salesforce, en *skicka e-post* sker via hello SMTP-koppling med ett meddelande om hello ny lead håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="55e1e-120">For our example, we'll set it up such that every time a new lead is created in Salesforce, a *send email* action occurs via hello SMTP connector with a notification of hello new lead being created.</span></span>

1. <span data-ttu-id="55e1e-121">Ange *salesforce* i hello sökrutan på hello logic apps designer väljer du sedan hello **Salesforce - när ett objekt skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="55e1e-121">Enter *salesforce* in hello search box on hello logic apps designer then select hello **Salesforce - When an object is created** trigger.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. <span data-ttu-id="55e1e-122">Hej **när ett objekt skapas** kontrollen visas.</span><span class="sxs-lookup"><span data-stu-id="55e1e-122">hello **When an object is created** control is displayed.</span></span>
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. <span data-ttu-id="55e1e-123">Välj hello **objekttyp** Välj *leda* hello listan över objekt.</span><span class="sxs-lookup"><span data-stu-id="55e1e-123">Select hello **Object Type** then select *Lead* from hello list of objects.</span></span> <span data-ttu-id="55e1e-124">I det här steget anger du att du skapar en utlösare som meddelar logikappen när en ny lead skapas i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="55e1e-124">In this step you are indicating that you are creating a trigger that will notify your logic app whenever a new lead is created in Salesforce.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. <span data-ttu-id="55e1e-125">hello utlösaren har skapats.</span><span class="sxs-lookup"><span data-stu-id="55e1e-125">hello trigger has been created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a><span data-ttu-id="55e1e-126">Använda en SMTP-åtgärden</span><span class="sxs-lookup"><span data-stu-id="55e1e-126">Use an SMTP action</span></span>
<span data-ttu-id="55e1e-127">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="55e1e-127">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="55e1e-128">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="55e1e-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="55e1e-129">Nu när hello utlösaren har lagts till, följa dessa steg tooadd en SMTP-åtgärden som utförs när en ny lead skapas i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="55e1e-129">Now that hello trigger has been added, follow these steps tooadd an SMTP action that will occur when a new lead is created in Salesforce.</span></span>

1. <span data-ttu-id="55e1e-130">Välj **+ nytt steg** tooadd hello åtgärd som tootake när en ny lead skapas.</span><span class="sxs-lookup"><span data-stu-id="55e1e-130">Select **+ New Step** tooadd hello action you would like tootake when a new lead is created.</span></span>  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. <span data-ttu-id="55e1e-131">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="55e1e-131">Select **Add an action**.</span></span> <span data-ttu-id="55e1e-132">Den här öppnas hello-sökrutan där du kan söka efter något du vill tootake.</span><span class="sxs-lookup"><span data-stu-id="55e1e-132">This opens hello search box where you can search for any action you would like tootake.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. <span data-ttu-id="55e1e-133">Ange *smtp* toosearch för åtgärder relaterade tooSMTP.</span><span class="sxs-lookup"><span data-stu-id="55e1e-133">Enter *smtp* toosearch for actions related tooSMTP.</span></span>  
4. <span data-ttu-id="55e1e-134">Välj **SMTP - skicka e-post** som hello åtgärd tootake när hello ny lead skapas.</span><span class="sxs-lookup"><span data-stu-id="55e1e-134">Select **SMTP - Send Email** as hello action tootake when hello new lead is created.</span></span> <span data-ttu-id="55e1e-135">hello åtgärd kontrollen block öppnas.</span><span class="sxs-lookup"><span data-stu-id="55e1e-135">hello action control block opens.</span></span> <span data-ttu-id="55e1e-136">Du har tooestablish SMTP-anslutningen i hello designer block om du inte har gjort det tidigare.</span><span class="sxs-lookup"><span data-stu-id="55e1e-136">You will have tooestablish your smtp connection in hello designer block if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. <span data-ttu-id="55e1e-137">Ange önskad e-information på hello **SMTP - skicka e-post** block.</span><span class="sxs-lookup"><span data-stu-id="55e1e-137">Input your desired email information in hello **SMTP - Send Email** block.</span></span>  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. <span data-ttu-id="55e1e-138">Spara ditt arbete i ordning tooactivate arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="55e1e-138">Save your work in order tooactivate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="55e1e-139">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="55e1e-139">Connector-specific details</span></span>

<span data-ttu-id="55e1e-140">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/smtpconnector/).</span><span class="sxs-lookup"><span data-stu-id="55e1e-140">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/smtpconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="55e1e-141">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="55e1e-141">More connectors</span></span>
<span data-ttu-id="55e1e-142">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="55e1e-142">Go back toohello [APIs list](apis-list.md).</span></span>
