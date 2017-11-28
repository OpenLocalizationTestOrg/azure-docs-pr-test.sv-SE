---
title: "Azure AD Privileged Identity Management-säkerhetsguiden"
description: "Första gången du använder Azure Active Directory Privileged Identity Management-tillägg visas med en säkerhetsguiden. Den här artikeln beskriver stegen för att med hjälp av guiden."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 260d178f3d8158411b3ad266e3b0d15edbebc722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="c628b-104">Guiden Säkerhet i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="c628b-104">Using the security wizard in Azure AD Privileged Identity Management</span></span> 
<span data-ttu-id="c628b-105">Om du är den första personen att köra Azure Privileged Identity Management (PIM) för din organisation, visas en guide.</span><span class="sxs-lookup"><span data-stu-id="c628b-105">If you're the first person to run Azure Privileged Identity Management (PIM) for your organization, you will be presented with a wizard.</span></span> <span data-ttu-id="c628b-106">Guiden hjälper dig att förstå säkerhetsriskerna med Privilegierade identiteter och hur du använder PIM för att minska riskerna.</span><span class="sxs-lookup"><span data-stu-id="c628b-106">The wizard helps you understand the security risks of privileged identities and how to use PIM to reduce those risks.</span></span> <span data-ttu-id="c628b-107">Du behöver inte göra ändringar i befintliga rolltilldelningar i guiden om du vill göra det senare.</span><span class="sxs-lookup"><span data-stu-id="c628b-107">You don't need to make any changes to existing role assignments in the wizard, if you prefer to do it later.</span></span>

## <a name="what-to-expect"></a><span data-ttu-id="c628b-108">Vad du kan förvänta dig</span><span class="sxs-lookup"><span data-stu-id="c628b-108">What to expect</span></span>
<span data-ttu-id="c628b-109">Innan din organisation kan börja använda PIM, alla rolltilldelningar är permanenta: användarna är alltid i dessa roller även om de inte för närvarande behöver sina privilegier.</span><span class="sxs-lookup"><span data-stu-id="c628b-109">Before your organization starts using PIM, all role assignments are permanent: the users are always in these roles even if they do not presently need their privileges.</span></span>  <span data-ttu-id="c628b-110">Det första steget i guiden visar en lista över privilegierad roller och hur många användare som för närvarande rollerna.</span><span class="sxs-lookup"><span data-stu-id="c628b-110">The first step of the wizard shows you a list of high-privileged roles and how many users are currently in those roles.</span></span> <span data-ttu-id="c628b-111">Du kan öka detaljnivån en viss roll för mer information om användare om en eller flera av dem är okänd.</span><span class="sxs-lookup"><span data-stu-id="c628b-111">You can drill in to a particular role to learn more about users if one or more of them are unfamiliar.</span></span>

<span data-ttu-id="c628b-112">Det andra steget i guiden får du möjlighet att ändra administratörens rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="c628b-112">The second step of the wizard gives you an opportunity to change administrator's role assignments.</span></span>  

> [!WARNING]
> <span data-ttu-id="c628b-113">Det är viktigt att du har minst en global administratör och mer än en administratör av Privilegierade roller med ett organisationskonto (inte ett Microsoft-konto).</span><span class="sxs-lookup"><span data-stu-id="c628b-113">It is important that you have at least one global administrator, and more than one privileged role administrator with an organizational account (not a Microsoft account).</span></span> <span data-ttu-id="c628b-114">Om det finns bara en administratör av Privilegierade roller, organisationen inte kan hantera PIM om kontot tas bort.</span><span class="sxs-lookup"><span data-stu-id="c628b-114">If there is only one privileged role administrator, the organization will not be able to manage PIM if that account is deleted.</span></span>
> <span data-ttu-id="c628b-115">Kom också rolltilldelningar permanent om en användare har ett Microsoft-konto (ett konto som de använder för att logga in på Microsoft-tjänster som Skype och Outlook.com).</span><span class="sxs-lookup"><span data-stu-id="c628b-115">Also, keep role assignments permanent if a user has a Microsoft account (An account they use to sign in to Microsoft services like Skype and Outlook.com).</span></span> <span data-ttu-id="c628b-116">Om du planerar att kräva MFA för aktivering för rollen utelåst användaren.</span><span class="sxs-lookup"><span data-stu-id="c628b-116">If you plan to require MFA for activation for that role, that user will be locked out.</span></span>
> 
> 

<span data-ttu-id="c628b-117">När du har gjort ändringar i guiden kommer inte längre att visas.</span><span class="sxs-lookup"><span data-stu-id="c628b-117">After you have made changes, the wizard will no longer show up.</span></span> <span data-ttu-id="c628b-118">Nästa gång du eller en annan administratör av Privilegierade roller använder PIM, visas PIM-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c628b-118">The next time you or another privileged role administrator use PIM, you will see the PIM dashboard.</span></span>  

* <span data-ttu-id="c628b-119">Om du vill lägga till eller ta bort användare från roller eller ändra tilldelningar från permanent till berättigade Läs mer på [lägga till eller ta bort en användarroll](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span><span class="sxs-lookup"><span data-stu-id="c628b-119">If you would like to add or remove users from roles or change assignments from permanent to eligible, read more at [how to add or remove a user's role](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span></span>
* <span data-ttu-id="c628b-120">Om du vill ge flera användare åtkomst till hantera PIM Läs mer på [ge åtkomst för att hantera i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="c628b-120">If you would like to give more users access to manage PIM, read more at [how to give access to manage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c628b-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c628b-121">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

