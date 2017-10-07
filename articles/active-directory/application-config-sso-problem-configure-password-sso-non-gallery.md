---
title: "Konfigurera lösenord enkel inloggning för ett icke-galleriet program aaaProblem | Microsoft Docs"
description: "Förstå hello vanliga problem personer yta när du konfigurerar lösenord enkel inloggning för anpassade program för icke-galleriet som inte listas i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 3aee0a4c525bb3da338da2da0882ec572cf0e5e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="14a49-103">Konfigurera lösenord enkel inloggning för ett icke-galleriet program problem</span><span class="sxs-lookup"><span data-stu-id="14a49-103">Problem configuring password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="14a49-104">Den här artikeln hjälper dig toounderstand hello vanliga problem personer står inför när du konfigurerar **lösenord enkel inloggning** med ett icke-galleriet program.</span><span class="sxs-lookup"><span data-stu-id="14a49-104">This article help you toounderstand hello common problems people face when configuring **Password single sign-on** with a non-gallery application.</span></span>

## <a name="how-toocapture-sign-in-fields-for-an-application"></a><span data-ttu-id="14a49-105">Hur inloggning toocapture fält för ett program</span><span class="sxs-lookup"><span data-stu-id="14a49-105">How toocapture sign-in fields for an application</span></span>

<span data-ttu-id="14a49-106">Logga in fältet stöds bara för HTML-aktiverade inloggningssidor och är **stöds inte för icke-standard inloggningssidor**, som de som använder Flash eller andra icke-HTML-aktiverade tekniker.</span><span class="sxs-lookup"><span data-stu-id="14a49-106">Sign-in field capture is only supported for HTML-enabled sign-in pages and is **not supported for non-standard sign-in pages**, like those that use Flash, or other non-HTML-enabled technologies.</span></span>

<span data-ttu-id="14a49-107">Det finns två sätt som du kan avbilda inloggning fält för dina anpassade program:</span><span class="sxs-lookup"><span data-stu-id="14a49-107">There are two ways you can capture sign-in fields for your custom applications:</span></span>

-   <span data-ttu-id="14a49-108">Fältet för automatisk inloggning avbildning</span><span class="sxs-lookup"><span data-stu-id="14a49-108">Automatic sign-in field capture</span></span>

-   <span data-ttu-id="14a49-109">Fältet för manuell inloggning avbildning</span><span class="sxs-lookup"><span data-stu-id="14a49-109">Manual sign-in field capture</span></span>

<span data-ttu-id="14a49-110">**Fältet för automatisk inloggning avbilda** fungerar väl med de flesta HTML-aktiverade inloggningssidor, om de använder **välkända DIV-ID: N för hello användarnamn och lösenord Ange** fältet.</span><span class="sxs-lookup"><span data-stu-id="14a49-110">**Automatic sign-in field capture** works well with most HTML-enabled sign-in pages, if they use **well-known DIV IDs for hello username and password input** field.</span></span> <span data-ttu-id="14a49-111">Hej hur detta fungerar är genom skrapning hello HTML på hello sidan toofind DIV-ID: N som matchar vissa villkor och sedan spara att metadata för det här programmet så att vi kan spela upp lösenord tooit senare.</span><span class="sxs-lookup"><span data-stu-id="14a49-111">hello way this works is by scraping hello HTML on hello page toofind DIV IDs that match certain criteria and by then saving that metadata for this application so we can replay passwords tooit later.</span></span>

<span data-ttu-id="14a49-112">**Fältet för manuell inloggning avbilda** kan användas i hello ärende hello programmet **leverantör inte etiketten** hello ange fälten som används för inloggning.</span><span class="sxs-lookup"><span data-stu-id="14a49-112">**Manual sign-in field capture** can be used in hello case that hello application **vendor does not label** hello input fields used for sign in.</span></span> <span data-ttu-id="14a49-113">Fältet för manuell inloggning avbilda kan också användas i hello fallet när hello **leverantör återgivningar flera fält** som inte kan identifieras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="14a49-113">Manual sign-in field capture can also be used in hello case when hello **vendor renders multiple fields** which cannot be auto-detected.</span></span> <span data-ttu-id="14a49-114">Azure AD kan lagra data för så många fält som finns på hello inloggningssidan så länge du berätta var dessa fält är i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="14a49-114">Azure AD can store data for as many fields as are on hello sign in page, so long as you tell us where those fields are on hello page.</span></span>

