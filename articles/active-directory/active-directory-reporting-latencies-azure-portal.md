---
title: Azure Active Directory reporting svarstiderna | Microsoft Docs
description: "Lär dig mer om hur lång tid det tar för rapporteringshändelser ska visas i din Azure-portalen"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: 93cb0baeab8f13f81257ed1bd32ed08561c54b72
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-reporting-latencies"></a><span data-ttu-id="49b58-103">Azure Active Directory reporting svarstider</span><span class="sxs-lookup"><span data-stu-id="49b58-103">Azure Active Directory reporting latencies</span></span>

<span data-ttu-id="49b58-104">Med [reporting](active-directory-preview-explainer.md) i Azure Active Directory så, du får all information du behöver ta reda på hur du gör din miljö.</span><span class="sxs-lookup"><span data-stu-id="49b58-104">With [reporting](active-directory-preview-explainer.md) in the Azure Active Directory, you get all the information you need to determine how your environment is doing.</span></span> <span data-ttu-id="49b58-105">Hur lång tid det tar för att rapportera data visas i Azure-portalen kallas även svarstid.</span><span class="sxs-lookup"><span data-stu-id="49b58-105">The amount of time it takes for reporting data to show up in the Azure portal is also known as latency.</span></span> 

<span data-ttu-id="49b58-106">Det här avsnittet innehåller information svarstid för alla reporting kategorier i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="49b58-106">This topic lists the latency information for the all reporting categories in the Azure portal.</span></span> 


## <a name="activity-reports"></a><span data-ttu-id="49b58-107">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="49b58-107">Activity reports</span></span>

<span data-ttu-id="49b58-108">Det finns två områden av aktiviteten reporting:</span><span class="sxs-lookup"><span data-stu-id="49b58-108">There are two areas of activity reporting:</span></span>

- <span data-ttu-id="49b58-109">**Inloggningsaktiviteter** – Information om användningen av hanterade program och användares inloggningsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="49b58-109">**Sign-in activities** – Information about the usage of managed applications and user sign-in activities</span></span>
- <span data-ttu-id="49b58-110">**Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter</span><span class="sxs-lookup"><span data-stu-id="49b58-110">**Audit logs** - System activity information about users and group management, your managed applications and directory activities</span></span>

<span data-ttu-id="49b58-111">I följande tabell visas latensinformation för aktivitetsrapporter.</span><span class="sxs-lookup"><span data-stu-id="49b58-111">The following table lists the latency information for activity reports.</span></span>

| <span data-ttu-id="49b58-112">Rapport</span><span class="sxs-lookup"><span data-stu-id="49b58-112">Report</span></span> | <span data-ttu-id="49b58-113">Minimum</span><span class="sxs-lookup"><span data-stu-id="49b58-113">Minimum</span></span> | <span data-ttu-id="49b58-114">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="49b58-114">Average</span></span> | <span data-ttu-id="49b58-115">Maximalt</span><span class="sxs-lookup"><span data-stu-id="49b58-115">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="49b58-116">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="49b58-116">Audit logs</span></span>             | <span data-ttu-id="49b58-117">30 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-117">30 minutes</span></span>  | <span data-ttu-id="49b58-118">45 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-118">45 Minutes</span></span> | <span data-ttu-id="49b58-119">1 timme</span><span class="sxs-lookup"><span data-stu-id="49b58-119">1 hour</span></span>     |
| <span data-ttu-id="49b58-120">Inloggningar</span><span class="sxs-lookup"><span data-stu-id="49b58-120">Sign-ins</span></span>               | <span data-ttu-id="49b58-121">15 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-121">15 minutes</span></span>  | <span data-ttu-id="49b58-122">15 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-122">15 minutes</span></span> | <span data-ttu-id="49b58-123">2 timmar *</span><span class="sxs-lookup"><span data-stu-id="49b58-123">2 hours*</span></span>   |

