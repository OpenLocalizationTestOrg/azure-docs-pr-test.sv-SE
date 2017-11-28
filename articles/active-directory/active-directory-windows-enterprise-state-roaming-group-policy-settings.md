---
title: "aaaGroup principen och MDM-inställningar | Microsoft Docs"
description: "Innehåller information om grupprinciper och mobila enheter (MDM) inställningar som ska användas på företagsägda enheter. Dessa principer är tillämpade toohello hela enheten."
services: active-directory
keywords: "Vad är grupp princip och MDM-inställningar för Enterprise tillstånd Roaming, Enterprise tillstånd Roaming, windows molnet"
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 762419b47014b1fb4d92ac528785e20078afe689
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="group-policy-and-mdm-settings"></a><span data-ttu-id="72447-105">Inställningar för Grupprincip och MDM</span><span class="sxs-lookup"><span data-stu-id="72447-105">Group Policy and MDM settings</span></span>
<span data-ttu-id="72447-106">Dessa Grupprincip och hanteringsinställningar för mobila enheter (MDM) endast användas på företagsägda enheter eftersom dessa principer används toohello hela enheten.</span><span class="sxs-lookup"><span data-stu-id="72447-106">Use these group policy and mobile device management (MDM) settings only on corporate-owned devices because these policies are applied toohello user’s entire device.</span></span> <span data-ttu-id="72447-107">Använda en MDM-princip toodisable synkronisera inställningar för en personlig ska enhet som ägs av användare påverka hello användning av enheten.</span><span class="sxs-lookup"><span data-stu-id="72447-107">Applying an MDM policy toodisable settings sync for a personal, user-owned device will negatively impact hello use of that device.</span></span> <span data-ttu-id="72447-108">Dessutom påverkas även andra konton på hello enhet av hello princip.</span><span class="sxs-lookup"><span data-stu-id="72447-108">Additionally, other user accounts on hello device will also be affected by hello policy.</span></span>

<span data-ttu-id="72447-109">Företag som vill toomanage roaming för personliga (ohanterade) enheter kan använda hello Azure portal tooenable eller inaktivera centrala i stället för att använda en grupprincip eller MDM.</span><span class="sxs-lookup"><span data-stu-id="72447-109">Enterprises that want toomanage roaming for personal (unmanaged) devices can use hello Azure portal tooenable or disable roaming, rather than using Group Policy or MDM.</span></span>
<span data-ttu-id="72447-110">hello följande tabeller beskrivs hello principinställningar som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="72447-110">hello following tables describe hello policy settings available.</span></span>

## <a name="mdm-settings"></a><span data-ttu-id="72447-111">MDM-inställningar</span><span class="sxs-lookup"><span data-stu-id="72447-111">MDM settings</span></span>
<span data-ttu-id="72447-112">hello MDM principinställningarna gäller tooboth Windows 10 och Windows 10 Mobile.</span><span class="sxs-lookup"><span data-stu-id="72447-112">hello MDM policy settings apply tooboth Windows 10 and Windows 10 Mobile.</span></span>  <span data-ttu-id="72447-113">Det finns stöd för Windows 10 Mobile endast för Microsoft-konto baserat centrala via användarens OneDrive-konto.</span><span class="sxs-lookup"><span data-stu-id="72447-113">Windows 10 Mobile support exists only for Microsoft account based roaming via user’s OneDrive account.</span></span>  <span data-ttu-id="72447-114">Se för avsnittet ”enheter och slutpunkter” mer information om vilka enheter som stöds för Azure AD baseras synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="72447-114">Please refer too“Devices and endpoints” section for details on what devices are supported for Azure AD based syncing.</span></span>

