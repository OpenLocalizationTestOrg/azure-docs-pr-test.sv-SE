---
title: "Självstudier: Konfigurera LinkedIn försäljning Navigator för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera och avinstallation etablera användarkonton tooLinkedIn försäljning Navigator."
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
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: 322c5271535994c13a9fafadbf74f356cdfe865d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-sales-navigator-for-automatic-user-provisioning"></a><span data-ttu-id="de882-103">Självstudier: Konfigurera LinkedIn försäljning Navigator för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="de882-103">Tutorial: Configuring LinkedIn Sales Navigator for Automatic User Provisioning</span></span>


<span data-ttu-id="de882-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i LinkedIn försäljning Navigator och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooLinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="de882-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in LinkedIn Sales Navigator and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooLinkedIn Sales Navigator.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="de882-105">Krav</span><span class="sxs-lookup"><span data-stu-id="de882-105">Prerequisites</span></span>

<span data-ttu-id="de882-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="de882-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="de882-107">En Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="de882-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="de882-108">En LinkedIn försäljning Navigator-klient</span><span class="sxs-lookup"><span data-stu-id="de882-108">A LinkedIn Sales Navigator tenant</span></span> 
*   <span data-ttu-id="de882-109">Ett administratörskontot i LinkedIn försäljning Navigator med åtkomst toohello LinkedIn Account Center</span><span class="sxs-lookup"><span data-stu-id="de882-109">An administrator account in LinkedIn Sales Navigator with access toohello LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="de882-110">Azure Active Directory kan integreras med LinkedIn försäljning Navigator med hello [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="de882-110">Azure Active Directory integrates with LinkedIn Sales Navigator using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toolinkedin-sales-navigator"></a><span data-ttu-id="de882-111">Tilldela användare tooLinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="de882-111">Assigning users tooLinkedIn Sales Navigator</span></span>

<span data-ttu-id="de882-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="de882-112">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="de882-113">Hello gäller automatisk konto användaretablering är kommer bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD att synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="de882-113">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="de882-114">Innan du konfigurerar och aktiverar hello etableras, behöver du toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooLinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="de882-114">Before configuring and enabling hello provisioning service, you will need toodecide what users and/or groups in Azure AD represent hello users who need access tooLinkedIn Sales Navigator.</span></span> <span data-ttu-id="de882-115">När du valt, kan du tilldela dessa användare tooLinkedIn försäljning Navigator genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="de882-115">Once decided, you can assign these users tooLinkedIn Sales Navigator by following hello instructions here:</span></span>

[<span data-ttu-id="de882-116">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="de882-116">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-sales-navigator"></a><span data-ttu-id="de882-117">Viktiga tips för att tilldela användare tooLinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="de882-117">Important tips for assigning users tooLinkedIn Sales Navigator</span></span>

*   <span data-ttu-id="de882-118">Vi rekommenderar att en enda Azure AD-användare tilldelas tooLinkedIn försäljning Navigator tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="de882-118">It is recommended that a single Azure AD user be assigned tooLinkedIn Sales Navigator tootest hello provisioning configuration.</span></span> <span data-ttu-id="de882-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="de882-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="de882-120">När du tilldelar en användare tooLinkedIn försäljning Navigator, måste du välja hello **användaren** roll i dialogrutan för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="de882-120">When assigning a user tooLinkedIn Sales Navigator, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="de882-121">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="de882-121">hello "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-toolinkedin-sales-navigator"></a><span data-ttu-id="de882-122">Konfigurera användaretablering tooLinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="de882-122">Configuring user provisioning tooLinkedIn Sales Navigator</span></span>

<span data-ttu-id="de882-123">Det här avsnittet hjälper dig att ansluta din Azure AD tooLinkedIn försäljning Navigator och SCIM användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i LinkedIn försäljning Navigator baserat på användare och grupptilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="de882-123">This section guides you through connecting your Azure AD tooLinkedIn Sales Navigator's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update and disable assigned user accounts in LinkedIn Sales Navigator based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="de882-124">Du kan också välja tooenabled SAML-baserade enkel inloggning för LinkedIn försäljning Navigator följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de882-124">You may also choose tooenabled SAML-based Single Sign-On for LinkedIn Sales Navigator, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="de882-125">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.</span><span class="sxs-lookup"><span data-stu-id="de882-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-sales-navigator-in-azure-ad"></a><span data-ttu-id="de882-126">tooconfigure automatisk användarkonto etablering tooLinkedIn försäljning Navigator i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="de882-126">tooconfigure automatic user account provisioning tooLinkedIn Sales Navigator in Azure AD:</span></span>


<span data-ttu-id="de882-127">hello första steget är tooretrieve LinkedIn-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="de882-127">hello first step is tooretrieve your LinkedIn access token.</span></span> <span data-ttu-id="de882-128">Om du är företagsadministratör kan etablera du själv en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="de882-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="de882-129">Gå för i din kontocenter**inställningar &gt; globala inställningar** och öppna hello **SCIM installationsprogrammet** panelen.</span><span class="sxs-lookup"><span data-stu-id="de882-129">In your account center, go too**Settings &gt; Global Settings** and open hello **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="de882-130">Om du öppnar hello kontocenter direkt i stället för via en länk, kan du nå den med hjälp av hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="de882-130">If you are accessing hello account center directly rather than through a link, you can reach it using hello following steps.</span></span>

1)  <span data-ttu-id="de882-131">Logga in tooAccount Center.</span><span class="sxs-lookup"><span data-stu-id="de882-131">Sign in tooAccount Center.</span></span>

2)  <span data-ttu-id="de882-132">Välj **Admin &gt; administrationsinställningar** .</span><span class="sxs-lookup"><span data-stu-id="de882-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="de882-133">Klicka på **avancerade integreringar** på hello vänstra sidopanelen.</span><span class="sxs-lookup"><span data-stu-id="de882-133">Click **Advanced Integrations** on hello left sidebar.</span></span> <span data-ttu-id="de882-134">Du är riktat toohello kontocenter.</span><span class="sxs-lookup"><span data-stu-id="de882-134">You are directed toohello account center.</span></span>

