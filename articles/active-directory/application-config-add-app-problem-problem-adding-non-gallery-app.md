---
title: "aaaProblem lägga till en icke-galleriet program | Microsoft Docs"
description: "Förstå hello vanliga problem personer yta när du lägger till anpassade program för icke-galleriet"
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
ms.openlocfilehash: cfe9b657ae18cbacaddbd85658471a2c57c9cf95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-adding-a-non-gallery-application"></a><span data-ttu-id="fe29a-103">Problem att lägga till en icke-galleriet program</span><span class="sxs-lookup"><span data-stu-id="fe29a-103">Problem adding a non-gallery application</span></span>

<span data-ttu-id="fe29a-104">Den här artikeln hjälper dig toounderstand hello vanliga problem personer står inför när du lägger till **anpassade program för icke-galleriet** och vad du kan göra tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="fe29a-104">This article help you toounderstand hello common problems people face when adding **custom non-gallery applications** and what you can do tooresolve them.</span></span> 

## <a name="i-clicked-hello-add-button-and-my-application-took-a-long-time-tooappear"></a><span data-ttu-id="fe29a-105">Klickade hello ”Lägg till”-knappen och mitt program tog en lång tid tooappear</span><span class="sxs-lookup"><span data-stu-id="fe29a-105">I clicked hello “add” button and my application took a long time tooappear</span></span>

