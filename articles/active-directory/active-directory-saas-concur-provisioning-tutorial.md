---
title: "Självstudier: Azure Active Directory-integrering med Concur | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Concur."
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
ms.openlocfilehash: cd35b6e2dc3171e9cffdb820bbc5b0d45ff58e07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-concur-for-user-provisioning"></a><span data-ttu-id="cfd42-103">Självstudier: Konfigurera verklig för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="cfd42-103">Tutorial: Configuring Concur for User Provisioning</span></span>

<span data-ttu-id="cfd42-104">Syftet med den här kursen är att visa de steg som du behöver göra i Concur och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till Concur.</span><span class="sxs-lookup"><span data-stu-id="cfd42-104">The objective of this tutorial is to show you the steps you need to perform in Concur and Azure AD to automatically provision and de-provision user accounts from Azure AD to Concur.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cfd42-105">Krav</span><span class="sxs-lookup"><span data-stu-id="cfd42-105">Prerequisites</span></span>

<span data-ttu-id="cfd42-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cfd42-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="cfd42-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="cfd42-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="cfd42-108">Aktivera prenumerationen en Concur enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cfd42-108">A Concur single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="cfd42-109">Ett användarkonto i Concur Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="cfd42-109">A user account in Concur with Team Admin permissions.</span></span>

## <a name="assigning-users-to-concur"></a><span data-ttu-id="cfd42-110">Tilldela användare till Concur</span><span class="sxs-lookup"><span data-stu-id="cfd42-110">Assigning users to Concur</span></span>

<span data-ttu-id="cfd42-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="cfd42-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="cfd42-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfd42-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="cfd42-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till appen Concur.</span><span class="sxs-lookup"><span data-stu-id="cfd42-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Concur app.</span></span> <span data-ttu-id="cfd42-114">När bestämt, kan du tilldela dessa användare i appen Concur genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="cfd42-114">Once decided, you can assign these users to your Concur app by following the instructions here:</span></span>