4)  <span data-ttu-id="de882-135">Klicka på **+ Lägg till ny SCIM konfiguration** och följa hello genom att fylla i varje fält.</span><span class="sxs-lookup"><span data-stu-id="de882-135">Click **+ Add new SCIM configuration** and follow hello procedure by filling in each field.</span></span>

> <span data-ttu-id="de882-136">När autoassign licenser inte är aktiverad, innebär det att endast användardata är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="de882-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> <span data-ttu-id="de882-138">När autolicense tilldelning har aktiverats måste toonote programinstansen och licenstyp.</span><span class="sxs-lookup"><span data-stu-id="de882-138">When auto­license assignment is enabled, you need toonote the application instance and license type.</span></span> <span data-ttu-id="de882-139">Licenser är kopplade till en första komma, först hantera bas tills alla hello licenser tas.</span><span class="sxs-lookup"><span data-stu-id="de882-139">Licenses are assigned on a first come, first serve basis until all hello licenses are taken.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  <span data-ttu-id="de882-141">Klicka på **skapa token**.</span><span class="sxs-lookup"><span data-stu-id="de882-141">Click **Generate token**.</span></span> <span data-ttu-id="de882-142">Du bör se bildskärmen åtkomst-token under hello **åtkomsttoken** fältet.</span><span class="sxs-lookup"><span data-stu-id="de882-142">You should see your access token display under hello **Access token** field.</span></span>

6)  <span data-ttu-id="de882-143">Spara din dator eller en åtkomst-token tooyour Urklipp innan de lämnar hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="de882-143">Save your access token tooyour clipboard or computer before leaving hello page.</span></span>

