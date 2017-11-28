---
title: aaaDropbox connector i Azure Logic Apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Ansluta tooDropbox toomanage dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i Dropbox."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1f307477836104c0bc0008341604a1400860987f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-dropbox-connector"></a><span data-ttu-id="9a3de-105">Kom igång med hello Dropbox-koppling</span><span class="sxs-lookup"><span data-stu-id="9a3de-105">Get started with hello Dropbox connector</span></span>
<span data-ttu-id="9a3de-106">Ansluta tooDropbox toomanage dina filer.</span><span class="sxs-lookup"><span data-stu-id="9a3de-106">Connect tooDropbox toomanage your files.</span></span> <span data-ttu-id="9a3de-107">Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i Dropbox.</span><span class="sxs-lookup"><span data-stu-id="9a3de-107">You can perform various actions such as upload, update, get, and delete files in Dropbox.</span></span>

<span data-ttu-id="9a3de-108">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9a3de-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="9a3de-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="9a3de-109">You can get started by [creating a Logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toodropbox"></a><span data-ttu-id="9a3de-110">Ansluta tooDropbox</span><span class="sxs-lookup"><span data-stu-id="9a3de-110">Connect tooDropbox</span></span>
<span data-ttu-id="9a3de-111">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="9a3de-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="9a3de-112">En anslutning kan du ansluta en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="9a3de-112">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="9a3de-113">Till exempel i ordning tooconnect tooDropbox måste du först en Dropbox *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="9a3de-113">For example, in order tooconnect tooDropbox, you first need a Dropbox *connection*.</span></span> <span data-ttu-id="9a3de-114">toocreate en anslutning måste tooprovide hello autentiseringsuppgifter som du vanligtvis använder tooaccess Hej tjänst du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="9a3de-114">toocreate a connection, you would need tooprovide hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="9a3de-115">Så i hello Dropbox exempelvis behöver du hello autentiseringsuppgifter tooyour Dropbox-konto i ordning toocreate hello anslutning tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="9a3de-115">So, in hello Dropbox example, you would need hello credentials tooyour Dropbox account in order toocreate hello connection tooDropbox.</span></span> [<span data-ttu-id="9a3de-116">Mer information om anslutningar</span><span class="sxs-lookup"><span data-stu-id="9a3de-116">Learn more about connections</span></span>]()

