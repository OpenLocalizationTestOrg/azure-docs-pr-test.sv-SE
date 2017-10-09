---
title: "aaaError på sidan för ett program när du har loggat in | Microsoft Docs"
description: "Hur tooresolve problem med Azure AD logga in när hello programmet genererar ett fel"
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
ms.openlocfilehash: 317b6f8e6417520ead80ae4e26c591ba6b134683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a><span data-ttu-id="b6526-103">Fel på sidan för ett program när du loggar in</span><span class="sxs-lookup"><span data-stu-id="b6526-103">Error on an application's page after signing in</span></span>

<span data-ttu-id="b6526-104">Azure AD har signerat hello användare i i det här scenariot, men programmet hello visas ett fel som inte tillåter hello toosuccessfully Slutför hello användarinloggning i flödet.</span><span class="sxs-lookup"><span data-stu-id="b6526-104">In this scenario, Azure AD has signed hello user in, but hello application is displaying an error not allowing hello user toosuccessfully finish hello sign in flow.</span></span> <span data-ttu-id="b6526-105">I det här scenariot accepterar inte hello programmet hello svar problemet av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6526-105">In this scenario, hello application is not accepting hello response issue by Azure AD.</span></span>

<span data-ttu-id="b6526-106">Det finns några möjliga orsaker till varför programmet hello accepterade hello svaret från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6526-106">There are some possible reasons why hello application didn’t accept hello response from Azure AD.</span></span> <span data-ttu-id="b6526-107">Om hello fel i hello program inte är tillräckligt tydligt tooknow är vad saknas hello svar:</span><span class="sxs-lookup"><span data-stu-id="b6526-107">If hello error in hello application is not clear enough tooknow what is missing in hello response, then:</span></span>

