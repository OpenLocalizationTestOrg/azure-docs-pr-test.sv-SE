---
title: "Azure AD Connect: Uttryck för deklarativ etablering | Microsoft Docs"
description: "Förklarar hello-uttryck för deklarativ etablering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 516bcf1991c608d33aefc19551254d8b2bfc024f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect-synkronisering: Förstå uttryck för deklarativ etablering
Azure AD Connect-synkronisering bygger på deklarativ etablering introducerades i Forefront Identity Manager 2010. Det gör att du tooimplement affärslogik fullständig identity integration utan hello måste toowrite kompilerad kod.

En väsentlig del av deklarativ etablering är hello Uttrycksspråk som används i attributflöden. hello-språk som används är en delmängd av Microsoft® Visual Basic for Applications (VBA). Det här språket används i Microsoft Office och användarna har erfarenhet av VBScript tolkar även. hello deklarativ etablering Expression Language endast med hjälp av funktioner och är inte ett strukturerade språk. Det finns inga metoder eller instruktioner. Funktioner i stället är kapslade tooexpress programmet flödet.

Mer information finns i [Välkommen toohello Visual Basic för program Språkreferens för Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

hello attribut har strikt typkontroll. En funktion accepterar endast attribut för hello rätt typ. Det är också skiftlägeskänsligt. Både funktionsnamnen och attributnamn måste ha rätt skiftläge eller ett felmeddelande.

## <a name="language-definitions-and-identifiers"></a>Språk definitioner och identifierare
* Funktioner har ett namn följt av argument inom parentes: FunctionName (argumentet 1, argumentet N).
* Attribut som identifieras av hakparenteser: [attributeName]
* Parametrar som identifieras av procenttecken: % ParameterName %
* Strängkonstanter omges av citattecken: till exempel ”Contoso” (Obs: måste använda enkla citattecken ”” och inte typografiska citattecken ””)
* Numeriska värden anges utan citattecken och förväntade toobe decimal. Hexadecimala värden föregås av & H. Till exempel 98052 & HFF
* Booleska värden uttrycks med konstanter: SANT, FALSKT.
* Inbyggda konstanter och literaler uttrycks med endast deras namn: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Funktioner
Deklarativ etablering använder många funktioner tooenable hello möjligheten tootransform attributvärden. Dessa funktioner kan kapslas så hello resultatet från en funktion som du har angett tooanother funktion.

`Function1(Function2(Function3()))`

hello fullständig lista över funktioner finns i hello [fungerar referens](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametrar
En parameter har definierats av en koppling eller av en administratör med hjälp av PowerShell. Parametrarna innehåller vanligtvis värden som skiljer sig från system toosystem, till exempel hello namnet på hello domänanvändare hello finns i. Dessa parametrar kan användas i attributflöden.

hello Active Directory-kopplingen angivna hello följande parametrar för regler för inkommande synkronisering:

| Parameternamn | Kommentar |
| --- | --- |
| Domain.Netbios |NetBIOS-format i hello-domän som importerats, till exempel FABRIKAMSALES |
| Domain.FQDN |FQDN-format i hello-domän som importerats, till exempel sales.fabrikam.com |
| Domain.LDAP |LDAP-format för hello domän som importerats, till exempel DC = försäljning, DC = fabrikam, DC = com |
| Forest.Netbios |NetBIOS-format för hello skogsnamnet som importerats, till exempel FABRIKAMCORP |
| Forest.FQDN |Hello skogsnamnet som importerats, till exempel fabrikam.com FQDN-format |
| Forest.LDAP |LDAP-format för hello skogsnamnet som importerats, till exempel DC = fabrikam, DC = com |

hello system ger hello följande parameter, vilket används tooget hello identifierare för hello Connector körs:  
`Connector.ID`

Här är ett exempel som fyller på hello metaversum attributdomän med hello netbios-namnet på hello domän där hello användaren finns:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operatorer
Du kan använda hello följande operatorer:

* **Jämförelse**: <, < =, <>, =, >, > =
* **Matematik**: +, -, \*, -
* **Strängen**: & (sammanfoga)
* **Logisk**: & & (och). (eller)
* **Utvärderingsordningen**:)

Operatörer utvärderade vänstra tooright och hello har samma prioritet för utvärdering. Det vill säga hello \* (multiplikator) utvärderas inte innan - (subtraktion). 2\*(5 + 3) inte är hello samma som 2\*5 + 3. hello hakparenteser () används toochange hello utvärdering ordning när lämnas tooright utvärderingsordning inte är lämpligt.

## <a name="multi-valued-attributes"></a>Flera värden attribut
hello funktioner fungerar med både enstaka och flera värden. För flera värden attribut hello-funktionen fungerar över varje värde och tillämpar hello samma fungera tooevery värde.

Exempel:  
`Trim([proxyAddresses])`Göra en Trimning av varje värde i hello proxyAddress attributet.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`För varje värde med en @-sign, Ersätt hello domän med @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Leta efter hello SIP-adress och ta bort den från hello värden.

## <a name="next-steps"></a>Nästa steg
* Läs mer om hello Konfigurationsmodell i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Se hur deklarativ etablering är att använda out-of-box i [förstå hello standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md).
* Se hur toomake en praktisk ändras med hjälp av deklarativ etablering i [hur toomake en ändring toohello standard configuration](active-directory-aadconnectsync-change-the-configuration.md).

**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

**Referensinformation**

* [Azure AD Connect-synkronisering: funktioner referens](active-directory-aadconnectsync-functions-reference.md)

