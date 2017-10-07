---
title: "Konfigurera federerad enkel inloggning för ett program för Azure AD-galleriet aaaProblem | Microsoft Docs"
description: "Vissa av hello vanliga problem som kan uppstå när du konfigurerar federerad enkel inloggning med SAML för program som listas i hello Azure AD Application Gallery"
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
ms.openlocfilehash: 2ae1e511bd49b19159e1ab83cf79a7db5403b9a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="fcc6a-103">Problem som konfigurerar federerad enkel inloggning för ett program för Azure AD-galleriet</span><span class="sxs-lookup"><span data-stu-id="fcc6a-103">Problem configuring federated single sign-on for an Azure AD Gallery application</span></span>

<span data-ttu-id="fcc6a-104">Om du stöter på problem när du konfigurerar ett program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-104">If you encounter a problem when configuring an application.</span></span> <span data-ttu-id="fcc6a-105">Kontrollera att du har följt alla steg i hello i hello självstudierna för hello program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-105">Verify you have followed all hello steps in hello tutorial for hello application.</span></span> <span data-ttu-id="fcc6a-106">I hello programmets konfiguration har infogad dokumentation om hur tooconfigure hello program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-106">In hello application’s configuration, you have inline documentation on how tooconfigure hello application.</span></span> <span data-ttu-id="fcc6a-107">Du kan också komma åt hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) en stegvisa instruktioner för detaljer.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-107">Also, you can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

## <a name="cant-add-another-instance-of-hello-application"></a><span data-ttu-id="fcc6a-108">Det går inte att lägga till en annan instans av programmet hello</span><span class="sxs-lookup"><span data-stu-id="fcc6a-108">Can’t add another instance of hello application</span></span>

<span data-ttu-id="fcc6a-109">tooadd en andra instans av ett program måste toobe kunna:</span><span class="sxs-lookup"><span data-stu-id="fcc6a-109">tooadd a second instance of an application, you need toobe able to:</span></span>

-   <span data-ttu-id="fcc6a-110">Konfigurera en unik identifierare för andra instans av hello.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-110">Configure a unique identifier for hello second instance.</span></span> <span data-ttu-id="fcc6a-111">Du kommer inte att kunna tooconfigure hello samma ID som används för hello första instansen.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-111">You won’t be able tooconfigure hello same identifier used for hello first instance.</span></span>

-   <span data-ttu-id="fcc6a-112">Konfigurera ett annat certifikat än hello som används för hello första instansen.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-112">Configure a different certificate than hello one used for hello first instance.</span></span>

<span data-ttu-id="fcc6a-113">Om hello stöder programmet inte någon av hello ovan.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-113">If hello application doesn’t support any of hello above.</span></span> <span data-ttu-id="fcc6a-114">Du kan sedan inte kan tooconfigure en andra instans.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-114">Then, you won’t be able tooconfigure a second instance.</span></span>

## <a name="cant-add-hello-identifier-or-hello-reply-url"></a><span data-ttu-id="fcc6a-115">Det går inte att lägga till hello identifierare eller hello Reply-URL</span><span class="sxs-lookup"><span data-stu-id="fcc6a-115">Can’t add hello Identifier or hello Reply URL</span></span>

<span data-ttu-id="fcc6a-116">Om du inte kan tooconfigure hello identifierare eller hello Reply URL, bekräfta hello identifierare och Reply URL-värdena matchar hello mönster förkonfigurerade för hello program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-116">If you’re not able tooconfigure hello Identifier or hello Reply URL, confirm hello Identifier and Reply URL values match hello patterns pre-configured for hello application.</span></span>

<span data-ttu-id="fcc6a-117">tooknow hello mönster förkonfigurerade för programmet hello:</span><span class="sxs-lookup"><span data-stu-id="fcc6a-117">tooknow hello patterns pre-configured for hello application:</span></span>

1.  <span data-ttu-id="fcc6a-118">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.** Gå toostep 7.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.** Go toostep 7.</span></span> <span data-ttu-id="fcc6a-119">Om du redan hello programmet configuration bladet på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-119">If you are already in hello application configuration blade on Azure AD.</span></span>

2.  <span data-ttu-id="fcc6a-120">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-120">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fcc6a-121">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-121">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fcc6a-122">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-122">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fcc6a-123">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-123">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="fcc6a-124">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="fcc6a-124">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="fcc6a-125">Välj hello-program som du vill tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-125">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="fcc6a-126">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-126">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="fcc6a-127">Välj **SAML-baserade inloggning** från hello **läge** listrutan.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-127">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="fcc6a-128">Gå toohello **identifierare** eller **Reply URL** textruta under hello **domän och URL: er.**</span><span class="sxs-lookup"><span data-stu-id="fcc6a-128">Go toohello **Identifier** or **Reply URL** textbox, under hello **Domain and URLs section.**</span></span>