>[!NOTE]
> <span data-ttu-id="49b58-124">Det kan ta upp till 8 timmar för rapporteringsdata att visas för vissa aktivitetsdata om inloggningsåtgärder från äldre Office-program.</span><span class="sxs-lookup"><span data-stu-id="49b58-124">For some sign-ins activity data coming from legacy office applications, it can take to 8 hours for the reporting data to show up.</span></span> 


## <a name="security-reports"></a><span data-ttu-id="49b58-125">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="49b58-125">Security reports</span></span>

<span data-ttu-id="49b58-126">Det finns två områden av säkerhet reporting:</span><span class="sxs-lookup"><span data-stu-id="49b58-126">There are two areas of security reporting:</span></span>

- <span data-ttu-id="49b58-127">**Riskfyllda inloggningar** – En riskfylld inloggning indikerar ett potentiellt inloggningsförsök av någon annan än användarkontots ägare.</span><span class="sxs-lookup"><span data-stu-id="49b58-127">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> 
- <span data-ttu-id="49b58-128">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="49b58-128">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> 

<span data-ttu-id="49b58-129">I följande tabell visas latensinformation för säkerhetsrapporter.</span><span class="sxs-lookup"><span data-stu-id="49b58-129">The following table lists the latency information for security reports.</span></span>

| <span data-ttu-id="49b58-130">Rapport</span><span class="sxs-lookup"><span data-stu-id="49b58-130">Report</span></span> | <span data-ttu-id="49b58-131">Minimum</span><span class="sxs-lookup"><span data-stu-id="49b58-131">Minimum</span></span> | <span data-ttu-id="49b58-132">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="49b58-132">Average</span></span> | <span data-ttu-id="49b58-133">Maximalt</span><span class="sxs-lookup"><span data-stu-id="49b58-133">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="49b58-134">Användare i farozonen</span><span class="sxs-lookup"><span data-stu-id="49b58-134">Users at risk</span></span>          | <span data-ttu-id="49b58-135">5 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-135">5 minutes</span></span>   | <span data-ttu-id="49b58-136">15 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-136">15 minutes</span></span>  | <span data-ttu-id="49b58-137">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-137">2 hours</span></span>  |
| <span data-ttu-id="49b58-138">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="49b58-138">Risky sign-ins</span></span>         | <span data-ttu-id="49b58-139">5 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-139">5 minutes</span></span>   | <span data-ttu-id="49b58-140">15 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-140">15 minutes</span></span>  | <span data-ttu-id="49b58-141">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-141">2 hours</span></span>  |

## <a name="risk-events"></a><span data-ttu-id="49b58-142">Riskhändelser</span><span class="sxs-lookup"><span data-stu-id="49b58-142">Risk events</span></span>

<span data-ttu-id="49b58-143">Azure Active Directory använder anpassningsbar maskininlärningsalgoritmer och heuristik för att identifiera misstänkta åtgärder som är relaterade till dina användarkonton.</span><span class="sxs-lookup"><span data-stu-id="49b58-143">Azure Active Directory uses adaptive machine learning algorithms and heuristics to detect suspicious actions that are related to your user accounts.</span></span> <span data-ttu-id="49b58-144">Varje upptäckt misstänkt åtgärd lagras i en post kallas risk händelse.</span><span class="sxs-lookup"><span data-stu-id="49b58-144">Each detected suspicious action is stored in a record called risk event.</span></span>

<span data-ttu-id="49b58-145">I följande tabell visas latensinformation för riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="49b58-145">The following table lists the latency information for risk events.</span></span>

