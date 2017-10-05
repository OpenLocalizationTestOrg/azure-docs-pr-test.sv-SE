---
title: "Självstudier: Azure Active Directory-integrering med | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och rutan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 9f061f3f5a0a4825854b893150ceccc8951487de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a><span data-ttu-id="c56fa-103">Självstudier: Konfigurera rutan för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="c56fa-103">Tutorial: Configuring Box for Automatic User Provisioning</span></span>

<span data-ttu-id="c56fa-104">Syftet med den här kursen är att visa steg som du behöver utföra i rutan och Azure AD att automatiskt etablera och avinstallation etablera användarkonton från Azure AD till Box.</span><span class="sxs-lookup"><span data-stu-id="c56fa-104">The objective of this tutorial is to show the steps you need to perform in Box and Azure AD to automatically provision and de-provision user accounts from Azure AD to Box.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c56fa-105">Krav</span><span class="sxs-lookup"><span data-stu-id="c56fa-105">Prerequisites</span></span>

<span data-ttu-id="c56fa-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c56fa-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="c56fa-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="c56fa-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="c56fa-108">En ruta enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c56fa-108">A Box single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="c56fa-109">Ett användarkonto i rutan Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="c56fa-109">A user account in Box with Team Admin permissions.</span></span>

## <a name="assigning-users-to-box"></a><span data-ttu-id="c56fa-110">Tilldela användare till rutan</span><span class="sxs-lookup"><span data-stu-id="c56fa-110">Assigning users to Box</span></span> 

<span data-ttu-id="c56fa-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="c56fa-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="c56fa-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c56fa-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="c56fa-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till Box-app.</span><span class="sxs-lookup"><span data-stu-id="c56fa-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Box app.</span></span> <span data-ttu-id="c56fa-114">När bestämt, kan du tilldela dessa användare till Box-app genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="c56fa-114">Once decided, you can assign these users to your Box app by following the instructions here:</span></span>

