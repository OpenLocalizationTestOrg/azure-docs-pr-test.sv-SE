---
title: "Komma igång med Azure AD reporting API | Microsoft Docs"
description: "Hur du kommer igång med Azure Active Directory reporting API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9944cbd2b1b7c4acb18d37da1394c0bbc170f77d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a><span data-ttu-id="cb023-103">Komma igång med Azure Active Directory reporting API</span><span class="sxs-lookup"><span data-stu-id="cb023-103">Getting started with the Azure Active Directory reporting API</span></span>

<span data-ttu-id="cb023-104">Azure Active Directory innehåller en mängd olika rapporter.</span><span class="sxs-lookup"><span data-stu-id="cb023-104">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="cb023-105">Data för de här rapporterna kan vara användbara för dina program, till exempel SIEM-system, gransknings- och business intelligence-verktyg.</span><span class="sxs-lookup"><span data-stu-id="cb023-105">The data of these reports can be very useful to your applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="cb023-106">Azure AD reporting API: er ger programmässig åtkomst till data via en uppsättning REST-baserade API: er.</span><span class="sxs-lookup"><span data-stu-id="cb023-106">The Azure AD reporting APIs provide programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="cb023-107">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="cb023-107">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="cb023-108">Den här artikeln ger dig den information du behöver att komma igång med Azure AD reporting API: er.</span><span class="sxs-lookup"><span data-stu-id="cb023-108">This article provides you with the information you need to get started with the Azure AD reporting APIs.</span></span>
<span data-ttu-id="cb023-109">I nästa avsnitt du hittar mer information om hur du använder revisionen och logga in API: er.</span><span class="sxs-lookup"><span data-stu-id="cb023-109">In the next section, you find more details about using the audit and sign-in APIs.</span></span> 

<span data-ttu-id="cb023-110">Vanliga frågor och svar, Läs vår [vanliga frågor och svar](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span><span class="sxs-lookup"><span data-stu-id="cb023-110">For frequently asked questions,read our [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span></span> <span data-ttu-id="cb023-111">Problem, ta [filen ett supportärende](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span><span class="sxs-lookup"><span data-stu-id="cb023-111">For issues, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span></span>

## <a name="learning-map"></a><span data-ttu-id="cb023-112">Inlärningskarta</span><span class="sxs-lookup"><span data-stu-id="cb023-112">Learning map</span></span>
1. <span data-ttu-id="cb023-113">**Förbereda** -innan du kan testa din API samples, måste du slutföra de [krav för att få åtkomst till Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cb023-113">**Prepare** - Before you can test your API samples, you need to complete the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span></span>
2. <span data-ttu-id="cb023-114">**Utforska** -få en första intryck reporting API:</span><span class="sxs-lookup"><span data-stu-id="cb023-114">**Explore** - Get a first impression of the reporting APIs:</span></span>
   
   * [<span data-ttu-id="cb023-115">Använda exemplen för granskning API</span><span class="sxs-lookup"><span data-stu-id="cb023-115">Using the samples for the audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="cb023-116">Använda exemplen för rapporten inloggningsaktivitet API</span><span class="sxs-lookup"><span data-stu-id="cb023-116">Using the samples for the sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="cb023-117">**Anpassa** -skapa din egen lösning:</span><span class="sxs-lookup"><span data-stu-id="cb023-117">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="cb023-118">Med hjälp av audit API-referens</span><span class="sxs-lookup"><span data-stu-id="cb023-118">Using the audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="cb023-119">Med hjälp av rapporten API-referens inloggningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="cb023-119">Using the sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="cb023-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb023-120">Next Steps</span></span>
<span data-ttu-id="cb023-121">Om du är nyfiken att se alla tillgängliga Azure AD Graph API-slutpunkter kan använda den här länken: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="cb023-121">If you are curious to see all available Azure AD Graph API endpoints, use this link: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span></span>

