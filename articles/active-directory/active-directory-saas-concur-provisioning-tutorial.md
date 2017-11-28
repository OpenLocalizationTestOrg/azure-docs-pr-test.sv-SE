---
title: "Självstudier: Azure Active Directory-integrering med Concur | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Concur."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 13ba364af26a5ce0f1d2b51aaa0f84a4c353b107
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a><span data-ttu-id="323f1-103">Självstudier: Konfigurera verklig för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="323f1-103">Tutorial: Configuring Concur for User Provisioning</span></span>

<span data-ttu-id="323f1-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Concur och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooConcur.</span><span class="sxs-lookup"><span data-stu-id="323f1-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Concur and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooConcur.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="323f1-105">Krav</span><span class="sxs-lookup"><span data-stu-id="323f1-105">Prerequisites</span></span>

<span data-ttu-id="323f1-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="323f1-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="323f1-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="323f1-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="323f1-108">Aktivera prenumerationen en Concur enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="323f1-108">A Concur single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="323f1-109">Ett användarkonto i Concur Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="323f1-109">A user account in Concur with Team Admin permissions.</span></span>

## <a name="assigning-users-tooconcur"></a><span data-ttu-id="323f1-110">Tilldela användare tooConcur</span><span class="sxs-lookup"><span data-stu-id="323f1-110">Assigning users tooConcur</span></span>

<span data-ttu-id="323f1-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="323f1-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="323f1-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="323f1-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="323f1-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Concur app.</span><span class="sxs-lookup"><span data-stu-id="323f1-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Concur app.</span></span> <span data-ttu-id="323f1-114">När du valt, kan du tilldela dessa användare tooyour Concur app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="323f1-114">Once decided, you can assign these users tooyour Concur app by following hello instructions here:</span></span>

