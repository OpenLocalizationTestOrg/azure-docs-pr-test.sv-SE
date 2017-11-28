---
title: "Självstudier: Konfigurera Samanage för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooSamanage."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 6cb36d2cc6ce33da4f8ebba65d138bfd4f2aca9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a><span data-ttu-id="647ac-103">Självstudier: Konfigurera Samanage för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="647ac-103">Tutorial: Configuring Samanage for Automatic User Provisioning</span></span>


<span data-ttu-id="647ac-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Samanage och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooSamanage.</span><span class="sxs-lookup"><span data-stu-id="647ac-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Samanage and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSamanage.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="647ac-105">Krav</span><span class="sxs-lookup"><span data-stu-id="647ac-105">Prerequisites</span></span>

<span data-ttu-id="647ac-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="647ac-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="647ac-107">En Azure Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="647ac-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="647ac-108">En Samanage klient med hello [Professional planen](https://www.samanage.com/pricing/) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="647ac-108">A Samanage tenant with hello [Professional plan](https://www.samanage.com/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="647ac-109">Ett användarkonto i Samanage med administratörsbehörigheter</span><span class="sxs-lookup"><span data-stu-id="647ac-109">A user account in Samanage with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="647ac-110">hello Azure AD-etablering integration förlitar sig på hello [Samanage REST API](https://www.samanage.com/api/), som är tillgängliga tooSamanage team på hello Professional planera eller bättre.</span><span class="sxs-lookup"><span data-stu-id="647ac-110">hello Azure AD provisioning integration relies on hello [Samanage REST API](https://www.samanage.com/api/), which is available tooSamanage teams on hello Professional plan or better.</span></span>

## <a name="assigning-users-toosamanage"></a><span data-ttu-id="647ac-111">Tilldela användare tooSamanage</span><span class="sxs-lookup"><span data-stu-id="647ac-111">Assigning users tooSamanage</span></span>

<span data-ttu-id="647ac-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="647ac-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="647ac-113">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="647ac-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="647ac-114">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Samanage app.</span><span class="sxs-lookup"><span data-stu-id="647ac-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Samanage app.</span></span> <span data-ttu-id="647ac-115">När du valt, kan du tilldela dessa användare tooyour Samanage app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="647ac-115">Once decided, you can assign these users tooyour Samanage app by following hello instructions here:</span></span>

