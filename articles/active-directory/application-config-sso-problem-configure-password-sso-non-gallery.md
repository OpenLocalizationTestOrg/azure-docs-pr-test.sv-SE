---
title: "Problem som konfigurerar lösenord enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Förstå de vanliga problem personer står inför när du konfigurerar lösenord enkel inloggning för anpassade program för icke-galleriet som inte listas i Azure AD Application Gallery"
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
ms.openlocfilehash: 9c76b6f3495e2dd759a156fcef97b57aece8d632
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="1c993-103">Konfigurera lösenord enkel inloggning för ett icke-galleriet program problem</span><span class="sxs-lookup"><span data-stu-id="1c993-103">Problem configuring password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="1c993-104">Den här artikeln hjälper dig att förstå de vanliga problem personer står inför när du konfigurerar **lösenord enkel inloggning** med ett icke-galleriet program.</span><span class="sxs-lookup"><span data-stu-id="1c993-104">This article help you to understand the common problems people face when configuring **Password single sign-on** with a non-gallery application.</span></span>

## <a name="how-to-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="1c993-105">Så här avbildar inloggning fält för ett program</span><span class="sxs-lookup"><span data-stu-id="1c993-105">How to capture sign-in fields for an application</span></span>

<span data-ttu-id="1c993-106">Logga in fältet stöds bara för HTML-aktiverade inloggningssidor och är **stöds inte för icke-standard inloggningssidor**, som de som använder Flash eller andra icke-HTML-aktiverade tekniker.</span><span class="sxs-lookup"><span data-stu-id="1c993-106">Sign-in field capture is only supported for HTML-enabled sign-in pages and is **not supported for non-standard sign-in pages**, like those that use Flash, or other non-HTML-enabled technologies.</span></span>

<span data-ttu-id="1c993-107">Det finns två sätt som du kan avbilda inloggning fält för dina anpassade program:</span><span class="sxs-lookup"><span data-stu-id="1c993-107">There are two ways you can capture sign-in fields for your custom applications:</span></span>

-   <span data-ttu-id="1c993-108">Fältet för automatisk inloggning avbildning</span><span class="sxs-lookup"><span data-stu-id="1c993-108">Automatic sign-in field capture</span></span>

-   <span data-ttu-id="1c993-109">Fältet för manuell inloggning avbildning</span><span class="sxs-lookup"><span data-stu-id="1c993-109">Manual sign-in field capture</span></span>

<span data-ttu-id="1c993-110">**Fältet för automatisk inloggning avbilda** fungerar väl med de flesta HTML-aktiverade inloggningssidor, om de använder **välkända DIV-ID: N för användarnamn och lösenord Ange** fältet.</span><span class="sxs-lookup"><span data-stu-id="1c993-110">**Automatic sign-in field capture** works well with most HTML-enabled sign-in pages, if they use **well-known DIV IDs for the username and password input** field.</span></span> <span data-ttu-id="1c993-111">Hur detta fungerar är genom skrapning HTML på sidan för att hitta DIV-ID som matchar vissa villkor och sedan spara att metadata för det här programmet så att vi kan spela upp lösenord till den senare.</span><span class="sxs-lookup"><span data-stu-id="1c993-111">The way this works is by scraping the HTML on the page to find DIV IDs that match certain criteria and by then saving that metadata for this application so we can replay passwords to it later.</span></span>

<span data-ttu-id="1c993-112">**Fältet för manuell inloggning avbilda** kan användas i fall som programmet **leverantör inte etiketten** indatafält används för inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c993-112">**Manual sign-in field capture** can be used in the case that the application **vendor does not label** the input fields used for sign in.</span></span> <span data-ttu-id="1c993-113">Manuell inloggning fältet avbilda kan också användas i fallet när den **leverantör återgivningar flera fält** som inte kan identifieras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1c993-113">Manual sign-in field capture can also be used in the case when the **vendor renders multiple fields** which cannot be auto-detected.</span></span> <span data-ttu-id="1c993-114">Azure AD kan lagra data för så många fält som finns på inloggningssidan, så länge du berätta var dessa fält finns på sidan.</span><span class="sxs-lookup"><span data-stu-id="1c993-114">Azure AD can store data for as many fields as are on the sign in page, so long as you tell us where those fields are on the page.</span></span>

