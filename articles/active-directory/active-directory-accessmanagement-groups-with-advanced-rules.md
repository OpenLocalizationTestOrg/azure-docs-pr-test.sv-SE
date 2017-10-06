---
title: "aaaPopulate grupper dynamiskt baserat på objektattribut i Azure Active Directory | Microsoft Docs"
description: "Hur-Servers toocreate avancerade regler för gruppmedlemskap inklusive stöds uttryck regeln operatorer och parametrar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 04813a42-d40a-48d6-ae96-15b7e5025884
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: fe22829118ed8f5137a619d93fa6f9bf80835863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="populate-groups-dynamically-based-on-object-attributes"></a>Fyll i grupper dynamiskt baserat på objektattribut
hello klassiska Azure-portalen ger dig hello möjlighet tooenable mer komplexa attributbaserad dynamiskt medlemskap för grupper i Azure Active Directory (AD Azure).  

När alla attribut för användare eller enhet hello utvärderas alla dynamisk gruppregler i en katalog toosee om hello ändringen skulle leda till valfri grupp lägger till eller tar bort. Om en användare eller enhet uppfyller en regel för en grupp, läggs de som medlem i gruppen. Om de inte längre uppfyller hello regeln tas bort.

> [!NOTE]
> - Du kan skapa en regel för dynamiskt medlemskap för säkerhetsgrupper eller Office 365-grupper.
>
> - Den här funktionen kräver en Azure AD Premium P1-licens för varje medlem tillagda tooat åtminstone en dynamisk grupp.
>
> - Du kan skapa en dynamisk grupp för enheter eller användare, men du kan inte skapa en regel som innehåller både användare och enhetsobjekt.

> - Hello tillfället är det inte möjligt toocreate en enhetsgrupp baserat på det ägande användarens attribut. Medlemskapsregler för enheten kan bara referens omedelbar attribut enhetsobjekt i hello directory.

## <a name="toocreate-an-advanced-rule"></a>toocreate en avancerad regel
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.
2. Välj hello **grupper** fliken och sedan öppna hello-grupp som du vill tooedit.
3. Välj hello **konfigurera** fliken, väljer hello **avancerade regeln** alternativet och ange sedan hello avancerade regeln i hello textruta.

## <a name="constructing-hello-body-of-an-advanced-rule"></a>Hur du skapar hello brödtexten i en avancerad regel
hello avancerade regeln som du kan skapa för hello dynamiskt medlemskap för grupper är i grunden en binär uttryck som består av tre delar och resulterar i ett true eller false resultat. hello tre delar är:

* Vänster parametern
* Binär operator
* Höger konstant

En fullständig avancerade regeln ser ut ungefär toothis: (leftParameter binaryOperator ”RightConstant”), där hello inledande och avslutande parentes krävs för hello hela binära uttryck, dubbla citattecken krävs för hello rätt konstant och hello-syntax för hello är vänstra parametern user.property. En avancerad regel kan bestå av fler än en binär uttryck avgränsade med hello- och- eller, och - inte logiska operatorer.
hello följande är exempel på ett korrekt angivet avancerad regel:

* (user.department - eq ”försäljning”)- eller (user.department - eq ”marknadsföring”)
* (user.department - eq ”försäljning”)- och - inte (user.jobTitle-innehåller ”SDE”)

Hello fullständig lista över parametrar som stöds och uttryck regeln operatorer, finns i avsnitten nedan.


Observera att hello-egenskapen måste föregås av hello rätt objekttyp: användare eller enhet.
hello nedan regeln misslyckas hello-verifiering: e – ne null

hello rätt regel är:

User.Mail – ne null

hello totala hello brödtexten i avancerade regeln kan inte vara längre än 2048 tecken.

