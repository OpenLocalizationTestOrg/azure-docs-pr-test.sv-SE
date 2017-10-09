---
title: "Självstudier: Azure Active Directory-integrering med Salesforce | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a916be8dbf0b4c6173cda873936a53cd1f3ff12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a><span data-ttu-id="8ac31-103">Självstudier: Konfigurera Salesforce för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="8ac31-103">Tutorial: Configuring Salesforce for Automatic User Provisioning</span></span>

<span data-ttu-id="8ac31-104">hello syftet med den här kursen är tooshow hello steg krävs tooperform i Salesforce och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="8ac31-104">hello objective of this tutorial is tooshow hello steps required tooperform in Salesforce and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooSalesforce.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ac31-105">Krav</span><span class="sxs-lookup"><span data-stu-id="8ac31-105">Prerequisites</span></span>

<span data-ttu-id="8ac31-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8ac31-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="8ac31-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="8ac31-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="8ac31-108">Du måste ha en giltig klient för Salesforce för arbets- eller Salesforce för utbildning.</span><span class="sxs-lookup"><span data-stu-id="8ac31-108">You must have a valid tenant for Salesforce for Work or Salesforce for Education.</span></span> <span data-ttu-id="8ac31-109">Du kan använda ett kostnadsfritt utvärderingskonto för antingen service.</span><span class="sxs-lookup"><span data-stu-id="8ac31-109">You may use a free trial     account for either service.</span></span>
*   <span data-ttu-id="8ac31-110">Ett användarkonto i Salesforce-teamet administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="8ac31-110">A user account in Salesforce with Team Admin permissions.</span></span>

## <a name="assigning-users-toosalesforce"></a><span data-ttu-id="8ac31-111">Tilldela användare tooSalesforce</span><span class="sxs-lookup"><span data-stu-id="8ac31-111">Assigning users tooSalesforce</span></span>

<span data-ttu-id="8ac31-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="8ac31-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="8ac31-113">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="8ac31-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="8ac31-114">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Salesforce-app.</span><span class="sxs-lookup"><span data-stu-id="8ac31-114">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Salesforce app.</span></span> <span data-ttu-id="8ac31-115">När du valt, kan du tilldela dessa användare tooyour Salesforce-app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="8ac31-115">Once decided, you can assign these users tooyour Salesforce app by following hello instructions here:</span></span>

