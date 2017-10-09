---
title: aaaAttribute-baserade dynamiskt medlemskap i Azure Active Directory | Microsoft Docs
description: "Hur toocreate avancerade regler för dynamiska gruppmedlemskap, inklusive stöds uttryck regeln operatorer och parametrar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fb434cc2-9a91-4ebf-9753-dd81e289787e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8cd06ed70433eff65401c67d7351d5dcc12a9dd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Skapa attributbaserade regler för dynamiska gruppmedlemskap i Azure Active Directory
I Azure Active Directory (Azure AD), kan du skapa avancerade regler tooenable komplexa attributbaserad dynamiskt medlemskap för grupper. Den här artikeln beskrivs hello attribut och syntax toocreate dynamiskt medlemskapsregler för användare eller enheter.

När alla attribut för användare eller enhet hello utvärderas alla dynamisk gruppregler i en katalog toosee om hello ändringen skulle leda till valfri grupp lägger till eller tar bort. Om en användare eller enhet uppfyller en regel för en grupp, läggs de som medlem i gruppen. Om de inte längre uppfyller hello regeln tas bort.

> [!NOTE]
> - Du kan skapa en regel för dynamiskt medlemskap för säkerhetsgrupper eller Office 365-grupper.
>
> - Den här funktionen kräver en Azure AD Premium P1-licens för varje medlem tillagda tooat åtminstone en dynamisk grupp.
>
> - Du kan skapa en dynamisk grupp för enheter eller användare, men du kan inte skapa en regel som innehåller både användare och enhetsobjekt.

> - Hello tillfället är det inte möjligt toocreate en enhetsgrupp baserat på det ägande användarens attribut. Medlemskapsregler för enheten kan bara referens omedelbar attribut enhetsobjekt i hello directory.

