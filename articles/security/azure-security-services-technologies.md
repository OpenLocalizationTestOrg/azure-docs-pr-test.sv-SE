---
title: "Säkerheten i Azure-tjänster och teknik | Microsoft Docs"
description: "Artikeln innehåller en granskad lista över Azure-säkerhet tjänster och teknik."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: yurid
ms.openlocfilehash: 0bea62a43cf6cac9132fe64f2d6c54e52def4c55
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-security-services-and-technologies"></a><span data-ttu-id="99bf4-103">Säkerheten i Azure-tjänster och teknik</span><span class="sxs-lookup"><span data-stu-id="99bf4-103">Azure Security Services and Technologies</span></span>
<span data-ttu-id="99bf4-104">I vår diskussioner med aktuella och framtida Azure-kunder uppmanas vi ofta ”har du en lista över alla relaterade tjänster och teknik som Azure har att erbjuda”?</span><span class="sxs-lookup"><span data-stu-id="99bf4-104">In our discussions with current and future Azure customers, we’re often asked “do you have a list of all the security related services and technologies that Azure has to offer?”</span></span>

<span data-ttu-id="99bf4-105">Vi förstår att när du utvärderar cloud service provider tekniska alternativen är det bra att ha en sådan lista kan användas för att sätta igång ned djupare när tiden är rätt för dig.</span><span class="sxs-lookup"><span data-stu-id="99bf4-105">We understand that when you’re evaluating your cloud service provider technical options, it’s helpful to have such a list available that you can use to dig down deeper when the time is right for you.</span></span>

<span data-ttu-id="99bf4-106">Följande är vårt första arbete med att tillhandahålla en lista.</span><span class="sxs-lookup"><span data-stu-id="99bf4-106">The following is our initial effort at providing a list.</span></span> <span data-ttu-id="99bf4-107">Över tiden, den här listan ändrar och växer, precis som Azure.</span><span class="sxs-lookup"><span data-stu-id="99bf4-107">Over time, this list will change and grow, just as Azure does.</span></span> <span data-ttu-id="99bf4-108">Kategoriseras i listan och listan över kategorier kommer också att växa med tiden.</span><span class="sxs-lookup"><span data-stu-id="99bf4-108">The list is categorized, and the list of categories will also grow over time.</span></span> <span data-ttu-id="99bf4-109">Se till att markera den här sidan med jämna mellanrum att hålla dig uppdaterad på vår säkerhetsrelaterade tjänster och teknik.</span><span class="sxs-lookup"><span data-stu-id="99bf4-109">Make sure to check this page on a regular basis to stay up-to-date on our security-related services and technologies.</span></span>

