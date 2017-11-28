---
title: "Självstudier: Azure Active Directory-integrering med Jive | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: b1c0d0bc2d79427c055f577fe5f9d30d10f1bbdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-jive-for-user-provisioning"></a><span data-ttu-id="cad0b-103">Självstudier: Konfigurera Jive för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="cad0b-103">Tutorial: Configuring Jive for User Provisioning</span></span>

<span data-ttu-id="cad0b-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Jive och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooJive.</span><span class="sxs-lookup"><span data-stu-id="cad0b-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Jive and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooJive.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cad0b-105">Krav</span><span class="sxs-lookup"><span data-stu-id="cad0b-105">Prerequisites</span></span>

<span data-ttu-id="cad0b-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cad0b-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="cad0b-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="cad0b-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="cad0b-108">En Jive enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="cad0b-108">A Jive single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="cad0b-109">Ett användarkonto i Jive Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="cad0b-109">A user account in Jive with Team Admin permissions.</span></span>

## <a name="assigning-users-toojive"></a><span data-ttu-id="cad0b-110">Tilldela användare tooJive</span><span class="sxs-lookup"><span data-stu-id="cad0b-110">Assigning users tooJive</span></span>

<span data-ttu-id="cad0b-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="cad0b-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="cad0b-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="cad0b-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="cad0b-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Jive app.</span><span class="sxs-lookup"><span data-stu-id="cad0b-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Jive app.</span></span> <span data-ttu-id="cad0b-114">När du valt, kan du tilldela dessa användare tooyour Jive app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="cad0b-114">Once decided, you can assign these users tooyour Jive app by following hello instructions here:</span></span>

