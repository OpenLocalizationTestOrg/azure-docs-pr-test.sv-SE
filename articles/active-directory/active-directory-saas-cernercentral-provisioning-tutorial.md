---
title: "Självstudier: Konfigurera Cerner Central för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory tooautomatically etablera användare tooa listan i Cerner Central."
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
ms.openlocfilehash: e96da98e783d24e7f34ae924824f909eead75f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a><span data-ttu-id="8249f-103">Självstudier: Konfigurera Cerner Central för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="8249f-103">Tutorial: Configuring Cerner Central for Automatic User Provisioning</span></span>

<span data-ttu-id="8249f-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Cerner Central och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooa användaren listan i Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="8249f-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Cerner Central and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooa user roster in Cerner Central.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="8249f-105">Krav</span><span class="sxs-lookup"><span data-stu-id="8249f-105">Prerequisites</span></span>

<span data-ttu-id="8249f-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8249f-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="8249f-107">En Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="8249f-107">An Azure Active Directory tenant</span></span>
*   <span data-ttu-id="8249f-108">En Cerner Central-klient</span><span class="sxs-lookup"><span data-stu-id="8249f-108">A Cerner Central tenant</span></span> 

> [!NOTE]
> <span data-ttu-id="8249f-109">Azure Active Directory kan integreras med Cerner Central med hello [SCIM](http://www.simplecloud.info/) protokoll.</span><span class="sxs-lookup"><span data-stu-id="8249f-109">Azure Active Directory integrates with Cerner Central using hello [SCIM](http://www.simplecloud.info/) protocol.</span></span>

## <a name="assigning-users-toocerner-central"></a><span data-ttu-id="8249f-110">Tilldela användare tooCerner Central</span><span class="sxs-lookup"><span data-stu-id="8249f-110">Assigning users tooCerner Central</span></span>

<span data-ttu-id="8249f-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="8249f-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="8249f-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserade.</span><span class="sxs-lookup"><span data-stu-id="8249f-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="8249f-113">Innan du konfigurerar och aktiverar hello etableras, bör du bestämma vilka användare och/eller grupper i Azure AD representerar hello-användare som behöver åtkomst till tooCerner Central.</span><span class="sxs-lookup"><span data-stu-id="8249f-113">Before configuring and enabling hello provisioning service, you should decide what users and/or groups in Azure AD represent hello users who need access tooCerner Central.</span></span> <span data-ttu-id="8249f-114">När du valt, kan du tilldela dessa användare tooCerner Central genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="8249f-114">Once decided, you can assign these users tooCerner Central by following hello instructions here:</span></span>

[<span data-ttu-id="8249f-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="8249f-115">Assign a user or group tooan enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toocerner-central"></a><span data-ttu-id="8249f-116">Viktiga tips för att tilldela användare tooCerner Central</span><span class="sxs-lookup"><span data-stu-id="8249f-116">Important tips for assigning users tooCerner Central</span></span>

*   <span data-ttu-id="8249f-117">Vi rekommenderar att en enda Azure AD-användare tilldelas tooCerner centrala tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8249f-117">It is recommended that a single Azure AD user be assigned tooCerner Central tootest hello provisioning configuration.</span></span> <span data-ttu-id="8249f-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="8249f-118">Additional users and/or groups may be assigned later.</span></span>

* <span data-ttu-id="8249f-119">När det första testet är klart för en enskild användare, rekommenderar Cerner Central tilldela hello hela listan över användare som är avsedda tooaccess alla Cerner lösningen (inte bara Cerner Central) toobe etableras Toocerner's användaren listan.</span><span class="sxs-lookup"><span data-stu-id="8249f-119">Once initial testing is complete for a single user, Cerner Central recommends assigning hello entire list of users intended tooaccess any Cerner solution (not just Cerner Central) toobe provisioned tooCerner’s user roster.</span></span>  <span data-ttu-id="8249f-120">Andra Cerner lösningar utnyttja den här listan över användare i listan för hello-användare.</span><span class="sxs-lookup"><span data-stu-id="8249f-120">Other Cerner solutions leverage this list of users in hello user roster.</span></span>

*   <span data-ttu-id="8249f-121">När du tilldelar en användare tooCerner Central, måste du välja hello **användaren** roll i dialogrutan för hello tilldelning.</span><span class="sxs-lookup"><span data-stu-id="8249f-121">When assigning a user tooCerner Central, you must select hello **User** role in hello assignment dialog.</span></span> <span data-ttu-id="8249f-122">Användare med hello ”standard” rollen som är undantagna från etablering.</span><span class="sxs-lookup"><span data-stu-id="8249f-122">Users with hello "Default Access" role are excluded from provisioning.</span></span>


## <a name="configuring-user-provisioning-toocerner-central"></a><span data-ttu-id="8249f-123">Konfigurera användaretablering tooCerner Central</span><span class="sxs-lookup"><span data-stu-id="8249f-123">Configuring user provisioning tooCerner Central</span></span>

<span data-ttu-id="8249f-124">Det här avsnittet hjälper dig att ansluta din Azure AD tooCerner Central användaren listan med Cerner's SCIM användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarbaserad konton i Cerner Central på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8249f-124">This section guides you through connecting your Azure AD tooCerner Central’s User Roster using Cerner's SCIM user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Cerner Central based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="8249f-125">Du kan också välja tooenabled SAML-baserade enkel inloggning för Cerner centrala, hello instruktionerna som anges i [Azure-portalen (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8249f-125">You may also choose tooenabled SAML-based Single Sign-On for Cerner Central, following hello instructions provided in [Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="8249f-126">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner kompletterar varandra.</span><span class="sxs-lookup"><span data-stu-id="8249f-126">Single sign-on can be configured independently of automatic provisioning, though these two features complement each other.</span></span> <span data-ttu-id="8249f-127">Mer information finns i hello [Cerner Central enkel inloggning kursen](active-directory-saas-cernercentral-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="8249f-127">For more information, see hello [Cerner Central single sign-on tutorial](active-directory-saas-cernercentral-tutorial.md).</span></span>


### <a name="tooconfigure-automatic-user-account-provisioning-toocerner-central-in-azure-ad"></a><span data-ttu-id="8249f-128">tooconfigure automatisk användarkonto etablering tooCerner Central i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="8249f-128">tooconfigure automatic user account provisioning tooCerner Central in Azure AD:</span></span>


<span data-ttu-id="8249f-129">I ordning tooprovision användaren konton tooCerner Central du behöver toorequest Cerner Central system-kontot från Cerner, och skapa en OAuth ägar-token som Azure AD kan använda tooconnect tooCerner SCIM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8249f-129">In order tooprovision user accounts tooCerner Central, you’ll need toorequest a Cerner Central system account from Cerner, and generate an OAuth bearer token that Azure AD can use tooconnect tooCerner's SCIM endpoint.</span></span> <span data-ttu-id="8249f-130">Vi rekommenderar också att hello integration utförs innan du distribuerar tooproduction i Cerner begränsat läge.</span><span class="sxs-lookup"><span data-stu-id="8249f-130">It is also recommended that hello integration be performed in a Cerner sandbox environment before deploying tooproduction.</span></span>

1.  <span data-ttu-id="8249f-131">hello första steget är tooensure hello personer hantera hello Cerner och Azure AD-integrering har ett CernerCare konto, som är nödvändiga tooaccess hello nödvändiga toocomplete hello instruktioner.</span><span class="sxs-lookup"><span data-stu-id="8249f-131">hello first step is tooensure hello people managing hello Cerner and Azure AD integration have a CernerCare account, which is required tooaccess hello documentation necessary toocomplete hello instructions.</span></span> <span data-ttu-id="8249f-132">Om du Använd hello URL: er nedan toocreate CernerCare konton i varje tillämplig miljö.</span><span class="sxs-lookup"><span data-stu-id="8249f-132">If necessary, use hello URLs below toocreate CernerCare accounts in each applicable environment.</span></span>

   * <span data-ttu-id="8249f-133">Sandbox: https://sandboxcernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="8249f-133">Sandbox:  https://sandboxcernercare.com/accounts/create</span></span>

   * <span data-ttu-id="8249f-134">Produktion: https://cernercare.com/accounts/create</span><span class="sxs-lookup"><span data-stu-id="8249f-134">Production:  https://cernercare.com/accounts/create</span></span>  

2.  <span data-ttu-id="8249f-135">Därefter måste en system-kontot skapas för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8249f-135">Next, a system account must be created for Azure AD.</span></span> <span data-ttu-id="8249f-136">Använd hello instruktionerna nedan toorequest ett systemkonto för din sandbox och produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="8249f-136">Use hello instructions below toorequest a System Account for your sandbox and production environments.</span></span>

   * <span data-ttu-id="8249f-137">Anvisningar: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span><span class="sxs-lookup"><span data-stu-id="8249f-137">Instructions:  https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account</span></span>

   * <span data-ttu-id="8249f-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="8249f-138">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="8249f-139">Produktion: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="8249f-139">Production:  https://cernercentral.com/system-accounts/</span></span>

3.  <span data-ttu-id="8249f-140">Därefter Generera en OAuth ägar-token för var och en av dina Systemkonton.</span><span class="sxs-lookup"><span data-stu-id="8249f-140">Next, generate an OAuth bearer token for each of your system accounts.</span></span> <span data-ttu-id="8249f-141">toodo detta, följ hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="8249f-141">toodo this, follow hello instructions below.</span></span>

   * <span data-ttu-id="8249f-142">Anvisningar: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span><span class="sxs-lookup"><span data-stu-id="8249f-142">Instructions:  https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token</span></span>

   * <span data-ttu-id="8249f-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="8249f-143">Sandbox: https://sandboxcernercentral.com/system-accounts/</span></span>

   * <span data-ttu-id="8249f-144">Produktion: https://cernercentral.com/system-accounts/</span><span class="sxs-lookup"><span data-stu-id="8249f-144">Production:  https://cernercentral.com/system-accounts/</span></span>

4. <span data-ttu-id="8249f-145">Slutligen måste tooacquire användaridentiteter listan sfär för båda hello sandbox och produktionsmiljöer i Cerner toocomplete hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8249f-145">Finally, you need tooacquire User Roster Realm IDs for both hello sandbox and production environments in Cerner toocomplete hello configuration.</span></span> <span data-ttu-id="8249f-146">Mer information om hur tooacquire detta, se: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span><span class="sxs-lookup"><span data-stu-id="8249f-146">For information on how tooacquire this, see: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM.</span></span> 

5. <span data-ttu-id="8249f-147">Du kan nu konfigurera Azure AD tooprovision användaren konton tooCerner.</span><span class="sxs-lookup"><span data-stu-id="8249f-147">Now you can configure Azure AD tooprovision user accounts tooCerner.</span></span> <span data-ttu-id="8249f-148">Logga in toohello [Azure-portalen](https://portal.azure.com), och bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8249f-148">Sign in toohello [Azure portal](https://portal.azure.com), and browse toohello **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

6. <span data-ttu-id="8249f-149">Om du redan har konfigurerat Cerner Central för enkel inloggning, söka efter din instans av Cerner Central hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="8249f-149">If you have already configured Cerner Central for single sign-on, search for your instance of Cerner Central using hello search field.</span></span> <span data-ttu-id="8249f-150">Annars väljer **Lägg till** och Sök efter **Cerner Central** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="8249f-150">Otherwise, select **Add** and search for **Cerner Central** in hello application gallery.</span></span> <span data-ttu-id="8249f-151">Välj Cerner Central från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="8249f-151">Select Cerner Central from hello search results, and add it tooyour list of applications.</span></span>

7.  <span data-ttu-id="8249f-152">Välj din instans av Cerner Central och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="8249f-152">Select your instance of Cerner Central, then select hello **Provisioning** tab.</span></span>

8.  <span data-ttu-id="8249f-153">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="8249f-153">Set hello **Provisioning Mode** too**Automatic**.</span></span>

   ![Cerner Central etablering](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  <span data-ttu-id="8249f-155">Fyll i följande fält under hello **administratörsautentiseringsuppgifter**:</span><span class="sxs-lookup"><span data-stu-id="8249f-155">Fill in hello following fields under **Admin Credentials**:</span></span>

   * <span data-ttu-id="8249f-156">I hello **klient URL** , ange en URL i formatet hello nedan, ersätter ”användare-listan-sfär-ID” med hello sfär ID som du har införskaffade i steg #4.</span><span class="sxs-lookup"><span data-stu-id="8249f-156">In hello **Tenant URL** field, enter a URL in hello format below, replacing "User-Roster-Realm-ID" with hello realm ID you acquired in step #4.</span></span>

> <span data-ttu-id="8249f-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="8249f-157">Sandbox: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

> <span data-ttu-id="8249f-158">Produktion: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span><span class="sxs-lookup"><span data-stu-id="8249f-158">Production: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/</span></span> 

   * <span data-ttu-id="8249f-159">I hello **hemlighet Token** skriver hello OAuth ägar-token som du genererade i steg #3 och klicka på **Testanslutningen**.</span><span class="sxs-lookup"><span data-stu-id="8249f-159">In hello **Secret Token** field, enter hello OAuth bearer token you generated in step #3 and click **Test Connection**.</span></span>

   * <span data-ttu-id="8249f-160">Du bör se ett meddelande om lyckade på hello upperright sida av portalen.</span><span class="sxs-lookup"><span data-stu-id="8249f-160">You should see a success notification on hello upper­right side of your portal.</span></span>

10. <span data-ttu-id="8249f-161">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fält och markera kryssrutan hello nedan.</span><span class="sxs-lookup"><span data-stu-id="8249f-161">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox below.</span></span>

11. <span data-ttu-id="8249f-162">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8249f-162">Click **Save**.</span></span> 

12. <span data-ttu-id="8249f-163">I hello **attributmappning** avsnittet, granska hello användare och grupp attribut toobe synkroniseras från Azure AD tooCerner Central.</span><span class="sxs-lookup"><span data-stu-id="8249f-163">In hello **Attribute Mappings** section, review hello user and group attributes toobe synchronized from Azure AD tooCerner Central.</span></span> <span data-ttu-id="8249f-164">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton och grupper i Cerner Central för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="8249f-164">hello attributes selected as **Matching** properties are used toomatch hello user accounts and groups in Cerner Central for update operations.</span></span> <span data-ttu-id="8249f-165">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="8249f-165">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="8249f-166">tooenable hello Azure AD-etablering tjänsten för Cerner centrala, ändra hello **Status för etablering** för**på** i hello **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="8249f-166">tooenable hello Azure AD provisioning service for Cerner Central, change hello **Provisioning Status** too**On** in hello **Settings** section</span></span>

14. <span data-ttu-id="8249f-167">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8249f-167">Click **Save**.</span></span> 

<span data-ttu-id="8249f-168">Detta startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooCerner Central under hello användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="8249f-168">This starts hello initial synchronization of any users and/or groups assigned tooCerner Central in hello Users and Groups section.</span></span> <span data-ttu-id="8249f-169">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello etablering Azure AD-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="8249f-169">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello Azure AD provisioning service is running.</span></span> <span data-ttu-id="8249f-170">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras Cerner Central appen.</span><span class="sxs-lookup"><span data-stu-id="8249f-170">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Cerner Central app.</span></span>

<span data-ttu-id="8249f-171">Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="8249f-171">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8249f-172">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8249f-172">Additional resources</span></span>

* [<span data-ttu-id="8249f-173">Cerner Central: Publicering av identitetsdata med Azure AD</span><span class="sxs-lookup"><span data-stu-id="8249f-173">Cerner Central: Publishing identity data using Azure AD</span></span>](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [<span data-ttu-id="8249f-174">Självstudier: Konfigurera Cerner Central för enkel inloggning med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8249f-174">Tutorial: Configuring Cerner Central for single sign-on with Azure Active Directory</span></span>](active-directory-saas-cernercentral-tutorial.md)
* [<span data-ttu-id="8249f-175">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="8249f-175">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="8249f-176">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8249f-176">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="8249f-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8249f-177">Next steps</span></span>
* <span data-ttu-id="8249f-178">[Lär dig hur tooreview loggar och få rapporter om etablering aktiviteten](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="8249f-178">[Learn how tooreview logs and get reports on provisioning activity](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>