| <span data-ttu-id="72447-115">Namn</span><span class="sxs-lookup"><span data-stu-id="72447-115">Name</span></span> | <span data-ttu-id="72447-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72447-116">Description</span></span> |
| --- | --- |
| <span data-ttu-id="72447-117">Tillåt Microsoft-konto-anslutning</span><span class="sxs-lookup"><span data-stu-id="72447-117">Allow Microsoft Account Connection</span></span> |<span data-ttu-id="72447-118">Tillåter användare tooauthenticate med ett Microsoft-konto på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="72447-118">Allows users tooauthenticate using a Microsoft account on hello device</span></span> |
| <span data-ttu-id="72447-119">Tillåt synkronisering av Mina inställningar</span><span class="sxs-lookup"><span data-stu-id="72447-119">Allow Sync My Settings</span></span> |<span data-ttu-id="72447-120">Gör inställningar för Windows-användare tooroam och appdata. Om du inaktiverar den här principen inaktiverar synkronisering samt säkerhetskopieringar på mobila enheter</span><span class="sxs-lookup"><span data-stu-id="72447-120">Allows users tooroam Windows settings and app data; Disabling this policy will disable sync as well as backups on mobile devices</span></span> |

## <a name="group-policy-settings"></a><span data-ttu-id="72447-121">Grupprincipinställningar</span><span class="sxs-lookup"><span data-stu-id="72447-121">Group Policy settings</span></span>
<span data-ttu-id="72447-122">hello grupprincipinställningar tillämpas tooWindows 10-enheter som är kopplade tooan Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="72447-122">hello Group Policy settings apply tooWindows 10 devices that are joined tooan Active Directory domain.</span></span> <span data-ttu-id="72447-123">hello tabellen innehåller också äldre inställningar som visas toomanage synkroniseringsinställningar, men som inte arbetar för Enterprise tillstånd Roaming för Windows 10, som är märkta med ”Använd inte” i hello beskrivning.</span><span class="sxs-lookup"><span data-stu-id="72447-123">hello table also includes legacy settings that would appear toomanage sync settings, but that do not work for Enterprise State Roaming for Windows 10, which are noted with ‘Do not use’ in hello description.</span></span>

