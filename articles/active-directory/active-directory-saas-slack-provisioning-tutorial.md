---
title: "Självstudier: Konfigurera Slack för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooSlack."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: asmalser-msft
ms.reviewer: asmalser
ms.openlocfilehash: d0a565bbe0bd7e229b9dd99b1ebbaf67d93a2206
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-slack-for-automatic-user-provisioning"></a><span data-ttu-id="90873-103">Självstudier: Konfigurera Slack för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="90873-103">Tutorial: Configuring Slack for Automatic User Provisioning</span></span>


<span data-ttu-id="90873-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Slack och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="90873-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Slack and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSlack.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="90873-105">Krav</span><span class="sxs-lookup"><span data-stu-id="90873-105">Prerequisites</span></span>

<span data-ttu-id="90873-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="90873-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="90873-107">En Azure Active Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="90873-107">An Azure Active Active directory tenant</span></span>
*   <span data-ttu-id="90873-108">En Slack-klienten med hello [Plus plan](https://aadsyncfabric.slack.com/pricing) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="90873-108">A Slack tenant with hello [Plus plan](https://aadsyncfabric.slack.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="90873-109">Ett användarkonto i Slack med administratörsbehörigheter för Team</span><span class="sxs-lookup"><span data-stu-id="90873-109">A user account in Slack with Team Admin permissions</span></span> 

<span data-ttu-id="90873-110">Obs: hello Azure AD-etablering integration förlitar sig på hello [Slack SCIM API](https://api.slack.com/scim) som är tillgängliga tooSlack team på hello Plus plan eller bättre.</span><span class="sxs-lookup"><span data-stu-id="90873-110">Note: hello Azure AD provisioning integration relies on hello [Slack SCIM API](https://api.slack.com/scim) which is available tooSlack teams on hello Plus plan or better.</span></span>

## <a name="assigning-users-tooslack"></a><span data-ttu-id="90873-111">Tilldela användare tooSlack</span><span class="sxs-lookup"><span data-stu-id="90873-111">Assigning users tooSlack</span></span>

<span data-ttu-id="90873-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="90873-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="90873-113">Hello gäller automatisk konto användaretablering är kommer bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD att synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="90873-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="90873-114">Innan du konfigurerar och aktiverar hello etableras, behöver du toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Slack app.</span><span class="sxs-lookup"><span data-stu-id="90873-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Slack app.</span></span> <span data-ttu-id="90873-115">När du valt, kan du tilldela dessa användare tooyour Slack-app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="90873-115">Once decided, you can assign these users tooyour Slack app by following hello instructions here:</span></span>

[<span data-ttu-id="90873-116">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="90873-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-tooslack"></a><span data-ttu-id="90873-117">Viktiga tips för att tilldela användare tooSlack</span><span class="sxs-lookup"><span data-stu-id="90873-117">Important tips for assigning users tooSlack</span></span>

*   <span data-ttu-id="90873-118">Vi rekommenderar att en enda Azure AD-användare tilldelas tooSlack tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="90873-118">It is recommended that a single Azure AD user be assigned tooSlack tootest hello provisioning configuration.</span></span> <span data-ttu-id="90873-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="90873-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="90873-120">När du tilldelar en användare tooSlack, måste du välja hello **användaren** eller ”grupp” roll i hello tilldelning dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="90873-120">When assigning a user tooSlack, you must select hello **User** or "Group" role in hello assignment dialog.</span></span> <span data-ttu-id="90873-121">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="90873-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-tooslack"></a><span data-ttu-id="90873-122">Konfigurera användaretablering tooSlack</span><span class="sxs-lookup"><span data-stu-id="90873-122">Configuring user provisioning tooSlack</span></span> 

<span data-ttu-id="90873-123">Det här avsnittet hjälper dig att ansluta din Azure AD-tooSlack användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Slack baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90873-123">This section guides you through connecting your Azure AD tooSlack's user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in Slack based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="90873-124">**Tips:** du kan också välja tooenabled SAML-baserade enkel inloggning för Slack hello instruktionerna som anges i (Azure portal) [https://portal.azure.com].</span><span class="sxs-lookup"><span data-stu-id="90873-124">**Tip:** You may also choose tooenabled SAML-based Single Sign-On for Slack, following hello instructions provided in (Azure portal)[https://portal.azure.com].</span></span> <span data-ttu-id="90873-125">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="90873-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-tooslack-in-azure-ad"></a><span data-ttu-id="90873-126">tooconfigure automatisk användarkonto etablering tooSlack i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="90873-126">tooconfigure automatic user account provisioning tooSlack in Azure AD:</span></span>


1)  <span data-ttu-id="90873-127">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="90873-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2) <span data-ttu-id="90873-128">Om du redan har konfigurerat Slack för enkel inloggning, söka efter din instans av Slack hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="90873-128">If you have already configured Slack for single sign-on, search for your instance of Slack using hello search field.</span></span> <span data-ttu-id="90873-129">Annars väljer **Lägg till** och Sök efter **Slack** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="90873-129">Otherwise, select **Add** and search for **Slack** in hello application gallery.</span></span> <span data-ttu-id="90873-130">Välj Slack från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="90873-130">Select Slack from hello search results, and add it tooyour list of applications.</span></span>

3)  <span data-ttu-id="90873-131">Välj din instans av Slack och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="90873-131">Select your instance of Slack, then select hello **Provisioning** tab.</span></span>

4)  <span data-ttu-id="90873-132">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="90873-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![Slack-etablering](./media/active-directory-saas-slack-provisioning-tutorial/Slack1.PNG)

