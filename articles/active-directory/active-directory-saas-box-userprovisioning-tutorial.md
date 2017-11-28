---
title: "Självstudier: Azure Active Directory-integrering med | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och rutan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e92baabb174642c22c99e2a30bc9c71845b3b75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a><span data-ttu-id="9bac5-103">Självstudier: Konfigurera rutan för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="9bac5-103">Tutorial: Configuring Box for Automatic User Provisioning</span></span>

<span data-ttu-id="9bac5-104">hello syftet med den här kursen är tooshow hello stegen tooperform i rutan och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-104">hello objective of this tutorial is tooshow hello steps you need tooperform in Box and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooBox.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9bac5-105">Krav</span><span class="sxs-lookup"><span data-stu-id="9bac5-105">Prerequisites</span></span>

<span data-ttu-id="9bac5-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9bac5-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="9bac5-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="9bac5-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="9bac5-108">En ruta enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="9bac5-108">A Box single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="9bac5-109">Ett användarkonto i rutan Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="9bac5-109">A user account in Box with Team Admin permissions.</span></span>

## <a name="assigning-users-toobox"></a><span data-ttu-id="9bac5-110">Tilldela användare tooBox</span><span class="sxs-lookup"><span data-stu-id="9bac5-110">Assigning users tooBox</span></span> 

<span data-ttu-id="9bac5-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="9bac5-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="9bac5-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="9bac5-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="9bac5-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD representerar hello användare som behöver åtkomst till tooyour Box-app.</span><span class="sxs-lookup"><span data-stu-id="9bac5-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Box app.</span></span> <span data-ttu-id="9bac5-114">När du valt, kan du tilldela dessa användare tooyour Box-app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="9bac5-114">Once decided, you can assign these users tooyour Box app by following hello instructions here:</span></span>

