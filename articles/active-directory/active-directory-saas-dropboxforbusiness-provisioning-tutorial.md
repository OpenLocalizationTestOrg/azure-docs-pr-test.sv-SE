---
title: "Självstudier: Azure Active Directory-integrering med Dropbox för företag | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Dropbox för företag."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 6f7616e47322242f01a13d763f71c93d4ac06a92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a><span data-ttu-id="a8c2b-103">Självstudier: Konfigurera Dropbox för företag för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="a8c2b-103">Tutorial: Configuring Dropbox for Business for Automatic User Provisioning</span></span>

<span data-ttu-id="a8c2b-104">Syftet med den här kursen är att visa de steg som du behöver göra i Dropbox för företag och Azure AD att automatiskt etablera och avetablera användarkonton från Azure AD till Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-104">The objective of this tutorial is to show you the steps you need to perform in Dropbox for Business and Azure AD to automatically provision and de-provision user accounts from Azure AD to Dropbox for Business.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8c2b-105">Krav</span><span class="sxs-lookup"><span data-stu-id="a8c2b-105">Prerequisites</span></span>

<span data-ttu-id="a8c2b-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a8c2b-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="a8c2b-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="a8c2b-108">En Dropbox för Business enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-108">A Dropbox for Business single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="a8c2b-109">Ett användarkonto i Dropbox för företag med Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-109">A user account in Dropbox for Business with Team Admin permissions.</span></span>

## <a name="assigning-users-to-dropbox-for-business"></a><span data-ttu-id="a8c2b-110">Tilldela användare till Dropbox för företag</span><span class="sxs-lookup"><span data-stu-id="a8c2b-110">Assigning users to Dropbox for Business</span></span>

<span data-ttu-id="a8c2b-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="a8c2b-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="a8c2b-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till din Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Dropbox for Business app.</span></span> <span data-ttu-id="a8c2b-114">När bestämt, kan du tilldela dessa användare till din Dropbox för företag genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="a8c2b-114">Once decided, you can assign these users to your Dropbox for Business app by following the instructions here:</span></span>

[<span data-ttu-id="a8c2b-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="a8c2b-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a><span data-ttu-id="a8c2b-116">Viktiga tips för att tilldela användare till Dropbox för företag</span><span class="sxs-lookup"><span data-stu-id="a8c2b-116">Important tips for assigning users to Dropbox for Business</span></span>

*   <span data-ttu-id="a8c2b-117">Vi rekommenderar att en enda Azure AD-användare är tilldelad till Dropbox för företag att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-117">It is recommended that a single Azure AD user is assigned to Dropbox for Business to test the provisioning configuration.</span></span> <span data-ttu-id="a8c2b-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="a8c2b-119">Du måste välja en giltig användarroll när du tilldelar en användare till Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-119">When assigning a user to Dropbox for Business, you must select a valid user role.</span></span> <span data-ttu-id="a8c2b-120">Rollen ”standard åtkomst” fungerar inte för att etablera...</span><span class="sxs-lookup"><span data-stu-id="a8c2b-120">The "Default Access" role does not work for provisioning..</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="a8c2b-121">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="a8c2b-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="a8c2b-122">Det här avsnittet hjälper dig att ansluta din Azure AD till Dropbox för företagets användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i Dropbox för företag baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-122">This section guides you through connecting your Azure AD to Dropbox for Business's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Dropbox for Business based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="a8c2b-123">Du kan också välja att aktivera SAML-baserade enkel inloggning för Dropbox för företag, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a8c2b-123">You may also choose to enabled SAML-based Single Sign-On for Dropbox for Business, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a8c2b-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="a8c2b-125">Konfigurera automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="a8c2b-125">To configure automatic user account provisioning:</span></span>

1. <span data-ttu-id="a8c2b-126">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="a8c2b-127">Om du redan har konfigurerat Dropbox för företag för enkel inloggning, söka efter din instans av Dropbox för företag med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-127">If you have already configured Dropbox for Business for single sign-on, search for your instance of Dropbox for Business using the search field.</span></span> <span data-ttu-id="a8c2b-128">Annars väljer **Lägg till** och Sök efter **Dropbox för företag** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-128">Otherwise, select **Add** and search for **Dropbox for Business** in the application gallery.</span></span> <span data-ttu-id="a8c2b-129">Välj Dropbox för företag i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-129">Select Dropbox for Business from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="a8c2b-130">Välj din instans av Dropbox för företag och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-130">Select your instance of Dropbox for Business, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="a8c2b-131">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-131">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="a8c2b-133">Under den **administratörsautentiseringsuppgifter** klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-133">Under the **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="a8c2b-134">En Dropbox för Business inloggningsruta öppnas i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-134">It opens a Dropbox for Business login dialog in a new browser window.</span></span>

6. <span data-ttu-id="a8c2b-135">På den **inloggning till Dropbox att länka med Azure AD** dialogrutan Logga in på din Dropbox för företag-klient.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-135">On the **Sign-in to Dropbox to link with Azure AD** dialog, sign in to your Dropbox for Business tenant.</span></span>

     <span data-ttu-id="a8c2b-136">![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "användaretablering")</span><span class="sxs-lookup"><span data-stu-id="a8c2b-136">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "User provisioning")</span></span>

