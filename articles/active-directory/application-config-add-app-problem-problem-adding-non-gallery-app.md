---
title: "Problem att lägga till en icke-galleriet program | Microsoft Docs"
description: "Förstå de vanliga problem personer står inför när du lägger till anpassade program för icke-galleriet"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: bb3fc7877f4e7cafc3904fc67abd87b897874d8a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-adding-a-non-gallery-application"></a><span data-ttu-id="148cd-103">Problem att lägga till en icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="148cd-103">Problem adding a non-gallery application</span></span>

<span data-ttu-id="148cd-104">Den här artikeln hjälper dig att förstå de vanliga problem personer står inför när du lägger till **anpassade program för icke-galleriet** och vad du kan göra för att lösa problemen.</span><span class="sxs-lookup"><span data-stu-id="148cd-104">This article help you to understand the common problems people face when adding **custom non-gallery applications** and what you can do to resolve them.</span></span> 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a><span data-ttu-id="148cd-105">Jag har klickat på knappen ”Lägg till” och mitt program tog lång tid ska visas</span><span class="sxs-lookup"><span data-stu-id="148cd-105">I clicked the “add” button and my application took a long time to appear</span></span>

<span data-ttu-id="148cd-106">I vissa fall kan det ta 1 till 2 minuter (och ibland längre) för ett program ska visas när du lägger till den i din katalog.</span><span class="sxs-lookup"><span data-stu-id="148cd-106">Under some circumstances, it can take 1-2 minutes (and sometimes longer) for an application to appear after adding it to your directory.</span></span> <span data-ttu-id="148cd-107">Även om detta inte är normal prestanda ser programmet tillägg pågår genom att klicka på den **meddelanden** ikonen (sal) i övre högra hörnet på den [Azure Portal](https://portal.azure.com/) och letar efter en **pågår** eller **slutförd** meddelande med etiketten **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="148cd-107">While this is not the normal expected performance, you can see the application addition is in progress by clicking on the **Notifications** icon (the bell) in the upper right of the [Azure Portal](https://portal.azure.com/) and looking for an **In Progress** or **Completed** notification labeled **Create application**.</span></span>

<span data-ttu-id="148cd-108">Om ditt program aldrig har lagts till, eller det uppstår ett fel när du klickar på den **Lägg till** knappen, visas en **meddelande** i en **fel** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="148cd-108">If your application is never added, or you encounter an error when clicking the **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="148cd-109">Om du vill ha mer information om felet för att lära dig mer om hur du eller dela med en stöd engingeer visas mer information om felet genom att följa stegen i den [så visas detaljerad information om en portalmeddelandet](#how-to-see-the-details-of-a-portal-notification) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="148cd-109">If you want more details about the error to learn more to or share with a support engingeer, you can see more information about the error by following the steps in the [How to see the details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a><span data-ttu-id="148cd-110">Jag har klickat på knappen ”Lägg till” och mitt program visas inte</span><span class="sxs-lookup"><span data-stu-id="148cd-110">I clicked the “add” button and my application didn’t appear</span></span>

<span data-ttu-id="148cd-111">Ibland på grund av temporära problem nätverksproblem eller ett programfel kan lägga till ett program misslyckas.</span><span class="sxs-lookup"><span data-stu-id="148cd-111">Sometimes, due to transient issues, networking problems, or a bug, adding an application fail.</span></span> <span data-ttu-id="148cd-112">Du ser detta händer när du klickar på den **meddelanden** ikonen (sal) i övre högra hörnet i Azure Portal och du ser ikonen red (!) bredvid ditt **skapa program** meddelande.</span><span class="sxs-lookup"><span data-stu-id="148cd-112">You can tell this happens when you click the **Notifications** icon (the bell) in the upper right of the Azure Portal and you see a red (!) icon next to your **Create application** notification.</span></span> <span data-ttu-id="148cd-113">Detta anger att ett fel uppstod när du skapar programmet.</span><span class="sxs-lookup"><span data-stu-id="148cd-113">This indicates there was an error when creating the application.</span></span>

<span data-ttu-id="148cd-114">Om det uppstår ett fel när du klickar på den **Lägg till** knappen, visas en **meddelande** i en **fel** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="148cd-114">If you encounter an error when clicking the **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="148cd-115">Om du vill ha mer information om felet för att lära dig mer om hur du eller dela med en stöd engingeer visas mer information om felet genom att följa stegen i den [så visas detaljerad information om en portalmeddelandet](#how-to-see-the-details-of-a-portal-notification) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="148cd-115">If you want more details about the error to learn more to or share with a support engingeer, you can see more information about the error by following the steps in the [How to see the details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a><span data-ttu-id="148cd-116">Jag vet inte hur du ställer in Mina program när jag har lagt till</span><span class="sxs-lookup"><span data-stu-id="148cd-116">I don’t know how to set up my application once I’ve added it</span></span>

<span data-ttu-id="148cd-117">Om du behöver hjälp med att lära dig om anpassade program i [dokumentbibliotek för Azure AD-program](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) hjälper dig att lära dig mer om enkel inloggning med Azure AD och hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="148cd-117">If you need help learning about custom applications, the [Azure AD Applications Document Library](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) help you to learn more about single sign-on with Azure AD and how it works.</span></span>

## <a name="how-to-see-the-details-of-a-portal-notification"></a><span data-ttu-id="148cd-118">Hur du visar information om ett meddelande om portal</span><span class="sxs-lookup"><span data-stu-id="148cd-118">How to see the details of a portal notification</span></span>

<span data-ttu-id="148cd-119">Du kan se information om eventuella portalmeddelandet genom att följa stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="148cd-119">You can see the details of any portal notification by following the steps below:</span></span>

1.  <span data-ttu-id="148cd-120">Klicka på den **meddelanden** ikonen (sal) i övre högra hörnet i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="148cd-120">click the **Notifications** icon (the bell) in the upper right of the Azure Portal</span></span>

2.  <span data-ttu-id="148cd-121">Markera alla meddelanden i en **fel** tillstånd (de med ett rött (!) bredvid dem).</span><span class="sxs-lookup"><span data-stu-id="148cd-121">Select any notification in an **Error** state (those with a red (!) next to them).</span></span>

   >[!NOTE]
   ><span data-ttu-id="148cd-122">Du kan klicka på meddelanden i en **lyckade** eller **pågår** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="148cd-122">You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
   >
   >

3.  <span data-ttu-id="148cd-123">Den här öppna den **detaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="148cd-123">This open the **Notification Details** blade.</span></span>

4.  <span data-ttu-id="148cd-124">Använd den här informationen dig att förstå mer information om problemet.</span><span class="sxs-lookup"><span data-stu-id="148cd-124">Use this information yourself to understand more details about the problem.</span></span>

5.  <span data-ttu-id="148cd-125">Om du fortfarande behöver hjälp kan du också dela informationen med en supporttekniker eller produktgruppen för att få hjälp med problemet.</span><span class="sxs-lookup"><span data-stu-id="148cd-125">If you still need help, you can also share this information with a support engineer or the product group to get help with your problem.</span></span>

6.  <span data-ttu-id="148cd-126">Klicka på den **kopiera-ikonen** till höger om den **Kopieringsfel** textruta för att kopiera meddelande allt delar med en support eller produkt grupp tekniker.</span><span class="sxs-lookup"><span data-stu-id="148cd-126">Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer.</span></span>

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a><span data-ttu-id="148cd-127">Få hjälp genom att skicka meddelandeinformation till en supporttekniker</span><span class="sxs-lookup"><span data-stu-id="148cd-127">How to get help by sending notification details to a support engineer</span></span>

<span data-ttu-id="148cd-128">Det är mycket viktigt att du dela **allt som anges nedan** med en supporttekniker om du behöver hjälp, så att de kan hjälpa dig snabbt.</span><span class="sxs-lookup"><span data-stu-id="148cd-128">It is very important that you share **all the details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="148cd-129">Du kan göra det enkelt av **tar en skärmbild** eller genom att klicka på den **kopiera felikonen**, hittade till höger om den **Kopieringsfel** textruta.</span><span class="sxs-lookup"><span data-stu-id="148cd-129">You can do this easily by **taking a screenshot,** or by clicking the **Copy error icon**, found to the right of the **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="148cd-130">Meddelandeinformation förklaras</span><span class="sxs-lookup"><span data-stu-id="148cd-130">Notification Details Explained</span></span>

<span data-ttu-id="148cd-131">Den nedan beskrivs mer vad varje av meddelandet objekt innebär och innehåller exempel på var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="148cd-131">The below explains more what each of the notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="148cd-132">Viktigt meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="148cd-132">Essential Notification Items</span></span>

-   <span data-ttu-id="148cd-133">**Rubrik** – beskrivande rubrik i meddelandet</span><span class="sxs-lookup"><span data-stu-id="148cd-133">**Title** – the descriptive title of the notification</span></span>
   *  <span data-ttu-id="148cd-134">Exempel – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="148cd-134">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="148cd-135">**Beskrivning** – beskrivning av vad som hänt på grund av åtgärden</span><span class="sxs-lookup"><span data-stu-id="148cd-135">**Description** – the description of what occurred as a result of the operation</span></span>

   *  <span data-ttu-id="148cd-136">Exempel – **intern url har angett används redan av ett annat program**</span><span class="sxs-lookup"><span data-stu-id="148cd-136">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="148cd-137">**Meddelande-Id** – unikt id för meddelandet</span><span class="sxs-lookup"><span data-stu-id="148cd-137">**Notification Id** – the unique id of the notification</span></span>

   *  <span data-ttu-id="148cd-138">Exempel – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="148cd-138">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="148cd-139">**Id för klientbegäran** – specifik begäran-id som din webbläsare</span><span class="sxs-lookup"><span data-stu-id="148cd-139">**Client Request Id** – the specific request id made by your browser</span></span>

   *  <span data-ttu-id="148cd-140">Exempel – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="148cd-140">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="148cd-141">**Tid UTC stämpel** – tidsstämpeln då uppstod meddelandet i UTC</span><span class="sxs-lookup"><span data-stu-id="148cd-141">**Time Stamp UTC** – the timestamp during which the notification occurred, in UTC</span></span>

   *  <span data-ttu-id="148cd-142">Exempel – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="148cd-142">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="148cd-143">**Internt transaktions-Id** – internt ID vi kan använda för att söka av fel i vårt system</span><span class="sxs-lookup"><span data-stu-id="148cd-143">**Internal Transaction Id** – the internal ID we can use to look the error up in our systems</span></span>

   *  <span data-ttu-id="148cd-144">Exempel – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="148cd-144">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="148cd-145">**UPN** – användaren som utförde åtgärden</span><span class="sxs-lookup"><span data-stu-id="148cd-145">**UPN** – the user who performed the operation</span></span>

   *  <span data-ttu-id="148cd-146">Exempel –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="148cd-146">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="148cd-147">**Klient-Id** – unikt ID för den klient som användaren som utförde åtgärden var medlem av</span><span class="sxs-lookup"><span data-stu-id="148cd-147">**Tenant Id** – the unique ID of the tenant that the user who performed the operation was a member of</span></span>

   *  <span data-ttu-id="148cd-148">Exempel – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="148cd-148">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="148cd-149">**Användarobjektet Id** – unikt ID för den användare som utförde åtgärden</span><span class="sxs-lookup"><span data-stu-id="148cd-149">**User object Id** – the unique ID of the user who performed the operation</span></span>

 *  <span data-ttu-id="148cd-150">Exempel – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="148cd-150">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="148cd-151">Detaljerat meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="148cd-151">Detailed Notification Items</span></span>

-   <span data-ttu-id="148cd-152">**Visningsnamn** – **(kan vara tom)** en mer detaljerad visningsnamn efter fel</span><span class="sxs-lookup"><span data-stu-id="148cd-152">**Display Name** – **(can be empty)** a more detailed display name for the error</span></span>

  *  <span data-ttu-id="148cd-153">Exempel – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="148cd-153">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="148cd-154">**Status för** – specifika status för meddelandet</span><span class="sxs-lookup"><span data-stu-id="148cd-154">**Status** – the specific status of the notification</span></span>

   *  <span data-ttu-id="148cd-155">Exempel – **misslyckades**</span><span class="sxs-lookup"><span data-stu-id="148cd-155">Example – **Failed**</span></span>

-   <span data-ttu-id="148cd-156">**Objekt-Id** – **(kan vara tom)** objekt-ID som åtgärden utfördes</span><span class="sxs-lookup"><span data-stu-id="148cd-156">**Object Id** – **(can be empty)** the object ID against which the operation was performed</span></span>

   *  <span data-ttu-id="148cd-157">Exempel – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="148cd-157">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="148cd-158">**Information om** – detaljerad beskrivning av vad som hänt på grund av åtgärden</span><span class="sxs-lookup"><span data-stu-id="148cd-158">**Details** – the detailed description of what occurred as a result of the operation</span></span>

   *  <span data-ttu-id="148cd-159">Exempel – **intern url 'http://bing.com/' är ogiltig eftersom den redan används**</span><span class="sxs-lookup"><span data-stu-id="148cd-159">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="148cd-160">**Kopiera fel** – klickar du på den **kopiera-ikonen** till höger om den **Kopieringsfel** textruta för att kopiera meddelande allt delar med en support eller produkt grupp tekniker</span><span class="sxs-lookup"><span data-stu-id="148cd-160">**Copy error** – Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

   *  <span data-ttu-id="148cd-161">Exempel```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="148cd-161">Example ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="148cd-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="148cd-162">Next steps</span></span>
[<span data-ttu-id="148cd-163">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="148cd-163">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)