10. <span data-ttu-id="fcc6a-129">Det finns tre sätt tooknow hello stöds mönster för programmet hello:</span><span class="sxs-lookup"><span data-stu-id="fcc6a-129">There are three ways tooknow hello supported patterns for hello application:</span></span>

   * <span data-ttu-id="fcc6a-130">I hello textruta hello stöds pattern(s) visas som platshållare *exempel:* <https://contoso.com>.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-130">In hello textbox, you see hello supported pattern(s) as a placeholder *Example:* <https://contoso.com>.</span></span>

   * <span data-ttu-id="fcc6a-131">Om mönstret hello inte stöds, visas ett rött utropstecken när du försöker tooenter hello värdet i textrutan hello.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-131">if hello pattern is not supported, you see a red exclamation mark when you try tooenter hello value in hello textbox.</span></span> <span data-ttu-id="fcc6a-132">Om du håller muspekaren över hello rött utropstecken visas hello stöds mönster.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-132">If you hover your mouse over hello red exclamation mark, you see hello supported patterns.</span></span>

   * <span data-ttu-id="fcc6a-133">Du kan också få information om hello stöds i hello självstudierna för hello program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-133">In hello tutorial for hello application, you can also get information about hello supported patterns.</span></span> <span data-ttu-id="fcc6a-134">Under hello **konfigurera Azure AD enkel inloggning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-134">Under hello **Configure Azure AD single sign-on** section.</span></span> <span data-ttu-id="fcc6a-135">Gå toohello steg för konfigurerade hello värden under hello **domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-135">Go toohello step for configured hello values under hello **Domain and URLs** section.</span></span>

<span data-ttu-id="fcc6a-136">Om hello värden stämmer inte överens med hello mönster förkonfigurerade på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-136">If hello values don’t match with hello patterns pre-configured on Azure AD.</span></span> <span data-ttu-id="fcc6a-137">Du kan:</span><span class="sxs-lookup"><span data-stu-id="fcc6a-137">You can:</span></span>

-   <span data-ttu-id="fcc6a-138">Arbeta med hello programmet leverantör tooget värden som matchar mönstret för hello förkonfigurerade på Azure AD</span><span class="sxs-lookup"><span data-stu-id="fcc6a-138">Work with hello application vendor tooget values that match hello pattern pre-configured on Azure AD</span></span>

-   <span data-ttu-id="fcc6a-139">Eller kontakta Azure AD-teamet på < aadapprequest@microsoft.com > eller lämna en kommentar i hello självstudiekursen toorequest hello uppdatering av hello stöds mönster för hello program</span><span class="sxs-lookup"><span data-stu-id="fcc6a-139">Or, you can contact Azure AD team at <aadapprequest@microsoft.com> or leave a comment in hello tutorial toorequest hello update of hello supported patterns for hello application</span></span>

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a><span data-ttu-id="fcc6a-140">Där anger hello ID för entiteterna (användar-ID)-formatet</span><span class="sxs-lookup"><span data-stu-id="fcc6a-140">Where do I set hello EntityID (User Identifier) format</span></span>

<span data-ttu-id="fcc6a-141">Du kommer inte att kunna tooselect hello ID för entiteterna (användar-ID) format att Azure AD skickar toohello programmet hello svar efter autentisering av användare.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-141">You won’t be able tooselect hello EntityID (User Identifier) format that Azure AD sends toohello application in hello response after user authentication.</span></span>

<span data-ttu-id="fcc6a-142">Azure AD väljer hello format för hello NameID attribut (användar-ID) baserat på valda hello värdet eller hello format som begärs av programmet hello i hello SAML AuthRequest.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-142">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="fcc6a-143">Mer information finns i artikeln hello [enkel inloggning SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello avsnittet NameIDPolicy,</span><span class="sxs-lookup"><span data-stu-id="fcc6a-143">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy,</span></span>

## <a name="cant-find-hello-azure-ad-metadata-toocomplete-hello-configuration-with-hello-application"></a><span data-ttu-id="fcc6a-144">Det går inte att hitta hello Azure AD metadata toocomplete hello konfigurationsfilen med hello program</span><span class="sxs-lookup"><span data-stu-id="fcc6a-144">Can’t find hello Azure AD metadata toocomplete hello configuration with hello application</span></span>

<span data-ttu-id="fcc6a-145">toodownload hello programmetadata eller certifikat från Azure AD gör hello nedan:</span><span class="sxs-lookup"><span data-stu-id="fcc6a-145">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="fcc6a-146">Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**</span><span class="sxs-lookup"><span data-stu-id="fcc6a-146">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="fcc6a-147">Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-147">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="fcc6a-148">Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-148">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="fcc6a-149">Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-149">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="fcc6a-150">Klicka på **alla program** tooview en lista över alla program.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-150">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="fcc6a-151">Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**</span><span class="sxs-lookup"><span data-stu-id="fcc6a-151">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="fcc6a-152">Välj hello-program som du har konfigurerat för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-152">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="fcc6a-153">När programmet hello läses in klickar du på hello **enkel inloggning** från hello programmet vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-153">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="fcc6a-154">Gå för**SAML-signeringscertifikat** avsnittet och klicka sedan på **hämta** värde i kolumnen.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-154">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="fcc6a-155">Beroende på vilka hello-programmet kräver Konfigurera enkel inloggning, kan du se antingen hello alternativet toodownload hello XML-Metadata eller hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-155">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

<span data-ttu-id="fcc6a-156">Azure AD Ange inte en URL tooget hello metadata.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-156">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="fcc6a-157">hello metadata kan endast hämtas som en XML-fil.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-157">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a><span data-ttu-id="fcc6a-158">Vet inte hur toocustomize SAML anspråk skickas tooan program</span><span class="sxs-lookup"><span data-stu-id="fcc6a-158">Don't know how toocustomize SAML claims sent tooan application</span></span>

<span data-ttu-id="fcc6a-159">toolearn hur toocustomize hello SAML attributet anspråk skickas tooyour program finns i [anspråk mappning i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) för mer information.</span><span class="sxs-lookup"><span data-stu-id="fcc6a-159">toolearn how toocustomize hello SAML attribute claims sent tooyour application, see [Claims mapping in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fcc6a-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fcc6a-160">Next steps</span></span>
[<span data-ttu-id="fcc6a-161">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fcc6a-161">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
