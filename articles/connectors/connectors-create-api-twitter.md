---
title: aaaLearn hur toouse hello Twitter-anslutningen i logikappar | Microsoft Docs
description: "Översikt över Twitter-anslutningen med REST API-parametrar"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: ead4e4dc95bf894fd72b908c5375b407ba27642d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twitter-connector"></a><span data-ttu-id="75216-103">Kom igång med hello Twitter-anslutningen</span><span class="sxs-lookup"><span data-stu-id="75216-103">Get started with hello Twitter connector</span></span>
<span data-ttu-id="75216-104">Med hello Twitter-anslutningen kan du:</span><span class="sxs-lookup"><span data-stu-id="75216-104">With hello Twitter connector you can:</span></span>

* <span data-ttu-id="75216-105">Efter tweets och få tweets</span><span class="sxs-lookup"><span data-stu-id="75216-105">Post tweets and get tweets</span></span>
* <span data-ttu-id="75216-106">Åtkomst tidslinjer, vänner och blandare</span><span class="sxs-lookup"><span data-stu-id="75216-106">Access timelines, friends and followers</span></span>
* <span data-ttu-id="75216-107">Utför någon av hello andra åtgärder och utlösare som beskrivs nedan</span><span class="sxs-lookup"><span data-stu-id="75216-107">Perform any of hello other actions and triggers described below</span></span>  

<span data-ttu-id="75216-108">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="75216-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="75216-109">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="75216-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>  

