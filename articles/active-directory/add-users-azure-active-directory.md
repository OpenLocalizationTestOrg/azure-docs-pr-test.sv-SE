---
title: "aaaAdd nya användare tooAzure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd nya användare i Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 6ca413c84a7a5238a30fd26fc751d687d827b24a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-new-users-tooazure-active-directory"></a><span data-ttu-id="262df-103">Snabbstart: Lägga till nya användare tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="262df-103">Quickstart: Add new users tooAzure Active Directory</span></span>
<span data-ttu-id="262df-104">Den här artikeln förklarar hur tooadd nya användare i din organisation i hello Azure Active Directory (AD Azure) en i taget med hello Azure-portalen eller genom att synkronisera lokala Windows Server AD-kontot data.</span><span class="sxs-lookup"><span data-stu-id="262df-104">This article explains how tooadd new users in your organization in hello Azure Active Directory (Azure AD) one at a time using hello Azure portal or by synchronizing your on-premises Windows Server AD user account data.</span></span> 

## <a name="add-cloud-based-users"></a><span data-ttu-id="262df-105">Lägga till molnbaserade användare</span><span class="sxs-lookup"><span data-stu-id="262df-105">Add cloud-based users</span></span>
1. <span data-ttu-id="262df-106">Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="262df-106">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="262df-107">Välj **Azure Active Directory** och sedan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="262df-107">Select **Azure Active Directory** and then **Users and groups**.</span></span>
3. <span data-ttu-id="262df-108">På hello **användare och grupper** bladet väljer **alla användare**, och välj sedan **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="262df-108">On hello **Users and groups** blade, select **All users**, and then select **New user**.</span></span>
   <span data-ttu-id="262df-109">![Att välja hello tilläggskommando](./media/add-users-azure-active-directory/add-user.png)</span><span class="sxs-lookup"><span data-stu-id="262df-109">![Selecting hello Add command](./media/add-users-azure-active-directory/add-user.png)</span></span>
4. <span data-ttu-id="262df-110">Ange information om hello användaren som **namn** och **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="262df-110">Enter details for hello user, such as **Name** and **User name**.</span></span> <span data-ttu-id="262df-111">Hej domännamndelen för hello användarnamn måste antingen vara hello inledande standard domain name ”[namn].onmicrosoft.com” eller en verifierad ofedererad [domännamn](add-custom-domain.md) , till exempel ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="262df-111">hello domain name portion of hello user name must either be hello initial default domain name "[domain name].onmicrosoft.com" or a verified, non-federated [custom domain name](add-custom-domain.md) such as "contoso.com."</span></span>
5. <span data-ttu-id="262df-112">Kopiera eller på annat sätt Obs hello genereras användarlösenord så att du kan ange den toohello användaren när processen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="262df-112">Copy or otherwise note hello generated user password so that you can provide it toohello user after this process is complete.</span></span>
6. <span data-ttu-id="262df-113">Du kan också öppna och fylla i hello information i hello **profil** bladet, hello **grupper** bladet eller hello **Directory rollen** bladet för hello användare.</span><span class="sxs-lookup"><span data-stu-id="262df-113">Optionally, you can open and fill out hello information in hello **Profile** blade, hello **Groups** blade, or hello **Directory role** blade for hello user.</span></span> <span data-ttu-id="262df-114">Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="262df-114">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>
7. <span data-ttu-id="262df-115">På hello **användare** bladet väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="262df-115">On hello **User** blade, select **Create**.</span></span>
8. <span data-ttu-id="262df-116">Distribuera hello genererade lösenordet toohello ny användare på ett säkert sätt så att hello användare kan logga in.</span><span class="sxs-lookup"><span data-stu-id="262df-116">Securely distribute hello generated password toohello new user so that hello user can sign in.</span></span>