[<span data-ttu-id="c56fa-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="c56fa-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a><span data-ttu-id="c56fa-116">Tilldela användare och grupper</span><span class="sxs-lookup"><span data-stu-id="c56fa-116">Assign users and groups</span></span>
<span data-ttu-id="c56fa-117">Den **rutan > användare och grupper** fliken i Azure-portalen kan du ange vilka användare och grupper bör ha åtkomst till Box.</span><span class="sxs-lookup"><span data-stu-id="c56fa-117">The **Box > Users and Groups** tab in the Azure portal allows you to specify which users and groups should be granted access to Box.</span></span> <span data-ttu-id="c56fa-118">Tilldelning av en användare eller grupp gör följande inträffar:</span><span class="sxs-lookup"><span data-stu-id="c56fa-118">Assignment of a user or group causes the following things to occur:</span></span>

* <span data-ttu-id="c56fa-119">Azure AD tillåter den tilldelade användaren (antingen direkt av tilldelning av eller gruppmedlemskap) för att autentisera till rutan.</span><span class="sxs-lookup"><span data-stu-id="c56fa-119">Azure AD permits the assigned user (either by direct assignment or group membership) to authenticate to Box.</span></span> <span data-ttu-id="c56fa-120">Om en användare inte har tilldelats, Azure AD tillåter inte dem att logga in på rutan och returnerar ett fel på sidan för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c56fa-120">If a user is not assigned, then Azure AD does not permit them to sign in to Box and returns an error on the Azure AD sign-in page.</span></span>
* <span data-ttu-id="c56fa-121">En app-panelen för Box har lagts till i användarens [startprogrammet](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="c56fa-121">An app tile for Box is added to the user's [application launcher](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>
* <span data-ttu-id="c56fa-122">Om automatisk etablering aktiveras sedan läggs tilldelade användare och/eller grupper till köns etablering etableras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c56fa-122">If automatic provisioning is enabled, then the assigned users and/or groups are added to the provisioning queue to be automatically provisioned.</span></span>
  
  * <span data-ttu-id="c56fa-123">Om endast användarobjekt har konfigurerats för att etableras, sedan alla direkt tilldelade användare placeras i kön för etablering och alla användare som är medlemmar i de tilldelade grupperna placeras i kön för etablering.</span><span class="sxs-lookup"><span data-stu-id="c56fa-123">If only user objects were configured to be provisioned, then all directly assigned users are placed in the provisioning queue, and all users that are members of any assigned groups are placed in the provisioning queue.</span></span> 
  * <span data-ttu-id="c56fa-124">Om gruppobjekt konfigurerades för att etableras, etableras alla tilldelade objekt till och alla användare som är medlemmar i dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="c56fa-124">If group objects were configured to be provisioned, then all assigned group objects are provisioned to Box, and all users that are members of those groups.</span></span> <span data-ttu-id="c56fa-125">Grupp- och medlemskap bevaras vid skrivs till rutan.</span><span class="sxs-lookup"><span data-stu-id="c56fa-125">The group and user memberships are preserved upon being written to Box.</span></span>

<span data-ttu-id="c56fa-126">Du kan använda den **attribut > enkel inloggning** att konfigurera vilka användarattribut (eller anspråk) visas texten i under SAML-baserad autentisering och **attribut > etablering** att konfigurera hur användar- och Gruppattribut flöda från Azure AD till rutan under etableringen åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c56fa-126">You can use the **Attributes > Single Sign-On** tab to configure which user attributes (or claims) are presented to Box during SAML-based authentication, and the **Attributes > Provisioning** tab to configure how user and group attributes flow from Azure AD to Box during provisioning operations.</span></span>

### <a name="important-tips-for-assigning-users-to-box"></a><span data-ttu-id="c56fa-127">Viktiga tips för att tilldela användare till rutan</span><span class="sxs-lookup"><span data-stu-id="c56fa-127">Important tips for assigning users to Box</span></span> 

*   <span data-ttu-id="c56fa-128">Vi rekommenderar att en enda Azure AD-användare som tilldelats rutan för att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c56fa-128">It is recommended that a single Azure AD user assigned to Box to test the provisioning configuration.</span></span> <span data-ttu-id="c56fa-129">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="c56fa-129">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="c56fa-130">Du måste välja en giltig användarroll när du tilldelar du en användare.</span><span class="sxs-lookup"><span data-stu-id="c56fa-130">When assigning a user to box, you must select a valid user role.</span></span> <span data-ttu-id="c56fa-131">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="c56fa-131">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="c56fa-132">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="c56fa-132">Enable Automated User Provisioning</span></span>

<span data-ttu-id="c56fa-133">Det här avsnittet hjälper att ansluta din Azure AD till rutans användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i rutan utifrån tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c56fa-133">This section guides through connecting your Azure AD to Box's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Box based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="c56fa-134">Om automatisk etablering aktiveras sedan läggs tilldelade användare och/eller grupper till köns etablering etableras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c56fa-134">If automatic provisioning is enabled, then the assigned users and/or groups are added to the provisioning queue to be automatically provisioned.</span></span>
    
 * <span data-ttu-id="c56fa-135">Om endast användarobjekt är konfigurerade för att etableras, och sedan direkt tilldelade användare placeras i kön för etablering och alla användare som är medlemmar i de tilldelade grupperna placeras i kön för etablering.</span><span class="sxs-lookup"><span data-stu-id="c56fa-135">If only user objects are configured to be provisioned, then directly assigned users are placed in the provisioning queue, and all users that are members of any assigned groups are placed in the provisioning queue.</span></span> 
    
 * <span data-ttu-id="c56fa-136">Om gruppobjekt konfigurerades för att etableras, etableras alla tilldelade objekt till och alla användare som är medlemmar i dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="c56fa-136">If group objects were configured to be provisioned, then all assigned group objects are provisioned to Box, and all users that are members of those groups.</span></span> <span data-ttu-id="c56fa-137">Grupp- och medlemskap bevaras vid skrivs till rutan.</span><span class="sxs-lookup"><span data-stu-id="c56fa-137">The group and user memberships are preserved upon being written to Box.</span></span>

> [!TIP] 
> <span data-ttu-id="c56fa-138">Du kan också välja att aktivera SAML-baserade enkel inloggning för, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c56fa-138">You may also choose to enabled SAML-based Single Sign-On for Box, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c56fa-139">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="c56fa-139">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="c56fa-140">Konfigurera automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="c56fa-140">To configure automatic user account provisioning:</span></span>

<span data-ttu-id="c56fa-141">Syftet med det här avsnittet är att beskriva hur du aktiverar etablering av Active Directory-användarkonton till Box.</span><span class="sxs-lookup"><span data-stu-id="c56fa-141">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Box.</span></span>

1. <span data-ttu-id="c56fa-142">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c56fa-142">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="c56fa-143">Om du redan har konfigurerat rutan för enkel inloggning, söka efter din instans av rutan med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="c56fa-143">If you have already configured Box for single sign-on, search for your instance of Box using the search field.</span></span> <span data-ttu-id="c56fa-144">Annars väljer **Lägg till** och Sök efter **rutan** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="c56fa-144">Otherwise, select **Add** and search for **Box** in the application gallery.</span></span> <span data-ttu-id="c56fa-145">Markera kryssrutan i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="c56fa-145">Select Box from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="c56fa-146">Välj din instans av kryssrutan och välj sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="c56fa-146">Select your instance of Box, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="c56fa-147">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="c56fa-147">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. <span data-ttu-id="c56fa-149">Under den **administratörsautentiseringsuppgifter** klickar du på **auktorisera** att öppna en dialogruta för inloggning i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="c56fa-149">Under the **Admin Credentials** section, click **Authorize** to open a Box login dialog in a new browser window.</span></span>

6. <span data-ttu-id="c56fa-150">På den **inloggning att bevilja åtkomst till rutan** anger autentiseringsuppgifterna som krävs, och klicka sedan på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="c56fa-150">On the **Login to grant access to Box** page, provide the required credentials, and then click **Authorize**.</span></span> 
   
    <span data-ttu-id="c56fa-151">![Aktivera automatisk användaretablering](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="c56fa-151">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "Enable automatic user provisioning")</span></span>

7. <span data-ttu-id="c56fa-152">Klicka på **bevilja åtkomst till rutan** att godkänna den här åtgärden och återgå till den Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c56fa-152">Click **Grant access to Box** to authorize this operation and to return to the Azure portal.</span></span> 
   
    <span data-ttu-id="c56fa-153">![Aktivera automatisk användaretablering](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="c56fa-153">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "Enable automatic user provisioning")</span></span>

8. <span data-ttu-id="c56fa-154">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till Box-app.</span><span class="sxs-lookup"><span data-stu-id="c56fa-154">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Box app.</span></span> <span data-ttu-id="c56fa-155">Om anslutningen misslyckas, kontrollera Box-konto har administratörsbehörigheter för teamet och försök den **”Godkänn”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="c56fa-155">If the connection fails, ensure your Box account has Team Admin permissions and try the **"Authorize"** step again.</span></span>

9. <span data-ttu-id="c56fa-156">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="c56fa-156">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

10. <span data-ttu-id="c56fa-157">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="c56fa-157">Click **Save.**</span></span>

11. <span data-ttu-id="c56fa-158">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till Box.**</span><span class="sxs-lookup"><span data-stu-id="c56fa-158">Under the Mappings section, select **Synchronize Azure Active Directory Users to Box.**</span></span>

12. <span data-ttu-id="c56fa-159">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till rutan.</span><span class="sxs-lookup"><span data-stu-id="c56fa-159">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Box.</span></span> <span data-ttu-id="c56fa-160">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i rutan för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="c56fa-160">The attributes selected as **Matching** properties are used to match the user accounts in Box for update operations.</span></span> <span data-ttu-id="c56fa-161">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c56fa-161">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="c56fa-162">Aktivera Azure AD etableras för att ändra den **Status för etablering** till **på** i avsnittet Inställningar</span><span class="sxs-lookup"><span data-stu-id="c56fa-162">To enable the Azure AD provisioning service for Box, change the **Provisioning Status** to **On** in the Settings section</span></span>

14. <span data-ttu-id="c56fa-163">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="c56fa-163">Click **Save.**</span></span>

<span data-ttu-id="c56fa-164">Som startar den första synkroniseringen av användare och/eller grupper som tilldelas till rutan i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="c56fa-164">That starts the initial synchronization of any users and/or groups assigned to Box in the Users and Groups section.</span></span> <span data-ttu-id="c56fa-165">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="c56fa-165">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="c56fa-166">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering på Box-app.</span><span class="sxs-lookup"><span data-stu-id="c56fa-166">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Box app.</span></span>

<span data-ttu-id="c56fa-167">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="c56fa-167">You can now create a test account.</span></span> <span data-ttu-id="c56fa-168">Vänta i upp till 20 minuter för att verifiera att kontot har synkroniserats till box.</span><span class="sxs-lookup"><span data-stu-id="c56fa-168">Wait for up to 20 minutes to verify that the account has been synchronized to box.</span></span>

<span data-ttu-id="c56fa-169">I rutan-klient synkroniserade användare visas under **hanterade användare** i den **administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="c56fa-169">In your Box tenant, synchronized users are listed under **Managed Users** in the **Admin Console**.</span></span>

<span data-ttu-id="c56fa-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "integrering status")</span><span class="sxs-lookup"><span data-stu-id="c56fa-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "Integration status")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="c56fa-171">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c56fa-171">Additional resources</span></span>

* [<span data-ttu-id="c56fa-172">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="c56fa-172">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c56fa-173">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c56fa-173">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c56fa-174">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c56fa-174">Configure Single Sign-on</span></span>](active-directory-saas-box-tutorial.md)