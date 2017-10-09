---
title: "aaaConnect tooDynamics 365 (online) från Azure Logic Apps | Microsoft Docs"
description: "Skapa logik app arbetsflöden som hanterar Dynamics 365 (online) enheter via hello API som tillhandahålls av hello Dynamics 365 connector"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: 183d7a6b8e5d2c0eecc70da0da3806e06c382df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toodynamics-365-from-logic-app-workflows"></a><span data-ttu-id="5c0ff-103">Ansluta tooDynamics 365 från logik app arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="5c0ff-103">Connect tooDynamics 365 from logic app workflows</span></span>

<span data-ttu-id="5c0ff-104">Du kan ansluta tooDynamics 365 (online) och skapa användbart business-flöden som skapar posterna, uppdatera objekt eller returnera en lista över poster med Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-104">With Logic Apps, you can connect tooDynamics 365 (online) and create useful business flows that create records, update items, or return a list of records.</span></span> <span data-ttu-id="5c0ff-105">Med hello Dynamics 365 connector kan du:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-105">With hello Dynamics 365 connector, you can:</span></span>

* <span data-ttu-id="5c0ff-106">Skapa ditt företag flödet baserat på hello data du får från Dynamics 365 (online).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-106">Build your business flow based on hello data you get from Dynamics 365 (online).</span></span>
* <span data-ttu-id="5c0ff-107">Använd åtgärder som får ett svar och sedan göra hello utdata för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-107">Use actions that get a response and then make hello output available for other actions.</span></span> <span data-ttu-id="5c0ff-108">När ett objekt uppdateras i Dynamics 365 (online), kan du skicka ett e-postmeddelande med hjälp av Office 365.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-108">For example, when an item is updated in Dynamics 365 (online), you can send an email using Office 365.</span></span>

<span data-ttu-id="5c0ff-109">Det här avsnittet visar hur toocreate en logikapp som skapar en uppgift i Dynamics 365 när en ny lead skapas i Dynamics 365.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-109">This topic shows you how toocreate a logic app that creates a task in Dynamics 365 whenever a new lead is created in Dynamics 365.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c0ff-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5c0ff-110">Prerequisites</span></span>
* <span data-ttu-id="5c0ff-111">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-111">An Azure account.</span></span>
* <span data-ttu-id="5c0ff-112">En Dynamics 365 (online)-konto.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-112">A Dynamics 365 (online) account.</span></span>

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a><span data-ttu-id="5c0ff-113">Skapa en uppgift när en ny lead skapas i Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="5c0ff-113">Create a task when a new lead is created in Dynamics 365</span></span>

