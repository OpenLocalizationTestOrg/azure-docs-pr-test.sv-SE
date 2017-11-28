---
title: "aaaProblems inloggning tooa icke-galleriet program konfigurerade för federerad enkel inloggning | Microsoft Docs"
description: "Riktlinjer för hello specifika problem som du kan stöta på när du loggar in tooan program som har konfigurerats för SAML-baserade federerad enkel inloggning med Azure AD"
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
ms.openlocfilehash: 1243456695c097f404a66fc89893efa2afdaaf22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-non-gallery-application-configured-for-federated-single-sign-on"></a><span data-ttu-id="9107b-103">Problem med att logga i tooa icke-galleriet program konfigurerade för federerad enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9107b-103">Problems signing in tooa non-gallery application configured for federated single sign-on</span></span>

<span data-ttu-id="9107b-104">tootroubleshoot ditt problem, behöver du tooverify hello programkonfigurationen i Azure AD som följer:</span><span class="sxs-lookup"><span data-stu-id="9107b-104">tootroubleshoot your problem, you need tooverify hello application configuration in Azure AD as follow:</span></span>

-   <span data-ttu-id="9107b-105">Du har följt alla hello konfigurationssteg för hello Azure AD-galleriet-program.</span><span class="sxs-lookup"><span data-stu-id="9107b-105">You have followed all hello configuration steps for hello Azure AD gallery application.</span></span>

-   <span data-ttu-id="9107b-106">hello identifierare och Reply-URL som konfigurerats i AAD matchar de förväntade värdena i hello program</span><span class="sxs-lookup"><span data-stu-id="9107b-106">hello Identifier and Reply URL configured in AAD match they expected values in hello application</span></span>

-   <span data-ttu-id="9107b-107">Du har tilldelat användare toohello program</span><span class="sxs-lookup"><span data-stu-id="9107b-107">You have assigned users toohello application</span></span>

## <a name="application-not-found-in-directory"></a><span data-ttu-id="9107b-108">Programmet hittades inte i katalog</span><span class="sxs-lookup"><span data-stu-id="9107b-108">Application not found in directory</span></span>

<span data-ttu-id="9107b-109">*Fel AADSTS70001: Programmet med ID 'https://contoso.com' hittades inte i hello directory*.</span><span class="sxs-lookup"><span data-stu-id="9107b-109">*Error AADSTS70001: Application with Identifier ‘https://contoso.com’ was not found in hello directory*.</span></span>

<span data-ttu-id="9107b-110">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="9107b-110">**Possible cause**</span></span>

<span data-ttu-id="9107b-111">hello utfärdaren attribut skickar från hello programmet tooAzure AD i hello SAML-begäran inte matchar hello identifierare värdet som konfigurerats i programmet hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9107b-111">hello Issuer attribute sends from hello application tooAzure AD in hello SAML request doesn’t match hello Identifier value configured in hello application Azure AD.</span></span>

<span data-ttu-id="9107b-112">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="9107b-112">**Resolution**</span></span>

<span data-ttu-id="9107b-113">Se till att attributet hello utfärdaren i hello SAML-begäran matchar hello ID-värdet som konfigurerats i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="9107b-113">Ensure that hello Issuer attribute in hello SAML request it’s matching hello Identifier value configured in Azure AD:</span></span>

1.  <span data-ttu-id="9107b-114">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="9107b-114">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="9107b-115">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-115">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9107b-116">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="9107b-116">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9107b-117">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-117">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="9107b-118">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="9107b-118">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="9107b-119">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="9107b-119">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="9107b-120">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9107b-120">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="9107b-121">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-121">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="9107b-122"><span id="_Hlk477190042" class="anchor"></span>Gå för**domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9107b-122"><span id="_Hlk477190042" class="anchor"></span>Go too**Domain and URLs** section.</span></span> <span data-ttu-id="9107b-123">Kontrollera att hello-värdet i hello identifierare textruta matchar hello värdet för hello identifierarvärde visas i hello-fel.</span><span class="sxs-lookup"><span data-stu-id="9107b-123">Verify that hello value in hello Identifier textbox is matching hello value for hello identifier value displayed in hello error.</span></span>

<span data-ttu-id="9107b-124">När du har uppdaterat hello identifierarvärde i Azure AD och dess matchande hello värdet skickar av programmet hello i hello SAML-begäran, bör du kunna toosign i toohello program.</span><span class="sxs-lookup"><span data-stu-id="9107b-124">After you have updated hello Identifier value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a><span data-ttu-id="9107b-125">hello svarsadressen matchar inte hello svar adresser som har konfigurerats för hello program.</span><span class="sxs-lookup"><span data-stu-id="9107b-125">hello reply address does not match hello reply addresses configured for hello application.</span></span> 

