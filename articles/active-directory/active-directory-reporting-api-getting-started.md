---
title: "Komma igång med Azure AD reporting API på den klassiska Azure AD-portalen | Microsoft Docs"
description: "Hur du kommer igång med Azure Active Directory reporting API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 5e98b660fe19bb8abebf1c3b996b6295a6c4e728
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api-on-the-azure-ad-classic-portal"></a><span data-ttu-id="0aacd-103">Komma igång med Azure Active Directory reporting API på den klassiska Azure AD-portalen</span><span class="sxs-lookup"><span data-stu-id="0aacd-103">Getting started with the Azure Active Directory reporting API on the Azure AD classic portal</span></span>
<span data-ttu-id="0aacd-104">*Det här avsnittet är en del av den [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span><span class="sxs-lookup"><span data-stu-id="0aacd-104">*This topic is part of the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span></span>

<span data-ttu-id="0aacd-105">Azure Active Directory innehåller en mängd olika rapporter.</span><span class="sxs-lookup"><span data-stu-id="0aacd-105">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="0aacd-106">Data för de här rapporterna kan vara användbara för dina program, till exempel SIEM-system, gransknings- och business intelligence-verktyg.</span><span class="sxs-lookup"><span data-stu-id="0aacd-106">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="0aacd-107">Azure AD reporting API: er ger programmässig åtkomst till data via en uppsättning REST-baserade API: er.</span><span class="sxs-lookup"><span data-stu-id="0aacd-107">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="0aacd-108">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="0aacd-108">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="0aacd-109">Den här artikeln ger dig den information du behöver att komma igång med Azure AD reporting API: er.</span><span class="sxs-lookup"><span data-stu-id="0aacd-109">This article provides you with the information you need to get started with the Azure AD reporting APIs.</span></span>
<span data-ttu-id="0aacd-110">I nästa avsnitt du hittar mer information om hur du använder revisionen och logga in API: er.</span><span class="sxs-lookup"><span data-stu-id="0aacd-110">In the next section, you find more details about using the audit and sign-in APIs.</span></span> <span data-ttu-id="0aacd-111">Alla andra API: er, finns det [Azure AD-rapporter och events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) artikel.</span><span class="sxs-lookup"><span data-stu-id="0aacd-111">For all other APIs, see the [Azure AD reports and events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) article.</span></span>

<span data-ttu-id="0aacd-112">För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="0aacd-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="learning-map"></a><span data-ttu-id="0aacd-113">Inlärningskarta</span><span class="sxs-lookup"><span data-stu-id="0aacd-113">Learning map</span></span>
1. <span data-ttu-id="0aacd-114">**Förbereda** -innan du kan testa din API samples, måste du slutföra de [krav för att få åtkomst till Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="0aacd-114">**Prepare** - Before you can test your API samples, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>
2. <span data-ttu-id="0aacd-115">**Utforska** -få en första intryck reporting API:</span><span class="sxs-lookup"><span data-stu-id="0aacd-115">**Explore** - Get a first impression of the reporting APIs:</span></span>
   
   * [<span data-ttu-id="0aacd-116">Använda exemplen för granskning API</span><span class="sxs-lookup"><span data-stu-id="0aacd-116">Using the samples for the audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="0aacd-117">Använda exemplen för rapporten inloggningsaktivitet API</span><span class="sxs-lookup"><span data-stu-id="0aacd-117">Using the samples for the sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="0aacd-118">**Anpassa** -skapa din egen lösning:</span><span class="sxs-lookup"><span data-stu-id="0aacd-118">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="0aacd-119">Med hjälp av audit API-referens</span><span class="sxs-lookup"><span data-stu-id="0aacd-119">Using the audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="0aacd-120">Med hjälp av rapporten API-referens inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="0aacd-120">Using the sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="0aacd-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0aacd-121">Next Steps</span></span>
<span data-ttu-id="0aacd-122">Om du är nyfiken på att se alla tillgängliga Azure AD Graph API-slutpunkter genom att gå till [https://graph.windows.net/tenant-name/reports/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="0aacd-122">If you are curious to see all of the available Azure AD Graph API endpoints by navigating to [https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span></span>

