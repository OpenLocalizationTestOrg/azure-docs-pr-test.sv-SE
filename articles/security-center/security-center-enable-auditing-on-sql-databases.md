---
title: "aaaEnable granskning och hotidentifiering identifiering på SQL-databaser i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera granskning och hotidentifiering identifiering på SQL-databaser **."
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
ms.openlocfilehash: c94140acf37cabaca3e681ba5db79d6827e7b9db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a><span data-ttu-id="7d625-103">Aktivera granskning och hotidentifiering identifiering på SQL-databaser i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="7d625-103">Enable auditing and threat detection on SQL databases in Azure Security Center</span></span>
<span data-ttu-id="7d625-104">Azure Security Center rekommenderar att du aktiverar granskning och hotidentifiering identifiering för alla SQL-databaser om granskning och hotidentifiering inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="7d625-104">Azure Security Center will recommend that you turn on auditing and threat detection for all SQL databases if auditing and threat detection is not already enabled.</span></span> <span data-ttu-id="7d625-105">Granskning och hotidentifiering identifiering kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="7d625-105">Auditing and threat detection can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

<span data-ttu-id="7d625-106">När du har aktiverat granskning kan du konfigurera Hotidentifiering inställningar och e-postmeddelanden tooreceive säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="7d625-106">Once you’ve turned on auditing you can configure Threat Detection settings and emails tooreceive security alerts.</span></span> <span data-ttu-id="7d625-107">Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="7d625-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="7d625-108">Detta kan du toodetect och svara toopotential hot när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="7d625-108">This enables you toodetect and respond toopotential threats as they occur.</span></span>

<span data-ttu-id="7d625-109">Den här rekommendationen gäller toohello Azure SQL-tjänsten. Det finns inget SQL som körs på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7d625-109">This recommendation applies toohello Azure SQL service only; it doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="7d625-110">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="7d625-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="7d625-111">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="7d625-111">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="7d625-112">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="7d625-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="7d625-113">I hello **rekommendationer** bladet väljer **aktivera Auditing & Threat detection på SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="7d625-113">In hello **Recommendations** blade, select **Enable Auditing & Threat detection on SQL databases**.</span></span>  <span data-ttu-id="7d625-114">Då öppnas hello **aktivera Auditing & Threat detection på SQL-databaser** bladet.</span><span class="sxs-lookup"><span data-stu-id="7d625-114">This opens hello **Enable Auditing & Threat detection on SQL databases** blade.</span></span>

   ![Aktivera granskning på SQL-databaser][1]
2. <span data-ttu-id="7d625-116">Välj en SQL database tooenable granskning på.</span><span class="sxs-lookup"><span data-stu-id="7d625-116">Select a SQL database tooenable auditing on.</span></span> <span data-ttu-id="7d625-117">Då öppnas hello **granskning och Hotidentifiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="7d625-117">This opens hello **Auditing & Threat Detection** blade.</span></span>

3. <span data-ttu-id="7d625-118">På hello **granskning och Hotidentifiering** bladet väljer **ON** under **granskning**.</span><span class="sxs-lookup"><span data-stu-id="7d625-118">On hello **Auditing & Threat Detection** blade, select **ON** under **Auditing**.</span></span>

   ![Aktivera granskning och hotidentifiering][2]
4. <span data-ttu-id="7d625-120">Gör så hello i [Hotidentifiering för SQL-databasen i hello Azure-portalen](../sql-database/sql-database-threat-detection-portal.md) tooturn på och konfigurera Hotidentifiering och tooconfigure hello lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7d625-120">Follow hello steps in [SQL Database Threat Detection in hello Azure portal](../sql-database/sql-database-threat-detection-portal.md) tooturn on and configure Threat Detection and tooconfigure hello list of emails that will receive security alerts upon detection of anomalous activities.</span></span>

## <a name="see-also"></a><span data-ttu-id="7d625-121">Se även</span><span class="sxs-lookup"><span data-stu-id="7d625-121">See also</span></span>
<span data-ttu-id="7d625-122">Den här artikeln visar hur hello tooimplement Security Center rekommendation ”Enable Auditing & Threat detection på SQL-databaser”.</span><span class="sxs-lookup"><span data-stu-id="7d625-122">This article showed you how tooimplement hello Security Center recommendation "Enable Auditing & Threat detection on SQL databases."</span></span> <span data-ttu-id="7d625-123">toolearn mer om att skydda din SQL-databas finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="7d625-123">toolearn more about securing your SQL database, see hello following:</span></span>

* [<span data-ttu-id="7d625-124">Säkra din SQL Database</span><span class="sxs-lookup"><span data-stu-id="7d625-124">Securing your SQL Database</span></span>](../sql-database/sql-database-security-overview.md)

<span data-ttu-id="7d625-125">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="7d625-125">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="7d625-126">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="7d625-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="7d625-127">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="7d625-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="7d625-128">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="7d625-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="7d625-129">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="7d625-129">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="7d625-130">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="7d625-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="7d625-131">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7d625-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="7d625-132">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="7d625-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
