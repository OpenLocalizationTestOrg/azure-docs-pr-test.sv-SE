---
title: "Självstudier: Azure Active Directory-integrering med Netsuite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 277c393536615fc8bfe8af0bc6d487115f04776c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a><span data-ttu-id="56436-103">Självstudier: Konfigurera Netsuite för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="56436-103">Tutorial: Configuring Netsuite for Automatic User Provisioning</span></span>

<span data-ttu-id="56436-104">Syftet med den här kursen är att visa de steg som du behöver göra i Netsuite och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till Netsuite.</span><span class="sxs-lookup"><span data-stu-id="56436-104">The objective of this tutorial is to show you the steps you need to perform in Netsuite and Azure AD to automatically provision and de-provision user accounts from Azure AD to Netsuite.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56436-105">Krav</span><span class="sxs-lookup"><span data-stu-id="56436-105">Prerequisites</span></span>

<span data-ttu-id="56436-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="56436-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="56436-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="56436-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="56436-108">En Netsuite enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="56436-108">A Netsuite single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="56436-109">Ett användarkonto i Netsuite Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="56436-109">A user account in Netsuite with Team Admin permissions.</span></span>

## <a name="assigning-users-to-netsuite"></a><span data-ttu-id="56436-110">Tilldela användare till Netsuite</span><span class="sxs-lookup"><span data-stu-id="56436-110">Assigning users to Netsuite</span></span>

<span data-ttu-id="56436-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="56436-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="56436-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56436-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="56436-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till appen Netsuite.</span><span class="sxs-lookup"><span data-stu-id="56436-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Netsuite app.</span></span> <span data-ttu-id="56436-114">När bestämt, kan du tilldela dessa användare i appen Netsuite genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="56436-114">Once decided, you can assign these users to your Netsuite app by following the instructions here:</span></span>

[<span data-ttu-id="56436-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="56436-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite"></a><span data-ttu-id="56436-116">Viktiga tips för att tilldela användare till Netsuite</span><span class="sxs-lookup"><span data-stu-id="56436-116">Important tips for assigning users to Netsuite</span></span>

*   <span data-ttu-id="56436-117">Vi rekommenderar att en enda Azure AD-användare har tilldelats Netsuite för att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="56436-117">It is recommended that a single Azure AD user is assigned to Netsuite to test the provisioning configuration.</span></span> <span data-ttu-id="56436-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="56436-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="56436-119">När du tilldelar en användare Netsuite, måste du välja en giltig användarroll.</span><span class="sxs-lookup"><span data-stu-id="56436-119">When assigning a user to Netsuite, you must select a valid user role.</span></span> <span data-ttu-id="56436-120">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="56436-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="56436-121">Aktivera etablering av användare</span><span class="sxs-lookup"><span data-stu-id="56436-121">Enable User Provisioning</span></span>

<span data-ttu-id="56436-122">Det här avsnittet hjälper dig att ansluta din Azure AD till Netsuites användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i Netsuite baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="56436-122">This section guides you through connecting your Azure AD to Netsuite's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Netsuite based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="56436-123">Du kan också välja att aktivera SAML-baserade enkel inloggning för Netsuite, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="56436-123">You may also choose to enabled SAML-based Single Sign-On for Netsuite, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="56436-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="56436-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="56436-125">Konfigurera användaretablering för kontot:</span><span class="sxs-lookup"><span data-stu-id="56436-125">To configure user account provisioning:</span></span>

<span data-ttu-id="56436-126">Syftet med det här avsnittet är att beskriva hur du aktiverar användaretablering för Active Directory-användarkonton till Netsuite.</span><span class="sxs-lookup"><span data-stu-id="56436-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Netsuite.</span></span>

1. <span data-ttu-id="56436-127">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="56436-127">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="56436-128">Om du redan har konfigurerat Netsuite för enkel inloggning, söka efter din instans av Netsuite med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="56436-128">If you have already configured Netsuite for single sign-on, search for your instance of Netsuite using the search field.</span></span> <span data-ttu-id="56436-129">Annars väljer **Lägg till** och Sök efter **Netsuite** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="56436-129">Otherwise, select **Add** and search for **Netsuite** in the application gallery.</span></span> <span data-ttu-id="56436-130">Välj Netsuite i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="56436-130">Select Netsuite from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="56436-131">Välj din instans av Netsuite och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="56436-131">Select your instance of Netsuite, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="56436-132">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="56436-132">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-netsuite-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="56436-134">Under den **administratörsautentiseringsuppgifter** och ange följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="56436-134">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="56436-135">a.</span><span class="sxs-lookup"><span data-stu-id="56436-135">a.</span></span> <span data-ttu-id="56436-136">I den **administratörsanvändarnamnet** textruta typen en Netsuite kontonamn som har den **systemadministratören** profil i Netsuite.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="56436-136">In the **Admin User Name** textbox, type a Netsuite account name that has the **System Administrator** profile in Netsuite.com assigned.</span></span>
   
    <span data-ttu-id="56436-137">b.</span><span class="sxs-lookup"><span data-stu-id="56436-137">b.</span></span> <span data-ttu-id="56436-138">I den **adminlösenord** textruta skriver du lösenordet för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="56436-138">In the **Admin Password** textbox, type the password for this account.</span></span>
      
6. <span data-ttu-id="56436-139">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din Netsuite app.</span><span class="sxs-lookup"><span data-stu-id="56436-139">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Netsuite app.</span></span>

7. <span data-ttu-id="56436-140">I den **e-postmeddelande** anger du den e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="56436-140">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

8. <span data-ttu-id="56436-141">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="56436-141">Click **Save.**</span></span>

9. <span data-ttu-id="56436-142">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare Netsuite.**</span><span class="sxs-lookup"><span data-stu-id="56436-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to Netsuite.**</span></span>

10. <span data-ttu-id="56436-143">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Netsuite.</span><span class="sxs-lookup"><span data-stu-id="56436-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Netsuite.</span></span> <span data-ttu-id="56436-144">Observera att attribut som är markerade som **matchande** egenskaper som används för att matcha användarkonton i Netsuite för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="56436-144">Note that the attributes selected as **Matching** properties are used to match the user accounts in Netsuite for update operations.</span></span> <span data-ttu-id="56436-145">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="56436-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="56436-146">Om du vill aktivera Azure AD-tjänsten för Netsuite-etablering, ändra den **Status för etablering** till **på** i avsnittet Inställningar</span><span class="sxs-lookup"><span data-stu-id="56436-146">To enable the Azure AD provisioning service for Netsuite, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="56436-147">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="56436-147">Click **Save.**</span></span>

<span data-ttu-id="56436-148">Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Netsuite i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="56436-148">It starts the initial synchronization of any users and/or groups assigned to Netsuite in the Users and Groups section.</span></span> <span data-ttu-id="56436-149">Observera att den första synkroniseringen tar längre tid att utföra än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="56436-149">Note that the initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="56436-150">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering i appen Netsuite.</span><span class="sxs-lookup"><span data-stu-id="56436-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Netsuite app.</span></span>

<span data-ttu-id="56436-151">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="56436-151">You can now create a test account.</span></span> <span data-ttu-id="56436-152">Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till Netsuite.</span><span class="sxs-lookup"><span data-stu-id="56436-152">Wait for up to 20 minutes to verify that the account has been synchronized to Netsuite.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56436-153">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="56436-153">Additional resources</span></span>

* [<span data-ttu-id="56436-154">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="56436-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="56436-155">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="56436-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="56436-156">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="56436-156">Configure Single Sign-on</span></span>](active-directory-saas-netsuite-tutorial.md)