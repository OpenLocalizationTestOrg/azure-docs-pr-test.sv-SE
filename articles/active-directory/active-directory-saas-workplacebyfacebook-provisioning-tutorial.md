---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 9b22679c304248ed7ba7a6bd9eaf82b64f7143cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="b1db1-103">Självstudier: Konfigurera arbetsplats av Facebook för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="b1db1-103">Tutorial: Configuring Workplace by Facebook for User Provisioning</span></span>

<span data-ttu-id="b1db1-104">Syftet med den här kursen är att visa de steg som du behöver utföra på arbetsplats av Facebook och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="b1db1-104">The objective of this tutorial is to show you the steps you need to perform in Workplace by Facebook and Azure AD to automatically provision and de-provision user accounts from Azure AD to Workplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1db1-105">Krav</span><span class="sxs-lookup"><span data-stu-id="b1db1-105">Prerequisites</span></span>

<span data-ttu-id="b1db1-106">För att konfigurera Azure AD-integrering med arbetsplats av Facebook, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b1db1-106">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="b1db1-107">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b1db1-107">An Azure AD subscription</span></span>
- <span data-ttu-id="b1db1-108">En arbetsplats av Facebook enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b1db1-108">A Workplace by Facebook single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1db1-109">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b1db1-109">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1db1-110">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b1db1-110">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1db1-111">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b1db1-111">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b1db1-112">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1db1-112">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assigning-users-to-workplace-by-facebook"></a><span data-ttu-id="b1db1-113">Tilldela användare till arbetsplatsen av Facebook</span><span class="sxs-lookup"><span data-stu-id="b1db1-113">Assigning users to Workplace by Facebook</span></span>

<span data-ttu-id="b1db1-114">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="b1db1-114">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="b1db1-115">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1db1-115">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span>

<span data-ttu-id="b1db1-116">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till din arbetsplats av Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="b1db1-116">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your Workplace by Facebook app.</span></span> <span data-ttu-id="b1db1-117">När bestämt, kan du tilldela dessa användare till din arbetsplats av Facebook-app genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="b1db1-117">Once decided, you can assign these users to your Workplace by Facebook app by following the instructions here:</span></span>

