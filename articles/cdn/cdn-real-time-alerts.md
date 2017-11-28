---
title: "aviseringar i realtid för aaaAzure CDN | Microsoft Docs"
description: Aviseringar i realtid i Microsoft Azure CDN. Meddelanden om hello prestanda hello slutpunkter i CDN-profilen med realtid aviseringar.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 269c90437088bbc41bf899a8c02749e8e6f3006c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a><span data-ttu-id="e2a35-104">Aviseringar i realtid i Microsoft Azure CDN</span><span class="sxs-lookup"><span data-stu-id="e2a35-104">Real-time alerts in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="e2a35-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="e2a35-105">Overview</span></span>
<span data-ttu-id="e2a35-106">Det här dokumentet förklarar aviseringar i realtid i Microsoft Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="e2a35-106">This document explains real-time alerts in Microsoft Azure CDN.</span></span> <span data-ttu-id="e2a35-107">Den här funktionen innehåller meddelanden i realtid om hello prestanda hello slutpunkter i CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="e2a35-107">This functionality provides real-time notifications about hello performance of hello endpoints in your CDN profile.</span></span>  <span data-ttu-id="e2a35-108">Du kan konfigurera e-post eller HTTP-varningar baserat på:</span><span class="sxs-lookup"><span data-stu-id="e2a35-108">You can set up email or HTTP alerts based on:</span></span>

* <span data-ttu-id="e2a35-109">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="e2a35-109">Bandwidth</span></span>
* <span data-ttu-id="e2a35-110">Statuskoder</span><span class="sxs-lookup"><span data-stu-id="e2a35-110">Status Codes</span></span>
* <span data-ttu-id="e2a35-111">Cache-status</span><span class="sxs-lookup"><span data-stu-id="e2a35-111">Cache Statuses</span></span>
* <span data-ttu-id="e2a35-112">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="e2a35-112">Connections</span></span>

