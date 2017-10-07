---
title: "aaaWriting uttryck för attributmappning i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toouse uttryck mappningar tootransform attributet värden i ett acceptabelt format vid automatisk etablering med SaaS-app i Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: markvi
ms.openlocfilehash: caa0dd8144f6e5279a869e015ed75bd24169d585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Skriva uttryck för attributmappning i Azure Active Directory
När du konfigurerar etablering tooa SaaS-program är hello typer av attributmappning som du kan ange mappningen för en uttryck. För dessa måste du skriva ett skript-liknande uttryck som du kan använda tootransform användarnas data i format som är mer godkänd för hello SaaS-program.

## <a name="syntax-overview"></a>Syntax: översikt
hello syntax för uttryck för attributmappning är påminner om Visual Basic för Applications (VBA)-funktioner.

* hello helt uttryck måste definieras vad gäller funktioner, som består av ett namn följt av argument inom parentes: <br>
  *FunctionName (<< argument 1 >> <<argument N>>)*
* Du kan kapsla funktioner i varandra. Exempel: <br> *FunctionOne (FunctionTwo (<<argument1>>))*
* Du kan skicka tre olika typer av argument till funktioner:
  
  1. Attribut måste stå inom klamrar kvadratisk. Exempel: [attributeName]
  2. Sträng som måste omges av dubbla citattecken. Till exempel: ”USA”
  3. Andra funktioner. Till exempel: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* För strängkonstanter, om du behöver ett omvänt snedstreck (\) eller citattecken (”) i hello sträng, måste den föregås hello omvänt snedstreck (\) symbolen. Till exempel ”: Företagsnamn: \"Contoso\"”

