---
title: "Självstudier: Konfigurera ZenDesk för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du konfigurerar Azure Active Directory för att automatiskt etablera och avetablera användarkonton till ZenDesk."
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
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a><span data-ttu-id="81ce0-103">Självstudier: Konfigurera ZenDesk för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="81ce0-103">Tutorial: Configuring ZenDesk for Automatic User Provisioning</span></span>


<span data-ttu-id="81ce0-104">Syftet med den här kursen är att visa de steg som du behöver göra i ZenDesk och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till ZenDesk.</span><span class="sxs-lookup"><span data-stu-id="81ce0-104">The objective of this tutorial is to show you the steps you need to perform in ZenDesk and Azure AD to automatically provision and de-provision user accounts from Azure AD to ZenDesk.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="81ce0-105">Krav</span><span class="sxs-lookup"><span data-stu-id="81ce0-105">Prerequisites</span></span>

<span data-ttu-id="81ce0-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="81ce0-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="81ce0-107">En Azure Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="81ce0-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="81ce0-108">En ZenDesk-klient med den [företagsplan](https://www.zendesk.com/product/pricing/) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="81ce0-108">A ZenDesk tenant with the [Enterprise plan](https://www.zendesk.com/product/pricing/) or better enabled</span></span> 
*   <span data-ttu-id="81ce0-109">Ett användarkonto i ZenDesk med administratörsbehörigheter</span><span class="sxs-lookup"><span data-stu-id="81ce0-109">A user account in ZenDesk with Admin permissions</span></span> 

> [!NOTE]
> <span data-ttu-id="81ce0-110">Azure AD-etablering integration förlitar sig på den [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), som är tillgängliga för ZenDesk-team på den grundläggande planen eller bättre.</span><span class="sxs-lookup"><span data-stu-id="81ce0-110">The Azure AD provisioning integration relies on the [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), which is available to ZenDesk teams on the Essential plan or better.</span></span>

## <a name="assigning-users-to-zendesk"></a><span data-ttu-id="81ce0-111">Tilldela användare till ZenDesk</span><span class="sxs-lookup"><span data-stu-id="81ce0-111">Assigning users to ZenDesk</span></span>

<span data-ttu-id="81ce0-112">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="81ce0-112">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="81ce0-113">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="81ce0-113">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD are synchronized.</span></span> 

<span data-ttu-id="81ce0-114">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till dina ZenDesk-app.</span><span class="sxs-lookup"><span data-stu-id="81ce0-114">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your ZenDesk app.</span></span> <span data-ttu-id="81ce0-115">När bestämt, kan du tilldela dessa användare i appen ZenDesk genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="81ce0-115">Once decided, you can assign these users to your ZenDesk app by following the instructions here:</span></span>