| <span data-ttu-id="72447-124">Namn</span><span class="sxs-lookup"><span data-stu-id="72447-124">Name</span></span> | <span data-ttu-id="72447-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72447-125">Description</span></span> |
| --- | --- |
| <span data-ttu-id="72447-126">Konton: Blockera Microsoft-konton</span><span class="sxs-lookup"><span data-stu-id="72447-126">Accounts: Block Microsoft Accounts</span></span> |<span data-ttu-id="72447-127">Den här inställningen förhindrar att användare lägger till nya Microsoft-konton på den här datorn</span><span class="sxs-lookup"><span data-stu-id="72447-127">This policy setting prevents users from adding new Microsoft accounts on this computer</span></span> |
| <span data-ttu-id="72447-128">Är inte synkroniserade</span><span class="sxs-lookup"><span data-stu-id="72447-128">Do not sync</span></span> |<span data-ttu-id="72447-129">Förhindrar att användare tooroam Windows-inställningar och AppData</span><span class="sxs-lookup"><span data-stu-id="72447-129">Prevents users tooroam Windows settings and app data</span></span> |
| <span data-ttu-id="72447-130">Synkroniserar inte anpassa</span><span class="sxs-lookup"><span data-stu-id="72447-130">Do not sync personalize</span></span> |<span data-ttu-id="72447-131">Inaktiverar synkroniseringen av hello teman grupp</span><span class="sxs-lookup"><span data-stu-id="72447-131">Disables syncing of hello Themes group</span></span> |
| <span data-ttu-id="72447-132">Synkronisera inte inställningar för webbläsaren</span><span class="sxs-lookup"><span data-stu-id="72447-132">Do not sync browser settings</span></span> |<span data-ttu-id="72447-133">Inaktiverar synkroniseringen av hello Internet Explorer-grupp</span><span class="sxs-lookup"><span data-stu-id="72447-133">Disables syncing of hello Internet Explorer group</span></span> |
| <span data-ttu-id="72447-134">Synkronisera inte lösenord</span><span class="sxs-lookup"><span data-stu-id="72447-134">Do not sync passwords</span></span> |<span data-ttu-id="72447-135">Inaktiverar synkronisering av lösenord grupp</span><span class="sxs-lookup"><span data-stu-id="72447-135">Disables syncing of Passwords group</span></span> |
| <span data-ttu-id="72447-136">Synkronisera inte andra Windows-inställningar</span><span class="sxs-lookup"><span data-stu-id="72447-136">Do not sync other Windows settings</span></span> |<span data-ttu-id="72447-137">Inaktiverar synkroniseringen av gruppen för andra Windows-inställningar</span><span class="sxs-lookup"><span data-stu-id="72447-137">Disables syncing of Other Windows settings group</span></span> |
| <span data-ttu-id="72447-138">Synkronisera inte skrivbordsanpassning</span><span class="sxs-lookup"><span data-stu-id="72447-138">Do not sync desktop personalization</span></span> |<span data-ttu-id="72447-139">Använd inte; har ingen effekt</span><span class="sxs-lookup"><span data-stu-id="72447-139">Do not use; has no effect</span></span> |
| <span data-ttu-id="72447-140">Synkronisera inte i med datapriser</span><span class="sxs-lookup"><span data-stu-id="72447-140">Do not sync on metered connections</span></span> |<span data-ttu-id="72447-141">Inaktiverar roaming på förbrukade anslutningar, till exempel mobil 3 G</span><span class="sxs-lookup"><span data-stu-id="72447-141">Disables roaming on metered connections, such as cellular 3G</span></span> |
| <span data-ttu-id="72447-142">Är inte synkroniserade appar</span><span class="sxs-lookup"><span data-stu-id="72447-142">Do not sync apps</span></span> |<span data-ttu-id="72447-143">Använd inte; har ingen effekt</span><span class="sxs-lookup"><span data-stu-id="72447-143">Do not use; has no effect</span></span> |
| <span data-ttu-id="72447-144">Synkronisera inte app-inställningar</span><span class="sxs-lookup"><span data-stu-id="72447-144">Do not sync app settings</span></span> |<span data-ttu-id="72447-145">Inaktiverar roaming för AppData</span><span class="sxs-lookup"><span data-stu-id="72447-145">Disables roaming of app data</span></span> |
| <span data-ttu-id="72447-146">Synkronisera inte inställningarna för start</span><span class="sxs-lookup"><span data-stu-id="72447-146">Do not sync start settings</span></span> |<span data-ttu-id="72447-147">Använd inte; har ingen effekt</span><span class="sxs-lookup"><span data-stu-id="72447-147">Do not use; has no effect</span></span> |

## <a name="related-topics"></a><span data-ttu-id="72447-148">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="72447-148">Related topics</span></span>
* [<span data-ttu-id="72447-149">Företagsroaming</span><span class="sxs-lookup"><span data-stu-id="72447-149">Enterprise State Roaming overview</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)
* [<span data-ttu-id="72447-150">Aktivera företagsroaming i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="72447-150">Enable enterprise state roaming in Azure Active Directory</span></span>](active-directory-windows-enterprise-state-roaming-enable.md)
* [<span data-ttu-id="72447-151">Inställningar och dataroaming vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="72447-151">Settings and data roaming FAQ</span></span>](active-directory-windows-enterprise-state-roaming-faqs.md)
* [<span data-ttu-id="72447-152">Centrala referens för Windows 10</span><span class="sxs-lookup"><span data-stu-id="72447-152">Windows 10 roaming settings reference</span></span>](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [<span data-ttu-id="72447-153">Felsökning</span><span class="sxs-lookup"><span data-stu-id="72447-153">Troubleshooting</span></span>](active-directory-windows-enterprise-state-roaming-troubleshooting.md)

