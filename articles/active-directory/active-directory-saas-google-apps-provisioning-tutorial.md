---
title: "Självstudier: Konfigurera Google Apps för automatisk användaretablering i Azure | Microsoft Docs"
description: "Lär dig hur tooautomatically etablera och avinstallation etablera användarkontona från Azure AD tooGoogle appar."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: d1fa8449bd6013d1627b3552aaa19db1c0f4f46f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a><span data-ttu-id="4d6e6-103">Självstudier: Konfigurera Google Apps för automatisk användaretablering</span><span class="sxs-lookup"><span data-stu-id="4d6e6-103">Tutorial: Configuring Google Apps for automatic user provisioning</span></span>

<span data-ttu-id="4d6e6-104">hello syftet med den här kursen är tooshow du hello steg du behöver tooperform i Google Apps och Azure AD tooautomatically etablera och avetablera användarkonton från Azure AD tooGoogle appar.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Google Apps and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooGoogle Apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d6e6-105">Krav</span><span class="sxs-lookup"><span data-stu-id="4d6e6-105">Prerequisites</span></span>

<span data-ttu-id="4d6e6-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4d6e6-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="4d6e6-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="4d6e6-108">Du måste ha en giltig klient för Google Apps för arbets- eller Google Apps för utbildning.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-108">You must have a valid tenant for Google Apps for Work or Google Apps for Education.</span></span> <span data-ttu-id="4d6e6-109">Du kan använda ett kostnadsfritt utvärderingskonto för antingen service.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-109">You may use a free trial account for either service.</span></span>
*   <span data-ttu-id="4d6e6-110">Ett användarkonto i Google Apps-teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-110">A user account in Google Apps with Team Admin permissions.</span></span>

## <a name="assigning-users-toogoogle-apps"></a><span data-ttu-id="4d6e6-111">Tilldela användare tooGoogle appar</span><span class="sxs-lookup"><span data-stu-id="4d6e6-111">Assigning users tooGoogle Apps</span></span>

<span data-ttu-id="4d6e6-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="4d6e6-113">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="4d6e6-114">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Google Apps app.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Google Apps app.</span></span> <span data-ttu-id="4d6e6-115">När du valt, kan du tilldela dessa användare tooyour Google Apps-app genom att följa hello anvisningarna här: [tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span><span class="sxs-lookup"><span data-stu-id="4d6e6-115">Once decided, you can assign these users tooyour Google Apps app by following hello instructions here: [Assign a user or group tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)</span></span>

> [!IMPORTANT]
>*   <span data-ttu-id="4d6e6-116">Vi rekommenderar att en enda Azure AD-användare tilldelas tooGoogle appar tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-116">It is recommended that a single Azure AD user be assigned tooGoogle Apps tootest hello provisioning configuration.</span></span> <span data-ttu-id="4d6e6-117">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-117">Additional users and/or groups may be assigned later.</span></span>
>*   <span data-ttu-id="4d6e6-118">När du tilldelar en användare tooGoogle appar kan välja du hello användaren eller rollen ”grupp” i dialogrutan för tilldelning av hello.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-118">When assigning a user tooGoogle Apps, you must select hello User or "Group" role in hello assignment dialog.</span></span> <span data-ttu-id="4d6e6-119">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-119">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="4d6e6-120">Aktivera automatisk användaretablering</span><span class="sxs-lookup"><span data-stu-id="4d6e6-120">Enable automated user provisioning</span></span>

<span data-ttu-id="4d6e6-121">Det här avsnittet hjälper dig att ansluta dina Azure AD tooGoogle appar användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Google Apps baserat på tilldelning av användare och grupper i Azure AD .</span><span class="sxs-lookup"><span data-stu-id="4d6e6-121">This section guides you through connecting your Azure AD tooGoogle Apps's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Google Apps based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="4d6e6-122">Du kan också välja tooenabled SAML-baserade enkel inloggning för Google Apps följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4d6e6-122">You may also choose tooenabled SAML-based Single Sign-On for Google Apps, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4d6e6-123">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-123">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="configure-automatic-user-account-provisioning"></a><span data-ttu-id="4d6e6-124">Konfigurera automatisk användarens konto-etablering</span><span class="sxs-lookup"><span data-stu-id="4d6e6-124">Configure automatic user account provisioning</span></span>

> [!NOTE]
> <span data-ttu-id="4d6e6-125">En annan genomförbart alternativ för att automatisera användaretablering tooGoogle appar är toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) som etablerar din lokala Active Directory identiteter tooGoogle appar.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-125">Another viable option for automating user provisioning tooGoogle Apps is toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) which provisions your on-premises Active Directory identities tooGoogle Apps.</span></span> <span data-ttu-id="4d6e6-126">Däremot etablerar hello lösning i den här självstudiekursen Azure Active Directory (moln) användare och e-postaktiverade grupper tooGoogle appar.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-126">In contrast, hello solution in this tutorial provisions your Azure Active Directory (cloud) users and mail-enabled groups tooGoogle Apps.</span></span> 

