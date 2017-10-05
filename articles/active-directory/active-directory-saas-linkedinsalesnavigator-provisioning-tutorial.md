---
title: "Självstudier: Konfigurera LinkedIn försäljning Navigator för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du konfigurerar Azure Active Directory för att automatiskt etablera och avetablera användarkonton till LinkedIn försäljning Navigator."
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
ms.openlocfilehash: 86357949c8e6927f78ca5bb8b7e20a6b88c37ef3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-linkedin-sales-navigator-for-automatic-user-provisioning"></a><span data-ttu-id="79c6d-103">Självstudier: Konfigurera LinkedIn försäljning Navigator för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="79c6d-103">Tutorial: Configuring LinkedIn Sales Navigator for Automatic User Provisioning</span></span>


<span data-ttu-id="79c6d-104">Syftet med den här kursen är att visa de steg som du behöver göra i LinkedIn försäljning Navigator och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="79c6d-104">The objective of this tutorial is to show you the steps you need to perform in LinkedIn Sales Navigator and Azure AD to automatically provision and de-provision user accounts from Azure AD to LinkedIn Sales Navigator.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="79c6d-105">Krav</span><span class="sxs-lookup"><span data-stu-id="79c6d-105">Prerequisites</span></span>

<span data-ttu-id="79c6d-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="79c6d-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="79c6d-107">En Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="79c6d-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="79c6d-108">En LinkedIn försäljning Navigator-klient</span><span class="sxs-lookup"><span data-stu-id="79c6d-108">A LinkedIn Sales Navigator tenant</span></span> 
*   <span data-ttu-id="79c6d-109">Ett administratörskontot i LinkedIn försäljning Navigator med åtkomst till LinkedIn Account Center</span><span class="sxs-lookup"><span data-stu-id="79c6d-109">An administrator account in LinkedIn Sales Navigator with access to the LinkedIn Account Center</span></span>

> [!NOTE]
> <span data-ttu-id="79c6d-110">Azure Active Directory kan integreras med LinkedIn försäljning Navigator med hjälp av den [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="79c6d-110">Azure Active Directory integrates with LinkedIn Sales Navigator using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-linkedin-sales-navigator"></a><span data-ttu-id="79c6d-111">Tilldela användare till LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="79c6d-111">Assigning users to LinkedIn Sales Navigator</span></span>

<span data-ttu-id="79c6d-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="79c6d-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="79c6d-113">I samband med automatisk konto användaretablering, kommer endast användare och grupper som har ”tilldelats” till ett program i Azure AD att synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="79c6d-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD will be synchronized.</span></span> 

<span data-ttu-id="79c6d-114">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="79c6d-114">Before configuring and enabling the provisioning service, you will need to decide what users and/or groups in Azure AD represent the users who need access to LinkedIn Sales Navigator.</span></span> <span data-ttu-id="79c6d-115">När bestämt, kan du tilldela dessa användare till LinkedIn försäljning Navigator genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="79c6d-115">Once decided, you can assign these users to LinkedIn Sales Navigator by following the instructions here:</span></span>

[<span data-ttu-id="79c6d-116">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="79c6d-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-linkedin-sales-navigator"></a><span data-ttu-id="79c6d-117">Viktiga tips för att tilldela användare till LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="79c6d-117">Important tips for assigning users to LinkedIn Sales Navigator</span></span>

*   <span data-ttu-id="79c6d-118">Vi rekommenderar att en enda Azure AD-användare tilldelas LinkedIn försäljning Navigator testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="79c6d-118">It is recommended that a single Azure AD user be assigned to LinkedIn Sales Navigator to test the provisioning configuration.</span></span> <span data-ttu-id="79c6d-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="79c6d-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="79c6d-120">När du tilldelar en användare LinkedIn försäljning Navigator, måste du välja den **användaren** roll i dialogrutan tilldelning.</span><span class="sxs-lookup"><span data-stu-id="79c6d-120">When assigning a user to LinkedIn Sales Navigator, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="79c6d-121">Rollen ”standard åtkomst” fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="79c6d-121">The "Default Access" role does not work for provisioning.</span></span>


## <a name="configuring-user-provisioning-to-linkedin-sales-navigator"></a><span data-ttu-id="79c6d-122">Konfigurering av användarförsörjning LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="79c6d-122">Configuring user provisioning to LinkedIn Sales Navigator</span></span>

<span data-ttu-id="79c6d-123">Det här avsnittet visar hur du ansluter din Azure AD till LinkedIn försäljning Navigator SCIM användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i LinkedIn försäljning Navigator baserat på användare och grupptilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79c6d-123">This section guides you through connecting your Azure AD to LinkedIn Sales Navigator's SCIM user account provisioning API, and configuring the provisioning service to create, update and disable assigned user accounts in LinkedIn Sales Navigator based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="79c6d-124">Du kan också välja att aktivera SAML-baserade enkel inloggning för LinkedIn försäljning Navigator, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79c6d-124">You may also choose to enabled SAML-based Single Sign-On for LinkedIn Sales Navigator, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="79c6d-125">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.</span><span class="sxs-lookup"><span data-stu-id="79c6d-125">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-linkedin-sales-navigator-in-azure-ad"></a><span data-ttu-id="79c6d-126">Konfigurera automatisk konto användaretablering LinkedIn försäljning Navigator i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="79c6d-126">To configure automatic user account provisioning to LinkedIn Sales Navigator in Azure AD:</span></span>


<span data-ttu-id="79c6d-127">Det första steget är att hämta LinkedIn-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="79c6d-127">The first step is to retrieve your LinkedIn access token.</span></span> <span data-ttu-id="79c6d-128">Om du är företagsadministratör kan etablera du själv en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="79c6d-128">If you are an Enterprise administrator, you can self-provision an access token.</span></span> <span data-ttu-id="79c6d-129">Gå till i din kontocenter **inställningar &gt; globala inställningar** och öppna den **SCIM installationsprogrammet** panelen.</span><span class="sxs-lookup"><span data-stu-id="79c6d-129">In your account center, go to **Settings &gt; Global Settings** and open the **SCIM Setup** panel.</span></span>

> [!NOTE]
> <span data-ttu-id="79c6d-130">Om du ansluter till mitt konto direkt i stället för via en länk, kan du nå den med hjälp av följande steg.</span><span class="sxs-lookup"><span data-stu-id="79c6d-130">If you are accessing the account center directly rather than through a link, you can reach it using the following steps.</span></span>

1)  <span data-ttu-id="79c6d-131">Logga in på kontot Center.</span><span class="sxs-lookup"><span data-stu-id="79c6d-131">Sign in to Account Center.</span></span>

2)  <span data-ttu-id="79c6d-132">Välj **Admin &gt; administrationsinställningar** .</span><span class="sxs-lookup"><span data-stu-id="79c6d-132">Select **Admin &gt; Admin Settings** .</span></span>

