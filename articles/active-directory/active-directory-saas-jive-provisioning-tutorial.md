---
title: "Självstudier: Azure Active Directory-integrering med Jive | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 957b152fdd40d08a867e788b0cb9f7d57ed481e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a><span data-ttu-id="8a55e-103">Självstudier: Konfigurera Jive för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="8a55e-103">Tutorial: Configuring Jive for User Provisioning</span></span>

<span data-ttu-id="8a55e-104">Syftet med den här kursen är att visa de steg som du behöver göra i Jive och Azure AD till automatiskt etablera och avinstallation etablera användarkonton från Azure AD till Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-104">The objective of this tutorial is to show you the steps you need to perform in Jive and Azure AD to automatically provision and de-provision user accounts from Azure AD to Jive.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8a55e-105">Krav</span><span class="sxs-lookup"><span data-stu-id="8a55e-105">Prerequisites</span></span>

<span data-ttu-id="8a55e-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8a55e-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="8a55e-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="8a55e-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="8a55e-108">En Jive enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="8a55e-108">A Jive single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="8a55e-109">Ett användarkonto i Jive Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="8a55e-109">A user account in Jive with Team Admin permissions.</span></span>

## <a name="assigning-users-to-jive"></a><span data-ttu-id="8a55e-110">Tilldela användare till Jive</span><span class="sxs-lookup"><span data-stu-id="8a55e-110">Assigning users to Jive</span></span>

<span data-ttu-id="8a55e-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="8a55e-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="8a55e-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a55e-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="8a55e-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till appen Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Jive app.</span></span> <span data-ttu-id="8a55e-114">När bestämt, kan du tilldela dessa användare i appen Jive genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="8a55e-114">Once decided, you can assign these users to your Jive app by following the instructions here:</span></span>

