---
title: "Självstudier: Konfigurera arbetsplats av Facebook för användaretablering | Microsoft Docs"
description: "Lär dig hur du automatiskt etablera och avetablera användarkonton från Azure AD till arbetsplatsen med Facebook."
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
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: a347eedbf5511dc83e1bc7721667441cfb87cb59
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="cc7ad-103">Självstudier: Konfigurera arbetsplats av Facebook för användaretablering</span><span class="sxs-lookup"><span data-stu-id="cc7ad-103">Tutorial: Configure Workplace by Facebook for user provisioning</span></span>

<span data-ttu-id="cc7ad-104">De här självstudierna visar steg som krävs för att automatiskt etablera och avinstallation etablera användarkonton från Azure Active Directory (AD Azure) till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-104">This tutorial shows you the steps necessary to automatically provision and de-provision user accounts from Azure Active Directory (Azure AD) to Workplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc7ad-105">Krav</span><span class="sxs-lookup"><span data-stu-id="cc7ad-105">Prerequisites</span></span>

<span data-ttu-id="cc7ad-106">För att konfigurera Azure AD-integrering med arbetsplats av Facebook, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cc7ad-106">To configure Azure AD integration with Workplace by Facebook, you need the following:</span></span>

- <span data-ttu-id="cc7ad-107">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc7ad-107">An Azure AD subscription</span></span>
- <span data-ttu-id="cc7ad-108">En arbetsplats av Facebook enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc7ad-108">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="cc7ad-109">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="cc7ad-109">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="cc7ad-110">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-110">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cc7ad-111">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cc7ad-111">If you don't have an Azure AD trial environment, you can get a [one-month trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assign-users-to-workplace-by-facebook"></a><span data-ttu-id="cc7ad-112">Tilldela användare till arbetsplatsen av Facebook</span><span class="sxs-lookup"><span data-stu-id="cc7ad-112">Assign users to Workplace by Facebook</span></span>

<span data-ttu-id="cc7ad-113">Azure AD använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-113">Azure AD uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="cc7ad-114">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har tilldelats till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-114">In the context of automatic user account provisioning, only the users and groups that have been assigned to an application in Azure AD are synchronized.</span></span>

<span data-ttu-id="cc7ad-115">Innan du konfigurerar och aktiverar tjänsten etablering, bestämma vilka användare och grupper i Azure AD representerar de användare som behöver åtkomst till din arbetsplats av Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-115">Before configuring and enabling the provisioning service, decide what users and groups in Azure AD represent the users who need access to your Workplace by Facebook app.</span></span> <span data-ttu-id="cc7ad-116">Du kan sedan tilldela dessa användare till din arbetsplats av Facebook-app genom att följa instruktionerna i [tilldela en användare eller grupp till en enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="cc7ad-116">You can then assign these users to your Workplace by Facebook app by following the instructions in [Assign a user or group to an enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span></span>

>[!IMPORTANT]
>*   <span data-ttu-id="cc7ad-117">Testa allokering konfigurationen genom att tilldela ett enda Azure AD-användare till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-117">Test the provisioning configuration by assigning a single Azure AD user to Workplace by Facebook.</span></span> <span data-ttu-id="cc7ad-118">Tilldela ytterligare användare och grupper senare.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-118">Assign additional users and groups later.</span></span>
>*   <span data-ttu-id="cc7ad-119">Du måste välja en giltig användarroll när du tilldelar en användare till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-119">When you assign a user to Workplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="cc7ad-120">Rollen som standard åtkomst fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-120">The Default Access role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="cc7ad-121">Aktivera automatisk användaretablering</span><span class="sxs-lookup"><span data-stu-id="cc7ad-121">Enable automated user provisioning</span></span>

<span data-ttu-id="cc7ad-122">Det här avsnittet hjälper dig att ansluta din Azure AD till det användarkonto som etablerar API för arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-122">This section guides you through connecting your Azure AD to the user account provisioning API of Workplace by Facebook.</span></span> <span data-ttu-id="cc7ad-123">Du också lära dig hur du konfigurerar tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-123">You also learn how to configure the provisioning service to create, update, and disable assigned user accounts in Workplace by Facebook.</span></span> <span data-ttu-id="cc7ad-124">Detta baseras på användare och grupptilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-124">This is based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="cc7ad-125">Du kan också välja att aktivera SAML-baserade SSO för arbetsplats av Facebook, av följa instruktionerna i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cc7ad-125">You can also choose to enabled SAML-based SSO for Workplace by Facebook, by following the instructions provided in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="cc7ad-126">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-126">SSO can be configured independently of automatic provisioning, though these two features complement each other.</span></span>

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a><span data-ttu-id="cc7ad-127">Konfigurera användarkonto till arbetsplatsen av Facebook-etablering i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc7ad-127">Configure user account provisioning to Workplace by Facebook in Azure AD</span></span>

