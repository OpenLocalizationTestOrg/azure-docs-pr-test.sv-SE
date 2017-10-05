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
# <a name="getting-started-with-the-azure-active-directory-reporting-api-on-the-azure-ad-classic-portal"></a>Komma igång med Azure Active Directory reporting API på den klassiska Azure AD-portalen
*Det här avsnittet är en del av den [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

Azure Active Directory innehåller en mängd olika rapporter. Data för de här rapporterna kan vara användbara för dina program, till exempel SIEM-system, gransknings- och business intelligence-verktyg. Azure AD reporting API: er ger programmässig åtkomst till data via en uppsättning REST-baserade API: er. Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.

Den här artikeln ger dig den information du behöver att komma igång med Azure AD reporting API: er.
I nästa avsnitt du hittar mer information om hur du använder revisionen och logga in API: er. Alla andra API: er, finns det [Azure AD-rapporter och events(preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) artikel.

För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).

## <a name="learning-map"></a>Inlärningskarta
1. **Förbereda** -innan du kan testa din API samples, måste du slutföra de [krav för att få åtkomst till Azure AD reporting API](active-directory-reporting-api-prerequisites.md).
2. **Utforska** -få en första intryck reporting API:
   
   * [Använda exemplen för granskning API](active-directory-reporting-api-audit-samples.md) 
   * [Använda exemplen för rapporten inloggningsaktivitet API](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Anpassa** -skapa din egen lösning: 
   
   * [Med hjälp av audit API-referens](active-directory-reporting-api-audit-reference.md) 
   * [Med hjälp av rapporten API-referens inloggningsaktivitet](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Nästa steg
Om du är nyfiken på att se alla tillgängliga Azure AD Graph API-slutpunkter genom att gå till [https://graph.windows.net/tenant-name/reports/$ metadata? api-version = beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta).

