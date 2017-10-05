---
title: "Självstudier: Konfigurera LucidChart för automatisk användaretablering med Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du konfigurerar Azure Active Directory för att automatiskt etablera och avetablera användarkonton till LucidChart."
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
ms.openlocfilehash: 1f9344a5e750360e21ed7dc8e3ed013c2c2e1a45
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a><span data-ttu-id="1a616-103">Självstudier: Konfigurera LucidChart för automatisk Användaretablering</span><span class="sxs-lookup"><span data-stu-id="1a616-103">Tutorial: Configuring LucidChart for Automatic User Provisioning</span></span>


<span data-ttu-id="1a616-104">Syftet med den här kursen är att visa de steg som du behöver göra i LucidChart och Azure AD för att automatiskt etablera och avetablera användarkonton från Azure AD till LucidChart.</span><span class="sxs-lookup"><span data-stu-id="1a616-104">The objective of this tutorial is to show you the steps you need to perform in LucidChart and Azure AD to automatically provision and de-provision user accounts from Azure AD to LucidChart.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1a616-105">Krav</span><span class="sxs-lookup"><span data-stu-id="1a616-105">Prerequisites</span></span>

<span data-ttu-id="1a616-106">Det scenario som beskrivs i den här kursen förutsätter att du redan har följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1a616-106">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