-   <span data-ttu-id="b6526-108">Om hello programmet hello Azure AD-galleriet, kan du kontrollera att du har följt alla hello stegen i artikeln hello [hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span><span class="sxs-lookup"><span data-stu-id="b6526-108">If hello application is hello Azure AD Gallery, verify you have followed all hello steps in hello article [How toodebug SAML-based single sign-on tooapplications in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).</span></span>

-   <span data-ttu-id="b6526-109">Använda ett verktyg som [Fiddler](http://www.telerik.com/fiddler) toocapture SAML begära SAML-svar och SAML-token.</span><span class="sxs-lookup"><span data-stu-id="b6526-109">Use a tool like [Fiddler](http://www.telerik.com/fiddler) toocapture SAML request, SAML response and SAML token.</span></span>

-   <span data-ttu-id="b6526-110">Dela hello SAML-svar med hello programmet leverantör tooknow vad som saknas.</span><span class="sxs-lookup"><span data-stu-id="b6526-110">Share hello SAML response with hello application vendor tooknow what is missing.</span></span>

## <a name="missing-attributes-in-hello-saml-response"></a><span data-ttu-id="b6526-111">Saknade attribut i hello SAML-svar</span><span class="sxs-lookup"><span data-stu-id="b6526-111">Missing attributes in hello SAML response</span></span>

<span data-ttu-id="b6526-112">tooadd ett attribut i hello Azure AD configuration toobe skickas som svar på hello Azure AD, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="b6526-112">tooadd an attribute in hello Azure AD configuration toobe sent in hello Azure AD response, follow hello steps below:</span></span>

1.  <span data-ttu-id="b6526-113">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="b6526-113">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b6526-114">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-114">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b6526-115">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="b6526-115">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b6526-116">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-116">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b6526-117">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="b6526-117">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="b6526-118">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="b6526-118">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="b6526-119">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6526-119">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="b6526-120">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-120">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b6526-121">Klicka på **visa och redigera alla andra användare attribut under** hello **användarattribut** avsnittet tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="b6526-121">click **View and edit all other user attributes under** hello **User Attributes** section tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="b6526-122">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="b6526-122">tooadd an attribute:</span></span>

   * <span data-ttu-id="b6526-123">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="b6526-123">click **Add attribute**.</span></span> <span data-ttu-id="b6526-124">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="b6526-124">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   * <span data-ttu-id="b6526-125">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="b6526-125">Click **Save.**</span></span> <span data-ttu-id="b6526-126">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b6526-126">You see hello new attribute in hello table.</span></span>

9.  <span data-ttu-id="b6526-127">Spara hello-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b6526-127">Save hello configuration.</span></span>

<span data-ttu-id="b6526-128">Nästa gång hello användaren loggar in toohello programmet Azure AD skicka hello nytt attribut i hello SAML-svar.</span><span class="sxs-lookup"><span data-stu-id="b6526-128">Next time hello user signs in toohello application, Azure AD send hello new attribute in hello SAML response.</span></span>

## <a name="hello-application-expects-a-different-user-identifier-value-or-format"></a><span data-ttu-id="b6526-129">hello program förväntar sig ett annat användar-ID-värde eller format</span><span class="sxs-lookup"><span data-stu-id="b6526-129">hello application expects a different User Identifier value or format</span></span>

<span data-ttu-id="b6526-130">hello misslyckas inloggning toohello programmet eftersom hello SAML-svaret saknar attribut, till exempel roller eller eftersom programmet hello förväntar sig ett annat format för hello ID för entiteterna attribut.</span><span class="sxs-lookup"><span data-stu-id="b6526-130">hello sign-in toohello application is failing because hello SAML response is missing attributes such as roles or because hello application is expecting a different format for hello EntityID attribute.</span></span>

## <a name="add-an-attribute-in-hello-azure-ad-application-configuration"></a><span data-ttu-id="b6526-131">Lägg till ett attribut i programkonfigurationen för hello Azure AD:</span><span class="sxs-lookup"><span data-stu-id="b6526-131">Add an attribute in hello Azure AD application configuration:</span></span>

<span data-ttu-id="b6526-132">toochange hello användar-ID-värde, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="b6526-132">toochange hello User Identifier value, follow hello steps below:</span></span>

1.  <span data-ttu-id="b6526-133">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="b6526-133">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b6526-134">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-134">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b6526-135">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="b6526-135">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b6526-136">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-136">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b6526-137">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="b6526-137">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="b6526-138">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="b6526-138">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="b6526-139">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6526-139">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="b6526-140">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-140">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b6526-141">Under hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="b6526-141">Under hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

## <a name="change-entityid-user-identifier-format"></a><span data-ttu-id="b6526-142">Ändra ID för entiteterna (användar-ID)-format</span><span class="sxs-lookup"><span data-stu-id="b6526-142">Change EntityID (User Identifier) format</span></span>

<span data-ttu-id="b6526-143">Om programmet hello förväntar sig ett annat format för hello ID för entiteterna attribut.</span><span class="sxs-lookup"><span data-stu-id="b6526-143">If hello application expects another format for hello EntityID attribute.</span></span> <span data-ttu-id="b6526-144">Du kan sedan inte kan tooselect hello ID för entiteterna (användar-ID) format att Azure AD skickar toohello programmet hello svar efter autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="b6526-144">Then, you won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="b6526-145">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="b6526-145">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="b6526-146">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="b6526-146">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>

## <a name="hello-application-expects-a-different-signature-method-for-hello-saml-response"></a><span data-ttu-id="b6526-147">hello program förväntar sig en annan signaturmetod för hello SAML-svar</span><span class="sxs-lookup"><span data-stu-id="b6526-147">hello application expects a different signature method for hello SAML response</span></span>

<span data-ttu-id="b6526-148">toochange vilka delar av hello SAML-token har signerats digitalt av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b6526-148">toochange what parts of hello SAML token are digitally signed by Azure Active Directory.</span></span> <span data-ttu-id="b6526-149">Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="b6526-149">Follow hello steps below:</span></span>

1.  <span data-ttu-id="b6526-150">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="b6526-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b6526-151">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b6526-152">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="b6526-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b6526-153">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b6526-154">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="b6526-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="b6526-155">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="b6526-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="b6526-156">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6526-156">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="b6526-157">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-157">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b6526-158">Klicka på **visa avancerade inställningar för signering av certifikat** under hello **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b6526-158">click **Show advanced certificate signing settings** under hello **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="b6526-159">Välj lämplig hello **signering alternativet** förväntades av programmet hello:</span><span class="sxs-lookup"><span data-stu-id="b6526-159">Select hello appropriate **Signing Option** expected by hello application:</span></span>

  * <span data-ttu-id="b6526-160">Signera SAML-svar</span><span class="sxs-lookup"><span data-stu-id="b6526-160">Sign SAML response</span></span>

  * <span data-ttu-id="b6526-161">Signera SAML-svar och assertion</span><span class="sxs-lookup"><span data-stu-id="b6526-161">Sign SAML response and assertion</span></span>

  * <span data-ttu-id="b6526-162">Signera SAML-kontroll</span><span class="sxs-lookup"><span data-stu-id="b6526-162">Sign SAML assertion</span></span>

<span data-ttu-id="b6526-163">Nästa gång hello användaren loggar in toohello programmet hello Azure AD-inloggning en del av hello SAML-svar som valts.</span><span class="sxs-lookup"><span data-stu-id="b6526-163">Next time hello user signs in toohello application, Azure AD sign hello part of hello SAML response selected.</span></span>

## <a name="hello-application-expects-hello-signing-algorithm-toobe-sha-1"></a><span data-ttu-id="b6526-164">hello program förväntar sig hello signering toobe algoritmen SHA-1</span><span class="sxs-lookup"><span data-stu-id="b6526-164">hello application expects hello signing algorithm toobe SHA-1</span></span>

<span data-ttu-id="b6526-165">Som standard loggar Azure AD hello SAML-token med hello algoritm för de flesta.</span><span class="sxs-lookup"><span data-stu-id="b6526-165">By default, Azure AD signs hello SAML token using hello most security algorithm.</span></span> <span data-ttu-id="b6526-166">Du bör inte ändra hello logga algoritmen tooSHA-1 om det inte krävs av hello program.</span><span class="sxs-lookup"><span data-stu-id="b6526-166">Changing hello sign algorithm tooSHA-1 is not recommended unless required by hello application.</span></span>

<span data-ttu-id="b6526-167">toochange Hej Signeringsalgoritm, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b6526-167">toochange hello signing algorithm, follow hello steps below:</span></span>

1.  <span data-ttu-id="b6526-168">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="b6526-168">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b6526-169">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-169">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b6526-170">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="b6526-170">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b6526-171">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-171">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b6526-172">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="b6526-172">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="b6526-173">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="b6526-173">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="b6526-174">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6526-174">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="b6526-175">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b6526-175">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b6526-176">Klicka på **visa avancerade inställningar för signering av certifikat** under hello **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b6526-176">click **Show advanced certificate signing settings** under hello **SAML Signing Certificate** section.</span></span>

9.  <span data-ttu-id="b6526-177">Välj SHA-1 i hello **signering algoritmen**.</span><span class="sxs-lookup"><span data-stu-id="b6526-177">Select SHA-1, in hello **Signing Algorithm**.</span></span>

<span data-ttu-id="b6526-178">Nästa gång Hej användaren loggar in toohello program, Azure AD logga hello SAML-token med hjälp av SHA-1-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="b6526-178">Next time hello user signs in toohello application, Azure AD sign hello SAML token using SHA-1 algorithm.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6526-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b6526-179">Next steps</span></span>
[<span data-ttu-id="b6526-180">Hur toodebug SAML-baserade enkel inloggning tooapplications i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b6526-180">How toodebug SAML-based single sign-on tooapplications in Azure Active Directory</span></span>](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