<span data-ttu-id="14a49-115">I allmänhet **om automatisk inloggning fältet avbilda inte fungerar, föreslår vi alltid försöker hello manuella alternativet.**</span><span class="sxs-lookup"><span data-stu-id="14a49-115">In general, **if automatic sign-in field capture does not work, we always suggest trying hello manual option.**</span></span>

### <a name="how-tooautomatically-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="14a49-116">Hur tooautomatically avbilda inloggning fält för ett program</span><span class="sxs-lookup"><span data-stu-id="14a49-116">How tooautomatically capture sign-in fields for an application</span></span>

<span data-ttu-id="14a49-117">tooconfigure **lösenordsbaserade enkel inloggning** för ett program som använder **fält för automatisk inloggning avbilda**, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="14a49-117">tooconfigure **Password-based Single Sign-on** for an application using **automatic sign-in field capture**, follow hello steps below:</span></span>

1.  <span data-ttu-id="14a49-118">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="14a49-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="14a49-119">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="14a49-119">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="14a49-120">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="14a49-120">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="14a49-121">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="14a49-121">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="14a49-122">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="14a49-122">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="14a49-123">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="14a49-123">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="14a49-124">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="14a49-124">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="14a49-125">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="14a49-125">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="14a49-126">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="14a49-126">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="14a49-127">Ange hello **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="14a49-127">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="14a49-128">Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till.</span><span class="sxs-lookup"><span data-stu-id="14a49-128">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="14a49-129">**Se till att hello inloggning fält är synliga på hello-URL som du anger**.</span><span class="sxs-lookup"><span data-stu-id="14a49-129">**Ensure hello sign in fields are visible at hello URL you provide**.</span></span>

10. <span data-ttu-id="14a49-130">Klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="14a49-130">Click hello **Save** button.</span></span>

11. <span data-ttu-id="14a49-131">När du gör det, kommer vi automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och kan du överföra toouse Azure AD toosecurely lösenord toothat program med hjälp av hello åtkomst panelen webbläsartillägg.</span><span class="sxs-lookup"><span data-stu-id="14a49-131">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span>

## <a name="how-toomanually-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="14a49-132">Hur toomanually avbilda inloggning fält för ett program</span><span class="sxs-lookup"><span data-stu-id="14a49-132">How toomanually capture sign-in fields for an application</span></span>