| <span data-ttu-id="49b58-146">Rapport</span><span class="sxs-lookup"><span data-stu-id="49b58-146">Report</span></span> | <span data-ttu-id="49b58-147">Minimum</span><span class="sxs-lookup"><span data-stu-id="49b58-147">Minimum</span></span> | <span data-ttu-id="49b58-148">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="49b58-148">Average</span></span> | <span data-ttu-id="49b58-149">Maximalt</span><span class="sxs-lookup"><span data-stu-id="49b58-149">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="49b58-150">Inloggningar från anonyma IP-adresser</span><span class="sxs-lookup"><span data-stu-id="49b58-150">Sign-ins from anonymous IP addresses</span></span> |<span data-ttu-id="49b58-151">5 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-151">5 minutes</span></span> |<span data-ttu-id="49b58-152">15 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-152">15 Minutes</span></span> |<span data-ttu-id="49b58-153">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-153">2 hours</span></span> |
| <span data-ttu-id="49b58-154">Inloggningar från okända platser</span><span class="sxs-lookup"><span data-stu-id="49b58-154">Sign-ins from unfamiliar locations</span></span> |<span data-ttu-id="49b58-155">5 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-155">5 minutes</span></span> |<span data-ttu-id="49b58-156">15 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-156">15 Minutes</span></span> |<span data-ttu-id="49b58-157">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-157">2 hours</span></span> |
| <span data-ttu-id="49b58-158">Används med läckta autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="49b58-158">Users with leaked credentials</span></span> |<span data-ttu-id="49b58-159">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-159">2 hours</span></span> |<span data-ttu-id="49b58-160">4 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-160">4 hours</span></span> |<span data-ttu-id="49b58-161">8 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-161">8 hours</span></span> |
| <span data-ttu-id="49b58-162">Omöjligt att resa till ovanliga platser</span><span class="sxs-lookup"><span data-stu-id="49b58-162">Impossible travel to atypical locations</span></span> |<span data-ttu-id="49b58-163">5 minuter</span><span class="sxs-lookup"><span data-stu-id="49b58-163">5 minutes</span></span> |<span data-ttu-id="49b58-164">1 timme</span><span class="sxs-lookup"><span data-stu-id="49b58-164">1 hour</span></span> |<span data-ttu-id="49b58-165">8 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-165">8 hours</span></span>  |
| <span data-ttu-id="49b58-166">Inloggningar från angripna enheter</span><span class="sxs-lookup"><span data-stu-id="49b58-166">Sign-ins from infected devices</span></span> |<span data-ttu-id="49b58-167">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-167">2 hours</span></span> |<span data-ttu-id="49b58-168">4 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-168">4 hours</span></span> |<span data-ttu-id="49b58-169">8 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-169">8 hours</span></span>  |
| <span data-ttu-id="49b58-170">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="49b58-170">Sign-ins from IP addresses with suspicious activity</span></span> |<span data-ttu-id="49b58-171">2 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-171">2 hours</span></span> |<span data-ttu-id="49b58-172">4 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-172">4 hours</span></span> |<span data-ttu-id="49b58-173">8 timmar</span><span class="sxs-lookup"><span data-stu-id="49b58-173">8 hours</span></span>  |



## <a name="next-steps"></a><span data-ttu-id="49b58-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49b58-174">Next steps</span></span>

<span data-ttu-id="49b58-175">Om du vill veta mer om aktivitetsrapporter i Azure-portalen, se:</span><span class="sxs-lookup"><span data-stu-id="49b58-175">If you want to know more about the activity reports in the Azure portal, see:</span></span>

- [<span data-ttu-id="49b58-176">Inloggningsaktivitet rapporterna i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="49b58-176">Sign-in activity reports in the Azure Active Directory portal</span></span>](active-directory-reporting-activity-sign-ins.md)
- [<span data-ttu-id="49b58-177">Granska aktivitetsrapporter i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="49b58-177">Audit activity reports in the Azure Active Directory portal</span></span>](active-directory-reporting-activity-audit-logs.md)

<span data-ttu-id="49b58-178">Om du vill veta mer om säkerhetsrapporter i Azure-portalen, se:</span><span class="sxs-lookup"><span data-stu-id="49b58-178">If you want to know more about the security reports in the Azure portal, see:</span></span>

- [<span data-ttu-id="49b58-179">Användare på risk säkerhetsrapporten i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="49b58-179">Users at risk security report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="49b58-180">Rapporten riskfyllda inloggningar i Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="49b58-180">Risky sign-ins report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)

<span data-ttu-id="49b58-181">Om du vill veta mer om riskhändelser finns [Azure Active Directory riskhändelser](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="49b58-181">If you want to know more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>