[<span data-ttu-id="9bac5-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="9bac5-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a><span data-ttu-id="9bac5-116">Tilldela användare och grupper</span><span class="sxs-lookup"><span data-stu-id="9bac5-116">Assign users and groups</span></span>
<span data-ttu-id="9bac5-117">Hej **rutan > användare och grupper** fliken i hello Azure-portalen kan du toospecify vilka användare och grupper som ska beviljas åtkomst tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-117">hello **Box > Users and Groups** tab in hello Azure portal allows you toospecify which users and groups should be granted access tooBox.</span></span> <span data-ttu-id="9bac5-118">Tilldelning av en användare eller grupp gör följande saker toooccur hello:</span><span class="sxs-lookup"><span data-stu-id="9bac5-118">Assignment of a user or group causes hello following things toooccur:</span></span>

* <span data-ttu-id="9bac5-119">Azure AD tillåter hello som tilldelats användaren (antingen direkt av tilldelning av eller gruppmedlemskap) tooauthenticate tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-119">Azure AD permits hello assigned user (either by direct assignment or group membership) tooauthenticate tooBox.</span></span> <span data-ttu-id="9bac5-120">Om en användare inte har tilldelats, Azure AD tillåter inte dem toosign i tooBox och returnerar ett fel på hello Azure AD-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="9bac5-120">If a user is not assigned, then Azure AD does not permit them toosign in tooBox and returns an error on hello Azure AD sign-in page.</span></span>
* <span data-ttu-id="9bac5-121">En app-panelen för läggs toohello användarens [startprogrammet](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="9bac5-121">An app tile for Box is added toohello user's [application launcher](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>
* <span data-ttu-id="9bac5-122">Om automatisk etablering aktiveras sedan läggs hello tilldelade användare och/eller grupper toohello etablering kön toobe etableras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9bac5-122">If automatic provisioning is enabled, then hello assigned users and/or groups are added toohello provisioning queue toobe automatically provisioned.</span></span>
  
  * <span data-ttu-id="9bac5-123">Om bara användarobjekt konfigurerade toobe etableras, sedan alla direkt tilldelade användare placeras i kö för hello etablering och alla användare som är medlemmar i de tilldelade grupperna placeras i hello etablering kön.</span><span class="sxs-lookup"><span data-stu-id="9bac5-123">If only user objects were configured toobe provisioned, then all directly assigned users are placed in hello provisioning queue, and all users that are members of any assigned groups are placed in hello provisioning queue.</span></span> 
  * <span data-ttu-id="9bac5-124">Om gruppobjekt konfigurerade toobe etableras, är alla tilldelade gruppobjekt etablerade tooBox och alla användare som är medlemmar i dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="9bac5-124">If group objects were configured toobe provisioned, then all assigned group objects are provisioned tooBox, and all users that are members of those groups.</span></span> <span data-ttu-id="9bac5-125">hello grupp och användare medlemskap bevaras vid skrivs tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-125">hello group and user memberships are preserved upon being written tooBox.</span></span>

<span data-ttu-id="9bac5-126">Du kan använda hello **attribut > enkel inloggning** fliken tooconfigure vilka användarattribut (eller anspråk) visas tooBox under SAML-baserad autentisering och hello **attribut > etablering** fliken tooconfigure hur användar- och Gruppattribut flödar från Azure AD tooBox under etableringen åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9bac5-126">You can use hello **Attributes > Single Sign-On** tab tooconfigure which user attributes (or claims) are presented tooBox during SAML-based authentication, and hello **Attributes > Provisioning** tab tooconfigure how user and group attributes flow from Azure AD tooBox during provisioning operations.</span></span>

### <a name="important-tips-for-assigning-users-toobox"></a><span data-ttu-id="9bac5-127">Viktiga tips för att tilldela användare tooBox</span><span class="sxs-lookup"><span data-stu-id="9bac5-127">Important tips for assigning users tooBox</span></span> 

*   <span data-ttu-id="9bac5-128">Vi rekommenderar att en enda Azure AD användartilldelade tooBox tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9bac5-128">It is recommended that a single Azure AD user assigned tooBox tootest hello provisioning configuration.</span></span> <span data-ttu-id="9bac5-129">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="9bac5-129">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="9bac5-130">Du måste välja en giltig användarroll när du tilldelar en toobox för användaren.</span><span class="sxs-lookup"><span data-stu-id="9bac5-130">When assigning a user toobox, you must select a valid user role.</span></span> <span data-ttu-id="9bac5-131">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="9bac5-131">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="9bac5-132">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="9bac5-132">Enable Automated User Provisioning</span></span>

<span data-ttu-id="9bac5-133">Det här avsnittet hjälper att ansluta din Azure AD-tooBox användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i rutan utifrån tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9bac5-133">This section guides through connecting your Azure AD tooBox's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Box based on user and group assignment in Azure AD.</span></span>

<span data-ttu-id="9bac5-134">Om automatisk etablering aktiveras sedan läggs hello tilldelade användare och/eller grupper toohello etablering kön toobe etableras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9bac5-134">If automatic provisioning is enabled, then hello assigned users and/or groups are added toohello provisioning queue toobe automatically provisioned.</span></span>
    
 * <span data-ttu-id="9bac5-135">Om endast användarobjekt är konfigurerade toobe etableras direkt tilldelade användare är placerade i kö för hello etablering och alla användare som är medlemmar i de tilldelade grupperna placeras i hello etablering kön.</span><span class="sxs-lookup"><span data-stu-id="9bac5-135">If only user objects are configured toobe provisioned, then directly assigned users are placed in hello provisioning queue, and all users that are members of any assigned groups are placed in hello provisioning queue.</span></span> 
    
 * <span data-ttu-id="9bac5-136">Om gruppobjekt konfigurerade toobe etableras, är alla tilldelade gruppobjekt etablerade tooBox och alla användare som är medlemmar i dessa grupper.</span><span class="sxs-lookup"><span data-stu-id="9bac5-136">If group objects were configured toobe provisioned, then all assigned group objects are provisioned tooBox, and all users that are members of those groups.</span></span> <span data-ttu-id="9bac5-137">hello grupp och användare medlemskap bevaras vid skrivs tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-137">hello group and user memberships are preserved upon being written tooBox.</span></span>

> [!TIP] 
> <span data-ttu-id="9bac5-138">Du kan också välja tooenabled SAML-baserade enkel inloggning för Box, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9bac5-138">You may also choose tooenabled SAML-based Single Sign-On for Box, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9bac5-139">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="9bac5-139">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="9bac5-140">tooconfigure automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="9bac5-140">tooconfigure automatic user account provisioning:</span></span>

<span data-ttu-id="9bac5-141">hello syftet med det här avsnittet är toooutline hur tooenable etablering av Active Directory-användare konton tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-141">hello objective of this section is toooutline how tooenable provisioning of Active Directory user accounts tooBox.</span></span>

1. <span data-ttu-id="9bac5-142">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9bac5-142">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="9bac5-143">Om du redan har konfigurerat rutan för enkel inloggning, söka efter din instans av rutan med hjälp av hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="9bac5-143">If you have already configured Box for single sign-on, search for your instance of Box using hello search field.</span></span> <span data-ttu-id="9bac5-144">Annars väljer **Lägg till** och Sök efter **rutan** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="9bac5-144">Otherwise, select **Add** and search for **Box** in hello application gallery.</span></span> <span data-ttu-id="9bac5-145">Markera kryssrutan från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="9bac5-145">Select Box from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="9bac5-146">Välj din instans av rutan och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="9bac5-146">Select your instance of Box, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="9bac5-147">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="9bac5-147">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. <span data-ttu-id="9bac5-149">Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera** tooopen en dialogruta för inloggning i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="9bac5-149">Under hello **Admin Credentials** section, click **Authorize** tooopen a Box login dialog in a new browser window.</span></span>

6. <span data-ttu-id="9bac5-150">På hello **inloggning toogrant åtkomst tooBox** anger hello krävs autentiseringsuppgifter, och klicka sedan på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="9bac5-150">On hello **Login toogrant access tooBox** page, provide hello required credentials, and then click **Authorize**.</span></span> 
   
    <span data-ttu-id="9bac5-151">![Aktivera automatisk användaretablering](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="9bac5-151">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "Enable automatic user provisioning")</span></span>

7. <span data-ttu-id="9bac5-152">Klicka på **bevilja åtkomst tooBox** tooauthorize den här åtgärden och tooreturn toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9bac5-152">Click **Grant access tooBox** tooauthorize this operation and tooreturn toohello Azure portal.</span></span> 
   
    <span data-ttu-id="9bac5-153">![Aktivera automatisk användaretablering](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "aktivera automatisk användaretablering")</span><span class="sxs-lookup"><span data-stu-id="9bac5-153">![Enable automatic user provisioning](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "Enable automatic user provisioning")</span></span>

8. <span data-ttu-id="9bac5-154">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Box-app.</span><span class="sxs-lookup"><span data-stu-id="9bac5-154">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Box app.</span></span> <span data-ttu-id="9bac5-155">Om hello anslutningen misslyckas, kontrollera Box-konto har administratörsbehörigheter för teamet och försök hello **”Godkänn”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="9bac5-155">If hello connection fails, ensure your Box account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

9. <span data-ttu-id="9bac5-156">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="9bac5-156">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

10. <span data-ttu-id="9bac5-157">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="9bac5-157">Click **Save.**</span></span>

11. <span data-ttu-id="9bac5-158">Välj under hello mappningar avsnitt, **tooBox synkronisera Azure Active Directory-användare.**</span><span class="sxs-lookup"><span data-stu-id="9bac5-158">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooBox.**</span></span>

12. <span data-ttu-id="9bac5-159">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooBox.</span><span class="sxs-lookup"><span data-stu-id="9bac5-159">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooBox.</span></span> <span data-ttu-id="9bac5-160">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i rutan för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="9bac5-160">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Box for update operations.</span></span> <span data-ttu-id="9bac5-161">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="9bac5-161">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="9bac5-162">tooenable hello Azure AD-etablering tjänsten efter, ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="9bac5-162">tooenable hello Azure AD provisioning service for Box, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

14. <span data-ttu-id="9bac5-163">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="9bac5-163">Click **Save.**</span></span>

<span data-ttu-id="9bac5-164">Som startar hello den första synkroniseringen av användare och/eller grupper som har tilldelats tooBox i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9bac5-164">That starts hello initial synchronization of any users and/or groups assigned tooBox in hello Users and Groups section.</span></span> <span data-ttu-id="9bac5-165">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="9bac5-165">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="9bac5-166">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras på Box-app.</span><span class="sxs-lookup"><span data-stu-id="9bac5-166">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Box app.</span></span>

<span data-ttu-id="9bac5-167">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="9bac5-167">You can now create a test account.</span></span> <span data-ttu-id="9bac5-168">Vänta tills in tooverify hello kontot har synkroniserats toobox too20 i minuter.</span><span class="sxs-lookup"><span data-stu-id="9bac5-168">Wait for up too20 minutes tooverify that hello account has been synchronized toobox.</span></span>

<span data-ttu-id="9bac5-169">I rutan-klient synkroniserade användare visas under **hanterade användare** i hello **administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="9bac5-169">In your Box tenant, synchronized users are listed under **Managed Users** in hello **Admin Console**.</span></span>

<span data-ttu-id="9bac5-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "integrering status")</span><span class="sxs-lookup"><span data-stu-id="9bac5-170">![Integration status](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "Integration status")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9bac5-171">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9bac5-171">Additional resources</span></span>

* [<span data-ttu-id="9bac5-172">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="9bac5-172">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9bac5-173">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9bac5-173">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="9bac5-174">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9bac5-174">Configure Single Sign-on</span></span>](active-directory-saas-box-tutorial.md)