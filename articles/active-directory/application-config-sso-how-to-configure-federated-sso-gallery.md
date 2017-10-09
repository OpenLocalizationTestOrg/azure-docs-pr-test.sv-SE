---
title: "aaaHow tooconfigure federerad enkel inloggning för ett program för Azure AD-galleriet | Microsoft Docs"
description: "Hur tooconfigure federerad enkel inloggning för en befintlig Azure AD-galleriet program och Använd självstudier tooget igång snabbare"
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
ms.openlocfilehash: a93de021fddd253e4fe663c221b822d12625fd54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="4a099-103">Hur tooconfigure federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="4a099-103">How tooconfigure federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="4a099-104">Alla program i hello Azure AD-galleriet aktiverad med Enterprise enkel inloggning kapaciteten har en stegvis självstudiekurs som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="4a099-104">All applications in hello Azure AD gallery enabled with Enterprise single sign-on capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="4a099-105">Du kan komma åt hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) detaljerade stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="4a099-105">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for detailed step-by-step guidance.</span></span>

## <a name="overview-of-steps-required"></a><span data-ttu-id="4a099-106">Översikt över steg som krävs</span><span class="sxs-lookup"><span data-stu-id="4a099-106">Overview of steps required</span></span>
<span data-ttu-id="4a099-107">tooconfigure ett program från hello Azure AD-galleriet måste du:</span><span class="sxs-lookup"><span data-stu-id="4a099-107">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="4a099-108">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="4a099-108">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="4a099-109">Konfigurera hello programmets metadatavärden i Azure AD (logga in på URL: en identifierare, Reply URL)</span><span class="sxs-lookup"><span data-stu-id="4a099-109">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="4a099-110">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="4a099-110">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="4a099-111">Hämta Azure AD-metadata och certifikat</span><span class="sxs-lookup"><span data-stu-id="4a099-111">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="4a099-112">Konfigurera Azure AD metadatavärden i programmet hello (logga in på URL: en, utfärdare, logga ut URL-Adressen och certifikatet)</span><span class="sxs-lookup"><span data-stu-id="4a099-112">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="4a099-113">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="4a099-113">Assign users toohello application</span></span>](#assign-users-to-the-application)

## <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="4a099-114">Lägga till ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="4a099-114">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="4a099-115">tooadd ett program från hello Azure AD-galleriet så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="4a099-115">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="4a099-116">Öppna hello [Azure Portal](https://portal.azure.com) och logga in som en **Global administratör** eller **medadministratör**</span><span class="sxs-lookup"><span data-stu-id="4a099-116">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="4a099-117">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-117">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4a099-118">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4a099-118">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4a099-119">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-119">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4a099-120">Klicka på hello **Lägg till** längst hello övre högra hörnet på hello **företagsprogram** bladet.</span><span class="sxs-lookup"><span data-stu-id="4a099-120">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="4a099-121">I hello **anger du ett namn** textruta från hello **Lägg till från galleriet hello** avsnitt, hello-typnamn för hello program.</span><span class="sxs-lookup"><span data-stu-id="4a099-121">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="4a099-122">Välj hello-program som du vill använda tooconfigure för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a099-122">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="4a099-123">Innan du lägger till programmet hello kan du ändra dess namn från hello **namn** textruta.</span><span class="sxs-lookup"><span data-stu-id="4a099-123">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="4a099-124">Klicka på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4a099-124">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="4a099-125">Efter en kort tidsperiod anges kan toosee hello programmets konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="4a099-125">After a short period of time, you be able toosee hello application’s configuration blade.</span></span>

## <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="4a099-126">Konfigurera enkel inloggning för ett program från hello Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="4a099-126">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="4a099-127">tooconfigure enkel inloggning för ett program gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="4a099-127">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="4a099-128">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **medadministratör**.</span><span class="sxs-lookup"><span data-stu-id="4a099-128">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="4a099-129">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-129">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4a099-130">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4a099-130">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4a099-131">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-131">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4a099-132">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4a099-132">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="4a099-133">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="4a099-133">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="4a099-134">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a099-134">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="4a099-135">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-135">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4a099-136">Välj **SAML-baserade inloggning** från hello **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="4a099-136">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="4a099-137">Ange hello krävs värden i **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="4a099-137">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="4a099-138">Du bör få värdena från hello programvaruleverantören.</span><span class="sxs-lookup"><span data-stu-id="4a099-138">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="4a099-139">tooconfigure hello program som SP-initierad SSO hello logga på URL: en är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="4a099-139">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="4a099-140">Hello identifierare är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="4a099-140">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="4a099-141">tooconfigure hello program som IdP-initierad SSO hello Reply-URL som det är ett obligatoriskt värde.</span><span class="sxs-lookup"><span data-stu-id="4a099-141">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="4a099-142">Hello identifierare är också ett obligatoriskt värde för vissa program.</span><span class="sxs-lookup"><span data-stu-id="4a099-142">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="4a099-143">**Valfritt:** klickar du på **visa avancerade inställningar för URL: en** om du vill toosee hello icke obligatoriska värden.</span><span class="sxs-lookup"><span data-stu-id="4a099-143">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="4a099-144">I hello **användarattribut**, Välj hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="4a099-144">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="4a099-145">**Valfritt:** klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="4a099-145">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

  <span data-ttu-id="4a099-146">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="4a099-146">tooadd an attribute:</span></span>
   
   1. <span data-ttu-id="4a099-147">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="4a099-147">click **Add attribute**.</span></span> <span data-ttu-id="4a099-148">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="4a099-148">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   1. <span data-ttu-id="4a099-149">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="4a099-149">Click **Save.**</span></span> <span data-ttu-id="4a099-150">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="4a099-150">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="4a099-151">Klicka på **konfigurera &lt;programnamn&gt;**  tooaccess-dokumentationen om tooconfigure enkel inloggning i hello program.</span><span class="sxs-lookup"><span data-stu-id="4a099-151">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="4a099-152">Du har dessutom hello metadata URL: er och certifikat som krävs för toosetup SSO med hello program.</span><span class="sxs-lookup"><span data-stu-id="4a099-152">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="4a099-153">Klicka på **spara** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4a099-153">Click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="4a099-154">Tilldela användare toohello program.</span><span class="sxs-lookup"><span data-stu-id="4a099-154">Assign users toohello application.</span></span>

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="4a099-155">Välj användar-ID och Lägg till attribut toobe skickas toohello-användarprogram</span><span class="sxs-lookup"><span data-stu-id="4a099-155">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="4a099-156">tooselect hello användar-ID och lägga till användarattribut, hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4a099-156">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="4a099-157">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4a099-157">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4a099-158">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4a099-159">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4a099-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4a099-160">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4a099-161">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4a099-161">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="4a099-162">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="4a099-162">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="4a099-163">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a099-163">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="4a099-164">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-164">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4a099-165">Under hello **användarattribut** väljer hello Unik identifierare för användarna i hello **användar-ID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="4a099-165">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="4a099-166">hello måste markerade alternativet toomatch hello förväntat värde i hello programanvändare tooauthenticate hello.</span><span class="sxs-lookup"><span data-stu-id="4a099-166">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

  >[!NOTE] 
  ><span data-ttu-id="4a099-167">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="4a099-167">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="4a099-168">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="4a099-168">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
  >
  >

9.  <span data-ttu-id="4a099-169">tooadd användarattribut klickar du på **visa och redigera andra användarattribut** tooedit hello attribut toobe skickas toohello programmet hello SAML-token när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="4a099-169">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="4a099-170">tooadd ett attribut:</span><span class="sxs-lookup"><span data-stu-id="4a099-170">tooadd an attribute:</span></span>
  
   1. <span data-ttu-id="4a099-171">Klicka på **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="4a099-171">click **Add attribute**.</span></span> <span data-ttu-id="4a099-172">Ange hello **namn** och hello väljer hello **värdet** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="4a099-172">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="4a099-173">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4a099-173">Click **Save**.</span></span> <span data-ttu-id="4a099-174">Du kan se hello nytt attribut i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="4a099-174">You see hello new attribute in hello table.</span></span>

## <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="4a099-175">Hämta metadata för hello Azure AD eller certifikat</span><span class="sxs-lookup"><span data-stu-id="4a099-175">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="4a099-176">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="4a099-176">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="4a099-177">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="4a099-177">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="4a099-178">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-178">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4a099-179">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4a099-179">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4a099-180">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-180">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4a099-181">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4a099-181">click **All Applications** tooview a list of all your applications.</span></span>

  *  <span data-ttu-id="4a099-182">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program**.</span><span class="sxs-lookup"><span data-stu-id="4a099-182">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications**.</span></span>

6.  <span data-ttu-id="4a099-183">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a099-183">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="4a099-184">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-184">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4a099-185">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="4a099-185">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="4a099-186">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="4a099-186">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="4a099-187">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="4a099-187">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="4a099-188">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="4a099-188">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="assign-users-toohello-application"></a><span data-ttu-id="4a099-189">Tilldela användare toohello program</span><span class="sxs-lookup"><span data-stu-id="4a099-189">Assign users toohello application</span></span>

<span data-ttu-id="4a099-190">tooassign en eller flera användare tooan programmet direkt, gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="4a099-190">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="4a099-191">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**</span><span class="sxs-lookup"><span data-stu-id="4a099-191">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="4a099-192">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-192">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="4a099-193">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="4a099-193">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="4a099-194">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-194">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="4a099-195">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="4a099-195">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="4a099-196">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="4a099-196">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="4a099-197">Välj hello-program som du vill tooassign en toofrom hello-användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4a099-197">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="4a099-198">När programmet hello läses in klickar du på **användare och grupper** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="4a099-198">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="4a099-199">Klicka på hello **Lägg till** knappen ovanpå hello **användare och grupper** lista tooopen hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="4a099-199">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="4a099-200">Klicka på hello **användare och grupper** selector från hello **Lägg uppdrag** bladet.</span><span class="sxs-lookup"><span data-stu-id="4a099-200">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="4a099-201">Typen i hello **fullständigt namn** eller **e-postadress** för hello-användare som du vill tilldela till hello **Sök efter namn eller e-postadress** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="4a099-201">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="4a099-202">Hovra över hello **användare** i hello listan tooreveal en **kryssrutan**.</span><span class="sxs-lookup"><span data-stu-id="4a099-202">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="4a099-203">Klicka på hello kryssrutan nästa toohello användarens profil foto eller logotypen tooadd användaren-toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="4a099-203">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="4a099-204">**Valfritt:** om du vill ha för**lägga till fler än en användare**, typ i en annan **fullständigt namn** eller **e-postadress** till hello **Sök efter namn eller e-postadress** sökrutan och klicka på hello kryssrutan tooadd den här användaren toohello **valda** lista.</span><span class="sxs-lookup"><span data-stu-id="4a099-204">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="4a099-205">När du har valt användare klickar du på hello **Välj** knappen tooadd dem toohello lista över användare och grupper toobe tilldelat toohello program.</span><span class="sxs-lookup"><span data-stu-id="4a099-205">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="4a099-206">**Valfritt:** klickar du på hello **Välj roll** Väljaren i hello **Lägg uppdrag** bladet tooselect en roll tooassign toohello användare som du har valt.</span><span class="sxs-lookup"><span data-stu-id="4a099-206">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="4a099-207">Klicka på hello **tilldela** knappen tooassign hello programmet toohello markerade användare.</span><span class="sxs-lookup"><span data-stu-id="4a099-207">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="4a099-208">Efter en kort tidsperiod att hello användare som du har valt kan toolaunch dessa program med hjälp av hello metoder som beskrivs under hello lösning beskrivning.</span><span class="sxs-lookup"><span data-stu-id="4a099-208">After a short period of time, hello users you have selected be able toolaunch these applications using hello methods described in hello solution description section.</span></span>

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a><span data-ttu-id="4a099-209">Anpassa hello SAML anspråk skickas tooan program</span><span class="sxs-lookup"><span data-stu-id="4a099-209">Customizing hello SAML claims sent tooan application</span></span>

<span data-ttu-id="4a099-210">toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4a099-210">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a099-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a099-211">Next steps</span></span>
[<span data-ttu-id="4a099-212">Tillhandahålla enkel inloggning tooyour appar med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="4a099-212">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)



