---
title: "Felsökning: Saknade data i aktivitetsloggen för Azure Active Directory | Microsoft Docs"
description: "Visar de olika tillgängliga rapporterna i Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 47617f8f727027de113a0f503308c8accc58859e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a><span data-ttu-id="03aef-103">Det går inte att hitta vissa åtgärder som jag utförde i aktivitetsloggen för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="03aef-103">I can’t find some actions that I performed in the Azure Active Directory activity log</span></span>


## <a name="symptoms"></a><span data-ttu-id="03aef-104">Symtom</span><span class="sxs-lookup"><span data-stu-id="03aef-104">Symptoms</span></span>

<span data-ttu-id="03aef-105">Jag utförde vissa åtgärder i Azure Portal och förväntade att se granskningsloggarna för dessa åtgärder på bladet `Activity logs > Audit Logs`, men det går inte att hitta dem.</span><span class="sxs-lookup"><span data-stu-id="03aef-105">I performed some actions in the Azure portal and expected to see the audit logs for those actions in the `Activity logs > Audit Logs` blade, but I can’t find them.</span></span>

 ![Rapportering](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a><span data-ttu-id="03aef-107">Orsak</span><span class="sxs-lookup"><span data-stu-id="03aef-107">Cause</span></span>

<span data-ttu-id="03aef-108">Åtgärder visas inte direkt i granskningsloggen för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="03aef-108">Actions don’t appear immediately in the Activity Audit log.</span></span> <span data-ttu-id="03aef-109">Det kan ta någonstans mellan 15 minuter och en timme att visa granskningsloggarna i portalen från den tidpunkt då åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="03aef-109">It can take anywhere from 15 minutes to an hour to see the audit logs in the portal from the time the operation is performed.</span></span>

## <a name="resolution"></a><span data-ttu-id="03aef-110">Lösning</span><span class="sxs-lookup"><span data-stu-id="03aef-110">Resolution</span></span>

<span data-ttu-id="03aef-111">Vänta i mellan 15 minuter och en timme och se om åtgärderna visas i loggen.</span><span class="sxs-lookup"><span data-stu-id="03aef-111">Wait for 15 minutes to an hour and see if the actions appear in the log.</span></span> <span data-ttu-id="03aef-112">Om du fortfarande inte ser dem skapar du ett supportärende hos oss så tittar vi på det.</span><span class="sxs-lookup"><span data-stu-id="03aef-112">If you still don’t see them, please raise a support ticket with us and we will look into it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="03aef-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03aef-113">Next steps</span></span>
<span data-ttu-id="03aef-114">Se guiden med [vanliga frågor om Azure Active Directory-rapportering](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="03aef-114">See the [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

