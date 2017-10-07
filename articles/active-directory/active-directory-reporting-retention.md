---
title: "rapportkvarhållningsregler i aaaAzure Active Directory | Microsoft Docs"
description: "Bevarandeprinciper på rapportdata i Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c46a68198cb34e9c92662b2f8461010745392c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-retention-policies"></a><span data-ttu-id="3208c-103">Rapportkvarhållningsregler i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3208c-103">Azure Active Directory report retention policies</span></span>


<span data-ttu-id="3208c-104">Det här avsnittet ger dig svar toohello vanligaste frågorna tillsammans med hello datalagring för hello olika aktivitetsrapporter i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3208c-104">This topic provides you with answers toohello most common questions in conjunction with hello data retention for hello different activity reports in Azure Active Directory.</span></span> 

<span data-ttu-id="3208c-105">**F: hur kan du få hello samling aktivitetsdata igång?**</span><span class="sxs-lookup"><span data-stu-id="3208c-105">**Q: How can you get hello collection of activity data started?**</span></span>

<span data-ttu-id="3208c-106">**S:**</span><span class="sxs-lookup"><span data-stu-id="3208c-106">**A:**</span></span>

| <span data-ttu-id="3208c-107">Azure AD-version</span><span class="sxs-lookup"><span data-stu-id="3208c-107">Azure AD Edition</span></span> | <span data-ttu-id="3208c-108">Samlingen Start</span><span class="sxs-lookup"><span data-stu-id="3208c-108">Collection Start</span></span> |
| :--              | :--   |
| <span data-ttu-id="3208c-109">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="3208c-109">Azure AD Premium P1</span></span> <br /> <span data-ttu-id="3208c-110">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="3208c-110">Azure AD Premium P2</span></span> | <span data-ttu-id="3208c-111">När du registrerar dig för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="3208c-111">When you sign-up for a subscription</span></span> |
| <span data-ttu-id="3208c-112">Azure AD Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="3208c-112">Azure AD Free</span></span> | <span data-ttu-id="3208c-113">Hej första gången du öppnar hello [Azure Active Directory-bladet](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) eller använda hello [reporting API: er](https://aka.ms/aadreports)</span><span class="sxs-lookup"><span data-stu-id="3208c-113">hello first time you open hello [Azure Active Directory blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) or use hello [reporting APIs](https://aka.ms/aadreports)</span></span>  |

---
<span data-ttu-id="3208c-114">**F: när är din aktivitetsdata i hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="3208c-114">**Q: When is your activity data available in hello Azure portal?**</span></span>

<span data-ttu-id="3208c-115">**S:**</span><span class="sxs-lookup"><span data-stu-id="3208c-115">**A:**</span></span>

- <span data-ttu-id="3208c-116">**Omedelbart** - om du redan har arbetat med rapporter i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3208c-116">**Immediately** - If you have already been working with reports in hello Azure classic portal</span></span>
- <span data-ttu-id="3208c-117">**Inom två timmar** - om du inte har aktiverat rapportering i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3208c-117">**Within 2 hours** - If you haven’t turned reporting on  in hello Azure classic portal</span></span>

---
<span data-ttu-id="3208c-118">**F: hur kan du få hello insamling av säkerhet signaler igång?**</span><span class="sxs-lookup"><span data-stu-id="3208c-118">**Q: How can you get hello collection of security signals started?**</span></span>  

<span data-ttu-id="3208c-119">**S:** för säkerhet signaler hello samling processen startar när du delta i toouse hello Identity Protection Center.</span><span class="sxs-lookup"><span data-stu-id="3208c-119">**A:** For security signals, hello collection process starts when you opt-in toouse hello Identity Protection Center.</span></span> 


---
<span data-ttu-id="3208c-120">**F: hur länge hello insamlade data lagras?**</span><span class="sxs-lookup"><span data-stu-id="3208c-120">**Q: For how long is hello collected data stored?**</span></span>

<span data-ttu-id="3208c-121">**S:**</span><span class="sxs-lookup"><span data-stu-id="3208c-121">**A:**</span></span>

<span data-ttu-id="3208c-122">**Aktivitetsrapporter**</span><span class="sxs-lookup"><span data-stu-id="3208c-122">**Activity reports**</span></span>    

| <span data-ttu-id="3208c-123">Rapport</span><span class="sxs-lookup"><span data-stu-id="3208c-123">Report</span></span>                 | <span data-ttu-id="3208c-124">Azure AD Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="3208c-124">Azure AD Free</span></span> | <span data-ttu-id="3208c-125">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="3208c-125">Azure AD Premium P1</span></span> | <span data-ttu-id="3208c-126">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="3208c-126">Azure AD Premium P2</span></span> |
| :--                    | :--           | :--                 | :--                 |
| <span data-ttu-id="3208c-127">Kataloggranskning</span><span class="sxs-lookup"><span data-stu-id="3208c-127">Directory Audit</span></span>        | <span data-ttu-id="3208c-128">7 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-128">7 days</span></span>        | <span data-ttu-id="3208c-129">30 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-129">30 days</span></span>             | <span data-ttu-id="3208c-130">30 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-130">30 days</span></span>             |
| <span data-ttu-id="3208c-131">Inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="3208c-131">Sign-in Activity</span></span>       | <span data-ttu-id="3208c-132">7 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-132">7 days</span></span>        | <span data-ttu-id="3208c-133">30 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-133">30 days</span></span>             | <span data-ttu-id="3208c-134">30 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-134">30 days</span></span>             |

<span data-ttu-id="3208c-135">**Säkerhet signaler**</span><span class="sxs-lookup"><span data-stu-id="3208c-135">**Security Signals**</span></span>

| <span data-ttu-id="3208c-136">Rapport</span><span class="sxs-lookup"><span data-stu-id="3208c-136">Report</span></span>         | <span data-ttu-id="3208c-137">Azure AD Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="3208c-137">Azure AD Free</span></span> | <span data-ttu-id="3208c-138">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="3208c-138">Azure AD Premium P1</span></span> | <span data-ttu-id="3208c-139">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="3208c-139">Azure AD Premium P2</span></span> |
| :--            | :--           | :--                 | :--                 |
| <span data-ttu-id="3208c-140">Användare i farozonen</span><span class="sxs-lookup"><span data-stu-id="3208c-140">Users at risk</span></span>  | <span data-ttu-id="3208c-141">7 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-141">7 days</span></span>        | <span data-ttu-id="3208c-142">30 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-142">30 days</span></span>             | <span data-ttu-id="3208c-143">90 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-143">90 days</span></span>             |
| <span data-ttu-id="3208c-144">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="3208c-144">Risky sign-ins</span></span> | <span data-ttu-id="3208c-145">7 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-145">7 days</span></span>        | <span data-ttu-id="3208c-146">30 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-146">30 days</span></span>             | <span data-ttu-id="3208c-147">90 dagar</span><span class="sxs-lookup"><span data-stu-id="3208c-147">90 days</span></span>             |

---