> [!TIP]
> <span data-ttu-id="262df-117">Du kan också synkronisera konto användardata från lokala Windows Server AD.</span><span class="sxs-lookup"><span data-stu-id="262df-117">You can also synchronize user account data from on-premises Windows Server AD.</span></span> <span data-ttu-id="262df-118">Microsofts identitetslösningar omfattar lokala och molnbaserade funktioner, och skapa en enda användaridentitet för autentisering och auktorisering tooall resurser, oavsett plats.</span><span class="sxs-lookup"><span data-stu-id="262df-118">Microsoft’s identity solutions span on-premises and cloud-based capabilities, creating a single user identity for authentication and authorization tooall resources, regardless of location.</span></span> <span data-ttu-id="262df-119">Vi kallar detta Hybrididentitet.</span><span class="sxs-lookup"><span data-stu-id="262df-119">We call this Hybrid Identity.</span></span> <span data-ttu-id="262df-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) kan vara används toointegrate dina lokala kataloger med Azure Active Directory hybrid identity scenarier.</span><span class="sxs-lookup"><span data-stu-id="262df-120">[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) can be used toointegrate your on-premises directories with Azure Active Directory for hybrid identity scenarios.</span></span> <span data-ttu-id="262df-121">Detta gör att du tooprovide en gemensam identitet för dina användare för Office 365 och Azure SaaS-program som är integrerade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="262df-121">This allows you tooprovide a common identity for your users for Office 365, Azure, and SaaS applications integrated with Azure AD.</span></span> 

## <a name="delete-users-from-azure-ad"></a><span data-ttu-id="262df-122">Ta bort användare från Azure AD</span><span class="sxs-lookup"><span data-stu-id="262df-122">Delete users from Azure AD</span></span>
1. <span data-ttu-id="262df-123">Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="262df-123">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="262df-124">Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="262df-124">Select **Users and groups**.</span></span>
3. <span data-ttu-id="262df-125">På hello **användare och grupper** bladet, Välj hello användaren toodelete hello-listan.</span><span class="sxs-lookup"><span data-stu-id="262df-125">On hello **Users and groups** blade, select hello user toodelete from hello list.</span></span> 
4. <span data-ttu-id="262df-126">På hello bladet för valda hello-användaren väljer **översikt**, och välj sedan i kommandofältet hello **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="262df-126">On hello blade for hello selected user, select **Overview**, and then in hello command bar, select **Delete**.</span></span>
   <span data-ttu-id="262df-127">![Att välja hello tilläggskommando](./media/add-users-azure-active-directory/delete-user.png)</span><span class="sxs-lookup"><span data-stu-id="262df-127">![Selecting hello Add command](./media/add-users-azure-active-directory/delete-user.png)</span></span>


### <a name="learn-more"></a><span data-ttu-id="262df-128">Läs mer</span><span class="sxs-lookup"><span data-stu-id="262df-128">Learn more</span></span> 
* [<span data-ttu-id="262df-129">Lägg till en extern användare</span><span class="sxs-lookup"><span data-stu-id="262df-129">Add an external user</span></span>](active-directory-users-create-external-azure-portal.md)

* [<span data-ttu-id="262df-130">Tilldela en användarroll tooa i din Azure AD</span><span class="sxs-lookup"><span data-stu-id="262df-130">Assign a user tooa role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a><span data-ttu-id="262df-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="262df-131">Next steps</span></span>
<span data-ttu-id="262df-132">I den här snabbstarten du har lärt dig hur tooadd nya användare tooAzure AD Premium.</span><span class="sxs-lookup"><span data-stu-id="262df-132">In this quickstart, you’ve learned how tooadd new users tooAzure AD Premium.</span></span> 

<span data-ttu-id="262df-133">Du kan använda hello följande länk toocreate en ny användare i Azure AD från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="262df-133">You can use hello following link toocreate a new user in Azure AD from hello Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="262df-134">Lägg till användare tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="262df-134">Add users tooAzure AD</span></span>](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