[<span data-ttu-id="cad0b-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="cad0b-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toojive"></a><span data-ttu-id="cad0b-116">Viktiga tips för att tilldela användare tooJive</span><span class="sxs-lookup"><span data-stu-id="cad0b-116">Important tips for assigning users tooJive</span></span>

*   <span data-ttu-id="cad0b-117">Vi rekommenderar att en enda Azure AD-användare tilldelas tooJive tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="cad0b-117">It is recommended that a single Azure AD user be assigned tooJive tootest hello provisioning configuration.</span></span> <span data-ttu-id="cad0b-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="cad0b-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="cad0b-119">Du måste välja en giltig användarroll när du tilldelar en tooJive för användaren.</span><span class="sxs-lookup"><span data-stu-id="cad0b-119">When assigning a user tooJive, you must select a valid user role.</span></span> <span data-ttu-id="cad0b-120">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="cad0b-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-user-provisioning"></a><span data-ttu-id="cad0b-121">Aktivera etablering av användare</span><span class="sxs-lookup"><span data-stu-id="cad0b-121">Enable User Provisioning</span></span>

<span data-ttu-id="cad0b-122">Det här avsnittet hjälper dig att ansluta din Azure AD-tooJive användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Jive baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cad0b-122">This section guides you through connecting your Azure AD tooJive's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Jive based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="cad0b-123">Du kan också välja tooenabled SAML-baserade enkel inloggning för Jive, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cad0b-123">You may also choose tooenabled SAML-based Single Sign-On for Jive, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="cad0b-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="cad0b-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-user-account-provisioning"></a><span data-ttu-id="cad0b-125">tooconfigure användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="cad0b-125">tooconfigure user account provisioning:</span></span>

<span data-ttu-id="cad0b-126">hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooJive.</span><span class="sxs-lookup"><span data-stu-id="cad0b-126">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooJive.</span></span>
<span data-ttu-id="cad0b-127">Som en del av den här proceduren är obligatoriska tooprovide en säkerhetstoken för användaren som du behöver toorequest från Jive.com.</span><span class="sxs-lookup"><span data-stu-id="cad0b-127">As part of this procedure, you are required tooprovide a user security token you need toorequest from Jive.com.</span></span>

1. <span data-ttu-id="cad0b-128">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cad0b-128">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="cad0b-129">Om du redan har konfigurerat Jive för enkel inloggning, söka efter din instans av Jive hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="cad0b-129">If you have already configured Jive for single sign-on, search for your instance of Jive using hello search field.</span></span> <span data-ttu-id="cad0b-130">Annars väljer **Lägg till** och Sök efter **Jive** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="cad0b-130">Otherwise, select **Add** and search for **Jive** in hello application gallery.</span></span> <span data-ttu-id="cad0b-131">Välj Jive från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="cad0b-131">Select Jive from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="cad0b-132">Välj din instans av Jive och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="cad0b-132">Select your instance of Jive, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="cad0b-133">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="cad0b-133">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-jive-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="cad0b-135">Under hello **administratörsautentiseringsuppgifter** avsnittet, ange hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="cad0b-135">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="cad0b-136">a.</span><span class="sxs-lookup"><span data-stu-id="cad0b-136">a.</span></span> <span data-ttu-id="cad0b-137">I hello **Jive administratörsanvändarnamnet** textruta typen en Jive kontonamn som har hello **systemadministratören** profil i Jive.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="cad0b-137">In hello **Jive Admin User Name** textbox, type a Jive account name that has hello **System Administrator** profile in Jive.com assigned.</span></span>
   
    <span data-ttu-id="cad0b-138">b.</span><span class="sxs-lookup"><span data-stu-id="cad0b-138">b.</span></span> <span data-ttu-id="cad0b-139">I hello **Jive adminlösenord** textruta typen hello lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="cad0b-139">In hello **Jive Admin Password** textbox, type hello password for this account.</span></span>
   
    <span data-ttu-id="cad0b-140">c.</span><span class="sxs-lookup"><span data-stu-id="cad0b-140">c.</span></span> <span data-ttu-id="cad0b-141">I hello **Jive klient URL** textruta typen hello Jive klient-URL.</span><span class="sxs-lookup"><span data-stu-id="cad0b-141">In hello **Jive Tenant URL** textbox, type hello Jive tenant URL.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="cad0b-142">Hej Jive klient URL är URL: en som används av din organisation toolog i tooJive.</span><span class="sxs-lookup"><span data-stu-id="cad0b-142">hello Jive tenant URL is URL that is used by your organization toolog in tooJive.</span></span>  
      > <span data-ttu-id="cad0b-143">Normalt hello URL har hello följande format: **www.\< organisation\>. jive.com**.</span><span class="sxs-lookup"><span data-stu-id="cad0b-143">Typically, hello URL has hello following format: **www.\<organization\>.jive.com**.</span></span>          

6. <span data-ttu-id="cad0b-144">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Jive app.</span><span class="sxs-lookup"><span data-stu-id="cad0b-144">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Jive app.</span></span>

7. <span data-ttu-id="cad0b-145">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.</span><span class="sxs-lookup"><span data-stu-id="cad0b-145">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

8. <span data-ttu-id="cad0b-146">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="cad0b-146">Click **Save.**</span></span>

9. <span data-ttu-id="cad0b-147">Välj under hello mappningar avsnitt, **tooJive synkronisera Azure Active Directory-användare.**</span><span class="sxs-lookup"><span data-stu-id="cad0b-147">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooJive.**</span></span>

10. <span data-ttu-id="cad0b-148">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooJive.</span><span class="sxs-lookup"><span data-stu-id="cad0b-148">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooJive.</span></span> <span data-ttu-id="cad0b-149">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Jive för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="cad0b-149">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Jive for update operations.</span></span> <span data-ttu-id="cad0b-150">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="cad0b-150">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="cad0b-151">tooenable hello Azure AD-etablering tjänsten för Jive, ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="cad0b-151">tooenable hello Azure AD provisioning service for Jive, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="cad0b-152">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="cad0b-152">Click **Save.**</span></span>

<span data-ttu-id="cad0b-153">Den startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooJive i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cad0b-153">It starts hello initial synchronization of any users and/or groups assigned tooJive in hello Users and Groups section.</span></span> <span data-ttu-id="cad0b-154">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="cad0b-154">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="cad0b-155">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras Jive appen.</span><span class="sxs-lookup"><span data-stu-id="cad0b-155">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Jive app.</span></span>

<span data-ttu-id="cad0b-156">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="cad0b-156">You can now create a test account.</span></span> <span data-ttu-id="cad0b-157">Vänta tills in tooverify hello kontot har synkroniserats tooJive too20 i minuter.</span><span class="sxs-lookup"><span data-stu-id="cad0b-157">Wait for up too20 minutes tooverify that hello account has been synchronized tooJive.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cad0b-158">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cad0b-158">Additional resources</span></span>

* [<span data-ttu-id="cad0b-159">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="cad0b-159">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cad0b-160">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cad0b-160">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="cad0b-161">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cad0b-161">Configure Single Sign-on</span></span>](active-directory-saas-jive-tutorial.md)