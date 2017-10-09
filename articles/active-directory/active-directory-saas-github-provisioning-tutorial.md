---
title: "Självstudier: Konfigurera GitHub för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooGitHub."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: c1f0f7a42e4f8a94db3f409cd463e13bb1bc13bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a><span data-ttu-id="b3da3-103">Självstudier: Konfigurera GitHub för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="b3da3-103">Tutorial: Configuring GitHub for Automatic User Provisioning</span></span>


<span data-ttu-id="b3da3-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i GitHub och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooGitHub.</span><span class="sxs-lookup"><span data-stu-id="b3da3-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in GitHub and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooGitHub.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b3da3-105">Krav</span><span class="sxs-lookup"><span data-stu-id="b3da3-105">Prerequisites</span></span>

<span data-ttu-id="b3da3-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b3da3-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="b3da3-107">En Azure Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="b3da3-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="b3da3-108">En Github-klient med hello [företagsplan](https://help.github.com/articles/organization-billing-plans/#business-plan) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="b3da3-108">A Github tenant with hello [Business plan](https://help.github.com/articles/organization-billing-plans/#business-plan) or better enabled</span></span> 
*   <span data-ttu-id="b3da3-109">Ett användarkonto i GitHub med administratörsbehörigheter</span><span class="sxs-lookup"><span data-stu-id="b3da3-109">A user account in GitHub with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="b3da3-110">hello Azure AD-etablering integration förlitar sig på hello [GitHub SCIM API](https://developer.github.com/v3/scim/), som är tillgängliga tooGithub team på hello företagsplan eller bättre.</span><span class="sxs-lookup"><span data-stu-id="b3da3-110">hello Azure AD provisioning integration relies on hello [GitHub SCIM API](https://developer.github.com/v3/scim/), which is available tooGithub teams on hello Business plan or better.</span></span>

## <a name="assigning-users-toogithub"></a><span data-ttu-id="b3da3-111">Tilldela användare tooGitHub</span><span class="sxs-lookup"><span data-stu-id="b3da3-111">Assigning users tooGitHub</span></span>

<span data-ttu-id="b3da3-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="b3da3-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="b3da3-113">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="b3da3-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="b3da3-114">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour GitHub app.</span><span class="sxs-lookup"><span data-stu-id="b3da3-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour GitHub app.</span></span> <span data-ttu-id="b3da3-115">När du valt, kan du tilldela dessa användare tooyour GitHub-app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="b3da3-115">Once decided, you can assign these users tooyour GitHub app by following hello instructions here:</span></span>

[<span data-ttu-id="b3da3-116">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="b3da3-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toogithub"></a><span data-ttu-id="b3da3-117">Viktiga tips för att tilldela användare tooGitHub</span><span class="sxs-lookup"><span data-stu-id="b3da3-117">Important tips for assigning users tooGitHub</span></span>

*   <span data-ttu-id="b3da3-118">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooGitHub tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b3da3-118">It is recommended that a single Azure AD user is assigned tooGitHub tootest hello provisioning configuration.</span></span> <span data-ttu-id="b3da3-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="b3da3-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="b3da3-120">När du tilldelar en användare tooGitHub, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="b3da3-120">When assigning a user tooGitHub, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="b3da3-121">Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.</span><span class="sxs-lookup"><span data-stu-id="b3da3-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toogithub"></a><span data-ttu-id="b3da3-122">Konfigurera användaretablering tooGitHub</span><span class="sxs-lookup"><span data-stu-id="b3da3-122">Configuring user provisioning tooGitHub</span></span> 

<span data-ttu-id="b3da3-123">Det här avsnittet hjälper dig att ansluta din Azure AD-tooGitHub användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i GitHub baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b3da3-123">This section guides you through connecting your Azure AD tooGitHub's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in GitHub based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="b3da3-124">Du kan också välja tooenabled SAML-baserade enkel inloggning för GitHub, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b3da3-124">You may also choose tooenabled SAML-based Single Sign-On for GitHub, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b3da3-125">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="b3da3-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toogithub-in-azure-ad"></a><span data-ttu-id="b3da3-126">Konfigurera automatisk användarkonto etablering tooGitHub i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b3da3-126">Configure automatic user account provisioning tooGitHub in Azure AD</span></span>


1. <span data-ttu-id="b3da3-127">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b3da3-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="b3da3-128">Om du redan har konfigurerat GitHub för enkel inloggning söka efter din instans av GitHub hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="b3da3-128">If you have already configured GitHub for single sign-on, search for your instance of GitHub using hello search field.</span></span> <span data-ttu-id="b3da3-129">Annars väljer **Lägg till** och Sök efter **GitHub** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="b3da3-129">Otherwise, select **Add** and search for **GitHub** in hello application gallery.</span></span> <span data-ttu-id="b3da3-130">Välj GitHub från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="b3da3-130">Select GitHub from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="b3da3-131">Välj din instans av GitHub och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="b3da3-131">Select your instance of GitHub, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="b3da3-132">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="b3da3-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![GitHub-etablering](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. <span data-ttu-id="b3da3-134">Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="b3da3-134">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="b3da3-135">Den här åtgärden öppnar en dialogruta för GitHub-auktorisering i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b3da3-135">This operation opens a GitHub authorization dialog in a new browser window.</span></span> 

6. <span data-ttu-id="b3da3-136">Logga in på GitHub med ditt administratörskonto i hello nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="b3da3-136">In hello new window, sign into GitHub using your Admin account.</span></span> <span data-ttu-id="b3da3-137">I hello resulterande auktorisering dialogrutan väljer hello GitHub team som du vill tooenable etablering för och väljer sedan **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="b3da3-137">In hello resulting authorization dialog, select hello GitHub team that you want tooenable provisioning for, and then select **Authorize**.</span></span> <span data-ttu-id="b3da3-138">När du är klar, returnerar toohello Azure portal toocomplete hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b3da3-138">Once completed, return toohello Azure portal toocomplete hello provisioning configuration.</span></span>

    ![Dialogrutan för auktorisering](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. <span data-ttu-id="b3da3-140">I hello Azure-portalen, ange **klient URL** och klicka på **Testanslutningen** tooensure Azure AD kan ansluta tooyour GitHub app.</span><span class="sxs-lookup"><span data-stu-id="b3da3-140">In hello Azure portal, input **Tenant URL** and click **Test Connection** tooensure Azure AD can connect tooyour GitHub app.</span></span> <span data-ttu-id="b3da3-141">Om hello anslutningen misslyckas, kan du kontrollera att ditt GitHub-konto som har administratörsbehörigheter och **klient URl** inputted är korrekt och försök hello ”verifiera” steg igen (du kan utgöra **klient URL** av regel: ”https : //api.github.com/scim/v2/organizations/ + < Organizations_name > ”, du kan hitta din organisationer under GitHub-konto: **inställningar** > **organisationer**).</span><span class="sxs-lookup"><span data-stu-id="b3da3-141">If hello connection fails, ensure your GitHub account has Admin permissions and **Tenant URl** is inputted correctly, then try hello "Authorize" step again (you can constitute **Tenant URL** by rule: "https://api.github.com/scim/v2/organizations/ + <Organizations_name>", you can find your organizations under your GitHub account: **Settings** > **Organizations**).</span></span>

    ![Dialogrutan för auktorisering](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. <span data-ttu-id="b3da3-143">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.</span><span class="sxs-lookup"><span data-stu-id="b3da3-143">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

9. <span data-ttu-id="b3da3-144">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b3da3-144">Click **Save**.</span></span> 

10. <span data-ttu-id="b3da3-145">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooGitHub**.</span><span class="sxs-lookup"><span data-stu-id="b3da3-145">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooGitHub**.</span></span>

11. <span data-ttu-id="b3da3-146">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooGitHub.</span><span class="sxs-lookup"><span data-stu-id="b3da3-146">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooGitHub.</span></span> <span data-ttu-id="b3da3-147">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i GitHub för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="b3da3-147">hello attributes selected as **Matching** properties are used toomatch hello user accounts in GitHub for update operations.</span></span> <span data-ttu-id="b3da3-148">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="b3da3-148">Select hello Save button toocommit any changes.</span></span>

12. <span data-ttu-id="b3da3-149">tooenable hello Azure AD-etablering tjänsten för GitHub, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="b3da3-149">tooenable hello Azure AD provisioning service for GitHub, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

13. <span data-ttu-id="b3da3-150">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b3da3-150">Click **Save**.</span></span> 

<span data-ttu-id="b3da3-151">Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooGitHub i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b3da3-151">This operation starts hello initial synchronization of any users and/or groups assigned tooGitHub in hello Users and Groups section.</span></span> <span data-ttu-id="b3da3-152">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="b3da3-152">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="b3da3-153">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.</span><span class="sxs-lookup"><span data-stu-id="b3da3-153">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="b3da3-154">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="b3da3-154">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b3da3-155">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b3da3-155">Additional resources</span></span>

* [<span data-ttu-id="b3da3-156">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="b3da3-156">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="b3da3-157">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b3da3-157">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="b3da3-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b3da3-158">Next steps</span></span>

* [<span data-ttu-id="b3da3-159">Lär dig hur tooreview loggar och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="b3da3-159">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
