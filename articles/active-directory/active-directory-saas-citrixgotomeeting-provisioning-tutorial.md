---
title: "Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a><span data-ttu-id="36e20-103">Självstudier: Konfigurera Citrix GoToMeeting för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="36e20-103">Tutorial: Configuring Citrix GoToMeeting for Automatic User Provisioning</span></span>

<span data-ttu-id="36e20-104">Syftet med den här kursen är att visa de steg som du behöver göra i Citrix GoToMeeting och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="36e20-104">The objective of this tutorial is to show you the steps you need to perform in Citrix GoToMeeting and Azure AD to automatically provision and de-provision user accounts from Azure AD to Citrix GoToMeeting.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36e20-105">Krav</span><span class="sxs-lookup"><span data-stu-id="36e20-105">Prerequisites</span></span>

<span data-ttu-id="36e20-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="36e20-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="36e20-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="36e20-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="36e20-108">En Citrix GoToMeeting enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="36e20-108">A Citrix GoToMeeting single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="36e20-109">Ett användarkonto i Citrix GoToMeeting Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="36e20-109">A user account in Citrix GoToMeeting with Team Admin permissions.</span></span>

## <a name="assigning-users-to-citrix-gotomeeting"></a><span data-ttu-id="36e20-110">Tilldela användare till Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="36e20-110">Assigning users to Citrix GoToMeeting</span></span>

<span data-ttu-id="36e20-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="36e20-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="36e20-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36e20-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="36e20-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till appen Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="36e20-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Citrix GoToMeeting app.</span></span> <span data-ttu-id="36e20-114">När bestämt, kan du tilldela dessa användare i appen Citrix GoToMeeting genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="36e20-114">Once decided, you can assign these users to your Citrix GoToMeeting app by following the instructions here:</span></span>

