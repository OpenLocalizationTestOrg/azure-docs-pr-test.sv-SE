---
title: "Aktivera granskning och hotidentifiering identifiering på SQL-servrar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur du implementerar rekommenderar Azure Security Center ** aktivera Auditing & Threat detection på SQL-servrar **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 660b537aef8d175a478ff93d60b8391d55fc92ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a><span data-ttu-id="6ffe1-103">Aktivera granskning och hotidentifiering identifiering på SQL-servrar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="6ffe1-103">Enable auditing and threat detection on SQL servers in Azure Security Center</span></span>
<span data-ttu-id="6ffe1-104">Azure Security Center rekommenderar att du aktiverar granskning och hotidentifiering för alla databaser på Azure SQL-servrar om granskning inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-104">Azure Security Center will recommend that you turn on auditing and threat detection for all databases on your Azure SQL servers if auditing is not already enabled.</span></span> <span data-ttu-id="6ffe1-105">Granskning och hotidentifiering identifiering kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="6ffe1-106">När du har aktiverat granskning kan du konfigurera Hotidentifiering inställningar och e-postmeddelanden för att ta emot säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails to receive security alerts.</span></span> <span data-ttu-id="6ffe1-107">Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella säkerhetshot mot databasen.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="6ffe1-108">På så sätt kan du identifiera och svara på potentiella hot när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-108">This enables you to detect and respond to potential threats as they occur.</span></span>

<span data-ttu-id="6ffe1-109">Den här rekommendationen gäller Azure SQL-tjänsten. Det innehåller inte SQL Server på virtuella datorer i Azure Infrastructure Services (Azure IaaS).</span><span class="sxs-lookup"><span data-stu-id="6ffe1-109">This recommendation applies to the Azure SQL service only; it doesn’t include SQL Server running on your virtual machines in Azure Infrastructure Services (Azure IaaS).</span></span>

> [!NOTE]
> <span data-ttu-id="6ffe1-110">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="6ffe1-111">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="6ffe1-112">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="6ffe1-112">Implement the recommendation</span></span>
1. <span data-ttu-id="6ffe1-113">I den **rekommendationer** bladet väljer **aktivera Auditing & Threat detection på SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-113">In the **Recommendations** blade, select **Enable Auditing & Threat detection on SQL servers**.</span></span>  <span data-ttu-id="6ffe1-114">Då öppnas den **aktivera Auditing & Threat detection på SQL-servrar** bladet.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-114">This opens the **Enable Auditing & Threat detection on SQL servers** blade.</span></span>

   ![Aktivera granskning på SQL-servrar][1]
2. <span data-ttu-id="6ffe1-116">Välj en SQL-server för att aktivera granskning och hotidentifiering på.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-116">Select a SQL server to enable auditing and threat detection on.</span></span> <span data-ttu-id="6ffe1-117">Då öppnas den **granskning och Hotidentifiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-117">This opens the **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="6ffe1-118">På den **granskning och Hotidentifiering** bladet väljer **ON** under **granskning**.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-118">On the **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Aktivera granskningsinställningar][2]
4. <span data-ttu-id="6ffe1-120">Följ stegen i [SQL database auditing i Azure portal](../sql-database/sql-database-auditing-portal.md) du konfigurerar lagring där din granskningsloggar ska lagras.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-120">Follow the steps in [SQL database auditing in the Azure portal](../sql-database/sql-database-auditing-portal.md) to configure storage where your audit logs will be stored.</span></span> <span data-ttu-id="6ffe1-121">Prenumerationens lagringskonto för datainsamling är standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-121">The subscription's storage account for data collection is the default storage account.</span></span>
5. <span data-ttu-id="6ffe1-122">Följ stegen i [Kom igång med SQL-databasen Hotidentifiering](../sql-database/sql-database-threat-detection.md) att aktivera och konfigurera Hotidentifiering och konfigurera en lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-122">Follow the steps in [Get started with SQL Database Threat Detection](../sql-database/sql-database-threat-detection.md) to turn on and configure Threat Detection and to configure the list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="6ffe1-123">Se även</span><span class="sxs-lookup"><span data-stu-id="6ffe1-123">See also</span></span>
<span data-ttu-id="6ffe1-124">Den här artikeln visar dig hur du implementerar Security Center rekommendation ”aktivera granskning och hotidentifiering identifieringen på SQL-servrar”.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-124">This article showed you how to implement the Security Center recommendation "Enable auditing and threat detection on SQL servers."</span></span> <span data-ttu-id="6ffe1-125">Mer information om att skydda din SQL-databas finns i:</span><span class="sxs-lookup"><span data-stu-id="6ffe1-125">To learn more about securing your SQL database, see the following:</span></span>

* [<span data-ttu-id="6ffe1-126">Säkra din SQL Database</span><span class="sxs-lookup"><span data-stu-id="6ffe1-126">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="6ffe1-127">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="6ffe1-127">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="6ffe1-128">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="6ffe1-129">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="6ffe1-130">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig att övervaka hälsotillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="6ffe1-131">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-131">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="6ffe1-132">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Här får du lära dig hur du övervakar dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="6ffe1-133">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="6ffe1-134">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="6ffe1-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