[<span data-ttu-id="cfd42-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="cfd42-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a><span data-ttu-id="cfd42-116">Viktiga tips för att tilldela användare till Concur</span><span class="sxs-lookup"><span data-stu-id="cfd42-116">Important tips for assigning users to Concur</span></span>

*   <span data-ttu-id="cfd42-117">Vi rekommenderar att en enda Azure AD-användare tilldelas Concur att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="cfd42-117">It is recommended that a single Azure AD user be assigned to Concur to test the provisioning configuration.</span></span> <span data-ttu-id="cfd42-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="cfd42-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="cfd42-119">När du tilldelar en användare Concur, måste du välja en giltig användarroll.</span><span class="sxs-lookup"><span data-stu-id="cfd42-119">When assigning a user to Concur, you must select a valid user role.</span></span> <span data-ttu-id="cfd42-120">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="cfd42-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="cfd42-121">Aktivera användaretablering</span><span class="sxs-lookup"><span data-stu-id="cfd42-121">Enable user provisioning</span></span>

<span data-ttu-id="cfd42-122">Det här avsnittet hjälper dig att ansluta din Azure AD till Concurs användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i Concur baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfd42-122">This section guides you through connecting your Azure AD to Concur's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Concur based on user and group assignment in Azure AD.</span></span>

> [!Tip] 
> <span data-ttu-id="cfd42-123">Du kan också välja att aktivera SAML-baserade enkel inloggning för Concur, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cfd42-123">You may also choose to enabled SAML-based Single Sign-On for Concur, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="cfd42-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="cfd42-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="cfd42-125">Konfigurera användaretablering för kontot:</span><span class="sxs-lookup"><span data-stu-id="cfd42-125">To configure user account provisioning:</span></span>

<span data-ttu-id="cfd42-126">Syftet med det här avsnittet är att beskriva hur du aktiverar etablering av Active Directory-användarkonton till Concur.</span><span class="sxs-lookup"><span data-stu-id="cfd42-126">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Concur.</span></span>

<span data-ttu-id="cfd42-127">Om du vill aktivera appar i utgifter tjänsten, det måste vara rätt inställning och användning av en Web Service Admin-profil.</span><span class="sxs-lookup"><span data-stu-id="cfd42-127">To enable apps in the Expense Service, there has to be proper setup and use of a Web Service Admin profile.</span></span> <span data-ttu-id="cfd42-128">Lägg inte till den WS administratörsroll till din befintliga administratörsprofil som du använder för T & E administrativa funktioner.</span><span class="sxs-lookup"><span data-stu-id="cfd42-128">Don't add the WS Admin role to your existing administrator profile that you use for T&E administrative functions.</span></span>

<span data-ttu-id="cfd42-129">Verklig konsulter eller klient-administratören måste skapa en egen Web tjänstadministratör profil och administratören klienten måste använda den här profilen för Web Services Administrator funktioner (till exempel aktiverar appar).</span><span class="sxs-lookup"><span data-stu-id="cfd42-129">Concur Consultants or the client administrator must create a distinct Web Service Administrator profile and the Client administrator must use this profile for the Web Services Administrator functions (for example, enabling apps).</span></span> <span data-ttu-id="cfd42-130">Dessa profiler måste hållas separat från klienten administratörens dagliga T & E admin-profil (T & E admin profilen inte ska ha rollen WSAdmin).</span><span class="sxs-lookup"><span data-stu-id="cfd42-130">These profiles must be kept separate from the client administrator's daily T&E admin profile (the T&E admin profile should not have the WSAdmin role assigned).</span></span>

<span data-ttu-id="cfd42-131">När du skapar en profil som ska användas för att aktivera appen, ange klienten administratörens namn i profilen Användarfält.</span><span class="sxs-lookup"><span data-stu-id="cfd42-131">When you create the profile to be used for enabling the app, enter the client administrator's name into the user profile fields.</span></span> <span data-ttu-id="cfd42-132">Den här tilldelas ägarskap till profilen.</span><span class="sxs-lookup"><span data-stu-id="cfd42-132">This assigns ownership to the profile.</span></span> <span data-ttu-id="cfd42-133">När du har skapat en eller flera profiler klienten måste logga in med den här profilen på den ”*aktivera*” knappen för en Partner-App i menyn webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="cfd42-133">Once one or more profiles is created, the client must log in with this profile to click the "*Enable*" button for a Partner App within the Web Services menu.</span></span>

<span data-ttu-id="cfd42-134">Av följande skäl bör den här åtgärden inte utföras med den profil som de använder för normal T & E-administration.</span><span class="sxs-lookup"><span data-stu-id="cfd42-134">For the following reasons, this action should not be done with the profile they use for normal T&E administration.</span></span>

* <span data-ttu-id="cfd42-135">Klienten måste vara det som klickar på ”*Ja*” i dialog-fönstret som visas när en app är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="cfd42-135">The client has to be the one that clicks "*Yes*" on the dialogue window that is displayed after an app is enabled.</span></span> <span data-ttu-id="cfd42-136">Detta klickar du på om klienten är villigt för programmets Partner att komma åt sina data så att du eller partnern inte kan klicka på knappen Ja.</span><span class="sxs-lookup"><span data-stu-id="cfd42-136">This click acknowledges the client is willing for the Partner application to access their data, so you or the Partner cannot click that Yes button.</span></span>

* <span data-ttu-id="cfd42-137">Om en administratör som har aktiverat en app lämnar med T & E administratör profil företaget (vilket ger den profil som inaktiverat), appar aktiverad med profil inte fungerar förrän appen aktiveras med en annan active WS Admin-profil.</span><span class="sxs-lookup"><span data-stu-id="cfd42-137">If a client administrator that has enabled an app using the T&E admin profile leaves the company (resulting in the profile being inactivated), any apps enabled using that profile does not function until the app is enabled with another active WS Admin profile.</span></span> <span data-ttu-id="cfd42-138">Det är därför du ska skapa distinkta WS-Admin-profiler.</span><span class="sxs-lookup"><span data-stu-id="cfd42-138">This is why you are supposed to create distinct WS Admin profiles.</span></span>

* <span data-ttu-id="cfd42-139">Om en administratör lämnar företaget, kan namnet som är kopplat till WS-Admin-profil ändras till ersättning-administratören om du vill utan att påverka aktiverade appen eftersom den här profilen inte behöver inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="cfd42-139">If an administrator leaves the company, the name associated to the WS Admin profile can be changed to the replacement administrator if desired without impacting the enabled app because that profile does not need inactivated.</span></span>

<span data-ttu-id="cfd42-140">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="cfd42-140">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="cfd42-141">Logga in på ditt **Concur** klient.</span><span class="sxs-lookup"><span data-stu-id="cfd42-141">Log on to your **Concur** tenant.</span></span>

2. <span data-ttu-id="cfd42-142">Från den **Administration** väljer du **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="cfd42-142">From the **Administration** menu, select **Web Services**.</span></span>
   
    <span data-ttu-id="cfd42-143">![Concur klient](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur klient")</span><span class="sxs-lookup"><span data-stu-id="cfd42-143">![Concur tenant](./media/active-directory-saas-concur-provisioning-tutorial/IC721729.png "Concur tenant")</span></span>

3. <span data-ttu-id="cfd42-144">På vänster sida från den **Web Services** väljer **aktivera Partner program**.</span><span class="sxs-lookup"><span data-stu-id="cfd42-144">On the left side, from the **Web Services** pane, select **Enable Partner Application**.</span></span>
   
    <span data-ttu-id="cfd42-145">![Aktivera Partner program](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "gör att Partner-program")</span><span class="sxs-lookup"><span data-stu-id="cfd42-145">![Enable Partner Application](./media/active-directory-saas-concur-provisioning-tutorial/ic721730.png "Enable Partner Application")</span></span>

4. <span data-ttu-id="cfd42-146">Från den **aktivera programmet** väljer **Azure Active Directory**, och klicka sedan på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="cfd42-146">From the **Enable Application** list, select **Azure Active Directory**, and then click **Enable**.</span></span>
   
    <span data-ttu-id="cfd42-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span><span class="sxs-lookup"><span data-stu-id="cfd42-147">![Microsoft Azure Active Directory](./media/active-directory-saas-concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")</span></span>

5. <span data-ttu-id="cfd42-148">Klicka på **Ja** att stänga den **bekräfta åtgärden** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfd42-148">Click **Yes** to close the **Confirm Action** dialog.</span></span>
   
    <span data-ttu-id="cfd42-149">![Bekräfta åtgärden](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "bekräfta åtgärden")</span><span class="sxs-lookup"><span data-stu-id="cfd42-149">![Confirm Action](./media/active-directory-saas-concur-provisioning-tutorial/ic721732.png "Confirm Action")</span></span>

6. <span data-ttu-id="cfd42-150">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cfd42-150">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

7. <span data-ttu-id="cfd42-151">Om du redan har konfigurerat Concur för enkel inloggning, söka efter din instans av Concur med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="cfd42-151">If you have already configured Concur for single sign-on, search for your instance of Concur using the search field.</span></span> <span data-ttu-id="cfd42-152">Annars väljer **Lägg till** och Sök efter **Concur** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="cfd42-152">Otherwise, select **Add** and search for **Concur** in the application gallery.</span></span> <span data-ttu-id="cfd42-153">Välj Concur i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="cfd42-153">Select Concur from the search results, and add it to your list of applications.</span></span>

8. <span data-ttu-id="cfd42-154">Välj din instans av Concur och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="cfd42-154">Select your instance of Concur, then select the **Provisioning** tab.</span></span>

9. <span data-ttu-id="cfd42-155">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="cfd42-155">Set the **Provisioning Mode** to **Automatic**.</span></span> 
 
    ![Etablering](./media/active-directory-saas-concur-provisioning-tutorial/provisioning.png)

10. <span data-ttu-id="cfd42-157">Under den **administratörsautentiseringsuppgifter** ange den **användarnamn** och **lösenord** av administratören Concur.</span><span class="sxs-lookup"><span data-stu-id="cfd42-157">Under the **Admin Credentials** section, enter the **user name** and the **password** of your Concur administrator.</span></span>

11. <span data-ttu-id="cfd42-158">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din Concur app.</span><span class="sxs-lookup"><span data-stu-id="cfd42-158">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Concur app.</span></span> <span data-ttu-id="cfd42-159">Om anslutningen misslyckas, kontrollera kontots Concur har teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="cfd42-159">If the connection fails, ensure your Concur account has Team Admin permissions.</span></span>

12. <span data-ttu-id="cfd42-160">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="cfd42-160">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

13. <span data-ttu-id="cfd42-161">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="cfd42-161">Click **Save.**</span></span>

14. <span data-ttu-id="cfd42-162">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare Concur.**</span><span class="sxs-lookup"><span data-stu-id="cfd42-162">Under the Mappings section, select **Synchronize Azure Active Directory Users to Concur.**</span></span>

15. <span data-ttu-id="cfd42-163">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Concur.</span><span class="sxs-lookup"><span data-stu-id="cfd42-163">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Concur.</span></span> <span data-ttu-id="cfd42-164">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Concur för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="cfd42-164">The attributes selected as **Matching** properties are used to match the user accounts in Concur for update operations.</span></span> <span data-ttu-id="cfd42-165">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="cfd42-165">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="cfd42-166">Om du vill aktivera Azure AD-tjänsten för Concur-etablering, ändra den **Status för etablering** till **på** i den **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="cfd42-166">To enable the Azure AD provisioning service for Concur, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

17. <span data-ttu-id="cfd42-167">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="cfd42-167">Click **Save.**</span></span>

<span data-ttu-id="cfd42-168">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="cfd42-168">You can now create a test account.</span></span> <span data-ttu-id="cfd42-169">Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till Concur.</span><span class="sxs-lookup"><span data-stu-id="cfd42-169">Wait for up to 20 minutes to verify that the account has been synchronized to Concur.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cfd42-170">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cfd42-170">Additional resources</span></span>

* [<span data-ttu-id="cfd42-171">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="cfd42-171">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cfd42-172">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cfd42-172">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="cfd42-173">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfd42-173">Configure Single Sign-on</span></span>](active-directory-saas-concur-tutorial.md)