3)  <span data-ttu-id="79c6d-133">Klicka på **avancerade integreringar** på vänster sidopanelen.</span><span class="sxs-lookup"><span data-stu-id="79c6d-133">Click **Advanced Integrations** on the left sidebar.</span></span> <span data-ttu-id="79c6d-134">Du dirigeras till mitt konto.</span><span class="sxs-lookup"><span data-stu-id="79c6d-134">You are directed to the account center.</span></span>

4)  <span data-ttu-id="79c6d-135">Klicka på **+ Lägg till ny SCIM konfiguration** och följer du proceduren genom att fylla i varje fält.</span><span class="sxs-lookup"><span data-stu-id="79c6d-135">Click **+ Add new SCIM configuration** and follow the procedure by filling in each field.</span></span>

> <span data-ttu-id="79c6d-136">När autoassign licenser inte är aktiverad, innebär det att endast användardata är synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="79c6d-136">When auto­assign licenses is not enabled, it means that only user data is synced.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_1.PNG)

> <span data-ttu-id="79c6d-138">När autolicense tilldelning har aktiverats måste du Observera programinstansen och licenstypen.</span><span class="sxs-lookup"><span data-stu-id="79c6d-138">When auto­license assignment is enabled, you need to note the application instance and license type.</span></span> <span data-ttu-id="79c6d-139">Licenser är kopplade till en första komma, först hantera bas tills alla licenser tas.</span><span class="sxs-lookup"><span data-stu-id="79c6d-139">Licenses are assigned on a first come, first serve basis until all the licenses are taken.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_2.PNG)

5)  <span data-ttu-id="79c6d-141">Klicka på **skapa token**.</span><span class="sxs-lookup"><span data-stu-id="79c6d-141">Click **Generate token**.</span></span> <span data-ttu-id="79c6d-142">Du bör se din token visas under den **åtkomsttoken** fältet.</span><span class="sxs-lookup"><span data-stu-id="79c6d-142">You should see your access token display under the **Access token** field.</span></span>

6)  <span data-ttu-id="79c6d-143">Spara åtkomst-token till Urklipp eller en dator innan du lämnar sidan.</span><span class="sxs-lookup"><span data-stu-id="79c6d-143">Save your access token to your clipboard or computer before leaving the page.</span></span>

