---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Salesforce Sandbox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 7d3c655a754f83284c386d2007c604a731367814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a><span data-ttu-id="308a9-103">Självstudier: Konfigurera Salesforce Sandbox för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="308a9-103">Tutorial: Configuring Salesforce Sandbox for Automatic User Provisioning</span></span>

<span data-ttu-id="308a9-104">Syftet med den här kursen är att visa de steg som du behöver göra i Salesforce Sandbox och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="308a9-104">The objective of this tutorial is to show you the steps you need to perform in Salesforce Sandbox and Azure AD to automatically provision and de-provision user accounts from Azure AD to Salesforce Sandbox.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="308a9-105">Krav</span><span class="sxs-lookup"><span data-stu-id="308a9-105">Prerequisites</span></span>

<span data-ttu-id="308a9-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="308a9-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="308a9-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="308a9-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="308a9-108">Du måste ha en giltig klient för Salesforce Sandbox för arbete eller Salesforce Sandbox för utbildning.</span><span class="sxs-lookup"><span data-stu-id="308a9-108">You must have a valid tenant for Salesforce Sandbox for Work or Salesforce Sandbox for Education.</span></span> <span data-ttu-id="308a9-109">Du kan använda ett kostnadsfritt utvärderingskonto för antingen service.</span><span class="sxs-lookup"><span data-stu-id="308a9-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="308a9-110">Ett användarkonto i Salesforce Sandbox Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="308a9-110">A user account in Salesforce Sandbox with Team Admin permissions.</span></span>

## <a name="assigning-users-to-salesforce-sandbox"></a><span data-ttu-id="308a9-111">Tilldela användare till Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="308a9-111">Assigning users to Salesforce Sandbox</span></span>

<span data-ttu-id="308a9-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="308a9-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="308a9-113">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="308a9-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="308a9-114">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till ditt Salesforce-Sandbox-app.</span><span class="sxs-lookup"><span data-stu-id="308a9-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Salesforce Sandbox app.</span></span> <span data-ttu-id="308a9-115">När du valt, kan du tilldela dessa användare Salesforce Sandbox appen genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="308a9-115">Once decided, you can assign these users to your Salesforce Sandbox app by following the instructions here:</span></span>

