---
title: "aaaEnable granskning och hotidentifiering identifiering på SQL-servrar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera Auditing & Threat detection på SQL-servrar **."
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
ms.openlocfilehash: b082c48cdbc386f14e677f4e13a7f306f37fd0e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a><span data-ttu-id="58c72-103">Aktivera granskning och hotidentifiering identifiering på SQL-servrar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="58c72-103">Enable auditing and threat detection on SQL servers in Azure Security Center</span></span>
<span data-ttu-id="58c72-104">Azure Security Center rekommenderar att du aktiverar granskning och hotidentifiering för alla databaser på Azure SQL-servrar om granskning inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="58c72-104">Azure Security Center will recommend that you turn on auditing and threat detection for all databases on your Azure SQL servers if auditing is not already enabled.</span></span> <span data-ttu-id="58c72-105">Granskning och hotidentifiering identifiering kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="58c72-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="58c72-106">När du har aktiverat granskning kan du konfigurera Hotidentifiering inställningar och e-postmeddelanden tooreceive säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="58c72-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails tooreceive security alerts.</span></span> <span data-ttu-id="58c72-107">Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="58c72-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="58c72-108">Detta kan du toodetect och svara toopotential hot när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="58c72-108">This enables you toodetect and respond toopotential threats as they occur.</span></span>

<span data-ttu-id="58c72-109">Den här rekommendationen gäller toohello Azure SQL-tjänsten. Det innehåller inte SQL Server på virtuella datorer i Azure Infrastructure Services (Azure IaaS).</span><span class="sxs-lookup"><span data-stu-id="58c72-109">This recommendation applies toohello Azure SQL service only; it doesn’t include SQL Server running on your virtual machines in Azure Infrastructure Services (Azure IaaS).</span></span>

> [!NOTE]
> <span data-ttu-id="58c72-110">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="58c72-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="58c72-111">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="58c72-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="58c72-112">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="58c72-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="58c72-113">I hello **rekommendationer** bladet väljer **aktivera Auditing & Threat detection på SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="58c72-113">In hello **Recommendations** blade, select **Enable Auditing & Threat detection on SQL servers**.</span></span>  <span data-ttu-id="58c72-114">Då öppnas hello **aktivera Auditing & Threat detection på SQL-servrar** bladet.</span><span class="sxs-lookup"><span data-stu-id="58c72-114">This opens hello **Enable Auditing & Threat detection on SQL servers** blade.</span></span>

   ![Aktivera granskning på SQL-servrar][1]
2. <span data-ttu-id="58c72-116">Välj en SQL server tooenable granskning och hotidentifiering identifiering på.</span><span class="sxs-lookup"><span data-stu-id="58c72-116">Select a SQL server tooenable auditing and threat detection on.</span></span> <span data-ttu-id="58c72-117">Då öppnas hello **granskning och Hotidentifiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="58c72-117">This opens hello **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="58c72-118">På hello **granskning och Hotidentifiering** bladet väljer **ON** under **granskning**.</span><span class="sxs-lookup"><span data-stu-id="58c72-118">On hello **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Aktivera granskningsinställningar][2]
4. <span data-ttu-id="58c72-120">Gör så hello i [SQL database auditing i hello Azure-portalen](../sql-database/sql-database-auditing-portal.md) tooconfigure lagring där din granskningsloggar ska lagras.</span><span class="sxs-lookup"><span data-stu-id="58c72-120">Follow hello steps in [SQL database auditing in hello Azure portal](../sql-database/sql-database-auditing-portal.md) tooconfigure storage where your audit logs will be stored.</span></span> <span data-ttu-id="58c72-121">hello är prenumerationens lagringskonto för datainsamling hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="58c72-121">hello subscription's storage account for data collection is hello default storage account.</span></span>
5. <span data-ttu-id="58c72-122">Gör så hello i [Kom igång med SQL-databasen Hotidentifiering](../sql-database/sql-database-threat-detection.md) tooturn på och konfigurera Hotidentifiering och tooconfigure hello lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="58c72-122">Follow hello steps in [Get started with SQL Database Threat Detection](../sql-database/sql-database-threat-detection.md) tooturn on and configure Threat Detection and tooconfigure hello list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="58c72-123">Se även</span><span class="sxs-lookup"><span data-stu-id="58c72-123">See also</span></span>
<span data-ttu-id="58c72-124">Den här artikeln visar hur tooimplement hello Security Center rekommendation ”aktivera granskning och hotidentifiering på SQL-servrar”.</span><span class="sxs-lookup"><span data-stu-id="58c72-124">This article showed you how tooimplement hello Security Center recommendation "Enable auditing and threat detection on SQL servers."</span></span> <span data-ttu-id="58c72-125">toolearn mer om att skydda din SQL-databas finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="58c72-125">toolearn more about securing your SQL database, see hello following:</span></span>

* [<span data-ttu-id="58c72-126">Säkra din SQL Database</span><span class="sxs-lookup"><span data-stu-id="58c72-126">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="58c72-127">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="58c72-127">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="58c72-128">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="58c72-128">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="58c72-129">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="58c72-129">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="58c72-130">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="58c72-130">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="58c72-131">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="58c72-131">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="58c72-132">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="58c72-132">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="58c72-133">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="58c72-133">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="58c72-134">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="58c72-134">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
