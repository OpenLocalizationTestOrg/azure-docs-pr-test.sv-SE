---
title: "aaaGetting igång med hello Azure AD reporting API | Microsoft Docs"
description: "Hur tooget igång med hello Azure Active Directory reporting API"
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
ms.openlocfilehash: bb7d72ba445daa367d7502889c38a605a16f26d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-active-directory-reporting-api"></a><span data-ttu-id="e9f31-103">Komma igång med hello Azure Active Directory reporting API</span><span class="sxs-lookup"><span data-stu-id="e9f31-103">Getting started with hello Azure Active Directory reporting API</span></span>

<span data-ttu-id="e9f31-104">Azure Active Directory innehåller en mängd olika rapporter.</span><span class="sxs-lookup"><span data-stu-id="e9f31-104">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="e9f31-105">hello data för de här rapporterna kan vara användbar tooyour program, till exempel SIEM-system, granskning och business intelligence-verktyg.</span><span class="sxs-lookup"><span data-stu-id="e9f31-105">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="e9f31-106">hello Azure AD reporting API: er ger Programmeringsåtkomst toohello data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="e9f31-106">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="e9f31-107">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="e9f31-107">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="e9f31-108">Den här artikeln innehåller hello information du behöver tooget igång med hello Azure AD reporting API: er.</span><span class="sxs-lookup"><span data-stu-id="e9f31-108">This article provides you with hello information you need tooget started with hello Azure AD reporting APIs.</span></span>
<span data-ttu-id="e9f31-109">I nästa avsnitt om hello hittar du mer information om hur du använder hello gransknings- och logga in API: er.</span><span class="sxs-lookup"><span data-stu-id="e9f31-109">In hello next section, you find more details about using hello audit and sign-in APIs.</span></span> 

<span data-ttu-id="e9f31-110">Vanliga frågor och svar, Läs vår [vanliga frågor och svar](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span><span class="sxs-lookup"><span data-stu-id="e9f31-110">For frequently asked questions,read our [FAQ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq).</span></span> <span data-ttu-id="e9f31-111">Problem, ta [filen ett supportärende](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span><span class="sxs-lookup"><span data-stu-id="e9f31-111">For issues, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)</span></span>

## <a name="learning-map"></a><span data-ttu-id="e9f31-112">Inlärningskarta</span><span class="sxs-lookup"><span data-stu-id="e9f31-112">Learning map</span></span>
1. <span data-ttu-id="e9f31-113">**Förbereda** -innan du kan testa din API samples måste toocomplete hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e9f31-113">**Prepare** - Before you can test your API samples, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites-azure-portal.md).</span></span>
2. <span data-ttu-id="e9f31-114">**Utforska** -hämta ett första intryck av hello reporting API: er:</span><span class="sxs-lookup"><span data-stu-id="e9f31-114">**Explore** - Get a first impression of hello reporting APIs:</span></span>
   
   * [<span data-ttu-id="e9f31-115">Med hjälp av hello prov för hello, granska API</span><span class="sxs-lookup"><span data-stu-id="e9f31-115">Using hello samples for hello audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="e9f31-116">Med hjälp av hello prov för hello inloggningsaktivitet rapporten API</span><span class="sxs-lookup"><span data-stu-id="e9f31-116">Using hello samples for hello sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="e9f31-117">**Anpassa** -skapa din egen lösning:</span><span class="sxs-lookup"><span data-stu-id="e9f31-117">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="e9f31-118">Med hjälp av hello audit API-referens</span><span class="sxs-lookup"><span data-stu-id="e9f31-118">Using hello audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="e9f31-119">Med hjälp av hello inloggningsaktivitet rapporten API-referens</span><span class="sxs-lookup"><span data-stu-id="e9f31-119">Using hello sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="e9f31-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9f31-120">Next Steps</span></span>
<span data-ttu-id="e9f31-121">Om du är nyfiken toosee alla tillgängliga Azure AD Graph API-slutpunkter, Använd den här länken: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="e9f31-121">If you are curious toosee all available Azure AD Graph API endpoints, use this link: [https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).</span></span>

