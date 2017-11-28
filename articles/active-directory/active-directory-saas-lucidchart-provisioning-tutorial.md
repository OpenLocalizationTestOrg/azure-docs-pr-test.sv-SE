---
title: "Självstudier: Konfigurera LucidChart för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooLucidChart."
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
ms.openlocfilehash: d3af45141731215f2edc8942ad21b016468c1e38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a><span data-ttu-id="65ae1-103">Självstudier: Konfigurera LucidChart för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="65ae1-103">Tutorial: Configuring LucidChart for Automatic User Provisioning</span></span>


<span data-ttu-id="65ae1-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i LucidChart och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooLucidChart.</span><span class="sxs-lookup"><span data-stu-id="65ae1-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LucidChart and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLucidChart.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="65ae1-105">Krav</span><span class="sxs-lookup"><span data-stu-id="65ae1-105">Prerequisites</span></span>

<span data-ttu-id="65ae1-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="65ae1-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="65ae1-107">En Azure Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="65ae1-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="65ae1-108">En LucidChart klient med hello [företagsplan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="65ae1-108">A LucidChart tenant with hello [Enterprise plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) or better enabled</span></span> 
*   <span data-ttu-id="65ae1-109">Ett användarkonto i LucidChart med administratörsbehörigheter</span><span class="sxs-lookup"><span data-stu-id="65ae1-109">A user account in LucidChart with Admin permissions</span></span> 

## <a name="assigning-users-toolucidchart"></a><span data-ttu-id="65ae1-110">Tilldela användare tooLucidChart</span><span class="sxs-lookup"><span data-stu-id="65ae1-110">Assigning users tooLucidChart</span></span>

<span data-ttu-id="65ae1-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="65ae1-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="65ae1-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="65ae1-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="65ae1-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour LucidChart app.</span><span class="sxs-lookup"><span data-stu-id="65ae1-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour LucidChart app.</span></span> <span data-ttu-id="65ae1-114">När du valt, kan du tilldela dessa användare tooyour LucidChart app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="65ae1-114">Once decided, you can assign these users tooyour LucidChart app by following hello instructions here:</span></span>