5)  <span data-ttu-id="90873-134">Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="90873-134">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="90873-135">En Slack auktorisering dialogruta öppnas i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="90873-135">This opens a Slack authorization dialog in a new browser window.</span></span> 

6) <span data-ttu-id="90873-136">Logga in till Slack med ditt Team administratörskonto i hello nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="90873-136">In hello new window, sign into Slack using your Team Admin account.</span></span> <span data-ttu-id="90873-137">hello resulterande auktorisering dialogrutan Välj hello Slack team som du vill tooenable etablering för och väljer sedan **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="90873-137">in hello resulting authorization dialog, select hello Slack team that you want tooenable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="90873-138">När du är klar, returnerar toohello Azure portal toocomplete hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="90873-138">Once completed, return toohello Azure portal toocomplete hello provisioning configuration.</span></span>

![Dialogrutan för auktorisering](./media/active-directory-saas-slack-provisioning-tutorial/Slack3.PNG)

7) <span data-ttu-id="90873-140">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Slack app.</span><span class="sxs-lookup"><span data-stu-id="90873-140">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Slack app.</span></span> <span data-ttu-id="90873-141">Om hello anslutningen misslyckas, kontrollera kontots Slack har administratörsbehörigheter för Team och försök hello ”Godkänn” steg igen.</span><span class="sxs-lookup"><span data-stu-id="90873-141">If hello connection fails, ensure your Slack account has Team Admin permissions and try hello "Authorize" step again.</span></span>

8) <span data-ttu-id="90873-142">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.</span><span class="sxs-lookup"><span data-stu-id="90873-142">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

9) <span data-ttu-id="90873-143">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="90873-143">Click **Save**.</span></span> 

10) <span data-ttu-id="90873-144">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooSlack**.</span><span class="sxs-lookup"><span data-stu-id="90873-144">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSlack**.</span></span>

11) <span data-ttu-id="90873-145">I hello **attributmappning** avsnittet kan du granska hello användarattribut som ska synkroniseras från Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="90873-145">In hello **Attribute Mappings** section, review hello user attributes that will be synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="90873-146">Observera att hello attribut som valts som **matchande** egenskaper kommer att använda toomatch hello användarkonton i Slack för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="90873-146">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts in Slack for update operations.</span></span> <span data-ttu-id="90873-147">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="90873-147">Select hello Save button toocommit any changes.</span></span>

