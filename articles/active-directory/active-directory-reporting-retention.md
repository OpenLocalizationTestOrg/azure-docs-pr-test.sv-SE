---
title: "Rapportkvarhållningsregler i Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: ffeba8a6c32a21c75af21f948bbd6ea88dd9278c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-report-retention-policies"></a><span data-ttu-id="e1ad2-103">Rapportkvarhållningsregler i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1ad2-103">Azure Active Directory report retention policies</span></span>


<span data-ttu-id="e1ad2-104">Det här avsnittet ger svar på de vanligaste frågorna tillsammans med datalagring för olika aktivitetsrapporter i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e1ad2-104">This topic provides you with answers to the most common questions in conjunction with the data retention for the different activity reports in Azure Active Directory.</span></span> 

<span data-ttu-id="e1ad2-105">**F: hur kan du få insamlingen av aktivitetsdata starta?**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-105">**Q: How can you get the collection of activity data started?**</span></span>

<span data-ttu-id="e1ad2-106">**S:**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-106">**A:**</span></span>

| <span data-ttu-id="e1ad2-107">Azure AD-version</span><span class="sxs-lookup"><span data-stu-id="e1ad2-107">Azure AD Edition</span></span> | <span data-ttu-id="e1ad2-108">Samlingen Start</span><span class="sxs-lookup"><span data-stu-id="e1ad2-108">Collection Start</span></span> |
| :--              | :--   |
| <span data-ttu-id="e1ad2-109">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="e1ad2-109">Azure AD Premium P1</span></span> <br /> <span data-ttu-id="e1ad2-110">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="e1ad2-110">Azure AD Premium P2</span></span> | <span data-ttu-id="e1ad2-111">När du registrerar dig för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="e1ad2-111">When you sign-up for a subscription</span></span> |
| <span data-ttu-id="e1ad2-112">Azure AD Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="e1ad2-112">Azure AD Free</span></span> | <span data-ttu-id="e1ad2-113">Första gången du öppnar den [Azure Active Directory-bladet](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) eller använda den [reporting API: er](https://aka.ms/aadreports)</span><span class="sxs-lookup"><span data-stu-id="e1ad2-113">The first time you open the [Azure Active Directory blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) or use the [reporting APIs](https://aka.ms/aadreports)</span></span>  |

---
<span data-ttu-id="e1ad2-114">**F: när är aktivitetsdata i Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-114">**Q: When is your activity data available in the Azure portal?**</span></span>

<span data-ttu-id="e1ad2-115">**S:**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-115">**A:**</span></span>

- <span data-ttu-id="e1ad2-116">**Omedelbart** - om du redan har arbetat med rapporter i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e1ad2-116">**Immediately** - If you have already been working with reports in the Azure classic portal</span></span>
- <span data-ttu-id="e1ad2-117">**Inom två timmar** - om du inte har aktiverat rapportering i den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e1ad2-117">**Within 2 hours** - If you haven’t turned reporting on  in the Azure classic portal</span></span>

---
<span data-ttu-id="e1ad2-118">**F: hur kan du få insamling av säkerhet signaler igång?**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-118">**Q: How can you get the collection of security signals started?**</span></span>  

<span data-ttu-id="e1ad2-119">**S:** för säkerhet signaler samling processen startar när du delta i använda Identity Protection Center.</span><span class="sxs-lookup"><span data-stu-id="e1ad2-119">**A:** For security signals, the collection process starts when you opt-in to use the Identity Protection Center.</span></span> 


---
<span data-ttu-id="e1ad2-120">**F: hur länge insamlade data lagras?**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-120">**Q: For how long is the collected data stored?**</span></span>

<span data-ttu-id="e1ad2-121">**S:**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-121">**A:**</span></span>

<span data-ttu-id="e1ad2-122">**Aktivitetsrapporter**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-122">**Activity reports**</span></span>    

| <span data-ttu-id="e1ad2-123">Rapport</span><span class="sxs-lookup"><span data-stu-id="e1ad2-123">Report</span></span>                 | <span data-ttu-id="e1ad2-124">Azure AD Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="e1ad2-124">Azure AD Free</span></span> | <span data-ttu-id="e1ad2-125">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="e1ad2-125">Azure AD Premium P1</span></span> | <span data-ttu-id="e1ad2-126">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="e1ad2-126">Azure AD Premium P2</span></span> |
| :--                    | :--           | :--                 | :--                 |
| <span data-ttu-id="e1ad2-127">Kataloggranskning</span><span class="sxs-lookup"><span data-stu-id="e1ad2-127">Directory Audit</span></span>        | <span data-ttu-id="e1ad2-128">7 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-128">7 days</span></span>        | <span data-ttu-id="e1ad2-129">30 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-129">30 days</span></span>             | <span data-ttu-id="e1ad2-130">30 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-130">30 days</span></span>             |
| <span data-ttu-id="e1ad2-131">Inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="e1ad2-131">Sign-in Activity</span></span>       | <span data-ttu-id="e1ad2-132">7 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-132">7 days</span></span>        | <span data-ttu-id="e1ad2-133">30 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-133">30 days</span></span>             | <span data-ttu-id="e1ad2-134">30 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-134">30 days</span></span>             |

<span data-ttu-id="e1ad2-135">**Säkerhet signaler**</span><span class="sxs-lookup"><span data-stu-id="e1ad2-135">**Security Signals**</span></span>

| <span data-ttu-id="e1ad2-136">Rapport</span><span class="sxs-lookup"><span data-stu-id="e1ad2-136">Report</span></span>         | <span data-ttu-id="e1ad2-137">Azure AD Kostnadsfri</span><span class="sxs-lookup"><span data-stu-id="e1ad2-137">Azure AD Free</span></span> | <span data-ttu-id="e1ad2-138">Azure AD Premium P1</span><span class="sxs-lookup"><span data-stu-id="e1ad2-138">Azure AD Premium P1</span></span> | <span data-ttu-id="e1ad2-139">Azure AD Premium P2</span><span class="sxs-lookup"><span data-stu-id="e1ad2-139">Azure AD Premium P2</span></span> |
| :--            | :--           | :--                 | :--                 |
| <span data-ttu-id="e1ad2-140">Användare i farozonen</span><span class="sxs-lookup"><span data-stu-id="e1ad2-140">Users at risk</span></span>  | <span data-ttu-id="e1ad2-141">7 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-141">7 days</span></span>        | <span data-ttu-id="e1ad2-142">30 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-142">30 days</span></span>             | <span data-ttu-id="e1ad2-143">90 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-143">90 days</span></span>             |
| <span data-ttu-id="e1ad2-144">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-144">Risky sign-ins</span></span> | <span data-ttu-id="e1ad2-145">7 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-145">7 days</span></span>        | <span data-ttu-id="e1ad2-146">30 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-146">30 days</span></span>             | <span data-ttu-id="e1ad2-147">90 dagar</span><span class="sxs-lookup"><span data-stu-id="e1ad2-147">90 days</span></span>             |

---