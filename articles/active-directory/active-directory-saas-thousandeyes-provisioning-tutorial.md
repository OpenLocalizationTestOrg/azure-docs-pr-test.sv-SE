---
title: "Självstudier: Konfigurera ThousandEyes för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooThousandEyes."
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
ms.openlocfilehash: f31883ab685d0ffcd9a830aa4a7d43c056f5f4cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-thousandeyes-for-automatic-user-provisioning"></a><span data-ttu-id="2c980-103">Självstudier: Konfigurera ThousandEyes för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="2c980-103">Tutorial: Configuring ThousandEyes for Automatic User Provisioning</span></span>


<span data-ttu-id="2c980-104">hello syftet med den här kursen är tooshow du hello steg du behöver tooperform i ThousandEyes och Azure AD tooautomatically etablera och avetablera användarkonton från Azure AD tooThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="2c980-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in ThousandEyes and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooThousandEyes.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2c980-105">Krav</span><span class="sxs-lookup"><span data-stu-id="2c980-105">Prerequisites</span></span>

<span data-ttu-id="2c980-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2c980-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="2c980-107">En Azure Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="2c980-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="2c980-108">En ThousandEyes klient med hello [standardplan](https://www.thousandeyes.com/pricing) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="2c980-108">A ThousandEyes tenant with hello [Standard plan](https://www.thousandeyes.com/pricing) or better enabled</span></span> 
*   <span data-ttu-id="2c980-109">Ett användarkonto i ThousandEyes med administratörsbehörigheter</span><span class="sxs-lookup"><span data-stu-id="2c980-109">A user account in ThousandEyes with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="2c980-110">hello Azure AD-etablering integration förlitar sig på hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), som är tillgängliga tooThousandEyes team på hello standardplan eller bättre.</span><span class="sxs-lookup"><span data-stu-id="2c980-110">hello Azure AD provisioning integration relies on hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), which is available tooThousandEyes teams on hello Standard plan or better.</span></span>

## <a name="assigning-users-toothousandeyes"></a><span data-ttu-id="2c980-111">Tilldela användare tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="2c980-111">Assigning users tooThousandEyes</span></span>

<span data-ttu-id="2c980-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="2c980-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="2c980-113">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="2c980-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="2c980-114">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour ThousandEyes app.</span><span class="sxs-lookup"><span data-stu-id="2c980-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour ThousandEyes app.</span></span> <span data-ttu-id="2c980-115">När du valt, kan du tilldela dessa användare tooyour ThousandEyes app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="2c980-115">Once decided, you can assign these users tooyour ThousandEyes app by following hello instructions here:</span></span>

