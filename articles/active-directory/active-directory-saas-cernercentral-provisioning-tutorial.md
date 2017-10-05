---
title: "Självstudier: Konfigurera Cerner Central för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du konfigurerar Azure Active Directory automatiskt etablera användare till en listan i Cerner Central."
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
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: 84613b7f8d7bd031d492a62da0bc53be96ac45a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a><span data-ttu-id="b10ab-103">Självstudier: Konfigurera Cerner Central för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="b10ab-103">Tutorial: Configuring Cerner Central for Automatic User Provisioning</span></span>

<span data-ttu-id="b10ab-104">Syftet med den här kursen är att visa de steg som du behöver göra i Cerner Central och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till en användare listan i Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="b10ab-104">The objective of this tutorial is to show you the steps you need to perform in Cerner Central and Azure AD to automatically provision and de-provision user accounts from Azure AD to a user roster in Cerner Central.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="b10ab-105">Krav</span><span class="sxs-lookup"><span data-stu-id="b10ab-105">Prerequisites</span></span>

<span data-ttu-id="b10ab-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b10ab-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="b10ab-107">En Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="b10ab-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="b10ab-108">En Cerner Central-klient</span><span class="sxs-lookup"><span data-stu-id="b10ab-108">A Cerner Central tenant</span></span> 

