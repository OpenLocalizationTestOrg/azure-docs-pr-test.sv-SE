---
title: "Självstudier: Azure Active Directory-integrering med Citrix GoToMeeting | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 3c6eed5309dfa384c292b0cf63f8aa58988add81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a><span data-ttu-id="7540c-103">Självstudier: Konfigurera Citrix GoToMeeting för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="7540c-103">Tutorial: Configuring Citrix GoToMeeting for Automatic User Provisioning</span></span>

<span data-ttu-id="7540c-104">hello syftet med den här kursen är tooshow du hello stegen tooperform i Citrix GoToMeeting och Azure AD tooautomatically etablera och avinstallation etablera användarkonton från Azure AD tooCitrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="7540c-104">hello objective of this tutorial is tooshow you hello steps you need tooperform in Citrix GoToMeeting and Azure AD tooautomatically provision and de-provision user accounts from Azure AD tooCitrix GoToMeeting.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7540c-105">Krav</span><span class="sxs-lookup"><span data-stu-id="7540c-105">Prerequisites</span></span>

<span data-ttu-id="7540c-106">hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7540c-106">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

*   <span data-ttu-id="7540c-107">En Azure Active directory-klient.</span><span class="sxs-lookup"><span data-stu-id="7540c-107">An Azure Active directory tenant.</span></span>
*   <span data-ttu-id="7540c-108">En Citrix GoToMeeting enkel inloggning för aktiverade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7540c-108">A Citrix GoToMeeting single-sign on enabled subscription.</span></span>
*   <span data-ttu-id="7540c-109">Ett användarkonto i Citrix GoToMeeting Team administratörsbehörigheter.</span><span class="sxs-lookup"><span data-stu-id="7540c-109">A user account in Citrix GoToMeeting with Team Admin permissions.</span></span>

## <a name="assigning-users-toocitrix-gotomeeting"></a><span data-ttu-id="7540c-110">Tilldela användare tooCitrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="7540c-110">Assigning users tooCitrix GoToMeeting</span></span>

<span data-ttu-id="7540c-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” toodetermine som användarna ska få åtkomst till tooselected appar.</span><span class="sxs-lookup"><span data-stu-id="7540c-111">Azure Active Directory uses a concept called "assignments" toodetermine which users should receive access tooselected apps.</span></span> <span data-ttu-id="7540c-112">Hello gäller automatisk konto användaretablering är är bara hello användare och grupper som har ”tilldelats” tooan program i Azure AD synkroniserad.</span><span class="sxs-lookup"><span data-stu-id="7540c-112">In hello context of automatic user account provisioning, only hello users and groups that have been "assigned" tooan application in Azure AD is synchronized.</span></span>

<span data-ttu-id="7540c-113">Innan du konfigurerar och aktiverar hello etableras, måste toodecide vilka användare och/eller grupper i Azure AD som representerar hello-användare som behöver åtkomst till tooyour Citrix GoToMeeting app.</span><span class="sxs-lookup"><span data-stu-id="7540c-113">Before configuring and enabling hello provisioning service, you need toodecide what users and/or groups in Azure AD represent hello users who need access tooyour Citrix GoToMeeting app.</span></span> <span data-ttu-id="7540c-114">När du valt, kan du tilldela dessa användare tooyour Citrix GoToMeeting app genom att följa hello anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="7540c-114">Once decided, you can assign these users tooyour Citrix GoToMeeting app by following hello instructions here:</span></span>