[<span data-ttu-id="b1db1-118">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="b1db1-118">Assign a user or group to an enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-workplace-by-facebook"></a><span data-ttu-id="b1db1-119">Viktiga tips för att tilldela användare till arbetsplatsen av Facebook</span><span class="sxs-lookup"><span data-stu-id="b1db1-119">Important tips for assigning users to Workplace by Facebook</span></span>

*   <span data-ttu-id="b1db1-120">Vi rekommenderar att en enda Azure AD-användare har tilldelats till arbetsplatsen av Facebook testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b1db1-120">It is recommended that a single Azure AD user is assigned to Workplace by Facebook to test the provisioning configuration.</span></span> <span data-ttu-id="b1db1-121">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="b1db1-121">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="b1db1-122">Du måste välja en giltig användarroll när du tilldelar en användare till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="b1db1-122">When assigning a user to Workplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="b1db1-123">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="b1db1-123">The "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="b1db1-124">Aktivera etablering av användare</span><span class="sxs-lookup"><span data-stu-id="b1db1-124">Enable User Provisioning</span></span>

<span data-ttu-id="b1db1-125">Det här avsnittet hjälper dig att ansluta din Azure AD till arbetsplatsen via Facebooks användarkontot etablering API och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i arbetsplats av Facebook baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1db1-125">This section guides you through connecting your Azure AD to Workplace by Facebook's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Workplace by Facebook based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="b1db1-126">Du kan också välja att aktivera SAML-baserade enkel inloggning för arbetsplats med Facebook, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b1db1-126">You may also choose to enabled SAML-based Single Sign-On for Workplace by Facebook, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="b1db1-127">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="b1db1-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="to-configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a><span data-ttu-id="b1db1-128">Så här konfigurerar du kontot användaretablering till arbetsplatsen av Facebook i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="b1db1-128">To configure user account provisioning to Workplace by Facebook in Azure AD:</span></span>

<span data-ttu-id="b1db1-129">Syftet med det här avsnittet är att beskriva hur du aktiverar etablering av Active Directory-användarkonton till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="b1db1-129">The objective of this section is to outline how to enable provisioning of Active Directory user accounts to Workplace by Facebook.</span></span>

<span data-ttu-id="b1db1-130">Azure AD stöder möjligheten att synkronisera automatiskt kontoinformation för tilldelade användare till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="b1db1-130">Azure AD supports the ability to automatically synchronize the account details of assigned users to Workplace by Facebook.</span></span> <span data-ttu-id="b1db1-131">Den automatiska synkroniseringen kan arbetsplatsen av Facebook och hämta data som behövs för att auktorisera användare för åtkomst, innan de försöker logga in för första gången.</span><span class="sxs-lookup"><span data-stu-id="b1db1-131">This automatic synchronization enables Workplace by Facebook to get the data it needs to authorize users for access, before them attempting to sign in for the first time.</span></span> <span data-ttu-id="b1db1-132">Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1db1-132">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="b1db1-133">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory** > **Företagsappar** > **alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b1db1-133">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory** > **Enterprise Apps** > **All applications** section.</span></span>

2. <span data-ttu-id="b1db1-134">Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning, söka efter din arbetsplats med hjälp av sökfältet Facebook-instans.</span><span class="sxs-lookup"><span data-stu-id="b1db1-134">If you have already configured Workplace by Facebook for single sign-on, search for your instance of Workplace by Facebook using the search field.</span></span> <span data-ttu-id="b1db1-135">Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="b1db1-135">Otherwise, select **Add** and search for **Workplace by Facebook** in the application gallery.</span></span> <span data-ttu-id="b1db1-136">Välj arbetsplats av Facebook i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="b1db1-136">Select Workplace by Facebook from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="b1db1-137">Välj din arbetsplats av Facebook-instans och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="b1db1-137">Select your instance of Workplace by Facebook, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="b1db1-138">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="b1db1-138">Set the **Provisioning Mode** to **Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="b1db1-140">Under den **administratörsautentiseringsuppgifter** Ange hemlighet-Token och klient-URL för din arbetsplats av Facebook-administratör.</span><span class="sxs-lookup"><span data-stu-id="b1db1-140">Under the **Admin Credentials** section, enter the Secret Token and the Tenant URL of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="b1db1-141">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din arbetsplats med Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="b1db1-141">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your Workplace by Facebook app.</span></span> <span data-ttu-id="b1db1-142">Om anslutningen misslyckas, kontrollera din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="b1db1-142">If the connection fails, ensure your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="b1db1-143">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b1db1-143">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox.</span></span>

8. <span data-ttu-id="b1db1-144">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="b1db1-144">Click **Save.**</span></span>

9. <span data-ttu-id="b1db1-145">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till arbetsplatsen med Facebook.**</span><span class="sxs-lookup"><span data-stu-id="b1db1-145">Under the Mappings section, select **Synchronize Azure Active Directory Users to Workplace by Facebook.**</span></span>

10. <span data-ttu-id="b1db1-146">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="b1db1-146">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Workplace by Facebook.</span></span> <span data-ttu-id="b1db1-147">De attribut som valts som **matchande** egenskaper används för att matcha användarkonton i arbetsplats av Facebook för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="b1db1-147">The attributes selected as **Matching** properties are used to match the user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="b1db1-148">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b1db1-148">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="b1db1-149">Om du vill aktivera Azure AD etableras för arbetsplats av Facebook, ändra den **Status för etablering** till **på** i den **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="b1db1-149">To enable the Azure AD provisioning service for Workplace by Facebook, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="b1db1-150">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="b1db1-150">Click **Save.**</span></span>

<span data-ttu-id="b1db1-151">Mer information om hur du konfigurerar automatisk etablering finns [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span><span class="sxs-lookup"><span data-stu-id="b1db1-151">For more information on how to configure automatic provisioning, see [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span></span>

<span data-ttu-id="b1db1-152">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="b1db1-152">You can now create a test account.</span></span> <span data-ttu-id="b1db1-153">Vänta i upp till 20 minuter att verifiera att kontot har synkroniserats till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="b1db1-153">Wait for up to 20 minutes to verify that the account has been synchronized to Workplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1db1-154">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b1db1-154">Additional resources</span></span>

* [<span data-ttu-id="b1db1-155">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="b1db1-155">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1db1-156">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b1db1-156">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b1db1-157">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b1db1-157">Configure Single Sign-on</span></span>](active-directory-saas-workplacebyfacebook-tutorial.md)