[<span data-ttu-id="2c980-116">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="2c980-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toothousandeyes"></a><span data-ttu-id="2c980-117">Viktiga tips för att tilldela användare tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="2c980-117">Important tips for assigning users tooThousandEyes</span></span>

*   <span data-ttu-id="2c980-118">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooThousandEyes tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2c980-118">It is recommended that a single Azure AD user is assigned tooThousandEyes tootest hello provisioning configuration.</span></span> <span data-ttu-id="2c980-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="2c980-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="2c980-120">När du tilldelar en användare tooThousandEyes, måste du välja antingen hello **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="2c980-120">When assigning a user tooThousandEyes, you must select either hello **User** role, or another valid application-specific role (if available) in hello assignment dialog.</span></span> <span data-ttu-id="2c980-121">Hej **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.</span><span class="sxs-lookup"><span data-stu-id="2c980-121">hello **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-toothousandeyes"></a><span data-ttu-id="2c980-122">Konfigurera användaretablering tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="2c980-122">Configuring user provisioning tooThousandEyes</span></span> 

<span data-ttu-id="2c980-123">Det här avsnittet hjälper dig att ansluta din Azure AD-tooThousandEyes användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i ThousandEyes baserat på användare och grupper tilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c980-123">This section guides you through connecting your Azure AD tooThousandEyes's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in ThousandEyes based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="2c980-124">Du kan också välja tooenabled SAML-baserade enkel inloggning för ThousandEyes, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2c980-124">You may also choose tooenabled SAML-based Single Sign-On for ThousandEyes, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="2c980-125">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="2c980-125">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-toothousandeyes-in-azure-ad"></a><span data-ttu-id="2c980-126">Konfigurera automatisk användarkonto etablering tooThousandEyes i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c980-126">Configure automatic user account provisioning tooThousandEyes in Azure AD</span></span>


1. <span data-ttu-id="2c980-127">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2c980-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="2c980-128">Om du redan har konfigurerat ThousandEyes för enkel inloggning, söka efter din instans av ThousandEyes hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="2c980-128">If you have already configured ThousandEyes for single sign-on, search for your instance of ThousandEyes using hello search field.</span></span> <span data-ttu-id="2c980-129">Annars väljer **Lägg till** och Sök efter **ThousandEyes** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="2c980-129">Otherwise, select **Add** and search for **ThousandEyes** in hello application gallery.</span></span> <span data-ttu-id="2c980-130">Välj ThousandEyes från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="2c980-130">Select ThousandEyes from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="2c980-131">Välj din instans av ThousandEyes och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="2c980-131">Select your instance of ThousandEyes, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="2c980-132">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="2c980-132">Set hello **Provisioning Mode** too**Automatic**.</span></span>

    ![ThousandEyes etablering](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. <span data-ttu-id="2c980-134">Under hello **administratörsautentiseringsuppgifter** avsnitt, inkommande hello **hemlighet Token** genereras av din ThousandEyes konto (du kan hitta hello token under kontot ThousandEyes: **säkerhet & Autentisering**).</span><span class="sxs-lookup"><span data-stu-id="2c980-134">Under hello **Admin Credentials** section, input hello **Secret Token** generated by your ThousandEyes's account (you can find hello token under your ThousandEyes account: **Security & Authentication**).</span></span> 

    ![ThousandEyes etablering](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. <span data-ttu-id="2c980-136">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour ThousandEyes app.</span><span class="sxs-lookup"><span data-stu-id="2c980-136">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour ThousandEyes app.</span></span> <span data-ttu-id="2c980-137">Om hello anslutningen misslyckas, kontrollera kontots ThousandEyes har administratörsbehörigheter och försök steg 5 igen.</span><span class="sxs-lookup"><span data-stu-id="2c980-137">If hello connection fails, ensure your ThousandEyes account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="2c980-138">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och kontrollera hello kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.</span><span class="sxs-lookup"><span data-stu-id="2c980-138">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="2c980-139">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2c980-139">Click **Save**.</span></span> 

9. <span data-ttu-id="2c980-140">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="2c980-140">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooThousandEyes**.</span></span>

10. <span data-ttu-id="2c980-141">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="2c980-141">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooThousandEyes.</span></span> <span data-ttu-id="2c980-142">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i ThousandEyes för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="2c980-142">hello attributes selected as **Matching** properties are used toomatch hello user accounts in ThousandEyes for update operations.</span></span> <span data-ttu-id="2c980-143">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="2c980-143">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="2c980-144">tooenable hello Azure AD-etablering tjänsten för ThousandEyes, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="2c980-144">tooenable hello Azure AD provisioning service for ThousandEyes, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

12. <span data-ttu-id="2c980-145">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2c980-145">Click **Save**.</span></span> 

<span data-ttu-id="2c980-146">Den här åtgärden startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooThousandEyes i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2c980-146">This operation starts hello initial synchronization of any users and/or groups assigned tooThousandEyes in hello Users and Groups section.</span></span> <span data-ttu-id="2c980-147">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="2c980-147">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="2c980-148">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras.</span><span class="sxs-lookup"><span data-stu-id="2c980-148">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service.</span></span>

<span data-ttu-id="2c980-149">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="2c980-149">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2c980-150">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2c980-150">Additional resources</span></span>

* [<span data-ttu-id="2c980-151">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="2c980-151">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="2c980-152">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2c980-152">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="2c980-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c980-153">Next steps</span></span>

* [<span data-ttu-id="2c980-154">Lär dig hur tooreview loggar och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="2c980-154">Learn how tooreview logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
