---
title: "Aktivera granskning och hotidentifiering identifiering på SQL-databaser i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur du implementerar rekommenderar Azure Security Center ** aktivera granskning och hotidentifiering identifiering på SQL-databaser **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 8f4febdaa4497fee0dc690b59cd6eaa415c5e5cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a><span data-ttu-id="9f76c-103">Aktivera granskning och hotidentifiering identifiering på SQL-databaser i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9f76c-103">Enable auditing and threat detection on SQL databases in Azure Security Center</span></span>
<span data-ttu-id="9f76c-104">Azure Security Center rekommenderar att du aktiverar granskning och hotidentifiering identifiering för alla SQL-databaser om granskning och hotidentifiering inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="9f76c-104">Azure Security Center will recommend that you turn on auditing and threat detection for all SQL databases if auditing and threat detection is not already enabled.</span></span> <span data-ttu-id="9f76c-105">Granskning och hotidentifiering identifiering kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="9f76c-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="9f76c-106">När du har aktiverat granskning kan du konfigurera Hotidentifiering inställningar och e-postmeddelanden för att ta emot säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="9f76c-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails to receive security alerts.</span></span> <span data-ttu-id="9f76c-107">Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella säkerhetshot mot databasen.</span><span class="sxs-lookup"><span data-stu-id="9f76c-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="9f76c-108">På så sätt kan du identifiera och svara på potentiella hot när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="9f76c-108">This enables you to detect and respond to potential threats as they occur.</span></span>

<span data-ttu-id="9f76c-109">Den här rekommendationen gäller Azure SQL-tjänsten. Det finns inget SQL som körs på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f76c-109">This recommendation applies to the Azure SQL service only; it doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="9f76c-110">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="9f76c-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="9f76c-111">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="9f76c-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="9f76c-112">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="9f76c-112">Implement the recommendation</span></span>
1. <span data-ttu-id="9f76c-113">I den **rekommendationer** bladet väljer **aktivera Auditing & Threat detection på SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="9f76c-113">In the **Recommendations** blade, select **Enable Auditing & Threat detection on SQL databases**.</span></span>  <span data-ttu-id="9f76c-114">Då öppnas den **aktivera Auditing & Threat detection på SQL-databaser** bladet.</span><span class="sxs-lookup"><span data-stu-id="9f76c-114">This opens the **Enable Auditing & Threat detection on SQL databases** blade.</span></span>

   ![Aktivera granskning på SQL-databaser][1]
2. <span data-ttu-id="9f76c-116">Välj en SQL-databas för att aktivera granskning på.</span><span class="sxs-lookup"><span data-stu-id="9f76c-116">Select a SQL database to enable auditing on.</span></span> <span data-ttu-id="9f76c-117">Då öppnas den **granskning och Hotidentifiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="9f76c-117">This opens the **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="9f76c-118">På den **granskning och Hotidentifiering** bladet väljer **ON** under **granskning**.</span><span class="sxs-lookup"><span data-stu-id="9f76c-118">On the **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Aktivera granskning och hotidentifiering][2]
4. <span data-ttu-id="9f76c-120">Följ stegen i [Hotidentifiering för SQL-databas på Azure-portalen](../sql-database/sql-database-threat-detection-portal.md) att aktivera och konfigurera Hotidentifiering och konfigurera en lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="9f76c-120">Follow the steps in [SQL Database Threat Detection in the Azure portal](../sql-database/sql-database-threat-detection-portal.md) to turn on and configure Threat Detection and to configure the list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="9f76c-121">Se även</span><span class="sxs-lookup"><span data-stu-id="9f76c-121">See also</span></span>
<span data-ttu-id="9f76c-122">Den här artikeln visar dig hur du implementerar Security Center-rekommendationen ”Enable Auditing & Threat detection på SQL-databaser”.</span><span class="sxs-lookup"><span data-stu-id="9f76c-122">This article showed you how to implement the Security Center recommendation "Enable Auditing & Threat detection on SQL databases."</span></span> <span data-ttu-id="9f76c-123">Mer information om att skydda din SQL-databas finns i:</span><span class="sxs-lookup"><span data-stu-id="9f76c-123">To learn more about securing your SQL database, see the following:</span></span>

* [<span data-ttu-id="9f76c-124">Säkra din SQL Database</span><span class="sxs-lookup"><span data-stu-id="9f76c-124">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="9f76c-125">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="9f76c-125">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="9f76c-126">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="9f76c-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="9f76c-127">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="9f76c-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="9f76c-128">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig att övervaka hälsotillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="9f76c-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="9f76c-129">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="9f76c-129">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="9f76c-130">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Här får du lära dig hur du övervakar dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="9f76c-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="9f76c-131">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9f76c-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="9f76c-132">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="9f76c-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