<span data-ttu-id="9107b-126">*Fel AADSTS50011: hello svarsadressen 'https://contoso.com' matchar inte hello svar adresser som har konfigurerats för hello program*</span><span class="sxs-lookup"><span data-stu-id="9107b-126">*Error AADSTS50011: hello reply address ‘https://contoso.com’ does not match hello reply addresses configured for hello application*</span></span> 

<span data-ttu-id="9107b-127">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="9107b-127">**Possible cause**</span></span> 

<span data-ttu-id="9107b-128">Hej AssertionConsumerServiceURL värdet i hello SAML-begäran matchar inte hello Reply URL värde eller mönster som konfigurerats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9107b-128">hello AssertionConsumerServiceURL value in hello SAML request doesn't match hello Reply URL value or pattern configured in Azure AD.</span></span> <span data-ttu-id="9107b-129">Hej AssertionConsumerServiceURL värdet i hello SAML-begäran är hello URL som visas i hello-fel.</span><span class="sxs-lookup"><span data-stu-id="9107b-129">hello AssertionConsumerServiceURL value in hello SAML request is hello URL you see in hello error.</span></span> 

<span data-ttu-id="9107b-130">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="9107b-130">**Resolution**</span></span> 

<span data-ttu-id="9107b-131">Se till att hello AssertionConsumerServiceURL värde i dess matchande hello Reply URL-värdet som konfigurerats i Azure AD hello SAML-begäran.</span><span class="sxs-lookup"><span data-stu-id="9107b-131">Ensure that hello AssertionConsumerServiceURL value in hello SAML request it's matching hello Reply URL value configured in Azure AD.</span></span> 
 
1.  <span data-ttu-id="9107b-132">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="9107b-132">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span> 

2.  <span data-ttu-id="9107b-133">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-133">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span> 

3.  <span data-ttu-id="9107b-134">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="9107b-134">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span> 

4.  <span data-ttu-id="9107b-135">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-135">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span> 

5.  <span data-ttu-id="9107b-136">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="9107b-136">click **All Applications** tooview a list of all your applications.</span></span> 

  * <span data-ttu-id="9107b-137">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="9107b-137">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and       set hello **Show** option too**All Applications.**</span></span>
  
6.  <span data-ttu-id="9107b-138">Välj hello-program som du vill tooconfigure enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9107b-138">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="9107b-139">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-139">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="9107b-140">Gå för**domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9107b-140">Go too**Domain and URLs** section.</span></span> <span data-ttu-id="9107b-141">Kontrollera eller uppdatera hello värdet i hello Reply URL textruta toomatch hello AssertionConsumerServiceURL värdet i hello SAML-begäran.</span><span class="sxs-lookup"><span data-stu-id="9107b-141">Verify or update hello value in hello Reply URL textbox toomatch hello AssertionConsumerServiceURL value in hello SAML request.</span></span>

  * <span data-ttu-id="9107b-142">Om du inte ser hello Reply URL-textrutan, Välj hello **visa avancerade inställningar för URL: en** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="9107b-142">If you don't see hello Reply URL textbox, select hello **Show advanced URL settings** checkbox.</span></span> 

<span data-ttu-id="9107b-143">När du har uppdaterat hello Reply URL-värdet i Azure AD och den har matchar hello värde skickar av hello program i hello SAML-begäran, bör du kunna toosign i toohello program.</span><span class="sxs-lookup"><span data-stu-id="9107b-143">After you have updated hello Reply URL value in Azure AD and it’s matching hello value sends by hello application in hello SAML request, you should be able toosign in toohello application.</span></span>

## <a name="user-not-assigned-a-role"></a><span data-ttu-id="9107b-144">Användare som inte har tilldelats en roll</span><span class="sxs-lookup"><span data-stu-id="9107b-144">User not assigned a role</span></span>

<span data-ttu-id="9107b-145">*Fel AADSTS50105: hello inloggad användaren 'brian@contoso.com' är inte tilldelad tooa roll för hello program*</span><span class="sxs-lookup"><span data-stu-id="9107b-145">*Error AADSTS50105: hello signed in user 'brian@contoso.com' is not assigned tooa role for hello application*</span></span>

<span data-ttu-id="9107b-146">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="9107b-146">**Possible cause**</span></span>

<span data-ttu-id="9107b-147">hello användaren har inte beviljats åtkomst toohello program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9107b-147">hello user has not been granted access toohello application in Azure AD.</span></span>

<span data-ttu-id="9107b-148">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="9107b-148">**Resolution**</span></span>

