---
title: "Konfigurera SaaS-appar för B2B-samarbete i Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: 149a493f7b369415f0a2726dd6a576f0195c13d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a><span data-ttu-id="18836-103">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="18836-103">Configure SaaS apps for B2B collaboration</span></span>

<span data-ttu-id="18836-104">Azure Active Directory (AD Azure) B2B-samarbete fungerar med de flesta appar som integreras med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="18836-104">Azure Active Directory (Azure AD) B2B collaboration works with most apps that integrate with Azure AD.</span></span> <span data-ttu-id="18836-105">I det här avsnittet går vi igenom instruktioner för att konfigurera vissa populära SaaS-appar för användning med Azure AD B2B.</span><span class="sxs-lookup"><span data-stu-id="18836-105">In this section, we walk through instructions for configuring some popular SaaS apps for use with Azure AD B2B.</span></span>

<span data-ttu-id="18836-106">Innan du tittar på app-specifik anvisningarna är här några tumregel:</span><span class="sxs-lookup"><span data-stu-id="18836-106">Before you look at app-specific instructions, here are some rules of thumb:</span></span>

* <span data-ttu-id="18836-107">För de flesta appar måste användaren ske manuellt.</span><span class="sxs-lookup"><span data-stu-id="18836-107">For most of the apps, user setup needs to happen manually.</span></span> <span data-ttu-id="18836-108">Det vill säga skapas användare manuellt i appen även.</span><span class="sxs-lookup"><span data-stu-id="18836-108">That is, users must be created manually in the app as well.</span></span>

* <span data-ttu-id="18836-109">För appar som har stöd för automatisk installation, till exempel Dropbox, skapas separat inbjudningar från apparna.</span><span class="sxs-lookup"><span data-stu-id="18836-109">For apps that support automatic setup, such as Dropbox, separate invitations are created from the apps.</span></span> <span data-ttu-id="18836-110">Användare måste vara säker på att du accepterar varje inbjudan.</span><span class="sxs-lookup"><span data-stu-id="18836-110">Users must be sure to accept each invitation.</span></span>

* <span data-ttu-id="18836-111">I användarattributen för att åtgärda eventuella problem med felaktig användarprofil-disk (UPD) i gästanvändare, alltid ange **användar-ID** till **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="18836-111">In the user attributes, to mitigate any issues with mangled user profile disk (UPD) in guest users, always set **User Identifier** to **user.mail**.</span></span>


## <a name="dropbox-business"></a><span data-ttu-id="18836-112">Dropbox företag</span><span class="sxs-lookup"><span data-stu-id="18836-112">Dropbox Business</span></span>

<span data-ttu-id="18836-113">Om du vill att användarna ska logga in med sina organisation, måste du manuellt konfigurera Dropbox företag om du vill använda Azure AD som en identitetsleverantör Security Assertion Markup Language (SAML).</span><span class="sxs-lookup"><span data-stu-id="18836-113">To enable users to sign in using their organization account, you must manually configure Dropbox Business to use Azure AD as a Security Assertion Markup Language (SAML) identity provider.</span></span> <span data-ttu-id="18836-114">Om Dropbox företag inte har konfigurerats för att göra det, kan inte fråga eller annars tillåter användare att logga in med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="18836-114">If Dropbox Business has not been configured to do so, it cannot prompt or otherwise allow users to sign in using Azure AD.</span></span>

