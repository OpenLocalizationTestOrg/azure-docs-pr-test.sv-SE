---
title: Aktivera Transparent datakryptering i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur du implementerar rekommenderar Azure Security Center ** aktivera Transparent Data kryptering **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2a2963affdbff3710ad08f86c6ed4e6304335559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a><span data-ttu-id="a527e-103">Aktivera Transparent datakryptering i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a527e-103">Enable Transparent Data Encryption in Azure Security Center</span></span>
<span data-ttu-id="a527e-104">Azure Security Center kommer rekommenderar att du aktiverar Transparent Data kryptering (TDE) på SQL-databaser om TDE inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a527e-104">Azure Security Center will recommend that you enable Transparent Data Encryption (TDE) on SQL databases if TDE is not already enabled.</span></span> <span data-ttu-id="a527e-105">TDE skyddar dina data och hjälper dig att uppfylla efterlevnadskrav genom att kryptera din databas, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan att ändra ditt program.</span><span class="sxs-lookup"><span data-stu-id="a527e-105">TDE protects your data and helps you meet compliance requirements by encrypting your database, associated backups, and transaction log files at rest, without requiring changes to your application.</span></span> <span data-ttu-id="a527e-106">Läs mer finns [Transparent datakryptering med Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span><span class="sxs-lookup"><span data-stu-id="a527e-106">To learn more see [Transparent Data Encryption with Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span></span>

<span data-ttu-id="a527e-107">Den här rekommendationen gäller Azure SQL-tjänsten. innehåller SQL som körs på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a527e-107">This recommendation applies to the Azure SQL service only; doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="a527e-108">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="a527e-108">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="a527e-109">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="a527e-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="a527e-110">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="a527e-110">Implement the recommendation</span></span>
1. <span data-ttu-id="a527e-111">I den **rekommendationer** bladet väljer **aktivera Transparent datakryptering**.</span><span class="sxs-lookup"><span data-stu-id="a527e-111">In the **Recommendations** blade, select **Enable Transparent Data Encryption**.</span></span>
   <span data-ttu-id="a527e-112">![Aktivera transparent datakryptering][1]</span><span class="sxs-lookup"><span data-stu-id="a527e-112">![Enable Transparent Data Encryption][1]</span></span>
2. <span data-ttu-id="a527e-113">Då öppnas den **aktivera Transparent datakryptering på SQL-databaser** bladet.</span><span class="sxs-lookup"><span data-stu-id="a527e-113">This opens the **Enable Transparent Data Encryption on SQL databases** blade.</span></span> <span data-ttu-id="a527e-114">Välj en SQL-databas för att aktivera TDE på.</span><span class="sxs-lookup"><span data-stu-id="a527e-114">Select a SQL database to enable TDE on.</span></span>
   <span data-ttu-id="a527e-115">![Välj SQL-databas för att aktivera TDE på][2]</span><span class="sxs-lookup"><span data-stu-id="a527e-115">![Select SQL DB to enable TDE on][2]</span></span>
3. <span data-ttu-id="a527e-116">På den **Transparent datakryptering** bladet väljer **ON** under datakryptering och välj **spara** på menyfliken överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="a527e-116">On the **Transparent data encryption** blade, select **ON** under Data encryption and select **Save** in the top ribbon of the blade.</span></span>
   <span data-ttu-id="a527e-117">![Aktivera TDE][3]</span><span class="sxs-lookup"><span data-stu-id="a527e-117">![Turn on TDE][3]</span></span>

   <span data-ttu-id="a527e-118">När TDE har aktiverats på den valda SQL-databasen i **krypteringsstatus** ändras till **krypterade**.</span><span class="sxs-lookup"><span data-stu-id="a527e-118">Once TDE is enabled on the selected SQL database, the **Encryption status** will change to **Encrypted**.</span></span>    

   ![Krypteringsstatus][4]

## <a name="see-also"></a><span data-ttu-id="a527e-120">Se även</span><span class="sxs-lookup"><span data-stu-id="a527e-120">See also</span></span>
<span data-ttu-id="a527e-121">Den här artikeln visar dig hur du implementerar Security Center-rekommendationen ”aktivera Transparent datakryptering”.</span><span class="sxs-lookup"><span data-stu-id="a527e-121">This article showed you how to implement the Security Center recommendation "Enable Transparent Data Encryption."</span></span> <span data-ttu-id="a527e-122">Mer information om SQL TDE finns i:</span><span class="sxs-lookup"><span data-stu-id="a527e-122">To learn more about SQL TDE, see the following:</span></span>

* [<span data-ttu-id="a527e-123">Transparent datakryptering med Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a527e-123">Transparent Data Encryption with Azure SQL Database</span></span>](https://msdn.microsoft.com/library/dn948096)
* [<span data-ttu-id="a527e-124">Kom igång med Transparent Data kryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="a527e-124">Get started with Transparent Data Encryption (TDE)</span></span>](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

<span data-ttu-id="a527e-125">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="a527e-125">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="a527e-126">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="a527e-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="a527e-127">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a527e-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="a527e-128">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig att övervaka hälsotillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a527e-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="a527e-129">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="a527e-129">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="a527e-130">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md): Här får du lära dig hur du övervakar dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="a527e-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how to monitor the health status of your partner solutions.</span></span>
* <span data-ttu-id="a527e-131">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a527e-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="a527e-132">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="a527e-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get the latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
