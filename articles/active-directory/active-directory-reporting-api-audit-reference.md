---
title: aaaAzure Active Directory audit API-referens | Microsoft Docs
description: "Hur tooget igång med hello Azure Active Directory granska API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5f33b62ede9be445f35704739e328580dc454368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory audit API-referens
Det här avsnittet är en del av en samling ämnen om hello Azure Active Directory reporting API.  
Azure AD-rapportering ger dig en API som gör att du tooaccess granskningsdata via kod eller relaterade verktyg.
hello omfånget för det här avsnittet är tooprovide dig med information om hello **granska API**.

Se:

* [Granskningsloggar](active-directory-reporting-azure-portal.md#activity-reports) mer information

* [Komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) mer information om hello reporting API.


För:

- Vanliga frågor och svar, Läs vår [vanliga frågor och svar](active-directory-reporting-faq.md) 

- Kontrollera utfärdar [filen ett supportärende](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-hello-data"></a>Vem som kan komma åt hello data?
* Användare med hello säkerhet Admin eller säkerhet Reader rollen
* Globala administratörer
* Alla appar som har tillståndet tooaccess hello API (auktorisering i apptjänst kan vara installationen endast baserat på Global administratör behörighet)

## <a name="prerequisites"></a>Krav
Hej Reporting API i ordning tooaccess detta rapporterar via, måste du ha:

* En [Azure Active Directory ledigt eller bättre edition](active-directory-editions.md)
* Slutförda hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-hello-api"></a>Åtkomst till hello-API
Du kan antingen använda detta API via hello [diagram Explorer](https://graphexplorer2.cloudapp.net) eller programmässigt med, till exempel PowerShell. I ordning för PowerShell toocorrectly tolka hello OData filter syntax används i AAD diagram REST-anrop, måste du använda hello backtick (även kallat: allvarligt accent) tecken för ”undantagstecken” hello $. Hej backtick tecknet som fungerar som [PowerShells escape-tecknet](https://technet.microsoft.com/library/hh847755.aspx), vilket gör att PowerShell toodo en literal tolkning av hello $-tecken och undvika förvirrande som ett PowerShell-variabelnamn (ie: $filter).

hello fokuserar i det här avsnittet på hello diagrammet Explorer. Ett PowerShell-exempel finns [PowerShell-skriptet](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-slutpunkt
Du kan komma åt den här API: et med hello följande URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Det finns ingen gräns för hello antalet poster som returneras av hello Azure AD granska API (med OData sidbrytning).
Kvarhållning begränsningar rapportdata, kolla [Reporting bevarandeprinciper](active-directory-reporting-retention.md).

Det här anropet returnerar hello data i batchar. Varje grupp som har högst 1000 poster.  
tooget hello nästa batch poster, Använd hello nästa länk. Hämta hello skiptoken information från hello första uppsättning returnerade poster. hello skip-token kommer att hello slutet av hello resultatmängden.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Filter som stöds
Du kan begränsa hello antalet poster som returneras av en API-anrop i form av ett filter.  
Relaterade data, hello följande filter stöds för inloggning API:

* **$top =\<antal poster toobe returneras\>**  -toolimit hello antalet returnerade poster. Det här är en kostsam åtgärd. Du bör inte använda det här filtret om du vill tooreturn tusentals objekt.     
* **$filter =\<filter-instruktionen\>**  -toospecify hello baserat på filter som stöds fält, hello typ av poster som intresserar dig

## <a name="supported-filter-fields-and-operators"></a>Filter som stöds fält och operatorer
toospecify hello typ av poster som intresserar dig, kan du skapa ett filter-uttryck som kan innehålla en eller en kombination av hello följande filterfält:

* [activityDate](#activitydate) -definierar ett datum eller datumintervall
* [kategori](#category) -definierar hello kategori toofilter på.
* [activityStatus](#activitystatus) -definierar hello status för en aktivitet
* [activityType](#activitytype) -definierar hello-typ för en aktivitet
* [aktiviteten](#activity) -definierar hello aktivitet som sträng  
* [namn påskådespelare/](#actorname) -definierar hello aktören i form av hello aktören namn
* [aktören/objectid](#actorobjectid) -definierar hello aktören i form av hello aktörs-ID   
* [aktören/upn](#actorupn) -definierar hello aktören i form av hello aktören principen (user Principal name) 
* [målnamn/](#targetname) -definierar hello mål i form av hello aktören namn
* [mål/objectid](#targetobjectid) -definierar hello mål i form av hello mål-ID  
* [mål/upn](#targetupn) -definierar hello aktören i form av hello aktören principen (user Principal name)   

- - -
### <a name="activitydate"></a>activityDate
**Stöd för operatörer**: eq, ge, le, gt, lt

**Exempel**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Anteckningar**:

datetime ska vara i UTC-format

- - -
### <a name="category"></a>category

**Värden som stöds**:

| Kategori                         | Värde     |
| :--                              | ---       |
| Kärnkatalog                   | Katalog |
| Hantering av lösenord för självbetjäning | SSPR      |
| Självbetjäning, grupphantering    | SSGM      |
| Kontoetablering             | Sync      |
| Automatiserad förnyelse av lösenord      | Automatiserad förnyelse av lösenord |
| Identity Protection              | IdentityProtection |
| Inbjudna användare                    | Inbjudna användare |
| MIM-tjänst                      | MIM-tjänst |



**Stöd för operatörer**: eq

**Exempel**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Värden som stöds**:

| Aktivitetsstatus | Värde |
| :--             | ---   |
| Lyckades         | 0     |
| Fel         | - 1   |

**Stöd för operatörer**: eq

**Exempel**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>activityType
**Stöd för operatörer**: eq

**Exempel**:

    $filter=activityType eq 'User'    

**Anteckningar**:

skiftlägeskänsligt

- - -
### <a name="activity"></a>Aktivitet
**Stöd för operatörer**: eq, innehåller startsWith

**Exempel**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Anteckningar**:

skiftlägeskänsligt

- - -
### <a name="actorname"></a>aktören/namn
**Stöd för operatörer**: eq, innehåller startsWith

**Exempel**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Anteckningar**:

Icke skiftlägeskänslig

- - -
### <a name="actorobjectid"></a>aktören/objectId
**Stöd för operatörer**: eq

**Exempel**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>målnamn /
**Stöd för operatörer**: eq, innehåller startsWith

**Exempel**:

    $filter=targets/any(t: t/name eq 'some name')    

**Anteckningar**:

Icke skiftlägeskänslig

- - -
### <a name="targetupn"></a>mål/upn
**Stöd för operatörer**: eq, startsWith

**Exempel**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Anteckningar**:

* Icke skiftlägeskänslig
* Du behöver tooadd hello fullständiga namnområde när du frågar Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity

- - -
### <a name="targetobjectid"></a>mål/objectId
**Stöd för operatörer**: eq

**Exempel**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>aktören/upn
**Stöd för operatörer**: eq, startsWith

**Exempel**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Anteckningar**:

* Icke skiftlägeskänslig 
* Du behöver tooadd hello fullständiga namnområde när du frågar Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity

- - -
## <a name="next-steps"></a>Nästa steg
* Vill du toosee exempel för filtrerade systemaktiviteter? Kolla in hello [Azure Active Directory audit API-exempel](active-directory-reporting-api-audit-samples.md).
* Vill du tooknow mer om hello Azure AD reporting API? Se [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