### <a name="create-a-connection-toodropbox"></a><span data-ttu-id="9a3de-117">Skapa en anslutning tooDropbox</span><span class="sxs-lookup"><span data-stu-id="9a3de-117">Create a connection tooDropbox</span></span>
> [!INCLUDE [Steps toocreate a connection tooDropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a><span data-ttu-id="9a3de-118">Använda en Dropbox-utlösare</span><span class="sxs-lookup"><span data-stu-id="9a3de-118">Use a Dropbox trigger</span></span>
<span data-ttu-id="9a3de-119">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="9a3de-119">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="9a3de-120">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9a3de-120">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="9a3de-121">I det här exemplet ska vi använda hello **när en fil skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="9a3de-121">In this example, we will use hello **When a file is created** trigger.</span></span> <span data-ttu-id="9a3de-122">När den här utlösaren uppstår kan vi ringer hello **hämta innehåll med sökvägen** Dropbox-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9a3de-122">When this trigger occurs, we will call hello **Get file content using path** Dropbox action.</span></span> 

1. <span data-ttu-id="9a3de-123">Ange *dropbox* i sökrutan på hello Logic Apps designer hello sedan väljer hello **Dropbox - när en fil skapas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="9a3de-123">Enter *dropbox* in hello search box on hello Logic Apps designer, then select hello **Dropbox - When a file is created** trigger.</span></span>      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. <span data-ttu-id="9a3de-124">Välj hello mapp där du vill skapa tootrack.</span><span class="sxs-lookup"><span data-stu-id="9a3de-124">Select hello folder in which you want tootrack file creation.</span></span> <span data-ttu-id="9a3de-125">Välj... (som identifieras i hello röd ruta) och bläddra toohello mapp som du vill tooselect för hello utlösare input.</span><span class="sxs-lookup"><span data-stu-id="9a3de-125">Select ... (identified in hello red box) and browse toohello folder you wish tooselect for hello trigger's input.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a><span data-ttu-id="9a3de-126">Använda en Dropbox-åtgärd</span><span class="sxs-lookup"><span data-stu-id="9a3de-126">Use a Dropbox action</span></span>
<span data-ttu-id="9a3de-127">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="9a3de-127">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="9a3de-128">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="9a3de-128">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="9a3de-129">Nu när hello utlösaren har lagts till, följa dessa steg tooadd en åtgärd som kommer att få hello nya dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="9a3de-129">Now that hello trigger has been added, follow these steps tooadd an action that will get hello new file's content.</span></span>

1. <span data-ttu-id="9a3de-130">Välj **+ nytt steg** tooadd hello åtgärd som tootake när en ny fil skapas.</span><span class="sxs-lookup"><span data-stu-id="9a3de-130">Select **+ New Step** tooadd hello action you would like tootake when a new file is created.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. <span data-ttu-id="9a3de-131">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="9a3de-131">Select **Add an action**.</span></span> <span data-ttu-id="9a3de-132">Den här öppnas hello-sökrutan där du kan söka efter något du vill tootake.</span><span class="sxs-lookup"><span data-stu-id="9a3de-132">This opens hello search box where you can search for any action you would like tootake.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. <span data-ttu-id="9a3de-133">Ange *dropbox* toosearch för åtgärder relaterade tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="9a3de-133">Enter *dropbox* toosearch for actions related tooDropbox.</span></span>  
4. <span data-ttu-id="9a3de-134">Välj **Dropbox - Get-filinnehåll med sökvägen** hello åtgärd tootake när en ny fil skapas i hello markerad Dropbox-mappen.</span><span class="sxs-lookup"><span data-stu-id="9a3de-134">Select **Dropbox - Get file content using path** as hello action tootake when a new file is created in hello selected Dropbox folder.</span></span> <span data-ttu-id="9a3de-135">hello åtgärd kontrollen block öppnas.</span><span class="sxs-lookup"><span data-stu-id="9a3de-135">hello action control block opens.</span></span> <span data-ttu-id="9a3de-136">Du kommer att tillfrågas tooauthorize din logik app tooaccess din Dropbox-konto om du inte har gjort det tidigare.</span><span class="sxs-lookup"><span data-stu-id="9a3de-136">You will be prompted tooauthorize your logic app tooaccess your Dropbox account if you have not done so previously.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. <span data-ttu-id="9a3de-137">Välj... (på hello höger sida av hello **filsökväg** kontroll) och bläddra toohello sökväg som toouse.</span><span class="sxs-lookup"><span data-stu-id="9a3de-137">Select ... (located at hello right side of hello **File Path** control) and browse toohello file path you would like toouse.</span></span> <span data-ttu-id="9a3de-138">Eller Använd hello **filsökväg** token toospeed in genereringen av din app logik.</span><span class="sxs-lookup"><span data-stu-id="9a3de-138">Or, use hello **file path** token toospeed up your logic app creation.</span></span>  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. <span data-ttu-id="9a3de-139">Spara ditt arbete och skapa en ny fil i Dropbox tooactivate arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="9a3de-139">Save your work and create a new file in Dropbox tooactivate your workflow.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="9a3de-140">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="9a3de-140">Connector-specific details</span></span>

<span data-ttu-id="9a3de-141">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/dropbox/).</span><span class="sxs-lookup"><span data-stu-id="9a3de-141">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/dropbox/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="9a3de-142">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="9a3de-142">More connectors</span></span>
<span data-ttu-id="9a3de-143">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="9a3de-143">Go back toohello [APIs list](apis-list.md).</span></span>
