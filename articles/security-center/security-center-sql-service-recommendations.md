---
title: "Skydda SQL Azure-tjänsten och data i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet behandlar rekommendationerna i Azure Security Center som hjälper dig skydda dina data och Azure SQL-tjänsten och vara kompatibla med säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 0c3a11e9a86767641533b16de1b96b4c59bfdf51
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a><span data-ttu-id="0cb7b-103">Skydda SQL Azure-tjänsten och data i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="0cb7b-103">Protecting Azure SQL service and data in Azure Security Center</span></span>
<span data-ttu-id="0cb7b-104">Azure Security Center analyserar säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-104">Azure Security Center analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="0cb7b-105">När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som hjälper dig att konfigurera nödvändiga kontroller.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through the process of configuring the needed controls.</span></span>  <span data-ttu-id="0cb7b-106">Rekommendationer gäller för Azure resurstyper: virtuella datorer (VM), nätverk, SQL och data och program.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-106">Recommendations apply to Azure resource types: virtual machines (VMs), networking, SQL and data, and applications.</span></span>

<span data-ttu-id="0cb7b-107">Den här artikeln tar rekommendationer som gäller för Azure SQL-tjänsten och data.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-107">This article addresses recommendations that apply to Azure SQL service and data.</span></span> <span data-ttu-id="0cb7b-108">Rekommendationer center runt aktivera granskning för Azure SQL-servrar och databaser, aktivera kryptering för SQL-databaser och aktivera kryptering av Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-108">Recommendations center around enabling auditing for Azure SQL servers and databases, enabling encryption for SQL databases, and enabling encryption of your Azure storage account.</span></span>  <span data-ttu-id="0cb7b-109">Använd tabellen nedan som referens för att hjälpa dig att förstå de tillgängliga SQL-tjänst och data rekommendationerna och det var och en gör om du använder den.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-109">Use the table below as a reference to help you understand the available SQL service and data recommendations and what each one does if you apply it.</span></span>

## <a name="available-sql-service-and-data-recommendations"></a><span data-ttu-id="0cb7b-110">Tillgängliga rekommendationer för SQL-tjänsten och data</span><span class="sxs-lookup"><span data-stu-id="0cb7b-110">Available SQL service and data recommendations</span></span>
| <span data-ttu-id="0cb7b-111">Rekommendation</span><span class="sxs-lookup"><span data-stu-id="0cb7b-111">Recommendation</span></span> | <span data-ttu-id="0cb7b-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0cb7b-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="0cb7b-113">Aktivera granskning och hotidentifiering på SQL-servrar</span><span class="sxs-lookup"><span data-stu-id="0cb7b-113">Enable auditing and threat detection on SQL servers</span></span>](security-center-enable-auditing-on-sql-servers.md) |<span data-ttu-id="0cb7b-114">Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-servrar (Azure SQL-tjänsten; inte omfattar SQL som körs på virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="0cb7b-114">Recommends that you turn on auditing and threat detection for Azure SQL servers (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="0cb7b-115">Aktivera granskning och hotidentifiering på SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="0cb7b-115">Enable auditing and threat detection on SQL databases</span></span>](security-center-enable-auditing-on-sql-databases.md) |<span data-ttu-id="0cb7b-116">Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-databaser (Azure SQL-tjänsten; inte omfattar SQL som körs på virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="0cb7b-116">Recommends that you turn on auditing and threat detection for Azure SQL databases (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="0cb7b-117">Aktivera Transparent datakryptering på SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="0cb7b-117">Enable Transparent Data Encryption on SQL databases</span></span>](security-center-enable-transparent-data-encryption.md) |<span data-ttu-id="0cb7b-118">Rekommenderar att du aktiverar kryptering för SQL-databaser (endast Azure SQL-tjänsten).</span><span class="sxs-lookup"><span data-stu-id="0cb7b-118">Recommends that you enable encryption for SQL databases (Azure SQL service only).</span></span> |

## <a name="see-also"></a><span data-ttu-id="0cb7b-119">Se även</span><span class="sxs-lookup"><span data-stu-id="0cb7b-119">See also</span></span>
<span data-ttu-id="0cb7b-120">Mer information om rekommendationer som gäller för andra typer av Azure finns i:</span><span class="sxs-lookup"><span data-stu-id="0cb7b-120">To learn more about recommendations that apply to other Azure resource types, see the following:</span></span>

* [<span data-ttu-id="0cb7b-121">Skydda dina virtuella datorer i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="0cb7b-121">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="0cb7b-122">Skydda dina program i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="0cb7b-122">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="0cb7b-123">Skydda ditt nätverk i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="0cb7b-123">Protecting your network in Azure Security Center</span></span>](security-center-network-recommendations.md)

<span data-ttu-id="0cb7b-124">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="0cb7b-124">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="0cb7b-125">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-125">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="0cb7b-126">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-126">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="0cb7b-127">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0cb7b-127">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
