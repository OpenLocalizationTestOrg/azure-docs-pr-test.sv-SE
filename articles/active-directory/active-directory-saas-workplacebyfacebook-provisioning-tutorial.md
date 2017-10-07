---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
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
ms.openlocfilehash: 551ec353a5ec1da936373587688c299a6f4acca7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-workplace-by-facebook-for-user-provisioning"></a><span data-ttu-id="f7cbc-103">Självstudier: Konfigurera arbetsplats av Facebook för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="f7cbc-103">Tutorial: Configuring Workplace by Facebook for User Provisioning</span></span>

<span data-ttu-id="f7cbc-104">hello syftet med den här kursen är tooshow du hello stegen tooperform arbetsplatsen av Facebook och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Workplace by Facebook and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooWorkplace by Facebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7cbc-105">Krav</span><span class="sxs-lookup"><span data-stu-id="f7cbc-105">Prerequisites</span></span>

<span data-ttu-id="f7cbc-106">tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f7cbc-106">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="f7cbc-107">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7cbc-107">An Azure AD subscription</span></span>
- <span data-ttu-id="f7cbc-108">En arbetsplats av Facebook enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7cbc-108">A Workplace by Facebook single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f7cbc-109">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-109">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f7cbc-110">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f7cbc-110">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f7cbc-111">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-111">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f7cbc-112">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f7cbc-112">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="assigning-users-tooworkplace-by-facebook"></a><span data-ttu-id="f7cbc-113">Tilldela användare tooWorkplace av Facebook</span><span class="sxs-lookup"><span data-stu-id="f7cbc-113">Assigning users tooWorkplace by Facebook</span></span>

<span data-ttu-id="f7cbc-114">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-114">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="f7cbc-115">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-115">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="f7cbc-116">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour arbetsplats av Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-116">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="f7cbc-117">När du valt, kan du tilldela dessa användare tooyour arbetsplats av Facebook-app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="f7cbc-117">Once decided, you can assign these users tooyour Workplace by Facebook app by following hello instructions here:</span></span>