## <a name="azure-security---general"></a><span data-ttu-id="99bf4-110">Azure Security - Allmänt</span><span class="sxs-lookup"><span data-stu-id="99bf4-110">Azure Security - General</span></span>
* [<span data-ttu-id="99bf4-111">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="99bf4-111">Azure Security Center</span></span>](https://azure.microsoft.com/documentation/services/security-center/)
* [<span data-ttu-id="99bf4-112">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="99bf4-112">Azure Key Vault</span></span>](https://azure.microsoft.com/documentation/services/key-vault/)
* [<span data-ttu-id="99bf4-113">Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="99bf4-113">Azure Disk Encryption</span></span>](azure-security-disk-encryption.md)
* [<span data-ttu-id="99bf4-114">Log Analytics</span><span class="sxs-lookup"><span data-stu-id="99bf4-114">Log Analytics</span></span>](../log-analytics/log-analytics-overview.md)
* [<span data-ttu-id="99bf4-115">Utveckling och testning i Azure-labb</span><span class="sxs-lookup"><span data-stu-id="99bf4-115">Azure Dev/Test Labs</span></span>](https://azure.microsoft.com/documentation/services/devtest-lab/)

## <a name="azure-storage-security"></a><span data-ttu-id="99bf4-116">Azure Storage-säkerhet</span><span class="sxs-lookup"><span data-stu-id="99bf4-116">Azure Storage Security</span></span>
* [<span data-ttu-id="99bf4-117">Azure Storage Service-kryptering</span><span class="sxs-lookup"><span data-stu-id="99bf4-117">Azure Storage Service Encryption</span></span>](../storage/common/storage-service-encryption.md)
* [<span data-ttu-id="99bf4-118">StorSimple krypterade Hybrid lagring</span><span class="sxs-lookup"><span data-stu-id="99bf4-118">StorSimple Encrypted Hybrid Storage</span></span>](https://azure.microsoft.com/documentation/services/storsimple/)
* [<span data-ttu-id="99bf4-119">Azure klientsidan kryptering</span><span class="sxs-lookup"><span data-stu-id="99bf4-119">Azure Client-Side Encryption</span></span>](../storage/common/storage-client-side-encryption.md)
* [<span data-ttu-id="99bf4-120">Azure Storage signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="99bf4-120">Azure Storage Shared Access Signatures</span></span>](../storage/common/storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="99bf4-121">Azure Lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="99bf4-121">Azure Storage Account Keys</span></span>](../storage/common/storage-create-storage-account.md)
* [<span data-ttu-id="99bf4-122">Azure-filresurser med SMB 3.0-kryptering</span><span class="sxs-lookup"><span data-stu-id="99bf4-122">Azure File shares with SMB 3.0 Encryption</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="99bf4-123">Azure Storage Analytics</span><span class="sxs-lookup"><span data-stu-id="99bf4-123">Azure Storage Analytics</span></span>](https://msdn.microsoft.com/library/hh343270.aspx)

## <a name="azure-database-security"></a><span data-ttu-id="99bf4-124">Azure Database-säkerhet</span><span class="sxs-lookup"><span data-stu-id="99bf4-124">Azure Database Security</span></span>
* [<span data-ttu-id="99bf4-125">Azure SQL-brandvägg</span><span class="sxs-lookup"><span data-stu-id="99bf4-125">Azure SQL Firewall</span></span>](../sql-database/sql-database-firewall-configure.md)
* [<span data-ttu-id="99bf4-126">Azure SQL Cell kryptering</span><span class="sxs-lookup"><span data-stu-id="99bf4-126">Azure SQL Cell Level Encryption</span></span>](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)
* [<span data-ttu-id="99bf4-127">Kryptering för Azure SQL-anslutning</span><span class="sxs-lookup"><span data-stu-id="99bf4-127">Azure SQL Connection Encryption</span></span>](../sql-database/sql-database-control-access.md)
* [<span data-ttu-id="99bf4-128">Azure SQL-autentisering</span><span class="sxs-lookup"><span data-stu-id="99bf4-128">Azure SQL Authentication</span></span>](../sql-database/sql-database-control-access.md)
* [<span data-ttu-id="99bf4-129">Azure SQL alltid kryptering</span><span class="sxs-lookup"><span data-stu-id="99bf4-129">Azure SQL Always Encryption</span></span>](https://msdn.microsoft.com/library/mt163865.aspx)
* [<span data-ttu-id="99bf4-130">Kryptering på Azure SQL kolumn</span><span class="sxs-lookup"><span data-stu-id="99bf4-130">Azure SQL Column Level Encryption</span></span>](https://msdn.microsoft.com/library/ms179331.aspx)
* [<span data-ttu-id="99bf4-131">Azure SQL Transparent datakryptering</span><span class="sxs-lookup"><span data-stu-id="99bf4-131">Azure SQL Transparent Data Encryption</span></span>](https://msdn.microsoft.com/library/dn948096.aspx)
* [<span data-ttu-id="99bf4-132">Azure SQL Database Auditing</span><span class="sxs-lookup"><span data-stu-id="99bf4-132">Azure SQL Database Auditing</span></span>](../sql-database/sql-database-auditing.md)

## <a name="azure-identity-and-access-management"></a><span data-ttu-id="99bf4-133">Azure identitets- och åtkomsthantering</span><span class="sxs-lookup"><span data-stu-id="99bf4-133">Azure Identity and Access Management</span></span>
* [<span data-ttu-id="99bf4-134">Azure rollbaserad åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="99bf4-134">Azure Role Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
* [<span data-ttu-id="99bf4-135">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99bf4-135">Azure Active Directory</span></span>](../active-directory/active-directory-whatis.md)
* [<span data-ttu-id="99bf4-136">Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="99bf4-136">Azure Active Directory B2C</span></span>](../active-directory-b2c/active-directory-b2c-get-started.md)
* [<span data-ttu-id="99bf4-137">Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="99bf4-137">Azure Active Directory Domain Services</span></span>](../active-directory-domain-services/active-directory-ds-overview.md)
* [<span data-ttu-id="99bf4-138">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="99bf4-138">Azure Multi-Factor Authentication</span></span>](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="backup-and-disaster-recovery"></a><span data-ttu-id="99bf4-139">Säkerhetskopiering och katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="99bf4-139">Backup and Disaster Recovery</span></span>
* [<span data-ttu-id="99bf4-140">Azure Backup</span><span class="sxs-lookup"><span data-stu-id="99bf4-140">Azure Backup</span></span>](https://azure.microsoft.com/documentation/services/backup/)
* [<span data-ttu-id="99bf4-141">Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="99bf4-141">Azure Site Recovery</span></span>](https://azure.microsoft.com/documentation/services/site-recovery/)

## <a name="azure-networking"></a><span data-ttu-id="99bf4-142">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="99bf4-142">Azure Networking</span></span>
* [<span data-ttu-id="99bf4-143">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="99bf4-143">Network Security Groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="99bf4-144">Azure VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="99bf4-144">Azure VPN Gateway</span></span>](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [<span data-ttu-id="99bf4-145">Azure Programgateway</span><span class="sxs-lookup"><span data-stu-id="99bf4-145">Azure Application Gateway</span></span>](../application-gateway/application-gateway-introduction.md)
* [<span data-ttu-id="99bf4-146">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="99bf4-146">Azure Load Balancer</span></span>](../load-balancer/load-balancer-overview.md)
* [<span data-ttu-id="99bf4-147">Azure ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="99bf4-147">Azure ExpressRoute</span></span>](../expressroute/expressroute-introduction.md)
* [<span data-ttu-id="99bf4-148">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="99bf4-148">Azure Traffic Manager</span></span>](../traffic-manager/traffic-manager-overview.md)
* [<span data-ttu-id="99bf4-149">Azure Application Proxy</span><span class="sxs-lookup"><span data-stu-id="99bf4-149">Azure Application Proxy</span></span>](../active-directory/active-directory-application-proxy-enable.md)