7) <span data-ttu-id="de882-144">Logga sedan in toohello [Azure-portalen](https://portal.azure.com), och bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="de882-144">Next, sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="de882-145">Om du redan har konfigurerat LinkedIn försäljning Navigator för enkel inloggning, söka efter din instans av LinkedIn försäljning Navigator hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="de882-145">If you have already configured LinkedIn Sales Navigator for single sign-on, search for your instance of LinkedIn Sales Navigator using hello search field.</span></span> <span data-ttu-id="de882-146">Annars väljer **Lägg till** och Sök efter **LinkedIn försäljning Navigator** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="de882-146">Otherwise, select **Add** and search for **LinkedIn Sales Navigator** in hello application gallery.</span></span> <span data-ttu-id="de882-147">Välj LinkedIn försäljning Navigator hello sökresultat och Lägg den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="de882-147">Select LinkedIn Sales Navigator from hello search results, and add it tooyour list of applications.</span></span>

9)  <span data-ttu-id="de882-148">Välj din instans av LinkedIn försäljning Navigator och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="de882-148">Select your instance of LinkedIn Sales Navigator, then select hello **Provisioning** tab.</span></span>

10) <span data-ttu-id="de882-149">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="de882-149">Set hello **Provisioning Mode** too**Automatic**.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  <span data-ttu-id="de882-151">Fyll i följande fält under hello **administratörsautentiseringsuppgifter** :</span><span class="sxs-lookup"><span data-stu-id="de882-151">Fill in hello following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="de882-152">I hello **klient URL** anger https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="de882-152">In hello **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="de882-153">I hello **hemlighet Token** skriver hello åtkomst-token som du genererade i steg 1 och klicka på **Testanslutningen** .</span><span class="sxs-lookup"><span data-stu-id="de882-153">In hello **Secret Token** field, enter hello access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="de882-154">Du bör se ett meddelande om lyckade på hello upperright sida av portalen.</span><span class="sxs-lookup"><span data-stu-id="de882-154">You should see a success notification on hello upper­right side of   your portal.</span></span>

12) <span data-ttu-id="de882-155">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.</span><span class="sxs-lookup"><span data-stu-id="de882-155">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

13) <span data-ttu-id="de882-156">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="de882-156">Click **Save**.</span></span> 

14) <span data-ttu-id="de882-157">I hello **attributmappning** avsnittet kan du granska hello användar- och Gruppattribut som ska synkroniseras från Azure AD tooLinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="de882-157">In hello **Attribute Mappings** section, review hello user and group attributes that will be synchronized from Azure AD tooLinkedIn Sales Navigator.</span></span> <span data-ttu-id="de882-158">Observera att hello attribut som valts som **matchande** egenskaper kommer att använda toomatch hello användarkonton och grupper i LinkedIn försäljning Navigator för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="de882-158">Note that hello attributes selected as **Matching** properties will be used toomatch hello user accounts and groups in LinkedIn Sales Navigator for update operations.</span></span> <span data-ttu-id="de882-159">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="de882-159">Select hello Save button toocommit any changes.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) <span data-ttu-id="de882-161">tooenable hello Azure AD-etablering tjänsten för LinkedIn försäljning Navigator ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="de882-161">tooenable hello Azure AD provisioning service for LinkedIn Sales Navigator, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

16) <span data-ttu-id="de882-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="de882-162">Click **Save**.</span></span> 

<span data-ttu-id="de882-163">Detta startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooLinkedIn försäljning Navigator under hello användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="de882-163">This will start hello initial synchronization of any users and/or groups assigned tooLinkedIn Sales Navigator in hello Users and Groups section.</span></span> <span data-ttu-id="de882-164">Observera att hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="de882-164">Note that hello initial sync will take longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="de882-165">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras i appen LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="de882-165">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your LinkedIn Sales Navigator app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="de882-166">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="de882-166">Additional Resources</span></span>

* [<span data-ttu-id="de882-167">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="de882-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="de882-168">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="de882-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
