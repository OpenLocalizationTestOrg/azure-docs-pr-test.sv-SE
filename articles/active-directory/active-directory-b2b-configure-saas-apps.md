---
title: "aaaConfigure SaaS-appar för B2B-samarbete i Azure Active Directory | Microsoft Docs"
description: "Koden och PowerShell-exempel för Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c3f22f81567c04ac23ef2316c09de718ecb15d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a><span data-ttu-id="82639-103">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="82639-103">Configure SaaS apps for B2B collaboration</span></span>

<span data-ttu-id="82639-104">Azure Active Directory (AD Azure) B2B-samarbete fungerar med de flesta appar som integreras med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82639-104">Azure Active Directory (Azure AD) B2B collaboration works with most apps that integrate with Azure AD.</span></span> <span data-ttu-id="82639-105">I det här avsnittet går vi igenom instruktioner för att konfigurera vissa populära SaaS-appar för användning med Azure AD B2B.</span><span class="sxs-lookup"><span data-stu-id="82639-105">In this section, we walk through instructions for configuring some popular SaaS apps for use with Azure AD B2B.</span></span>

<span data-ttu-id="82639-106">Innan du tittar på app-specifik anvisningarna är här några tumregel:</span><span class="sxs-lookup"><span data-stu-id="82639-106">Before you look at app-specific instructions, here are some rules of thumb:</span></span>

* <span data-ttu-id="82639-107">För de flesta hello appar måste användarinställningar toohappen manuellt.</span><span class="sxs-lookup"><span data-stu-id="82639-107">For most of hello apps, user setup needs toohappen manually.</span></span> <span data-ttu-id="82639-108">Det vill säga skapas användare manuellt i hello-app.</span><span class="sxs-lookup"><span data-stu-id="82639-108">That is, users must be created manually in hello app as well.</span></span>

* <span data-ttu-id="82639-109">För appar som har stöd för automatisk installation, till exempel Dropbox, skapas separat inbjudningar från hello appar.</span><span class="sxs-lookup"><span data-stu-id="82639-109">For apps that support automatic setup, such as Dropbox, separate invitations are created from hello apps.</span></span> <span data-ttu-id="82639-110">Användare måste vara säker på att tooaccept varje inbjudan.</span><span class="sxs-lookup"><span data-stu-id="82639-110">Users must be sure tooaccept each invitation.</span></span>

* <span data-ttu-id="82639-111">Ange alltid i hello användarattribut, toomitigate eventuella problem med felaktig användarprofil-disk (UPD) i gästanvändare, **användar-ID** för**user.mail**.</span><span class="sxs-lookup"><span data-stu-id="82639-111">In hello user attributes, toomitigate any issues with mangled user profile disk (UPD) in guest users, always set **User Identifier** too**user.mail**.</span></span>


## <a name="dropbox-business"></a><span data-ttu-id="82639-112">Dropbox företag</span><span class="sxs-lookup"><span data-stu-id="82639-112">Dropbox Business</span></span>

<span data-ttu-id="82639-113">tooenable användare toosign in med sina organisation, måste du manuellt konfigurera Dropbox Business toouse Azure AD som en identitetsleverantör Security Assertion Markup Language (SAML).</span><span class="sxs-lookup"><span data-stu-id="82639-113">tooenable users toosign in using their organization account, you must manually configure Dropbox Business toouse Azure AD as a Security Assertion Markup Language (SAML) identity provider.</span></span> <span data-ttu-id="82639-114">Fråga om Dropbox företag inte har konfigurerat toodo så kan inte eller annars tillåter användare toosign i med hjälp av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82639-114">If Dropbox Business has not been configured toodo so, it cannot prompt or otherwise allow users toosign in using Azure AD.</span></span>

