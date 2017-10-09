---
title: "aaaSwitch uttryck för olika åtgärder i Azure Logic Apps | Microsoft Docs"
description: "Välj olika åtgärder tooperform i logikappar utifrån uttrycken med hjälp av en switch-sats"
services: logic-apps
keywords: Switch-instruktionen
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 09ed7e4a752003aba157e9156bf4dc89ef86f5ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a><span data-ttu-id="f3dfc-104">Utför olika åtgärder i logikappar med en switch-sats</span><span class="sxs-lookup"><span data-stu-id="f3dfc-104">Perform different actions in logic apps with a switch statement</span></span>

<span data-ttu-id="f3dfc-105">När du redigerar ett arbetsflöde har ofta tootake olika åtgärder baserat på hello-värdet för ett objekt eller ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-105">When authoring a workflow, you often have tootake different actions based on hello value of an object or expression.</span></span> <span data-ttu-id="f3dfc-106">Du kanske till exempel din logik app toobehave på olika sätt beroende på hello statuskod för en HTTP-begäran eller ett alternativ som valts i ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-106">For example, you might want your logic app toobehave differently based on hello status code of an HTTP request, or an option selected in an email.</span></span>

<span data-ttu-id="f3dfc-107">Du kan använda en switch-instruktionen tooimplement dessa scenarier.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-107">You can use a switch statement tooimplement these scenarios.</span></span> <span data-ttu-id="f3dfc-108">Din logikapp kan utvärdera en token eller ett uttryck och välj hello fallet med hello samma värde tooexecute hello anges åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-108">Your logic app can evaluate a token or expression, and choose hello case with hello same value tooexecute hello specified actions.</span></span> <span data-ttu-id="f3dfc-109">Endast ett fall ska matcha hello switch-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-109">Only one case should match hello switch statement.</span></span>

> [!TIP]
> <span data-ttu-id="f3dfc-110">Precis som alla programmeringsspråk stöder switch-satser endast likheten operatörer.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-110">Like all programming languages, switch statements support only equality operators.</span></span> <span data-ttu-id="f3dfc-111">Om du behöver andra relationsoperatorer använder du en villkorssatsen, till exempel ”större än”.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-111">If you need other relational operators, such as "greater than", use a condition statement.</span></span>
> <span data-ttu-id="f3dfc-112">tooensure deterministisk körningsbeteende fall måste innehålla ett unikt och statiska värde i stället för dynamiska token eller uttryck.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-112">tooensure deterministic execution behavior, cases must contain a unique and static value instead of dynamic tokens or expression.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3dfc-113">Krav</span><span class="sxs-lookup"><span data-stu-id="f3dfc-113">Prerequisites</span></span>