1. <span data-ttu-id="18836-115">Om du vill lägga till Dropbox Business-app till Azure AD, Välj **företagsprogram** i den vänstra rutan, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="18836-115">To add the Dropbox Business app into Azure AD, select **Enterprise applications** in the left pane, and then click **Add**.</span></span>

  ![Knappen ”Lägg till” på sidan Enterprise program](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. <span data-ttu-id="18836-117">I den **lägga till ett program** fönstret Ange **dropbox** i sökrutan och väljer sedan **Dropbox för företag** i resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="18836-117">In the **Add an application** window, enter **dropbox** in the search box, and then select **Dropbox for Business** in the results list.</span></span>

  ![Sök efter ”dropbox” på Lägg till en program-sida](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. <span data-ttu-id="18836-119">På den **enkel inloggning** väljer **enkel inloggning** i den vänstra rutan och ange sedan **user.mail** i den **användar-ID** rutan.</span><span class="sxs-lookup"><span data-stu-id="18836-119">On the **Single sign-on** page, select **Single sign-on** in the left pane, and then enter **user.mail** in the **User Identifier** box.</span></span> <span data-ttu-id="18836-120">(Det är angivet som UPN som standard.)</span><span class="sxs-lookup"><span data-stu-id="18836-120">(It's set as UPN by default.)</span></span>

  ![Konfigurera enkel inloggning för appen](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. <span data-ttu-id="18836-122">För att hämta certifikatet som ska användas för Dropbox-konfiguration, markera **konfigurera DropBox**, och välj sedan **SAML logga på tjänst-URL för enkel** i listan.</span><span class="sxs-lookup"><span data-stu-id="18836-122">To download the certificate to use for Dropbox configuration, select **Configure DropBox**, and then select **SAML Single Sign On Service URL** in the list.</span></span>

  ![Hämta certifikatet för Dropbox-konfiguration](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. <span data-ttu-id="18836-124">Logga in till Dropbox med den inloggnings-URL från den **enkel inloggning** sidan.</span><span class="sxs-lookup"><span data-stu-id="18836-124">Sign in to Dropbox with the sign-on URL from the **Single sign-on** page.</span></span>

  ![Sidan för Dropbox](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. <span data-ttu-id="18836-126">Välj på menyn **administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="18836-126">On the menu, select **Admin Console**.</span></span>

  ![Länken ”Admin Console” på Dropbox-menyn](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. <span data-ttu-id="18836-128">I den **autentisering** dialogrutan **mer**, ladda upp certifikatet och sedan, i den **logga in URL: en** ange SAML enkel inloggning URL: en.</span><span class="sxs-lookup"><span data-stu-id="18836-128">In the **Authentication** dialog box, select **More**, upload the certificate and then, in the **Sign in URL** box, enter the SAML single sign-on URL.</span></span>

  ![Länken ”Mer” i dialogrutan minimerade autentisering](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![I ”logga in URL: en” i dialogrutan för utökade autentisering](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. <span data-ttu-id="18836-131">Om du vill konfigurera automatisk användarinställningar i Azure portal, Välj **etablering** i den vänstra rutan, Välj **automatisk** i den **etablering läge** och välj sedan **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="18836-131">To configure automatic user setup in the Azure portal, select **Provisioning** in the left pane, select **Automatic** in the **Provisioning Mode** box, and then select **Authorize**.</span></span>

  ![Konfigurera automatisk användaretablering i Azure-portalen](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

<span data-ttu-id="18836-133">När gästen eller medlem användare har ställts in i appen Dropbox, får de en separat inbjudan från Dropbox.</span><span class="sxs-lookup"><span data-stu-id="18836-133">After guest or member users have been set up in the Dropbox app, they receive a separate invitation from Dropbox.</span></span> <span data-ttu-id="18836-134">Om du vill använda Dropbox enkel inloggning acceptera inbjudna vill inbjudan genom att klicka på en länk i den.</span><span class="sxs-lookup"><span data-stu-id="18836-134">To use Dropbox single sign-on, invitees must accept the invitation by clicking a link in it.</span></span>

## <a name="box"></a><span data-ttu-id="18836-135">Box</span><span class="sxs-lookup"><span data-stu-id="18836-135">Box</span></span>
<span data-ttu-id="18836-136">Du kan låta användare autentisera rutan gästanvändare med sina Azure AD-kontot med hjälp av federation som baseras på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="18836-136">You can enable users to authenticate Box guest users with their Azure AD account by using federation that's based on the SAML protocol.</span></span> <span data-ttu-id="18836-137">Den här metoden Överför metadata till Box.com.</span><span class="sxs-lookup"><span data-stu-id="18836-137">In this procedure, you upload metadata to Box.com.</span></span>

1. <span data-ttu-id="18836-138">Lägg till Box-app från enterprise-appar.</span><span class="sxs-lookup"><span data-stu-id="18836-138">Add the Box app from the enterprise apps.</span></span>

2. <span data-ttu-id="18836-139">Konfigurera enkel inloggning i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="18836-139">Configure single sign-on in the following order:</span></span>

  ![Konfigurera rutan enkel inloggning](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 <span data-ttu-id="18836-141">a.</span><span class="sxs-lookup"><span data-stu-id="18836-141">a.</span></span> <span data-ttu-id="18836-142">I den **inloggning URL** kontrollerar du att den inloggnings-URL är inställd på rätt sätt för rutan i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18836-142">In the **Sign on URL** box, ensure that the sign-on URL is set appropriately for Box in the Azure portal.</span></span> <span data-ttu-id="18836-143">Denna URL är Webbadressen till din Box.com-klient.</span><span class="sxs-lookup"><span data-stu-id="18836-143">This URL is the URL of your Box.com tenant.</span></span> <span data-ttu-id="18836-144">Det bör följa en namngivningskonvention *https://.box.com*.</span><span class="sxs-lookup"><span data-stu-id="18836-144">It should follow the naming convention *https://.box.com*.</span></span>  
 <span data-ttu-id="18836-145">Den **identifierare** gäller inte för den här appen, men den fortfarande visas som ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="18836-145">The **Identifier** does not apply to this app, but it still appears as a mandatory field.</span></span>

 <span data-ttu-id="18836-146">b.</span><span class="sxs-lookup"><span data-stu-id="18836-146">b.</span></span> <span data-ttu-id="18836-147">I den **användar-ID** ange **user.mail** (för enkel inloggning för gästkonton).</span><span class="sxs-lookup"><span data-stu-id="18836-147">In the **User identifier** box, enter **user.mail** (for SSO for guest accounts).</span></span>

 <span data-ttu-id="18836-148">c.</span><span class="sxs-lookup"><span data-stu-id="18836-148">c.</span></span> <span data-ttu-id="18836-149">Under **SAML-signeringscertifikat**, klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="18836-149">Under **SAML Signing Certificate**, click **Create new certificate**.</span></span>

 <span data-ttu-id="18836-150">d.</span><span class="sxs-lookup"><span data-stu-id="18836-150">d.</span></span> <span data-ttu-id="18836-151">Om du vill börja konfigurera din Box.com klient om du vill använda Azure AD som en identitetsleverantör och hämta metadatafilen och spara den på din lokala enhet.</span><span class="sxs-lookup"><span data-stu-id="18836-151">To begin configuring your Box.com tenant to use Azure AD as an identity provider, download the metadata file and then save it to your local drive.</span></span>

 <span data-ttu-id="18836-152">e.</span><span class="sxs-lookup"><span data-stu-id="18836-152">e.</span></span> <span data-ttu-id="18836-153">Vidarebefordra metadatafil till rutan supportteam, som konfigurerar enkel inloggning för dig.</span><span class="sxs-lookup"><span data-stu-id="18836-153">Forward the metadata file to the Box support team, which configures single sign-on for you.</span></span>

3. <span data-ttu-id="18836-154">Azure AD automatisk användarinställningar, i den vänstra rutan, Välj **etablering**, och välj sedan **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="18836-154">For Azure AD automatic user setup, in the left pane, select **Provisioning**, and then select **Authorize**.</span></span>

  ![Verifiera Azure AD för att ansluta till rutan](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

<span data-ttu-id="18836-156">Som Dropbox inbjudna lösa rutan inbjudna vill sina inbjudan från Box-app.</span><span class="sxs-lookup"><span data-stu-id="18836-156">Like Dropbox invitees, Box invitees must redeem their invitation from the Box app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18836-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18836-157">Next steps</span></span>

<span data-ttu-id="18836-158">Se följande artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="18836-158">See the following articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="18836-159">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="18836-159">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="18836-160">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="18836-160">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="18836-161">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="18836-161">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="18836-162">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="18836-162">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="18836-163">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="18836-163">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="18836-164">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="18836-164">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="18836-165">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="18836-165">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="18836-166">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="18836-166">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="18836-167">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="18836-167">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="18836-168">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="18836-168">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