[<span data-ttu-id="8ac31-116">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="8ac31-116">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce"></a><span data-ttu-id="8ac31-117">Viktiga tips för att tilldela användare tooSalesforce</span><span class="sxs-lookup"><span data-stu-id="8ac31-117">Important tips for assigning users tooSalesforce</span></span>

*   <span data-ttu-id="8ac31-118">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooSalesforce tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8ac31-118">It is recommended that a single Azure AD user is assigned tooSalesforce tootest hello provisioning configuration.</span></span> <span data-ttu-id="8ac31-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="8ac31-119">Additional users and/or groups may be assigned later.</span></span>

*  <span data-ttu-id="8ac31-120">Du måste välja en giltig användarroll när du tilldelar en tooSalesforce för användaren.</span><span class="sxs-lookup"><span data-stu-id="8ac31-120">When assigning a user tooSalesforce, you must select a valid user role.</span></span> <span data-ttu-id="8ac31-121">Hej ”standard” rollen fungerar inte för etablering</span><span class="sxs-lookup"><span data-stu-id="8ac31-121">hello "Default Access" role does not work for provisioning</span></span>

    > [!NOTE]
    > <span data-ttu-id="8ac31-122">Den här appen importerar anpassade roller från Salesforce som en del av hello etableringsprocessen, vilken hello-kund vill kanske tooselect när du tilldelar användare</span><span class="sxs-lookup"><span data-stu-id="8ac31-122">This app imports custom roles from Salesforce as part of hello provisioning process, which hello customer may want tooselect when assigning users</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="8ac31-123">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="8ac31-123">Enable Automated User Provisioning</span></span>

<span data-ttu-id="8ac31-124">Det här avsnittet hjälper dig att ansluta din Azure AD-tooSalesforce användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Salesforce baserat på tilldelning av användare och grupper i Azure AD .</span><span class="sxs-lookup"><span data-stu-id="8ac31-124">This section guides you through connecting your Azure AD tooSalesforce's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Salesforce based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="8ac31-125">Du kan också välja tooenabled SAML-baserade enkel inloggning för Salesforce, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8ac31-125">You may also choose tooenabled SAML-based Single Sign-On for Salesforce, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8ac31-126">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="8ac31-126">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="8ac31-127">tooconfigure automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="8ac31-127">tooconfigure automatic user account provisioning:</span></span>

<span data-ttu-id="8ac31-128">hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="8ac31-128">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooSalesforce.</span></span>

1. <span data-ttu-id="8ac31-129">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8ac31-129">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="8ac31-130">Om du redan har konfigurerat Salesforce för enkel inloggning söka efter din instans av Salesforce hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="8ac31-130">If you have already configured Salesforce for single sign-on, search for your instance of Salesforce using hello search field.</span></span> <span data-ttu-id="8ac31-131">Annars väljer **Lägg till** och Sök efter **Salesforce** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="8ac31-131">Otherwise, select **Add** and search for **Salesforce** in hello application gallery.</span></span> <span data-ttu-id="8ac31-132">Välj Salesforce från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="8ac31-132">Select Salesforce from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="8ac31-133">Välj din instans av Salesforce och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="8ac31-133">Select your instance of Salesforce, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="8ac31-134">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="8ac31-134">Set hello **Provisioning Mode** too**Automatic**.</span></span> 
<span data-ttu-id="8ac31-135">![etablering](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span><span class="sxs-lookup"><span data-stu-id="8ac31-135">![provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)</span></span>

5. <span data-ttu-id="8ac31-136">Under hello **administratörsautentiseringsuppgifter** avsnittet, ange hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="8ac31-136">Under hello **Admin Credentials** section, provide hello following configuration settings:</span></span>
   
    <span data-ttu-id="8ac31-137">a.</span><span class="sxs-lookup"><span data-stu-id="8ac31-137">a.</span></span> <span data-ttu-id="8ac31-138">I hello **administratörsanvändarnamnet** textruta typen en Salesforce-kontonamn som har hello **systemadministratören** profil i Salesforce.com som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="8ac31-138">In hello **Admin User Name** textbox, type a Salesforce account name that has hello **System Administrator** profile in Salesforce.com assigned.</span></span>
   
    <span data-ttu-id="8ac31-139">b.</span><span class="sxs-lookup"><span data-stu-id="8ac31-139">b.</span></span> <span data-ttu-id="8ac31-140">I hello **adminlösenord** textruta typen hello lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="8ac31-140">In hello **Admin Password** textbox, type hello password for this account.</span></span>

6. <span data-ttu-id="8ac31-141">tooget Salesforce säkerhets-token öppnar en ny flik och logga in på hello samma Salesforce-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="8ac31-141">tooget your Salesforce security token, open a new tab and sign into hello same Salesforce admin account.</span></span> <span data-ttu-id="8ac31-142">Klicka på ditt namn i hello övre högra hörnet av hello-sidan, och klicka sedan på **Mina inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8ac31-142">On hello top right corner of hello page, click your name, and then click **My Settings**.</span></span>

     <span data-ttu-id="8ac31-143">![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="8ac31-143">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "Enable automatic user provisioning")</span></span>
7. <span data-ttu-id="8ac31-144">På hello vänstra navigationsfönstret klickar du på **personliga** tooexpand hello Närliggande avsnitt och klickar sedan på **Återställ mina säkerhetstoken**.</span><span class="sxs-lookup"><span data-stu-id="8ac31-144">On hello left navigation pane, click **Personal** tooexpand hello related section, and then click **Reset My Security Token**.</span></span>
  
    <span data-ttu-id="8ac31-145">![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="8ac31-145">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "Enable automatic user provisioning")</span></span>
8. <span data-ttu-id="8ac31-146">På hello **Återställ mina säkerhetstoken** klickar du på **återställa säkerhetstoken** knappen.</span><span class="sxs-lookup"><span data-stu-id="8ac31-146">On hello **Reset My Security Token** page, click **Reset Security Token** button.</span></span>

    <span data-ttu-id="8ac31-147">![Aktivera automatisk användaretablering](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="8ac31-147">![Enable automatic user provisioning](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "Enable automatic user provisioning")</span></span>
9. <span data-ttu-id="8ac31-148">Kontrollera hello inkorg som är associerade med den här administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="8ac31-148">Check hello email inbox associated with this admin account.</span></span> <span data-ttu-id="8ac31-149">Leta efter ett e-postmeddelande från Salesforce.com som innehåller hello ny säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="8ac31-149">Look for an email from Salesforce.com that contains hello new security token.</span></span>
10. <span data-ttu-id="8ac31-150">Kopiera hello token, gå tooyour Azure AD-fönstret och klistra in den i hello **Socket Token** fältet.</span><span class="sxs-lookup"><span data-stu-id="8ac31-150">Copy hello token, go tooyour Azure AD window, and paste it into hello **Socket Token** field.</span></span>

11. <span data-ttu-id="8ac31-151">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Salesforce-app.</span><span class="sxs-lookup"><span data-stu-id="8ac31-151">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Salesforce app.</span></span>

12. <span data-ttu-id="8ac31-152">I hello **e-postmeddelande** anger hello e-postadressen för en person eller grupp som ska ta emot meddelanden om etablering fel och markera kryssrutan hello nedan.</span><span class="sxs-lookup"><span data-stu-id="8ac31-152">In hello **Notification Email** field, enter hello email address of a person or group who should receive provisioning error notifications, and check hello checkbox below.</span></span>

13. <span data-ttu-id="8ac31-153">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="8ac31-153">Click **Save.**</span></span>  
    
14.  <span data-ttu-id="8ac31-154">Välj under hello mappningar avsnitt, **tooSalesforce synkronisera Azure Active Directory-användare.**</span><span class="sxs-lookup"><span data-stu-id="8ac31-154">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooSalesforce.**</span></span>

15. <span data-ttu-id="8ac31-155">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="8ac31-155">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooSalesforce.</span></span> <span data-ttu-id="8ac31-156">Observera att hello attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Salesforce för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="8ac31-156">Note that hello attributes selected as **Matching** properties are used toomatch hello user accounts in Salesforce for update operations.</span></span> <span data-ttu-id="8ac31-157">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="8ac31-157">Select hello Save button toocommit any changes.</span></span>

16. <span data-ttu-id="8ac31-158">tooenable hello Azure AD-etablering tjänsten för Salesforce, ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="8ac31-158">tooenable hello Azure AD provisioning service for Salesforce, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

17. <span data-ttu-id="8ac31-159">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="8ac31-159">Click **Save.**</span></span>

<span data-ttu-id="8ac31-160">Detta startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooSalesforce i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8ac31-160">This starts hello initial synchronization of any users and/or groups assigned tooSalesforce in hello Users and Groups section.</span></span> <span data-ttu-id="8ac31-161">Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="8ac31-161">Note that hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="8ac31-162">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras på ditt Salesforce-app.</span><span class="sxs-lookup"><span data-stu-id="8ac31-162">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Salesforce app.</span></span>

<span data-ttu-id="8ac31-163">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="8ac31-163">You can now create a test account.</span></span> <span data-ttu-id="8ac31-164">Vänta tills in tooverify hello kontot har synkroniserats tooSalesforce too20 i minuter.</span><span class="sxs-lookup"><span data-stu-id="8ac31-164">Wait for up too20 minutes tooverify that hello account has been synchronized tooSalesforce.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ac31-165">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8ac31-165">Additional resources</span></span>

* [<span data-ttu-id="8ac31-166">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="8ac31-166">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ac31-167">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ac31-167">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="8ac31-168">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ac31-168">Configure Single Sign-on</span></span>](active-directory-saas-salesforce-tutorial.md)