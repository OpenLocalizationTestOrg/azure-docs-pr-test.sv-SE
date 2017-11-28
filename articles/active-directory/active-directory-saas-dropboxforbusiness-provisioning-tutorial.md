---
title: "Självstudier: Azure Active Directory-integrering med Dropbox för företag | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Dropbox för företag."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f3a42e4-6897-4234-af84-b47c148ec3e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 0fb01eab4f7c6c4516eac64a4343e46ea221f98d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-dropbox-for-business-for-automatic-user-provisioning"></a><span data-ttu-id="7e8bc-103">Självstudier: Konfigurera Dropbox för företag för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="7e8bc-103">Tutorial: Configuring Dropbox for Business for Automatic User Provisioning</span></span>

<span data-ttu-id="7e8bc-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Dropbox för företag och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooDropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Dropbox for Business and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooDropbox for Business.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e8bc-105">Krav</span><span class="sxs-lookup"><span data-stu-id="7e8bc-105">Prerequisites</span></span>

<span data-ttu-id="7e8bc-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7e8bc-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="7e8bc-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="7e8bc-108">En Dropbox för Business enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-108">A Dropbox for Business single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="7e8bc-109">Ett användarkonto i Dropbox för företag med Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-109">A user account in Dropbox for Business with Team Admin permissions.</span></span>

## <a name="assigning-users-toodropbox-for-business"></a><span data-ttu-id="7e8bc-110">Tilldela användare tooDropbox för företag</span><span class="sxs-lookup"><span data-stu-id="7e8bc-110">Assigning users tooDropbox for Business</span></span>

<span data-ttu-id="7e8bc-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="7e8bc-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="7e8bc-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Dropbox for Business app.</span></span> <span data-ttu-id="7e8bc-114">När du valt, kan du tilldela dessa användare tooyour Dropbox för företag genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="7e8bc-114">Once decided, you can assign these users tooyour Dropbox for Business app by following hello instructions here:</span></span>

[<span data-ttu-id="7e8bc-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="7e8bc-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toodropbox-for-business"></a><span data-ttu-id="7e8bc-116">Viktiga tips för att tilldela användare tooDropbox för företag</span><span class="sxs-lookup"><span data-stu-id="7e8bc-116">Important tips for assigning users tooDropbox for Business</span></span>

*   <span data-ttu-id="7e8bc-117">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooDropbox för Business tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-117">It is recommended that a single Azure AD user is assigned tooDropbox for Business tootest hello provisioning configuration.</span></span> <span data-ttu-id="7e8bc-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="7e8bc-119">Du måste välja en giltig användarroll när du tilldelar en användare tooDropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-119">When assigning a user tooDropbox for Business, you must select a valid user role.</span></span> <span data-ttu-id="7e8bc-120">Hej ”standard” rollen fungerar inte för att etablera...</span><span class="sxs-lookup"><span data-stu-id="7e8bc-120">hello "Default Access" role does not work for provisioning..</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="7e8bc-121">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="7e8bc-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="7e8bc-122">Det här avsnittet hjälper dig att ansluta din Azure AD-tooDropbox för företagets användarkonto API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelade användarkonton i Dropbox för företag baserat på användare och grupper tilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-122">This section guides you through connecting your Azure AD tooDropbox for Business's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Dropbox for Business based on user and group assignment in Azure AD.</span></span>

>[!Tip]
><span data-ttu-id="7e8bc-123">Du kan också välja tooenabled SAML-baserade enkel inloggning för Dropbox för företag, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e8bc-123">You may also choose tooenabled SAML-based Single Sign-On for Dropbox for Business, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7e8bc-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="7e8bc-125">tooconfigure automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="7e8bc-125">tooconfigure automatic user account provisioning:</span></span>

1. <span data-ttu-id="7e8bc-126">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="7e8bc-127">Om du redan har konfigurerat Dropbox för företag för enkel inloggning söka efter din instans av Dropbox för företag med hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-127">If you have already configured Dropbox for Business for single sign-on, search for your instance of Dropbox for Business using hello search field.</span></span> <span data-ttu-id="7e8bc-128">Annars väljer **Lägg till** och Sök efter **Dropbox för företag** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-128">Otherwise, select **Add** and search for **Dropbox for Business** in hello application gallery.</span></span> <span data-ttu-id="7e8bc-129">Välj Dropbox för företag från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-129">Select Dropbox for Business from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="7e8bc-130">Välj din instans av Dropbox för företag och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-130">Select your instance of Dropbox for Business, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="7e8bc-131">Ange hello **etablering läge** för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-131">Set hello **Provisioning Mode** too**Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="7e8bc-133">Under hello **administratörsautentiseringsuppgifter** klickar du på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-133">Under hello **Admin Credentials** section, click **Authorize**.</span></span> <span data-ttu-id="7e8bc-134">En Dropbox för Business inloggningsruta öppnas i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-134">It opens a Dropbox for Business login dialog in a new browser window.</span></span>

6. <span data-ttu-id="7e8bc-135">På hello **inloggning tooDropbox toolink med Azure AD** dialogrutan Logga in tooyour Dropbox för företag-klient.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-135">On hello **Sign-in tooDropbox toolink with Azure AD** dialog, sign in tooyour Dropbox for Business tenant.</span></span>

     <span data-ttu-id="7e8bc-136">![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "användaretablering")</span><span class="sxs-lookup"><span data-stu-id="7e8bc-136">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769518.png "User provisioning")</span></span>