- <span data-ttu-id="f3dfc-114">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-114">An active Azure subscription.</span></span> <span data-ttu-id="f3dfc-115">Om du inte har en aktiv Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/), eller försök [Logic Apps för kostnadsfri](https://tryappservice.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f3dfc-115">If you don't have an active Azure subscription, [create a free account](https://azure.microsoft.com/free/), or try [Logic Apps for free](https://tryappservice.azure.com/).</span></span>
- [<span data-ttu-id="f3dfc-116">Grundläggande kunskaper om logikappar</span><span class="sxs-lookup"><span data-stu-id="f3dfc-116">Basic knowledge about logic apps</span></span>](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-tooyour-workflow"></a><span data-ttu-id="f3dfc-117">Lägga till ett switch-instruktionen tooyour arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="f3dfc-117">Add a switch statement tooyour workflow</span></span>

<span data-ttu-id="f3dfc-118">tooshow som så här fungerar en switch-instruktionen i det här exemplet skapar en logikapp att övervakar filer upp tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-118">tooshow how a switch statement works, this example creates a logic app that monitors files uploaded tooDropbox.</span></span> <span data-ttu-id="f3dfc-119">Hello nya filer som överförs hello logikapp skickar e-post tooan godkännare som väljer om tootransfer tooSharePoint dessa filer.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-119">When hello new files are uploaded, hello logic app sends email tooan approver who chooses whether tootransfer those files tooSharePoint.</span></span> <span data-ttu-id="f3dfc-120">hello appen använder en switch-sats som utför olika åtgärder baserat på hello-värde som hello godkännare väljer.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-120">hello app uses a switch statement that performs different actions based on hello value that hello approver selects.</span></span>

1. <span data-ttu-id="f3dfc-121">Skapa en logikapp och välj den här utlösaren: **Dropbox - när en fil skapas**.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-121">Create a logic app, and select this trigger: **Dropbox - When a file is created**.</span></span>

   ![Använda Dropbox - när en fil skapas utlösare](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. <span data-ttu-id="f3dfc-123">Lägg till den här åtgärden under hello utlösare: **Outlook.com - Skicka godkännande e-post**</span><span class="sxs-lookup"><span data-stu-id="f3dfc-123">Under hello trigger, add this action: **Outlook.com - Send approval email**</span></span>

   > [!TIP]
   > <span data-ttu-id="f3dfc-124">Logikappar stöder även skicka e-scenarier för godkännande från ett Office 365 Outlook-konto.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-124">Logic apps also support sending approval email scenarios from an Office 365 Outlook account.</span></span>

   - <span data-ttu-id="f3dfc-125">Om du inte har en befintlig anslutning, uppmanas du toocreate en.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-125">If you don't have an existing connection, you're prompted toocreate one.</span></span>
   - <span data-ttu-id="f3dfc-126">Fyll i hello krävs.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-126">Fill in hello required fields.</span></span> <span data-ttu-id="f3dfc-127">Till exempel under **till**, ange hello e-postadress för att skicka e-post för hello godkännare.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-127">For example, under **To**, specify hello email address for sending hello approver email.</span></span>
   - <span data-ttu-id="f3dfc-128">Under **användaralternativ**, ange `Approve, Reject`.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-128">Under **User Options**, enter `Approve, Reject`.</span></span>

   ![Konfigurera anslutning](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. <span data-ttu-id="f3dfc-130">Lägg till en switch-sats.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-130">Add a switch statement.</span></span>

   - <span data-ttu-id="f3dfc-131">Välj **+ nytt steg** > **... Flera** > **lägga till ett switch-fall**.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-131">Select **+ New step** > **... More** > **Add a switch case**.</span></span> 
   - <span data-ttu-id="f3dfc-132">Nu vi vill tooselect hello åtgärd tooperform baserat på hello `SelectedOptions` utdata från hello *skicka e-post för godkännande* åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-132">Now we want tooselect hello action tooperform based on hello `SelectedOptions` output from hello *Send approval email* action.</span></span> 
   <span data-ttu-id="f3dfc-133">Du hittar det här fältet i hello **lägga till dynamiskt innehåll** väljare.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-133">You can find this field in hello **Add dynamic content** selector.</span></span>
   - <span data-ttu-id="f3dfc-134">Använd *fall 1* toohandle när hello godkännare väljer `Approve`.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-134">Use *Case 1* toohandle when hello approver selects `Approve`.</span></span>
     - <span data-ttu-id="f3dfc-135">Om det godkänns, kopiera hello ursprungliga filen tooSharePoint Online med hello [ **SharePoint Online - skapa filen** åtgärd](../connectors/connectors-create-api-sharepointonline.md).</span><span class="sxs-lookup"><span data-stu-id="f3dfc-135">If approved, copy hello original file tooSharePoint Online with hello [**SharePoint Online - Create file** action](../connectors/connectors-create-api-sharepointonline.md).</span></span>
     - <span data-ttu-id="f3dfc-136">Lägg till en annan åtgärd hello case toonotify användare att en ny fil är tillgänglig i SharePoint.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-136">Add another action within hello case toonotify users that a new file is available on SharePoint.</span></span>
   - <span data-ttu-id="f3dfc-137">Lägg till en annan case toohandle när användaren väljer `Reject`.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-137">Add another case toohandle when user selects `Reject`.</span></span>
     - <span data-ttu-id="f3dfc-138">Skicka ett e-postmeddelande som informerar andra godkännare att hello filen avvisas och ingen ytterligare åtgärd krävs om avvisas.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-138">If rejected, send a notification email informing other approvers that hello file is rejected and no further action is required.</span></span>
   - <span data-ttu-id="f3dfc-139">`SelectedOptions`ger endast två alternativ, så vi kan lämna hello **standard** fallet är tom.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-139">`SelectedOptions` provides only two options, so we can leave hello **Default** case empty.</span></span>

   ![Switch-instruktionen](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > <span data-ttu-id="f3dfc-141">En switch-instruktionen måste minst ett fall i tillägg toohello standardfallet.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-141">A switch statement needs at least one case in addition toohello default case.</span></span>

4. <span data-ttu-id="f3dfc-142">Ta bort hello ursprungliga filen har överförts tooDropbox genom att lägga till den här åtgärden efter hello switch-instruktionen: **Dropbox - ta bort filen**</span><span class="sxs-lookup"><span data-stu-id="f3dfc-142">After hello switch statement, delete hello original file uploaded tooDropbox by adding this action: **Dropbox - Delete file**</span></span>

5. <span data-ttu-id="f3dfc-143">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-143">Save your logic app.</span></span> <span data-ttu-id="f3dfc-144">Testa din app genom att överföra en fil tooDropbox.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-144">Test your app by uploading a file tooDropbox.</span></span> <span data-ttu-id="f3dfc-145">Du bör få ett godkännande e-postmeddelande inom kort.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-145">You should receive an approval email shortly.</span></span> <span data-ttu-id="f3dfc-146">Välj ett alternativ och Observera hello beteende.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-146">Select an option, and observe hello behavior.</span></span>

   > [!TIP]
   > <span data-ttu-id="f3dfc-147">Kolla hur för[övervaka dina logikappar](logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="f3dfc-147">Check out how too[monitor your logic apps](logic-apps-monitor-your-logic-apps.md).</span></span>

## <a name="understand-hello-code-behind-switch-statements"></a><span data-ttu-id="f3dfc-148">Förstå hello koden bakom switch-satser</span><span class="sxs-lookup"><span data-stu-id="f3dfc-148">Understand hello code behind switch statements</span></span>

<span data-ttu-id="f3dfc-149">Nu när du har skapat en logikapp med hjälp av en switch-sats, vi titta på hello kod definition bakom hello switch-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-149">Now that you successfully created a logic app using a switch statement, let's look at hello code definition behind hello switch statement.</span></span>

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* <span data-ttu-id="f3dfc-150">`"Switch"`är hello namn hello switch-instruktionen, som du kan byta namn för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-150">`"Switch"` is hello name of hello switch statement, which you can rename for readability.</span></span> 
* <span data-ttu-id="f3dfc-151">`"type": "Switch"`Anger att hello har en switch-sats.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-151">`"type": "Switch"` indicates that hello action is a switch statement.</span></span> 
* <span data-ttu-id="f3dfc-152">`"expression"`hello godkännare valda alternativ i det här exemplet och utvärderas mot varje fall som deklarerats senare i hello definition.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-152">`"expression"` is hello approver's selected option in this example and is evaluated against each case declared later in hello definition.</span></span> 
* <span data-ttu-id="f3dfc-153">`"cases"`kan innehålla valfritt antal fall.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-153">`"cases"` can contain any number of cases.</span></span> <span data-ttu-id="f3dfc-154">För varje fall `"Case *"` är hello standardnamnet hello fall där du kan byta namn för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-154">For each case, `"Case *"` is hello default name of hello case, which you can rename for readability.</span></span> 
<span data-ttu-id="f3dfc-155">`"case"`Anger hello case etiketten hello växeln uttryck används för jämförelse, och måste vara ett konstant och unikt värde.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-155">`"case"` specifies hello case label, which hello switch expression uses for comparison, and must be a constant and unique value.</span></span> <span data-ttu-id="f3dfc-156">Om inget av hello fall matchar hello switch-uttryck, åtgärder under `"default"` utförs.</span><span class="sxs-lookup"><span data-stu-id="f3dfc-156">If none of hello cases match hello switch expression, actions under `"default"` are executed.</span></span>

## <a name="get-help"></a><span data-ttu-id="f3dfc-157">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="f3dfc-157">Get help</span></span>

<span data-ttu-id="f3dfc-158">tooask frågor, besvara frågor och se vilka andra användare i Azure Logic Apps gör finns hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="f3dfc-158">tooask questions, answer questions, and see what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="f3dfc-159">toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="f3dfc-159">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3dfc-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3dfc-160">Next steps</span></span>

- <span data-ttu-id="f3dfc-161">Lär dig hur för[Lägg till villkor](logic-apps-use-logic-app-features.md)</span><span class="sxs-lookup"><span data-stu-id="f3dfc-161">Learn how too[add conditions](logic-apps-use-logic-app-features.md)</span></span>
- <span data-ttu-id="f3dfc-162">Lär dig mer om [fel och undantagshantering](logic-apps-exception-handling.md)</span><span class="sxs-lookup"><span data-stu-id="f3dfc-162">Learn about [error and exception handling](logic-apps-exception-handling.md)</span></span>
- <span data-ttu-id="f3dfc-163">Utforska mer [arbetsflöde språk funktioner](logic-apps-author-definitions.md)</span><span class="sxs-lookup"><span data-stu-id="f3dfc-163">Explore more [workflow language capabilities](logic-apps-author-definitions.md)</span></span>