[<span data-ttu-id="65ae1-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="65ae1-115">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolucidchart"></a><span data-ttu-id="65ae1-116">Viktiga tips för att tilldela användare tooLucidChart</span><span class="sxs-lookup"><span data-stu-id="65ae1-116">Important tips for assigning users tooLucidChart</span></span>

*   <span data-ttu-id="65ae1-117">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooLucidChart tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="65ae1-117">It is recommended that a single Azure AD user is assigned tooLucidChart tootest hello provisioning configuration.</span></span> <span data-ttu-id="65ae1-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="65ae1-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="65ae1-119">När du tilldelar en användare tooLucidChart, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="65ae1-119">When assigning a user tooLucidChart, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="65ae1-120">Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.</span><span class="sxs-lookup"><span data-stu-id="65ae1-120">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toolucidchart"></a><span data-ttu-id="65ae1-121">Konfigurera användaretablering tooLucidChart</span><span class="sxs-lookup"><span data-stu-id="65ae1-121">Configuring user provisioning tooLucidChart</span></span> 

<span data-ttu-id="65ae1-122">Det här avsnittet hjälper dig att ansluta din Azure AD-tooLucidChart användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i LucidChart baserat på tilldelning av användare och grupper i Azure AD .</span><span class="sxs-lookup"><span data-stu-id="65ae1-122">This section guides you through connecting your Azure AD tooLucidChart's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in LucidChart based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="65ae1-123">Du kan också välja tooenabled SAML-baserade enkel inloggning för LucidChart, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="65ae1-123">You may also choose tooenabled SAML-based Single Sign-On for LucidChart, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="65ae1-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="65ae1-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toolucidchart-in-azure-ad"></a><span data-ttu-id="65ae1-125">Konfigurera automatisk användarkonto etablering tooLucidChart i Azure AD</span><span class="sxs-lookup"><span data-stu-id="65ae1-125">Configure automatic user account provisioning tooLucidChart in Azure AD</span></span>


1. <span data-ttu-id="65ae1-126">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="65ae1-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="65ae1-127">Om du redan har konfigurerat LucidChart för enkel inloggning, söka efter din instans av LucidChart hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="65ae1-127">If you have already configured LucidChart for single sign-on, search for your instance of LucidChart using hello search field.</span></span> <span data-ttu-id="65ae1-128">Annars väljer **Lägg till** och Sök efter **LucidChart** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="65ae1-128">Otherwise, select **Add** and search for **LucidChart** in hello application gallery.</span></span> <span data-ttu-id="65ae1-129">Välj LucidChart från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="65ae1-129">Select LucidChart from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="65ae1-130">Välj din instans av LucidChart och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="65ae1-130">Select your instance of LucidChart, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="65ae1-131">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="65ae1-131">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![LucidChart etablering](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. <span data-ttu-id="65ae1-133">Under hello **administratörsautentiseringsuppgifter** avsnitt, inkommande hello **hemlighet Token** genereras av din LucidChart konto (du kan hitta hello token under kontot: **Team**  >  **Appintegrering** > **SCIM**).</span><span class="sxs-lookup"><span data-stu-id="65ae1-133">Under hello **Admin Credentials** section, input hello **Secret Token** generated by your LucidChart's account (you can find hello token under your account: **Team** > **App Integration** > **SCIM**).</span></span> 

    ![LucidChart etablering](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. <span data-ttu-id="65ae1-135">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour LucidChart app.</span><span class="sxs-lookup"><span data-stu-id="65ae1-135">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour LucidChart app.</span></span> <span data-ttu-id="65ae1-136">Om hello anslutningen misslyckas, kontrollera kontots LucidChart har administratörsbehörigheter och försök steg 5 igen.</span><span class="sxs-lookup"><span data-stu-id="65ae1-136">If hello connection fails, ensure your LucidChart account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="65ae1-137">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.</span><span class="sxs-lookup"><span data-stu-id="65ae1-137">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="65ae1-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="65ae1-138">Click **Save**.</span></span> 

9. <span data-ttu-id="65ae1-139">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooLucidChart**.</span><span class="sxs-lookup"><span data-stu-id="65ae1-139">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooLucidChart**.</span></span>

10. <span data-ttu-id="65ae1-140">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooLucidChart.</span><span class="sxs-lookup"><span data-stu-id="65ae1-140">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooLucidChart.</span></span> <span data-ttu-id="65ae1-141">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i LucidChart för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="65ae1-141">hello attributes selected as **Matching** properties are used toomatch hello user accounts in LucidChart for update operations.</span></span> <span data-ttu-id="65ae1-142">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="65ae1-142">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="65ae1-143">tooenable hello Azure AD-etablering tjänsten för LucidChart, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="65ae1-143">tooenable hello Azure AD provisioning service for LucidChart, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="65ae1-144">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="65ae1-144">Click **Save**.</span></span> 

<span data-ttu-id="65ae1-145">Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooLucidChart i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="65ae1-145">This operation starts hello initial synchronization of any users and/or groups assigned tooLucidChart in hello Users and Groups section.</span></span> <span data-ttu-id="65ae1-146">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="65ae1-146">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="65ae1-147">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.</span><span class="sxs-lookup"><span data-stu-id="65ae1-147">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="65ae1-148">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="65ae1-148">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="65ae1-149">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="65ae1-149">Additional resources</span></span>

* [<span data-ttu-id="65ae1-150">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="65ae1-150">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="65ae1-151">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="65ae1-151">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="65ae1-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65ae1-152">Next steps</span></span>

* [<span data-ttu-id="65ae1-153">Lär dig hur tooreview loggar och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="65ae1-153">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
