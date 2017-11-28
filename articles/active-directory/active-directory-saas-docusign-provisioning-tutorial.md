---
title: "Självstudier: Azure Active Directory-integrering med DocuSign | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 8562a8f9e05fb72d3331507b7da5c6afee38f9b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a><span data-ttu-id="4b4f3-103">Självstudier: Konfigurera DocuSign för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="4b4f3-103">Tutorial: Configuring DocuSign for User Provisioning</span></span>

<span data-ttu-id="4b4f3-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i DocuSign och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooDocuSign.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in DocuSign and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooDocuSign.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b4f3-105">Krav</span><span class="sxs-lookup"><span data-stu-id="4b4f3-105">Prerequisites</span></span>

<span data-ttu-id="4b4f3-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4b4f3-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="4b4f3-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="4b4f3-108">Aktivera prenumerationen en DocuSign enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-108">A DocuSign single sign-on enabled subscription.</span></span>
*   <span data-ttu-id="4b4f3-109">Ett användarkonto i DocuSign Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-109">A user account in DocuSign with Team Admin permissions.</span></span>

## <a name="assigning-users-toodocusign"></a><span data-ttu-id="4b4f3-110">Tilldela användare tooDocuSign</span><span class="sxs-lookup"><span data-stu-id="4b4f3-110">Assigning users tooDocuSign</span></span>

<span data-ttu-id="4b4f3-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="4b4f3-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserade.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span>

<span data-ttu-id="4b4f3-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour DocuSign app.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour DocuSign app.</span></span> <span data-ttu-id="4b4f3-114">När du valt, kan du tilldela dessa användare tooyour DocuSign app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="4b4f3-114">Once decided, you can assign these users tooyour DocuSign app by following hello instructions here:</span></span>

[<span data-ttu-id="4b4f3-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="4b4f3-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodocusign"></a><span data-ttu-id="4b4f3-116">Viktiga tips för att tilldela användare tooDocuSign</span><span class="sxs-lookup"><span data-stu-id="4b4f3-116">Important tips for assigning users tooDocuSign</span></span>

*   <span data-ttu-id="4b4f3-117">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooDocuSign tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-117">It is recommended that a single Azure AD user is assigned tooDocuSign tootest hello provisioning configuration.</span></span> <span data-ttu-id="4b4f3-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="4b4f3-119">Du måste välja en giltig användarroll när du tilldelar en tooDocuSign för användaren.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-119">When assigning a user tooDocuSign, you must select a valid user role.</span></span> <span data-ttu-id="4b4f3-120">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="4b4f3-121">Aktivera etablering av användare</span><span class="sxs-lookup"><span data-stu-id="4b4f3-121">Enable User Provisioning</span></span>

<span data-ttu-id="4b4f3-122">Det här avsnittet hjälper dig att ansluta din Azure AD-tooDocuSign användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i DocuSign baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-122">This section guides you through connecting your Azure AD tooDocuSign's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in DocuSign based on user and group assignment in Azure AD.</span></span>

> [!Tip]
> <span data-ttu-id="4b4f3-123">Du kan också välja tooenabled SAML-baserade enkel inloggning för DocuSign, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4b4f3-123">You may also choose tooenabled SAML-based Single Sign-On for DocuSign, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="4b4f3-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="4b4f3-125">tooconfigure användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="4b4f3-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="4b4f3-126">hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooDocuSign.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-126">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooDocuSign.</span></span>

1. <span data-ttu-id="4b4f3-127">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-127">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="4b4f3-128">Om du redan har konfigurerat DocuSign för enkel inloggning, söka efter din instans av DocuSign hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-128">If you have already configured DocuSign for single sign-on, search for your instance of DocuSign using hello search field.</span></span> <span data-ttu-id="4b4f3-129">Annars väljer **Lägg till** och Sök efter **DocuSign** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-129">Otherwise, select **Add** and search for **DocuSign** in hello application gallery.</span></span> <span data-ttu-id="4b4f3-130">Välj DocuSign från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-130">Select DocuSign from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="4b4f3-131">Välj din instans av DocuSign och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-131">Select your instance of DocuSign, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="4b4f3-132">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-132">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="4b4f3-134">Under hello **administratörsautentiseringsuppgifter** avsnittet, ange hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="4b4f3-134">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="4b4f3-135">a.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-135">a.</span></span> <span data-ttu-id="4b4f3-136">I hello **administratörsanvändarnamnet** textruta typen en DocuSign kontonamn som har hello **systemadministratören** profil i DocuSign.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-136">In hello **Admin User Name** textbox, type a DocuSign account name that has hello **System Administrator** profile in DocuSign.com assigned.</span></span>
   
    <span data-ttu-id="4b4f3-137">b.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-137">b.</span></span> <span data-ttu-id="4b4f3-138">I hello **adminlösenord** textruta typen hello lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-138">In hello **Admin Password** textbox, type hello password for this account.</span></span>

6. <span data-ttu-id="4b4f3-139">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour DocuSign app.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-139">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour DocuSign app.</span></span>

7. <span data-ttu-id="4b4f3-140">I hello **e-postmeddelande** anger hello e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-140">In hello **Notification Email** field, enter hello email address of a person or group who should receive provisioning error notifications, and check hello checkbox.</span></span>

8. <span data-ttu-id="4b4f3-141">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="4b4f3-141">Click **Save.**</span></span>

9. <span data-ttu-id="4b4f3-142">Välj under hello mappningar avsnitt, **tooDocuSign synkronisera Azure Active Directory-användare.**</span><span class="sxs-lookup"><span data-stu-id="4b4f3-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooDocuSign.**</span></span>

10. <span data-ttu-id="4b4f3-143">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooDocuSign.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooDocuSign.</span></span> <span data-ttu-id="4b4f3-144">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i DocuSign för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in DocuSign for update operations.</span></span> <span data-ttu-id="4b4f3-145">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="4b4f3-146">tooenable hello Azure AD-etablering tjänsten för DocuSign, ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="4b4f3-146">tooenable hello Azure AD provisioning service for DocuSign, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="4b4f3-147">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="4b4f3-147">Click **Save.**</span></span>

<span data-ttu-id="4b4f3-148">Den startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooDocuSign i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-148">It starts hello initial synchronization of any users and/or groups assigned tooDocuSign in hello Users and Groups section.</span></span> <span data-ttu-id="4b4f3-149">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="4b4f3-150">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras DocuSign appen.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your DocuSign app.</span></span>

<span data-ttu-id="4b4f3-151">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-151">You can now create a test account.</span></span> <span data-ttu-id="4b4f3-152">Vänta tills in tooverify hello kontot har synkroniserats tooDocuSign too20 i minuter.</span><span class="sxs-lookup"><span data-stu-id="4b4f3-152">Wait for up too20 minutes tooverify that hello account has been synchronized tooDocuSign.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b4f3-153">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4b4f3-153">Additional resources</span></span>

* [<span data-ttu-id="4b4f3-154">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="4b4f3-154">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4b4f3-155">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4b4f3-155">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="4b4f3-156">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4b4f3-156">Configure Single Sign-on</span></span>](active-directory-saas-docusign-tutorial.md)