[<span data-ttu-id="81ce0-116">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="81ce0-116">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a><span data-ttu-id="81ce0-117">Viktiga tips för att tilldela användare till ZenDesk</span><span class="sxs-lookup"><span data-stu-id="81ce0-117">Important tips for assigning users to ZenDesk</span></span>

*   <span data-ttu-id="81ce0-118">Vi rekommenderar att en enda Azure AD-användare har tilldelats ZenDesk för att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="81ce0-118">It is recommended that a single Azure AD user is assigned to ZenDesk to test the provisioning configuration.</span></span> <span data-ttu-id="81ce0-119">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="81ce0-119">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="81ce0-120">När du tilldelar en användare ZenDesk, måste du välja någon av **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan tilldelning.</span><span class="sxs-lookup"><span data-stu-id="81ce0-120">When assigning a user to ZenDesk, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="81ce0-121">Den **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.</span><span class="sxs-lookup"><span data-stu-id="81ce0-121">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>

> [!NOTE]
> <span data-ttu-id="81ce0-122">Som en extra funktion etablering tjänsten läser eventuella anpassade roller som definierats i Zendesk och importerar dem till Azure AD där de kan väljas i dialogrutan Välj roll.</span><span class="sxs-lookup"><span data-stu-id="81ce0-122">As an added feature, the provisioning service reads any custom roles defined in Zendesk, and imports them into Azure AD where they can be selected in the Select Role dialog.</span></span> <span data-ttu-id="81ce0-123">De här rollerna kommer att vara synliga i Azure-portalen när tjänsten etablering är aktiverat och en synkroniseringscykel har slutförts.</span><span class="sxs-lookup"><span data-stu-id="81ce0-123">These roles will be visible in the Azure portal after the provisioning service is enabled and one synchronization cycle has completed.</span></span>

## <a name="configuring-user-provisioning-to-zendesk"></a><span data-ttu-id="81ce0-124">Konfigurering av användarförsörjning till ZenDesk</span><span class="sxs-lookup"><span data-stu-id="81ce0-124">Configuring user provisioning to ZenDesk</span></span> 

<span data-ttu-id="81ce0-125">Det här avsnittet hjälper dig att ansluta din Azure AD till Zendesks användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i ZenDesk baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="81ce0-125">This section guides you through connecting your Azure AD to ZenDesk's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in ZenDesk based on user and group assignment in Azure AD.</span></span>

> [!TIP] 
> <span data-ttu-id="81ce0-126">Du kan också välja att aktivera SAML-baserade enkel inloggning för ZenDesk, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="81ce0-126">You may also choose to enabled SAML-based Single Sign-On for ZenDesk, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="81ce0-127">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="81ce0-127">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a><span data-ttu-id="81ce0-128">Konfigurera automatisk konto användaretablering till ZenDesk i Azure AD</span><span class="sxs-lookup"><span data-stu-id="81ce0-128">Configure automatic user account provisioning to ZenDesk in Azure AD</span></span>


1. <span data-ttu-id="81ce0-129">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="81ce0-129">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="81ce0-130">Om du redan har konfigurerat ZenDesk för enkel inloggning, söka efter din instans av ZenDesk med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="81ce0-130">If you have already configured ZenDesk for single sign-on, search for your instance of ZenDesk using the search field.</span></span> <span data-ttu-id="81ce0-131">Annars väljer **Lägg till** och Sök efter **ZenDesk** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="81ce0-131">Otherwise, select **Add** and search for **ZenDesk** in the application gallery.</span></span> <span data-ttu-id="81ce0-132">Välj ZenDesk i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="81ce0-132">Select ZenDesk from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="81ce0-133">Välj din instans av ZenDesk och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="81ce0-133">Select your instance of ZenDesk, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="81ce0-134">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="81ce0-134">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![ZenDesk-etablering](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. <span data-ttu-id="81ce0-136">Under den **administratörsautentiseringsuppgifter** avsnitt, ange den **Admin Username tokenkey & domän** genereras av dina ZenDesk-konto (du kan söka efter token under kontot: **Admin**  >  **API** > **inställningar**).</span><span class="sxs-lookup"><span data-stu-id="81ce0-136">Under the **Admin Credentials** section, input the **Admin Username&tokenkey&Domain** generated by your ZenDesk's account (you can find the token under your account: **Admin** > **API** > **Settings**).</span></span> 

    ![ZenDesk-etablering](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. <span data-ttu-id="81ce0-138">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din ZenDesk-app.</span><span class="sxs-lookup"><span data-stu-id="81ce0-138">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your ZenDesk app.</span></span> <span data-ttu-id="81ce0-139">Om anslutningen misslyckas, kontrollera ditt ZenDesk-konto som har administratörsbehörigheter och försök steg 5 igen.</span><span class="sxs-lookup"><span data-stu-id="81ce0-139">If the connection fails, ensure your ZenDesk account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="81ce0-140">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.</span><span class="sxs-lookup"><span data-stu-id="81ce0-140">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="81ce0-141">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="81ce0-141">Click **Save**.</span></span> 

9. <span data-ttu-id="81ce0-142">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare ZenDesk**.</span><span class="sxs-lookup"><span data-stu-id="81ce0-142">Under the Mappings section, select **Synchronize Azure Active Directory Users to ZenDesk**.</span></span>

10. <span data-ttu-id="81ce0-143">I den **attributmappning** avsnittet kan du granska användarattribut som ska synkroniseras från Azure AD med ZenDesk.</span><span class="sxs-lookup"><span data-stu-id="81ce0-143">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to ZenDesk.</span></span> <span data-ttu-id="81ce0-144">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i ZenDesk för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="81ce0-144">The attributes selected as **Matching** properties are used to match the user accounts in ZenDesk for update operations.</span></span> <span data-ttu-id="81ce0-145">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="81ce0-145">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="81ce0-146">Om du vill aktivera Azure AD-tjänsten för ZenDesk-etablering, ändra den **Status för etablering** till **på** i den **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="81ce0-146">To enable the Azure AD provisioning service for ZenDesk, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="81ce0-147">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="81ce0-147">Click **Save**.</span></span> 

<span data-ttu-id="81ce0-148">Den här åtgärden startar den första synkroniseringen av användare och/eller grupper som tilldelas till ZenDesk i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="81ce0-148">This operation starts the initial synchronization of any users and/or groups assigned to ZenDesk in the Users and Groups section.</span></span> <span data-ttu-id="81ce0-149">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="81ce0-149">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="81ce0-150">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering.</span><span class="sxs-lookup"><span data-stu-id="81ce0-150">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="81ce0-151">Mer information om hur du tolkar Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="81ce0-151">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="81ce0-152">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="81ce0-152">Additional resources</span></span>

* [<span data-ttu-id="81ce0-153">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="81ce0-153">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="81ce0-154">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="81ce0-154">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="81ce0-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81ce0-155">Next steps</span></span>

* [<span data-ttu-id="81ce0-156">Lär dig hur du granska loggarna och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="81ce0-156">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