7. <span data-ttu-id="7e8bc-137">Bekräfta att du vill att toogive Azure Active Directory behörighet toomake ändringar tooyour Dropbox för företag-klient.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-137">Confirm that you would like toogive Azure Active Directory permission toomake changes tooyour Dropbox for Business tenant.</span></span> <span data-ttu-id="7e8bc-138">Klicka på **Tillåt**.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-138">Click **Allow**.</span></span>
    
      <span data-ttu-id="7e8bc-139">![Användaretablering](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "användaretablering")</span><span class="sxs-lookup"><span data-stu-id="7e8bc-139">![User provisioning](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/ic769519.png "User provisioning")</span></span>

8. <span data-ttu-id="7e8bc-140">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Dropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-140">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Dropbox for Business app.</span></span> <span data-ttu-id="7e8bc-141">Om hello anslutningen misslyckas, kontrollera din Dropbox för företag-konto som har administratörsbehörigheter för teamet och försök hello **”Godkänn”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-141">If hello connection fails, ensure your Dropbox for Business account has Team Admin permissions and try hello **"Authorize"** step again.</span></span>

9. <span data-ttu-id="7e8bc-142">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-142">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

10. <span data-ttu-id="7e8bc-143">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="7e8bc-143">Click **Save.**</span></span>

11. <span data-ttu-id="7e8bc-144">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooDropbox för företag.**</span><span class="sxs-lookup"><span data-stu-id="7e8bc-144">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooDropbox for Business.**</span></span>

12. <span data-ttu-id="7e8bc-145">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooDropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-145">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooDropbox for Business.</span></span> <span data-ttu-id="7e8bc-146">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Dropbox för företag för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-146">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Dropbox for Business for update operations.</span></span> <span data-ttu-id="7e8bc-147">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-147">Select hello Save button toocommit any changes.</span></span>

13. <span data-ttu-id="7e8bc-148">tooenable hello Azure AD-etablering tjänsten för Dropbox för företag, ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="7e8bc-148">tooenable hello Azure AD provisioning service for Dropbox for Business, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

14. <span data-ttu-id="7e8bc-149">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="7e8bc-149">Click **Save.**</span></span>

<span data-ttu-id="7e8bc-150">Den startar hello den första synkroniseringen av användare och/eller grupper som tilldelas tooDropbox för företag i hello användare och grupper avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-150">It starts hello initial synchronization of any users and/or groups assigned tooDropbox for Business in hello Users and Groups section.</span></span> <span data-ttu-id="7e8bc-151">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-151">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="7e8bc-152">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello för företag-tjänsten på din Dropbox-etablering.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-152">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Dropbox for Business app.</span></span>

<span data-ttu-id="7e8bc-153">Du kan nu skapa ett testkonto.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-153">You can now create a test account.</span></span> <span data-ttu-id="7e8bc-154">Vänta tills in too20 minuter tooverify hello kontot har synkroniserats tooDropbox för företag.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-154">Wait for up too20 minutes tooverify that hello account has been synchronized tooDropbox for Business.</span></span>

<span data-ttu-id="7e8bc-155">En användare som har slutförts etablering cykel visas med status relaterade.</span><span class="sxs-lookup"><span data-stu-id="7e8bc-155">A successfully completed user provisioning cycle is indicated by a related status.</span></span>

<span data-ttu-id="7e8bc-156">![Tilldela användare](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "tilldela användare")</span><span class="sxs-lookup"><span data-stu-id="7e8bc-156">![Assign users](./media/active-directory-saas-dropboxforbusiness-provisioning-tutorial/IC769523.png "Assign users")</span></span>


## <a name="additional-resources"></a><span data-ttu-id="7e8bc-157">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7e8bc-157">Additional resources</span></span>

* [<span data-ttu-id="7e8bc-158">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="7e8bc-158">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7e8bc-159">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7e8bc-159">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7e8bc-160">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e8bc-160">Configure Single Sign-on</span></span>](active-directory-saas-dropboxforbusiness-tutorial.md)