---
title: "Rapportkvarhållningsregler i Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: ffeba8a6c32a21c75af21f948bbd6ea88dd9278c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Rapportkvarhållningsregler i Azure Active Directory


Det här avsnittet ger svar på de vanligaste frågorna tillsammans med datalagring för olika aktivitetsrapporter i Azure Active Directory. 

**F: hur kan du få insamlingen av aktivitetsdata starta?**

**S:**

| Azure AD-version | Samlingen Start |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | När du registrerar dig för en prenumeration |
| Azure AD Kostnadsfri | Första gången du öppnar den [Azure Active Directory-bladet](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) eller använda den [reporting API: er](https://aka.ms/aadreports)  |

---
**F: när är aktivitetsdata i Azure portal?**

**S:**

- **Omedelbart** - om du redan har arbetat med rapporter i den klassiska Azure-portalen
- **Inom två timmar** - om du inte har aktiverat rapportering i den klassiska Azure-portalen

---
**F: hur kan du få insamling av säkerhet signaler igång?**  

**S:** för säkerhet signaler samling processen startar när du delta i använda Identity Protection Center. 


---
**F: hur länge insamlade data lagras?**

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