<span data-ttu-id="1c993-115">I allmänhet **om automatisk inloggning fältet avbilda inte fungerar alltid föreslår vi försöker det manuella alternativet.**</span><span class="sxs-lookup"><span data-stu-id="1c993-115">In general, **if automatic sign-in field capture does not work, we always suggest trying the manual option.**</span></span>

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="1c993-116">Så här avbildar automatiskt inloggning fält för ett program</span><span class="sxs-lookup"><span data-stu-id="1c993-116">How to automatically capture sign-in fields for an application</span></span>

<span data-ttu-id="1c993-117">Så här konfigurerar du **lösenordsbaserade enkel inloggning** för ett program som använder **fält för automatisk inloggning avbilda**, Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="1c993-117">To configure **Password-based Single Sign-on** for an application using **automatic sign-in field capture**, follow the steps below:</span></span>

1.  <span data-ttu-id="1c993-118">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="1c993-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="1c993-119">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1c993-119">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="1c993-120">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="1c993-120">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="1c993-121">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1c993-121">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="1c993-122">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="1c993-122">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="1c993-123">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="1c993-123">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="1c993-124">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c993-124">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="1c993-125">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1c993-125">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="1c993-126">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="1c993-126">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="1c993-127">Ange den **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="1c993-127">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="1c993-128">Detta är den URL där användarna anger sina användarnamn och lösenord för att logga in på.</span><span class="sxs-lookup"><span data-stu-id="1c993-128">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="1c993-129">**Se till att logga in fält som är synliga på den URL som du anger**.</span><span class="sxs-lookup"><span data-stu-id="1c993-129">**Ensure the sign in fields are visible at the URL you provide**.</span></span>

10. <span data-ttu-id="1c993-130">Klicka på knappen **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1c993-130">Click the **Save** button.</span></span>

11. <span data-ttu-id="1c993-131">När du gör det kan vi ska automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och du kan använda Azure AD för att säkert överföra lösenord för det aktuella programmet med hjälp av webbläsartillägget för åtkomst-panelen.</span><span class="sxs-lookup"><span data-stu-id="1c993-131">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span>

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="1c993-132">Så här avbildar inloggning fält för ett program manuellt</span><span class="sxs-lookup"><span data-stu-id="1c993-132">How to manually capture sign-in fields for an application</span></span>