<span data-ttu-id="14a49-133">toomanually avbilda inloggning fält, måste du först ha hello åtkomst panelen webbläsartillägget installerad och **inte körs i inPrivate incognito eller privat läge.**</span><span class="sxs-lookup"><span data-stu-id="14a49-133">toomanually capture sign in fields, you must first have hello Access Panel Browser extension installed and **not be running in inPrivate, incognito, or private mode.**</span></span> <span data-ttu-id="14a49-134">tooinstall hello webbläsartillägg, följ hello stegen i hello [hur tooinstall hello åtkomst panelen webbläsartillägget](#i-cannot-manually-detect-sign-in-fields-for-my-application) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="14a49-134">tooinstall hello browser extension, follow hello steps in hello [How tooinstall hello Access Panel Browser extension](#i-cannot-manually-detect-sign-in-fields-for-my-application) section.</span></span>

<span data-ttu-id="14a49-135">tooconfigure **lösenordsbaserade enkel inloggning** för ett program som använder **manuell inloggning fältet avbilda**, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="14a49-135">tooconfigure **Password-based Single Sign-on** for an application using **manual sign-in field capture**, follow hello steps below:</span></span>

1.  <span data-ttu-id="14a49-136">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="14a49-136">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="14a49-137">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="14a49-137">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="14a49-138">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="14a49-138">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="14a49-139">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="14a49-139">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="14a49-140">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="14a49-140">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="14a49-141">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="14a49-141">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="14a49-142">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="14a49-142">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="14a49-143">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="14a49-143">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="14a49-144">Välj hello läge **lösenordsbaserade inloggning.**</span><span class="sxs-lookup"><span data-stu-id="14a49-144">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="14a49-145">Ange hello **inloggnings-URL**.</span><span class="sxs-lookup"><span data-stu-id="14a49-145">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="14a49-146">Det här är hello URL där användarna anger sina användarnamn och lösenord toosign i till.</span><span class="sxs-lookup"><span data-stu-id="14a49-146">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="14a49-147">**Se till att hello inloggning fält är synliga på hello-URL som du anger**.</span><span class="sxs-lookup"><span data-stu-id="14a49-147">**Ensure hello sign in fields are visible at hello URL you provide**.</span></span>

10. <span data-ttu-id="14a49-148">Klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="14a49-148">Click hello **Save** button.</span></span>

11. <span data-ttu-id="14a49-149">När du gör det, kommer vi automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och kan du överföra toouse Azure AD toosecurely lösenord toothat program med hjälp av hello åtkomst panelen webbläsartillägg.</span><span class="sxs-lookup"><span data-stu-id="14a49-149">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span> <span data-ttu-id="14a49-150">Hello om detta misslyckas, kan du **ändra hello inloggning läge toouse manuell inloggning fältet avbilda** genom att fortsätta toostep 12.</span><span class="sxs-lookup"><span data-stu-id="14a49-150">In hello case that this fails, you can **change hello sign-in mode toouse manual sign-in field capture** by continuing toostep 12.</span></span>

12. <span data-ttu-id="14a49-151">Klicka på **konfigurera &lt;appname&gt; inställningar för lösenord enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="14a49-151">click **Configure &lt;appname&gt; Password Single Sign-on Settings**.</span></span>

13. <span data-ttu-id="14a49-152">Välj hello **identifieras manuellt inloggning fält** konfigurationsalternativet.</span><span class="sxs-lookup"><span data-stu-id="14a49-152">Select hello **Manually detect sign-in fields** configuration option.</span></span>

14. <span data-ttu-id="14a49-153">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="14a49-153">Click **Ok**.</span></span>

15. <span data-ttu-id="14a49-154">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="14a49-154">Click **Save**.</span></span>

16. <span data-ttu-id="14a49-155">Följ hello på skärmen instruktioner toouse hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="14a49-155">Follow hello on screen instructions toouse hello Access Panel.</span></span>

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a><span data-ttu-id="14a49-156">Felet ”Det gick inte att hitta alla fält på den URL” visas</span><span class="sxs-lookup"><span data-stu-id="14a49-156">I see a “We couldn’t find any sign-in fields at that URL” error</span></span>

<span data-ttu-id="14a49-157">Det här felet visas när automatisk identifiering av inloggning fält misslyckas.</span><span class="sxs-lookup"><span data-stu-id="14a49-157">You see this error when automatic detection of sign-in fields fails.</span></span> <span data-ttu-id="14a49-158">Det här problemet, försök manuell inloggning fältet identifiering av följande hello stegen i hello tooresolve [hur toomanually avbilda inloggning fält för ett program](#how-to-manually-capture-sign-in-fields-for-an-application) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="14a49-158">tooresolve this issue, try manual sign-in field detection by following hello steps in hello [How toomanually capture sign-in fields for an application](#how-to-manually-capture-sign-in-fields-for-an-application) section.</span></span>

## <a name="i-see-an-unable-toosave-single-sign-on-configuration-error"></a><span data-ttu-id="14a49-159">Visas felmeddelandet ”Det gick inte toosave Single Sign-on-konfiguration”</span><span class="sxs-lookup"><span data-stu-id="14a49-159">I see an “Unable toosave Single Sign-on configuration” error</span></span>

<span data-ttu-id="14a49-160">I vissa sällsynta fall kan uppdatera hello enkel inloggning konfigurationen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="14a49-160">In certain rare cases, updating hello single sign-on configuration can fail.</span></span> <span data-ttu-id="14a49-161">tooresolve detta försök spara hello enkel inloggning konfigurationen igen.</span><span class="sxs-lookup"><span data-stu-id="14a49-161">tooresolve this try saving hello single sign-on configuration again.</span></span>

<span data-ttu-id="14a49-162">Om problemet kvarstår toofail konsekvent, öppna ett supportärende och ange hello information som samlats in i hello [hur toosee hello information om en portalmeddelandet](#i-cannot-manually-detect-sign-in-fields-for-my-application) och [hur tooget genom att skicka meddelande information tooa stöd för tekniker](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="14a49-162">If this continues toofail consistently, open a support case and provide hello information gathered in hello [How toosee hello details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How tooget help by sending notification details tooa support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections.</span></span>

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a><span data-ttu-id="14a49-163">Jag kan inte manuellt identifiera tecken i fälten för Mina program</span><span class="sxs-lookup"><span data-stu-id="14a49-163">I cannot manually detect sign in fields for my application</span></span>

<span data-ttu-id="14a49-164">Hello-beteenden som kan uppstå när Manuell identifiering inte fungerar bland annat:</span><span class="sxs-lookup"><span data-stu-id="14a49-164">Some of hello behaviors you might see when manual detection is not working include:</span></span>

-   <span data-ttu-id="14a49-165">hello manuell bildtagningen fanns toowork, men hello fält avbildas inte korrekt</span><span class="sxs-lookup"><span data-stu-id="14a49-165">hello manual capture process appeared toowork, but hello fields captured were not correct</span></span>

-   <span data-ttu-id="14a49-166">hello höger fält hämta inte markeras när du utför hello avbildningsprocessen</span><span class="sxs-lookup"><span data-stu-id="14a49-166">hello right fields don’t get highlighted when performing hello capture process</span></span>

-   <span data-ttu-id="14a49-167">hello processen tar mig toohello programmets inloggningssida som förväntat, men ingenting händer</span><span class="sxs-lookup"><span data-stu-id="14a49-167">hello capture process takes me toohello application’s login page as expected, but nothing happens</span></span>

-   <span data-ttu-id="14a49-168">Manuell avbilda visas toowork men SSO inträffa inte när användarna navigerar toohello programmet från hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="14a49-168">Manual capture appears toowork, but SSO doesn’t happen when my users navigate toohello application from hello Access Panel.</span></span>

<span data-ttu-id="14a49-169">Kontrollera hello följande om du stöter på något av följande problem:</span><span class="sxs-lookup"><span data-stu-id="14a49-169">check hello following if you encounter any of these issues:</span></span>

-   <span data-ttu-id="14a49-170">Kontrollera att du har hello senaste versionen av hello åtkomst panelen webbläsartillägget **installerat** och **aktiverat** genom att följa stegen hello i hello [hur tooinstall hello åtkomst panelen webbläsare tillägget](#how-to-install-the-access-panel-browser-extension) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="14a49-170">Ensure that you have hello latest version of hello access panel browser extension **installed** and **enabled** by following hello steps in hello [How tooinstall hello Access Panel Browser extension](#how-to-install-the-access-panel-browser-extension) section.</span></span>

-   <span data-ttu-id="14a49-171">Se till att du inte försöker hello processen när webbläsaren i **incognito InPrivate- eller privat läge**.</span><span class="sxs-lookup"><span data-stu-id="14a49-171">Ensure that you are not attempting hello capture process while your browser in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="14a49-172">tillägget för hello åtkomst panelen stöds inte i dessa lägen.</span><span class="sxs-lookup"><span data-stu-id="14a49-172">hello access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="14a49-173">Se till att användarna inte försöker toosign i toohello programmet hello åtkomst panelen när i **incognito InPrivate- eller privat läge**.</span><span class="sxs-lookup"><span data-stu-id="14a49-173">Ensure that your users are not trying toosign in toohello application from hello access panel while in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="14a49-174">tillägget för hello åtkomst panelen stöds inte i dessa lägen.</span><span class="sxs-lookup"><span data-stu-id="14a49-174">hello access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="14a49-175">Försök hello manuella processen igen, säkerställt hello röda markörer är över hello rätt fält.</span><span class="sxs-lookup"><span data-stu-id="14a49-175">Try hello manual capture process again, ensuring hello red markers are over hello correct fields.</span></span>

-   <span data-ttu-id="14a49-176">Om hello manuell bildtagningen verkar toohang eller hello inloggningssidan inte göra något (fall 3 ovan), försök hello manuella processen igen.</span><span class="sxs-lookup"><span data-stu-id="14a49-176">If hello manual capture process seems toohang, or hello sign in page doesn’t do anything (case 3 above), try hello manual capture process again.</span></span> <span data-ttu-id="14a49-177">Men nu när du har slutfört hello process, tryck på hello **F12** knappen tooopen webbläsarens developer-konsolen.</span><span class="sxs-lookup"><span data-stu-id="14a49-177">But, this time after completing hello process, press hello **F12** button tooopen your browser’s developer console.</span></span> <span data-ttu-id="14a49-178">Öppna en gång, hello **konsolen** och skriv **window.location= ”&lt;ange hello tecken i url som du angav när du konfigurerar hello app&gt;”** och tryck sedan på  **Ange**.</span><span class="sxs-lookup"><span data-stu-id="14a49-178">Once there, open hello **console** and type **window.location=”&lt;enter hello sign in url you specified when configuring hello app&gt;”** and then press **Enter**.</span></span> <span data-ttu-id="14a49-179">Denna kraft en sida omdirigera som avslutas hello processen och lagra hello fält som har spelats in.</span><span class="sxs-lookup"><span data-stu-id="14a49-179">This force a page redirect which end hello capture process and store hello fields that have been captured.</span></span>

<span data-ttu-id="14a49-180">Om inget av dessa sätt fungerar för dig kan vi hjälpa.</span><span class="sxs-lookup"><span data-stu-id="14a49-180">If none of these approaches work for you, we can help.</span></span> <span data-ttu-id="14a49-181">Öppna ett supportärende hello uppgifter om du försökte samt hello information som samlats in i hello [hur toosee hello information om en portalmeddelandet](#i-cannot-manually-detect-sign-in-fields-for-my-application) och [hur tooget genom att skicka meddelandeinformation tooa stöd tekniker](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) avsnitten (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="14a49-181">Open a support case with hello details of what you tried, as well as hello information gathered in hello [How toosee hello details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How tooget help by sending notification details tooa support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections (if applicable).</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="14a49-182">Hur tooinstall hello åtkomst panelen webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="14a49-182">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="14a49-183">tooinstall hello webbläsartillägget för åtkomst till Kontrollpanelen, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="14a49-183">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="14a49-184">Öppna hello [åtkomstpanelen](https://myapps.microsoft.com) i någon av hello stöds webbläsare och logga in som en **användaren** i din Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14a49-184">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="14a49-185">Klicka på en **lösenord SSO-program** i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="14a49-185">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="14a49-186">I hello fråga frågar tooinstall hello programvara, väljer **installera nu**.</span><span class="sxs-lookup"><span data-stu-id="14a49-186">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="14a49-187">Baserat på din webbläsare vara du riktad toohello länken.</span><span class="sxs-lookup"><span data-stu-id="14a49-187">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="14a49-188">**Lägg till** hello tillägget tooyour webbläsare.</span><span class="sxs-lookup"><span data-stu-id="14a49-188">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="14a49-189">Om din webbläsare frågar väljer tooeither **aktivera** eller **Tillåt** hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="14a49-189">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="14a49-190">När den har installerats, **starta om** webbläsarsessionen.</span><span class="sxs-lookup"><span data-stu-id="14a49-190">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="14a49-191">Logga in till hello åtkomstpanelen och se om kan du **starta** lösenord SSO-program.</span><span class="sxs-lookup"><span data-stu-id="14a49-191">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications.</span></span>

<span data-ttu-id="14a49-192">Du kan också hämta hello-tillägget för Chrome och Firefox från hello Direktlänkar nedan:</span><span class="sxs-lookup"><span data-stu-id="14a49-192">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="14a49-193">Tillägget för Chrome åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="14a49-193">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="14a49-194">Tillägget för Firefox åtkomst panelen</span><span class="sxs-lookup"><span data-stu-id="14a49-194">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-toosee-hello-details-of-a-portal-notification"></a><span data-ttu-id="14a49-195">Hur toosee hello information om ett meddelande om portal</span><span class="sxs-lookup"><span data-stu-id="14a49-195">How toosee hello details of a portal notification</span></span>

<span data-ttu-id="14a49-196">Du kan se hello information om eventuella portalmeddelandet genom att följa hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="14a49-196">You can see hello details of any portal notification by following hello steps below:</span></span>

1.  <span data-ttu-id="14a49-197">Klicka på hello **meddelanden** ikon (hello bell) i hello övre högra hörnet av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="14a49-197">click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal</span></span>

2.  <span data-ttu-id="14a49-198">Markera alla meddelanden i en **fel** tillstånd (de med en röd (!) nästa toothem).</span><span class="sxs-lookup"><span data-stu-id="14a49-198">Select any notification in an **Error** state (those with a red (!) next toothem).</span></span>

  ><span data-ttu-id="14a49-199">! Observera] du kan klicka på meddelanden i en **lyckade** eller **pågår** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="14a49-199">!NOTE] You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
  >
  >

3.  <span data-ttu-id="14a49-200">Den här öppna hello **detaljer** bladet.</span><span class="sxs-lookup"><span data-stu-id="14a49-200">This open hello **Notification Details** blade.</span></span>

4.  <span data-ttu-id="14a49-201">Använd den här informationen själv toounderstand mer information om hello problem.</span><span class="sxs-lookup"><span data-stu-id="14a49-201">Use this information yourself toounderstand more details about hello problem.</span></span>

5.  <span data-ttu-id="14a49-202">Om du fortfarande behöver hjälp kan också dela informationen med en stöd tekniker eller hello grupp tooget produkthjälp med ditt problem.</span><span class="sxs-lookup"><span data-stu-id="14a49-202">If you still need help, you can also share this information with a support engineer or hello product group tooget help with your problem.</span></span>

6.  <span data-ttu-id="14a49-203">Klicka på hello **kopia** **ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker.</span><span class="sxs-lookup"><span data-stu-id="14a49-203">Click hello **copy** **icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer.</span></span>

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a><span data-ttu-id="14a49-204">Hur tooget genom att skicka meddelande information tooa supportteknikern</span><span class="sxs-lookup"><span data-stu-id="14a49-204">How tooget help by sending notification details tooa support engineer</span></span>

<span data-ttu-id="14a49-205">Det är mycket viktigt att du dela **alla hello information som visas nedan** med en supporttekniker om du behöver hjälp, så att de kan hjälpa dig snabbt.</span><span class="sxs-lookup"><span data-stu-id="14a49-205">It is very important that you share **all hello details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="14a49-206">Du kan göra det enkelt av **tar en skärmbild** eller genom att klicka på hello **kopiera felikonen**, hitta toohello höger om hello **Kopieringsfel** textruta.</span><span class="sxs-lookup"><span data-stu-id="14a49-206">You can do this easily by **taking a screenshot,** or by clicking hello **Copy error icon**, found toohello right of hello **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="14a49-207">Meddelandeinformation förklaras</span><span class="sxs-lookup"><span data-stu-id="14a49-207">Notification Details Explained</span></span>

<span data-ttu-id="14a49-208">hello nedan förklaras mer vad varje hello avisering artiklar innebär och ger exempel på var och en av dem.</span><span class="sxs-lookup"><span data-stu-id="14a49-208">hello below explains more what each of hello notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="14a49-209">Viktigt meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="14a49-209">Essential Notification Items</span></span>

-   <span data-ttu-id="14a49-210">**Rubrik** – hello beskrivande rubrik av hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="14a49-210">**Title** – hello descriptive title of hello notification</span></span>

    -   <span data-ttu-id="14a49-211">Exempel – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="14a49-211">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="14a49-212">**Beskrivning** – hello beskrivning av vad som hänt på grund av hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="14a49-212">**Description** – hello description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="14a49-213">Exempel – **intern url har angett används redan av ett annat program**</span><span class="sxs-lookup"><span data-stu-id="14a49-213">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="14a49-214">**Meddelande-Id** – hello unikt id för hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="14a49-214">**Notification Id** – hello unique id of hello notification</span></span>

    -   <span data-ttu-id="14a49-215">Exempel – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="14a49-215">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="14a49-216">**Id för klientbegäran** – hello specifik begäran-id som din webbläsare</span><span class="sxs-lookup"><span data-stu-id="14a49-216">**Client Request Id** – hello specific request id made by your browser</span></span>

    -   <span data-ttu-id="14a49-217">Exempel – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="14a49-217">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="14a49-218">**Tid UTC stämpel** – hello tidsstämpeln då uppstod hello-meddelande i UTC</span><span class="sxs-lookup"><span data-stu-id="14a49-218">**Time Stamp UTC** – hello timestamp during which hello notification occurred, in UTC</span></span>

    -   <span data-ttu-id="14a49-219">Exempel – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="14a49-219">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="14a49-220">**Internt transaktions-Id** – hello internt ID vi kan använda toolook hello fel i vårt system</span><span class="sxs-lookup"><span data-stu-id="14a49-220">**Internal Transaction Id** – hello internal ID we can use toolook hello error up in our systems</span></span>

    -   <span data-ttu-id="14a49-221">Exempel – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="14a49-221">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="14a49-222">**UPN** – hello-användaren som utförde åtgärden hello</span><span class="sxs-lookup"><span data-stu-id="14a49-222">**UPN** – hello user who performed hello operation</span></span>

    -   <span data-ttu-id="14a49-223">Exempel –**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="14a49-223">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="14a49-224">**Klient-Id** – hello unikt ID för hello-klient som hello användaren som utförde åtgärden hello var medlem av</span><span class="sxs-lookup"><span data-stu-id="14a49-224">**Tenant Id** – hello unique ID of hello tenant that hello user who performed hello operation was a member of</span></span>

    -   <span data-ttu-id="14a49-225">Exempel – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="14a49-225">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="14a49-226">**Användarobjektet Id** – hello unikt ID för hello-användaren som utförde åtgärden hello</span><span class="sxs-lookup"><span data-stu-id="14a49-226">**User object Id** – hello unique ID of hello user who performed hello operation</span></span>

    -   <span data-ttu-id="14a49-227">Exempel – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="14a49-227">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="14a49-228">Detaljerat meddelande objekt</span><span class="sxs-lookup"><span data-stu-id="14a49-228">Detailed Notification Items</span></span>

-   <span data-ttu-id="14a49-229">**Visningsnamn** – **(kan vara tom)** en mer detaljerad visningsnamn för hello-fel</span><span class="sxs-lookup"><span data-stu-id="14a49-229">**Display Name** – **(can be empty)** a more detailed display name for hello error</span></span>

    -   <span data-ttu-id="14a49-230">Exempel * – **Application proxy-inställningar**</span><span class="sxs-lookup"><span data-stu-id="14a49-230">Example *– **Application proxy settings**</span></span>

-   <span data-ttu-id="14a49-231">**Status för** – hello viss status av hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="14a49-231">**Status** – hello specific status of hello notification</span></span>

    -   <span data-ttu-id="14a49-232">Exempel * – **misslyckades**</span><span class="sxs-lookup"><span data-stu-id="14a49-232">Example *– **Failed**</span></span>

-   <span data-ttu-id="14a49-233">**Objekt-Id** – **(kan vara tom)** hello mot vilken hello åtgärden utfördes objekt-ID</span><span class="sxs-lookup"><span data-stu-id="14a49-233">**Object Id** – **(can be empty)** hello object ID against which hello operation was performed</span></span>

    -   <span data-ttu-id="14a49-234">Exempel – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="14a49-234">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="14a49-235">**Information om** – hello detaljerad beskrivning av vad som hänt på grund av hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="14a49-235">**Details** – hello detailed description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="14a49-236">Exempel – **intern url 'http://bing.com/' är ogiltig eftersom den redan används**</span><span class="sxs-lookup"><span data-stu-id="14a49-236">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="14a49-237">**Kopiera fel** – Klicka på hello **kopiera-ikonen** toohello höger i hello **Kopieringsfel** textruta toocopy alla hello meddelande information tooshare med en support eller produkt grupp tekniker</span><span class="sxs-lookup"><span data-stu-id="14a49-237">**Copy error** – Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

    -   <span data-ttu-id="14a49-238">Exempel –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="14a49-238">Example – ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="14a49-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14a49-239">Next steps</span></span>
[<span data-ttu-id="14a49-240">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="14a49-240">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