[<span data-ttu-id="308a9-116">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="308a9-116">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a><span data-ttu-id="308a9-117">Viktiga tips för att tilldela användare till Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="308a9-117">Important tips for assigning users to Salesforce Sandbox</span></span>

* <span data-ttu-id="308a9-118">Vi rekommenderar att en enda Azure AD-användare har tilldelats till Salesforce Sandbox testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="308a9-118">It is recommended that a single Azure AD user is assigned to Salesforce Sandbox to test the provisioning configuration.</span></span> <span data-ttu-id="308a9-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="308a9-119">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="308a9-120">När du tilldelar en användare Salesforce Sandbox, måste du välja en giltig användarroll.</span><span class="sxs-lookup"><span data-stu-id="308a9-120">When assigning a user to Salesforce Sandbox, you must select a valid user role.</span></span> <span data-ttu-id="308a9-121">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="308a9-121">The "Default Access" role does not work for provisioning.</span></span>

> [!NOTE]
> <span data-ttu-id="308a9-122">Den här appen importerar anpassade roller från Salesforce Sandbox som en del av etableringsprocessen som kunden kanske du väljer när du tilldelar användare.</span><span class="sxs-lookup"><span data-stu-id="308a9-122">This app imports custom roles from Salesforce Sandbox as part of the provisioning process, which the customer may want to select when assigning users.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="308a9-123">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="308a9-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="308a9-124">Det här avsnittet hjälper dig att ansluta din Azure AD till Salesforce Sandbox användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelad användare konton i Salesforce Sandbox baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="308a9-124">This section guides you through connecting your Azure AD to Salesforce Sandbox's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Salesforce Sandbox based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="308a9-125">Du kan också välja att aktivera SAML-baserade enkel inloggning för Salesforce begränsat läge, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="308a9-125">You may also choose to enabled SAML-based Single Sign-On for Salesforce Sandbox, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="308a9-126">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="308a9-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="308a9-127">Konfigurera automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="308a9-127">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="308a9-128">Syftet med det här avsnittet är att beskriva hur du aktiverar användaretablering för Active Directory-användarkonton till Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="308a9-128">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to Salesforce Sandbox.</span></span>

1. <span data-ttu-id="308a9-129">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="308a9-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="308a9-130">Om du redan har konfigurerat Salesforce Sandbox för enkel inloggning söka efter din instans av Salesforce Sandbox med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="308a9-130">If you have already configured Salesforce Sandbox for single sign-on, search for your instance of Salesforce Sandbox using the search field.</span></span> <span data-ttu-id="308a9-131">Annars väljer **Lägg till** och Sök efter **Salesforce Sandbox** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="308a9-131">Otherwise, select **Add** and search for **Salesforce Sandbox** in the application gallery.</span></span> <span data-ttu-id="308a9-132">Välj Salesforce Sandbox i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="308a9-132">Select Salesforce Sandbox from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="308a9-133">Välj din instans av Salesforce begränsat, och välj sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="308a9-133">Select your instance of Salesforce Sandbox, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="308a9-134">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="308a9-134">Set the **Provisioning Mode** to **Automatic**.</span></span> 
    <span data-ttu-id="308a9-135">![etablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="308a9-135">![provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="308a9-136">Under den **administratörsautentiseringsuppgifter** och ange följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="308a9-136">Under the **Admin Credentials** section, provide the following configuration settings:</span></span>
   
    <span data-ttu-id="308a9-137">a.</span><span class="sxs-lookup"><span data-stu-id="308a9-137">a.</span></span> <span data-ttu-id="308a9-138">I den **administratörsanvändarnamnet** textruta typen en Salesforce Sandbox kontonamn som har den **systemadministratören** profil i Salesforce.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="308a9-138">In the **Admin User Name** textbox, type a Salesforce Sandbox account name that has the **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="308a9-139">b.</span><span class="sxs-lookup"><span data-stu-id="308a9-139">b.</span></span> <span data-ttu-id="308a9-140">I den **adminlösenord** textruta skriver du lösenordet för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="308a9-140">In the **Admin Password** textbox, type the password for this account.</span></span>

6. <span data-ttu-id="308a9-141">Om du vill hämta dina Salesforce Sandbox säkerhetstoken, öppnar du en ny flik och logga i samma Salesforce Sandbox-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="308a9-141">To get your Salesforce Sandbox security token, open a new tab and sign into the same Salesforce Sandbox admin account.</span></span> <span data-ttu-id="308a9-142">Klicka på ditt namn i det övre högra hörnet på sidan och klicka sedan på **Mina inställningar**.</span><span class="sxs-lookup"><span data-stu-id="308a9-142">On the top right corner of the page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="308a9-143">![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="308a9-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="308a9-144">I det vänstra navigeringsfönstret klickar du på **personliga** Expandera avsnittet relaterade och klicka sedan på **Återställ mina säkerhetstoken**.</span><span class="sxs-lookup"><span data-stu-id="308a9-144">On the left navigation pane, click **Personal** to expand the related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="308a9-145">![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="308a9-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="308a9-146">På den **Återställ mina säkerhetstoken** klickar du på den **återställa säkerhetstoken** knappen.</span><span class="sxs-lookup"><span data-stu-id="308a9-146">On the **Reset My Security Token** page, click the **Reset Security Token** button.</span></span>

    <span data-ttu-id="308a9-147">![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="308a9-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="308a9-148">Kontrollera den inkorg som är associerade med den här administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="308a9-148">Check the email inbox associated with this admin account.</span></span> <span data-ttu-id="308a9-149">Leta efter ett e-postmeddelande från Salesforce-Sandbox.com som innehåller ny säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="308a9-149">Look for an email from Salesforce Sandbox.com that contains the new security token.</span></span>
10. <span data-ttu-id="308a9-150">Kopiera token, gå till Azure AD-fönstret och klistrar in det i den **Socket Token** fältet.</span><span class="sxs-lookup"><span data-stu-id="308a9-150">Copy the token, go to your Azure AD window, and paste it into the **Socket Token** field.</span></span>

11. <span data-ttu-id="308a9-151">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till ditt Salesforce-Sandbox-app.</span><span class="sxs-lookup"><span data-stu-id="308a9-151">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Salesforce Sandbox app.</span></span>

12. <span data-ttu-id="308a9-152">I den **e-postmeddelande** anger du den e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="308a9-152">In the **Notification Email** field, enter the email address of a person or group who should receive provisioning error notifications, and check the checkbox.</span></span>

13. <span data-ttu-id="308a9-153">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="308a9-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="308a9-154">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare Salesforce Sandbox.**</span><span class="sxs-lookup"><span data-stu-id="308a9-154">Under the Mappings section, select **Synchronize Azure Active Directory Users to Salesforce Sandbox.**</span></span>

15. <span data-ttu-id="308a9-155">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="308a9-155">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Salesforce Sandbox.</span></span> <span data-ttu-id="308a9-156">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Salesforce Sandbox för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="308a9-156">The attributes selected as **Matching** properties are used to match the user accounts in Salesforce Sandbox for update operations.</span></span> <span data-ttu-id="308a9-157">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="308a9-157">Select the Save button to commit any changes.</span></span>

16. <span data-ttu-id="308a9-158">Om du vill aktivera Azure AD etableras för Salesforce begränsat läge, ändra den **Status för etablering** till **på** i avsnittet Inställningar</span><span class="sxs-lookup"><span data-stu-id="308a9-158">To enable the Azure AD provisioning service for Salesforce Sandbox, change the **Provisioning Status** to **On** in the Settings section</span></span>

17. <span data-ttu-id="308a9-159">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="308a9-159">Click **Save.**</span></span>


<span data-ttu-id="308a9-160">Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Salesforce Sandbox i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="308a9-160">It starts the initial synchronization of any users and/or groups assigned to Salesforce Sandbox in the Users and Groups section.</span></span> <span data-ttu-id="308a9-161">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="308a9-161">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="308a9-162">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering på Salesforce Sandbox app.</span><span class="sxs-lookup"><span data-stu-id="308a9-162">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on Salesforce Sandbox app.</span></span>

<span data-ttu-id="308a9-163">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="308a9-163">You can now create a test account.</span></span> <span data-ttu-id="308a9-164">Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till salesforce.</span><span class="sxs-lookup"><span data-stu-id="308a9-164">Wait for up to 20 minutes to verify that the account has been synchronized to salesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="308a9-165">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="308a9-165">Additional resources</span></span>

* [<span data-ttu-id="308a9-166">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="308a9-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="308a9-167">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="308a9-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="308a9-168">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="308a9-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforcesandbox-tutorial.md)