1.  <span data-ttu-id="5c0ff-114">[Logga in tooAzure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-114">[Sign in tooAzure](https://portal.azure.com).</span></span>

2.  <span data-ttu-id="5c0ff-115">Skriv i hello Azure search `Logic apps`, och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-115">In hello Azure search box, type `Logic apps`, and press ENTER.</span></span>

      ![Sök efter Logic Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  <span data-ttu-id="5c0ff-117">Under **logikappar**, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-117">Under **Logic apps**, click **Add**.</span></span>

      ![Lägg till LogicApp](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  <span data-ttu-id="5c0ff-119">toocreate hello logikapp, en fullständig hello **namn**, **prenumeration**, **resursgruppen**, och **plats** fält och klicka sedan på  **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-119">toocreate hello logic app, complete hello **Name**, **Subscription**, **Resource Group**, and **Location** fields, and then click **Create**.</span></span>

5.  <span data-ttu-id="5c0ff-120">Välj hello nya logikappen.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-120">Select hello new logic app.</span></span> <span data-ttu-id="5c0ff-121">När du får hello **distributionen lyckades** meddelande, klicka på **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-121">When you receive hello **Deployment Succeeded** notification, click **Refresh**.</span></span>

6.  <span data-ttu-id="5c0ff-122">Under **utvecklingsverktyg**, klickar du på **logik App Designer**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-122">Under **Development Tools**, click **Logic App Designer**.</span></span> <span data-ttu-id="5c0ff-123">I hello mall klickar du på **tom Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-123">In hello template list, click **Blank Logic App**.</span></span>

7.  <span data-ttu-id="5c0ff-124">Skriv i sökrutan hello `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-124">In hello search box, type `Dynamics 365`.</span></span> <span data-ttu-id="5c0ff-125">Från hello Dynamics 365 utlöser listan, Välj **Dynamics 365 – när en post skapas**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-125">From hello Dynamics 365 triggers list, select **Dynamics 365 – When a record is created**.</span></span>

8.  <span data-ttu-id="5c0ff-126">Om du tillfrågas toosign i tooDynamics 365 kan du göra det nu.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-126">If you are prompted toosign in tooDynamics 365, do so now.</span></span>

9.  <span data-ttu-id="5c0ff-127">Ange hello följande information i hello utlösaren information:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-127">In hello trigger details, enter hello following information:</span></span>

  * <span data-ttu-id="5c0ff-128">**Organisationens namn**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-128">**Organization Name**.</span></span> <span data-ttu-id="5c0ff-129">Välj hello Dynamics 365 instans som du vill hello logik app toolisten till.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-129">Select hello Dynamics 365 instance that you want hello logic app toolisten to.</span></span>

  * <span data-ttu-id="5c0ff-130">**Entitetsnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-130">**Entity Name**.</span></span> <span data-ttu-id="5c0ff-131">Välj hello entitet som du vill toolisten till.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-131">Select hello entity that you want toolisten to.</span></span> <span data-ttu-id="5c0ff-132">Den här händelsen fungerar som en utlösare toostart hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-132">This event acts as a trigger toostart hello logic app.</span></span> 
  <span data-ttu-id="5c0ff-133">I den här genomgången **leder** är markerad.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-133">In this walkthrough, **Leads** is selected.</span></span>

  * <span data-ttu-id="5c0ff-134">**Hur ofta ska toocheck objekt?**</span><span class="sxs-lookup"><span data-stu-id="5c0ff-134">**How often do you want toocheck for items?**</span></span> <span data-ttu-id="5c0ff-135">Dessa värden anger hur ofta hello logikapp söker efter uppdateringar relaterade toohello utlösare.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-135">These values set how often hello logic app checks for updates related toohello trigger.</span></span> <span data-ttu-id="5c0ff-136">hello standardinställningen är toocheck för uppdateringar tre minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-136">hello default setting is toocheck for updates every three minutes.</span></span>

    * <span data-ttu-id="5c0ff-137">**Frekvensen**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-137">**Frequency**.</span></span> <span data-ttu-id="5c0ff-138">Välj sekunder, minuter, timmar eller dagar.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-138">Select seconds, minutes, hours, or days.</span></span>

    * <span data-ttu-id="5c0ff-139">**Intervallet**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-139">**Interval**.</span></span> <span data-ttu-id="5c0ff-140">Ange hello antal sekunder, minuter, timmar eller dagar som du vill toopass innan hello nästa kontroll.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-140">Enter hello number of seconds, minutes, hours, or days that you want toopass before hello next check.</span></span>

      ![Logik Apputlösare information](./media/connectors-create-api-crmonline/trigger-details.png)

10. <span data-ttu-id="5c0ff-142">Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-142">Click **New step**, and then click **Add an action**.</span></span>

11. <span data-ttu-id="5c0ff-143">Skriv i sökrutan hello `Dynamics 365`.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-143">In hello search box, type `Dynamics 365`.</span></span> <span data-ttu-id="5c0ff-144">Välj hello åtgärder listan **Dynamics 365 – skapa en ny post**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-144">From hello actions list, select **Dynamics 365 – Create a new record**.</span></span>

12. <span data-ttu-id="5c0ff-145">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-145">Enter hello following information:</span></span>

    * <span data-ttu-id="5c0ff-146">**Organisationens namn**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-146">**Organization Name**.</span></span> <span data-ttu-id="5c0ff-147">Välj hello Dynamics 365 instans där du vill att hello flödet toocreate hello post.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-147">Select hello Dynamics 365 instance where you want hello flow toocreate hello record.</span></span> 
    <span data-ttu-id="5c0ff-148">Observera att den här instansen inte har toobe hello samma instans där hello händelsen utlöses från.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-148">Notice that this instance doesn’t have toobe hello same instance where hello event is triggered from.</span></span>

    * <span data-ttu-id="5c0ff-149">**Entitetsnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-149">**Entity Name**.</span></span> <span data-ttu-id="5c0ff-150">Välj hello entitet som du vill toocreate en post när hello händelsen utlöses.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-150">Select hello entity that you want toocreate a record when hello event is triggered.</span></span> 
    <span data-ttu-id="5c0ff-151">I den här genomgången **uppgifter** är markerad.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-151">In this walkthrough, **Tasks** is selected.</span></span>

13. <span data-ttu-id="5c0ff-152">Klicka i hello **ämne** som visas.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-152">Click in hello **Subject** box that appears.</span></span> <span data-ttu-id="5c0ff-153">Du kan välja något av dessa fält hello dynamiskt innehåll listan som visas:</span><span class="sxs-lookup"><span data-stu-id="5c0ff-153">From hello dynamic content list that appears, you can select either of these fields:</span></span>

    * <span data-ttu-id="5c0ff-154">**Efternamn**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-154">**Last Name**.</span></span> <span data-ttu-id="5c0ff-155">Att välja det här fältet infogas hello efternamn för hello lead hello ämnesfältet för hello aktivitet när posten för hello uppgift har skapats.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-155">Selecting this field inserts hello last name for hello lead into hello Subject field for hello task, when hello task record is created.</span></span>
    * <span data-ttu-id="5c0ff-156">**Avsnittet**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-156">**Topic**.</span></span> <span data-ttu-id="5c0ff-157">Att välja fältet fältet infogningar hello avsnittet för hello lead till hello ämnesfältet för hello aktivitet när hello uppgiftspost skapas.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-157">Selecting this field inserts hello Topic field for hello lead into hello Subject field for hello task, when hello task record is created.</span></span> 
    <span data-ttu-id="5c0ff-158">Klicka på **avsnittet** tooadd som toohello **ämne** rutan.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-158">Click **Topic** tooadd that toohello **Subject** box.</span></span>

      ![Logik App skapa nya registrera information](./media/connectors-create-api-crmonline/create-record-details.png)

14. <span data-ttu-id="5c0ff-160">Hello logik App Designer i verktygsfältet klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-160">On hello Logic App Designer toolbar, click **Save**.</span></span>

    ![Logik App Designer verktygsfältet spara](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. <span data-ttu-id="5c0ff-162">toostart hello Logikapp, klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-162">toostart hello Logic App, click **Run**.</span></span>

    ![Logik App Designer verktygsfältet spara](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. <span data-ttu-id="5c0ff-164">Nu skapa en lead-post i Dynamics 365 för försäljning och se din flödet i praktiken!</span><span class="sxs-lookup"><span data-stu-id="5c0ff-164">Now create a lead record in Dynamics 365 for Sales and see your flow in action!</span></span>

## <a name="set-advanced-options-for-a-logic-app-step"></a><span data-ttu-id="5c0ff-165">Ange avancerade alternativ för en logik app steg</span><span class="sxs-lookup"><span data-stu-id="5c0ff-165">Set advanced options for a logic app step</span></span>

<span data-ttu-id="5c0ff-166">hur toofilter data i en app steg logik Klicka toospecify **visa avancerade alternativ** steget och sedan lägga till ett filter eller ordning av frågan.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-166">toospecify how toofilter data in a logic app step, click **Show advanced options** in that step, then add a filter or order by query.</span></span>

<span data-ttu-id="5c0ff-167">Du kan till exempel använder en filter frågan tooget endast aktiva konton och sortera efter hello kontonamn.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-167">For example, you can use a filter query tooget only active accounts and order by hello account name.</span></span> <span data-ttu-id="5c0ff-168">tooperform detta uppgift, ange hello OData-filterfråga `statuscode eq 1`, och välj **kontonamn** hello dynamiskt innehåll listan.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-168">tooperform this task, enter hello OData filter query `statuscode eq 1`, and select **Account Name** from hello dynamic content list.</span></span> <span data-ttu-id="5c0ff-169">Mer information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) och [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-169">More information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) and [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).</span></span>

![Logikapp avancerade alternativ](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a><span data-ttu-id="5c0ff-171">Rekommenderade metoder när du använder avancerade alternativ</span><span class="sxs-lookup"><span data-stu-id="5c0ff-171">Best practices when using advanced options</span></span>

<span data-ttu-id="5c0ff-172">När du lägger till tooa värdefältet matcha du fälttypen hello om du skriver ett värde eller välj ett värde från hello dynamiska innehållslistan.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-172">When you add a value tooa field, you must match hello field type whether you type a value or select a value from hello dynamic content list.</span></span>

<span data-ttu-id="5c0ff-173">Typ av fält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-173">Field type</span></span>  |<span data-ttu-id="5c0ff-174">Hur toouse</span><span class="sxs-lookup"><span data-stu-id="5c0ff-174">How toouse</span></span>  |<span data-ttu-id="5c0ff-175">Där toofind</span><span class="sxs-lookup"><span data-stu-id="5c0ff-175">Where toofind</span></span>  |<span data-ttu-id="5c0ff-176">Namn</span><span class="sxs-lookup"><span data-stu-id="5c0ff-176">Name</span></span>  |<span data-ttu-id="5c0ff-177">Datatyp</span><span class="sxs-lookup"><span data-stu-id="5c0ff-177">Data type</span></span>  
---------|---------|---------|---------|---------
<span data-ttu-id="5c0ff-178">Textfält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-178">Text fields</span></span>|<span data-ttu-id="5c0ff-179">Textfält kräver en rad med text eller dynamiskt innehåll som är ett fält av typen text.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-179">Text fields require a single line of text or dynamic content that is a text type field.</span></span> <span data-ttu-id="5c0ff-180">Exempel inkluderar hello kategori och underkategori fält.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-180">Examples include hello Category and Sub-Category fields.</span></span>|<span data-ttu-id="5c0ff-181">Inställningar > anpassningar > Anpassa hello System > enheter > aktivitet > fält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-181">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="5c0ff-182">category</span><span class="sxs-lookup"><span data-stu-id="5c0ff-182">category</span></span> |<span data-ttu-id="5c0ff-183">Rad med Text</span><span class="sxs-lookup"><span data-stu-id="5c0ff-183">Single Line of Text</span></span>        
<span data-ttu-id="5c0ff-184">Heltalsfält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-184">Integer fields</span></span> | <span data-ttu-id="5c0ff-185">Vissa fält kräver heltal eller dynamiskt innehåll som är av typen Integer.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-185">Some fields require integer or dynamic content that is an integer type field.</span></span> <span data-ttu-id="5c0ff-186">Exempel: procent färdigt och varaktighet.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-186">Examples include Percent Complete and Duration.</span></span> |<span data-ttu-id="5c0ff-187">Inställningar > anpassningar > Anpassa hello System > enheter > aktivitet > fält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-187">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="5c0ff-188">värdet</span><span class="sxs-lookup"><span data-stu-id="5c0ff-188">percentcomplete</span></span> |<span data-ttu-id="5c0ff-189">Heltal</span><span class="sxs-lookup"><span data-stu-id="5c0ff-189">Whole Number</span></span>         
<span data-ttu-id="5c0ff-190">Datumfält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-190">Date fields</span></span> | <span data-ttu-id="5c0ff-191">Vissa fält kräver ett datum som anges i formatet åååå-mm-dd eller dynamiskt innehåll som är ett fält av typen date.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-191">Some fields require a date entered in mm/dd/yyyy format or dynamic content that is a date type field.</span></span> <span data-ttu-id="5c0ff-192">Exempel är skapad, startdatum, verklig Start senast på håller tid, Verkligt slut och förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-192">Examples include Created On, Start Date, Actual Start, Last on Hold Time, Actual End, and Due Date.</span></span> | <span data-ttu-id="5c0ff-193">Inställningar > anpassningar > Anpassa hello System > enheter > aktivitet > fält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-193">Settings > Customizations > Customize hello System > Entities > Task > Fields</span></span> |<span data-ttu-id="5c0ff-194">createdon</span><span class="sxs-lookup"><span data-stu-id="5c0ff-194">createdon</span></span> |<span data-ttu-id="5c0ff-195">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="5c0ff-195">Date and Time</span></span>
<span data-ttu-id="5c0ff-196">Ange fälten som kräver både en post-ID och sökning</span><span class="sxs-lookup"><span data-stu-id="5c0ff-196">Fields that require both a record ID and lookup type</span></span> |<span data-ttu-id="5c0ff-197">Vissa fält som refererar till en annan entitetspost kräver både hello post-ID och hello sökning.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-197">Some fields that reference another entity record require both hello record ID and hello lookup type.</span></span> |<span data-ttu-id="5c0ff-198">Inställningar > anpassningar > Anpassa hello System > enheter > konto > fält</span><span class="sxs-lookup"><span data-stu-id="5c0ff-198">Settings > Customizations > Customize hello System > Entities > Account > Fields</span></span>  | <span data-ttu-id="5c0ff-199">accountid</span><span class="sxs-lookup"><span data-stu-id="5c0ff-199">accountid</span></span>  | <span data-ttu-id="5c0ff-200">Primär nyckel</span><span class="sxs-lookup"><span data-stu-id="5c0ff-200">Primary Key</span></span>

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a><span data-ttu-id="5c0ff-201">Fler exempel på fält som kräver en post-ID och ett sökning skriver</span><span class="sxs-lookup"><span data-stu-id="5c0ff-201">More examples of fields that require both a record ID and lookup type</span></span>
<span data-ttu-id="5c0ff-202">Expandera på hello föregående tabell följer fler exempel på fält som inte fungerar med värden som väljs i hello dynamiska innehållslistan.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-202">Expanding on hello previous table, here are more examples of fields that don't work with values selected from hello dynamic content list.</span></span> <span data-ttu-id="5c0ff-203">I stället kräver dessa fält båda ID och sökning posttyp anges i hello fält i PowerApps.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-203">Instead, these fields require both a record ID and lookup type entered into hello fields in PowerApps.</span></span>  
* <span data-ttu-id="5c0ff-204">Ägare och typen ägare.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-204">Owner and Owner Type.</span></span> <span data-ttu-id="5c0ff-205">fältet för hello ägare måste vara ett giltigt användar- eller post-ID.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-205">hello Owner field must be a valid user or team record ID.</span></span> <span data-ttu-id="5c0ff-206">hello typen ägare måste vara antingen **systemusers** eller **team**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-206">hello Owner Type must be either **systemusers** or **teams**.</span></span>
* <span data-ttu-id="5c0ff-207">Kund och kunden.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-207">Customer and Customer Type.</span></span> <span data-ttu-id="5c0ff-208">hello kund-fältet måste vara ett giltigt konto eller kontakta post-ID.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-208">hello Customer field must be a valid account or contact record ID.</span></span> <span data-ttu-id="5c0ff-209">hello typen ägare måste vara antingen **konton** eller **kontakter**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-209">hello Owner Type must be either **accounts** or **contacts**.</span></span>
* <span data-ttu-id="5c0ff-210">Angående och för typen.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-210">Regarding and Regarding Type.</span></span> <span data-ttu-id="5c0ff-211">hello om fältet måste vara en giltig post-ID, till exempel ett konto eller kontakta post-ID.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-211">hello Regarding field must be a valid record ID, such as an account or contact record ID.</span></span> <span data-ttu-id="5c0ff-212">hello angående typ måste vara hello sökning typ för hello post som **konton** eller **kontakter**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-212">hello Regarding Type must be hello lookup type for hello record, such as **accounts** or **contacts**.</span></span>

<span data-ttu-id="5c0ff-213">hello läggs följande aktivitet Skapa åtgärd exempel en kontopost som motsvarar toohello-ID för att lägga till den toohello om fältet för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-213">hello following task creation action example adds an account record that corresponds toohello record ID adding it toohello regarding field of hello task.</span></span>

![Flödet recordId och typ för kontot](./media/connectors-create-api-crmonline/recordid-type-account.png)

<span data-ttu-id="5c0ff-215">Det här exemplet ger också hello uppgiften tooa specifika användare baserat på hello användarens post-ID.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-215">This example also assigns hello task tooa specific user based on hello user's record ID.</span></span>

![Flödet recordId och typ för kontot](./media/connectors-create-api-crmonline/recordid-type-user.png)

<span data-ttu-id="5c0ff-217">toofind en post har ID, se följande avsnitt hello: *hitta hello post-ID*</span><span class="sxs-lookup"><span data-stu-id="5c0ff-217">toofind a record's ID, see hello following section: *Find hello record ID*</span></span>

## <a name="find-hello-record-id"></a><span data-ttu-id="5c0ff-218">Hitta hello post-ID</span><span class="sxs-lookup"><span data-stu-id="5c0ff-218">Find hello record ID</span></span>

1. <span data-ttu-id="5c0ff-219">Öppna en post, till exempel en kontopost.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-219">Open a record, such as an account record.</span></span>

2. <span data-ttu-id="5c0ff-220">Hello åtgärder i verktygsfältet klickar du på **Pop-ut** ![popout post](./media/connectors-create-api-crmonline/popout-record.png).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-220">On hello actions toolbar, click **Pop Out** ![popout record](./media/connectors-create-api-crmonline/popout-record.png).</span></span>
<span data-ttu-id="5c0ff-221">Du kan också hello åtgärder toocopy hello fullständiga URL: en till e-postprogram, klicka på verktygsfältet **e-post en länk**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-221">Alternatively, on hello actions toolbar, toocopy hello full URL into your default email program, click **EMAIL A LINK**.</span></span>

   <span data-ttu-id="5c0ff-222">hello post-ID visas mellan hello 7b och %7 %d tecken i hello URL-kodning.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-222">hello record ID is displayed in between hello %7b and %7d encoding characters of hello URL.</span></span>

   ![Flödet recordId och typ för kontot](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a><span data-ttu-id="5c0ff-224">Felsökning</span><span class="sxs-lookup"><span data-stu-id="5c0ff-224">Troubleshooting</span></span>
<span data-ttu-id="5c0ff-225">tootroubleshoot ett steg som inte i en logikapp, visa hello statusinformation om hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-225">tootroubleshoot a failed step in a logic app, view hello status details of hello event.</span></span>

1. <span data-ttu-id="5c0ff-226">Under **Logikappar**, Välj din logikapp och klicka sedan på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-226">Under **Logic Apps**, select your logic app, and then click **Overview**.</span></span> 

   <span data-ttu-id="5c0ff-227">hello sammanfattningen visas samt hello kör status för hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-227">hello Summary area is shown and provides hello run status for hello logic app.</span></span> 

   ![Logikapp Körstatus](./media/connectors-create-api-crmonline/tshoot1.png)

2. <span data-ttu-id="5c0ff-229">tooview mer information om eventuella misslyckade körs klickar du på hello misslyckades händelse.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-229">tooview more information about any failed runs, click hello failed event.</span></span> <span data-ttu-id="5c0ff-230">tooexpand ett steg som inte klicka på steget.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-230">tooexpand a failed step, click that step.</span></span>

   ![Expandera steg som misslyckades](./media/connectors-create-api-crmonline/tshoot2.png)

   <span data-ttu-id="5c0ff-232">hello steg information visas och kan hjälpa dig att felsöka hello orsaken till hello felet.</span><span class="sxs-lookup"><span data-stu-id="5c0ff-232">hello step details appear and can help troubleshoot hello cause of hello failure.</span></span>

   ![Det gick inte steg-information](./media/connectors-create-api-crmonline/tshoot3.png)

<span data-ttu-id="5c0ff-234">Läs mer om hur du felsöker logikappar [diagnostisera fel i logik app](../logic-apps/logic-apps-diagnosing-failures.md).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-234">For more information about troubleshooting logic apps, see [Diagnosing logic app failures](../logic-apps/logic-apps-diagnosing-failures.md).</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="5c0ff-235">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="5c0ff-235">Connector-specific details</span></span>

<span data-ttu-id="5c0ff-236">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/crm/).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-236">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/crm/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5c0ff-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c0ff-237">Next steps</span></span>
<span data-ttu-id="5c0ff-238">Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="5c0ff-238">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
