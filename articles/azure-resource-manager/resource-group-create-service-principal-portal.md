---
title: "aaaCreate identitet för Azure-app i portalen | Microsoft Docs"
description: "Beskriver hur toocreate ett nytt Azure Active Directory-program och tjänstens huvudnamn som kan användas med hello rollbaserad åtkomstkontroll i Azure Resource Manager toomanage åt tooresources."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 9624715ac612f42df6f9e9e67b8233bd4b914174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-portal-toocreate-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a><span data-ttu-id="99e56-103">Använda portalen toocreate ett Azure Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser</span><span class="sxs-lookup"><span data-stu-id="99e56-103">Use portal toocreate an Azure Active Directory application and service principal that can access resources</span></span>

<span data-ttu-id="99e56-104">När du har ett program som behöver tooaccess eller ändra resurser, måste du konfigurera ett program för Azure Active Directory (AD) och tilldelar hello krävs behörigheter tooit.</span><span class="sxs-lookup"><span data-stu-id="99e56-104">When you have an application that needs tooaccess or modify resources, you must set up an Azure Active Directory (AD) application and assign hello required permissions tooit.</span></span> <span data-ttu-id="99e56-105">Den här metoden är bättre toorunning hello app enligt dina autentiseringsuppgifter eftersom:</span><span class="sxs-lookup"><span data-stu-id="99e56-105">This approach is preferable toorunning hello app under your own credentials because:</span></span>

* <span data-ttu-id="99e56-106">Du kan tilldela behörigheter toohello app identitet som skiljer sig från din egen behörighet.</span><span class="sxs-lookup"><span data-stu-id="99e56-106">You can assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="99e56-107">Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="99e56-107">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="99e56-108">Toochange hello app autentiseringsuppgifter har inte ändrar dina ansvarsområden.</span><span class="sxs-lookup"><span data-stu-id="99e56-108">You do not have toochange hello app's credentials if your responsibilities change.</span></span> 
* <span data-ttu-id="99e56-109">Du kan använda en tooautomate certifikatautentisering vid körning av ett oövervakat skript.</span><span class="sxs-lookup"><span data-stu-id="99e56-109">You can use a certificate tooautomate authentication when executing an unattended script.</span></span>

<span data-ttu-id="99e56-110">Det här avsnittet visar hur tooperform de går igenom hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="99e56-110">This topic shows you how tooperform those steps through hello portal.</span></span> <span data-ttu-id="99e56-111">Den fokuserar på en enskild klient-program där programmet hello är avsedda toorun i en enda organisation.</span><span class="sxs-lookup"><span data-stu-id="99e56-111">It focuses on a single-tenant application where hello application is intended toorun within only one organization.</span></span> <span data-ttu-id="99e56-112">Du använder vanligtvis stöd för en innehavare program för line-of-business-program som körs i din organisation.</span><span class="sxs-lookup"><span data-stu-id="99e56-112">You typically use single-tenant applications for line-of-business applications that run within your organization.</span></span>
 
## <a name="required-permissions"></a><span data-ttu-id="99e56-113">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="99e56-113">Required permissions</span></span>
<span data-ttu-id="99e56-114">toocomplete det här avsnittet, du måste ha tillräckliga behörigheter tooregister ett program med Azure AD-klienten och tilldela hello programmet tooa roll i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="99e56-114">toocomplete this topic, you must have sufficient permissions tooregister an application with your Azure AD tenant, and assign hello application tooa role in your Azure subscription.</span></span> <span data-ttu-id="99e56-115">Vi behöver kontrollera att du har hello rätt behörigheter tooperform de här stegen.</span><span class="sxs-lookup"><span data-stu-id="99e56-115">Let's make sure you have hello right permissions tooperform those steps.</span></span>