## <a name="connect-tootwitter"></a><span data-ttu-id="75216-110">Ansluta tooTwitter</span><span class="sxs-lookup"><span data-stu-id="75216-110">Connect tooTwitter</span></span>
<span data-ttu-id="75216-111">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="75216-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="75216-112">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="75216-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-tootwitter"></a><span data-ttu-id="75216-113">Skapa en anslutning tooTwitter</span><span class="sxs-lookup"><span data-stu-id="75216-113">Create a connection tooTwitter</span></span>
> [!INCLUDE [Steps toocreate a connection tooTwitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a><span data-ttu-id="75216-114">Använda en Twitter-utlösare</span><span class="sxs-lookup"><span data-stu-id="75216-114">Use a Twitter trigger</span></span>
<span data-ttu-id="75216-115">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="75216-115">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="75216-116">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="75216-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="75216-117">I det här exemplet ska jag visa hur toouse hello **när en ny tweet skickas** utlösa toosearch för #Seattle och, om #Seattle hittas, uppdaterar du en fil i Dropbox med hello text från hello tweet.</span><span class="sxs-lookup"><span data-stu-id="75216-117">In this example, I will show you how toouse hello **When a new tweet is posted**  trigger toosearch for #Seattle and, if #Seattle is found, update a file in Dropbox with hello text from hello tweet.</span></span> <span data-ttu-id="75216-118">I enterprise exempelvis kan du söka efter hello för ditt företag och uppdatera en SQL-databas med hello text från hello tweet.</span><span class="sxs-lookup"><span data-stu-id="75216-118">In an enterprise example, you could search for hello name of your company and update a SQL database with hello text from hello tweet.</span></span>

1. <span data-ttu-id="75216-119">Ange *twitter* i hello sökrutan på hello logic apps designer väljer du sedan hello **Twitter - när en ny tweet skickas** utlösare</span><span class="sxs-lookup"><span data-stu-id="75216-119">Enter *twitter* in hello search box on hello logic apps designer then select hello **Twitter - When a new tweet is posted**  trigger</span></span>   
   <span data-ttu-id="75216-120">![Twitter-utlösarbild 1](./media/connectors-create-api-twitter/trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="75216-120">![Twitter trigger image 1](./media/connectors-create-api-twitter/trigger-1.png)</span></span>  
2. <span data-ttu-id="75216-121">Ange *#Seattle* i hello **söktext** kontroll</span><span class="sxs-lookup"><span data-stu-id="75216-121">Enter *#Seattle* in hello **Search Text** control</span></span>  
   <span data-ttu-id="75216-122">![Bild 2 till Twitter-utlösare](./media/connectors-create-api-twitter/trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="75216-122">![Twitter trigger image 2](./media/connectors-create-api-twitter/trigger-2.png)</span></span> 

<span data-ttu-id="75216-123">Din logikapp har nu konfigurerats med en utlösare som börjar en körning av hello andra utlösare och åtgärder i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="75216-123">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="75216-124">Det måste innehålla minst en utlösare och en åtgärd för en logik app toobe funktionella.</span><span class="sxs-lookup"><span data-stu-id="75216-124">For a logic app toobe functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="75216-125">Hello åtgärderna i hello nästa avsnitt tooadd en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="75216-125">Follow hello steps in hello next section tooadd an action.</span></span>  
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="75216-126">Lägg till ett villkor</span><span class="sxs-lookup"><span data-stu-id="75216-126">Add a condition</span></span>
<span data-ttu-id="75216-127">Eftersom vi bara är intresserad av tweets från användare med mer än 50 användare måste ett villkor som bekräftar hello antalet blandare först läggas toohello logikapp.</span><span class="sxs-lookup"><span data-stu-id="75216-127">Since we are only interested in tweets from users with more than 50 users, a condition that confirms hello number of followers must first be added toohello logic app.</span></span>  

1. <span data-ttu-id="75216-128">Välj **+ nytt steg** tooadd hello åtgärd som tootake när #Seattle hittas i en ny tweet</span><span class="sxs-lookup"><span data-stu-id="75216-128">Select **+ New step** tooadd hello action you would like tootake when #Seattle is found in a new tweet</span></span>  
   <span data-ttu-id="75216-129">![Twitter-åtgärd bild 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span><span class="sxs-lookup"><span data-stu-id="75216-129">![Twitter action image 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span></span>  
2. <span data-ttu-id="75216-130">Välj hello **Lägg till ett villkor** länk.</span><span class="sxs-lookup"><span data-stu-id="75216-130">Select hello **Add a condition** link.</span></span>  
   <span data-ttu-id="75216-131">![Twitter villkoret bild 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span><span class="sxs-lookup"><span data-stu-id="75216-131">![Twitter condition image 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span></span>  
   <span data-ttu-id="75216-132">Då öppnas hello **villkoret** kontroll där du kan kontrollera villkor som *är lika med*, *är mindre än*, *är större än*, *innehåller*osv.</span><span class="sxs-lookup"><span data-stu-id="75216-132">This opens hello **Condition** control where you can check conditions such as *is equal to*, *is less than*, *is greater than*, *contains*, etc.</span></span>  
   <span data-ttu-id="75216-133">![Bild 2 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-2.png)</span><span class="sxs-lookup"><span data-stu-id="75216-133">![Twitter condition image 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span></span>   
3. <span data-ttu-id="75216-134">Välj hello **väljer ett värde** kontroll.</span><span class="sxs-lookup"><span data-stu-id="75216-134">Select hello **Choose a value** control.</span></span>  
   <span data-ttu-id="75216-135">I den här kontrollen kan du välja en eller flera av hello egenskaper från tidigare åtgärder eller utlösare som hello värde vars hälsotillstånd kommer att utvärderas tootrue eller false.</span><span class="sxs-lookup"><span data-stu-id="75216-135">In this control, you can select one or more of hello properties from any previous actions or triggers as hello value whose condition will be evaluated tootrue or false.</span></span>
   <span data-ttu-id="75216-136">![Bild 3 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-3.png)</span><span class="sxs-lookup"><span data-stu-id="75216-136">![Twitter condition image 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span></span>   
4. <span data-ttu-id="75216-137">Välj hello **...**  tooexpand hello lista över egenskaper så att du kan se alla hello-egenskaper som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="75216-137">Select hello **...** tooexpand hello list of properties so you can see all hello properties that are available.</span></span>        
   <span data-ttu-id="75216-138">![Bild 4 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-4.png)</span><span class="sxs-lookup"><span data-stu-id="75216-138">![Twitter condition image 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span></span>   
5. <span data-ttu-id="75216-139">Välj hello **blandare antal** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="75216-139">Select hello **Followers count** property.</span></span>    
   <span data-ttu-id="75216-140">![Bild 5 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-5.png)</span><span class="sxs-lookup"><span data-stu-id="75216-140">![Twitter condition image 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span></span>   
6. <span data-ttu-id="75216-141">Observera hello blandare egenskapen Antal är nu i hello värdet kontroll.</span><span class="sxs-lookup"><span data-stu-id="75216-141">Notice hello Followers count property is now in hello value control.</span></span>    
   ![Bild 6 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. <span data-ttu-id="75216-143">Välj **är större än** hello ansvariga för listan.</span><span class="sxs-lookup"><span data-stu-id="75216-143">Select **is greater than** from hello operators list.</span></span>    
   <span data-ttu-id="75216-144">![Bild 7 till Twitter villkor](../../includes/media/connectors-create-api-twitter/condition-7.png)</span><span class="sxs-lookup"><span data-stu-id="75216-144">![Twitter condition image 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span></span>   
8. <span data-ttu-id="75216-145">Ange 50 som hello operanden för hello *är större än* operator.</span><span class="sxs-lookup"><span data-stu-id="75216-145">Enter 50 as hello operand for hello *is greater than* operator.</span></span>  
   <span data-ttu-id="75216-146">hello villkor läggs nu.</span><span class="sxs-lookup"><span data-stu-id="75216-146">hello condition is now added.</span></span> <span data-ttu-id="75216-147">Spara ditt arbete med hjälp av hello **spara** länk på hello menyn ovan.</span><span class="sxs-lookup"><span data-stu-id="75216-147">Save your work using hello **Save** link on hello menu above.</span></span>    
   <span data-ttu-id="75216-148">![Twitter villkoret bild 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span><span class="sxs-lookup"><span data-stu-id="75216-148">![Twitter condition image 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span></span>   

## <a name="use-a-twitter-action"></a><span data-ttu-id="75216-149">Använda en Twitter-åtgärd</span><span class="sxs-lookup"><span data-stu-id="75216-149">Use a Twitter action</span></span>
<span data-ttu-id="75216-150">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="75216-150">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="75216-151">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="75216-151">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="75216-152">Nu när du har lagt till en utlösare, följer du dessa steg tooadd en åtgärd som postar nya tweets med hello innehållet i hello tweets hittades av hello-utlösare.</span><span class="sxs-lookup"><span data-stu-id="75216-152">Now that you have added a trigger, follow these steps tooadd an action that will post a new tweet with hello contents of hello tweets found by hello trigger.</span></span> <span data-ttu-id="75216-153">Endast tweets från användare med mer än 50 blandare hello enligt den här genomgången kommer att publiceras.</span><span class="sxs-lookup"><span data-stu-id="75216-153">For hello purposes of this walk-through only tweets from users with more than 50 followers will be posted.</span></span>  

<span data-ttu-id="75216-154">I hello nästa steg ska du lägga till en Twitter-åtgärd som kommer efter tweets med hello egenskaper för varje tweet som har publicerats av en användare som har fler än 50 blandare.</span><span class="sxs-lookup"><span data-stu-id="75216-154">In hello next step, you will add a Twitter action that will post a tweet using some of hello properties of each tweet that has been posted by a user who has more than 50 followers.</span></span>  

1. <span data-ttu-id="75216-155">Välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="75216-155">Select **Add an action**.</span></span> <span data-ttu-id="75216-156">Då öppnas hello sökkontrollen där du kan söka efter andra åtgärder och utlösare.</span><span class="sxs-lookup"><span data-stu-id="75216-156">This opens hello search control where you can search for other actions and triggers.</span></span>  
   <span data-ttu-id="75216-157">![Twitter villkoret bild 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span><span class="sxs-lookup"><span data-stu-id="75216-157">![Twitter condition image 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span></span>   
2. <span data-ttu-id="75216-158">Ange *twitter* i sökrutan hello Välj hello **Twitter - efter tweets** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="75216-158">Enter *twitter* into hello search box then select hello **Twitter - Post a tweet** action.</span></span> <span data-ttu-id="75216-159">Då öppnas hello **efter tweets** styra där du ska ange all information för hello tweet att anslås.</span><span class="sxs-lookup"><span data-stu-id="75216-159">This opens hello **Post a tweet** control where you will enter all details for hello tweet being posted.</span></span>      
   <span data-ttu-id="75216-160">![Twitter-åtgärd bild 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span><span class="sxs-lookup"><span data-stu-id="75216-160">![Twitter action image 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span></span>   
3. <span data-ttu-id="75216-161">Välj hello **Twittra text** kontroll.</span><span class="sxs-lookup"><span data-stu-id="75216-161">Select hello **Tweet text** control.</span></span> <span data-ttu-id="75216-162">Alla utdata från tidigare åtgärder och utlösare i hello logikapp visas nu.</span><span class="sxs-lookup"><span data-stu-id="75216-162">All outputs from previous actions and triggers in hello logic app are now visible.</span></span> <span data-ttu-id="75216-163">Du kan välja något av dessa och använda dem som en del av hello tweet text hello nya tweet.</span><span class="sxs-lookup"><span data-stu-id="75216-163">You can select any of these and use them as part of hello tweet text of hello new tweet.</span></span>     
   <span data-ttu-id="75216-164">![Bild 2 till Twitter åtgärd](../../includes/media/connectors-create-api-twitter/action-2.png)</span><span class="sxs-lookup"><span data-stu-id="75216-164">![Twitter action image 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span></span>   
4. <span data-ttu-id="75216-165">Välj **användarnamn**</span><span class="sxs-lookup"><span data-stu-id="75216-165">Select **User name**</span></span>   
5. <span data-ttu-id="75216-166">Ange *säger:* hello tweet text kontrollen.</span><span class="sxs-lookup"><span data-stu-id="75216-166">Enter *says:* in hello tweet text control.</span></span> <span data-ttu-id="75216-167">Gör det bara efter användarnamn.</span><span class="sxs-lookup"><span data-stu-id="75216-167">Do this just after User name.</span></span>  
6. <span data-ttu-id="75216-168">Välj *Twittra text*.</span><span class="sxs-lookup"><span data-stu-id="75216-168">Select *Tweet text*.</span></span>       
   <span data-ttu-id="75216-169">![Bild 3 till Twitter åtgärd](../../includes/media/connectors-create-api-twitter/action-3.png)</span><span class="sxs-lookup"><span data-stu-id="75216-169">![Twitter action image 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span></span>   
7. <span data-ttu-id="75216-170">Spara ditt arbete och skickar tweets med hello #Seattle hashtaggar tooactivate arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="75216-170">Save your work and send a tweet with hello #Seattle hashtag tooactivate your workflow.</span></span>  


## <a name="connector-specific-details"></a><span data-ttu-id="75216-171">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="75216-171">Connector-specific details</span></span>

<span data-ttu-id="75216-172">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/twitterconnector/).</span><span class="sxs-lookup"><span data-stu-id="75216-172">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/twitterconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="75216-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75216-173">Next steps</span></span>
[<span data-ttu-id="75216-174">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="75216-174">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