## <a name="list-of-functions"></a>Lista över funktioner
[Lägg till](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [ansluta](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; [inte](#not) &nbsp; &nbsp; &nbsp; &nbsp; [ersätta](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [växel](#switch)

- - -
### <a name="append"></a>Lägg till
**Funktionen:**<br> Append(Source, suffix)

**Beskrivning:**<br> Tar ett strängvärde för källa och lägger till hello suffix toohello slutet av den.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Sträng |Vanligtvis namnet på hello attribut från hello källobjektet |
| **suffix** |Krävs |Sträng |hello sträng som du vill tooappend toohello slutet av hello källvärdet. |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Funktionen:**<br> FormatDateTime (källa, inputFormat, outputFormat)

**Beskrivning:**<br> Tar en datumsträng från ett format och konverterar den till ett annat format.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Sträng |Vanligtvis hello attributets namn från hello källobjektet. |
| **inputFormat** |Krävs |Sträng |Förväntade format för hello källvärdet. Format som stöds, se [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Krävs |Sträng |Format för hello Utdatadatum. |

- - -
### <a name="join"></a>Slå ihop
**Funktionen:**<br> Ansluta till (avgränsare, källa1, källa2...)

**Beskrivning:**<br> JOIN() är liknande tooAppend(), förutom att den kan kombinera flera **källa** sträng värden till en sträng, och varje värde skiljs åt av en **avgränsare** sträng.

Om någon av hello källvärden är ett attribut med flera värden vara domänansluten tillsammans, åtskilda hello avgränsare värde varje värde i attributet.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **avgränsare** |Krävs |Sträng |Strängen används tooseparate källvärden när de sammanfogas till en sträng. Kan vara ”” om ingen avgränsare krävs. |
| ** källa1... Källan ** |Obligatoriska variabeln antal gånger |Sträng |Sträng värden toobe sammankopplade. |

- - -
### <a name="mid"></a>Mid
**Funktionen:**<br> MID (källa, start, length)

**Beskrivning:**<br> Returnerar en understräng av hello källvärdet. En understräng är en sträng som innehåller endast en del av hello tecken från hello Källsträngen.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Sträng |Vanligtvis hello attributets namn. |
| **start** |Krävs |heltal |Index i hello **källa** strängen där delsträngen ska starta. Första tecknet i hello strängen har index 1, andra tecknet kommer har index 2 och så vidare. |
| **längd** |Krävs |heltal |Längden på hello delsträngen. Om längden slutar utanför hello **källa** sträng, funktionen returnerar delsträngen från **starta** indexet till slutet av **källa** sträng. |

- - -
### <a name="not"></a>inte
**Funktionen:**<br> Not(Source)

**Beskrivning:**<br> Vändningar hello booleska värdet hello **källa**. Om **källa** värdet är ”*SANT*”, returnerar ”*FALSKT*”. Annars returnerar ”*SANT*”.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Booleskt sträng |Förväntade **källa** värden är ”True” eller ”False”... |

- - -
### <a name="replace"></a>Ersätt
**Funktionen:**<br> ObsoleteReplace (källa, oldValue, regexPattern, regexGroupName, ersättningsvärde, replacementAttributeName, mall)

**Beskrivning:**<br>
Ersätter värden i en sträng. Den fungerar på olika sätt beroende på hello parametrar som ges:

* När **oldValue** och **ersättningsvärde** tillhandahålls:
  
  * Ersätter alla förekomster av oldValue i hello källan med ersättningsvärde
* När **oldValue** och **mallen** tillhandahålls:
  
  * Ersätter alla förekomster av hello **oldValue** i hello **mallen** med hello **källa** värde
* När **oldValueRegexPattern**, **oldValueRegexGroupName**, **ersättningsvärde** tillhandahålls:
  
  * Ersätter alla värden som matchar oldValueRegexPattern i hello Källsträngen med ersättningsvärde
* När **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementPropertyName** tillhandahålls:
  
  * Om **källa** värde, **källa** returneras
  * Om **källa** har inte något värde, använder **oldValueRegexPattern** och **oldValueRegexGroupName** tooextract ersättningsvärde från hello egenskap med  **replacementPropertyName**. Ersättningsvärde returneras som hello resultat

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Sträng |Vanligtvis hello attributets namn från hello källobjektet. |
| **oldValue** |Valfri |Sträng |Värdet toobe ersättas i **källa** eller **mallen**. |
| **regexPattern** |Valfri |Sträng |Regex-mönster för hello värdet toobe ersättas i **källa**. Eller, om replacementPropertyName används mönstret tooextract värde från egendom. |
| **regexGroupName** |Valfri |Sträng |Namnet på gruppen hello i **regexPattern**. Endast när replacementPropertyName används, kommer vi extrahera värdet för den här gruppen som ersättningsvärde från egendom. |
| **Ersättningsvärde** |Valfri |Sträng |Nytt värde tooreplace gamla med. |
| **replacementAttributeName** |Valfri |Sträng |Namnet på hello attributet toobe används för ersättningsvärde, när datakällan har inte något värde. |
| **mallen** |Valfri |Sträng |När **mallen** värde har angetts, kommer vi att leta efter **oldValue** inuti hello mall och Ersätt den med källvärdet. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Funktionen:**<br> StripSpaces(source)

**Beskrivning:**<br> Tar bort alla blanksteg (””) tecken från hello datakällan sträng.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Sträng |**källan** värdet tooupdate. |

- - -
### <a name="switch"></a>Växel
**Funktionen:**<br> Växel (källa, defaultValue, key1, värde1, key2, värde2,...)

**Beskrivning:**<br> När **källa** värdet matchar en **nyckeln**, returnerar **värdet** för som **nyckeln**. Om **källa** värdet matchar inte några nycklar, returnerar **defaultValue**.  **Nyckeln** och **värdet** parametrar måste alltid komma parvis. hello förväntar alltid sig ett jämnt antal parametrar.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **källa** |Krävs |Sträng |**Källan** värdet tooupdate. |
| **Standardvärde** |Valfri |Sträng |Standard värdet toobe används när datakällan inte matchar några nycklar. Kan vara en tom sträng (””). |
| **nyckel** |Krävs |Sträng |**Nyckeln** toocompare **källa** värde med. |
| **värdet** |Krävs |Sträng |Ersättningsvärde för hello **källa** motsvarande hello-nyckel. |

## <a name="examples"></a>Exempel
### <a name="strip-known-domain-name"></a>Remsans kända domännamn
Du måste toostrip ett känt domännamn från en användares e-tooobtain ett användarnamn. <br>
Till exempel om hello domänen är ”contoso.com”, kan du använda hello följande uttryck:

**Uttryck:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Exempel på indata / utdata:** <br>

* **INDATA** (e) ”:john.doe@contoso.com”
* **UTDATA**: ”john.doe”

### <a name="append-constant-suffix-toouser-name"></a>Lägga till namnet på konstant suffix toouser
Om du använder en Salesforce Sandbox kanske du måste tooappend en ytterligare suffix tooall ditt användarnamn innan du synkroniserar dem.

**Uttryck:** <br>
`Append([userPrincipalName], ".test"))`

**I/o-exempel:** <br>

* **INDATA**: (userPrincipalName) ”:John.Doe@contoso.com”
* **UTDATA**”:John.Doe@contoso.com.test”

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Skapa användaralias genom att sammanbinda delar av för- och efternamn
Du måste toogenerate ett användaralias genom att först 3 bokstäver i användarens förnamn och 5 första bokstäver i användarens efternamn.

**Uttryck:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**I/o-exempel:** <br>

* **INDATA** (givenName): ”John”
* **INDATA** (efternamn): ”Berg”
* **UTDATA**: ”JohDoe”

### <a name="output-date-as-a-string-in-a-certain-format"></a>Utdatadatum som en sträng i ett visst format
Vill du toosend datum tooa SaaS-program i ett visst format. <br>
Till exempel vill du tooformat datum för ServiceNow.

**Uttryck:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**I/o-exempel:**

* **INDATA** (extensionAttribute1): ”20150123105347.1Z”
* **UTDATA**: ”2015-01-23”

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Ersätt ett värde baserat på fördefinierade uppsättning alternativ
Du måste toodefine hello tidszon hello användare baserat på hello statuskod som lagras i Azure AD. <br>
Om hello statuskod inte matchar någon av hello fördefinierade alternativ, använder du standardvärdet ”Australien/Sydney”.

**Uttryck:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**I/o-exempel:**

* **INDATA** (tillstånd): ”QLD”
* **UTDATA**: ”Australien/Brisbane”

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Automatisera användaren etablering/avetablering tooSaaS appar](active-directory-saas-app-provisioning.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Omfångsfilter för Användaretablering](active-directory-saas-scoping-filters.md)
* [Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar](active-directory-saas-tutorial-list.md)