7) <span data-ttu-id="79c6d-144">Logga sedan in till den [Azure-portalen](https://portal.azure.com), och bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="79c6d-144">Next, sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

8) <span data-ttu-id="79c6d-145">Om du redan har konfigurerat LinkedIn försäljning Navigator för enkel inloggning söka efter din instans av LinkedIn försäljning Navigator med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="79c6d-145">If you have already configured LinkedIn Sales Navigator for single sign-on, search for your instance of LinkedIn Sales Navigator using the search field.</span></span> <span data-ttu-id="79c6d-146">Annars väljer **Lägg till** och Sök efter **LinkedIn försäljning Navigator** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="79c6d-146">Otherwise, select **Add** and search for **LinkedIn Sales Navigator** in the application gallery.</span></span> <span data-ttu-id="79c6d-147">Välj LinkedIn försäljning Navigator i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="79c6d-147">Select LinkedIn Sales Navigator from the search results, and add it to your list of applications.</span></span>

9)  <span data-ttu-id="79c6d-148">Välj din instans av LinkedIn försäljning Navigator och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="79c6d-148">Select your instance of LinkedIn Sales Navigator, then select the **Provisioning** tab.</span></span>

10) <span data-ttu-id="79c6d-149">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="79c6d-149">Set the **Provisioning Mode** to **Automatic**.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_3.PNG)

11)  <span data-ttu-id="79c6d-151">Fyll i följande fält under **administratörsautentiseringsuppgifter** :</span><span class="sxs-lookup"><span data-stu-id="79c6d-151">Fill in the following fields under **Admin Credentials** :</span></span>

* <span data-ttu-id="79c6d-152">I den **klient URL** anger https://api.linkedin.com.</span><span class="sxs-lookup"><span data-stu-id="79c6d-152">In the **Tenant URL** field, enter https://api.linkedin.com.</span></span>

* <span data-ttu-id="79c6d-153">I den **hemlighet Token** skriver den åtkomst-token som du genererade i steg 1 och klicka på **Testanslutningen** .</span><span class="sxs-lookup"><span data-stu-id="79c6d-153">In the **Secret Token** field, enter the access token you generated in step 1 and click **Test Connection** .</span></span>

* <span data-ttu-id="79c6d-154">Du bör se ett meddelande om lyckade på upperright sida av portalen.</span><span class="sxs-lookup"><span data-stu-id="79c6d-154">You should see a success notification on the upper­right side of   your portal.</span></span>

12) <span data-ttu-id="79c6d-155">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan nedan.</span><span class="sxs-lookup"><span data-stu-id="79c6d-155">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

13) <span data-ttu-id="79c6d-156">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="79c6d-156">Click **Save**.</span></span> 

14) <span data-ttu-id="79c6d-157">I den **attributmappning** avsnittet kan du granska de användar- och attribut som ska synkroniseras från Azure AD LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="79c6d-157">In the **Attribute Mappings** section, review the user and group attributes that will be synchronized from Azure AD to LinkedIn Sales Navigator.</span></span> <span data-ttu-id="79c6d-158">Observera att attribut som är markerade som **matchande** egenskaper som används för att matcha användarkonton och grupper i LinkedIn försäljning Navigator för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="79c6d-158">Note that the attributes selected as **Matching** properties will be used to match the user accounts and groups in LinkedIn Sales Navigator for update operations.</span></span> <span data-ttu-id="79c6d-159">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="79c6d-159">Select the Save button to commit any changes.</span></span>

![LinkedIn försäljning Navigator etablering](./media/active-directory-saas-linkedinsalesnavigator-provisioning-tutorial/linkedin_4.PNG)

15) <span data-ttu-id="79c6d-161">Om du vill aktivera Azure AD etableras för LinkedIn försäljning Navigator ändra den **Status för etablering** till **på** i den **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="79c6d-161">To enable the Azure AD provisioning service for LinkedIn Sales Navigator, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

16) <span data-ttu-id="79c6d-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="79c6d-162">Click **Save**.</span></span> 

<span data-ttu-id="79c6d-163">Detta startar den första synkroniseringen av användare och/eller grupper som tilldelas till LinkedIn försäljning Navigator i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="79c6d-163">This will start the initial synchronization of any users and/or groups assigned to LinkedIn Sales Navigator in the Users and Groups section.</span></span> <span data-ttu-id="79c6d-164">Observera att den första synkroniseringen ta längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="79c6d-164">Note that the initial sync will take longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="79c6d-165">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering i appen LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="79c6d-165">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your LinkedIn Sales Navigator app.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="79c6d-166">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="79c6d-166">Additional Resources</span></span>

* [<span data-ttu-id="79c6d-167">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="79c6d-167">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="79c6d-168">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79c6d-168">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
