---
title: aaaAzure Active Directory-inloggning aktivitetsrapport API-referens | Microsoft Docs
description: "Referens för hello Azure Active Directory-inloggning aktivitetsrapport API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 8123f308a291503f2c61ac5de26696806c6402ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory inloggningsaktivitet rapporten API-referens
Det här avsnittet är en del av en samling ämnen om hello Azure Active Directory reporting API.  
Azure AD-rapportering ger dig en API som gör att du tooaccess inloggningsaktivitet rapportdata via kod eller relaterade verktyg.
hello omfånget för det här avsnittet är tooprovide dig med information om hello **aktivitet rapporten API inloggning**.

Se:

* [Logga in aktiviteter](active-directory-reporting-azure-portal.md#activity-reports) mer information
* [Komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) mer information om hello reporting API.


## <a name="who-can-access-hello-api-data"></a>Vem som kan komma åt hello API data?
* Användare och tjänstens huvudnamn i hello säkerhet Admin eller säkerhet Reader roll
* Globala administratörer
* Alla appar som har tillståndet tooaccess hello API (auktorisering i apptjänst kan vara installationen endast baserat på Global administratör behörighet)

tooconfigure åtkomst för en tooaccess programsäkerhet API: er, till exempel logga händelser, Använd hello följande PowerShell tooadd hello program tjänstens huvudnamn i rollen för hello säkerhet läsare

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a>Krav
tooaccess detta rapporterar via hello reporting API måste du ha:

* En [Azure Active Directory Premium P1 eller P2 edition](active-directory-editions.md)
* Slutförda hello [krav tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-hello-api"></a>Åtkomst till hello-API
Du kan antingen använda detta API via hello [diagram Explorer](https://graphexplorer2.cloudapp.net) eller programmässigt med, till exempel PowerShell. I ordning för PowerShell toocorrectly tolka hello OData filter syntax används i AAD diagram REST-anrop, måste du använda hello backtick (även kallat: allvarligt accent) tecken för ”undantagstecken” hello $. Hej backtick tecknet som fungerar som [PowerShells escape-tecknet](https://technet.microsoft.com/library/hh847755.aspx), vilket gör att PowerShell toodo en literal tolkning av hello $-tecken och undvika förvirrande som ett PowerShell-variabelnamn (ie: $filter).

hello fokuserar i det här avsnittet på hello diagrammet Explorer. Ett PowerShell-exempel finns [PowerShell-skriptet](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-slutpunkt
Du kan komma åt den här API: et med hello följande bas-URI:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Detta API är begränsad till en miljon returnerade poster på grund av toohello mängden data. 

Det här anropet returnerar hello data i batchar. Varje grupp som har högst 1000 poster.  
tooget hello nästa batch poster, Använd hello nästa länk. Hämta hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information från hello första uppsättning returnerade poster. hello skip-token kommer att hello slutet av hello resultatmängden.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Filter som stöds
Du kan begränsa hello antalet poster som returneras av en API-anrop i form av ett filter.  
Relaterade data, hello följande filter stöds för inloggning API:

* **$top =\<antal poster toobe returneras\>**  -toolimit hello antalet returnerade poster. Det här är en kostsam åtgärd. Du bör inte använda det här filtret om du vill tooreturn tusentals objekt.  
* **$filter =\<filter-instruktionen\>**  -toospecify hello baserat på filter som stöds fält, hello typ av poster som intresserar dig

## <a name="supported-filter-fields-and-operators"></a>Filter som stöds fält och operatorer
toospecify hello typ av poster som intresserar dig, kan du skapa ett filter-uttryck som kan innehålla en eller en kombination av hello följande filterfält:

* [signinDateTime](#signindatetime) -definierar ett datum eller datumintervall
* [användar-ID](#userid) -definierar en specifik användare baserade hello användar-ID.
* [userPrincipalName](#userprincipalname) -definierar en specifik användare baserade hello användarens huvudnamn (UPN)
* [appId](#appid) -definierar en viss app baserat hello app-ID
* [appDisplayName](#appdisplayname) -definierar en viss app baserat hello app visningsnamn
* [loginStatus](#loginStatus) -definierar hello status för hello inloggningar (lyckade / misslyckade)

> [!NOTE]
> När du använder diagram Explorer, måste du hello toouse hello rätt fallet för varje brev är filterfält.
> 
> 

Du kan skapa kombinationer av hello stöds filter och filterfält toonarrow ned hello omfattning hello returnerade data. Till exempel returnerar hello följande sats hello översta 10 poster mellan 1 juli 2016 och juli 6 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**Stöd för operatörer**: eq, ge, le, gt, lt

**Exempel**:

Med hjälp av ett visst datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Med hjälp av ett datumintervall    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Anteckningar**:

hello datetime-parametern måste vara i hello UTC-format 

- - -
### <a name="userid"></a>Användar-ID
**Stöd för operatörer**: eq

**Exempel**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Anteckningar**:

hello-värdet för användar-ID är ett strängvärde

- - -
### <a name="userprincipalname"></a>UserPrincipalName
**Stöd för operatörer**: eq

**Exempel**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Anteckningar**:

hello-värdet för userPrincipalName är ett strängvärde

- - -
### <a name="appid"></a>appId
**Stöd för operatörer**: eq

**Exempel**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Anteckningar**:

hello-värdet för appId är ett strängvärde

- - -
### <a name="appdisplayname"></a>appDisplayName
**Stöd för operatörer**: eq

**Exempel**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Anteckningar**:

hello-värdet för appDisplayName är ett strängvärde

- - -
### <a name="loginstatus"></a>loginStatus
**Stöd för operatörer**: eq

**Exempel**:

    $filter=loginStatus+eq+'1'  


**Anteckningar**:

Det finns två alternativ för hello loginStatus: 0 - lyckades, 1 – fel

- - -
## <a name="next-steps"></a>Nästa steg
* Vill du toosee exempel för filtrerade inloggning aktiviteter? Kolla in hello [Azure Active Directory inloggningsaktivitet rapporten API-exempel](active-directory-reporting-api-sign-in-activity-samples.md).
* Vill du tooknow mer om hello Azure AD reporting API? Se [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).

