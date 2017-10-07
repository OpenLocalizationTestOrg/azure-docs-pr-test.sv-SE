---
title: aaaAzure Active Directory reporting svarstiderna | Microsoft Docs
description: "Lär dig mer om hello lång tid det tar för rapportering händelser tooshow in i din Azure-portalen"
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
ms.openlocfilehash: eee959331262ba59b313dd038cb54699dbef48a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-latencies"></a><span data-ttu-id="9495c-103">Azure Active Directory reporting svarstider</span><span class="sxs-lookup"><span data-stu-id="9495c-103">Azure Active Directory reporting latencies</span></span>

<span data-ttu-id="9495c-104">Med [reporting](active-directory-preview-explainer.md) i hello Azure Active Directory, du får alla hello information som du behöver toodetermine hur din miljö gör.</span><span class="sxs-lookup"><span data-stu-id="9495c-104">With [reporting](active-directory-preview-explainer.md) in hello Azure Active Directory, you get all hello information you need toodetermine how your environment is doing.</span></span> <span data-ttu-id="9495c-105">hello mängden tid det tar för att rapportera data tooshow upp i hello Azure-portalen kallas även svarstid.</span><span class="sxs-lookup"><span data-stu-id="9495c-105">hello amount of time it takes for reporting data tooshow up in hello Azure portal is also known as latency.</span></span> 

<span data-ttu-id="9495c-106">Det här avsnittet listar hello latensinformation för hello alla reporting kategorier i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9495c-106">This topic lists hello latency information for hello all reporting categories in hello Azure portal.</span></span> 


## <a name="activity-reports"></a><span data-ttu-id="9495c-107">Aktivitetsrapporter</span><span class="sxs-lookup"><span data-stu-id="9495c-107">Activity reports</span></span>

<span data-ttu-id="9495c-108">Det finns två områden av aktiviteten reporting:</span><span class="sxs-lookup"><span data-stu-id="9495c-108">There are two areas of activity reporting:</span></span>

- <span data-ttu-id="9495c-109">**Logga in aktiviteter** – Information om hello användning av hanterade program och användaren loggar in aktiviteter</span><span class="sxs-lookup"><span data-stu-id="9495c-109">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
- <span data-ttu-id="9495c-110">**Granskningsloggar** – Granska information om systemaktivitet för användare och grupphantering, dina hanterade program och katalogaktiviteter</span><span class="sxs-lookup"><span data-stu-id="9495c-110">**Audit logs** - System activity information about users and group management, your managed applications and directory activities</span></span>

<span data-ttu-id="9495c-111">hello i den följande tabellen listas hello latensinformation för aktivitetsrapporter.</span><span class="sxs-lookup"><span data-stu-id="9495c-111">hello following table lists hello latency information for activity reports.</span></span>

| <span data-ttu-id="9495c-112">Rapport</span><span class="sxs-lookup"><span data-stu-id="9495c-112">Report</span></span> | <span data-ttu-id="9495c-113">Minimum</span><span class="sxs-lookup"><span data-stu-id="9495c-113">Minimum</span></span> | <span data-ttu-id="9495c-114">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="9495c-114">Average</span></span> | <span data-ttu-id="9495c-115">Maximalt</span><span class="sxs-lookup"><span data-stu-id="9495c-115">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="9495c-116">Granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="9495c-116">Audit logs</span></span>             | <span data-ttu-id="9495c-117">30 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-117">30 minutes</span></span>  | <span data-ttu-id="9495c-118">45 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-118">45 Minutes</span></span> | <span data-ttu-id="9495c-119">1 timme</span><span class="sxs-lookup"><span data-stu-id="9495c-119">1 hour</span></span>     |
| <span data-ttu-id="9495c-120">Inloggningar</span><span class="sxs-lookup"><span data-stu-id="9495c-120">Sign-ins</span></span>               | <span data-ttu-id="9495c-121">15 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-121">15 minutes</span></span>  | <span data-ttu-id="9495c-122">15 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-122">15 minutes</span></span> | <span data-ttu-id="9495c-123">2 timmar *</span><span class="sxs-lookup"><span data-stu-id="9495c-123">2 hours*</span></span>   |