## <a name="creating-a-real-time-alert"></a><span data-ttu-id="e2a35-113">Skapa en avisering om realtid</span><span class="sxs-lookup"><span data-stu-id="e2a35-113">Creating a real-time alert</span></span>
1. <span data-ttu-id="e2a35-114">I hello [Azure Portal](https://portal.azure.com), bläddra tooyour CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="e2a35-114">In hello [Azure Portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>
   
    ![CDN-profilbladet](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. <span data-ttu-id="e2a35-116">Klicka på hello hello CDN-profilbladet **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2a35-116">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profilbladet hantera knappen](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    <span data-ttu-id="e2a35-118">hello CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-118">hello CDN management portal opens.</span></span>
3. <span data-ttu-id="e2a35-119">Hovra över hello **Analytics** fliken och sedan hovra över hello **realtid Stats** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="e2a35-119">Hover over hello **Analytics** tab, then hover over hello **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="e2a35-120">Klicka på **Aviserinar**.</span><span class="sxs-lookup"><span data-stu-id="e2a35-120">Click on **Real-Time Alerts**.</span></span>
   
    ![CDN-hanteringsportalen](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    <span data-ttu-id="e2a35-122">hello lista över befintliga aviseringen konfigurationer (eventuella) visas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-122">hello list of existing alert configurations (if any) is displayed.</span></span>
4. <span data-ttu-id="e2a35-123">Klicka på hello **Lägg till avisering** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2a35-123">Click hello **Add Alert** button.</span></span>
   
    ![Aviseringen knappen Lägg till](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    <span data-ttu-id="e2a35-125">Ett formulär för att skapa en ny avisering visas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-125">A form for creating a new alert is displayed.</span></span>
   
    ![Ny avisering formulär](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. <span data-ttu-id="e2a35-127">Om du vill att den här aviseringen toobe aktiv när du klickar på **spara**, kontrollera hello **avisering aktiverad** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e2a35-127">If you want this alert toobe active when you click **Save**, check hello **Alert Enabled** checkbox.</span></span>
6. <span data-ttu-id="e2a35-128">Ange ett beskrivande namn för aviseringen i hello **namn** fältet.</span><span class="sxs-lookup"><span data-stu-id="e2a35-128">Enter a descriptive name for your alert in hello **Name** field.</span></span>
7. <span data-ttu-id="e2a35-129">I hello **medietyp** listrutan **HTTP stort objekt**.</span><span class="sxs-lookup"><span data-stu-id="e2a35-129">In hello **Media Type** dropdown, select **HTTP Large Object**.</span></span>
   
    ![Medietyp med HTTP-stort objekt som valts](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > <span data-ttu-id="e2a35-131">Du måste välja **HTTP stort objekt** som hello **medietyp**.</span><span class="sxs-lookup"><span data-stu-id="e2a35-131">You must select **HTTP Large Object** as hello **Media Type**.</span></span>  <span data-ttu-id="e2a35-132">hello andra alternativ används inte av **Azure CDN från Verizon**.</span><span class="sxs-lookup"><span data-stu-id="e2a35-132">hello other choices are not used by **Azure CDN from Verizon**.</span></span>  <span data-ttu-id="e2a35-133">Fel tooselect **HTTP Large Object** kommer aviseringen toonever utlösas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-133">Failure tooselect **HTTP Large Object** will cause your alert toonever be triggered.</span></span>
   > 
   > 
8. <span data-ttu-id="e2a35-134">Skapa en **uttryck** toomonitor genom att välja en **mått**, **operatorn**, och **utlösa värdet**.</span><span class="sxs-lookup"><span data-stu-id="e2a35-134">Create an **Expression** toomonitor by selecting a **Metric**, **Operator**, and **Trigger value**.</span></span>
   
   * <span data-ttu-id="e2a35-135">För **mått**, Välj hello typ av villkor som önskade övervakade.</span><span class="sxs-lookup"><span data-stu-id="e2a35-135">For **Metric**, select hello type of condition you want monitored.</span></span>  <span data-ttu-id="e2a35-136">**Mbit/s bandbredd** hello mängden bandbreddsanvändning i megabit per sekund.</span><span class="sxs-lookup"><span data-stu-id="e2a35-136">**Bandwidth Mbps** is hello amount of bandwidth usage in megabits per second.</span></span>  <span data-ttu-id="e2a35-137">**Totalt antal anslutningar** är hello antalet samtidiga HTTP-anslutningar tooour kant-servrar.</span><span class="sxs-lookup"><span data-stu-id="e2a35-137">**Total Connections** is hello number of concurrent HTTP connections tooour edge servers.</span></span>  <span data-ttu-id="e2a35-138">Definitioner av hello olika cache status och statuskoder finns [Azure CDN Cache-statuskoder](https://msdn.microsoft.com/library/mt759237.aspx) och [Azure CDN HTTP-statuskoder](https://msdn.microsoft.com/library/mt759238.aspx)</span><span class="sxs-lookup"><span data-stu-id="e2a35-138">For definitions of hello various cache statuses and status codes, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) and [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx)</span></span>
   * <span data-ttu-id="e2a35-139">**Operatorn** hello matematisk operator som fastställer hello förhållandet mellan hello mått och hello utlösaren värde.</span><span class="sxs-lookup"><span data-stu-id="e2a35-139">**Operator** is hello mathematical operator that establishes hello relationship between hello metric and hello trigger value.</span></span>
   * <span data-ttu-id="e2a35-140">**Utlösa värdet** är hello tröskelvärde som måste vara uppfyllda innan ett meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-140">**Trigger Value** is hello threshold value that must be met before a notification will be sent out.</span></span>
     
     <span data-ttu-id="e2a35-141">I hello exemplet nedan visar hello-uttryck som jag har skapat jag vill att ett meddelande när hello antalet 404 statuskoder är större än 25 toobe.</span><span class="sxs-lookup"><span data-stu-id="e2a35-141">In hello below example, hello expression I have created indicates that I would like toobe notified when hello number of 404 status codes is greater than 25.</span></span>
     
     ![Realtid avisering exempeluttryck](./media/cdn-real-time-alerts/cdn-expression.png)
9. <span data-ttu-id="e2a35-143">För **intervall**, ange hur ofta du vill att hello-uttryck som utvärderas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-143">For **Interval**, enter how frequently you would like hello expression evaluated.</span></span>
10. <span data-ttu-id="e2a35-144">I hello **meddela på** listrutan när du vill toobe meddelas när hello uttrycket är sant.</span><span class="sxs-lookup"><span data-stu-id="e2a35-144">In hello **Notify on** dropdown, select when you would like toobe notified when hello expression is true.</span></span>
    
    * <span data-ttu-id="e2a35-145">**Condition-Start** anger att en avisering skickas när hello angivna villkor först har identifierats.</span><span class="sxs-lookup"><span data-stu-id="e2a35-145">**Condition Start** indicates that a notification will be sent when hello specified condition is first detected.</span></span>
    * <span data-ttu-id="e2a35-146">**Condition-End** anger att en avisering skickas när hello angetts villkoret kan inte längre hittas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-146">**Condition End** indicates that a notification will be sent when hello specified condition is no longer detected.</span></span> <span data-ttu-id="e2a35-147">Det här meddelandet kan bara aktiveras när våra nätverksövervakning systemet upptäckte att hello som angetts tillstånd inträffade.</span><span class="sxs-lookup"><span data-stu-id="e2a35-147">This notification can only be triggered after our network monitoring system detected that hello specified condition occurred.</span></span>
    * <span data-ttu-id="e2a35-148">**Kontinuerlig** anger att ett meddelande ska skickas varje gång som hello kontrollsystem för nätverk identifierar hello angivna villkor.</span><span class="sxs-lookup"><span data-stu-id="e2a35-148">**Continuous** indicates that a notification will be sent each time that hello network monitoring system detects hello specified condition.</span></span> <span data-ttu-id="e2a35-149">Tänk på att hello nätverksövervakning system kommer bara kontrollera en gång per intervall för hello angivna villkor.</span><span class="sxs-lookup"><span data-stu-id="e2a35-149">Keep in mind that hello network monitoring system will only check once per interval for hello specified condition.</span></span>
    * <span data-ttu-id="e2a35-150">**Villkoret Start- och slutdatum** anger att ett meddelande ska skickas hello första gången som hello angivet villkor har identifierats och igen när hello villkoret kan inte längre hittas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-150">**Condition Start and End** indicates that a notification will be sent hello first time that hello specified condition is detected and once again when hello condition is no longer detected.</span></span>
11. <span data-ttu-id="e2a35-151">Om du vill tooreceive meddelanden via e-post, kontrollera hello **Avisera via e-post** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e2a35-151">If you want tooreceive notifications by email, check hello **Notify by Email** checkbox.</span></span>  
    
    ![Meddela via e-formulär](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    <span data-ttu-id="e2a35-153">I hello **till** anger hello e-postadress du där du vill att meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-153">In hello **To** field, enter hello email address you where you want notifications sent.</span></span> <span data-ttu-id="e2a35-154">För **ämne** och **brödtext**, kan du lämna hello standard och du kan anpassa hello-meddelande med hello **tillgängliga nyckelord** lista toodynamically Infoga aviseringsdata när hello-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-154">For **Subject** and **Body**, you may leave hello default, or you may customize hello message using hello **Available keywords** list toodynamically insert alert data when hello message is sent.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="e2a35-155">Du kan testa hello e-postmeddelande genom att klicka på hello **testmeddelande** knappen, men endast efter hello varningskonfigurationen har sparats.</span><span class="sxs-lookup"><span data-stu-id="e2a35-155">You can test hello email notification by clicking hello **Test Notification** button, but only after hello alert configuration has been saved.</span></span>
    > 
    > 
12. <span data-ttu-id="e2a35-156">Om du vill att meddelanden toobe anslås tooa webbservern Kontrollera hello **Avisera av HTTP Post** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e2a35-156">If you want notifications toobe posted tooa web server, check hello **Notify by HTTP Post** checkbox.</span></span>
    
    ![Meddela via HTTP Post-formulär](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    <span data-ttu-id="e2a35-158">I hello **Url** anger hello-URL du där du vill att hello HTTP-meddelande skickat.</span><span class="sxs-lookup"><span data-stu-id="e2a35-158">In hello **Url** field, enter hello URL you where you want hello HTTP message posted.</span></span> <span data-ttu-id="e2a35-159">I hello **huvuden** textruta ange hello HTTP-huvuden toobe skickas i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="e2a35-159">In hello **Headers** textbox, enter hello HTTP headers toobe sent in hello request.</span></span>  <span data-ttu-id="e2a35-160">För **brödtext** du kan anpassa hello-meddelande med hello **tillgängliga nyckelord** lista toodynamically Infoga aviseringsdata när hello-meddelande skickas.</span><span class="sxs-lookup"><span data-stu-id="e2a35-160">For **Body** you may customize hello message using hello **Available keywords** list toodynamically insert alert data when hello message is sent.</span></span>  <span data-ttu-id="e2a35-161">**Huvuden** och **brödtext** standard tooan XML-nyttolasten liknande toohello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="e2a35-161">**Headers** and **Body** default tooan XML payload similar toohello below example.</span></span>
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > <span data-ttu-id="e2a35-162">Du kan testa hello HTTP Post-meddelanden genom att klicka på hello **testmeddelande** knappen, men endast efter hello varningskonfigurationen har sparats.</span><span class="sxs-lookup"><span data-stu-id="e2a35-162">You can test hello HTTP Post notification by clicking hello **Test Notification** button, but only after hello alert configuration has been saved.</span></span>
    > 
    > 
13. <span data-ttu-id="e2a35-163">Klicka på hello **spara** knappen toosave avisering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e2a35-163">Click hello **Save** button toosave your alert configuration.</span></span>  <span data-ttu-id="e2a35-164">När du har markerat **avisering aktiverad** i steg 5 aviseringen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="e2a35-164">If you checked **Alert Enabled** in step 5, your alert is now active.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2a35-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2a35-165">Next Steps</span></span>
* <span data-ttu-id="e2a35-166">Analysera [realtid statistik i Azure CDN](cdn-real-time-stats.md)</span><span class="sxs-lookup"><span data-stu-id="e2a35-166">Analyze [Real-time stats in Azure CDN](cdn-real-time-stats.md)</span></span>
* <span data-ttu-id="e2a35-167">Gräva djupare med [avancerade http-rapporter](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="e2a35-167">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="e2a35-168">Analysera [användningsmönster](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="e2a35-168">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