<span data-ttu-id="9107b-149">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="9107b-149">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="9107b-150">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="9107b-150">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="9107b-151">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-151">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9107b-152">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="9107b-152">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9107b-153">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-153">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="9107b-154">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="9107b-154">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="9107b-155">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="9107b-155">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="9107b-156">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9107b-156">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="9107b-157">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-157">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="9107b-158">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="9107b-158">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="9107b-159">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="9107b-159">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="9107b-160">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="9107b-160">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="9107b-161">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="9107b-161">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="9107b-162">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="9107b-162">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="9107b-163">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="9107b-163">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="9107b-164">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="9107b-164">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="9107b-165">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="9107b-165">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="9107b-166">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="9107b-166">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="9107b-167">Efter en kort tidsperiod att hello användare som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning.</span><span class="sxs-lookup"><span data-stu-id="9107b-167">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="not-a-valid-saml-request"></a><span data-ttu-id="9107b-168">Inte en giltig SAML-begäran</span><span class="sxs-lookup"><span data-stu-id="9107b-168">Not a valid SAML Request</span></span>

<span data-ttu-id="9107b-169">*Fel AADSTS75005: hello-begäran är inte ett giltigt Saml2-protokollmeddelande.*</span><span class="sxs-lookup"><span data-stu-id="9107b-169">*Error AADSTS75005: hello request is not a valid Saml2 protocol message.*</span></span>

<span data-ttu-id="9107b-170">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="9107b-170">**Possible cause**</span></span>

<span data-ttu-id="9107b-171">Azure AD stöder inte hello SAML-begäran skickas av programmet hello för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9107b-171">Azure AD doesn’t support hello SAML Request sent by hello application for Single Sign-on.</span></span> <span data-ttu-id="9107b-172">Några vanliga problem är:</span><span class="sxs-lookup"><span data-stu-id="9107b-172">Some common issues are:</span></span>

-   <span data-ttu-id="9107b-173">Obligatoriska fält i hello SAML-begäran som saknas</span><span class="sxs-lookup"><span data-stu-id="9107b-173">Missing required fields in hello SAML request</span></span>

-   <span data-ttu-id="9107b-174">SAML-kodade metoden</span><span class="sxs-lookup"><span data-stu-id="9107b-174">SAML request encoded method</span></span>

<span data-ttu-id="9107b-175">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="9107b-175">**Resolution**</span></span>