>[!NOTE]
> <span data-ttu-id="9495c-124">För vissa inloggningar aktivitetsdata från äldre office-program, kan det ta too8 timmar för hello rapporterar data tooshow.</span><span class="sxs-lookup"><span data-stu-id="9495c-124">For some sign-ins activity data coming from legacy office applications, it can take too8 hours for hello reporting data tooshow up.</span></span> 


## <a name="security-reports"></a><span data-ttu-id="9495c-125">Säkerhetsrapporter</span><span class="sxs-lookup"><span data-stu-id="9495c-125">Security reports</span></span>

<span data-ttu-id="9495c-126">Det finns två områden av säkerhet reporting:</span><span class="sxs-lookup"><span data-stu-id="9495c-126">There are two areas of security reporting:</span></span>

- <span data-ttu-id="9495c-127">**Riskfyllda inloggningar** -riskfyllda loggar in är en indikator för en inloggning försök som kan ha utförts av någon som inte är hello legitima ägare för ett användarkonto.</span><span class="sxs-lookup"><span data-stu-id="9495c-127">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> 
- <span data-ttu-id="9495c-128">**Användare som har flaggats för risk** – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.</span><span class="sxs-lookup"><span data-stu-id="9495c-128">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> 

<span data-ttu-id="9495c-129">hello i den följande tabellen listas hello latensinformation för säkerhetsrapporter.</span><span class="sxs-lookup"><span data-stu-id="9495c-129">hello following table lists hello latency information for security reports.</span></span>

| <span data-ttu-id="9495c-130">Rapport</span><span class="sxs-lookup"><span data-stu-id="9495c-130">Report</span></span> | <span data-ttu-id="9495c-131">Minimum</span><span class="sxs-lookup"><span data-stu-id="9495c-131">Minimum</span></span> | <span data-ttu-id="9495c-132">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="9495c-132">Average</span></span> | <span data-ttu-id="9495c-133">Maximalt</span><span class="sxs-lookup"><span data-stu-id="9495c-133">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="9495c-134">Användare i farozonen</span><span class="sxs-lookup"><span data-stu-id="9495c-134">Users at risk</span></span>          | <span data-ttu-id="9495c-135">5 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-135">5 minutes</span></span>   | <span data-ttu-id="9495c-136">15 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-136">15 minutes</span></span>  | <span data-ttu-id="9495c-137">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-137">2 hours</span></span>  |
| <span data-ttu-id="9495c-138">Riskfyllda inloggningar</span><span class="sxs-lookup"><span data-stu-id="9495c-138">Risky sign-ins</span></span>         | <span data-ttu-id="9495c-139">5 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-139">5 minutes</span></span>   | <span data-ttu-id="9495c-140">15 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-140">15 minutes</span></span>  | <span data-ttu-id="9495c-141">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-141">2 hours</span></span>  |

## <a name="risk-events"></a><span data-ttu-id="9495c-142">Riskhändelser</span><span class="sxs-lookup"><span data-stu-id="9495c-142">Risk events</span></span>

<span data-ttu-id="9495c-143">Azure Active Directory använder anpassningsbar maskininlärning algoritmer och heuristik toodetect misstänkta åtgärder som är relaterade tooyour användarkonton.</span><span class="sxs-lookup"><span data-stu-id="9495c-143">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="9495c-144">Varje upptäckt misstänkt åtgärd lagras i en post kallas risk händelse.</span><span class="sxs-lookup"><span data-stu-id="9495c-144">Each detected suspicious action is stored in a record called risk event.</span></span>

<span data-ttu-id="9495c-145">hello i den följande tabellen listas hello latensinformation för riskhändelser.</span><span class="sxs-lookup"><span data-stu-id="9495c-145">hello following table lists hello latency information for risk events.</span></span>

