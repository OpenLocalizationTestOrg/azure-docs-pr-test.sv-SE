---
title: aaaEnable Transparent datakryptering i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera Transparent Data kryptering **."
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
ms.openlocfilehash: 94c6e9a1feddaa48faac6c835d416c4d131cd5c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a><span data-ttu-id="2019f-103">Aktivera Transparent datakryptering i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="2019f-103">Enable Transparent Data Encryption in Azure Security Center</span></span>
<span data-ttu-id="2019f-104">Azure Security Center kommer rekommenderar att du aktiverar Transparent Data kryptering (TDE) på SQL-databaser om TDE inte redan är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="2019f-104">Azure Security Center will recommend that you enable Transparent Data Encryption (TDE) on SQL databases if TDE is not already enabled.</span></span> <span data-ttu-id="2019f-105">TDE skyddar dina data och hjälper dig att uppfylla efterlevnadskrav genom att kryptera din databas, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan ändringar tooyour program.</span><span class="sxs-lookup"><span data-stu-id="2019f-105">TDE protects your data and helps you meet compliance requirements by encrypting your database, associated backups, and transaction log files at rest, without requiring changes tooyour application.</span></span> <span data-ttu-id="2019f-106">Det finns fler toolearn [Transparent datakryptering med Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span><span class="sxs-lookup"><span data-stu-id="2019f-106">toolearn more see [Transparent Data Encryption with Azure SQL Database](https://msdn.microsoft.com/library/dn948096).</span></span>

<span data-ttu-id="2019f-107">Den här rekommendationen gäller toohello Azure SQL-tjänsten. innehåller SQL som körs på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2019f-107">This recommendation applies toohello Azure SQL service only; doesn't include SQL running on your virtual machines.</span></span>

> [!NOTE]
> <span data-ttu-id="2019f-108">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="2019f-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="2019f-109">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="2019f-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="2019f-110">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="2019f-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="2019f-111">I hello **rekommendationer** bladet väljer **aktivera Transparent datakryptering**.</span><span class="sxs-lookup"><span data-stu-id="2019f-111">In hello **Recommendations** blade, select **Enable Transparent Data Encryption**.</span></span>
   <span data-ttu-id="2019f-112">![Aktivera transparent datakryptering][1]</span><span class="sxs-lookup"><span data-stu-id="2019f-112">![Enable Transparent Data Encryption][1]</span></span>
2. <span data-ttu-id="2019f-113">Då öppnas hello **aktivera Transparent datakryptering på SQL-databaser** bladet.</span><span class="sxs-lookup"><span data-stu-id="2019f-113">This opens hello **Enable Transparent Data Encryption on SQL databases** blade.</span></span> <span data-ttu-id="2019f-114">Välj en SQL-databas tooenable TDE på.</span><span class="sxs-lookup"><span data-stu-id="2019f-114">Select a SQL database tooenable TDE on.</span></span>
   <span data-ttu-id="2019f-115">![Välj SQL DB tooenable TDE på][2]</span><span class="sxs-lookup"><span data-stu-id="2019f-115">![Select SQL DB tooenable TDE on][2]</span></span>
3. <span data-ttu-id="2019f-116">På hello **Transparent datakryptering** bladet väljer **ON** under datakryptering och välj **spara** hello översta menyfliken av hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="2019f-116">On hello **Transparent data encryption** blade, select **ON** under Data encryption and select **Save** in hello top ribbon of hello blade.</span></span>
   <span data-ttu-id="2019f-117">![Aktivera TDE][3]</span><span class="sxs-lookup"><span data-stu-id="2019f-117">![Turn on TDE][3]</span></span>

   <span data-ttu-id="2019f-118">När TDE har aktiverats på hello valda SQL-databas hello **krypteringsstatus** ändras för**krypterade**.</span><span class="sxs-lookup"><span data-stu-id="2019f-118">Once TDE is enabled on hello selected SQL database, hello **Encryption status** will change too**Encrypted**.</span></span>    

   ![Krypteringsstatus][4]

## <a name="see-also"></a><span data-ttu-id="2019f-120">Se även</span><span class="sxs-lookup"><span data-stu-id="2019f-120">See also</span></span>
<span data-ttu-id="2019f-121">Den här artikeln visar hur hello tooimplement Security Center rekommendation ”aktivera Transparent kryptering av Data”.</span><span class="sxs-lookup"><span data-stu-id="2019f-121">This article showed you how tooimplement hello Security Center recommendation "Enable Transparent Data Encryption."</span></span> <span data-ttu-id="2019f-122">toolearn mer information om SQL TDE finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="2019f-122">toolearn more about SQL TDE, see hello following:</span></span>

* [<span data-ttu-id="2019f-123">Transparent datakryptering med Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2019f-123">Transparent Data Encryption with Azure SQL Database</span></span>](https://msdn.microsoft.com/library/dn948096)
* [<span data-ttu-id="2019f-124">Kom igång med Transparent Data kryptering (TDE)</span><span class="sxs-lookup"><span data-stu-id="2019f-124">Get started with Transparent Data Encryption (TDE)</span></span>](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

<span data-ttu-id="2019f-125">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="2019f-125">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="2019f-126">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="2019f-126">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="2019f-127">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="2019f-127">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="2019f-128">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="2019f-128">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="2019f-129">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="2019f-129">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="2019f-130">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="2019f-130">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="2019f-131">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2019f-131">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="2019f-132">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="2019f-132">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
