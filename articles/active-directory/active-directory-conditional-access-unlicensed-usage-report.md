---
title: "Olicensierad användningsrapporten | Microsoft Docs"
description: "Olicensierad användning rapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: c0b4f455f067e825362bdecc02ea62d7984f0d31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="unlicensed-usage-report"></a><span data-ttu-id="12eac-103">Olicensierad användningsrapporten</span><span class="sxs-lookup"><span data-stu-id="12eac-103">Unlicensed usage report</span></span>
<span data-ttu-id="12eac-104">Olicensierad användning rapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner.</span><span class="sxs-lookup"><span data-stu-id="12eac-104">The unlicensed usage report helps you identify unlicensed users that are using paid Azure AD features.</span></span> <span data-ttu-id="12eac-105">Detta gör att du kan göra bättre användning av licenser som du har köpt och för att identifiera du vet när du eventuellt ytterligare licenser.</span><span class="sxs-lookup"><span data-stu-id="12eac-105">This allows you to make better use of licenses that you have purchased and to identify you know when you may need additional licenses.</span></span> 

<span data-ttu-id="12eac-106">Rapporten visar aktiva användning av funktionerna för betalda under de senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="12eac-106">The report shows active usage of the paid features in the last 30 days.</span></span> 

## <a name="report-structure"></a><span data-ttu-id="12eac-107">Rapporten struktur</span><span class="sxs-lookup"><span data-stu-id="12eac-107">Report structure</span></span>
| <span data-ttu-id="12eac-108">Kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="12eac-108">Column name</span></span> | <span data-ttu-id="12eac-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="12eac-109">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="12eac-110">Olicensierad användare</span><span class="sxs-lookup"><span data-stu-id="12eac-110">Unlicensed User</span></span> |<span data-ttu-id="12eac-111">Användarens namn</span><span class="sxs-lookup"><span data-stu-id="12eac-111">Name of the user</span></span> |
| <span data-ttu-id="12eac-112">Funktion</span><span class="sxs-lookup"><span data-stu-id="12eac-112">Feature</span></span> |<span data-ttu-id="12eac-113">Funktionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="12eac-113">The feature name.</span></span> <span data-ttu-id="12eac-114">Till exempel: villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="12eac-114">For example: conditional access</span></span> |
| <span data-ttu-id="12eac-115">Program som nås</span><span class="sxs-lookup"><span data-stu-id="12eac-115">Application Accessed</span></span> |<span data-ttu-id="12eac-116">Namnet på det program som nås med funktionen.</span><span class="sxs-lookup"><span data-stu-id="12eac-116">The name of the application that is being accessed with the feature.</span></span> <span data-ttu-id="12eac-117">Till exempel: Office 365 SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="12eac-117">For example: Office 365 SharePoint Online</span></span> |

> [!NOTE]
> <span data-ttu-id="12eac-118">Om ett användarkonto har tagits bort kolumnen 'Olicensierad User' fylls i med ett ID som 1003000090D8B285</span><span class="sxs-lookup"><span data-stu-id="12eac-118">If a user account has been deleted the ‘Unlicensed User’ column will be populated with an ID, like 1003000090D8B285</span></span>
> 
> 

## <a name="conditional-access-feature"></a><span data-ttu-id="12eac-119">Funktionen för villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="12eac-119">Conditional access feature</span></span>
<span data-ttu-id="12eac-120">Ej licensierade användare flaggas när de har åtkomst till en tjänst som har principen för villkorlig åtkomst tillämpas om de inte har en Azure AD Premium-licens.</span><span class="sxs-lookup"><span data-stu-id="12eac-120">Unlicensed users will be flagged when they access a service that has conditional access policy applied if they do not have an Azure AD Premium license.</span></span> 

<span data-ttu-id="12eac-121">Detta gäller för MFA / plats principer samt enheten principer som använder Intune.</span><span class="sxs-lookup"><span data-stu-id="12eac-121">This applies to MFA / Location policies as well as device polices that use Intune.</span></span>

## <a name="see-also"></a><span data-ttu-id="12eac-122">Se även</span><span class="sxs-lookup"><span data-stu-id="12eac-122">See also</span></span>
* [<span data-ttu-id="12eac-123">Använda villkorlig åtkomst med Office 365 och andra Azure Active Directory anslutna appar</span><span class="sxs-lookup"><span data-stu-id="12eac-123">Using Conditional Access with Office 365 and other Azure Active Directory connected apps</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="12eac-124">Komma igång med villkorlig åtkomst till Azure AD</span><span class="sxs-lookup"><span data-stu-id="12eac-124">Getting started with conditional access to Azure AD</span></span>](active-directory-conditional-access-azuread-connected-apps.md) 