| <span data-ttu-id="9495c-146">Rapport</span><span class="sxs-lookup"><span data-stu-id="9495c-146">Report</span></span> | <span data-ttu-id="9495c-147">Minimum</span><span class="sxs-lookup"><span data-stu-id="9495c-147">Minimum</span></span> | <span data-ttu-id="9495c-148">Genomsnittlig</span><span class="sxs-lookup"><span data-stu-id="9495c-148">Average</span></span> | <span data-ttu-id="9495c-149">Maximalt</span><span class="sxs-lookup"><span data-stu-id="9495c-149">Maximum</span></span> |
| :-- | --- | --- | --- |
| <span data-ttu-id="9495c-150">Inloggningar från anonyma IP-adresser</span><span class="sxs-lookup"><span data-stu-id="9495c-150">Sign-ins from anonymous IP addresses</span></span> |<span data-ttu-id="9495c-151">5 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-151">5 minutes</span></span> |<span data-ttu-id="9495c-152">15 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-152">15 Minutes</span></span> |<span data-ttu-id="9495c-153">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-153">2 hours</span></span> |
| <span data-ttu-id="9495c-154">Inloggningar från okända platser</span><span class="sxs-lookup"><span data-stu-id="9495c-154">Sign-ins from unfamiliar locations</span></span> |<span data-ttu-id="9495c-155">5 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-155">5 minutes</span></span> |<span data-ttu-id="9495c-156">15 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-156">15 Minutes</span></span> |<span data-ttu-id="9495c-157">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-157">2 hours</span></span> |
| <span data-ttu-id="9495c-158">Används med läckta autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="9495c-158">Users with leaked credentials</span></span> |<span data-ttu-id="9495c-159">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-159">2 hours</span></span> |<span data-ttu-id="9495c-160">4 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-160">4 hours</span></span> |<span data-ttu-id="9495c-161">8 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-161">8 hours</span></span> |
| <span data-ttu-id="9495c-162">Omöjlig resa tooatypical platser</span><span class="sxs-lookup"><span data-stu-id="9495c-162">Impossible travel tooatypical locations</span></span> |<span data-ttu-id="9495c-163">5 minuter</span><span class="sxs-lookup"><span data-stu-id="9495c-163">5 minutes</span></span> |<span data-ttu-id="9495c-164">1 timme</span><span class="sxs-lookup"><span data-stu-id="9495c-164">1 hour</span></span> |<span data-ttu-id="9495c-165">8 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-165">8 hours</span></span>  |
| <span data-ttu-id="9495c-166">Inloggningar från angripna enheter</span><span class="sxs-lookup"><span data-stu-id="9495c-166">Sign-ins from infected devices</span></span> |<span data-ttu-id="9495c-167">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-167">2 hours</span></span> |<span data-ttu-id="9495c-168">4 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-168">4 hours</span></span> |<span data-ttu-id="9495c-169">8 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-169">8 hours</span></span>  |
| <span data-ttu-id="9495c-170">Inloggningar från IP-adresser med misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="9495c-170">Sign-ins from IP addresses with suspicious activity</span></span> |<span data-ttu-id="9495c-171">2 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-171">2 hours</span></span> |<span data-ttu-id="9495c-172">4 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-172">4 hours</span></span> |<span data-ttu-id="9495c-173">8 timmar</span><span class="sxs-lookup"><span data-stu-id="9495c-173">8 hours</span></span>  |



## <a name="next-steps"></a><span data-ttu-id="9495c-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9495c-174">Next steps</span></span>

<span data-ttu-id="9495c-175">Om du vill tooknow mer om hello aktivitetsrapporter i hello Azure-portalen, se:</span><span class="sxs-lookup"><span data-stu-id="9495c-175">If you want tooknow more about hello activity reports in hello Azure portal, see:</span></span>

- [<span data-ttu-id="9495c-176">Inloggningsaktivitet rapporter i hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="9495c-176">Sign-in activity reports in hello Azure Active Directory portal</span></span>](active-directory-reporting-activity-sign-ins.md)
- [<span data-ttu-id="9495c-177">Granska aktivitetsrapporter hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="9495c-177">Audit activity reports in hello Azure Active Directory portal</span></span>](active-directory-reporting-activity-audit-logs.md)

<span data-ttu-id="9495c-178">Om du vill tooknow mer om hello säkerhetsrapporter i hello Azure-portalen, se:</span><span class="sxs-lookup"><span data-stu-id="9495c-178">If you want tooknow more about hello security reports in hello Azure portal, see:</span></span>

- [<span data-ttu-id="9495c-179">Användare på risk säkerhetsrapporten hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="9495c-179">Users at risk security report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="9495c-180">Riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen</span><span class="sxs-lookup"><span data-stu-id="9495c-180">Risky sign-ins report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)

<span data-ttu-id="9495c-181">Om du vill tooknow mer om riskhändelser finns [Azure Active Directory riskhändelser](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="9495c-181">If you want tooknow more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>