<span data-ttu-id="fe29a-106">I vissa fall kan det ta 1 till 2 minuter (och ibland längre) för ett program tooappear har lagt till tooyour directory.</span><span class="sxs-lookup"><span data-stu-id="fe29a-106">Under some circumstances, it can take 1-2 minutes (and sometimes longer) for an application tooappear after adding it tooyour directory.</span></span> <span data-ttu-id="fe29a-107">Även om detta inte är hello normal prestanda ser hello programmet tillägg pågår genom att klicka på hello **meddelanden** ikonen (hello bell) i hello övre högra hörnet på hello [Azure Portal](https://portal.azure.com/)och letar efter en **pågår** eller **slutförd** meddelande med etiketten **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="fe29a-107">While this is not hello normal expected performance, you can see hello application addition is in progress by clicking on hello **Notifications** icon (hello bell) in hello upper right of hello [Azure Portal](https://portal.azure.com/) and looking for an **In Progress** or **Completed** notification labeled **Create application**.</span></span>

<span data-ttu-id="fe29a-108">Om programmet aldrig har lagts till, eller om det uppstår ett fel när du klickar på hello **Lägg till** knappen, visas en **meddelande** i en **fel** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fe29a-108">If your application is never added, or you encounter an error when clicking hello **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="fe29a-109">Om du vill veta mer om hello fel toolearn mer tooor dela med en stöd engingeer du kan se mer information om hello felet genom att följa stegen hello i hello [hur toosee hello information om en portalmeddelandet](#how-to-see-the-details-of-a-portal-notification) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fe29a-109">If you want more details about hello error toolearn more tooor share with a support engingeer, you can see more information about hello error by following hello steps in hello [How toosee hello details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-clicked-hello-add-button-and-my-application-didnt-appear"></a><span data-ttu-id="fe29a-110">Klickade hello ”Lägg till”-knappen och mitt program visas inte</span><span class="sxs-lookup"><span data-stu-id="fe29a-110">I clicked hello “add” button and my application didn’t appear</span></span>

<span data-ttu-id="fe29a-111">Ibland på grund av problem med tootransient nätverksproblem eller ett programfel kan lägga till ett program misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fe29a-111">Sometimes, due tootransient issues, networking problems, or a bug, adding an application fail.</span></span> <span data-ttu-id="fe29a-112">Du kan se detta händer när du klickar på hello **meddelanden** ikon (hello bell) i hello övre högra hörnet av hello Azure Portal och du ser ett rött (!) ikonen nästa tooyour **skapa program** meddelande.</span><span class="sxs-lookup"><span data-stu-id="fe29a-112">You can tell this happens when you click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal and you see a red (!) icon next tooyour **Create application** notification.</span></span> <span data-ttu-id="fe29a-113">Detta anger att ett fel uppstod när du skapar hello program.</span><span class="sxs-lookup"><span data-stu-id="fe29a-113">This indicates there was an error when creating hello application.</span></span>

<span data-ttu-id="fe29a-114">Om det uppstår ett fel när du klickar på hello **Lägg till** knappen, visas en **meddelande** i en **fel** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fe29a-114">If you encounter an error when clicking hello **Add** button, you’ll see a **Notification** in an **Error** state.</span></span> <span data-ttu-id="fe29a-115">Om du vill veta mer om hello fel toolearn mer tooor dela med en stöd engingeer du kan se mer information om hello felet genom att följa stegen hello i hello [hur toosee hello information om en portalmeddelandet](#how-to-see-the-details-of-a-portal-notification) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fe29a-115">If you want more details about hello error toolearn more tooor share with a support engingeer, you can see more information about hello error by following hello steps in hello [How toosee hello details of a portal notification](#how-to-see-the-details-of-a-portal-notification) section.</span></span>

## <a name="i-dont-know-how-tooset-up-my-application-once-ive-added-it"></a><span data-ttu-id="fe29a-116">Jag vet inte hur tooset in Mina program när jag har lagt till den</span><span class="sxs-lookup"><span data-stu-id="fe29a-116">I don’t know how tooset up my application once I’ve added it</span></span>

<span data-ttu-id="fe29a-117">Om du behöver hjälp med Lär dig mer om anpassade program hello [dokumentbibliotek för Azure AD-program](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) toolearn mer om enkel inloggning med Azure AD och hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="fe29a-117">If you need help learning about custom applications, hello [Azure AD Applications Document Library](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) help you toolearn more about single sign-on with Azure AD and how it works.</span></span>

## <a name="how-toosee-hello-details-of-a-portal-notification"></a><span data-ttu-id="fe29a-118">Hur toosee hello information om ett meddelande om portal</span><span class="sxs-lookup"><span data-stu-id="fe29a-118">How toosee hello details of a portal notification</span></span>

<span data-ttu-id="fe29a-119">Du kan se hello information om eventuella portalmeddelandet genom att följa hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="fe29a-119">You can see hello details of any portal notification by following hello steps below:</span></span>

1.  <span data-ttu-id="fe29a-120">Klicka på hello **meddelanden** ikon (hello bell) i hello övre högra hörnet av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fe29a-120">click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal</span></span>

2.  <span data-ttu-id="fe29a-121">Markera alla meddelanden i en **fel** tillstånd (de med en röd (!) nästa toothem).</span><span class="sxs-lookup"><span data-stu-id="fe29a-121">Select any notification in an **Error** state (those with a red (!) next toothem).</span></span>

   >[!NOTE]
   ><span data-ttu-id="fe29a-122">Du kan klicka på meddelanden i en **lyckade** eller **pågår** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fe29a-122">You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
   >
   >

3.  <span data-ttu-id="fe29a-123">Den här öppna hello **detaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="fe29a-123">This open hello **Notification Details** blade.</span></span>

4.  <span data-ttu-id="fe29a-124">Använd den här informationen själv toounderstand mer information om hello problem.</span><span class="sxs-lookup"><span data-stu-id="fe29a-124">Use this information yourself toounderstand more details about hello problem.</span></span>

5.  <span data-ttu-id="fe29a-125">Om du fortfarande behöver hjälp kan också dela informationen med en stöd tekniker eller hello grupp tooget produkthjälp med ditt problem.</span><span class="sxs-lookup"><span data-stu-id="fe29a-125">If you still need help, you can also share this information with a support engineer or hello product group tooget help with your problem.</span></span>

6.  <span data-ttu-id="fe29a-126">Klicka på hello **kopiera-ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker.</span><span class="sxs-lookup"><span data-stu-id="fe29a-126">Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer.</span></span>

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a><span data-ttu-id="fe29a-127">Hur tooget genom att skicka meddelande information tooa supportteknikern</span><span class="sxs-lookup"><span data-stu-id="fe29a-127">How tooget help by sending notification details tooa support engineer</span></span>

<span data-ttu-id="fe29a-128">Det är mycket viktigt att du dela **alla hello information som visas nedan** med en supporttekniker om du behöver hjälp, så att de kan hjälpa dig snabbt.</span><span class="sxs-lookup"><span data-stu-id="fe29a-128">It is very important that you share **all hello details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="fe29a-129">Du kan göra det enkelt av **tar en skärmbild** eller genom att klicka på hello **kopiera felikonen**, hitta toohello höger om hello **Kopieringsfel** textruta.</span><span class="sxs-lookup"><span data-stu-id="fe29a-129">You can do this easily by **taking a screenshot,** or by clicking hello **Copy error icon**, found toohello right of hello **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="fe29a-130">Meddelandeinformation förklaras</span><span class="sxs-lookup"><span data-stu-id="fe29a-130">Notification Details Explained</span></span>

<span data-ttu-id="fe29a-131">hello nedan förklaras mer vad varje hello avisering artiklar innebär och ger exempel på var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="fe29a-131">hello below explains more what each of hello notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="fe29a-132">Viktigt meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="fe29a-132">Essential Notification Items</span></span>

-   <span data-ttu-id="fe29a-133">**Rubrik** – hello beskrivande rubrik av hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="fe29a-133">**Title** – hello descriptive title of hello notification</span></span>
   *  <span data-ttu-id="fe29a-134">Exempel – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="fe29a-134">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="fe29a-135">**Beskrivning** – hello beskrivning av vad som hänt på grund av hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="fe29a-135">**Description** – hello description of what occurred as a result of hello operation</span></span>

   *  <span data-ttu-id="fe29a-136">Exempel – **intern url har angett används redan av ett annat program**</span><span class="sxs-lookup"><span data-stu-id="fe29a-136">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="fe29a-137">**Meddelande-Id** – hello unikt id för hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="fe29a-137">**Notification Id** – hello unique id of hello notification</span></span>

   *  <span data-ttu-id="fe29a-138">Exempel – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="fe29a-138">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="fe29a-139">**Id för klientbegäran** – hello specifik begäran-id som din webbläsare</span><span class="sxs-lookup"><span data-stu-id="fe29a-139">**Client Request Id** – hello specific request id made by your browser</span></span>

   *  <span data-ttu-id="fe29a-140">Exempel – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="fe29a-140">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="fe29a-141">**Tid UTC stämpel** – hello tidsstämpeln då uppstod hello-meddelande i UTC</span><span class="sxs-lookup"><span data-stu-id="fe29a-141">**Time Stamp UTC** – hello timestamp during which hello notification occurred, in UTC</span></span>

   *  <span data-ttu-id="fe29a-142">Exempel – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="fe29a-142">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="fe29a-143">**Internt transaktions-Id** – hello internt ID vi kan använda toolook hello fel i vårt system</span><span class="sxs-lookup"><span data-stu-id="fe29a-143">**Internal Transaction Id** – hello internal ID we can use toolook hello error up in our systems</span></span>

   *  <span data-ttu-id="fe29a-144">Exempel – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="fe29a-144">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="fe29a-145">**UPN** – hello-användaren som utförde åtgärden hello</span><span class="sxs-lookup"><span data-stu-id="fe29a-145">**UPN** – hello user who performed hello operation</span></span>

   *  <span data-ttu-id="fe29a-146">Exempel –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="fe29a-146">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="fe29a-147">**Klient-Id** – hello unikt ID för hello-klient som hello användaren som utförde åtgärden hello var medlem av</span><span class="sxs-lookup"><span data-stu-id="fe29a-147">**Tenant Id** – hello unique ID of hello tenant that hello user who performed hello operation was a member of</span></span>

   *  <span data-ttu-id="fe29a-148">Exempel – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="fe29a-148">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="fe29a-149">**Användarobjektet Id** – hello unikt ID för hello-användaren som utförde åtgärden hello</span><span class="sxs-lookup"><span data-stu-id="fe29a-149">**User object Id** – hello unique ID of hello user who performed hello operation</span></span>

 *  <span data-ttu-id="fe29a-150">Exempel – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="fe29a-150">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="fe29a-151">Detaljerat meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="fe29a-151">Detailed Notification Items</span></span>

-   <span data-ttu-id="fe29a-152">**Visningsnamn** – **(kan vara tom)** en mer detaljerad visningsnamn för hello-fel</span><span class="sxs-lookup"><span data-stu-id="fe29a-152">**Display Name** – **(can be empty)** a more detailed display name for hello error</span></span>

  *  <span data-ttu-id="fe29a-153">Exempel – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="fe29a-153">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="fe29a-154">**Status för** – hello viss status av hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="fe29a-154">**Status** – hello specific status of hello notification</span></span>

   *  <span data-ttu-id="fe29a-155">Exempel – **misslyckades**</span><span class="sxs-lookup"><span data-stu-id="fe29a-155">Example – **Failed**</span></span>

-   <span data-ttu-id="fe29a-156">**Objekt-Id** – **(kan vara tom)** hello mot vilken hello åtgärden utfördes objekt-ID</span><span class="sxs-lookup"><span data-stu-id="fe29a-156">**Object Id** – **(can be empty)** hello object ID against which hello operation was performed</span></span>

   *  <span data-ttu-id="fe29a-157">Exempel – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="fe29a-157">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="fe29a-158">**Information om** – hello detaljerad beskrivning av vad som hänt på grund av hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="fe29a-158">**Details** – hello detailed description of what occurred as a result of hello operation</span></span>

   *  <span data-ttu-id="fe29a-159">Exempel – **intern url 'http://bing.com/' är ogiltig eftersom den redan används**</span><span class="sxs-lookup"><span data-stu-id="fe29a-159">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="fe29a-160">**Kopiera fel** – Klicka på hello **kopiera-ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker</span><span class="sxs-lookup"><span data-stu-id="fe29a-160">**Copy error** – Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

   *  <span data-ttu-id="fe29a-161">Exempel```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="fe29a-161">Example ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe29a-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe29a-162">Next steps</span></span>
[<span data-ttu-id="fe29a-163">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fe29a-163">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)



