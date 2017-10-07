---
title: "Självstudier: Konfigurera arbetsplats av Facebook för användaretablering | Microsoft Docs"
description: "Lär dig hur tooautomatically etablera och avinstallation etablera användarkontona från Azure AD tooWorkplace av Facebook."
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
ms.openlocfilehash: 33d294dbc8f441b29138408b3c9ca41f2141f8af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="c23ce-103">Självstudier: Konfigurera arbetsplats av Facebook för användaretablering</span><span class="sxs-lookup"><span data-stu-id="c23ce-103">Tutorial: Configure Workplace by Facebook for user provisioning</span></span>

<span data-ttu-id="c23ce-104">Den här självstudiekursen visas du hello steg nödvändiga tooautomatically etablera och avetablera användarkonton från Azure Active Directory (AD Azure) tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-104">This tutorial shows you hello steps necessary tooautomatically provision and de-provision user accounts from Azure Active Directory (Azure AD) tooWorkplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c23ce-105">Krav</span><span class="sxs-lookup"><span data-stu-id="c23ce-105">Prerequisites</span></span>

<span data-ttu-id="c23ce-106">tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="c23ce-106">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following:</span></span>

- <span data-ttu-id="c23ce-107">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c23ce-107">An Azure AD subscription</span></span>
- <span data-ttu-id="c23ce-108">En arbetsplats av Facebook enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="c23ce-108">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="c23ce-109">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c23ce-109">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="c23ce-110">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c23ce-110">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c23ce-111">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c23ce-111">If you don't have an Azure AD trial environment, you can get a [one-month trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assign-users-tooworkplace-by-facebook"></a><span data-ttu-id="c23ce-112">Tilldela användare tooWorkplace av Facebook</span><span class="sxs-lookup"><span data-stu-id="c23ce-112">Assign users tooWorkplace by Facebook</span></span>

<span data-ttu-id="c23ce-113">Azure AD använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="c23ce-113">Azure AD uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="c23ce-114">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har tilldelats tooan program i Azure AD synkroniserade.</span><span class="sxs-lookup"><span data-stu-id="c23ce-114">In hello context of automatic user account provisioning, only hello users and groups that have been assigned tooan application in Azure AD are synchronized.</span></span>