1. <span data-ttu-id="4d6e6-127">Logga in på hello [Google Apps-administrationskonsolen](http://admin.google.com/) med ditt administratörskonto och på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-127">Sign into hello [Google Apps Admin Console](http://admin.google.com/) using your administrator account, and click **Security**.</span></span> <span data-ttu-id="4d6e6-128">Om du inte ser hello länken, det kan vara dolt under hello **fler kontroller** menyn längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-128">If you don't see hello link, it may be hidden under hello **More Controls** menu at hello bottom of hello screen.</span></span>
   
    ![Klicka på Security.][10]

2. <span data-ttu-id="4d6e6-130">På hello **säkerhet** klickar du på **API-referens för**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-130">On hello **Security** page, click **API Reference**.</span></span>
   
    ![Klicka på API-referens.][15]

3. <span data-ttu-id="4d6e6-132">Välj **aktivera API-åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-132">Select **Enable API access**.</span></span>
   
    ![Klicka på API-referens.][16]

    > [!IMPORTANT]
    > <span data-ttu-id="4d6e6-134">För varje användare som du avser tooprovision tooGoogle appar, sitt lösenord i Azure Active Directory *måste* vara bundet tooa anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-134">For every user that you intend tooprovision tooGoogle Apps, their username in Azure Active Directory *must* be tied tooa custom domain.</span></span> <span data-ttu-id="4d6e6-135">Till exempel användarnamn som ser ut som bob@contoso.onmicrosoft.com accepteras inte av Google Apps medan bob@contoso.com accepteras.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-135">For example, usernames that look like bob@contoso.onmicrosoft.com is not accepted by Google Apps, whereas bob@contoso.com is accepted.</span></span> <span data-ttu-id="4d6e6-136">Du kan ändra en befintlig användares domän genom att redigera deras egenskaper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-136">You can change an existing user's domain by editing their properties in Azure AD.</span></span> <span data-ttu-id="4d6e6-137">Instruktioner för hur tooset en anpassad domän för både Azure Active Directory och Google Apps ingår i följande steg.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-137">Instructions for how tooset a custom domain for both Azure Active Directory and Google Apps are included in following steps.</span></span>
      
4. <span data-ttu-id="4d6e6-138">Om du inte har lagt till en anpassad domän namn tooyour Azure Active Directory ännu, så du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4d6e6-138">If you haven't added a custom domain name tooyour Azure Active Directory yet, then follow hello following steps:</span></span>
  
    <span data-ttu-id="4d6e6-139">a.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-139">a.</span></span> <span data-ttu-id="4d6e6-140">I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-140">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span> <span data-ttu-id="4d6e6-141">Välj din katalog i hello directory lista.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-141">In hello directory list, select your directory.</span></span> 

    <span data-ttu-id="4d6e6-142">b.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-142">b.</span></span> <span data-ttu-id="4d6e6-143">Klicka på **domäner namnet** på hello vänstra navigeringsfönstret och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-143">Click **Domains name** on hello left navigation pane, and then click **Add**.</span></span>
     
     ![Domän](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![Lägg till domän](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    <span data-ttu-id="4d6e6-146">c.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-146">c.</span></span> <span data-ttu-id="4d6e6-147">Skriv ditt domännamn i hello **domännamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-147">Type your domain name into hello **Domain name** field.</span></span> <span data-ttu-id="4d6e6-148">Det här domännamnet måste vara hello samma domännamn som du avser toouse för Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-148">This domain name should be hello same domain name that you intend toouse for Google Apps.</span></span> <span data-ttu-id="4d6e6-149">När du är klar klickar du på hello **Lägg till domän** knappen.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-149">When ready, click hello **Add Domain** button.</span></span>
     
     ![Domännamn](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    <span data-ttu-id="4d6e6-151">d.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-151">d.</span></span> <span data-ttu-id="4d6e6-152">Klicka på **nästa** toogo toohello bekräftelsesidan.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-152">Click **Next** toogo toohello verification page.</span></span> <span data-ttu-id="4d6e6-153">tooverify som du äger den här domänen måste du redigera hello domänens DNS-poster enligt toohello värden på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-153">tooverify that you own this domain, you must edit hello domain's DNS records according toohello values provided on this page.</span></span> <span data-ttu-id="4d6e6-154">Du kan välja att använda antingen tooverify **MX-poster** eller **TXT-poster**, beroende på vad du väljer för hello **posttyp** alternativet.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-154">You may choose tooverify using either **MX records** or **TXT records**, depending on what you select for hello **Record Type** option.</span></span> <span data-ttu-id="4d6e6-155">Mer omfattande instruktioner för hur tooverify domännamn med Azure AD [lägga till egna domänen namnet tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="4d6e6-155">For more comprehensive instructions on how tooverify domain name with Azure AD, refer [Add your own domain name tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).</span></span>
     
     ![Domän](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    <span data-ttu-id="4d6e6-157">e.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-157">e.</span></span> <span data-ttu-id="4d6e6-158">Upprepa föregående steg för alla hello-domäner som du avser tooadd tooyour directory hello.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-158">Repeat hello preceding steps for all hello domains that you intend tooadd tooyour directory.</span></span>

5. <span data-ttu-id="4d6e6-159">Nu när du har verifierat dina domäner med Azure AD, måste du nu kontrollera dem igen med Google Apps.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-159">Now that you have verified all your domains with Azure AD, you must now verify them again with Google Apps.</span></span> <span data-ttu-id="4d6e6-160">Utför hello följande steg för varje domän som inte redan har registrerats med Google Apps:</span><span class="sxs-lookup"><span data-stu-id="4d6e6-160">For each domain that isn't already registered with Google Apps, perform hello following steps:</span></span>
   
    <span data-ttu-id="4d6e6-161">a.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-161">a.</span></span> <span data-ttu-id="4d6e6-162">I hello [Google Apps-administrationskonsolen](http://admin.google.com/), klickar du på **domäner**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-162">In hello [Google Apps Admin Console](http://admin.google.com/), click **Domains**.</span></span>
     
     ![Klicka på domäner][20]

    <span data-ttu-id="4d6e6-164">b.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-164">b.</span></span> <span data-ttu-id="4d6e6-165">Klicka på **lägga till en domän eller en domän alias**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-165">Click **Add a domain or a domain alias**.</span></span>
     
     ![Lägg till en ny domän][21]

    <span data-ttu-id="4d6e6-167">c.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-167">c.</span></span> <span data-ttu-id="4d6e6-168">Välj **lägga till en annan domän**, och Skriv hello namn i hello-domän som du vill att tooadd.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-168">Select **Add another domain**, and type in hello name of hello domain that you would like tooadd.</span></span>
     
     ![Ange ditt domännamn][22]

    <span data-ttu-id="4d6e6-170">d.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-170">d.</span></span> <span data-ttu-id="4d6e6-171">Klicka på **Fortsätt och verifiera domänen ägarskap**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-171">Click **Continue and verify domain ownership**.</span></span> <span data-ttu-id="4d6e6-172">Följ sedan hello steg tooverify att du äger hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-172">Then follow hello steps tooverify that you own hello domain name.</span></span> <span data-ttu-id="4d6e6-173">Omfattande anvisningar för hur tooverify domänen med Google Apps finns i.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-173">For comprehensive instructions on how tooverify your domain with Google Apps, see.</span></span> <span data-ttu-id="4d6e6-174">[Verifiera ditt platsägarskap med Google Apps](https://support.google.com/webmasters/answer/35179).</span><span class="sxs-lookup"><span data-stu-id="4d6e6-174">[Verify your site ownership with Google Apps](https://support.google.com/webmasters/answer/35179).</span></span>

    <span data-ttu-id="4d6e6-175">e.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-175">e.</span></span> <span data-ttu-id="4d6e6-176">Upprepa föregående steg för eventuella ytterligare domäner som du avser tooadd tooGoogle appar hello.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-176">Repeat hello preceding steps for any additional domains that you intend tooadd tooGoogle Apps.</span></span>
     
     > [!WARNING]
     > <span data-ttu-id="4d6e6-177">Om du ändrar hello primär domän för Google Apps klienten, och om du redan har konfigurerat enkel inloggning med Azure AD och du har toorepeat steg #3 under [steg två: Aktivera enkel inloggning](#step-two-enable-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="4d6e6-177">If you change hello primary domain for your Google Apps tenant, and if you have already configured single sign-on with Azure AD, then you have toorepeat step #3 under [Step Two: Enable Single Sign-On](#step-two-enable-single-sign-on).</span></span>
       
6. <span data-ttu-id="4d6e6-178">I hello [Google Apps-administrationskonsolen](http://admin.google.com/), klickar du på **administratörsroller**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-178">In hello [Google Apps Admin Console](http://admin.google.com/), click **Admin Roles**.</span></span>
   
     ![Klicka på Google Apps][26]

7. <span data-ttu-id="4d6e6-180">Bestämma vilka administrativa konto som du vill att toouse toomanage användaretablering.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-180">Determine which admin account you would like toouse toomanage user provisioning.</span></span> <span data-ttu-id="4d6e6-181">För hello **administratörsroll** på det kontot, redigera hello **privilegier** för rollen.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-181">For hello **admin role** of that account, edit hello **Privileges** for that role.</span></span> <span data-ttu-id="4d6e6-182">Se till att alla hello **API administratörsrättigheter** aktiverade så att det här kontot kan användas för att etablera.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-182">Make sure it has all hello **Admin API Privileges** enabled so that this account can be used for provisioning.</span></span>
   
     ![Klicka på Google Apps][27]
   
    > [!NOTE]
    > <span data-ttu-id="4d6e6-184">Om du konfigurerar en produktionsmiljö är bra för hello toocreate ett administratörskonto i Google Apps specifikt för det här steget.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-184">If you are configuring a production environment, hello best practice is toocreate an admin account in Google Apps specifically for this step.</span></span> <span data-ttu-id="4d6e6-185">Dessa konton måste ha en administratörsroll som är associerade med den som har behörighet som krävs hello API.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-185">These accounts must have an admin role associated with it that has hello necessary API privileges.</span></span>
     
8. <span data-ttu-id="4d6e6-186">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-186">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

9. <span data-ttu-id="4d6e6-187">Om du redan har konfigurerat Google Apps för enkel inloggning söka efter din instans av Google-appar som använder hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-187">If you have already configured Google Apps for single sign-on, search for your instance of Google Apps using hello search field.</span></span> <span data-ttu-id="4d6e6-188">Annars väljer **Lägg till** och Sök efter **Google Apps** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-188">Otherwise, select **Add** and search for **Google Apps** in hello application gallery.</span></span> <span data-ttu-id="4d6e6-189">Välj Google Apps från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-189">Select Google Apps from hello search results, and add it tooyour list of applications.</span></span>

10. <span data-ttu-id="4d6e6-190">Välj din instans av Google Apps och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-190">Select your instance of Google Apps, then select hello **Provisioning** tab.</span></span>

11. <span data-ttu-id="4d6e6-191">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-191">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

     ![Etablering](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. <span data-ttu-id="4d6e6-193">Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-193">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="4d6e6-194">En dialogruta för auktorisering av Google Apps öppnas i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-194">It opens a Google Apps authorization dialog in a new browser window.</span></span>

13. <span data-ttu-id="4d6e6-195">Bekräfta att du vill ha toogive Azure Active Directory toomake behörighetsändringar tooyour Google Apps-klient.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-195">Confirm that you would like toogive Azure Active Directory permission toomake changes tooyour Google Apps tenant.</span></span> <span data-ttu-id="4d6e6-196">Klicka på **acceptera**.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-196">Click **Accept**.</span></span>
    
     ![Kontrollera behörigheter.][28]

14. <span data-ttu-id="4d6e6-198">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Google Apps app.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-198">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Google Apps app.</span></span> <span data-ttu-id="4d6e6-199">Om hello anslutningen misslyckas, kontrollera ditt Google Apps-konto som har administratörsbehörigheter för Team och försöker hello **”Godkänn”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-199">If hello connection fails, ensure your Google Apps account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

15. <span data-ttu-id="4d6e6-200">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-200">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

16. <span data-ttu-id="4d6e6-201">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="4d6e6-201">Click **Save.**</span></span>

17. <span data-ttu-id="4d6e6-202">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooGoogle appar.**</span><span class="sxs-lookup"><span data-stu-id="4d6e6-202">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooGoogle Apps.**</span></span>

18. <span data-ttu-id="4d6e6-203">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooGoogle appar.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-203">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooGoogle Apps.</span></span> <span data-ttu-id="4d6e6-204">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Google Apps för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-204">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Google Apps for update operations.</span></span> <span data-ttu-id="4d6e6-205">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-205">Select hello Save button toocommit any changes.</span></span>

19. <span data-ttu-id="4d6e6-206">tooenable hello Azure AD-etablering tjänsten för Google Apps kan ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="4d6e6-206">tooenable hello Azure AD provisioning service for Google Apps, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

20. <span data-ttu-id="4d6e6-207">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="4d6e6-207">Click **Save.**</span></span>

<span data-ttu-id="4d6e6-208">Den startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooGoogle appar under hello användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-208">It starts hello initial synchronization of any users and/or groups assigned tooGoogle Apps in hello Users and Groups section.</span></span> <span data-ttu-id="4d6e6-209">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-209">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="4d6e6-210">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras på Google Apps-app.</span><span class="sxs-lookup"><span data-stu-id="4d6e6-210">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Google Apps app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d6e6-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4d6e6-211">Additional resources</span></span>

* [<span data-ttu-id="4d6e6-212">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="4d6e6-212">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4d6e6-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4d6e6-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="4d6e6-214">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4d6e6-214">Configure Single Sign-on</span></span>](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png