[<span data-ttu-id="8a55e-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="8a55e-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a><span data-ttu-id="8a55e-116">Viktiga tips för att tilldela användare till Jive</span><span class="sxs-lookup"><span data-stu-id="8a55e-116">Important tips for assigning users to Jive</span></span>

*   <span data-ttu-id="8a55e-117">Vi rekommenderar att en enda Azure AD-användare tilldelas Jive att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="8a55e-117">It is recommended that a single Azure AD user be assigned to Jive to test the provisioning configuration.</span></span> <span data-ttu-id="8a55e-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="8a55e-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="8a55e-119">När du tilldelar en användare Jive, måste du välja en giltig användarroll.</span><span class="sxs-lookup"><span data-stu-id="8a55e-119">When assigning a user to Jive, you must select a valid user role.</span></span> <span data-ttu-id="8a55e-120">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="8a55e-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="8a55e-121">Aktivera etablering av användare</span><span class="sxs-lookup"><span data-stu-id="8a55e-121">Enable User Provisioning</span></span>

<span data-ttu-id="8a55e-122">Det här avsnittet hjälper dig att ansluta din Azure AD till Jives användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i Jive baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8a55e-122">This section guides you through connecting your Azure AD to Jive's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Jive based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="8a55e-123">Du kan också välja att aktivera SAML-baserade enkel inloggning för Jive, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8a55e-123">You may also choose to enabled SAML-based Single Sign-On for Jive, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8a55e-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="8a55e-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning"></a><span data-ttu-id="8a55e-125">Konfigurera användaretablering för kontot:</span><span class="sxs-lookup"><span data-stu-id="8a55e-125">To configure user account provisioning:</span></span>

<span data-ttu-id="8a55e-126">Syftet med det här avsnittet är att beskriva hur du aktiverar användaretablering för Active Directory-användarkonton till Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-126">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Jive.</span></span>
<span data-ttu-id="8a55e-127">Som en del av den här proceduren måste måste du tillhandahålla en säkerhetstoken för användare som behöver begära från Jive.com.</span><span class="sxs-lookup"><span data-stu-id="8a55e-127">As part of this procedure, you are required to provide a user security token you need to request from Jive.com.</span></span>

1. <span data-ttu-id="8a55e-128">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8a55e-128">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="8a55e-129">Om du redan har konfigurerat Jive för enkel inloggning, söka efter din instans av Jive med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="8a55e-129">If you have already configured Jive for single sign-on, search for your instance of Jive using the search field.</span></span> <span data-ttu-id="8a55e-130">Annars väljer **Lägg till** och Sök efter **Jive** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="8a55e-130">Otherwise, select **Add** and search for **Jive** in the application gallery.</span></span> <span data-ttu-id="8a55e-131">Välj Jive i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="8a55e-131">Select Jive from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="8a55e-132">Välj din instans av Jive och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="8a55e-132">Select your instance of Jive, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="8a55e-133">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="8a55e-133">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="8a55e-135">Under den **administratörsautentiseringsuppgifter** och ange följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="8a55e-135">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="8a55e-136">a.</span><span class="sxs-lookup"><span data-stu-id="8a55e-136">a.</span></span> <span data-ttu-id="8a55e-137">I den **Jive administratörsanvändarnamnet** textruta typen en Jive kontonamn som har den **systemadministratören** profil i Jive.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="8a55e-137">In the **Jive Admin User Name** textbox, type a Jive account name that has the **System Administrator** profile in Jive.com assigned.</span></span>
   
    <span data-ttu-id="8a55e-138">b.</span><span class="sxs-lookup"><span data-stu-id="8a55e-138">b.</span></span> <span data-ttu-id="8a55e-139">I den **Jive adminlösenord** textruta skriver du lösenordet för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="8a55e-139">In the **Jive Admin Password** textbox, type the password for this account.</span></span>
   
    <span data-ttu-id="8a55e-140">c.</span><span class="sxs-lookup"><span data-stu-id="8a55e-140">c.</span></span> <span data-ttu-id="8a55e-141">I den **Jive klient URL** textruta anger Jive klient-URL.</span><span class="sxs-lookup"><span data-stu-id="8a55e-141">In the **Jive Tenant URL** textbox, type the Jive tenant URL.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="8a55e-142">Jive klient-URL är URL som används av din organisation för att logga in på Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-142">The Jive tenant URL is URL that is used by your organization to log in to Jive.</span></span>  
      > <span data-ttu-id="8a55e-143">Normalt URL har följande format: **www.\< organisation\>. jive.com**.</span><span class="sxs-lookup"><span data-stu-id="8a55e-143">Typically, the URL has the following format: **www.\<organization\>.jive.com**.</span></span>          

6. <span data-ttu-id="8a55e-144">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din Jive app.</span><span class="sxs-lookup"><span data-stu-id="8a55e-144">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Jive app.</span></span>

7. <span data-ttu-id="8a55e-145">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan nedan.</span><span class="sxs-lookup"><span data-stu-id="8a55e-145">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

8. <span data-ttu-id="8a55e-146">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="8a55e-146">Click **Save.**</span></span>

9. <span data-ttu-id="8a55e-147">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare Jive.**</span><span class="sxs-lookup"><span data-stu-id="8a55e-147">Under the Mappings section, select **Synchronize Azure Active Directory Users to Jive.**</span></span>

10. <span data-ttu-id="8a55e-148">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-148">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Jive.</span></span> <span data-ttu-id="8a55e-149">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Jive för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="8a55e-149">The attributes selected as **Matching** properties are used to match the user accounts in Jive for update operations.</span></span> <span data-ttu-id="8a55e-150">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8a55e-150">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="8a55e-151">Om du vill aktivera Azure AD-tjänsten för Jive-etablering, ändra den **Status för etablering** till **på** i avsnittet Inställningar</span><span class="sxs-lookup"><span data-stu-id="8a55e-151">To enable the Azure AD provisioning service for Jive, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="8a55e-152">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="8a55e-152">Click **Save.**</span></span>

<span data-ttu-id="8a55e-153">Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Jive i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="8a55e-153">It starts the initial synchronization of any users and/or groups assigned to Jive in the Users and Groups section.</span></span> <span data-ttu-id="8a55e-154">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="8a55e-154">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="8a55e-155">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering i appen Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-155">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Jive app.</span></span>

<span data-ttu-id="8a55e-156">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="8a55e-156">You can now create a test account.</span></span> <span data-ttu-id="8a55e-157">Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till Jive.</span><span class="sxs-lookup"><span data-stu-id="8a55e-157">Wait for up to 20 minutes to verify that the account has been synchronized to Jive.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a55e-158">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8a55e-158">Additional resources</span></span>

* [<span data-ttu-id="8a55e-159">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="8a55e-159">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8a55e-160">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8a55e-160">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="8a55e-161">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8a55e-161">Configure Single Sign-on</span></span>](active-directory-saas-jive-tutorial.md)