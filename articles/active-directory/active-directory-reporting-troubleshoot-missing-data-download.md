---
title: "Felsökning: Hämtade data saknas i hello Azure Active Directory aktivitetsloggar | Microsoft Docs"
description: "Ger dig en lösning toomissing data i hämtade aktivitetsloggar för Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 027b70e6efc570f81d3c836f50ee52aaa89be71a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="i-cant-find-any-data-in-hello-azure-active-directory-activity-logs-i-have-downloaded"></a><span data-ttu-id="9a190-103">Det går inte att hitta några data i hello Azure Active Directory aktivitetsloggar jag har hämtat</span><span class="sxs-lookup"><span data-stu-id="9a190-103">I can’t find any data in hello Azure Active Directory activity logs I have downloaded</span></span>


## <a name="symptoms"></a><span data-ttu-id="9a190-104">Symtom</span><span class="sxs-lookup"><span data-stu-id="9a190-104">Symptoms</span></span>

<span data-ttu-id="9a190-105">Jag har hämtat hello aktivitetsloggar (granskning eller inloggningar) och visas inte alla hello-poster för hello gången som du har valt.</span><span class="sxs-lookup"><span data-stu-id="9a190-105">I downloaded hello activity logs (audit or sign-ins) and I don’t see all hello records for hello time I chose.</span></span> <span data-ttu-id="9a190-106">Varför?</span><span class="sxs-lookup"><span data-stu-id="9a190-106">Why?</span></span> 

 ![Rapportering](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a><span data-ttu-id="9a190-108">Orsak</span><span class="sxs-lookup"><span data-stu-id="9a190-108">Cause</span></span>

<span data-ttu-id="9a190-109">När du hämtar aktivitetsloggar i hello Azure-portalen, vi begränsa hello skala too120K poster, sorterade efter senaste.</span><span class="sxs-lookup"><span data-stu-id="9a190-109">When you download activity logs in hello Azure portal, we limit hello scale too120K records, sorted by most recent.</span></span> 

## <a name="resolution"></a><span data-ttu-id="9a190-110">Lösning</span><span class="sxs-lookup"><span data-stu-id="9a190-110">Resolution</span></span>

<span data-ttu-id="9a190-111">Du kan utnyttja [Azure AD Reporting API: erna](active-directory-reporting-api-getting-started.md) toofetch upp tooa miljoner poster vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="9a190-111">You can leverage [Azure AD Reporting APIs](active-directory-reporting-api-getting-started.md) toofetch up tooa million records at any given point.</span></span> <span data-ttu-id="9a190-112">Vår rekommendation är toorun ett skript enligt ett schema som anropar hello reporting API: er toofetch registrerar på en inkrementell sätt under en viss tidsperiod (t.ex. varje dag eller varje vecka).</span><span class="sxs-lookup"><span data-stu-id="9a190-112">Our recommended approach is toorun a script on a scheduled basis that calls hello reporting APIs toofetch records in an incremental fashion over a period of time (e.g., daily or weekly).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a190-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a190-113">Next steps</span></span>
<span data-ttu-id="9a190-114">Se hello [Azure Active Directory reporting vanliga frågor och svar](active-directory-reporting-faq.md).</span><span class="sxs-lookup"><span data-stu-id="9a190-114">See hello [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md).</span></span>