> [!NOTE]
> Sträng och regex åtgärder är inte skiftlägeskänsligt.
> Strängar som innehåller citattecken ”ska avgränsas med-tecken, till exempel user.department - eq \`” försäljning ”.
> Använd endast citationstecken strängvärde för typen och bara använda engelska citattecken.
>
>

## <a name="supported-expression-rule-operators"></a>Stöds uttryck regeln operatorer
hello följande tabell visas alla operatorer i hello stöds uttryck regeln och deras syntax toobe som används i hello brödtext hello avancerade regeln:

| Operatorn | Syntax |
| --- | --- |
| Är inte lika med |-ne |
| är lika med |-eq |
| Börjar inte med |-notStartsWith |
| Börjar med |startsWith- |
| Innehåller inte |-notContains |
| Contains |-innehåller |
| Matchar inte |-notMatch |
| Matcha |-matchar |
| i | -i |
| Inte i | -notIn |

## <a name="operator-precedence"></a>Operatorer

Alla operatorer nedan per prioritet från lägre toohigher, operator i samma rad har samma prioritet-alla - alla - eller - och - inte - eq - ne - startsWith - notStartsWith-innehåller - notContains-matchar – notMatch-i - notIn

Alla operatorer kan användas med eller utan bindestreck prefix.

Observera att en parentes som inte behövs alltid, behöver du bara tooadd parenteser när prioritet inte uppfyller dina krav till exempel:

   User.Department-eq ”marknadsföring” – och User.Country.-eq ”USA”

motsvarar:

   (user.department-eq ”marknadsföringsmaterial”) – och (User.Country.-eq ”USA”)

## <a name="using-hello--in-and--notin-operators"></a>Med hjälp av hello - i och - notIn operatorer

Om du vill toocompare hello värdet för ett användarattribut mot ett antal olika värden kan du använda hello - i eller notIn - operatorer. Här är ett exempel med hjälp av hello - i-operatorn:

    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]

Observera hello användning av hello ”[” och ”]” i hello början och slutet av hello lista med värden. Det här villkoret utvärderar tooTrue värdets hello user.department är lika med en av hello värdena i hello lista.

## <a name="query-error-remediation"></a>Frågan fel reparation
hello följande tabell visas eventuella fel och hur toocorrect dem om de sker

| Parsningsfel i frågan | Fel-användning | Korrigerade användning |
| --- | --- | --- |
| Fel: Attribut stöds inte. |(user.invalidProperty - eq ”värde”) |(user.department - eq ”värde”)<br/>-Egenskap måste matcha en från hello [lista över egenskaper som stöds](#supported-properties). |
| Fel: Operatorn stöds inte i attributet. |(user.accountEnabled-innehåller true) |(user.accountEnabled - eq SANT)<br/>Egenskapen är av typen boolean. Använda hello stöds operatorer (-eq eller - n) för boolesk typ från hello ovanför listan. |
| Fel: Frågekompileringsfel. |(user.department - eq ”försäljning”)- och (user.department - eq ”marknadsföring”) (user.userPrincipalName-matchning ”*@domain.ext”) |(user.department - eq ”försäljning”)- och (user.department - eq ”marknadsföring”)<br/>Logisk operator måste matcha en hello stöds egenskaper listan ovan. (user.userPrincipalName-matchar ”. *@domain.ext”) eller (user.userPrincipalName-matchar ”@domain.ext$”) i reguljära uttryck. |
| Fel: Binära uttrycket är inte i rätt format. |(user.department-eq ”försäljning”) (user.department - eq ”försäljning”) (user.department-eq ”försäljning”) |(user.accountEnabled - eq SANT)- och (user.userPrincipalName-innehåller ”alias@domain”)<br/>Frågan har flera fel. En parentes som inte rätt plats. |
| Fel: Okänt fel uppstod under konfigurerar dynamiskt medlemskap. |(user.accountEnabled - eq ”True” och user.userPrincipalName-innehåller ”alias@domain”) |(user.accountEnabled - eq SANT)- och (user.userPrincipalName-innehåller ”alias@domain”)<br/>Frågan har flera fel. En parentes som inte rätt plats. |

## <a name="supported-properties"></a>Egenskaper som stöds
hello följande är hello användaregenskaper som du kan använda i din avancerade regel:

### <a name="properties-of-type-boolean"></a>Egenskaper av typen boolean
Tillåtna operatörer

* -eq
* -ne

| Egenskaper | Tillåtna värden | Användning |
| --- | --- | --- |
| accountEnabled |SANT FALSKT |user.accountEnabled - eq true |
| DirSyncEnabled |SANT FALSKT |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Egenskaper av typen sträng
Tillåtna operatörer

* -eq
* -ne
* -notStartsWith
* StartsWith-
* -innehåller
* -notContains
* -matchar
* -notMatch
* -i
* -notIn

| Egenskaper | Tillåtna värden | Användning |
| --- | --- | --- |
| city |Ett string-värde eller $null |(user.city - eq ”värde”) |
| Land |Ett string-värde eller $null |(User.Country. - eq ”värde”) |
| Företagsnamn | Ett string-värde eller $null | (user.companyName - eq ”värde”) |
| Avdelning |Ett string-värde eller $null |(user.department - eq ”värde”) |
| Visningsnamn |Ett värde |(user.displayName - eq ”värde”) |
| facsimileTelephoneNumber |Ett string-värde eller $null |(user.facsimileTelephoneNumber - eq ”värde”) |
| givenName |Ett string-värde eller $null |(user.givenName - eq ”värde”) |
| Befattning |Ett string-värde eller $null |(user.jobTitle - eq ”värde”) |
| E-post |Alla strängvärde eller $null (SMTP-adress för hello användare) |(user.mail - eq ”värde”) |
| mailNickName |Ett värde (e postalias för hello användare) |(user.mailNickName - eq ”värde”) |
| mobila |Ett string-värde eller $null |(user.mobile - eq ”värde”) |
| objekt-ID |GUID för användarobjektet hello |(user.objectId - eq ”1111111-1111-1111-1111-111111111111”) |
| onPremisesSecurityIdentifier | Lokala säkerhetsidentifierare (SID) för användare som har synkroniserats från lokala toohello moln. |(user.onPremisesSecurityIdentifier - eq ”S-1-1-11-1111111111-1111111111-1111111111-1111111”) |
| passwordPolicies |Ingen DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies - eq ”DisableStrongPassword”) |
| physicalDeliveryOfficeName |Ett string-värde eller $null |(user.physicalDeliveryOfficeName - eq ”värde”) |
| Postnummer |Ett string-värde eller $null |(user.postalCode - eq ”värde”) |
| preferredLanguage |ISO 639-1-kod |(user.preferredLanguage - eq ”sv-se”) |
| sipProxyAddress |Ett string-värde eller $null |(user.sipProxyAddress - eq ”värde”) |
| state |Ett string-värde eller $null |(user.state - eq ”värde”) |
| StreetAddress |Ett string-värde eller $null |(user.streetAddress - eq ”värde”) |
| Efternamn |Ett string-värde eller $null |(user.surname - eq ”värde”) |
| telephoneNumber |Ett string-värde eller $null |(user.telephoneNumber - eq ”värde”) |
| usageLocation |Två bokstäver landskod |(user.usageLocation - eq ”USA”) |
| UserPrincipalName |Ett värde |(user.userPrincipalName - eq ”alias@domain”) |
| UserType |medlemmen gäst $null |(user.userType - eq ”medlem”) |

### <a name="properties-of-type-string-collection"></a>Egenskaper av typen sträng samling
Tillåtna operatörer

* -innehåller
* -notContains

| Egenskaper | Tillåtna värden | Användning |
| --- | --- | --- |
| otherMails |Ett värde |(user.otherMails-innehåller ”alias@domain”) |
| proxyAddresses |SMTP: alias@domain smtp:alias@domain |(user.proxyAddresses-innehåller ”SMTP: alias@domain”) |

## <a name="multi-value-properties"></a>Egenskaper för flera värden
Tillåtna operatörer

* -alla (nöjd när minst ett objekt i samlingen hello matchar hello villkor)
* -alla (uppfyllt när alla objekt i samlingen hello matchar hello villkor)

| Egenskaper | Värden | Användning |
| --- | --- | --- |
| assignedPlans |Varje objekt i samlingen hello visar hello följande strängegenskaper: capabilityStatus, tjänst, servicePlanId |user.assignedPlans-alla (assignedPlan.servicePlanId - eq ”efb87545-963c-4e0d-99df-69c6916d9eb0”- och assignedPlan.capabilityStatus - eq ”aktiverad”) |

Egenskaper för flera värden är samlingar av objekt i hello samma typ. Du kan använda - alla och -alla operatorer tooapply ett villkor tooone eller alla hello objekt i hello samling respektive. Exempel:

assignedPlans är en egenskap för flera värden med en lista över alla serviceplaner tilldelad toohello användare. hello nedan uttrycket väljer användare som har hello Exchange Online (Plan 2) service-plan som även är i aktiverat läge:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(hello Guid-identifierare identifierar hello Exchange Online (Plan 2) service-plan.)

> [!NOTE]
> Detta är användbart om du vill tooidentify alla användare som en Office 365 (eller andra Microsoft-onlinetjänst) kapaciteten har aktiverats för exempel tootarget dem med en viss uppsättning principer.

hello väljer följande uttryck alla användare som har alla service-plan som är associerad med hello Intune-tjänsten (som identifieras av tjänstnamn ”SCO”):
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Null-värden

toospecify ett null-värde i en regel kan du använda ”null” eller $null. Exempel:

   User.Mail – ne null motsvarar user.mail – ne $null

## <a name="extension-attributes-and-custom-attributes"></a>Tilläggsattribut och anpassade attribut
Tilläggsattribut och anpassade attribut stöds i regler för dynamiskt medlemskap.

Tilläggsattribut synkroniseras från lokala Windows Server AD och vidta hello format ”ExtensionAttributeX”, där X är lika med 1 och 15.
Ett exempel på en regel som använder ett tillägg-attribut är

(user.extensionAttribute15 - eq ”marknadsföring”)

Anpassade attribut som synkroniseras från lokala Windows Server AD eller från en ansluten SaaS-program och hello hello formatering av ”user.extension_[GUID]\__ [attribut]”, där [GUID] är hello Unik identifierare i AAD för hello program som skapade hello-attributet i AAD och [attribut] är hello hello attributets namn som det skapades.
Är ett exempel på en regel som använder ett anpassat attribut

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

hello anpassade attributets namn finns i hello directory genom att fråga användaren attributet diagrammet Utforskaren och söker efter hello attributets namn.

## <a name="direct-reports-rule"></a>Regeln ”direktrapporter”
Du kan skapa en grupp som innehåller alla direktrapporter för en chef. När hello manager direktrapporter ändras i framtida hello, ska hello gruppmedlemskap justeras automatiskt.

> [!NOTE]
> 1. Se till att hello för hello regeln toowork **ID Manager** egenskapen är korrekt inställda på användare i din klient. Du kan kontrollera hello aktuellt värde för en användare på deras **fliken profil**.
> 2. Den här regeln har bara stöd för **direkt** rapporter. Det är för närvarande inte möjligt toocreate en grupp för en kapslad hierarki, t.ex. en grupp som innehåller direktrapporter och deras rapporter.

**tooconfigure hello grupp**

1. Följ steg 1-5 från avsnittet [toocreate hello avancerade regeln](#to-create-the-advanced-rule), och välj en **medlemskapstypen** av **dynamiska användaren**.
2. På hello **dynamiskt medlemskapsregler** bladet ange hello regeln med hello följande syntax:

    *Direktrapporter för ”{obectID_of_manager}”*

    Ett exempel på en giltig regel:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is hello objectID of hello manager. hello object ID can be found on manager's **Profile tab**.
3. När du har sparat hello regel anges alla användare med hello Manager ID-värde läggs toohello grupp.

## <a name="using-attributes-toocreate-rules-for-device-objects"></a>Med attribut toocreate regler för enhetsobjekt
Du kan också skapa en regel som väljer enhetsobjekten för medlemskap i en grupp. hello följande attribut för enheten kan användas:

| Egenskaper              | Tillåtna värden                  | Användning                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| accountEnabled          | SANT FALSKT                      | (device.accountEnabled - eq SANT)                            |
| Visningsnamn             | Ett värde                | (device.displayName - eq ”Anders Iphone”)                       |
| DeviceOSType            | Ett värde                | (device.deviceOSType - eq ”IOS”)                             |
| DeviceOSVersion         | Ett värde                | (enhet. OSVersion - eq ”9.1”)                                |
| deviceCategory          | Ett värde                | (device.deviceCategory - eq ””)                              |
| DeviceManufacturer      | Ett värde                | (device.deviceManufacturer - eq ”Microsoft”)                 |
| DeviceModel             | Ett värde                | (device.deviceModel - eq ”IPhone 7 +”)                        |
| deviceOwnership         | Ett värde                | (device.deviceOwnership - eq ””)                             |
| Domännamn              | Ett värde                | (device.domainName - eq ”contoso.com”)                       |
| enrollmentProfileName   | Ett värde                | (device.enrollmentProfileName - eq ””)                       |
| isRooted                | SANT FALSKT                      | (device.deviceOSType - eq SANT)                              |
| managementType          | Ett värde                | (device.managementType - eq ””)                              |
| OrganizationalUnit      | Ett värde                | (device.organizationalUnit - eq ””)                          |
| deviceId                | en giltig deviceId                | (device.deviceId - eq ”d4fe7726-5966-431c-b3b8-cddc8fdb717d”) |
| objekt-ID                | en giltig AAD-objectId            | (device.objectId - eq ”76ad43c9-32c5-45e8-a272-7b58b58f596d”) |

> [!NOTE]
> Reglerna enheten kan inte skapas med hello ”enkel regel” listrutan i hello klassiska Azure-portalen.
>
>

## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Felsöka dynamiskt medlemskap för grupper](active-directory-accessmanagement-troubleshooting.md)
* [Hantera åtkomst tooresources med Azure Active Directory-grupper](active-directory-manage-groups.md)
* [Azure Active Directory-cmdletar för att konfigurera gruppinställningar](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
