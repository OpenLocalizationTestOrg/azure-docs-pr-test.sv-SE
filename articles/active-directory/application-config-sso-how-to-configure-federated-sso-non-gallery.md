---
title: "aaaHow tooconfigure federerad enkel inloggning för ett icke-galleriet program | Microsoft Docs"
description: "Hur tooconfigure federerad enkel inloggning för ett anpassat program för icke-galleriet som du vill toointegrate med Azure AD"
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
ms.openlocfilehash: f4a37cb500c075d0ce917ad8f13411f2ce208c56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="bc69d-103">Hur tooconfigure federerad enkel inloggning för ett program för icke-galleriet</span><span class="sxs-lookup"><span data-stu-id="bc69d-103">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="bc69d-104">tooconfigure ett icke-galleriet program behöver du toohave Azure AD premium och hello programmet stöder SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="bc69d-104">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="bc69d-105">Mer information om Azure AD-versioner finns [priser för Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="bc69d-105">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="bc69d-106">Översikt över steg som krävs</span><span class="sxs-lookup"><span data-stu-id="bc69d-106">Overview of steps required</span></span>
<span data-ttu-id="bc69d-107">Nedan finns en hög nivå översikt över hello steg krävs tooconfigure federerad enkel inloggning för ett galleri (t.ex. anpassade) program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-107">Below is a high level overview of hello steps required tooconfigure federated single sign-on for a non-gallery (e.g., custom) application.</span></span>

-   [<span data-ttu-id="bc69d-108">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="bc69d-108">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="bc69d-109">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="bc69d-109">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="bc69d-110">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="bc69d-110">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="bc69d-111">Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="bc69d-111">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#_Configuring_single_sign-on)

-   [<span data-ttu-id="bc69d-112">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="bc69d-112">Assign users toohello application</span></span>](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-toonon-gallery-applications"></a><span data-ttu-id="bc69d-113">Konfigurera enkel inloggning toonon-galleriet program</span><span class="sxs-lookup"><span data-stu-id="bc69d-113">Configuring single sign-on toonon-gallery applications</span></span>

<span data-ttu-id="bc69d-114">tooconfigure enkel inloggning för ett program som inte är i hello Azure AD-galleriet gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="bc69d-114">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="bc69d-115">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-115">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bc69d-116">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-116">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc69d-117">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="bc69d-117">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc69d-118">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-118">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc69d-119">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="bc69d-119">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="bc69d-120">Klicka på **icke-galleriet programmet** i hello **lägga till egna app** avsnitt</span><span class="sxs-lookup"><span data-stu-id="bc69d-120">click **Non-gallery application** in hello **Add your own app** section</span></span>

7.  <span data-ttu-id="bc69d-121">Ange hello namnet på programmet hello i hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="bc69d-121">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="bc69d-122">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-122">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="bc69d-123">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-123">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="bc69d-124">Välj **SAML-baserade inloggning** i hello **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-124">Select **SAML-based Sign-on** in hello **Mode** dropdown.</span></span>

11. <span data-ttu-id="bc69d-125">Ange hello krävs värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-125">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="bc69d-126">Du bör få värdena från hello programvaruleverantören.</span><span class="sxs-lookup"><span data-stu-id="bc69d-126">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="bc69d-127">tooconfigure hello program som IdP-initierad SSO ange hello Reply URL och hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="bc69d-127">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

   2. <span data-ttu-id="bc69d-128">**Valfritt:** tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="bc69d-128">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="bc69d-129">I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-129">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="bc69d-130">**Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="bc69d-130">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="bc69d-131">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="bc69d-131">tooadd an attribute:</span></span>

   1. <span data-ttu-id="bc69d-132">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="bc69d-132">click **Add attribute**.</span></span> <span data-ttu-id="bc69d-133">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-133">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="bc69d-134">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-134">Click **Save.**</span></span> <span data-ttu-id="bc69d-135">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="bc69d-135">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="bc69d-136">Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-136">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="bc69d-137">Du har även Azure AD-URL: er och certifikat som krävs för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="bc69d-137">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

15. [<span data-ttu-id="bc69d-138">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-138">Assign users toohello application.</span></span>](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="bc69d-139">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="bc69d-139">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="bc69d-140">tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bc69d-140">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="bc69d-141">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-141">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bc69d-142">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-142">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc69d-143">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="bc69d-143">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc69d-144">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-144">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc69d-145">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-145">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bc69d-146">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-146">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bc69d-147">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bc69d-147">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="bc69d-148">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-148">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bc69d-149">Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-149">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="bc69d-150">hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="bc69d-150">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

 ><span data-ttu-id="bc69d-151">[! Observera} Azure AD Välj hello format för attributet NameID hello (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="bc69d-151">[!NOTE} Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="bc69d-152">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="bc69d-152">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
 >
 >

9.  <span data-ttu-id="bc69d-153">tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="bc69d-153">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="bc69d-154">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="bc69d-154">tooadd an attribute:</span></span>

   1. <span data-ttu-id="bc69d-155">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="bc69d-155">click **Add attribute**.</span></span> <span data-ttu-id="bc69d-156">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-156">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="bc69d-157">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-157">Click **Save.**</span></span> <span data-ttu-id="bc69d-158">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="bc69d-158">You see hello new attribute in hello table.</span></span>

## <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="bc69d-159">Hämta metadata för hello Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="bc69d-159">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="bc69d-160">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="bc69d-160">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="bc69d-161">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-161">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="bc69d-162">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-162">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc69d-163">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="bc69d-163">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc69d-164">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-164">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc69d-165">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-165">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="bc69d-166">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-166">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bc69d-167">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bc69d-167">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="bc69d-168">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-168">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bc69d-169">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="bc69d-169">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="bc69d-170">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="bc69d-170">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="bc69d-171">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="bc69d-171">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="bc69d-172">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="bc69d-172">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-toohello-application"></a><span data-ttu-id="bc69d-173">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="bc69d-173">Assign users toohello application</span></span>

<span data-ttu-id="bc69d-174">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="bc69d-174">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="bc69d-175">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-175">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="bc69d-176">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-176">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="bc69d-177">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="bc69d-177">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="bc69d-178">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-178">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="bc69d-179">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-179">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="bc69d-180">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="bc69d-180">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="bc69d-181">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-181">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="bc69d-182">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="bc69d-182">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="bc69d-183">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="bc69d-183">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="bc69d-184">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="bc69d-184">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="bc69d-185">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="bc69d-185">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="bc69d-186">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="bc69d-186">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="bc69d-187">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="bc69d-187">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="bc69d-188">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="bc69d-188">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="bc69d-189">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="bc69d-189">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="bc69d-190">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="bc69d-190">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="bc69d-191">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="bc69d-191">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="bc69d-192">Efter en kort tidsperiod att hello användare som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning.</span><span class="sxs-lookup"><span data-stu-id="bc69d-192">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="bc69d-193">Anpassa hello SAML anspråk skickas tooan program</span><span class="sxs-lookup"><span data-stu-id="bc69d-193">Customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="bc69d-194">toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="bc69d-194">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc69d-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc69d-195">Next steps</span></span>
[<span data-ttu-id="bc69d-196">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="bc69d-196">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
