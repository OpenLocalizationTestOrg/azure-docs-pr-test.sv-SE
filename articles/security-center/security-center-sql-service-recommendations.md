---
title: "aaaProtecting Azure SQL-tjänsten och data i Azure Security Center | Microsoft Docs"
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
ms.openlocfilehash: 75d782d3c2418f9645139e4cd6ecb7765c488f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a><span data-ttu-id="4be86-103">Skydda SQL Azure-tjänsten och data i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4be86-103">Protecting Azure SQL service and data in Azure Security Center</span></span>
<span data-ttu-id="4be86-104">Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="4be86-104">Azure Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="4be86-105">När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som guidar dig igenom hello konfigureringen av hello behövs kontroller.</span><span class="sxs-lookup"><span data-stu-id="4be86-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through hello process of configuring hello needed controls.</span></span>  <span data-ttu-id="4be86-106">Rekommendationer gäller tooAzure resurstyper: virtuella datorer (VM), nätverk, SQL och data och program.</span><span class="sxs-lookup"><span data-stu-id="4be86-106">Recommendations apply tooAzure resource types: virtual machines (VMs), networking, SQL and data, and applications.</span></span>

<span data-ttu-id="4be86-107">Den här artikeln tar rekommendationer som gäller tooAzure SQL-tjänsten och data.</span><span class="sxs-lookup"><span data-stu-id="4be86-107">This article addresses recommendations that apply tooAzure SQL service and data.</span></span> <span data-ttu-id="4be86-108">Rekommendationer center runt aktivera granskning för Azure SQL-servrar och databaser, aktivera kryptering för SQL-databaser och aktivera kryptering av Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="4be86-108">Recommendations center around enabling auditing for Azure SQL servers and databases, enabling encryption for SQL databases, and enabling encryption of your Azure storage account.</span></span>  <span data-ttu-id="4be86-109">Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga SQL-tjänsten och data rekommendationer och det var och en gör om du använder den.</span><span class="sxs-lookup"><span data-stu-id="4be86-109">Use hello table below as a reference toohelp you understand hello available SQL service and data recommendations and what each one does if you apply it.</span></span>

## <a name="available-sql-service-and-data-recommendations"></a><span data-ttu-id="4be86-110">Tillgängliga rekommendationer för SQL-tjänsten och data</span><span class="sxs-lookup"><span data-stu-id="4be86-110">Available SQL service and data recommendations</span></span>
| <span data-ttu-id="4be86-111">Rekommendation</span><span class="sxs-lookup"><span data-stu-id="4be86-111">Recommendation</span></span> | <span data-ttu-id="4be86-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4be86-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="4be86-113">Aktivera granskning och hotidentifiering på SQL-servrar</span><span class="sxs-lookup"><span data-stu-id="4be86-113">Enable auditing and threat detection on SQL servers</span></span>](security-center-enable-auditing-on-sql-servers.md) |<span data-ttu-id="4be86-114">Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-servrar (Azure SQL-tjänsten; inte omfattar SQL som körs på virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="4be86-114">Recommends that you turn on auditing and threat detection for Azure SQL servers (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="4be86-115">Aktivera granskning och hotidentifiering på SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="4be86-115">Enable auditing and threat detection on SQL databases</span></span>](security-center-enable-auditing-on-sql-databases.md) |<span data-ttu-id="4be86-116">Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-databaser (Azure SQL-tjänsten; inte omfattar SQL som körs på virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="4be86-116">Recommends that you turn on auditing and threat detection for Azure SQL databases (Azure SQL service only; doesn't include SQL running on your virtual machines).</span></span> |
| [<span data-ttu-id="4be86-117">Aktivera Transparent datakryptering på SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="4be86-117">Enable Transparent Data Encryption on SQL databases</span></span>](security-center-enable-transparent-data-encryption.md) |<span data-ttu-id="4be86-118">Rekommenderar att du aktiverar kryptering för SQL-databaser (endast Azure SQL-tjänsten).</span><span class="sxs-lookup"><span data-stu-id="4be86-118">Recommends that you enable encryption for SQL databases (Azure SQL service only).</span></span> |

## <a name="see-also"></a><span data-ttu-id="4be86-119">Se även</span><span class="sxs-lookup"><span data-stu-id="4be86-119">See also</span></span>
<span data-ttu-id="4be86-120">toolearn mer om rekommendationer som gäller tooother Azure resurstyper finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="4be86-120">toolearn more about recommendations that apply tooother Azure resource types, see hello following:</span></span>

* [<span data-ttu-id="4be86-121">Skydda dina virtuella datorer i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4be86-121">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="4be86-122">Skydda dina program i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4be86-122">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="4be86-123">Skydda ditt nätverk i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4be86-123">Protecting your network in Azure Security Center</span></span>](security-center-network-recommendations.md)

<span data-ttu-id="4be86-124">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="4be86-124">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="4be86-125">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="4be86-125">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="4be86-126">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4be86-126">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="4be86-127">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4be86-127">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