[<span data-ttu-id="323f1-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="323f1-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooconcur"></a><span data-ttu-id="323f1-116">Viktiga tips för att tilldela användare tooConcur</span><span class="sxs-lookup"><span data-stu-id="323f1-116">Important tips for assigning users tooConcur</span></span>

*   <span data-ttu-id="323f1-117">Vi rekommenderar att en enda Azure AD-användare tilldelas tooConcur tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="323f1-117">It is recommended that a single Azure AD user be assigned tooConcur tootest hello provisioning configuration.</span></span> <span data-ttu-id="323f1-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="323f1-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="323f1-119">Du måste välja en giltig användarroll när du tilldelar en tooConcur för användaren.</span><span class="sxs-lookup"><span data-stu-id="323f1-119">When assigning a user tooConcur, you must select a valid user role.</span></span> <span data-ttu-id="323f1-120">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="323f1-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="323f1-121">Aktivera användaretablering</span><span class="sxs-lookup"><span data-stu-id="323f1-121">Enable user provisioning</span></span>

<span data-ttu-id="323f1-122">Det här avsnittet hjälper dig att ansluta din Azure AD-tooConcur användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Concur baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="323f1-122">This section guides you through connecting your Azure AD tooConcur's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Concur based on user and group assignment in Azure AD.</span></span>

> [!Tip] 
> <span data-ttu-id="323f1-123">Du kan också välja tooenabled SAML-baserade enkel inloggning för Concur, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="323f1-123">You may also choose tooenabled SAML-based Single Sign-On for Concur, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="323f1-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="323f1-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="323f1-125">tooconfigure användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="323f1-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="323f1-126">hello syftet med det här avsnittet är toooutline hur tooenable etablering av Active Directory-användare konton tooConcur.</span><span class="sxs-lookup"><span data-stu-id="323f1-126">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooConcur.</span></span>

<span data-ttu-id="323f1-127">tooenable appar i Hej utgifter Service, har det rätt inställning för toobe och användning av en Web Service Admin-profil.</span><span class="sxs-lookup"><span data-stu-id="323f1-127">tooenable apps in hello Expense Service, there has toobe proper setup and use of a Web Service Admin profile.</span></span> <span data-ttu-id="323f1-128">Lägg inte till hello WS Admin rollen tooyour befintliga administratörsprofilen som du använder för T & E administrativa funktioner.</span><span class="sxs-lookup"><span data-stu-id="323f1-128">Don't add hello WS Admin role tooyour existing administrator profile that you use for T&E administrative functions.</span></span>

<span data-ttu-id="323f1-129">Verklig konsulter eller Hej administratör måste skapa en egen Web tjänstadministratör profil och Hej administratör måste använda den här profilen för hello Web Services Administrator funktioner (till exempel aktiverar appar).</span><span class="sxs-lookup"><span data-stu-id="323f1-129">Concur Consultants or hello client administrator must create a distinct Web Service Administrator profile and hello Client administrator must use this profile for hello Web Services Administrator functions (for example, enabling apps).</span></span> <span data-ttu-id="323f1-130">Dessa profiler måste hållas separat från hello klienten administratör dagliga T & E admin-profil (hello T & E admin-profil ska inte ha tilldelats hello WSAdmin rollen).</span><span class="sxs-lookup"><span data-stu-id="323f1-130">These profiles must be kept separate from hello client administrator's daily T&E admin profile (hello T&E admin profile should not have hello WSAdmin role assigned).</span></span>

<span data-ttu-id="323f1-131">När du skapar hello profil toobe används för att aktivera hello app måste ange hello klienten administratörens namn i hello Användarfält profil.</span><span class="sxs-lookup"><span data-stu-id="323f1-131">When you create hello profile toobe used for enabling hello app, enter hello client administrator's name into hello user profile fields.</span></span> <span data-ttu-id="323f1-132">Den här tilldelas ägarskap toohello profil.</span><span class="sxs-lookup"><span data-stu-id="323f1-132">This assigns ownership toohello profile.</span></span> <span data-ttu-id="323f1-133">När du har skapat en eller flera profiler hello klienten måste logga in med den här profilen tooclick hello ”*aktivera*” knappen för en Partner App hello Web Services-menyn.</span><span class="sxs-lookup"><span data-stu-id="323f1-133">Once one or more profiles is created, hello client must log in with this profile tooclick hello "*Enable*" button for a Partner App within hello Web Services menu.</span></span>

<span data-ttu-id="323f1-134">För hello följande orsaker, utföras åtgärden inte med hello-profil som de använder för normal T & E-administration.</span><span class="sxs-lookup"><span data-stu-id="323f1-134">For hello following reasons, this action should not be done with hello profile they use for normal T&E administration.</span></span>

* <span data-ttu-id="323f1-135">hello klienten har toobe hello som klickar på ”*Ja*” hello dialog-fönstret som visas när en app är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="323f1-135">hello client has toobe hello one that clicks "*Yes*" on hello dialogue window that is displayed after an app is enabled.</span></span> <span data-ttu-id="323f1-136">Detta klickar du på bekräftar hello klienten beredda för hello Partner program tooaccess sina data så att du eller hello Partner inte kan klicka på som Ja knappar.</span><span class="sxs-lookup"><span data-stu-id="323f1-136">This click acknowledges hello client is willing for hello Partner application tooaccess their data, so you or hello Partner cannot click that Yes button.</span></span>

* <span data-ttu-id="323f1-137">Om en administratör som har aktiverat en app med hello T & E admin-profil slutar hello företaget (vilket ger hello-profil som inaktiverat), aktiverade appar med hjälp av den här profilen fungerar inte förrän hello app aktiveras med en annan active WS-administratör profil.</span><span class="sxs-lookup"><span data-stu-id="323f1-137">If a client administrator that has enabled an app using hello T&E admin profile leaves hello company (resulting in hello profile being inactivated), any apps enabled using that profile does not function until hello app is enabled with another active WS Admin profile.</span></span> <span data-ttu-id="323f1-138">Det är därför du borde toocreate distinkta WS Admin-profiler.</span><span class="sxs-lookup"><span data-stu-id="323f1-138">This is why you are supposed toocreate distinct WS Admin profiles.</span></span>

* <span data-ttu-id="323f1-139">Om en administratör slutar hello företaget, associerade hello namn toohello WS Admin-profil kan vara ändrade toohello ersättning administratör om du vill utan att påverka hello aktiverat appen eftersom den här profilen inte behöver inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="323f1-139">If an administrator leaves hello company, hello name associated toohello WS Admin profile can be changed toohello replacement administrator if desired without impacting hello enabled app because that profile does not need inactivated.</span></span>

<span data-ttu-id="323f1-140">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="323f1-140">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="323f1-141">Logga in tooyour **Concur** klient.</span><span class="sxs-lookup"><span data-stu-id="323f1-141">Log on tooyour **Concur** tenant.</span></span>

2. <span data-ttu-id="323f1-142">Från hello **Administration** väljer du **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="323f1-142">From hello **Administration** menu, select **Web Services**.</span></span>
   
    <span data-ttu-id="323f1-143">![Concur klient](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur klient")</span><span class="sxs-lookup"><span data-stu-id="323f1-143">![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")</span></span>

3. <span data-ttu-id="323f1-144">På vänster sida, från hello hello **Web Services** väljer **aktivera Partner program**.</span><span class="sxs-lookup"><span data-stu-id="323f1-144">On hello left side, from hello **Web Services** pane, select **Enable Partner Application**.</span></span>
   
    <span data-ttu-id="323f1-145">![Aktivera Partner program](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "gör att Partner-program")</span><span class="sxs-lookup"><span data-stu-id="323f1-145">![Enable Partner Application](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Enable Partner Application")</span></span>

4. <span data-ttu-id="323f1-146">Från hello **aktivera programmet** väljer **Azure Active Directory**, och klicka sedan på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="323f1-146">From hello **Enable Application** list, select **Azure Active Directory**, and then click **Enable**.</span></span>
   
    <span data-ttu-id="323f1-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span><span class="sxs-lookup"><span data-stu-id="323f1-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span></span>

5. <span data-ttu-id="323f1-148">Klicka på **Ja** tooclose hello **bekräfta åtgärden** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="323f1-148">Click **Yes** tooclose hello **Confirm Action** dialog.</span></span>
   
    <span data-ttu-id="323f1-149">![Bekräfta åtgärden](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "bekräfta åtgärden")</span><span class="sxs-lookup"><span data-stu-id="323f1-149">![Confirm Action](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Confirm Action")</span></span>

6. <span data-ttu-id="323f1-150">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="323f1-150">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

7. <span data-ttu-id="323f1-151">Om du redan har konfigurerat Concur för enkel inloggning, söka efter din instans av Concur hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="323f1-151">If you have already configured Concur for single sign-on, search for your instance of Concur using hello search field.</span></span> <span data-ttu-id="323f1-152">Annars väljer **Lägg till** och Sök efter **Concur** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="323f1-152">Otherwise, select **Add** and search for **Concur** in hello application gallery.</span></span> <span data-ttu-id="323f1-153">Välj Concur från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="323f1-153">Select Concur from hello search results, and add it tooyour list of applications.</span></span>

8. <span data-ttu-id="323f1-154">Välj din instans av Concur och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="323f1-154">Select your instance of Concur, then select hello **Provisioning** tab.</span></span>

9. <span data-ttu-id="323f1-155">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="323f1-155">Set hello **Provisioning Mode** too**Automatic**.</span></span> 
 
    ![Etablering](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. <span data-ttu-id="323f1-157">Under hello **administratörsautentiseringsuppgifter** ange hello **användarnamn** och hello **lösenord** av administratören Concur.</span><span class="sxs-lookup"><span data-stu-id="323f1-157">Under hello **Admin Credentials** section, enter hello **user name** and hello **password** of your Concur administrator.</span></span>

11. <span data-ttu-id="323f1-158">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Concur app.</span><span class="sxs-lookup"><span data-stu-id="323f1-158">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Concur app.</span></span> <span data-ttu-id="323f1-159">Om hello anslutning misslyckas kan du kontrollera att ditt Concur-konto har teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="323f1-159">If hello connection fails, ensure your Concur account has Team Admin permissions.</span></span>

12. <span data-ttu-id="323f1-160">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="323f1-160">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

13. <span data-ttu-id="323f1-161">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="323f1-161">Click **Save.**</span></span>

14. <span data-ttu-id="323f1-162">Välj under hello mappningar avsnitt, **tooConcur synkronisera Azure Active Directory-användare.**</span><span class="sxs-lookup"><span data-stu-id="323f1-162">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooConcur.**</span></span>

15. <span data-ttu-id="323f1-163">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooConcur.</span><span class="sxs-lookup"><span data-stu-id="323f1-163">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooConcur.</span></span> <span data-ttu-id="323f1-164">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Concur för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="323f1-164">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Concur for update operations.</span></span> <span data-ttu-id="323f1-165">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="323f1-165">Select hello Save button toocommit any changes.</span></span>

16. <span data-ttu-id="323f1-166">tooenable hello Azure AD-etablering tjänsten för Concur, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="323f1-166">tooenable hello Azure AD provisioning service for Concur, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

17. <span data-ttu-id="323f1-167">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="323f1-167">Click **Save.**</span></span>

<span data-ttu-id="323f1-168">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="323f1-168">You can now create a test account.</span></span> <span data-ttu-id="323f1-169">Vänta tills in tooverify hello kontot har synkroniserats tooConcur too20 i minuter.</span><span class="sxs-lookup"><span data-stu-id="323f1-169">Wait for up too20 minutes tooverify that hello account has been synchronized tooConcur.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="323f1-170">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="323f1-170">Additional resources</span></span>

* [<span data-ttu-id="323f1-171">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="323f1-171">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="323f1-172">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="323f1-172">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="323f1-173">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="323f1-173">Configure Single Sign-on</span></span>](active-directory-saas-concur-tutorial.md)

