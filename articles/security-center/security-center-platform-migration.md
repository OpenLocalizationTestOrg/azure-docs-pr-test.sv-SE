---
title: Migrering av Azure Security Center-plattformen | Microsoft Docs
description: "I det här dokumentet beskrivs några ändringar av hur Azure Security Center-data samlas in."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: yurid
ms.openlocfilehash: 5ddf71dcd9c5a2b03e3b1441d8c9b4d91b6bad12
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-security-center-platform-migration"></a><span data-ttu-id="b065d-103">Migrering av Azure Security Center-plattformen</span><span class="sxs-lookup"><span data-stu-id="b065d-103">Azure Security Center platform migration</span></span>

<span data-ttu-id="b065d-104">Från och med början av juni 2017 har det gjorts viktiga ändringar av hur säkerhetsdata samlas in och lagras i Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="b065d-104">Beginning in early June 2017, Azure Security Center rolls out important changes to the way security data is collected and stored.</span></span>  <span data-ttu-id="b065d-105">De här ändringarna möjliggör nya funktioner, som att enkelt kunna söka säkerhetsdata, och insamlingen ligger nu mer i linje med andra tjänster för hantering och övervakning i Azure.</span><span class="sxs-lookup"><span data-stu-id="b065d-105">These changes unlock new capabilities like the ability to easily search security data and better aligns with other Azure management and monitoring services.</span></span>

> [!NOTE]
> <span data-ttu-id="b065d-106">Plattformsmigreringen bör inte påverka dina produktionsresurser och ingen åtgärd krävs från din sida.</span><span class="sxs-lookup"><span data-stu-id="b065d-106">The platform migration should not impact your production resources, and no action is necessary from your side.</span></span>


## <a name="whats-happening-during-this-platform-migration"></a><span data-ttu-id="b065d-107">Vad händer under den här plattformsmigreringen?</span><span class="sxs-lookup"><span data-stu-id="b065d-107">What’s happening during this platform migration?</span></span>

<span data-ttu-id="b065d-108">Tidigare användes Azure Monitoring Agent till att samla in säkerhetsdata från dina virtuella datorer i Security Center.</span><span class="sxs-lookup"><span data-stu-id="b065d-108">Previously, Security Center used the Azure Monitoring Agent to collect security data from your VMs.</span></span> <span data-ttu-id="b065d-109">Det här är bland annat information om säkerhetskonfigurationer som används till att identifiera säkerhetsproblem, och säkerhetshändelser som används till att identifiera hot.</span><span class="sxs-lookup"><span data-stu-id="b065d-109">This includes information about security configurations, which are used to identify vulnerabilities, and security events, which are used to detect threats.</span></span> <span data-ttu-id="b065d-110">Dessa data lagrades i dina lagringskonton i Azure.</span><span class="sxs-lookup"><span data-stu-id="b065d-110">This data was stored in your Storage account(s) in Azure.</span></span>

<span data-ttu-id="b065d-111">Från och med nu används Microsoft Monitoring Agent i Security Center (samma agent används av Operations Management Suite och Log Analytics-tjänsten).</span><span class="sxs-lookup"><span data-stu-id="b065d-111">Going forward, Security Center uses the Microsoft Monitoring Agent – this is the same agent used by the Operations Management Suite and Log Analytics service.</span></span> <span data-ttu-id="b065d-112">Data som samlas in från agenten lagras antingen i en befintlig *Log Analytics*-[arbetsyta](../log-analytics/log-analytics-manage-access.md) som är associerad med din Azure-prenumeration eller i en ny arbetsyta med hänsyn till den virtuella datorns geografiska plats.</span><span class="sxs-lookup"><span data-stu-id="b065d-112">Data collected from this agent is stored in either an existing *Log Analytics* [workspace](../log-analytics/log-analytics-manage-access.md) associated with your Azure subscription or a new workspace(s), taking into account the geolocation of the VM.</span></span>

## <a name="agent"></a><span data-ttu-id="b065d-113">Agent</span><span class="sxs-lookup"><span data-stu-id="b065d-113">Agent</span></span>

<span data-ttu-id="b065d-114">Som en del av övergången installeras Microsoft Monitoring Agent (för [Windows](../log-analytics/log-analytics-windows-agents.md) eller [Linux](../log-analytics/log-analytics-linux-agents.md)) på alla virtuella Azure-datorer som data samlas in från.</span><span class="sxs-lookup"><span data-stu-id="b065d-114">As part of the transition, the Microsoft Monitoring Agent (for [Windows](../log-analytics/log-analytics-windows-agents.md) or [Linux](../log-analytics/log-analytics-linux-agents.md)) is installed on all Azure VMs from which data is currently being collected.</span></span>  <span data-ttu-id="b065d-115">Om den virtuella datorn redan har Microsoft Monitoring Agent installerad används den installerade agenten i Security Center.</span><span class="sxs-lookup"><span data-stu-id="b065d-115">If the VM already has the Microsoft Monitoring Agent installed, Security Center leverages the current installed agent.</span></span>