<span data-ttu-id="1c993-133">Om du vill samla in tecken i fälten manuellt, måste du först ha åtkomst panelen webbläsartillägget installerad och **inte körs i inPrivate incognito eller privat läge.**</span><span class="sxs-lookup"><span data-stu-id="1c993-133">To manually capture sign in fields, you must first have the Access Panel Browser extension installed and **not be running in inPrivate, incognito, or private mode.**</span></span> <span data-ttu-id="1c993-134">Om du vill installera webbläsartillägg för, följer du stegen i den [installera åtkomst panelen webbläsartillägget](#i-cannot-manually-detect-sign-in-fields-for-my-application) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c993-134">To install the browser extension, follow the steps in the [How to install the Access Panel Browser extension](#i-cannot-manually-detect-sign-in-fields-for-my-application) section.</span></span>

<span data-ttu-id="1c993-135">Så här konfigurerar du **lösenordsbaserade enkel inloggning** för ett program som använder **manuell inloggning fältet avbilda**, Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="1c993-135">To configure **Password-based Single Sign-on** for an application using **manual sign-in field capture**, follow the steps below:</span></span>

1.  <span data-ttu-id="1c993-136">Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="1c993-136">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="1c993-137">Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1c993-137">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="1c993-138">Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="1c993-138">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="1c993-139">Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1c993-139">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="1c993-140">Klicka på **alla program** att visa en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="1c993-140">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="1c993-141">Om du inte ser programmet som du vill visa här använder du den **Filter** kontrollen längst upp i den **listan med alla program** och ange den **visa** att **alla program.**</span><span class="sxs-lookup"><span data-stu-id="1c993-141">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="1c993-142">Välj det program som du vill konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c993-142">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="1c993-143">När programmet läses in klickar du på den **enkel inloggning** från programmets vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="1c993-143">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="1c993-144">Välj läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="1c993-144">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="1c993-145">Ange den **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="1c993-145">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="1c993-146">Detta är den URL där användarna anger sina användarnamn och lösenord för att logga in på.</span><span class="sxs-lookup"><span data-stu-id="1c993-146">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="1c993-147">**Se till att logga in fält som är synliga på den URL som du anger**.</span><span class="sxs-lookup"><span data-stu-id="1c993-147">**Ensure the sign in fields are visible at the URL you provide**.</span></span>

10. <span data-ttu-id="1c993-148">Klicka på knappen **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1c993-148">Click the **Save** button.</span></span>

11. <span data-ttu-id="1c993-149">När du gör det kan vi ska automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och du kan använda Azure AD för att säkert överföra lösenord för det aktuella programmet med hjälp av webbläsartillägget för åtkomst-panelen.</span><span class="sxs-lookup"><span data-stu-id="1c993-149">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span> <span data-ttu-id="1c993-150">Om detta misslyckas kan du **Ändra läget för att använda manuell inloggning fältet avbilda** genom att fortsätta att steg 12.</span><span class="sxs-lookup"><span data-stu-id="1c993-150">In the case that this fails, you can **change the sign-in mode to use manual sign-in field capture** by continuing to step 12.</span></span>

12. <span data-ttu-id="1c993-151">Klicka på **konfigurera &lt;appname&gt; inställningar för lösenord enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1c993-151">click **Configure &lt;appname&gt; Password Single Sign-on Settings**.</span></span>

13. <span data-ttu-id="1c993-152">Välj den **identifieras manuellt inloggning fält** konfigurationsalternativet.</span><span class="sxs-lookup"><span data-stu-id="1c993-152">Select the **Manually detect sign-in fields** configuration option.</span></span>

14. <span data-ttu-id="1c993-153">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c993-153">Click **Ok**.</span></span>

15. <span data-ttu-id="1c993-154">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1c993-154">Click **Save**.</span></span>

16. <span data-ttu-id="1c993-155">Följ instruktionerna på skärmen att använda åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1c993-155">Follow the on screen instructions to use the Access Panel.</span></span>

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a><span data-ttu-id="1c993-156">Felet ”Det gick inte att hitta alla fält på den URL” visas</span><span class="sxs-lookup"><span data-stu-id="1c993-156">I see a “We couldn’t find any sign-in fields at that URL” error</span></span>

<span data-ttu-id="1c993-157">Det här felet visas när automatisk identifiering av inloggning fält misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1c993-157">You see this error when automatic detection of sign-in fields fails.</span></span> <span data-ttu-id="1c993-158">Försök för att lösa problemet manuell inloggning fältet identifiering genom att följa stegen i den [hur du manuellt samla in inloggning fält för ett program](#how-to-manually-capture-sign-in-fields-for-an-application) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c993-158">To resolve this issue, try manual sign-in field detection by following the steps in the [How to manually capture sign-in fields for an application](#how-to-manually-capture-sign-in-fields-for-an-application) section.</span></span>

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a><span data-ttu-id="1c993-159">”Det går inte att spara konfigurationen enkel inloggning” visas fel</span><span class="sxs-lookup"><span data-stu-id="1c993-159">I see an “Unable to save Single Sign-on configuration” error</span></span>

<span data-ttu-id="1c993-160">I vissa sällsynta fall, kan uppdatera konfigurationen för enkel inloggning misslyckas.</span><span class="sxs-lookup"><span data-stu-id="1c993-160">In certain rare cases, updating the single sign-on configuration can fail.</span></span> <span data-ttu-id="1c993-161">Du kan lösa detta försök att spara enkel inloggning konfigurationen igen.</span><span class="sxs-lookup"><span data-stu-id="1c993-161">TO resolve this try saving the single sign-on configuration again.</span></span>

<span data-ttu-id="1c993-162">Om problemet kvarstår misslyckas regelbundet, öppna ett supportärende och ange den information som samlats in i den [så visas detaljerad information om en portalmeddelandet](#i-cannot-manually-detect-sign-in-fields-for-my-application) och [få hjälp genom att skicka meddelandeinformation till en supportbegäran tekniker](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c993-162">If this continues to fail consistently, open a support case and provide the information gathered in the [How to see the details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How to get help by sending notification details to a support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections.</span></span>

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a><span data-ttu-id="1c993-163">Jag kan inte manuellt identifiera tecken i fälten för Mina program</span><span class="sxs-lookup"><span data-stu-id="1c993-163">I cannot manually detect sign in fields for my application</span></span>

<span data-ttu-id="1c993-164">Några av de funktioner som kan uppstå när Manuell identifiering inte fungerar är:</span><span class="sxs-lookup"><span data-stu-id="1c993-164">Some of the behaviors you might see when manual detection is not working include:</span></span>

-   <span data-ttu-id="1c993-165">Den manuella processen fanns ska fungera, men fälten avbildas inte korrekt</span><span class="sxs-lookup"><span data-stu-id="1c993-165">The manual capture process appeared to work, but the fields captured were not correct</span></span>

-   <span data-ttu-id="1c993-166">Höger-fält hämta inte markeras när du utför den här processen</span><span class="sxs-lookup"><span data-stu-id="1c993-166">The right fields don’t get highlighted when performing the capture process</span></span>

-   <span data-ttu-id="1c993-167">Den här processen för att komma till programmets inloggningssida som förväntat, men ingenting händer</span><span class="sxs-lookup"><span data-stu-id="1c993-167">The capture process takes me to the application’s login page as expected, but nothing happens</span></span>

-   <span data-ttu-id="1c993-168">Manuell avbilda verkar fungera, men SSO inträffa inte när användarna navigerar till programmet från åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1c993-168">Manual capture appears to work, but SSO doesn’t happen when my users navigate to the application from the Access Panel.</span></span>

<span data-ttu-id="1c993-169">Kontrollera följande om du stöter på något av följande problem:</span><span class="sxs-lookup"><span data-stu-id="1c993-169">check the following if you encounter any of these issues:</span></span>

-   <span data-ttu-id="1c993-170">Kontrollera att du har den senaste versionen av webbläsartillägget för åtkomst till Kontrollpanelen **installerat** och **aktiverat** genom att följa stegen i den [installera åtkomst panelen webbläsartillägget](#how-to-install-the-access-panel-browser-extension) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1c993-170">Ensure that you have the latest version of the access panel browser extension **installed** and **enabled** by following the steps in the [How to install the Access Panel Browser extension](#how-to-install-the-access-panel-browser-extension) section.</span></span>

-   <span data-ttu-id="1c993-171">Se till att du inte försöker utföra den här processen när webbläsaren i **incognito InPrivate- eller privat läge**.</span><span class="sxs-lookup"><span data-stu-id="1c993-171">Ensure that you are not attempting the capture process while your browser in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="1c993-172">Tillägget för åtkomst-panelen stöds inte i dessa lägen.</span><span class="sxs-lookup"><span data-stu-id="1c993-172">The access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="1c993-173">Se till att användarna inte försöker logga in till programmet från åtkomstpanelen när i **incognito InPrivate- eller privat läge**.</span><span class="sxs-lookup"><span data-stu-id="1c993-173">Ensure that your users are not trying to sign in to the application from the access panel while in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="1c993-174">Tillägget för åtkomst-panelen stöds inte i dessa lägen.</span><span class="sxs-lookup"><span data-stu-id="1c993-174">The access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="1c993-175">Försök manuella processen igen, säkerställt röda markörer är över rätt fält.</span><span class="sxs-lookup"><span data-stu-id="1c993-175">Try the manual capture process again, ensuring the red markers are over the correct fields.</span></span>

-   <span data-ttu-id="1c993-176">Om manuell insamlingen verkar låser sig eller inloggningssidan inte göra något (fall 3 ovan), försök manuella processen igen.</span><span class="sxs-lookup"><span data-stu-id="1c993-176">If the manual capture process seems to hang, or the sign in page doesn’t do anything (case 3 above), try the manual capture process again.</span></span> <span data-ttu-id="1c993-177">Men nu när du har slutfört processen, tryck på den **F12** knappen för att öppna din webbläsare developer-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1c993-177">But, this time after completing the process, press the **F12** button to open your browser’s developer console.</span></span> <span data-ttu-id="1c993-178">En gång, öppna den **konsolen** och skriv **window.location= ”&lt;ange tecken i url som du angav när du konfigurerar appen&gt;”** och tryck sedan på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="1c993-178">Once there, open the **console** and type **window.location=”&lt;enter the sign in url you specified when configuring the app&gt;”** and then press **Enter**.</span></span> <span data-ttu-id="1c993-179">Denna kraft en sida omdirigera som Avsluta processen och lagra de fält som har spelats in.</span><span class="sxs-lookup"><span data-stu-id="1c993-179">This force a page redirect which end the capture process and store the fields that have been captured.</span></span>

<span data-ttu-id="1c993-180">Om inget av dessa sätt fungerar för dig kan vi hjälpa.</span><span class="sxs-lookup"><span data-stu-id="1c993-180">If none of these approaches work for you, we can help.</span></span> <span data-ttu-id="1c993-181">Öppna ett supportärende med information om vad du försökte, samt information som samlats in i den [så visas detaljerad information om en portalmeddelandet](#i-cannot-manually-detect-sign-in-fields-for-my-application) och [få hjälp genom att skicka meddelandeinformation till en supporttekniker ](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) avsnitten (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="1c993-181">Open a support case with the details of what you tried, as well as the information gathered in the [How to see the details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How to get help by sending notification details to a support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections (if applicable).</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="1c993-182">Så här installerar du Access panelen webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="1c993-182">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="1c993-183">Följ stegen nedan om du vill installera webbläsartillägget för åtkomst panelen:</span><span class="sxs-lookup"><span data-stu-id="1c993-183">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="1c993-184">Öppna den [åtkomstpanelen](https://myapps.microsoft.com) i en webbläsare som stöds och logga in som en **användaren** i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1c993-184">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="1c993-185">Klicka på en **lösenord SSO-program** på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1c993-185">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="1c993-186">Fråga om att installera programvara, Välj **installera nu**.</span><span class="sxs-lookup"><span data-stu-id="1c993-186">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="1c993-187">Baserat på din webbläsare du dirigeras till länken.</span><span class="sxs-lookup"><span data-stu-id="1c993-187">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="1c993-188">**Lägg till** tillägg till webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="1c993-188">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="1c993-189">Om din webbläsare, Välj antingen **aktivera** eller **Tillåt** tillägget.</span><span class="sxs-lookup"><span data-stu-id="1c993-189">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="1c993-190">När den har installerats, **starta om** webbläsarsessionen.</span><span class="sxs-lookup"><span data-stu-id="1c993-190">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="1c993-191">Logga in till åtkomstpanelen och se om kan du **starta** lösenord SSO-program.</span><span class="sxs-lookup"><span data-stu-id="1c993-191">Sign in into the Access Panel and see if you can **launch** your password-SSO applications.</span></span>

<span data-ttu-id="1c993-192">Du kan också ladda ned tillägget för Chrome och Firefox från direkt med länkarna nedan:</span><span class="sxs-lookup"><span data-stu-id="1c993-192">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="1c993-193">Tillägget för Chrome åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="1c993-193">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="1c993-194">Tillägget för Firefox åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="1c993-194">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a><span data-ttu-id="1c993-195">Hur du visar information om ett meddelande om portal</span><span class="sxs-lookup"><span data-stu-id="1c993-195">How to see the details of a portal notification</span></span>

<span data-ttu-id="1c993-196">Du kan se information om eventuella portalmeddelandet genom att följa stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="1c993-196">You can see the details of any portal notification by following the steps below:</span></span>

1.  <span data-ttu-id="1c993-197">Klicka på den **meddelanden** ikonen (sal) i övre högra hörnet i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1c993-197">click the **Notifications** icon (the bell) in the upper right of the Azure Portal</span></span>

2.  <span data-ttu-id="1c993-198">Markera alla meddelanden i en **fel** tillstånd (de med ett rött (!) bredvid dem).</span><span class="sxs-lookup"><span data-stu-id="1c993-198">Select any notification in an **Error** state (those with a red (!) next to them).</span></span>

  ><span data-ttu-id="1c993-199">! Observera] du kan klicka på meddelanden i en **lyckade** eller **pågår** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1c993-199">!NOTE] You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
  >
  >

3.  <span data-ttu-id="1c993-200">Den här öppna den **detaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="1c993-200">This open the **Notification Details** blade.</span></span>

4.  <span data-ttu-id="1c993-201">Använd den här informationen dig att förstå mer information om problemet.</span><span class="sxs-lookup"><span data-stu-id="1c993-201">Use this information yourself to understand more details about the problem.</span></span>

5.  <span data-ttu-id="1c993-202">Om du fortfarande behöver hjälp kan du också dela informationen med en supporttekniker eller produktgruppen för att få hjälp med problemet.</span><span class="sxs-lookup"><span data-stu-id="1c993-202">If you still need help, you can also share this information with a support engineer or the product group to get help with your problem.</span></span>

6.  <span data-ttu-id="1c993-203">Klickar du på den **kopiera** **ikonen** till höger om den **Kopieringsfel** textruta för att kopiera meddelande allt delar med en support eller produkt grupp tekniker.</span><span class="sxs-lookup"><span data-stu-id="1c993-203">Click the **copy** **icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer.</span></span>

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a><span data-ttu-id="1c993-204">Få hjälp genom att skicka meddelandeinformation till en supporttekniker</span><span class="sxs-lookup"><span data-stu-id="1c993-204">How to get help by sending notification details to a support engineer</span></span>

<span data-ttu-id="1c993-205">Det är mycket viktigt att du dela **allt som anges nedan** med en supporttekniker om du behöver hjälp, så att de kan hjälpa dig snabbt.</span><span class="sxs-lookup"><span data-stu-id="1c993-205">It is very important that you share **all the details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="1c993-206">Du kan göra det enkelt av **tar en skärmbild** eller genom att klicka på den **kopiera felikonen**, hittade till höger om den **Kopieringsfel** textruta.</span><span class="sxs-lookup"><span data-stu-id="1c993-206">You can do this easily by **taking a screenshot,** or by clicking the **Copy error icon**, found to the right of the **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="1c993-207">Meddelandeinformation förklaras</span><span class="sxs-lookup"><span data-stu-id="1c993-207">Notification Details Explained</span></span>

<span data-ttu-id="1c993-208">Den nedan beskrivs mer vad varje av meddelandet objekt innebär och innehåller exempel på var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="1c993-208">The below explains more what each of the notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="1c993-209">Viktigt meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="1c993-209">Essential Notification Items</span></span>

-   <span data-ttu-id="1c993-210">**Rubrik** – beskrivande rubrik i meddelandet</span><span class="sxs-lookup"><span data-stu-id="1c993-210">**Title** – the descriptive title of the notification</span></span>

    -   <span data-ttu-id="1c993-211">Exempel – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="1c993-211">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="1c993-212">**Beskrivning** – beskrivning av vad som hänt på grund av åtgärden</span><span class="sxs-lookup"><span data-stu-id="1c993-212">**Description** – the description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="1c993-213">Exempel – **intern url har angett används redan av ett annat program**</span><span class="sxs-lookup"><span data-stu-id="1c993-213">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="1c993-214">**Meddelande-Id** – unikt id för meddelandet</span><span class="sxs-lookup"><span data-stu-id="1c993-214">**Notification Id** – the unique id of the notification</span></span>

    -   <span data-ttu-id="1c993-215">Exempel – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="1c993-215">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="1c993-216">**Id för klientbegäran** – specifik begäran-id som din webbläsare</span><span class="sxs-lookup"><span data-stu-id="1c993-216">**Client Request Id** – the specific request id made by your browser</span></span>

    -   <span data-ttu-id="1c993-217">Exempel – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="1c993-217">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="1c993-218">**Tid UTC stämpel** – tidsstämpeln då uppstod meddelandet i UTC</span><span class="sxs-lookup"><span data-stu-id="1c993-218">**Time Stamp UTC** – the timestamp during which the notification occurred, in UTC</span></span>

    -   <span data-ttu-id="1c993-219">Exempel – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="1c993-219">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="1c993-220">**Internt transaktions-Id** – internt ID vi kan använda för att söka av fel i vårt system</span><span class="sxs-lookup"><span data-stu-id="1c993-220">**Internal Transaction Id** – the internal ID we can use to look the error up in our systems</span></span>

    -   <span data-ttu-id="1c993-221">Exempel – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="1c993-221">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="1c993-222">**UPN** – användaren som utförde åtgärden</span><span class="sxs-lookup"><span data-stu-id="1c993-222">**UPN** – the user who performed the operation</span></span>

    -   <span data-ttu-id="1c993-223">Exempel –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="1c993-223">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="1c993-224">**Klient-Id** – unikt ID för den klient som användaren som utförde åtgärden var medlem av</span><span class="sxs-lookup"><span data-stu-id="1c993-224">**Tenant Id** – the unique ID of the tenant that the user who performed the operation was a member of</span></span>

    -   <span data-ttu-id="1c993-225">Exempel – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="1c993-225">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="1c993-226">**Användarobjektet Id** – unikt ID för den användare som utförde åtgärden</span><span class="sxs-lookup"><span data-stu-id="1c993-226">**User object Id** – the unique ID of the user who performed the operation</span></span>

    -   <span data-ttu-id="1c993-227">Exempel – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="1c993-227">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="1c993-228">Detaljerat meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="1c993-228">Detailed Notification Items</span></span>

-   <span data-ttu-id="1c993-229">**Visningsnamn** – **(kan vara tom)** en mer detaljerad visningsnamn efter fel</span><span class="sxs-lookup"><span data-stu-id="1c993-229">**Display Name** – **(can be empty)** a more detailed display name for the error</span></span>

    -   <span data-ttu-id="1c993-230">Exempel * – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="1c993-230">Example *– **Application proxy settings**</span></span>

-   <span data-ttu-id="1c993-231">**Status för** – specifika status för meddelandet</span><span class="sxs-lookup"><span data-stu-id="1c993-231">**Status** – the specific status of the notification</span></span>

    -   <span data-ttu-id="1c993-232">Exempel * – **misslyckades**</span><span class="sxs-lookup"><span data-stu-id="1c993-232">Example *– **Failed**</span></span>

-   <span data-ttu-id="1c993-233">**Objekt-Id** – **(kan vara tom)** objekt-ID som åtgärden utfördes</span><span class="sxs-lookup"><span data-stu-id="1c993-233">**Object Id** – **(can be empty)** the object ID against which the operation was performed</span></span>

    -   <span data-ttu-id="1c993-234">Exempel – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="1c993-234">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="1c993-235">**Information om** – detaljerad beskrivning av vad som hänt på grund av åtgärden</span><span class="sxs-lookup"><span data-stu-id="1c993-235">**Details** – the detailed description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="1c993-236">Exempel – **intern url 'http://bing.com/' är ogiltig eftersom den redan används**</span><span class="sxs-lookup"><span data-stu-id="1c993-236">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="1c993-237">**Kopiera fel** – klickar du på den **kopiera-ikonen** till höger om den **Kopieringsfel** textruta för att kopiera meddelande allt delar med en support eller produkt grupp tekniker</span><span class="sxs-lookup"><span data-stu-id="1c993-237">**Copy error** – Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

    -   <span data-ttu-id="1c993-238">Exempel –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="1c993-238">Example – ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c993-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1c993-239">Next steps</span></span>
[<span data-ttu-id="1c993-240">Tillhandahålla enkel inloggning till dina appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="1c993-240">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

