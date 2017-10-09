---
title: "aaaGetting igång med hello Azure AD reporting API på klassiska hello Azure AD-portalen | Microsoft Docs"
description: "Hur tooget igång med hello Azure Active Directory reporting API"
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
ms.openlocfilehash: 52e22d442650731fc6ed28991fc65f9182af0540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-azure-active-directory-reporting-api-on-hello-azure-ad-classic-portal"></a><span data-ttu-id="404e1-103">Komma igång med hello Azure Active Directory reporting API på klassiska hello Azure AD-portalen</span><span class="sxs-lookup"><span data-stu-id="404e1-103">Getting started with hello Azure Active Directory reporting API on hello Azure AD classic portal</span></span>
<span data-ttu-id="404e1-104">*Det här avsnittet är en del av hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span><span class="sxs-lookup"><span data-stu-id="404e1-104">*This topic is part of hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*</span></span>

<span data-ttu-id="404e1-105">Azure Active Directory innehåller en mängd olika rapporter.</span><span class="sxs-lookup"><span data-stu-id="404e1-105">Azure Active Directory provides you with a variety of reports.</span></span> <span data-ttu-id="404e1-106">hello data för de här rapporterna kan vara användbar tooyour program, till exempel SIEM-system, granskning och business intelligence-verktyg.</span><span class="sxs-lookup"><span data-stu-id="404e1-106">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="404e1-107">hello Azure AD reporting API: er ger Programmeringsåtkomst toohello data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="404e1-107">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="404e1-108">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="404e1-108">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="404e1-109">Den här artikeln innehåller hello information du behöver tooget igång med hello Azure AD reporting API: er.</span><span class="sxs-lookup"><span data-stu-id="404e1-109">This article provides you with hello information you need tooget started with hello Azure AD reporting APIs.</span></span>
<span data-ttu-id="404e1-110">I nästa avsnitt om hello hittar du mer information om hur du använder hello gransknings- och logga in API: er.</span><span class="sxs-lookup"><span data-stu-id="404e1-110">In hello next section, you find more details about using hello audit and sign-in APIs.</span></span> <span data-ttu-id="404e1-111">För alla andra API: er, se hello [Azure AD-rapporter och events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) artikel.</span><span class="sxs-lookup"><span data-stu-id="404e1-111">For all other APIs, see hello [Azure AD reports and events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) article.</span></span>

<span data-ttu-id="404e1-112">För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="404e1-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="learning-map"></a><span data-ttu-id="404e1-113">Inlärningskarta</span><span class="sxs-lookup"><span data-stu-id="404e1-113">Learning map</span></span>
1. <span data-ttu-id="404e1-114">**Förbereda** -innan du kan testa din API samples måste toocomplete hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="404e1-114">**Prepare** - Before you can test your API samples, you need toocomplete hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span>
2. <span data-ttu-id="404e1-115">**Utforska** -hämta ett första intryck av hello reporting API: er:</span><span class="sxs-lookup"><span data-stu-id="404e1-115">**Explore** - Get a first impression of hello reporting APIs:</span></span>
   
   * [<span data-ttu-id="404e1-116">Med hjälp av hello prov för hello, granska API</span><span class="sxs-lookup"><span data-stu-id="404e1-116">Using hello samples for hello audit API</span></span>](active-directory-reporting-api-audit-samples.md) 
   * [<span data-ttu-id="404e1-117">Med hjälp av hello prov för hello inloggningsaktivitet rapporten API</span><span class="sxs-lookup"><span data-stu-id="404e1-117">Using hello samples for hello sign-in activity report API</span></span>](active-directory-reporting-api-sign-in-activity-samples.md)
3. <span data-ttu-id="404e1-118">**Anpassa** -skapa din egen lösning:</span><span class="sxs-lookup"><span data-stu-id="404e1-118">**Customize** -  Create your own solution:</span></span> 
   
   * [<span data-ttu-id="404e1-119">Med hjälp av hello audit API-referens</span><span class="sxs-lookup"><span data-stu-id="404e1-119">Using hello audit API reference</span></span>](active-directory-reporting-api-audit-reference.md) 
   * [<span data-ttu-id="404e1-120">Med hjälp av hello inloggningsaktivitet rapporten API-referens</span><span class="sxs-lookup"><span data-stu-id="404e1-120">Using hello sign-in activity report API reference</span></span>](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a><span data-ttu-id="404e1-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="404e1-121">Next Steps</span></span>
<span data-ttu-id="404e1-122">Om du är nyfiken toosee alla hello tillgängliga Azure AD Graph API-slutpunkter genom att navigera för[https://graph.windows.net/tenant-name/reports/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span><span class="sxs-lookup"><span data-stu-id="404e1-122">If you are curious toosee all of hello available Azure AD Graph API endpoints by navigating too[https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).</span></span>

