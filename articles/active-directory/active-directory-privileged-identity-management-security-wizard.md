---
title: "aaaThe Azure AD Privileged Identity Management-säkerhetsguiden"
description: "hello visas första gången du använder hello Azure Active Directory Privileged Identity Management-tillägg, med en säkerhetsguiden. Den här artikeln beskriver hello steg för att använda hello guiden."
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
ms.openlocfilehash: 0b3019134d3c7cfac33b3acfcf430b4d4f67b119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-security-wizard-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="433e7-104">Med hjälp av guiden för hello säkerhet i Azure AD Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="433e7-104">Using hello security wizard in Azure AD Privileged Identity Management</span></span> 
<span data-ttu-id="433e7-105">Om du är hello första person toorun Azure Privileged Identity Management (PIM) för din organisation, visas en guide.</span><span class="sxs-lookup"><span data-stu-id="433e7-105">If you're hello first person toorun Azure Privileged Identity Management (PIM) for your organization, you will be presented with a wizard.</span></span> <span data-ttu-id="433e7-106">hello-guiden hjälper dig att förstå hello säkerhetsriskerna med Privilegierade identiteter och hur toouse PIM tooreduce riskerna.</span><span class="sxs-lookup"><span data-stu-id="433e7-106">hello wizard helps you understand hello security risks of privileged identities and how toouse PIM tooreduce those risks.</span></span> <span data-ttu-id="433e7-107">Du behöver inte toomake alla ändringar tooexisting rolltilldelningar hello i guiden om du föredrar toodo senare.</span><span class="sxs-lookup"><span data-stu-id="433e7-107">You don't need toomake any changes tooexisting role assignments in hello wizard, if you prefer toodo it later.</span></span>

## <a name="what-tooexpect"></a><span data-ttu-id="433e7-108">Vilka tooexpect</span><span class="sxs-lookup"><span data-stu-id="433e7-108">What tooexpect</span></span>
<span data-ttu-id="433e7-109">Innan din organisation kan börja använda PIM, alla rolltilldelningar är permanenta: hello användare är alltid i dessa roller även om de inte för närvarande behöver sina privilegier.</span><span class="sxs-lookup"><span data-stu-id="433e7-109">Before your organization starts using PIM, all role assignments are permanent: hello users are always in these roles even if they do not presently need their privileges.</span></span>  <span data-ttu-id="433e7-110">hello första steget hello guiden visar en lista över privilegierad roller och hur många användare som för närvarande rollerna.</span><span class="sxs-lookup"><span data-stu-id="433e7-110">hello first step of hello wizard shows you a list of high-privileged roles and how many users are currently in those roles.</span></span> <span data-ttu-id="433e7-111">Du kan gå i tooa viss roll toolearn mer information om användare om en eller flera av dem är okänd.</span><span class="sxs-lookup"><span data-stu-id="433e7-111">You can drill in tooa particular role toolearn more about users if one or more of them are unfamiliar.</span></span>

<span data-ttu-id="433e7-112">hello andra steget hello guiden ger dig en möjlighet toochange administratör rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="433e7-112">hello second step of hello wizard gives you an opportunity toochange administrator's role assignments.</span></span>  

> [!WARNING]
> <span data-ttu-id="433e7-113">Det är viktigt att du har minst en global administratör och mer än en administratör av Privilegierade roller med ett organisationskonto (inte ett Microsoft-konto).</span><span class="sxs-lookup"><span data-stu-id="433e7-113">It is important that you have at least one global administrator, and more than one privileged role administrator with an organizational account (not a Microsoft account).</span></span> <span data-ttu-id="433e7-114">Om det finns bara en administratör av Privilegierade roller, tas inte hello organisation kan toomanage PIM om att kontot har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="433e7-114">If there is only one privileged role administrator, hello organization will not be able toomanage PIM if that account is deleted.</span></span>
> <span data-ttu-id="433e7-115">Kom också rolltilldelningar permanent om en användare har ett Microsoft-konto (ett konto som de använder toosign i tooMicrosoft tjänster som Skype och Outlook.com).</span><span class="sxs-lookup"><span data-stu-id="433e7-115">Also, keep role assignments permanent if a user has a Microsoft account (An account they use toosign in tooMicrosoft services like Skype and Outlook.com).</span></span> <span data-ttu-id="433e7-116">Om du planerar toorequire MFA för aktivering för rollen, kommer den användaren utelåst.</span><span class="sxs-lookup"><span data-stu-id="433e7-116">If you plan toorequire MFA for activation for that role, that user will be locked out.</span></span>
> 
> 

<span data-ttu-id="433e7-117">När du har gjort ändringar hello guiden kommer inte längre att visas.</span><span class="sxs-lookup"><span data-stu-id="433e7-117">After you have made changes, hello wizard will no longer show up.</span></span> <span data-ttu-id="433e7-118">hello visas nästa gång du eller en annan administratör av Privilegierade roller använder PIM, hello PIM-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="433e7-118">hello next time you or another privileged role administrator use PIM, you will see hello PIM dashboard.</span></span>  

* <span data-ttu-id="433e7-119">Om du skulle t.ex. tooadd eller ta bort användare från roller eller ändra tilldelningar från permanent tooeligible, kan du läsa mer i [hur tooadd eller ta bort en användarroll](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span><span class="sxs-lookup"><span data-stu-id="433e7-119">If you would like tooadd or remove users from roles or change assignments from permanent tooeligible, read more at [how tooadd or remove a user's role](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span></span>
* <span data-ttu-id="433e7-120">Om du vill att toogive fler användare komma åt toomanage PIM, Läs mer på [hur toogive åt toomanage i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="433e7-120">If you would like toogive more users access toomanage PIM, read more at [how toogive access toomanage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="433e7-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="433e7-121">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

