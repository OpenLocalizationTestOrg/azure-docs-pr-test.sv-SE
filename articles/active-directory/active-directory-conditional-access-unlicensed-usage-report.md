---
title: "aaaUnlicensed användningsrapporten | Microsoft Docs"
description: "Hej olicensierad användningsrapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner."
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
ms.openlocfilehash: c44d1756b4641d7ca88434017eedb6c5e2567cb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unlicensed-usage-report"></a><span data-ttu-id="8e6cd-103">Olicensierad användningsrapporten</span><span class="sxs-lookup"><span data-stu-id="8e6cd-103">Unlicensed usage report</span></span>
<span data-ttu-id="8e6cd-104">Hej olicensierad användningsrapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-104">hello unlicensed usage report helps you identify unlicensed users that are using paid Azure AD features.</span></span> <span data-ttu-id="8e6cd-105">Det innebär att du toomake bättre användning av licenser som du har köpt och tooidentify som du vet när du eventuellt ytterligare licenser.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-105">This allows you toomake better use of licenses that you have purchased and tooidentify you know when you may need additional licenses.</span></span> 

<span data-ttu-id="8e6cd-106">hello rapporten visar aktiva användning av hello betald funktioner i hello senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-106">hello report shows active usage of hello paid features in hello last 30 days.</span></span> 

## <a name="report-structure"></a><span data-ttu-id="8e6cd-107">Rapporten struktur</span><span class="sxs-lookup"><span data-stu-id="8e6cd-107">Report structure</span></span>
| <span data-ttu-id="8e6cd-108">Kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="8e6cd-108">Column name</span></span> | <span data-ttu-id="8e6cd-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e6cd-109">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e6cd-110">Olicensierad användare</span><span class="sxs-lookup"><span data-stu-id="8e6cd-110">Unlicensed User</span></span> |<span data-ttu-id="8e6cd-111">Namnet på hello användare</span><span class="sxs-lookup"><span data-stu-id="8e6cd-111">Name of hello user</span></span> |
| <span data-ttu-id="8e6cd-112">Funktion</span><span class="sxs-lookup"><span data-stu-id="8e6cd-112">Feature</span></span> |<span data-ttu-id="8e6cd-113">hello funktionsnamnet.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-113">hello feature name.</span></span> <span data-ttu-id="8e6cd-114">Till exempel: villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="8e6cd-114">For example: conditional access</span></span> |
| <span data-ttu-id="8e6cd-115">Program som nås</span><span class="sxs-lookup"><span data-stu-id="8e6cd-115">Application Accessed</span></span> |<span data-ttu-id="8e6cd-116">hello namnet på hello-program som nås med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-116">hello name of hello application that is being accessed with hello feature.</span></span> <span data-ttu-id="8e6cd-117">Till exempel: Office 365 SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="8e6cd-117">For example: Office 365 SharePoint Online</span></span> |

> [!NOTE]
> <span data-ttu-id="8e6cd-118">Om ett användarkonto har tagits bort hello 'Olicensierad User' kolumnen fylls i med ett ID som 1003000090D8B285</span><span class="sxs-lookup"><span data-stu-id="8e6cd-118">If a user account has been deleted hello ‘Unlicensed User’ column will be populated with an ID, like 1003000090D8B285</span></span>
> 
> 

## <a name="conditional-access-feature"></a><span data-ttu-id="8e6cd-119">Funktionen för villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="8e6cd-119">Conditional access feature</span></span>
<span data-ttu-id="8e6cd-120">Ej licensierade användare flaggas när de har åtkomst till en tjänst som har principen för villkorlig åtkomst tillämpas om de inte har en Azure AD Premium-licens.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-120">Unlicensed users will be flagged when they access a service that has conditional access policy applied if they do not have an Azure AD Premium license.</span></span> 

<span data-ttu-id="8e6cd-121">Detta gäller tooMFA / plats principer samt enheten principer som använder Intune.</span><span class="sxs-lookup"><span data-stu-id="8e6cd-121">This applies tooMFA / Location policies as well as device polices that use Intune.</span></span>

## <a name="see-also"></a><span data-ttu-id="8e6cd-122">Se även</span><span class="sxs-lookup"><span data-stu-id="8e6cd-122">See also</span></span>
* [<span data-ttu-id="8e6cd-123">Använda villkorlig åtkomst med Office 365 och andra Azure Active Directory anslutna appar</span><span class="sxs-lookup"><span data-stu-id="8e6cd-123">Using Conditional Access with Office 365 and other Azure Active Directory connected apps</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="8e6cd-124">Komma igång med villkorlig åtkomst tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="8e6cd-124">Getting started with conditional access tooAzure AD</span></span>](active-directory-conditional-access-azuread-connected-apps.md) 

