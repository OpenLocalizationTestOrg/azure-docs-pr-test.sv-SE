---
title: "rapportkvarhållningsregler i aaaAzure Active Directory | Microsoft Docs"
description: "Bevarandeprinciper på rapportdata i Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c46a68198cb34e9c92662b2f8461010745392c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Rapportkvarhållningsregler i Azure Active Directory


Det här avsnittet ger dig svar toohello vanligaste frågorna tillsammans med hello datalagring för hello olika aktivitetsrapporter i Azure Active Directory. 

**F: hur kan du få hello samling aktivitetsdata igång?**

**S:**

| Azure AD-version | Samlingen Start |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | När du registrerar dig för en prenumeration |
| Azure AD Kostnadsfri | Hej första gången du öppnar hello [Azure Active Directory-bladet](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) eller använda hello [reporting API: er](https://aka.ms/aadreports)  |

---
**F: när är din aktivitetsdata i hello Azure-portalen?**

**S:**

- **Omedelbart** - om du redan har arbetat med rapporter i hello klassiska Azure-portalen
- **Inom två timmar** - om du inte har aktiverat rapportering i hello klassiska Azure-portalen

---
**F: hur kan du få hello insamling av säkerhet signaler igång?**  

**S:** för säkerhet signaler hello samling processen startar när du delta i toouse hello Identity Protection Center. 


---
**F: hur länge hello insamlade data lagras?**

**S:**

**Aktivitetsrapporter**    

| Rapport                 | Azure AD Kostnadsfri | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Kataloggranskning        | 7 dagar        | 30 dagar             | 30 dagar             |
| Inloggningsaktivitet       | 7 dagar        | 30 dagar             | 30 dagar             |

**Säkerhet signaler**

| Rapport         | Azure AD Kostnadsfri | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Användare i farozonen  | 7 dagar        | 30 dagar             | 90 dagar             |
| Riskfyllda inloggningar | 7 dagar        | 30 dagar             | 90 dagar             |

---