<span data-ttu-id="c23ce-115">Innan du konfigurerar och aktiverar hello etableras, bestämma vilka användare och grupper i Azure AD representera hello-användare som behöver åtkomst till tooyour arbetsplats av Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="c23ce-115">Before configuring and enabling hello provisioning service, decide what users and groups in Azure AD represent hello users who need access tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="c23ce-116">Du kan tilldela dessa användare tooyour arbetsplats av Facebook-app genom att följa anvisningarna i hello [tilldela en användare eller grupp tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c23ce-116">You can then assign these users tooyour Workplace by Facebook app by following hello instructions in [Assign a user or group tooan enterprise app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).</span></span>

>[!IMPORTANT]
>*   <span data-ttu-id="c23ce-117">Testa hello etablering konfiguration genom att tilldela en enda tooWorkplace för Azure AD-användare med Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-117">Test hello provisioning configuration by assigning a single Azure AD user tooWorkplace by Facebook.</span></span> <span data-ttu-id="c23ce-118">Tilldela ytterligare användare och grupper senare.</span><span class="sxs-lookup"><span data-stu-id="c23ce-118">Assign additional users and groups later.</span></span>
>*   <span data-ttu-id="c23ce-119">Du måste välja en giltig användarroll när du tilldelar en användare tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-119">When you assign a user tooWorkplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="c23ce-120">hello standard Access roll fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="c23ce-120">hello Default Access role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="c23ce-121">Aktivera automatisk användaretablering</span><span class="sxs-lookup"><span data-stu-id="c23ce-121">Enable automated user provisioning</span></span>

<span data-ttu-id="c23ce-122">Det här avsnittet hjälper dig att ansluta din Azure AD toohello användarkonto etablering API för arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-122">This section guides you through connecting your Azure AD toohello user account provisioning API of Workplace by Facebook.</span></span> <span data-ttu-id="c23ce-123">Du också lära dig hur tooconfigure hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-123">You also learn how tooconfigure hello provisioning service toocreate, update, and disable assigned user accounts in Workplace by Facebook.</span></span> <span data-ttu-id="c23ce-124">Detta baseras på användare och grupptilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c23ce-124">This is based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="c23ce-125">Du kan också välja tooenabled SAML-baserade SSO för arbetsplats av Facebook, av följande hello instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c23ce-125">You can also choose tooenabled SAML-based SSO for Workplace by Facebook, by following hello instructions provided in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c23ce-126">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.</span><span class="sxs-lookup"><span data-stu-id="c23ce-126">SSO can be configured independently of automatic provisioning, though these two features complement each other.</span></span>

### <a name="configure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a><span data-ttu-id="c23ce-127">Konfigurera användarkonto tooWorkplace av Facebook-etablering i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c23ce-127">Configure user account provisioning tooWorkplace by Facebook in Azure AD</span></span>

<span data-ttu-id="c23ce-128">Azure AD stöder hello möjlighet tooautomatically synkronisera hello kontoinformation för tilldelade användare tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-128">Azure AD supports hello ability tooautomatically synchronize hello account details of assigned users tooWorkplace by Facebook.</span></span> <span data-ttu-id="c23ce-129">Den automatiska synkroniseringen kan arbetsplatsen Facebook tooget hello data måste tooauthorize användare för åtkomst, innan du dem och försök toosign i för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="c23ce-129">This automatic synchronization enables Workplace by Facebook tooget hello data it needs tooauthorize users for access, before them attempting toosign in for hello first time.</span></span> <span data-ttu-id="c23ce-130">Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c23ce-130">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="c23ce-131">I hello [Azure-portalen](https://portal.azure.com)väljer **Azure Active Directory** > **Företagsappar** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-131">In hello [Azure portal](https://portal.azure.com), select **Azure Active Directory** > **Enterprise Apps** > **All applications**.</span></span>

2. <span data-ttu-id="c23ce-132">Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning kan du söka efter din arbetsplats av Facebook-instans med hjälp av hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="c23ce-132">If you have already configured Workplace by Facebook for SSO, search for your instance of Workplace by Facebook by using hello search field.</span></span> <span data-ttu-id="c23ce-133">Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="c23ce-133">Otherwise, select **Add** and search for **Workplace by Facebook** in hello application gallery.</span></span> <span data-ttu-id="c23ce-134">Välj **arbetsplats av Facebook** från hello sökresultat och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="c23ce-134">Select **Workplace by Facebook** from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="c23ce-135">Välj din instans av arbetsplatsen av Facebook och välj sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="c23ce-135">Select your instance of Workplace by Facebook, and then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="c23ce-136">Ange **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-136">Set **Provisioning Mode** too**Automatic**.</span></span> 

    ![Skärmbild av arbetsplatsen av Facebook alternativ för etablering](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="c23ce-138">Under hello **administratörsautentiseringsuppgifter** ange hello **hemlighet Token** och hello **klient URL** för din arbetsplats av Facebook-administratör.</span><span class="sxs-lookup"><span data-stu-id="c23ce-138">Under hello **Admin Credentials** section, enter hello **Secret Token** and hello **Tenant URL** of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="c23ce-139">Välj i hello Azure-portalen, **Testanslutningen** tooensure Azure AD kan ansluta tooyour arbetsplats av Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="c23ce-139">In hello Azure portal, select **Test Connection** tooensure Azure AD can connect tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="c23ce-140">Om hello anslutning misslyckas kan du kontrollera att din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="c23ce-140">If hello connection fails, ensure that your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="c23ce-141">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="c23ce-141">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello check box.</span></span>

8. <span data-ttu-id="c23ce-142">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-142">Select **Save**.</span></span>

9. <span data-ttu-id="c23ce-143">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooWorkplace av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-143">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooWorkplace by Facebook**.</span></span>

10. <span data-ttu-id="c23ce-144">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-144">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooWorkplace by Facebook.</span></span> <span data-ttu-id="c23ce-145">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i arbetsplats av Facebook för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="c23ce-145">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="c23ce-146">toocommit ändringar, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-146">toocommit any changes, select **Save**.</span></span>

11. <span data-ttu-id="c23ce-147">tooenable hello Azure AD-etablering tjänsten för arbetsplats av Facebook, i hello **inställningar** ändrar hello **Status för etablering** för**på**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-147">tooenable hello Azure AD provisioning service for Workplace by Facebook, in hello **Settings** section, change hello **Provisioning Status** too**On**.</span></span>

12. <span data-ttu-id="c23ce-148">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c23ce-148">Select **Save**.</span></span>

<span data-ttu-id="c23ce-149">Mer information om hur tooconfigure Automatisk etablering, se [hello Facebook dokumentationen](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span><span class="sxs-lookup"><span data-stu-id="c23ce-149">For more information on how tooconfigure automatic provisioning, see [hello Facebook documentation](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).</span></span>

<span data-ttu-id="c23ce-150">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="c23ce-150">You can now create a test account.</span></span> <span data-ttu-id="c23ce-151">Vänta tills in too20 minuter tooverify hello kontot har synkroniserats tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="c23ce-151">Wait for up too20 minutes tooverify that hello account has been synchronized tooWorkplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c23ce-152">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c23ce-152">Additional resources</span></span>

* [<span data-ttu-id="c23ce-153">Hantera användare konto-etablering för företagsappar</span><span class="sxs-lookup"><span data-stu-id="c23ce-153">Managing user account provisioning for enterprise apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c23ce-154">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c23ce-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="c23ce-155">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c23ce-155">Configure single sign-on</span></span>](active-directory-saas-facebook-at-work-tutorial.md)