<span data-ttu-id="b065d-116">Under en tid (vanligtvis några dagar) körs båda agenterna sida vid sida för att säkerställa en smidig övergång utan att data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="b065d-116">For a period of time (typically a few days), both agents will run side by side to ensure a smooth transition without any loss of data.</span></span> <span data-ttu-id="b065d-117">På så sätt kan Microsoft verifiera att den nya pipelinen för data fungerar innan den nuvarande pipelinen avslutas.</span><span class="sxs-lookup"><span data-stu-id="b065d-117">This will enable Microsoft to validate that the new data pipeline is operational before discontinuing use of the current pipeline.</span></span> <span data-ttu-id="b065d-118">Därefter tas Azure Monitoring Agent bort från dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b065d-118">Once verified, the Azure Monitoring Agent will be removed from your VMs.</span></span> <span data-ttu-id="b065d-119">Ingen åtgärd krävs från din sida.</span><span class="sxs-lookup"><span data-stu-id="b065d-119">No work is required on your part.</span></span> <span data-ttu-id="b065d-120">Du får ett e-postmeddelande när alla kunder har migrerats.</span><span class="sxs-lookup"><span data-stu-id="b065d-120">An email will notify you when all customers have been migrated.</span></span>
 
<span data-ttu-id="b065d-121">Du bör inte avinstallera Azure Monitoring Agent manuellt under migreringen eftersom det kan uppstå luckor i säkerhetsdata.</span><span class="sxs-lookup"><span data-stu-id="b065d-121">It is not recommended that you manually uninstall the Azure Monitoring Agent during the migration as gaps in security data could result.</span></span> <span data-ttu-id="b065d-122">Kontakta [Microsofts kundservice och support](https://support.microsoft.com/contactus/) om du behöver mer hjälp.</span><span class="sxs-lookup"><span data-stu-id="b065d-122">Please consult [Microsoft Customer Service and Support](https://support.microsoft.com/contactus/) if you need further assistance.</span></span> 

<span data-ttu-id="b065d-123">För Microsoft Monitoring Agent för Windows krävs användning av TCP-port 443, läs mer i [felsökningsguiden för Azure Security Center](security-center-troubleshooting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="b065d-123">The Microsoft Monitoring Agent for Windows requires use TCP port 443, read [Azure Security Center Troubleshooting Guide](security-center-troubleshooting-guide.md) for more information.</span></span>


> [!NOTE] 
> <span data-ttu-id="b065d-124">Eftersom Microsoft Monitoring Agent kan användas av andra tjänster för hantering och övervakning i Azure avinstalleras agenten inte automatiskt när du stänger av datainsamling i Security Center.</span><span class="sxs-lookup"><span data-stu-id="b065d-124">Because the Microsoft Monitoring Agent may be used by other Azure management and monitoring services, the agent will not be uninstalled automatically when you turn off data collection in Security Center.</span></span> <span data-ttu-id="b065d-125">Du kan däremot avinstallera agenten manuellt om det behövs.</span><span class="sxs-lookup"><span data-stu-id="b065d-125">However, you can manually uninstall the agent if needed.</span></span>

## <a name="workspace"></a><span data-ttu-id="b065d-126">Arbetsyta</span><span class="sxs-lookup"><span data-stu-id="b065d-126">Workspace</span></span>

<span data-ttu-id="b065d-127">Så som beskrivits tidigare lagras data som samlas in från Microsoft Monitoring Agent (för Security Center) antingen i befintliga Log Analytics-arbetsytor som är associerade med din Azure-prenumeration eller i nya arbetsytor med hänsyn till den virtuella datorns geografiska plats.</span><span class="sxs-lookup"><span data-stu-id="b065d-127">As described previously, data collected from the Microsoft Monitoring Agent (on behalf of Security Center) are stored in either an existing Log Analytics workspace(s) associated with your Azure subscription or a new workspace(s), taking into account the geolocation of the VM.</span></span>

<span data-ttu-id="b065d-128">Du kan bläddra om du vill se en lista med dina Log Analytics-arbetsytor, inklusive alla som skapats av Security Center i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b065d-128">In the Azure portal, you can browse to see a list of your Log Analytics workspaces, including any created by Security Center.</span></span> <span data-ttu-id="b065d-129">En relaterad resursgrupp skapas för nya arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="b065d-129">A related resource group will be created for new workspaces.</span></span> <span data-ttu-id="b065d-130">Både har följande namnkonvention:</span><span class="sxs-lookup"><span data-stu-id="b065d-130">Both follow this naming convention:</span></span>

- <span data-ttu-id="b065d-131">Arbetsyta: *DefaultWorkspace-[prenumerations-id]-[geo]*</span><span class="sxs-lookup"><span data-stu-id="b065d-131">Workspace: *DefaultWorkspace-[subscription-ID]-[geo]*</span></span>
- <span data-ttu-id="b065d-132">Resursgrupp: *DefaultResourceGroup-[geo]*</span><span class="sxs-lookup"><span data-stu-id="b065d-132">Resource Group: *DefaultResouceGroup-[geo]*</span></span> 
 
<span data-ttu-id="b065d-133">För arbetsytor som skapats av Security Center sparas data i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="b065d-133">For workspaces created by Security Center, data is retained for 30 days.</span></span> <span data-ttu-id="b065d-134">För befintliga arbetsytor baseras kvarhållningen på arbetsytans prisnivå.</span><span class="sxs-lookup"><span data-stu-id="b065d-134">For existing workspaces, retention is based on the workspace pricing tier.</span></span>

> [!NOTE]
> <span data-ttu-id="b065d-135">Data som tidigare samlats in av Security Center ligger kvar i dina lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="b065d-135">Data previously collected by Security Center remains in your Storage account(s).</span></span> <span data-ttu-id="b065d-136">När migreringen är klar kan du ta bort dessa lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="b065d-136">After the migration is complete, you can delete these Storage accounts.</span></span>

### <a name="oms-security-solution"></a><span data-ttu-id="b065d-137">OMS-säkerhetslösning</span><span class="sxs-lookup"><span data-stu-id="b065d-137">OMS Security Solution</span></span> 

<span data-ttu-id="b065d-138">För befintliga kunder som inte har OMS-säkerhetslösningen installerad så installerar Microsoft den på arbetsytan, men endast med virtuella Azure-datorer som mål.</span><span class="sxs-lookup"><span data-stu-id="b065d-138">For existing customers that don’t have OMS Security solution installed, Microsoft is installing it on their workspace, but targeting only Azure VMs.</span></span> <span data-ttu-id="b065d-139">Avinstallera inte den här lösningen, eftersom det inte finns något automatiskt sätt att åtgärda detta om du gör det från OMS-hanteringskonsolen.</span><span class="sxs-lookup"><span data-stu-id="b065d-139">Do not uninstall this solution, as there is no automatic remediation if this is done from OMS management console.</span></span>


## <a name="other-updates"></a><span data-ttu-id="b065d-140">Övriga uppdateringar</span><span class="sxs-lookup"><span data-stu-id="b065d-140">Other updates</span></span>

<span data-ttu-id="b065d-141">Tillsammans med plattformsmigreringen lanserar vi några ytterligare mindre uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="b065d-141">In conjunction with the platform migration, we are rolling out some additional minor updates:</span></span>

- <span data-ttu-id="b065d-142">Fler OS-versioner stöds.</span><span class="sxs-lookup"><span data-stu-id="b065d-142">Additional OS versions will be supported.</span></span> <span data-ttu-id="b065d-143">Se listan [här](security-center-faq.md#virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="b065d-143">See the list [here](security-center-faq.md#virtual-machines).</span></span>
- <span data-ttu-id="b065d-144">Listan med OS-säkerhetsproblem utökas.</span><span class="sxs-lookup"><span data-stu-id="b065d-144">The list of OS vulnerabilities will be expanded.</span></span> <span data-ttu-id="b065d-145">Se listan [här](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span><span class="sxs-lookup"><span data-stu-id="b065d-145">See the list [here](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).</span></span>
- <span data-ttu-id="b065d-146">[Prissättningen](https://azure.microsoft.com/pricing/details/security-center/) kommer att räknas om per timme (tidigare var det per dag), vilket leder till kostnadsbesparingar för vissa kunder.</span><span class="sxs-lookup"><span data-stu-id="b065d-146">[Pricing](https://azure.microsoft.com/pricing/details/security-center/) will be pro-rated hourly (previously it was daily), which will result in cost savings for some customers.</span></span>
- <span data-ttu-id="b065d-147">Insamling av data kommer att krävas och aktiveras automatiskt för kunder på prisnivån Standard.</span><span class="sxs-lookup"><span data-stu-id="b065d-147">Data Collection will be required and automatically enabled for customers on the Standard pricing tier.</span></span>
- <span data-ttu-id="b065d-148">Azure Security Center kommer att börja identifiera lösningar mot skadlig kod som inte har distribuerats via Azure-tillägg.</span><span class="sxs-lookup"><span data-stu-id="b065d-148">Azure Security Center will begin discovering antimalware solutions that were not deployed via Azure extensions.</span></span> <span data-ttu-id="b065d-149">Identifiering av Symantec Endpoint Protection och Defender för Windows 2016 blir tillgänglig först.</span><span class="sxs-lookup"><span data-stu-id="b065d-149">Discovery of Symantec Endpoint Protection and Defender for Windows 2016 will be available first.</span></span>
- <span data-ttu-id="b065d-150">Förebyggandeprinciper och -meddelanden kan endast konfigureras på *prenumerationsnivå*, men priser kan fortfarande anges på *resursgruppsnivå*l</span><span class="sxs-lookup"><span data-stu-id="b065d-150">Prevention policies and notifications are only configurable at the *Subscription* level, but pricing can still be set at the *Resource Group* level</span></span>