[<span data-ttu-id="7540c-115">Tilldela en användare eller grupp tooan enterprise app</span><span class="sxs-lookup"><span data-stu-id="7540c-115">Assign a user or group tooan enterprise app</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toocitrix-gotomeeting"></a><span data-ttu-id="7540c-116">Viktiga tips för att tilldela användare tooCitrix GoToMeeting</span><span class="sxs-lookup"><span data-stu-id="7540c-116">Important tips for assigning users tooCitrix GoToMeeting</span></span>

*   <span data-ttu-id="7540c-117">Vi rekommenderar att en enda Azure AD-användare är tilldelad tooCitrix GoToMeeting tootest hello etablering konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7540c-117">It is recommended that a single Azure AD user is assigned tooCitrix GoToMeeting tootest hello provisioning configuration.</span></span> <span data-ttu-id="7540c-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="7540c-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="7540c-119">Du måste välja en giltig användarroll när du tilldelar en användare tooCitrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="7540c-119">When assigning a user tooCitrix GoToMeeting, you must select a valid user role.</span></span> <span data-ttu-id="7540c-120">Hej ”standard” rollen fungerar inte för etablering.</span><span class="sxs-lookup"><span data-stu-id="7540c-120">hello "Default Access" role does not work for provisioning.</span></span>

## <a name="enable-automated-user-provisioning"></a><span data-ttu-id="7540c-121">Aktivera automatiserad etablering av användare</span><span class="sxs-lookup"><span data-stu-id="7540c-121">Enable Automated User Provisioning</span></span>

<span data-ttu-id="7540c-122">Det här avsnittet hjälper dig att ansluta din Azure AD tooCitrix GoToMeeting användarkontot API-etablering och konfigurerar hello etablering service toocreate, uppdatera och inaktivera tilldelad användare konton i Citrix GoToMeeting baserat på användare och grupper tilldelning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7540c-122">This section guides you through connecting your Azure AD tooCitrix GoToMeeting's user account provisioning API, and configuring hello provisioning service toocreate, update, and disable assigned user accounts in Citrix GoToMeeting based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="7540c-123">Du kan också välja tooenabled SAML-baserade enkel inloggning för Citrix GoToMeeting, följa instruktionerna i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7540c-123">You may also choose tooenabled SAML-based Single Sign-On for Citrix GoToMeeting, following hello instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7540c-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="7540c-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>

### <a name="tooconfigure-automatic-user-account-provisioning"></a><span data-ttu-id="7540c-125">tooconfigure automatisk användarens konto-etablering:</span><span class="sxs-lookup"><span data-stu-id="7540c-125">tooconfigure automatic user account provisioning:</span></span>

1. <span data-ttu-id="7540c-126">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7540c-126">In hello [Azure portal](https://portal.azure.com), browse toohello **Azure Active Directory > Enterprise Apps > All applications** section.</span></span>

2. <span data-ttu-id="7540c-127">Om du redan har konfigurerat Citrix GoToMeeting för enkel inloggning, söka efter din instans av Citrix GoToMeeting hjälp hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="7540c-127">If you have already configured Citrix GoToMeeting for single sign-on, search for your instance of Citrix GoToMeeting using hello search field.</span></span> <span data-ttu-id="7540c-128">Annars väljer **Lägg till** och Sök efter **Citrix GoToMeeting** i hello programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="7540c-128">Otherwise, select **Add** and search for **Citrix GoToMeeting** in hello application gallery.</span></span> <span data-ttu-id="7540c-129">Välj Citrix GoToMeeting från hello sökresultaten och lägga till den tooyour listan med program.</span><span class="sxs-lookup"><span data-stu-id="7540c-129">Select Citrix GoToMeeting from hello search results, and add it tooyour list of applications.</span></span>

3. <span data-ttu-id="7540c-130">Välj din instans av Citrix GoToMeeting och sedan hello **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="7540c-130">Select your instance of Citrix GoToMeeting, then select hello **Provisioning** tab.</span></span>

4. <span data-ttu-id="7540c-131">Ange hello **etablering** läge för**automatisk**.</span><span class="sxs-lookup"><span data-stu-id="7540c-131">Set hello **Provisioning** Mode too**Automatic**.</span></span> 

    ![Etablering](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. <span data-ttu-id="7540c-133">Gör följande hello under hello administratörsautentiseringsuppgifter avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7540c-133">Under hello Admin Credentials section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7540c-134">a.</span><span class="sxs-lookup"><span data-stu-id="7540c-134">a.</span></span> <span data-ttu-id="7540c-135">I hello **Citrix GoToMeeting administratörsanvändarnamnet** textruta Skriv hello användarnamn för en administratör.</span><span class="sxs-lookup"><span data-stu-id="7540c-135">In hello **Citrix GoToMeeting Admin User Name** textbox, type hello user name of an administrator.</span></span>

    <span data-ttu-id="7540c-136">b.</span><span class="sxs-lookup"><span data-stu-id="7540c-136">b.</span></span> <span data-ttu-id="7540c-137">I hello **Citrix GoToMeeting adminlösenord** textruta hello administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="7540c-137">In hello **Citrix GoToMeeting Admin Password** textbox, hello administrator's password.</span></span>

6. <span data-ttu-id="7540c-138">I hello Azure-portalen klickar du på **Testanslutningen** tooensure Azure AD kan ansluta tooyour Citrix GoToMeeting app.</span><span class="sxs-lookup"><span data-stu-id="7540c-138">In hello Azure portal, click **Test Connection** tooensure Azure AD can connect tooyour Citrix GoToMeeting app.</span></span> <span data-ttu-id="7540c-139">Om hello anslutningen misslyckas, kontrollera kontots Citrix GoToMeeting har administratörsbehörigheter för Team och försök hello **”administratörsautentiseringsuppgifter”** steg igen.</span><span class="sxs-lookup"><span data-stu-id="7540c-139">If hello connection fails, ensure your Citrix GoToMeeting account has Team Admin permissions and try hello **"Admin Credentials"** step again.</span></span>

7. <span data-ttu-id="7540c-140">Ange hello e-postadress för en person eller grupp som ska få meddelanden om etablering fel i hello **e-postmeddelande** fältet och hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7540c-140">Enter hello email address of a person or group who should receive provisioning error notifications in hello **Notification Email** field, and check hello checkbox.</span></span>

8. <span data-ttu-id="7540c-141">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="7540c-141">Click **Save.**</span></span>

9. <span data-ttu-id="7540c-142">Välj under hello mappningar avsnitt, **synkronisera Azure Active Directory-användare tooCitrix GoToMeeting.**</span><span class="sxs-lookup"><span data-stu-id="7540c-142">Under hello Mappings section, select **Synchronize Azure Active Directory Users tooCitrix GoToMeeting.**</span></span>

10. <span data-ttu-id="7540c-143">I hello **attributmappning** avsnittet kan du granska hello användarattribut som synkroniseras från Azure AD tooCitrix GoToMeeting.</span><span class="sxs-lookup"><span data-stu-id="7540c-143">In hello **Attribute Mappings** section, review hello user attributes that are synchronized from Azure AD tooCitrix GoToMeeting.</span></span> <span data-ttu-id="7540c-144">Hej attribut som valts som **matchande** egenskaper är används toomatch hello användarkonton i Citrix GoToMeeting för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7540c-144">hello attributes selected as **Matching** properties are used toomatch hello user accounts in Citrix GoToMeeting for update operations.</span></span> <span data-ttu-id="7540c-145">Välj hello spara knappen toocommit ändringar.</span><span class="sxs-lookup"><span data-stu-id="7540c-145">Select hello Save button toocommit any changes.</span></span>

11. <span data-ttu-id="7540c-146">tooenable hello Azure AD-etablering tjänsten för Citrix GoToMeeting, ändra hello **Status för etablering** för**på** i hello inställningar</span><span class="sxs-lookup"><span data-stu-id="7540c-146">tooenable hello Azure AD provisioning service for Citrix GoToMeeting, change hello **Provisioning Status** too**On** in hello Settings section</span></span>

12. <span data-ttu-id="7540c-147">Klicka på **spara.**</span><span class="sxs-lookup"><span data-stu-id="7540c-147">Click **Save.**</span></span>

<span data-ttu-id="7540c-148">Den startar hello inledande synkronisering av alla användare och/eller grupper som har tilldelats tooCitrix GoToMeeting under hello användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="7540c-148">It starts hello initial synchronization of any users and/or groups assigned tooCitrix GoToMeeting in hello Users and Groups section.</span></span> <span data-ttu-id="7540c-149">hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="7540c-149">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> <span data-ttu-id="7540c-150">Du kan använda hello **synkroniseringsinformation** avsnittet toomonitor förlopp och följ länkarna tooprovisioning aktivitetsrapporter, som beskriver alla åtgärder som utförs av hello etableras Citrix GoToMeeting appen.</span><span class="sxs-lookup"><span data-stu-id="7540c-150">You can use hello **Synchronization Details** section toomonitor progress and follow links tooprovisioning activity reports, which describe all actions performed by hello provisioning service on your Citrix GoToMeeting app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7540c-151">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7540c-151">Additional resources</span></span>

* [<span data-ttu-id="7540c-152">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="7540c-152">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7540c-153">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7540c-153">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7540c-154">Konfigurera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7540c-154">Configure Single Sign-on</span></span>](active-directory-saas-citrix-gotomeeting-tutorial.md)