[<span data-ttu-id="36e20-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="36e20-115">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a><span data-ttu-id="36e20-116">Viktiga tips för att tilldela användare till Citrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="36e20-116">Important tips for assigning users to Citrix GoToMeeting</span></span>

*   <span data-ttu-id="36e20-117">Vi rekommenderar att en enda Azure AD-användare har tilldelats till Citrix GoToMeeting testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="36e20-117">It is recommended that a single Azure AD user is assigned to Citrix GoToMeeting to test the provisioning configuration.</span></span> <span data-ttu-id="36e20-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="36e20-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="36e20-119">När du tilldelar en användare Citrix GoToMeeting, måste du välja en giltig användarroll.</span><span class="sxs-lookup"><span data-stu-id="36e20-119">When assigning a user to Citrix GoToMeeting, you must select a valid user role.</span></span> <span data-ttu-id="36e20-120">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="36e20-120">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="36e20-121">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="36e20-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="36e20-122">Det här avsnittet hjälper dig att ansluta din Azure AD till Citrix GoToMeeting användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelad användare konton i Citrix GoToMeeting baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36e20-122">This section guides you through connecting your Azure AD to Citrix GoToMeeting's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Citrix GoToMeeting based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="36e20-123">Du kan också välja att aktivera SAML-baserade enkel inloggning för Citrix GoToMeeting, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36e20-123">You may also choose to enabled SAML-based Single Sign-On for Citrix GoToMeeting, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="36e20-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="36e20-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-automatic-user-account-provisioning"></a><span data-ttu-id="36e20-125">Konfigurera automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="36e20-125">To configure automatic user account provisioning:</span></span>

1. <span data-ttu-id="36e20-126">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="36e20-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="36e20-127">Om du redan har konfigurerat Citrix GoToMeeting för enkel inloggning, söka efter din instans av Citrix GoToMeeting med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="36e20-127">If you have already configured Citrix GoToMeeting for single sign-on, search for your instance of Citrix GoToMeeting using the search field.</span></span> <span data-ttu-id="36e20-128">Annars väljer **Lägg till** och Sök efter **Citrix GoToMeeting** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="36e20-128">Otherwise, select **Add** and search for **Citrix GoToMeeting** in the application gallery.</span></span> <span data-ttu-id="36e20-129">Välj Citrix GoToMeeting i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="36e20-129">Select Citrix GoToMeeting from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="36e20-130">Välj din instans av Citrix GoToMeeting och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="36e20-130">Select your instance of Citrix GoToMeeting, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="36e20-131">Ange den **etablering** läge för att **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="36e20-131">Set the **Provisioning** Mode to **Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="36e20-133">Under avsnittet autentiseringsuppgifter som administratör utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="36e20-133">Under the Admin Credentials section, perform the following steps:</span></span>
   
    <span data-ttu-id="36e20-134">a.</span><span class="sxs-lookup"><span data-stu-id="36e20-134">a.</span></span> <span data-ttu-id="36e20-135">I den **Citrix GoToMeeting administratörsanvändarnamnet** textruta anger användarnamn för en administratör.</span><span class="sxs-lookup"><span data-stu-id="36e20-135">In the **Citrix GoToMeeting Admin User Name** textbox, type the user name of an administrator.</span></span>

    <span data-ttu-id="36e20-136">b.</span><span class="sxs-lookup"><span data-stu-id="36e20-136">b.</span></span> <span data-ttu-id="36e20-137">I den **Citrix GoToMeeting adminlösenord** textruta administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="36e20-137">In the **Citrix GoToMeeting Admin Password** textbox, the administrator's password.</span></span>

6. <span data-ttu-id="36e20-138">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till Citrix GoToMeeting appen.</span><span class="sxs-lookup"><span data-stu-id="36e20-138">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Citrix GoToMeeting app.</span></span> <span data-ttu-id="36e20-139">Om anslutningen misslyckas, kontrollera kontots Citrix GoToMeeting har administratörsbehörigheter för teamet och försök den **”administratörsautentiseringsuppgifter”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="36e20-139">If the connection fails, ensure your Citrix GoToMeeting account has Team Admin permissions and try the **"Admin Credentials"** step again.</span></span>

7. <span data-ttu-id="36e20-140">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="36e20-140">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

8. <span data-ttu-id="36e20-141">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="36e20-141">Click **Save.**</span></span>

9. <span data-ttu-id="36e20-142">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till Citrix GoToMeeting.**</span><span class="sxs-lookup"><span data-stu-id="36e20-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to Citrix GoToMeeting.**</span></span>

10. <span data-ttu-id="36e20-143">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="36e20-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Citrix GoToMeeting.</span></span> <span data-ttu-id="36e20-144">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i Citrix GoToMeeting för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="36e20-144">The attributes selected as **Matching** properties are used to match the user accounts in Citrix GoToMeeting for update operations.</span></span> <span data-ttu-id="36e20-145">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="36e20-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="36e20-146">Om du vill aktivera Azure AD-tjänsten för Citrix GoToMeeting-etablering, ändra den **Status för etablering** till **på** i avsnittet Inställningar</span><span class="sxs-lookup"><span data-stu-id="36e20-146">To enable the Azure AD provisioning service for Citrix GoToMeeting, change the **Provisioning Status** to **On** in the Settings section</span></span>

12. <span data-ttu-id="36e20-147">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="36e20-147">Click **Save.**</span></span>

<span data-ttu-id="36e20-148">Startar den första synkroniseringen av användare och/eller grupper som tilldelas till Citrix GoToMeeting i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="36e20-148">It starts the initial synchronization of any users and/or groups assigned to Citrix GoToMeeting in the Users and Groups section.</span></span> <span data-ttu-id="36e20-149">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="36e20-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="36e20-150">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering i appen Citrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="36e20-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Citrix GoToMeeting app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36e20-151">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="36e20-151">Additional resources</span></span>

* [<span data-ttu-id="36e20-152">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="36e20-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36e20-153">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="36e20-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="36e20-154">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36e20-154">Configure Single Sign-on</span></span>](active-directory-saas-citrix-gotomeeting-tutorial.md)