7. <span data-ttu-id="a8c2b-137">Bekräfta att du vill ge Azure Active Directory behörighet att göra ändringar i din Dropbox för företag-klient.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-137">Confirm that you would like to give Azure Active Directory permission to make changes to your Dropbox for Business tenant.</span></span> <span data-ttu-id="a8c2b-138">Klicka på **Tillåt**.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-138">Click **Allow**.</span></span>
    
      <span data-ttu-id="a8c2b-139">![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "användaretablering")</span><span class="sxs-lookup"><span data-stu-id="a8c2b-139">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "User provisioning")</span></span>

8. <span data-ttu-id="a8c2b-140">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-140">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Dropbox for Business app.</span></span> <span data-ttu-id="a8c2b-141">Om anslutningen misslyckas, kontrollera din Dropbox för företag-konto som har administratörsbehörigheter för teamet och försök den **”Godkänn”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-141">If the connection fails, ensure your Dropbox for Business account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

9. <span data-ttu-id="a8c2b-142">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-142">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

10. <span data-ttu-id="a8c2b-143">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="a8c2b-143">Click **Save.**</span></span>

11. <span data-ttu-id="a8c2b-144">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till Dropbox för företag.**</span><span class="sxs-lookup"><span data-stu-id="a8c2b-144">Under the Mappings section, select **Synchronize Azure Active Directory Users to Dropbox for Business.**</span></span>

12. <span data-ttu-id="a8c2b-145">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-145">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Dropbox for Business.</span></span> <span data-ttu-id="a8c2b-146">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Dropbox för företag för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-146">The attributes selected as **Matching** properties are used to match the user accounts in Dropbox for Business for update operations.</span></span> <span data-ttu-id="a8c2b-147">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-147">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="a8c2b-148">Aktivera Azure AD etableras för Dropbox för företag att ändra den **Status för etablering** till **på** i avsnittet Inställningar</span><span class="sxs-lookup"><span data-stu-id="a8c2b-148">To enable the Azure AD provisioning service for Dropbox for Business, change the **Provisioning Status** to **On** in the Settings section</span></span>

14. <span data-ttu-id="a8c2b-149">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="a8c2b-149">Click **Save.**</span></span>

<span data-ttu-id="a8c2b-150">Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Dropbox för företag i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-150">It starts the initial synchronization of any users and/or groups assigned to Dropbox for Business in the Users and Groups section.</span></span> <span data-ttu-id="a8c2b-151">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-151">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="a8c2b-152">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av etablering tjänsten i din Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-152">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Dropbox for Business app.</span></span>

<span data-ttu-id="a8c2b-153">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-153">You can now create a test account.</span></span> <span data-ttu-id="a8c2b-154">Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-154">Wait for up to 20 minutes to verify that the account has been synchronized to Dropbox for Business.</span></span>

<span data-ttu-id="a8c2b-155">En användare som har slutförts etablering cykel visas med status relaterade.</span><span class="sxs-lookup"><span data-stu-id="a8c2b-155">A successfully completed user provisioning cycle is indicated by a related status.</span></span>

<span data-ttu-id="a8c2b-156">![Tilldela användare](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="a8c2b-156">![Assign users](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "Assign users")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="a8c2b-157">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a8c2b-157">Additional resources</span></span>

* [<span data-ttu-id="a8c2b-158">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="a8c2b-158">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8c2b-159">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8c2b-159">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="a8c2b-160">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8c2b-160">Configure Single Sign-on</span></span>](active-directory-saas-dropboxforbusiness-tutorial.md)