1. <span data-ttu-id="82639-115">tooadd hello Dropbox Business appen till Azure AD, Välj **företagsprogram** i hello till vänster och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="82639-115">tooadd hello Dropbox Business app into Azure AD, select **Enterprise applications** in hello left pane, and then click **Add**.</span></span>

  ![Hej ”Lägg till”-knappen hello Enterprise program sida](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. <span data-ttu-id="82639-117">I hello **lägga till ett program** fönstret Ange **dropbox** i hello sökrutan och väljer sedan **Dropbox för företag** i hello resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="82639-117">In hello **Add an application** window, enter **dropbox** in hello search box, and then select **Dropbox for Business** in hello results list.</span></span>

  ![Sök efter ”dropbox” på hello Lägg till ett program på sidan](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. <span data-ttu-id="82639-119">På hello **enkel inloggning** väljer **enkel inloggning** i hello till vänster och sedan ange **user.mail** i hello **användar-ID** ruta.</span><span class="sxs-lookup"><span data-stu-id="82639-119">On hello **Single sign-on** page, select **Single sign-on** in hello left pane, and then enter **user.mail** in hello **User Identifier** box.</span></span> <span data-ttu-id="82639-120">(Det är angivet som UPN som standard.)</span><span class="sxs-lookup"><span data-stu-id="82639-120">(It's set as UPN by default.)</span></span>

  ![Konfigurera enkel inloggning för hello app](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. <span data-ttu-id="82639-122">toodownload hello certifikat toouse för Dropbox-konfiguration, markera **konfigurera DropBox**, och välj sedan **SAML logga på tjänst-URL för enkel** i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="82639-122">toodownload hello certificate toouse for Dropbox configuration, select **Configure DropBox**, and then select **SAML Single Sign On Service URL** in hello list.</span></span>

  ![Hämta hello certifikat för Dropbox-konfiguration](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. <span data-ttu-id="82639-124">Logga in tooDropbox med hello inloggning URL från hello **enkel inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="82639-124">Sign in tooDropbox with hello sign-on URL from hello **Single sign-on** page.</span></span>

  ![sidan hello Dropbox-inloggning](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. <span data-ttu-id="82639-126">Välj på menyn hello **administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="82639-126">On hello menu, select **Admin Console**.</span></span>

  ![Hej ”Admin Console” länk på hello Dropbox-menyn](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. <span data-ttu-id="82639-128">I hello **autentisering** dialogrutan **mer**, överför hello certifikat och sedan, i hello **logga in URL: en** anger Webbadressen för hello SAML enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="82639-128">In hello **Authentication** dialog box, select **More**, upload hello certificate and then, in hello **Sign in URL** box, enter hello SAML single sign-on URL.</span></span>

  ![Hej länken ”Mer” hello komprimerad dialogrutan för autentisering](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![Hej ”tecken i URL” i hello expanderas dialogrutan autentisering](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. <span data-ttu-id="82639-131">Välj tooconfigure automatisk användarinställningar i hello Azure-portalen **etablering** i hello vänster och välj **automatisk** i hello **etablering läge** och välj sedan **Auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="82639-131">tooconfigure automatic user setup in hello Azure portal, select **Provisioning** in hello left pane, select **Automatic** in hello **Provisioning Mode** box, and then select **Authorize**.</span></span>

  ![Konfigurera automatisk användaretablering i hello Azure-portalen](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

<span data-ttu-id="82639-133">När gästen eller medlem användare har ställts in i hello Dropbox app, kan de få en separat inbjudan från Dropbox.</span><span class="sxs-lookup"><span data-stu-id="82639-133">After guest or member users have been set up in hello Dropbox app, they receive a separate invitation from Dropbox.</span></span> <span data-ttu-id="82639-134">toouse Dropbox enkel inloggning inbjudna måste acceptera inbjudan hello genom att klicka på en länk i den.</span><span class="sxs-lookup"><span data-stu-id="82639-134">toouse Dropbox single sign-on, invitees must accept hello invitation by clicking a link in it.</span></span>

## <a name="box"></a><span data-ttu-id="82639-135">Box</span><span class="sxs-lookup"><span data-stu-id="82639-135">Box</span></span>
<span data-ttu-id="82639-136">Du kan aktivera användare tooauthenticate rutan gästanvändare med sina Azure AD-kontot med hjälp av federation som baseras på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="82639-136">You can enable users tooauthenticate Box guest users with their Azure AD account by using federation that's based on hello SAML protocol.</span></span> <span data-ttu-id="82639-137">I den här proceduren kan du överföra metadata tooBox.com.</span><span class="sxs-lookup"><span data-stu-id="82639-137">In this procedure, you upload metadata tooBox.com.</span></span>

1. <span data-ttu-id="82639-138">Lägg till hello Box-app från hello företagsappar.</span><span class="sxs-lookup"><span data-stu-id="82639-138">Add hello Box app from hello enterprise apps.</span></span>

2. <span data-ttu-id="82639-139">Konfigurera enkel inloggning i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="82639-139">Configure single sign-on in hello following order:</span></span>

  ![Konfigurera rutan enkel inloggning](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 <span data-ttu-id="82639-141">a.</span><span class="sxs-lookup"><span data-stu-id="82639-141">a.</span></span> <span data-ttu-id="82639-142">I hello **inloggning URL** kontrollerar du att hello inloggnings-URL är inställd på rätt sätt för rutan i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="82639-142">In hello **Sign on URL** box, ensure that hello sign-on URL is set appropriately for Box in hello Azure portal.</span></span> <span data-ttu-id="82639-143">Denna URL är hello URL för din Box.com-klient.</span><span class="sxs-lookup"><span data-stu-id="82639-143">This URL is hello URL of your Box.com tenant.</span></span> <span data-ttu-id="82639-144">Det bör följa hello namngivningskonvention *https://.box.com*.</span><span class="sxs-lookup"><span data-stu-id="82639-144">It should follow hello naming convention *https://.box.com*.</span></span>  
 <span data-ttu-id="82639-145">Hej **identifierare** gäller inte toothis appen, men som fortfarande visas som ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="82639-145">hello **Identifier** does not apply toothis app, but it still appears as a mandatory field.</span></span>

 <span data-ttu-id="82639-146">b.</span><span class="sxs-lookup"><span data-stu-id="82639-146">b.</span></span> <span data-ttu-id="82639-147">I hello **användar-ID** ange **user.mail** (för enkel inloggning för gästkonton).</span><span class="sxs-lookup"><span data-stu-id="82639-147">In hello **User identifier** box, enter **user.mail** (for SSO for guest accounts).</span></span>

 <span data-ttu-id="82639-148">c.</span><span class="sxs-lookup"><span data-stu-id="82639-148">c.</span></span> <span data-ttu-id="82639-149">Under **SAML-signeringscertifikat**, klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="82639-149">Under **SAML Signing Certificate**, click **Create new certificate**.</span></span>

 <span data-ttu-id="82639-150">d.</span><span class="sxs-lookup"><span data-stu-id="82639-150">d.</span></span> <span data-ttu-id="82639-151">Konfigurera din Box.com klient toouse Azure AD som en identitetsleverantör toobegin hämta hello metadatafil och spara sedan det tooyour lokal enhet.</span><span class="sxs-lookup"><span data-stu-id="82639-151">toobegin configuring your Box.com tenant toouse Azure AD as an identity provider, download hello metadata file and then save it tooyour local drive.</span></span>

 <span data-ttu-id="82639-152">e.</span><span class="sxs-lookup"><span data-stu-id="82639-152">e.</span></span> <span data-ttu-id="82639-153">Vidarebefordra hello metadata filen toohello rutan supportteamet, som konfigurerar enkel inloggning för dig.</span><span class="sxs-lookup"><span data-stu-id="82639-153">Forward hello metadata file toohello Box support team, which configures single sign-on for you.</span></span>

3. <span data-ttu-id="82639-154">Azure AD automatisk användarinställningar, i hello vänster och välj **etablering**, och välj sedan **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="82639-154">For Azure AD automatic user setup, in hello left pane, select **Provisioning**, and then select **Authorize**.</span></span>

  ![Verifiera Azure AD tooconnect tooBox](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

<span data-ttu-id="82639-156">Som Dropbox inbjudna lösa rutan inbjudna vill sina inbjudan från hello Box-app.</span><span class="sxs-lookup"><span data-stu-id="82639-156">Like Dropbox invitees, Box invitees must redeem their invitation from hello Box app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="82639-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82639-157">Next steps</span></span>

<span data-ttu-id="82639-158">Se följande artiklar om Azure AD B2B-samarbete hello:</span><span class="sxs-lookup"><span data-stu-id="82639-158">See hello following articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="82639-159">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="82639-159">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="82639-160">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="82639-160">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="82639-161">Lägga till en B2B-samarbete tooa användarroll</span><span class="sxs-lookup"><span data-stu-id="82639-161">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="82639-162">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="82639-162">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="82639-163">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="82639-163">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="82639-164">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="82639-164">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="82639-165">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="82639-165">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="82639-166">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="82639-166">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="82639-167">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="82639-167">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="82639-168">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="82639-168">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