<span data-ttu-id="cc7ad-128">Azure AD stöder möjligheten att synkronisera automatiskt kontoinformation för tilldelade användare till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-128">Azure AD supports the ability to automatically synchronize the account details of assigned users to Workplace by Facebook.</span></span> <span data-ttu-id="cc7ad-129">Den automatiska synkroniseringen kan arbetsplatsen av Facebook och hämta data som behövs för att auktorisera användare för åtkomst, innan de försöker logga in för första gången.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-129">This automatic synchronization enables Workplace by Facebook to get the data it needs to authorize users for access, before them attempting to sign in for the first time.</span></span> <span data-ttu-id="cc7ad-130">Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-130">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="cc7ad-131">I den [Azure-portalen](https://portal.azure.com)väljer **Azure Active Directory** > **Företagsappar** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-131">In the [Azure portal](https://portal.azure.com), select **Azure Active Directory** > **Enterprise Apps** > **All applications**.</span></span>

2. <span data-ttu-id="cc7ad-132">Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning kan du söka efter din arbetsplats av Facebook-instans med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-132">If you have already configured Workplace by Facebook for SSO, search for your instance of Workplace by Facebook by using the search field.</span></span> <span data-ttu-id="cc7ad-133">Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-133">Otherwise, select **Add** and search for **Workplace by Facebook** in the application gallery.</span></span> <span data-ttu-id="cc7ad-134">Välj **arbetsplats av Facebook** i sökresultatet och lägga till den i listan över program.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-134">Select **Workplace by Facebook** from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="cc7ad-135">Välj din instans av arbetsplatsen av Facebook och välj sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-135">Select your instance of Workplace by Facebook, and then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="cc7ad-136">Ange **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-136">Set **Provisioning Mode** to **Automatic**.</span></span> 

    ![Skärmbild av arbetsplatsen av Facebook alternativ för etablering](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="cc7ad-138">Under den **administratörsautentiseringsuppgifter** ange den **hemlighet Token** och **klient URL** för din arbetsplats av Facebook-administratör.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-138">Under the **Admin Credentials** section, enter the **Secret Token** and the **Tenant URL** of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="cc7ad-139">Välj i Azure-portalen **Testanslutningen** så Azure AD kan ansluta till din arbetsplats med Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-139">In the Azure portal, select **Test Connection** to ensure Azure AD can connect to your Workplace by Facebook app.</span></span> <span data-ttu-id="cc7ad-140">Om anslutningen misslyckas, kan du kontrollera att din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-140">If the connection fails, ensure that your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="cc7ad-141">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-141">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the check box.</span></span>

8. <span data-ttu-id="cc7ad-142">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-142">Select **Save**.</span></span>

9. <span data-ttu-id="cc7ad-143">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare till arbetsplatsen av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-143">Under the Mappings section, select **Synchronize Azure Active Directory Users to Workplace by Facebook**.</span></span>

10. <span data-ttu-id="cc7ad-144">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-144">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to Workplace by Facebook.</span></span> <span data-ttu-id="cc7ad-145">De attribut som valts som **matchande** egenskaper används för att matcha användarkonton i arbetsplats av Facebook för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-145">The attributes selected as **Matching** properties are used to match the user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="cc7ad-146">För att genomföra ändringarna, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-146">To commit any changes, select **Save**.</span></span>

11. <span data-ttu-id="cc7ad-147">Aktivera Azure AD etableras för arbetsplats av Facebook, i den **inställningar** ändrar den **Status för etablering** till **på**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-147">To enable the Azure AD provisioning service for Workplace by Facebook, in the **Settings** section, change the **Provisioning Status** to **On**.</span></span>

12. <span data-ttu-id="cc7ad-148">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-148">Select **Save**.</span></span>

<span data-ttu-id="cc7ad-149">Mer information om hur du konfigurerar automatisk etablering finns [Facebook-dokumentationen](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span><span class="sxs-lookup"><span data-stu-id="cc7ad-149">For more information on how to configure automatic provisioning, see [the Facebook documentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span></span>

<span data-ttu-id="cc7ad-150">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-150">You can now create a test account.</span></span> <span data-ttu-id="cc7ad-151">Vänta i upp till 20 minuter att verifiera att kontot har synkroniserats till arbetsplatsen med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cc7ad-151">Wait for up to 20 minutes to verify that the account has been synchronized to Workplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc7ad-152">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cc7ad-152">Additional resources</span></span>

* [<span data-ttu-id="cc7ad-153">Hantera användare konto-etablering för företagsappar</span><span class="sxs-lookup"><span data-stu-id="cc7ad-153">Managing user account provisioning for enterprise apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cc7ad-154">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cc7ad-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="cc7ad-155">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc7ad-155">Configure single sign-on</span></span>](active-directory-saas-facebook-at-work-tutorial.md)