[<span data-ttu-id="647ac-116">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="647ac-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toosamanage"></a><span data-ttu-id="647ac-117">Viktiga tips för att tilldela användare tooSamanage</span><span class="sxs-lookup"><span data-stu-id="647ac-117">Important tips for assigning users tooSamanage</span></span>

*   <span data-ttu-id="647ac-118">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooSamanage tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="647ac-118">It is recommended that a single Azure AD user is assigned tooSamanage tootest hello provisioning configuration.</span></span> <span data-ttu-id="647ac-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="647ac-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="647ac-120">När du tilldelar en användare tooSamanage, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="647ac-120">When assigning a user tooSamanage, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="647ac-121">Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.</span><span class="sxs-lookup"><span data-stu-id="647ac-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="647ac-122">Som en extra funktion hello etableras läser eventuella anpassade roller som definierats i Samanage och importerar dem till Azure AD där de kan väljas i dialogrutan för hello Välj roll.</span><span class="sxs-lookup"><span data-stu-id="647ac-122">As an added feature, hello provisioning service reads any custom roles defined in Samanage, and imports them into Azure AD where they can be selected in hello Select Role dialog.</span></span> <span data-ttu-id="647ac-123">De här rollerna kommer att visas i hello Azure-portalen när hello etableras är aktiverat och en synkroniseringscykel har slutförts.</span><span class="sxs-lookup"><span data-stu-id="647ac-123">These roles will be visible in hello Azure portal after hello provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-toosamanage"></a><span data-ttu-id="647ac-124">Konfigurera användaretablering tooSamanage</span><span class="sxs-lookup"><span data-stu-id="647ac-124">Configuring user provisioning tooSamanage</span></span> 

<span data-ttu-id="647ac-125">Det här avsnittet hjälper dig att ansluta din Azure AD-tooSamanage användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Samanage baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="647ac-125">This section guides you through connecting your Azure AD tooSamanage's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Samanage based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="647ac-126">Du kan också välja tooenabled SAML-baserade enkel inloggning för Samanage, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="647ac-126">You may also choose tooenabled SAML-based Single Sign-On for Samanage, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="647ac-127">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="647ac-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toosamanage-in-azure-ad"></a><span data-ttu-id="647ac-128">Konfigurera automatisk användarkonto etablering tooSamanage i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="647ac-128">Configure automatic user account provisioning tooSamanage in Azure AD:</span></span>


1. <span data-ttu-id="647ac-129">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="647ac-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="647ac-130">Om du redan har konfigurerat Samanage för enkel inloggning, söka efter din instans av Samanage hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="647ac-130">If you have already configured Samanage for single sign-on, search for your instance of Samanage using hello search field.</span></span> <span data-ttu-id="647ac-131">Annars väljer **Lägg till** och Sök efter **Samanage** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="647ac-131">Otherwise, select **Add** and search for **Samanage** in hello application gallery.</span></span> <span data-ttu-id="647ac-132">Välj Samanage från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="647ac-132">Select Samanage from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="647ac-133">Välj din instans av Samanage och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="647ac-133">Select your instance of Samanage, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="647ac-134">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="647ac-134">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![Samanage etablering](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. <span data-ttu-id="647ac-136">Under hello **administratörsautentiseringsuppgifter** avsnitt, inkommande hello **Admin Username & adminlösenord** för din Samanage konto.</span><span class="sxs-lookup"><span data-stu-id="647ac-136">Under hello **Admin Credentials** section, input hello **Admin Username&Admin Password** of your Samanage's account.</span></span> 

6. <span data-ttu-id="647ac-137">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Samanage app.</span><span class="sxs-lookup"><span data-stu-id="647ac-137">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Samanage app.</span></span> <span data-ttu-id="647ac-138">Om hello anslutningen misslyckas, kontrollera kontots Samanage har administratörsbehörigheter och försök steg 5 igen.</span><span class="sxs-lookup"><span data-stu-id="647ac-138">If hello connection fails, ensure your Samanage account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="647ac-139">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.</span><span class="sxs-lookup"><span data-stu-id="647ac-139">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="647ac-140">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="647ac-140">Click **Save**.</span></span> 

9. <span data-ttu-id="647ac-141">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooSamanage**.</span><span class="sxs-lookup"><span data-stu-id="647ac-141">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSamanage**.</span></span>

10. <span data-ttu-id="647ac-142">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooSamanage.</span><span class="sxs-lookup"><span data-stu-id="647ac-142">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooSamanage.</span></span> <span data-ttu-id="647ac-143">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Samanage för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="647ac-143">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Samanage for update operations.</span></span> <span data-ttu-id="647ac-144">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="647ac-144">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="647ac-145">tooenable hello Azure AD-etablering tjänsten för Samanage, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="647ac-145">tooenable hello Azure AD provisioning service for Samanage, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="647ac-146">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="647ac-146">Click **Save**.</span></span> 

<span data-ttu-id="647ac-147">Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooSamanage i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="647ac-147">This operation starts hello initial synchronization of any users and/or groups assigned tooSamanage in hello Users and Groups section.</span></span> <span data-ttu-id="647ac-148">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="647ac-148">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="647ac-149">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.</span><span class="sxs-lookup"><span data-stu-id="647ac-149">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="647ac-150">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="647ac-150">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="647ac-151">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="647ac-151">Additional resources</span></span>

* [<span data-ttu-id="647ac-152">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="647ac-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="647ac-153">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="647ac-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="647ac-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="647ac-154">Next steps</span></span>

* [<span data-ttu-id="647ac-155">Lär dig hur tooreview loggar och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="647ac-155">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