[<span data-ttu-id="f7cbc-118">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="f7cbc-118">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-tooworkplace-by-facebook"></a><span data-ttu-id="f7cbc-119">Viktiga tips för att tilldela användare tooWorkplace av Facebook</span><span class="sxs-lookup"><span data-stu-id="f7cbc-119">Important tips for assigning users tooWorkplace by Facebook</span></span>

*   <span data-ttu-id="f7cbc-120">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooWorkplace av Facebook tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-120">It is recommended that a single Azure AD user is assigned tooWorkplace by Facebook tootest hello provisioning configuration.</span></span> <span data-ttu-id="f7cbc-121">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-121">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="f7cbc-122">Du måste välja en giltig användarroll när du tilldelar en användare tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-122">When assigning a user tooWorkplace by Facebook, you must select a valid user role.</span></span> <span data-ttu-id="f7cbc-123">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-123">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="f7cbc-124">Aktivera etablering av användare</span><span class="sxs-lookup"><span data-stu-id="f7cbc-124">Enable User Provisioning</span></span>

<span data-ttu-id="f7cbc-125">Det här avsnittet hjälper dig att ansluta din Azure AD-tooWorkplace av Facebooks användarkontot API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i arbetsplats av Facebook baserat på användare och grupper tilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-125">This section guides you through connecting your Azure AD tooWorkplace by Facebook's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Workplace by Facebook based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="f7cbc-126">Du kan också välja tooenabled SAML-baserade enkel inloggning för arbetsplats av Facebook, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f7cbc-126">You may also choose tooenabled SAML-based Single Sign-On for Workplace by Facebook, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="f7cbc-127">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning-tooworkplace-by-facebook-in-azure-ad"></a><span data-ttu-id="f7cbc-128">tooconfigure användarkonto tooWorkplace av Facebook-etablering i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="f7cbc-128">tooconfigure user account provisioning tooWorkplace by Facebook in Azure AD:</span></span>

<span data-ttu-id="f7cbc-129">hello syftet med det här avsnittet är toooutline hur tooenable etablering av Active Directory-användare konton tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-129">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooWorkplace by Facebook.</span></span>

<span data-ttu-id="f7cbc-130">Azure AD stöder hello möjlighet tooautomatically synkronisera hello kontoinformation för tilldelade användare tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-130">Azure AD supports hello ability tooautomatically synchronize hello account details of assigned users tooWorkplace by Facebook.</span></span> <span data-ttu-id="f7cbc-131">Den automatiska synkroniseringen kan arbetsplatsen Facebook tooget hello data måste tooauthorize användare för åtkomst, innan du dem och försök toosign i för hello första gången.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-131">This automatic synchronization enables Workplace by Facebook tooget hello data it needs tooauthorize users for access, before them attempting toosign in for hello first time.</span></span> <span data-ttu-id="f7cbc-132">Den också etablerar Frigör användare från arbetsplats av Facebook när åtkomst har återkallats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-132">It also de-provisions users from Workplace by Facebook when access has been revoked in Azure AD.</span></span>

1. <span data-ttu-id="f7cbc-133">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory** > **Företagsappar** > **alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-133">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory** > **Enterprise Apps** > **All applications** section.</span></span>

2. <span data-ttu-id="f7cbc-134">Om du redan har konfigurerat arbetsplats av Facebook för enkel inloggning, söka efter din instans av arbetsplatsen av Facebook hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-134">If you have already configured Workplace by Facebook for single sign-on, search for your instance of Workplace by Facebook using hello search field.</span></span> <span data-ttu-id="f7cbc-135">Annars väljer **Lägg till** och Sök efter **arbetsplats av Facebook** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-135">Otherwise, select **Add** and search for **Workplace by Facebook** in hello application gallery.</span></span> <span data-ttu-id="f7cbc-136">Välj arbetsplats av Facebook från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-136">Select Workplace by Facebook from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="f7cbc-137">Välj din arbetsplats av Facebook-instans och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-137">Select your instance of Workplace by Facebook, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="f7cbc-138">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-138">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-workplacebyfacebook-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="f7cbc-140">Under hello **administratörsautentiseringsuppgifter** avsnittet anger hello hemlighet Token och hello klient-URL för din arbetsplats av Facebook-administratör.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-140">Under hello **Admin Credentials** section, enter hello Secret Token and hello Tenant URL of your Workplace by Facebook administrator.</span></span>

6. <span data-ttu-id="f7cbc-141">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour arbetsplats av Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-141">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Workplace by Facebook app.</span></span> <span data-ttu-id="f7cbc-142">Om hello anslutningen misslyckas, kontrollera din arbetsplats av Facebook-konto har teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-142">If hello connection fails, ensure your Workplace by Facebook account has Team Admin permissions.</span></span>

7. <span data-ttu-id="f7cbc-143">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-143">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

8. <span data-ttu-id="f7cbc-144">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="f7cbc-144">Click **Save.**</span></span>

9. <span data-ttu-id="f7cbc-145">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooWorkplace av Facebook.**</span><span class="sxs-lookup"><span data-stu-id="f7cbc-145">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooWorkplace by Facebook.**</span></span>

10. <span data-ttu-id="f7cbc-146">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-146">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooWorkplace by Facebook.</span></span> <span data-ttu-id="f7cbc-147">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i arbetsplats av Facebook för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-147">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Workplace by Facebook for update operations.</span></span> <span data-ttu-id="f7cbc-148">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-148">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="f7cbc-149">tooenable hello Azure AD-etablering tjänsten för arbetsplats av Facebook, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="f7cbc-149">tooenable hello Azure AD provisioning service for Workplace by Facebook, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="f7cbc-150">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="f7cbc-150">Click **Save.**</span></span>

<span data-ttu-id="f7cbc-151">Mer information om hur tooconfigure Automatisk etablering, se [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span><span class="sxs-lookup"><span data-stu-id="f7cbc-151">For more information on how tooconfigure automatic provisioning, see [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)</span></span>

<span data-ttu-id="f7cbc-152">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-152">You can now create a test account.</span></span> <span data-ttu-id="f7cbc-153">Vänta tills in too20 minuter tooverify hello kontot har synkroniserats tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="f7cbc-153">Wait for up too20 minutes tooverify that hello account has been synchronized tooWorkplace by Facebook.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7cbc-154">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f7cbc-154">Additional resources</span></span>

* [<span data-ttu-id="f7cbc-155">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="f7cbc-155">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f7cbc-156">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f7cbc-156">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="f7cbc-157">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7cbc-157">Configure Single Sign-on</span></span>](active-directory-saas-workplacebyfacebook-tutorial.md)