### <a name="check-azure-active-directory-permissions"></a><span data-ttu-id="99e56-116">Kontrollera behörigheter för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99e56-116">Check Azure Active Directory permissions</span></span>
1. <span data-ttu-id="99e56-117">Logga in tooyour Azure-konto via hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="99e56-117">Log in tooyour Azure Account through hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="99e56-118">Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="99e56-118">Select **Azure Active Directory**.</span></span>

     ![Välj azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. <span data-ttu-id="99e56-120">Välj i Azure Active Directory, **användarinställningar**.</span><span class="sxs-lookup"><span data-stu-id="99e56-120">In Azure Active Directory, select **User settings**.</span></span>

     ![Välj användarinställningar](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. <span data-ttu-id="99e56-122">Kontrollera hello **App registreringar** inställningen.</span><span class="sxs-lookup"><span data-stu-id="99e56-122">Check hello **App registrations** setting.</span></span> <span data-ttu-id="99e56-123">Om anges för**Ja**, icke-administratörer kan registrera AD-appar.</span><span class="sxs-lookup"><span data-stu-id="99e56-123">If set too**Yes**, non-admin users can register AD apps.</span></span> <span data-ttu-id="99e56-124">Den här inställningen innebär att alla användare i hello Azure AD-klient kan registrera en app.</span><span class="sxs-lookup"><span data-stu-id="99e56-124">This setting means any user in hello Azure AD tenant can register an app.</span></span> <span data-ttu-id="99e56-125">Du kan fortsätta för[Kontrollera Azure-Prenumerationsbehörigheter](#check-azure-subscription-permissions).</span><span class="sxs-lookup"><span data-stu-id="99e56-125">You can proceed too[Check Azure subscription permissions](#check-azure-subscription-permissions).</span></span>

     ![Visa appen registreringar](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. <span data-ttu-id="99e56-127">Om hello app registreringar inställning har angetts för**nr**, men endast administrativa användare kan registrera appar.</span><span class="sxs-lookup"><span data-stu-id="99e56-127">If hello app registrations setting is set too**No**, only admin users can register apps.</span></span> <span data-ttu-id="99e56-128">Du måste toocheck om ditt konto är administratör för hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="99e56-128">You need toocheck whether your account is an admin for hello Azure AD tenant.</span></span> <span data-ttu-id="99e56-129">Välj **översikt** och **söka efter en användare** från snabb uppgifter.</span><span class="sxs-lookup"><span data-stu-id="99e56-129">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![Sök efter användare](./media/resource-group-create-service-principal-portal/find-user.png)
6. <span data-ttu-id="99e56-131">Sök efter ditt konto och markera den när du har hittat.</span><span class="sxs-lookup"><span data-stu-id="99e56-131">Search for your account, and select it when you find it.</span></span>

     ![Sök användare](./media/resource-group-create-service-principal-portal/show-user.png)
7. <span data-ttu-id="99e56-133">Ditt konto, Välj **Directory rollen**.</span><span class="sxs-lookup"><span data-stu-id="99e56-133">For your account, select **Directory role**.</span></span> 

     ![Directory roll](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. <span data-ttu-id="99e56-135">Visa tilldelade directory-rollen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99e56-135">View your assigned directory role in Azure AD.</span></span> <span data-ttu-id="99e56-136">Om ditt konto har tilldelats användarrollen toohello, men hello registrering appinställning (från föregående steg hello) är begränsad tooadmin användare, kan du be din administratör tooeither tilldela tooan administratörsroll eller tooenable användare tooregister appar.</span><span class="sxs-lookup"><span data-stu-id="99e56-136">If your account is assigned toohello User role, but hello app registration setting (from hello preceding steps) is limited tooadmin users, ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

     ![Visa roll](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a><span data-ttu-id="99e56-138">Kontrollera behörigheter för Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="99e56-138">Check Azure subscription permissions</span></span>
<span data-ttu-id="99e56-139">Kontot måste ha i din Azure-prenumeration `Microsoft.Authorization/*/Write` komma åt tooassign en AD app tooa roll.</span><span class="sxs-lookup"><span data-stu-id="99e56-139">In your Azure subscription, your account must have `Microsoft.Authorization/*/Write` access tooassign an AD app tooa role.</span></span> <span data-ttu-id="99e56-140">Den här åtgärden beviljas genom hello [ägare](../active-directory/role-based-access-built-in-roles.md#owner) roll eller [administratör för användaråtkomst](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) roll.</span><span class="sxs-lookup"><span data-stu-id="99e56-140">This action is granted through hello [Owner](../active-directory/role-based-access-built-in-roles.md#owner) role or [User Access Administrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span></span> <span data-ttu-id="99e56-141">Om ditt konto har tilldelats toohello **deltagare** roll, du har inte tillräcklig behörighet.</span><span class="sxs-lookup"><span data-stu-id="99e56-141">If your account is assigned toohello **Contributor** role, you do not have adequate permission.</span></span> <span data-ttu-id="99e56-142">Du får ett fel vid försök tooassign hello service principal tooa roll.</span><span class="sxs-lookup"><span data-stu-id="99e56-142">You will receive an error when attempting tooassign hello service principal tooa role.</span></span> 

<span data-ttu-id="99e56-143">toocheck din Prenumerationsbehörighet:</span><span class="sxs-lookup"><span data-stu-id="99e56-143">toocheck your subscription permissions:</span></span>

1. <span data-ttu-id="99e56-144">Om du inte redan tittar på Azure AD-kontot från hello föregående steg, väljer **Azure Active Directory** från hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="99e56-144">If you are not already looking at your Azure AD account from hello preceding steps, select **Azure Active Directory** from hello left pane.</span></span>

2. <span data-ttu-id="99e56-145">Hitta din Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="99e56-145">Find your Azure AD account.</span></span> <span data-ttu-id="99e56-146">Välj **översikt** och **söka efter en användare** från snabb uppgifter.</span><span class="sxs-lookup"><span data-stu-id="99e56-146">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![Sök efter användare](./media/resource-group-create-service-principal-portal/find-user.png)
2. <span data-ttu-id="99e56-148">Sök efter ditt konto och markera den när du har hittat.</span><span class="sxs-lookup"><span data-stu-id="99e56-148">Search for your account, and select it when you find it.</span></span>

     ![Sök användare](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. <span data-ttu-id="99e56-150">Välj **Azure-resurser**.</span><span class="sxs-lookup"><span data-stu-id="99e56-150">Select **Azure resources**.</span></span>

     ![Välj resurser](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. <span data-ttu-id="99e56-152">Visa tilldelade rollerna och kontrollera om du har tillräcklig behörighet tooassign en AD app tooa roll.</span><span class="sxs-lookup"><span data-stu-id="99e56-152">View your assigned roles, and determine if you have adequate permissions tooassign an AD app tooa role.</span></span> <span data-ttu-id="99e56-153">Om inte, ber du din prenumeration administratören tooadd du tooUser åtkomst rollen Administratör.</span><span class="sxs-lookup"><span data-stu-id="99e56-153">If not, ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span> <span data-ttu-id="99e56-154">I följande bild hello, är hello användare tilldelade toohello ägarrollen för två prenumerationer, vilket innebär att användaren har tillräcklig behörighet.</span><span class="sxs-lookup"><span data-stu-id="99e56-154">In hello following image, hello user is assigned toohello Owner role for two subscriptions, which means that user has adequate permissions.</span></span> 

     ![Visa behörighet](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a><span data-ttu-id="99e56-156">Skapa ett Azure Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="99e56-156">Create an Azure Active Directory application</span></span>
1. <span data-ttu-id="99e56-157">Logga in tooyour Azure-konto via hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="99e56-157">Log in tooyour Azure Account through hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="99e56-158">Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="99e56-158">Select **Azure Active Directory**.</span></span>

     ![Välj azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. <span data-ttu-id="99e56-160">Välj **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="99e56-160">Select **App registrations**.</span></span>   

     ![Välj app-registreringar](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. <span data-ttu-id="99e56-162">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="99e56-162">Select **Add**.</span></span>

     ![Lägg till app](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. <span data-ttu-id="99e56-164">Ange ett namn och URL: en för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="99e56-164">Provide a name and URL for hello application.</span></span> <span data-ttu-id="99e56-165">Välj antingen **webbapp / API** eller **interna** hello typen av program som du vill ha toocreate.</span><span class="sxs-lookup"><span data-stu-id="99e56-165">Select either **Web app / API** or **Native** for hello type of application you want toocreate.</span></span> <span data-ttu-id="99e56-166">När du har angett hello värden, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="99e56-166">After setting hello values, select **Create**.</span></span>

     ![namn på program](./media/resource-group-create-service-principal-portal/create-app.png)

<span data-ttu-id="99e56-168">Du har skapat ditt program.</span><span class="sxs-lookup"><span data-stu-id="99e56-168">You have created your application.</span></span>

## <a name="get-application-id-and-authentication-key"></a><span data-ttu-id="99e56-169">Hämta program-ID och autentisering nyckel</span><span class="sxs-lookup"><span data-stu-id="99e56-169">Get application ID and authentication key</span></span>
<span data-ttu-id="99e56-170">När du programmässigt loggar in, behöver du hello-ID för ditt program och en autentiseringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="99e56-170">When programmatically logging in, you need hello ID for your application and an authentication key.</span></span> <span data-ttu-id="99e56-171">tooget dessa värden används hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="99e56-171">tooget those values, use hello following steps:</span></span>

1. <span data-ttu-id="99e56-172">Från **App registreringar** i Azure Active Directory, väljer du ditt program.</span><span class="sxs-lookup"><span data-stu-id="99e56-172">From **App registrations** in Azure Active Directory, select your application.</span></span>

     ![Välj program](./media/resource-group-create-service-principal-portal/select-app.png)
2. <span data-ttu-id="99e56-174">Kopiera hello **program-ID** och lagra den i din programkod.</span><span class="sxs-lookup"><span data-stu-id="99e56-174">Copy hello **Application ID** and store it in your application code.</span></span> <span data-ttu-id="99e56-175">Hej program i hello [programexempel](#sample-applications) avsnittet finns toothis värde som hello klient-id.</span><span class="sxs-lookup"><span data-stu-id="99e56-175">hello applications in hello [sample applications](#sample-applications) section refer toothis value as hello client id.</span></span>

     ![klient-id](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. <span data-ttu-id="99e56-177">Välj toogenerate en autentiseringsnyckel **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="99e56-177">toogenerate an authentication key, select **Keys**.</span></span>

     ![Välj nycklar](./media/resource-group-create-service-principal-portal/select-keys.png)
4. <span data-ttu-id="99e56-179">Ange en beskrivning av hello nyckel och en varaktighet för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="99e56-179">Provide a description of hello key, and a duration for hello key.</span></span> <span data-ttu-id="99e56-180">När du är klar väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="99e56-180">When done, select **Save**.</span></span>

     ![Spara nyckel](./media/resource-group-create-service-principal-portal/save-key.png)

     <span data-ttu-id="99e56-182">När du har sparat hello nyckeln visas hello värdet för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="99e56-182">After saving hello key, hello value of hello key is displayed.</span></span> <span data-ttu-id="99e56-183">Kopiera det här värdet eftersom du inte kan tooretrieve hello nyckeln senare.</span><span class="sxs-lookup"><span data-stu-id="99e56-183">Copy this value because you are not able tooretrieve hello key later.</span></span> <span data-ttu-id="99e56-184">Du kan ange hello nyckel/värde med hello program-ID toolog i som hello program.</span><span class="sxs-lookup"><span data-stu-id="99e56-184">You provide hello key value with hello application ID toolog in as hello application.</span></span> <span data-ttu-id="99e56-185">Lagra hello nyckelvärdet där programmet kan hämta.</span><span class="sxs-lookup"><span data-stu-id="99e56-185">Store hello key value where your application can retrieve it.</span></span>

     ![Spara nyckel](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a><span data-ttu-id="99e56-187">Hämta klient-ID</span><span class="sxs-lookup"><span data-stu-id="99e56-187">Get tenant ID</span></span>
<span data-ttu-id="99e56-188">När programmässigt inloggningen måste toopass hello klient-ID med din autentiseringsbegäran.</span><span class="sxs-lookup"><span data-stu-id="99e56-188">When programmatically logging in, you need toopass hello tenant ID with your authentication request.</span></span> 

1. <span data-ttu-id="99e56-189">tooget hello klient-ID, Välj **egenskaper** för din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="99e56-189">tooget hello tenant ID, select **Properties** for your Azure AD tenant.</span></span> 

     ![Välj Azure AD-egenskaper](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. <span data-ttu-id="99e56-191">Kopiera hello **katalog-ID**.</span><span class="sxs-lookup"><span data-stu-id="99e56-191">Copy hello **Directory ID**.</span></span> <span data-ttu-id="99e56-192">Det här värdet är klient-ID.</span><span class="sxs-lookup"><span data-stu-id="99e56-192">This value is your tenant ID.</span></span>

     ![klient-id](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-toorole"></a><span data-ttu-id="99e56-194">Tilldela programmet toorole</span><span class="sxs-lookup"><span data-stu-id="99e56-194">Assign application toorole</span></span>
<span data-ttu-id="99e56-195">tooaccess resurser i din prenumeration, måste du tilldela hello tooa programrollen.</span><span class="sxs-lookup"><span data-stu-id="99e56-195">tooaccess resources in your subscription, you must assign hello application tooa role.</span></span> <span data-ttu-id="99e56-196">Bestäm vilken roll representerar hello rätt behörigheter för hello program.</span><span class="sxs-lookup"><span data-stu-id="99e56-196">Decide which role represents hello right permissions for hello application.</span></span> <span data-ttu-id="99e56-197">toolearn om hello tillgängliga roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="99e56-197">toolearn about hello available roles, see [RBAC: Built in Roles](../active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="99e56-198">Du kan ange hello scope på hello nivå hello prenumerationen, resursgruppen eller resursen.</span><span class="sxs-lookup"><span data-stu-id="99e56-198">You can set hello scope at hello level of hello subscription, resource group, or resource.</span></span> <span data-ttu-id="99e56-199">Behörigheter som är ärvda toolower nivåer av omfång.</span><span class="sxs-lookup"><span data-stu-id="99e56-199">Permissions are inherited toolower levels of scope.</span></span> <span data-ttu-id="99e56-200">Till exempel kan att lägga till en programmet toohello Reader roll för en resursgrupp innebär att den läsa hello resursgruppen och alla resurser som den innehåller.</span><span class="sxs-lookup"><span data-stu-id="99e56-200">For example, adding an application toohello Reader role for a resource group means it can read hello resource group and any resources it contains.</span></span>

1. <span data-ttu-id="99e56-201">Navigera toohello nivå av omfång som du vill tooassign hello programmet till.</span><span class="sxs-lookup"><span data-stu-id="99e56-201">Navigate toohello level of scope you wish tooassign hello application to.</span></span> <span data-ttu-id="99e56-202">Välj till exempel tooassign en roll i hello prenumerationsomfattningen **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="99e56-202">For example, tooassign a role at hello subscription scope, select **Subscriptions**.</span></span> <span data-ttu-id="99e56-203">Du kan i stället välja en resursgrupp eller resurs.</span><span class="sxs-lookup"><span data-stu-id="99e56-203">You could instead select a resource group or resource.</span></span>

     ![Välj prenumeration](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. <span data-ttu-id="99e56-205">Välj hello viss prenumeration (resursgrupp eller resurs) tooassign hello.</span><span class="sxs-lookup"><span data-stu-id="99e56-205">Select hello particular subscription (resource group or resource) tooassign hello application to.</span></span>

     ![Välj prenumerationen för tilldelning](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. <span data-ttu-id="99e56-207">Välj **åtkomstkontroll (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="99e56-207">Select **Access Control (IAM)**.</span></span>

     ![Välj åtkomst](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. <span data-ttu-id="99e56-209">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="99e56-209">Select **Add**.</span></span>

     ![Välj Lägg till](./media/resource-group-create-service-principal-portal/select-add.png)
6. <span data-ttu-id="99e56-211">Välj hello-roll som du vill tooassign toohello program.</span><span class="sxs-lookup"><span data-stu-id="99e56-211">Select hello role you wish tooassign toohello application.</span></span> <span data-ttu-id="99e56-212">hello följande bild visar hello **Reader** roll.</span><span class="sxs-lookup"><span data-stu-id="99e56-212">hello following image shows hello **Reader** role.</span></span>

     ![Välj rollen](./media/resource-group-create-service-principal-portal/select-role.png)

8. <span data-ttu-id="99e56-214">Sök efter programmet och markera den.</span><span class="sxs-lookup"><span data-stu-id="99e56-214">Search for your application, and select it.</span></span>

     ![Sök efter app](./media/resource-group-create-service-principal-portal/search-app.png)
9. <span data-ttu-id="99e56-216">Välj **OK** toofinish tilldela hello roll.</span><span class="sxs-lookup"><span data-stu-id="99e56-216">Select **OK** toofinish assigning hello role.</span></span> <span data-ttu-id="99e56-217">Du ser ditt program i hello lista över användare som tilldelats tooa roll för detta omfång.</span><span class="sxs-lookup"><span data-stu-id="99e56-217">You see your application in hello list of users assigned tooa role for that scope.</span></span>

## <a name="log-in-as-hello-application"></a><span data-ttu-id="99e56-218">Logga in som hello program</span><span class="sxs-lookup"><span data-stu-id="99e56-218">Log in as hello application</span></span>

<span data-ttu-id="99e56-219">Programmet har konfigurerats i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="99e56-219">Your application is now set up in Azure Active Directory.</span></span> <span data-ttu-id="99e56-220">Du har ett ID och nyckel toouse för att logga in som hello program.</span><span class="sxs-lookup"><span data-stu-id="99e56-220">You have an ID and key toouse for signing in as hello application.</span></span> <span data-ttu-id="99e56-221">hello programmet tilldelas tooa roll som ger den vissa åtgärder kan utföras.</span><span class="sxs-lookup"><span data-stu-id="99e56-221">hello application is assigned tooa role that gives it certain actions it can perform.</span></span> <span data-ttu-id="99e56-222">För information om att logga in som hello program via olika plattformar, se:</span><span class="sxs-lookup"><span data-stu-id="99e56-222">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="99e56-223">PowerShell</span><span class="sxs-lookup"><span data-stu-id="99e56-223">PowerShell</span></span>](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [<span data-ttu-id="99e56-224">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="99e56-224">Azure CLI</span></span>](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [<span data-ttu-id="99e56-225">REST</span><span class="sxs-lookup"><span data-stu-id="99e56-225">REST</span></span>](/rest/api/#create-the-request)
* [<span data-ttu-id="99e56-226">.NET</span><span class="sxs-lookup"><span data-stu-id="99e56-226">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="99e56-227">Java</span><span class="sxs-lookup"><span data-stu-id="99e56-227">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="99e56-228">Node.js</span><span class="sxs-lookup"><span data-stu-id="99e56-228">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="99e56-229">Python</span><span class="sxs-lookup"><span data-stu-id="99e56-229">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="99e56-230">Ruby</span><span class="sxs-lookup"><span data-stu-id="99e56-230">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a><span data-ttu-id="99e56-231">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99e56-231">Next steps</span></span>
* <span data-ttu-id="99e56-232">tooset upp ett program med flera innehavare, se [Developer's guide tooauthorization med hello Azure Resource Manager API](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="99e56-232">tooset up a multi-tenant application, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="99e56-233">toolearn om hur du anger IPSec-principer finns [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="99e56-233">toolearn about specifying security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>  
* <span data-ttu-id="99e56-234">En lista över tillgängliga åtgärder som kan beviljas eller nekas toousers finns [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="99e56-234">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