> [!NOTE]
> <span data-ttu-id="b10ab-109">Azure Active Directory kan integreras med Cerner Central med hjälp av den [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="b10ab-109">Azure Active Directory integrates with Cerner Central using the [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-to-cerner-central"></a><span data-ttu-id="b10ab-110">Tilldela användare till Cerner Central</span><span class="sxs-lookup"><span data-stu-id="b10ab-110">Assigning users to Cerner Central</span></span>

<span data-ttu-id="b10ab-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="b10ab-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="b10ab-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b10ab-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="b10ab-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="b10ab-113">Before configuring and enabling the provisioning service, you should decide what users and/or groups in Azure AD represent the users who need access to Cerner Central.</span></span> <span data-ttu-id="b10ab-114">När du valt, kan du tilldela dessa användare till Cerner Central genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="b10ab-114">Once decided, you can assign these users to Cerner Central by following the instructions here:</span></span>

[<span data-ttu-id="b10ab-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="b10ab-115">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-cerner-central"></a><span data-ttu-id="b10ab-116">Viktiga tips för att tilldela användare till Cerner Central</span><span class="sxs-lookup"><span data-stu-id="b10ab-116">Important tips for assigning users to Cerner Central</span></span>

*   <span data-ttu-id="b10ab-117">Vi rekommenderar att en enda Azure AD-användare tilldelas Cerner Central att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b10ab-117">It is recommended that a single Azure AD user be assigned to Cerner Central to test the provisioning configuration.</span></span> <span data-ttu-id="b10ab-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="b10ab-118">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="b10ab-119">När det första testet är klart för en enskild användare, rekommenderar Cerner Central tilldelar hela listan över användare som ska få åtkomst till alla Cerner lösningen (inte bara Cerner Central) som ska etableras till Cerners användaren listan.</span><span class="sxs-lookup"><span data-stu-id="b10ab-119">Once initial testing is complete for a single user, Cerner Central recommends assigning the entire list of users intended to access any Cerner solution (not just Cerner Central) to be provisioned to Cerner’s user roster.</span></span>  <span data-ttu-id="b10ab-120">Andra Cerner lösningar utnyttja den här listan över användare i listan för användaren.</span><span class="sxs-lookup"><span data-stu-id="b10ab-120">Other Cerner solutions leverage this list of users in the user roster.</span></span>

*   <span data-ttu-id="b10ab-121">När du tilldelar en användare Cerner Central, måste du välja den **användaren** roll i dialogrutan tilldelning.</span><span class="sxs-lookup"><span data-stu-id="b10ab-121">When assigning a user to Cerner Central, you must select the **User** role in the assignment dialog.</span></span> <span data-ttu-id="b10ab-122">Användare med rollen ”standard åtkomst” undantas från etablering.</span><span class="sxs-lookup"><span data-stu-id="b10ab-122">Users with the "Default Access" role are excluded from provisioning.</span></span>


## <a name="configuring-user-provisioning-to-cerner-central"></a><span data-ttu-id="b10ab-123">Konfigurering av användarförsörjning till Cerner Central</span><span class="sxs-lookup"><span data-stu-id="b10ab-123">Configuring user provisioning to Cerner Central</span></span>

<span data-ttu-id="b10ab-124">Det här avsnittet hjälper dig att ansluta din Azure AD till Cerner Central användaren listan med Cerner's SCIM användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelad användare konton i Cerner Central baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b10ab-124">This section guides you through connecting your Azure AD to Cerner Central’s User Roster using Cerner's SCIM user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in Cerner Central based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="b10ab-125">Du kan också välja att aktivera SAML-baserade enkel inloggning för Cerner centrala, följa instruktionerna i [Azure-portalen (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b10ab-125">You may also choose to enabled SAML-based Single Sign-On for Cerner Central, following the instructions provided in [Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="b10ab-126">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.</span><span class="sxs-lookup"><span data-stu-id="b10ab-126">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span> <span data-ttu-id="b10ab-127">Mer information finns i [Cerner Central enkel inloggning kursen](active-directory-saas-cernercentral-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="b10ab-127">For more information, see the [Cerner Central single sign-on tutorial](active-directory-saas-cernercentral-tutorial.md).</span></span>


### <a name="to-configure-automatic-user-account-provisioning-to-cerner-central-in-azure-ad"></a><span data-ttu-id="b10ab-128">Konfigurera automatisk konto användaretablering till Cerner Central i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="b10ab-128">To configure automatic user account provisioning to Cerner Central in Azure AD:</span></span>


<span data-ttu-id="b10ab-129">För att kunna etablera användarkonton till Cerner Central måste du begära en Cerner Central systemkontot från Cerner och generera en OAuth ägar-token som Azure AD kan använda för att ansluta till Cerner's SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b10ab-129">In order to provision user accounts to Cerner Central, you’ll need to request a Cerner Central system account from Cerner, and generate an OAuth bearer token that Azure AD can use to connect to Cerner's SCIM endpoint.</span></span> <span data-ttu-id="b10ab-130">Vi rekommenderar också att integrationen ska utföras i begränsat läge Cerner innan du distribuerar till produktionen.</span><span class="sxs-lookup"><span data-stu-id="b10ab-130">It is also recommended that the integration be performed in a Cerner sandbox environment before deploying to production.</span></span>

1.  <span data-ttu-id="b10ab-131">Det första steget är att se till att de personer som hanterar Cerner och Azure AD-integrering har ett konto för CernerCare som krävs för att få åtkomst till dokumentationen som behövs för att slutföra anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="b10ab-131">The first step is to ensure the people managing the Cerner and Azure AD integration have a CernerCare account, which is required to access the documentation necessary to complete the instructions.</span></span> <span data-ttu-id="b10ab-132">Om det behövs kan du använda adresserna nedan för att skapa CernerCare konton i varje tillämplig miljö.</span><span class="sxs-lookup"><span data-stu-id="b10ab-132">If necessary, use the URLs below to create CernerCare accounts in each applicable environment.</span></span>

   * <span data-ttu-id="b10ab-133">Sandbox: https://sandboxcernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="b10ab-133">Sandbox:  https://sandboxcernercare.com/accounts/create</span></span>

   * <span data-ttu-id="b10ab-134">Produktion: https://cernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="b10ab-134">Production:  https://cernercare.com/accounts/create</span></span>  

2.  <span data-ttu-id="b10ab-135">Därefter måste en system-kontot skapas för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b10ab-135">Next, a system account must be created for Azure AD.</span></span> <span data-ttu-id="b10ab-136">Du kan följa anvisningarna nedan för att begära ett systemkonto för din sandbox och produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="b10ab-136">Use the instructions below to request a System Account for your sandbox and production environments.</span></span>

   * <span data-ttu-id="b10ab-137">Anvisningar: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span><span class="sxs-lookup"><span data-stu-id="b10ab-137">Instructions:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span></span>

   * <span data-ttu-id="b10ab-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="b10ab-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="b10ab-139">Produktion: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="b10ab-139">Production:  https://cernercentral.com/system-accounts/</span></span>

3.  <span data-ttu-id="b10ab-140">Därefter Generera en OAuth ägar-token för var och en av dina Systemkonton.</span><span class="sxs-lookup"><span data-stu-id="b10ab-140">Next, generate an OAuth bearer token for each of your system accounts.</span></span> <span data-ttu-id="b10ab-141">Följ anvisningarna nedan om du vill göra detta.</span><span class="sxs-lookup"><span data-stu-id="b10ab-141">To do this, follow the instructions below.</span></span>

   * <span data-ttu-id="b10ab-142">Anvisningar: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span><span class="sxs-lookup"><span data-stu-id="b10ab-142">Instructions:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span></span>

   * <span data-ttu-id="b10ab-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="b10ab-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="b10ab-144">Produktion: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="b10ab-144">Production:  https://cernercentral.com/system-accounts/</span></span>

4. <span data-ttu-id="b10ab-145">Slutligen måste du hämta användaridentiteter listan sfär för både den sandbox och produktionsmiljöer i Cerner för att slutföra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b10ab-145">Finally, you need to acquire User Roster Realm IDs for both the sandbox and production environments in Cerner to complete the configuration.</span></span> <span data-ttu-id="b10ab-146">Mer information om hur du skaffar detta finns: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span><span class="sxs-lookup"><span data-stu-id="b10ab-146">For information on how to acquire this, see: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span></span> 

5. <span data-ttu-id="b10ab-147">Du kan nu konfigurera Azure AD för att etablera användarkonton till Cerner.</span><span class="sxs-lookup"><span data-stu-id="b10ab-147">Now you can configure Azure AD to provision user accounts to Cerner.</span></span> <span data-ttu-id="b10ab-148">Logga in på den [Azure-portalen](https://portal.azure.com), och bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b10ab-148">Sign in to the [Azure portal](https://portal.azure.com), and browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

6. <span data-ttu-id="b10ab-149">Om du redan har konfigurerat Cerner Central för enkel inloggning söka efter din instans av Cerner Central med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="b10ab-149">If you have already configured Cerner Central for single sign-on, search for your instance of Cerner Central using the search field.</span></span> <span data-ttu-id="b10ab-150">Annars väljer **Lägg till** och Sök efter **Cerner Central** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="b10ab-150">Otherwise, select **Add** and search for **Cerner Central** in the application gallery.</span></span> <span data-ttu-id="b10ab-151">Välj Cerner Central i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="b10ab-151">Select Cerner Central from the search results, and add it to your list of applications.</span></span>

7.  <span data-ttu-id="b10ab-152">Välj din instans av Cerner Central och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="b10ab-152">Select your instance of Cerner Central, then select the **Provisioning** tab.</span></span>

8.  <span data-ttu-id="b10ab-153">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="b10ab-153">Set the **Provisioning Mode** to **Automatic**.</span></span>

   ![Cerner Central etablering](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  <span data-ttu-id="b10ab-155">Fyll i följande fält under **administratörsautentiseringsuppgifter**:</span><span class="sxs-lookup"><span data-stu-id="b10ab-155">Fill in the following fields under **Admin Credentials**:</span></span>

   * <span data-ttu-id="b10ab-156">I den **klient URL** , ange en URL i formatet nedan, ersätter du ”användare-listan-sfär-ID” med sfär-ID som du har införskaffade i steg #4.</span><span class="sxs-lookup"><span data-stu-id="b10ab-156">In the **Tenant URL** field, enter a URL in the format below, replacing "User-Roster-Realm-ID" with the realm ID you acquired in step #4.</span></span>

> <span data-ttu-id="b10ab-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="b10ab-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

> <span data-ttu-id="b10ab-158">Produktion: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="b10ab-158">Production: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

   * <span data-ttu-id="b10ab-159">I den **hemlighet Token** skriver OAuth-ägar-token som du genererade i steg #3 och klicka på **Testanslutningen**.</span><span class="sxs-lookup"><span data-stu-id="b10ab-159">In the **Secret Token** field, enter the OAuth bearer token you generated in step #3 and click **Test Connection**.</span></span>

   * <span data-ttu-id="b10ab-160">Du bör se ett meddelande om lyckade på upperright sida av portalen.</span><span class="sxs-lookup"><span data-stu-id="b10ab-160">You should see a success notification on the upper­right side of your portal.</span></span>

10. <span data-ttu-id="b10ab-161">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan nedan.</span><span class="sxs-lookup"><span data-stu-id="b10ab-161">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox below.</span></span>

11. <span data-ttu-id="b10ab-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b10ab-162">Click **Save**.</span></span> 

12. <span data-ttu-id="b10ab-163">I den **attributmappning** avsnittet kan du granska de användar- och attribut som ska synkroniseras från Azure AD till Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="b10ab-163">In the **Attribute Mappings** section, review the user and group attributes to be synchronized from Azure AD to Cerner Central.</span></span> <span data-ttu-id="b10ab-164">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton och grupper i Cerner Central för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="b10ab-164">The attributes selected as **Matching** properties are used to match the user accounts and groups in Cerner Central for update operations.</span></span> <span data-ttu-id="b10ab-165">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b10ab-165">Select the Save button to commit any changes.</span></span>

13. <span data-ttu-id="b10ab-166">Om du vill aktivera Azure AD etableras för Cerner centrala ändra den **Status för etablering** till **på** i den **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="b10ab-166">To enable the Azure AD provisioning service for Cerner Central, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

14. <span data-ttu-id="b10ab-167">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b10ab-167">Click **Save**.</span></span> 

<span data-ttu-id="b10ab-168">Detta startar den första synkroniseringen av användare och/eller grupper som tilldelas till Cerner Central i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="b10ab-168">This starts the initial synchronization of any users and/or groups assigned to Cerner Central in the Users and Groups section.</span></span> <span data-ttu-id="b10ab-169">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som Azure AD Etablerar-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="b10ab-169">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the Azure AD provisioning service is running.</span></span> <span data-ttu-id="b10ab-170">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering i appen Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="b10ab-170">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service on your Cerner Central app.</span></span>

<span data-ttu-id="b10ab-171">Mer information om hur du tolkar Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="b10ab-171">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b10ab-172">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b10ab-172">Additional resources</span></span>

* [<span data-ttu-id="b10ab-173">Cerner Central: Publicering av identitetsdata med Azure AD</span><span class="sxs-lookup"><span data-stu-id="b10ab-173">Cerner Central: Publishing identity data using Azure AD</span></span>](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [<span data-ttu-id="b10ab-174">Självstudier: Konfigurera Cerner Central för enkel inloggning med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b10ab-174">Tutorial: Configuring Cerner Central for single sign-on with Azure Active Directory</span></span>](active-directory-saas-cernercentral-tutorial.md)
* [<span data-ttu-id="b10ab-175">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="b10ab-175">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="b10ab-176">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b10ab-176">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="b10ab-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b10ab-177">Next steps</span></span>
* <span data-ttu-id="b10ab-178">[Lär dig hur du granska loggarna och få rapporter om etablering aktiviteten](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="b10ab-178">[Learn how to review logs and get reports on provisioning activity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>