1.  <span data-ttu-id="9107b-176">Avbilda SAML-begäran.</span><span class="sxs-lookup"><span data-stu-id="9107b-176">Capture SAML request.</span></span> <span data-ttu-id="9107b-177">Följ hello kursen på [hur toodebug SAML-baserade enkel inloggning tooapplications i Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn hur toocapture hello SAML begäran.</span><span class="sxs-lookup"><span data-stu-id="9107b-177">follow hello tutorial on [how toodebug SAML-based single sign-on tooapplications in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn how toocapture hello SAML request.</span></span>

2.  <span data-ttu-id="9107b-178">Kontakta programvaruleverantören för hello och resursen:</span><span class="sxs-lookup"><span data-stu-id="9107b-178">Contact hello application vendor and share:</span></span>

    -   <span data-ttu-id="9107b-179">SAML-begäran</span><span class="sxs-lookup"><span data-stu-id="9107b-179">SAML request</span></span>

    -   [<span data-ttu-id="9107b-180">Krav för Azure AD enkel inloggning SAML-protokoll</span><span class="sxs-lookup"><span data-stu-id="9107b-180">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

<span data-ttu-id="9107b-181">De bör verifiera de stöder hello Azure AD SAML-implementering för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9107b-181">They should validate they support hello Azure AD SAML implementation for Single Sign-on.</span></span>

## <a name="no-resource-in-requiredresourceaccess-list"></a><span data-ttu-id="9107b-182">Ingen resurs i requiredResourceAccess lista</span><span class="sxs-lookup"><span data-stu-id="9107b-182">No resource in requiredResourceAccess list</span></span>

<span data-ttu-id="9107b-183">*Fel AADSTS65005: hello klientprogrammet har begärt åtkomst tooresource ' 00000002-0000-0000-c000-000000000000'. Begäran misslyckades på grund av hello-klienten inte har angett den här resursen i listan över requiredResourceAccess*.</span><span class="sxs-lookup"><span data-stu-id="9107b-183">*Error AADSTS65005: hello client application has requested access tooresource '00000002-0000-0000-c000-000000000000'. This request has failed because hello client has not specified this resource in its requiredResourceAccess list*.</span></span>

<span data-ttu-id="9107b-184">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="9107b-184">**Possible cause**</span></span>

<span data-ttu-id="9107b-185">hello programobjektet är skadad.</span><span class="sxs-lookup"><span data-stu-id="9107b-185">hello application object is corrupted.</span></span>

<span data-ttu-id="9107b-186">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="9107b-186">**Resolution**</span></span>

<span data-ttu-id="9107b-187">toosolve hello problem, ta bort hello programmet från hello directory.</span><span class="sxs-lookup"><span data-stu-id="9107b-187">toosolve hello problem, remove hello application from hello directory.</span></span> <span data-ttu-id="9107b-188">Sedan kan lägga till och konfigurera om programmet hello, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9107b-188">Then, add and reconfigure hello application, follow hello steps below:</span></span>

1.  <span data-ttu-id="9107b-189">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="9107b-189">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="9107b-190">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-190">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9107b-191">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="9107b-191">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9107b-192">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-192">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="9107b-193">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="9107b-193">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="9107b-194">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="9107b-194">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="9107b-195">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9107b-195">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="9107b-196">Klicka på **ta bort** på hello övre vänstra av programmet hello **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="9107b-196">Click **Delete** at hello top-left of hello application **Overview** blade.</span></span>

8.  <span data-ttu-id="9107b-197">Uppdatera Azure AD och lägga till hello program från hello Azure AD-galleriet.</span><span class="sxs-lookup"><span data-stu-id="9107b-197">Refresh Azure AD and Add hello application from hello Azure AD gallery.</span></span> <span data-ttu-id="9107b-198">Konfigurera sedan hello programmet igen.</span><span class="sxs-lookup"><span data-stu-id="9107b-198">Then, Configure hello application again.</span></span>

<span data-ttu-id="9107b-199">När du konfigurerar om programmet hello ska kunna toosign i toohello program.</span><span class="sxs-lookup"><span data-stu-id="9107b-199">After reconfiguring hello application, you should be able toosign in toohello application.</span></span>

## <a name="certificate-or-key-not-configured"></a><span data-ttu-id="9107b-200">Certifikatet eller nyckeln som inte har konfigurerats</span><span class="sxs-lookup"><span data-stu-id="9107b-200">Certificate or key not configured</span></span>

<span data-ttu-id="9107b-201">Fel AADSTS50003: Inga signeringsnyckeln konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="9107b-201">Error AADSTS50003: No signing key configured.</span></span>

<span data-ttu-id="9107b-202">**Möjlig orsak**</span><span class="sxs-lookup"><span data-stu-id="9107b-202">**Possible cause**</span></span>

<span data-ttu-id="9107b-203">hello programobjektet är skadad och kan identifiera inte hello-certifikatet som konfigurerats för programmet hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9107b-203">hello application object is corrupted and Azure AD doesn’t recognize hello certificate configured for hello application.</span></span>

<span data-ttu-id="9107b-204">**Lösning**</span><span class="sxs-lookup"><span data-stu-id="9107b-204">**Resolution**</span></span>

<span data-ttu-id="9107b-205">toodelete och skapa ett nytt certifikat, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9107b-205">toodelete and create a new certificate, follow hello steps below:</span></span>

1.  <span data-ttu-id="9107b-206">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="9107b-206">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="9107b-207">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-207">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="9107b-208">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="9107b-208">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="9107b-209">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-209">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="9107b-210">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="9107b-210">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="9107b-211">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="9107b-211">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="9107b-212">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9107b-212">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="9107b-213">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="9107b-213">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="9107b-214">Klicka på **Skapa nytt certifikat** under hello **SAML signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9107b-214">click **Create new certificate** under hello **SAML signing Certificate** section.</span></span>

9.  <span data-ttu-id="9107b-215">Välj upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="9107b-215">Select Expiration date.</span></span> <span data-ttu-id="9107b-216">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="9107b-216">Then, click **Save.**</span></span>

10. <span data-ttu-id="9107b-217">Kontrollera **aktivera nya certifikatet** toooverride hello aktiva certifikatet.</span><span class="sxs-lookup"><span data-stu-id="9107b-217">Check **Make new certificate active** toooverride hello active certificate.</span></span> <span data-ttu-id="9107b-218">Klicka på **spara** hello överst på bladet hello och acceptera tooactivate hello förnyelsecertifikat.</span><span class="sxs-lookup"><span data-stu-id="9107b-218">Then, click **Save** at hello top of hello blade and accept tooactivate hello rollover certificate.</span></span>

11. <span data-ttu-id="9107b-219">Under hello **SAML-signeringscertifikat** klickar du på **ta bort** tooremove hello **används inte** certifikat.</span><span class="sxs-lookup"><span data-stu-id="9107b-219">Under hello **SAML Signing Certificate** section, click **remove** tooremove hello **Unused** certificate.</span></span>

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="9107b-220">Problem när du anpassar hello SAML anspråk skickas tooan program</span><span class="sxs-lookup"><span data-stu-id="9107b-220">Problem when customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="9107b-221">toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9107b-221">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9107b-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9107b-222">Next steps</span></span>
[<span data-ttu-id="9107b-223">Krav för Azure AD enkel inloggning SAML-protokoll</span><span class="sxs-lookup"><span data-stu-id="9107b-223">Azure AD Single Sign-on SAML protocol requirements</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)