## <a name="toocreate-an-advanced-rule"></a>toocreate en avancerad regel
1. Logga in toohello [administrationscentret för Azure AD](https://aad.portal.azure.com) med ett konto som är en global administratör eller en användare kontoadministratör.
2. Välj **användare och grupper**.
3. Välj **alla grupper**.

   ![Öppna hello grupper bladet](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)
4. I **alla grupper**väljer **ny grupp**.

   ![Lägg till ny grupp](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)
5. På hello **grupp** bladet, ange ett namn och beskrivning för hello nya. Välj en **medlemskapstypen** antingen **dynamiska användaren** eller **dynamisk enhet**, beroende på om du vill toocreate en regel för användare eller enheter och välj sedan **Lägg till dynamiska frågan**. Hello-attribut som används för enheten regler, se [med attribut toocreate regler för enhetsobjekt](#using-attributes-to-create-rules-for-device-objects).

   ![Lägg till regel för dynamiskt medlemskap](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)
6. På hello **dynamiskt medlemskapsregler** bladet, ange din regel i hello **Lägg till dynamiskt medlemskap avancerade regeln** , tryck på RETUR och välj sedan **skapa** längst hello hello-bladet.
7. Välj **skapa** på hello **grupp** bladet toocreate hello grupp.

## <a name="constructing-hello-body-of-an-advanced-rule"></a>Hur du skapar hello brödtexten i en avancerad regel
hello avancerade regeln som du kan skapa för hello dynamiskt medlemskap för grupper är i grunden en binär uttryck som består av tre delar och resulterar i ett true eller false resultat. hello tre delar är:

* Vänster parametern
* Binär operator
* Höger konstant

En fullständig avancerade regeln ser ut ungefär toothis: (leftParameter binaryOperator ”RightConstant”), där hello inledande och avslutande parentes är valfria för hello hela binära uttrycket dubbla citattecken är valfria, krävs för hello rätt bara konstant när det är sträng och hello syntaxen för hello vänstra parametern är user.property. En avancerad regel kan bestå av fler än en binär uttryck avgränsade med hello- och- eller, och - inte logiska operatorer.

hello följande är exempel på ett korrekt angivet avancerad regel:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Hello fullständig lista över parametrar som stöds och uttryck regeln operatorer, finns i avsnitten nedan. Hello-attribut som används för enheten regler, se [med attribut toocreate regler för enhetsobjekt](#using-attributes-to-create-rules-for-device-objects).

hello totala hello brödtexten i avancerade regeln kan inte vara längre än 2048 tecken.

> [!NOTE]
> Sträng och regex åtgärder är inte skiftlägeskänsliga. Du kan också utföra Null-kontroller, till exempel med $null som en konstant, user.department - eq $null.
> Strängar som innehåller citattecken ”ska avgränsas med-tecken, till exempel user.department - eq \`” försäljning ”.

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

Nedan visas alla operatorer per prioritet från lägre toohigher. Operatorer på samma rad har samma prioritet:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Alla operatorer kan användas med eller utan hello bindestreck prefix. Parenteser krävs bara när prioritet inte uppfyller dina krav.
Exempel:
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
motsvarar:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-hello--in-and--notin-operators"></a>Med hjälp av hello - i och - notIn operatorer

Om du vill toocompare hello värdet för ett användarattribut mot ett antal olika värden kan du använda hello - i eller notIn - operatorer. Här är ett exempel med hjälp av hello - i-operatorn:
```
    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]
```
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
```
   user.mail –ne null
```
motsvarar
```
   user.mail –ne $null
   ```

## <a name="extension-attributes-and-custom-attributes"></a>Tilläggsattribut och anpassade attribut
Tilläggsattribut och anpassade attribut stöds i regler för dynamiskt medlemskap.

Tilläggsattribut synkroniseras från lokala Windows Server AD och vidta hello format ”ExtensionAttributeX”, där X är lika med 1 och 15.
Ett exempel på en regel som använder ett tillägg-attribut är
```
(user.extensionAttribute15 -eq "Marketing")
```
Anpassade attribut som synkroniseras från lokala Windows Server AD eller från en ansluten SaaS-program och hello hello formatering av ”user.extension_[GUID]\__ [attribut]”, där [GUID] är hello Unik identifierare i AAD för hello program som skapade hello-attributet i AAD och [attribut] är hello hello attributets namn som det skapades.
Är ett exempel på en regel som använder ett anpassat attribut
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
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
Du kan också skapa en regel som väljer enhetsobjekten för medlemskap i en grupp. hello följande attribut för enheten kan användas.

 Attribut för enheten  | Värden | Exempel
 ----- | ----- | ----------------
 accountEnabled | SANT FALSKT | (device.accountEnabled - eq SANT)
 Visningsnamn | Ett värde |(device.displayName - eq ”Anders Iphone”)
 DeviceOSType | Ett värde | (device.deviceOSType - eq ”IOS”)
 DeviceOSVersion | Ett värde | (enhet. OSVersion - eq ”9.1”)
 deviceCategory | ett giltigt enhetsnamn kategori | (device.deviceCategory - eq ”BYOD”)
 DeviceManufacturer | Ett värde | (device.deviceManufacturer - eq ”Samsung”)
 DeviceModel | Ett värde | (device.deviceModel - eq ”iPad luften”)
 deviceOwnership | Privat, företag | (device.deviceOwnership - eq ”företagets”)
 Domännamn | Ett värde | (device.domainName - eq ”contoso.com”)
 enrollmentProfileName | Profilnamn för Apple enheten registreringen | (device.enrollmentProfileName - eq ”DEP iPhone”)
 isRooted | SANT FALSKT | (device.isRooted - eq SANT)
 managementType | MDM (för mobila enheter)<br>PC (för datorer som hanteras av hello Intune PC-agent) | (device.managementType - eq ”MDM”)
 OrganizationalUnit | ett värde matchar hello namnet på hello organisationsenhet som angetts av en lokal Active Directory | (device.organizationalUnit - eq ”USA datorer”)
 deviceId | en giltig enhets-ID för Azure AD | (device.deviceId - eq ”d4fe7726-5966-431c-b3b8-cddc8fdb717d”)
 objekt-ID | en giltig Azure AD-objekt-ID |  (device.objectId - eq 76ad43c9-32c5-45e8-a272-7b58b58f596d ”)




## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om grupper i Azure Active Directory.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Skapa en ny grupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemskap i en grupp](active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