*   <span data-ttu-id="1a616-107">En Azure Active directory-klient</span><span class="sxs-lookup"><span data-stu-id="1a616-107">An Azure Active directory tenant</span></span>
*   <span data-ttu-id="1a616-108">En LucidChart klient med den [företagsplan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) eller bättre aktiverat</span><span class="sxs-lookup"><span data-stu-id="1a616-108">A LucidChart tenant with the [Enterprise plan](https://www.lucidchart.com/user/117598685#/subscriptionLevel) or better enabled</span></span> 
*   <span data-ttu-id="1a616-109">Ett användarkonto i LucidChart med administratörsbehörigheter</span><span class="sxs-lookup"><span data-stu-id="1a616-109">A user account in LucidChart with Admin permissions</span></span> 

## <a name="assigning-users-to-lucidchart"></a><span data-ttu-id="1a616-110">Tilldela användare till LucidChart</span><span class="sxs-lookup"><span data-stu-id="1a616-110">Assigning users to LucidChart</span></span>

<span data-ttu-id="1a616-111">Azure Active Directory använder ett begrepp som kallas ”tilldelningar” för att avgöra vilka användare ska få åtkomst till valda appar.</span><span class="sxs-lookup"><span data-stu-id="1a616-111">Azure Active Directory uses a concept called "assignments" to determine which users should receive access to selected apps.</span></span> <span data-ttu-id="1a616-112">I samband med automatisk konto användaretablering, synkroniseras de användare och grupper som har ”tilldelats” till ett program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a616-112">In the context of automatic user account provisioning, only the users and groups that have been "assigned" to an application in Azure AD is synchronized.</span></span> 

<span data-ttu-id="1a616-113">Innan du konfigurerar och aktiverar tjänsten etablering, måste du bestämma vilka användare och/eller grupper i Azure AD representerar de användare som behöver åtkomst till appen LucidChart.</span><span class="sxs-lookup"><span data-stu-id="1a616-113">Before configuring and enabling the provisioning service, you need to decide what users and/or groups in Azure AD represent the users who need access to your LucidChart app.</span></span> <span data-ttu-id="1a616-114">När bestämt, kan du tilldela dessa användare i appen LucidChart genom att följa anvisningarna här:</span><span class="sxs-lookup"><span data-stu-id="1a616-114">Once decided, you can assign these users to your LucidChart app by following the instructions here:</span></span>

[<span data-ttu-id="1a616-115">Tilldela en användare eller grupp till en enterprise-app</span><span class="sxs-lookup"><span data-stu-id="1a616-115">Assign a user or group to an enterprise app</span></span>](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-lucidchart"></a><span data-ttu-id="1a616-116">Viktiga tips för att tilldela användare till LucidChart</span><span class="sxs-lookup"><span data-stu-id="1a616-116">Important tips for assigning users to LucidChart</span></span>

*   <span data-ttu-id="1a616-117">Vi rekommenderar att en enda Azure AD-användare har tilldelats LucidChart för att testa allokering konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1a616-117">It is recommended that a single Azure AD user is assigned to LucidChart to test the provisioning configuration.</span></span> <span data-ttu-id="1a616-118">Ytterligare användare och/eller grupper kan tilldelas senare.</span><span class="sxs-lookup"><span data-stu-id="1a616-118">Additional users and/or groups may be assigned later.</span></span>

*   <span data-ttu-id="1a616-119">När du tilldelar en användare LucidChart, måste du välja någon av **användaren** roll eller en annan giltig programspecifika roll (om tillgängliga) i dialogrutan tilldelning.</span><span class="sxs-lookup"><span data-stu-id="1a616-119">When assigning a user to LucidChart, you must select either the **User** role, or another valid application-specific role (if available) in the assignment dialog.</span></span> <span data-ttu-id="1a616-120">Den **standard åtkomst** roll fungerar inte för etablering och dessa användare hoppas över.</span><span class="sxs-lookup"><span data-stu-id="1a616-120">The **Default Access** role does not work for provisioning, and these users are skipped.</span></span>


## <a name="configuring-user-provisioning-to-lucidchart"></a><span data-ttu-id="1a616-121">Konfigurering av användarförsörjning till LucidChart</span><span class="sxs-lookup"><span data-stu-id="1a616-121">Configuring user provisioning to LucidChart</span></span> 

<span data-ttu-id="1a616-122">Det här avsnittet hjälper dig att ansluta din Azure AD till Lucidcharts användarkonto API-etablering och konfigurera tjänsten etablering för att skapa, uppdatera och inaktivera tilldelade användarkonton i LucidChart baserat på tilldelning av användare och grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a616-122">This section guides you through connecting your Azure AD to LucidChart's user account provisioning API, and configuring the provisioning service to create, update, and disable assigned user accounts in LucidChart based on user and group assignment in Azure AD.</span></span>

> [!TIP]
> <span data-ttu-id="1a616-123">Du kan också välja att aktivera SAML-baserade enkel inloggning för LucidChart, följer du instruktionerna som anges i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1a616-123">You may also choose to enabled SAML-based Single Sign-On for LucidChart, following the instructions provided in [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="1a616-124">Enkel inloggning kan konfigureras oberoende av Automatisk etablering, även om dessa två funktioner komplettera varandra.</span><span class="sxs-lookup"><span data-stu-id="1a616-124">Single sign-on can be configured independently of automatic provisioning, though these two features compliment each other.</span></span>


### <a name="configure-automatic-user-account-provisioning-to-lucidchart-in-azure-ad"></a><span data-ttu-id="1a616-125">Konfigurera automatisk konto användaretablering till LucidChart i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a616-125">Configure automatic user account provisioning to LucidChart in Azure AD</span></span>


1. <span data-ttu-id="1a616-126">I den [Azure-portalen](https://portal.azure.com), bläddra till den **Azure Active Directory > Företagsappar > alla program** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1a616-126">In the [Azure portal](https://portal.azure.com), browse to the **Azure Active Directory > Enterprise Apps > All applications**  section.</span></span>

2. <span data-ttu-id="1a616-127">Om du redan har konfigurerat LucidChart för enkel inloggning, söka efter din instans av LucidChart med hjälp av sökfältet.</span><span class="sxs-lookup"><span data-stu-id="1a616-127">If you have already configured LucidChart for single sign-on, search for your instance of LucidChart using the search field.</span></span> <span data-ttu-id="1a616-128">Annars väljer **Lägg till** och Sök efter **LucidChart** i programgalleriet.</span><span class="sxs-lookup"><span data-stu-id="1a616-128">Otherwise, select **Add** and search for **LucidChart** in the application gallery.</span></span> <span data-ttu-id="1a616-129">Välj LucidChart i sökresultatet och lägga till den i listan med program.</span><span class="sxs-lookup"><span data-stu-id="1a616-129">Select LucidChart from the search results, and add it to your list of applications.</span></span>

3. <span data-ttu-id="1a616-130">Välj din instans av LucidChart och sedan den **etablering** fliken.</span><span class="sxs-lookup"><span data-stu-id="1a616-130">Select your instance of LucidChart, then select the **Provisioning** tab.</span></span>

4. <span data-ttu-id="1a616-131">Ange den **Etableringsläge** till **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="1a616-131">Set the **Provisioning Mode** to **Automatic**.</span></span>

    ![LucidChart etablering](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. <span data-ttu-id="1a616-133">Under den **administratörsautentiseringsuppgifter** avsnitt, ange den **hemlighet Token** genereras av din LucidChart konto (du kan söka efter token under kontot: **Team**  >  **Appintegrering** > **SCIM**).</span><span class="sxs-lookup"><span data-stu-id="1a616-133">Under the **Admin Credentials** section, input the **Secret Token** generated by your LucidChart's account (you can find the token under your account: **Team** > **App Integration** > **SCIM**).</span></span> 

    ![LucidChart etablering](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. <span data-ttu-id="1a616-135">I Azure-portalen klickar du på **Testanslutningen** så Azure AD kan ansluta till din LucidChart app.</span><span class="sxs-lookup"><span data-stu-id="1a616-135">In the Azure portal, click **Test Connection** to ensure Azure AD can connect to your LucidChart app.</span></span> <span data-ttu-id="1a616-136">Om anslutningen misslyckas, kontrollera kontots LucidChart har administratörsbehörigheter och försök steg 5 igen.</span><span class="sxs-lookup"><span data-stu-id="1a616-136">If the connection fails, ensure your LucidChart account has Admin permissions and try step 5 again.</span></span>

7. <span data-ttu-id="1a616-137">Ange e-postadressen för en person eller grupp som ska få meddelanden om etablering fel i den **e-postmeddelande** fält och markera kryssrutan ”Skicka ett e-postmeddelande när ett fel uppstår”.</span><span class="sxs-lookup"><span data-stu-id="1a616-137">Enter the email address of a person or group who should receive provisioning error notifications in the **Notification Email** field, and check the checkbox "Send an email notification when a failure occurs."</span></span>

8. <span data-ttu-id="1a616-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1a616-138">Click **Save**.</span></span> 

9. <span data-ttu-id="1a616-139">Välj under avsnittet mappningar **synkronisera Azure Active Directory-användare LucidChart**.</span><span class="sxs-lookup"><span data-stu-id="1a616-139">Under the Mappings section, select **Synchronize Azure Active Directory Users to LucidChart**.</span></span>

10. <span data-ttu-id="1a616-140">I den **attributmappning** avsnittet kan du granska användarattribut som synkroniseras från Azure AD till LucidChart.</span><span class="sxs-lookup"><span data-stu-id="1a616-140">In the **Attribute Mappings** section, review the user attributes that are synchronized from Azure AD to LucidChart.</span></span> <span data-ttu-id="1a616-141">De attribut som valts som **matchande** egenskaper som används för att matcha användarkonton i LucidChart för uppdateringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1a616-141">The attributes selected as **Matching** properties are used to match the user accounts in LucidChart for update operations.</span></span> <span data-ttu-id="1a616-142">Välj knappen Spara för att genomföra ändringarna.</span><span class="sxs-lookup"><span data-stu-id="1a616-142">Select the Save button to commit any changes.</span></span>

11. <span data-ttu-id="1a616-143">Om du vill aktivera Azure AD-tjänsten för LucidChart-etablering, ändra den **Status för etablering** till **på** i den **inställningar** avsnitt</span><span class="sxs-lookup"><span data-stu-id="1a616-143">To enable the Azure AD provisioning service for LucidChart, change the **Provisioning Status** to **On** in the **Settings** section</span></span>

12. <span data-ttu-id="1a616-144">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1a616-144">Click **Save**.</span></span> 

<span data-ttu-id="1a616-145">Den här åtgärden startar den första synkroniseringen av användare och/eller grupper som tilldelas till LucidChart i avsnittet användare och grupper.</span><span class="sxs-lookup"><span data-stu-id="1a616-145">This operation starts the initial synchronization of any users and/or groups assigned to LucidChart in the Users and Groups section.</span></span> <span data-ttu-id="1a616-146">Den första synkroniseringen tar längre tid än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge som tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="1a616-146">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> <span data-ttu-id="1a616-147">Du kan använda den **synkroniseringsinformation** avsnittet för att övervaka förloppet och följ länkarna till att etablera aktivitetsrapporter som beskriver alla åtgärder som utförs av tjänsten etablering.</span><span class="sxs-lookup"><span data-stu-id="1a616-147">You can use the **Synchronization Details** section to monitor progress and follow links to provisioning activity reports, which describe all actions performed by the provisioning service.</span></span>

<span data-ttu-id="1a616-148">Mer information om hur du tolkar Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="1a616-148">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1a616-149">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1a616-149">Additional resources</span></span>

* [<span data-ttu-id="1a616-150">Hantera användare konto-etablering för företag-appar</span><span class="sxs-lookup"><span data-stu-id="1a616-150">Managing user account provisioning for Enterprise Apps</span></span>](active-directory-enterprise-apps-manage-provisioning.md)
* [<span data-ttu-id="1a616-151">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1a616-151">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a><span data-ttu-id="1a616-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a616-152">Next steps</span></span>

* [<span data-ttu-id="1a616-153">Lär dig hur du granska loggarna och få rapporter om etablering aktivitet</span><span class="sxs-lookup"><span data-stu-id="1a616-153">Learn how to review logs and get reports on provisioning activity</span></span>](active-directory-saas-provisioning-reporting.md)