12) <span data-ttu-id="90873-148">tooenable hello Azure AD-etablering tjänsten för Slack, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="90873-148">tooenable hello Azure AD provisioning service for Slack, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

13) <span data-ttu-id="90873-149">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="90873-149">Click **Save**.</span></span> 

<span data-ttu-id="90873-150">Detta startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooSlack i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="90873-150">This will start hello initial synchronization of any users and/or groups assigned tooSlack in hello Users and Groups section.</span></span> <span data-ttu-id="90873-151">Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tionde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="90873-151">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 10 minutes as long as hello service is running.</span></span> <span data-ttu-id="90873-152">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello-tjänsten på appen Slack-etablering.</span><span class="sxs-lookup"><span data-stu-id="90873-152">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Slack app.</span></span>

## <a name="optional-configuring-group-object-provisioning-tooslack"></a><span data-ttu-id="90873-153">[Valfritt] Konfigurera gruppobjekt etablering tooSlack</span><span class="sxs-lookup"><span data-stu-id="90873-153">[Optional] Configuring group object provisioning tooSlack</span></span> 

<span data-ttu-id="90873-154">Du kan också, aktivera hello etablering av gruppobjekt från Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="90873-154">Optionally, you can enable hello provisioning of group objects from Azure AD tooSlack.</span></span> <span data-ttu-id="90873-155">Detta skiljer sig från ”tilldela grupper av användare”, i det faktiska gruppobjektet hello dessutom tooits medlemmar kommer att replikeras från Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="90873-155">This is different from "assigning groups of users", in that hello actual group object in addition tooits members will be replicated from Azure AD tooSlack.</span></span> <span data-ttu-id="90873-156">Om du har en grupp med namnet ”min grupp” i Azure AD, till exempel skapas en identitical grupp med namnet ”min grupp” inuti Slack.</span><span class="sxs-lookup"><span data-stu-id="90873-156">For example, if you have a group named "My Group" in Azure AD, an identitical group named "My Group" will be created inside Slack.</span></span>

### <a name="tooenable-provisioning-of-group-objects"></a><span data-ttu-id="90873-157">tooenable etablering av gruppobjekt:</span><span class="sxs-lookup"><span data-stu-id="90873-157">tooenable provisioning of group objects:</span></span>

1) <span data-ttu-id="90873-158">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-grupper tooSlack**.</span><span class="sxs-lookup"><span data-stu-id="90873-158">Under hello Mappings section, select **Synchronize Azure Active Directory Groups tooSlack**.</span></span>

2) <span data-ttu-id="90873-159">Ange aktiverad tooYes hello attributmappning-bladet.</span><span class="sxs-lookup"><span data-stu-id="90873-159">In hello Attribute Mapping blade, set Enabled tooYes.</span></span>

3) <span data-ttu-id="90873-160">I hello **attributmappning** avsnittet kan du granska hello Gruppattribut som synkroniseras från Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="90873-160">In hello **Attribute Mappings** section, review hello group attributes that will be synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="90873-161">Observera att hello attribut som valts som **matchande** egenskaper kommer att använda toomatch hello grupper i Slack för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="90873-161">Note that hello attributes selected as **Matching** properties will be used toomatch hello groups in Slack for update operations.</span></span> 

4) <span data-ttu-id="90873-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="90873-162">Click **Save**.</span></span>

<span data-ttu-id="90873-163">Det här resultatet i en grupp objekt som tilldelats tooSlack i hello **användare och grupper** avsnittet fullständigt som synkroniseras från Azure AD tooSlack.</span><span class="sxs-lookup"><span data-stu-id="90873-163">This result in any group objects assigned tooSlack in hello **Users and Groups** section being fully synchronized from Azure AD tooSlack.</span></span> <span data-ttu-id="90873-164">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello-tjänsten på appen Slack-etablering.</span><span class="sxs-lookup"><span data-stu-id="90873-164">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Slack app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="90873-165">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="90873-165">Additional Resources</span></span>

* [<span data-ttu-id="90873-166">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="90873-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="90873-167">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="90873-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
