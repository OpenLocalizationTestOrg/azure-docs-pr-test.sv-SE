---
title: 'Azure AD Connect-synkronisering: funktioner referens | Microsoft Docs'
description: "Referens för uttryck för deklarativ etablering i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: fbe0df856ca2efda965650fb85c7e831a0be32c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect-synkronisering: funktioner referens
I Azure AD Connect är funktioner används toomanipulate ett attributvärde under synkroniseringen.  
Hej för hello uttrycks med hello följande format:  
`<output type> FunctionName(<input type> <position name>, ..)`

Om en funktion är överbelastad och accepterar flera syntax, visas alla giltig syntax.  
hello funktioner är starkt typbestämd och de bekräftar att hello typ som skickades matchar hello dokumenterade typen.  
Ett fel returneras om hello typ inte matchar.

hello typer uttrycks med hello följande syntax:

* **bin** – binära
* **bool** – booleskt
* **DT** – UTC-datum/tid
* **uppräkningen** – uppräkning av kända konstanter
* **EXP** – uttryck, vilket är förväntat tooevaluate tooa booleskt värde
* **mvbin** – multivärdes binära
* **mvstr** – multivärdes sträng
* **mvref** – multivärdes-referens
* **NUM** – numeriskt
* **REF** – referens
* **str** – sträng
* **var** – en variant av (nästan) en annan typ
* **void** – returnerar inte ett värde

Hej funktioner med hello typer **mvbin**, **mvstr**, och **mvref** fungerar bara på flera värden attribut. Fungerar med **bin**, **str**, och **ref** fungerar på både enstaka och flera värden.

## <a name="functions-reference"></a>Referens för funktioner
| Lista över funktioner |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| **Certifikat** | | | | |
| [CertExtensionOids](#certextensionoids) |[CertFormat](#certformat) |[CertFriendlyName](#certfriendlyname) |[CertHashString](#certhashstring) | |
| [CertIssuer](#certissuer) |[CertIssuerDN](#certissuerdn) |[CertIssuerOid](#certissueroid) |[CertKeyAlgorithm](#certkeyalgorithm) | |
| [CertKeyAlgorithmParams](#certkeyalgorithmparams) |[CertNameInfo](#certnameinfo) |[CertNotAfter](#certnotafter) |[CertNotBefore](#certnotbefore) | |
| [CertPublicKeyOid](#certpublickeyoid) |[CertPublicKeyParametersOid](#certpublickeyparametersoid) |[CertSerialNumber](#certserialnumber) |[CertSignatureAlgorithmOid](#certsignaturealgorithmoid) | |
| [CertSubject](#certsubject) |[CertSubjectNameDN](#certsubjectnamedn) |[CertSubjectNameOid](#certsubjectnameoid) |[CertThumbprint](#certthumbprint) | |
[CertVersion](#certversion) |[IsCert](#iscert) | | | |
| **Konvertering** | | | | |
| [CBool](#cbool) |[CDate](#cdate) |[CGuid](#cguid) |[ConvertFromBase64](#convertfrombase64) | |
| [ConvertToBase64](#converttobase64) |[ConvertFromUTF8Hex](#convertfromutf8hex) |[ConvertToUTF8Hex](#converttoutf8hex) |[CNum](#cnum) | |
| [CRef](#cref) |[CStr](#cstr) |[GUIDFromString](#StringFromGuid) |[StringFromSid](#stringfromsid) | |
| **Datum / tid** | | | | |
| [DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[FormatDateTime](#formatdatetime) |[Nu](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Directory** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Utvärdering** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[IsEmpty](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[IsPresent](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Math** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Flera värden** | | | | |
| [Innehåller](#contains) |[Antal](#count) |[Objektet](#item) |[ItemOrNull](#itemornull) | |
| [Anslut dig](#join) |[RemoveDuplicates](#removeduplicates) |[Dela](#split) | | |
| **Programflöde** | | | | |
| [Fel](#error) |[IIF](#iif) |[Välj](#select) |[Växel](#switch) | |
| [Där](#where) |[Med](#with) | | | |
| **Text** | | | | |
| [GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Vänster](#left) |[Len](#len) |[LTrim](#ltrim) |[Mid](#mid) | |
| [PadLeft](#padleft) |[PadRight](#padright) |[PCase](#pcase) |[Ersätt](#replace) | |
| [ReplaceChars](#replacechars) |[Höger](#right) |[RTrim](#rtrim) |[Rensa](#trim) | |
| [UCase](#ucase) |[Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Beskrivning:**  
Hej BitAnd-funktion anger angivna bits på ett värde.

**Syntax:**  
`num BitAnd(num value1, num value2)`

* value1, value2: numeriska värden som ska vara AND'ed tillsammans

**Anmärkning:**  
Den här funktionen konverteras binär representation av båda parametrarna toohello och anger lite till:

* 0 – om något eller båda av hello motsvarande bitar i *mask* och *flaggan* är 0
* 1 – om båda hello motsvarande bits är 1.

Med andra ord returneras 0, utom när hello motsvarande bitarna i båda parametrarna är 1.

**Exempel:**  
`BitAnd(&HF, &HF7)`  
Returnerar 7 eftersom hexadecimala ”F” och ”F7” utvärdera toothis värde.

- - -
### <a name="bitor"></a>BitOr
**Beskrivning:**  
Hej BITOR-funktion anger angivna bits på ett värde.

**Syntax:**  
`num BitOr(num value1, num value2)`

* value1, value2: numeriska värden som ska vara sammansatta med or tillsammans

**Anmärkning:**  
Den här funktionen konverteras binär representation av båda parametrarna toohello och anger en bit too1 om något eller båda av hello motsvarande bitar i mask och flaggan är 1 och too0 om båda hello motsvarande bits är 0. Med andra ord returnerar 1, utom där hello motsvarande bitarna av båda parametrarna är 0.

- - -
### <a name="cbool"></a>CBool
**Beskrivning:**  
hello CBool funktionen returnerar ett booleskt värde baserat på hello utvärderas uttryck

**Syntax:**  
`bool CBool(exp Expression)`

**Anmärkning:**  
Om hello uttrycket utvärderas tooa noll, och sedan CBool returnerar True, annars returneras False.

**Exempel:**  
`CBool([attrib1] = [attrib2])`  

Returnerar True om båda attribut har hello samma värde.

- - -
### <a name="cdate"></a>CDate
**Beskrivning:**  
hello CDate funktionen returnerar UTC DateTime från en sträng. DateTime är inte en inbyggd attributtyp synkroniserade men används av vissa funktioner.

**Syntax:**  
`dt CDate(str value)`

* Värde: En sträng med ett datum, tid och du kan också tidszon

**Anmärkning:**  
hello returnerade strängen är alltid i UTC.

**Exempel:**  
`CDate([employeeStartTime])`  
Returnerar ett datetime-värde baserat på hello medarbetarens starttid

`CDate("2013-01-10 4:00 PM -8")`  
Returnerar en DateTime som representerar ”2013-01-11 12:00:00”








- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Beskrivning:**  
Returnerar hello Oid-värden för alla kritiska hello-tillägg för ett certifikatobjekt.

**Syntax:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certformat"></a>CertFormat
**Beskrivning:**  
Returnerar hello namnet på hello format för den här X.509v3-certifikat.

**Syntax:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Beskrivning:**  
Returnerar hello associerade alias för ett certifikat.

**Syntax:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certhashstring"></a>CertHashString
**Beskrivning:**  
Returnerar hello SHA1-hash-värdet för hello X.509v3-certifikat som en hexadecimal sträng.

**Syntax:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certissuer"></a>CertIssuer
**Beskrivning:**  
Returnerar hello namnet på hello certifikatutfärdaren som utfärdade hello X.509v3-certifikat.

**Syntax:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Beskrivning:**  
Returnerar hello huvudnamnet på hello certifikatutfärdare.

**Syntax:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Beskrivning:**  
Returnerar hello Oid för hello certifikatutfärdare.

**Syntax:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Beskrivning:**  
Returnerar information om hello nyckelalgoritm för den här X.509v3-certifikat som en sträng.

**Syntax:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Beskrivning:**  
Returnerar hello nyckelalgoritm parametrar för hello X.509v3-certifikat som en hexadecimal sträng.

**Syntax:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Beskrivning:**  
Returnerar hello ämne och Utfärdarens namn från ett certifikat.

**Syntax:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.
*   X509NameType: hello X509NameType värde för hello ämnet.
*   includesIssuerName: true tooinclude hello Utfärdarens namn; Annars, FALSKT.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Beskrivning:**  
Returnerar hello datum i lokal tid som ett certifikat är inte längre giltig.

**Syntax:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Beskrivning:**  
Returnerar hello datum i lokal tid som ett certifikat börjar gälla.

**Syntax:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Beskrivning:**  
Returnerar hello Oid för hello offentlig nyckel för hello X.509v3-certifikat.

**Syntax:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Beskrivning:**  
Returnerar hello Oid för hello offentliga nyckelparametrar för hello X.509v3-certifikat.

**Syntax:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Beskrivning:**  
Returnerar hello serienumret för hello X.509v3-certifikat.

**Syntax:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Beskrivning:**  
Returnerar hello Oid för hello algoritmen används toocreate hello signaturen för ett certifikat.

**Syntax:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certsubject"></a>CertSubject
**Beskrivning:**  
Hämtar hello unika ämnesnamnet från ett certifikat.

**Syntax:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Beskrivning:**  
Returnerar hello unika ämnesnamnet från ett certifikat.

**Syntax:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Beskrivning:**  
Returnerar hello Oid för hello ämnesnamnet från ett certifikat.

**Syntax:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certthumbprint"></a>certThumbprint
**Beskrivning:**  
Returnerar hello tumavtrycket för ett certifikat.

**Syntax:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="certversion"></a>CertVersion
**Beskrivning:**  
Returnerar hello X.509-Formatversion för ett certifikat.

**Syntax:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.

- - -
### <a name="cguid"></a>CGuid
**Beskrivning:**  
Hej CGuid funktionen konverterar hello strängrepresentation av en a binär representation tooits för GUID.

**Syntax:**  
`bin CGuid(str GUID)`

* En sträng formaterad i det här mönstret: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx eller {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>Contains
**Beskrivning:**  
hello innehåller funktionen returnerar en sträng i ett flervärdesattribut

**Syntax:**  
`num Contains (mvstring attribute, str search)`-skiftlägeskänsligt  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-skiftlägeskänsligt

* attributet: hello flervärdesattribut toosearch.
* Sök: string toofind i hello-attribut.
* Casetype: CaseInsensitive- eller CaseSensitive.

Returnerar index i hello flervärdesattribut där hello strängen hittades. 0 returneras om hello strängen inte hittas.

**Anmärkning:**  
För flera värden strängattribut hello sökningen hitta delsträngar hello värden.  
För referensattribut måste hello eftersökt sträng exakt matcha hello värdet toobe en matchning.

**Exempel:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Om hello proxyAddresses attribut har en primär e-postadress (anges med versaler ”SMTP”:), returnera hello proxyAddress attribut, annars returneras ett fel.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Beskrivning:**  
Hej ConvertFromBase64 funktionen konverterar hello angetts base64-kodad värdet tooa vanlig sträng.

**Syntax:**  
`str ConvertFromBase64(str source)`-förutsätter Unicode för kodning  
`str ConvertFromBase64(str source, enum Encoding)`

* källa: Base64-kodad sträng  
* Kodning: Unicode, ASCII, UTF8

**Exempel**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Båda exempel returnera ”*Hello world!*”

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Beskrivning:**  
Hej ConvertFromUTF8Hex funktionen konverterar hello angett Hex UTF8-kodade värdet tooa sträng.

**Syntax:**  
`str ConvertFromUTF8Hex(str source)`

* källa: UTF8 2 byte kodade förekomster av textsträngen

**Anmärkning:**  
hello skillnaden mellan den här funktionen och ConvertFromBase64([],UTF8) i resultatmängden hello är eget för hello DN-attribut.  
Det här formatet används av Azure Active Directory som unikt namn.

**Exempel:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Returnerar ”*Hello world!*”

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Beskrivning:**  
Hej ConvertToBase64 funktionen konverterar en sträng tooa Unicode base64-sträng.  
Konverterar hello-värdet för en matris med heltal tooits motsvarande strängrepresentation som kodats med Base64-siffror.

**Syntax:**  
`str ConvertToBase64(str source)`

**Exempel:**  
`ConvertToBase64("Hello world!")`  
Returnerar ”SABlAGwAbABvACAAdwBvAHIAbABkACEA”

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Beskrivning:**  
Hej ConvertToUTF8Hex funktionen konverterar en sträng tooa Hex UTF8-kodade värde.

**Syntax:**  
`str ConvertToUTF8Hex(str source)`

**Anmärkning:**  
hello utdataformatet för den här funktionen används av Azure Active Directory som DN attribute-format.

**Exempel:**  
`ConvertToUTF8Hex("Hello world!")`  
Returnerar 48656C6C6F20776F726C6421

- - -
### <a name="count"></a>Antal
**Beskrivning:**  
hello Count-funktionen returnerar hello antalet element i ett flervärdesattribut

**Syntax:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Beskrivning:**  
Hej CNum funktionen en sträng och returnerar en numerisk datatyp.

**Syntax:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef
**Beskrivning:**  
Konverterar en sträng tooa referensattribut

**Syntax:**  
`ref CRef(str value)`

**Exempel:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a>CStr
**Beskrivning:**  
hello CStr funktionen konverterar tooa strängdatatypen.

**Syntax:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* värde: kan vara ett numeriskt värde, referensattribut eller booleskt värde.

**Exempel:**  
`CStr([dn])`  
Kan returnera ”cn = Johan, dc = contoso, dc = com”

- - -
### <a name="dateadd"></a>DateAdd
**Beskrivning:**  
Returnerar ett datum som innehåller ett datum toowhich ett angivet tidsintervall har lagts till.

**Syntax:**  
`dt DateAdd(str interval, num value, dt date)`

* intervall: stränguttryck som är hello tidsintervall du vill tooadd. hello sträng måste ha något av följande värden hello:
  * åååå år
  * q kvartal
  * m månad
  * y dagen på året
  * d dag
  * w veckodag
  * ww vecka
  * h timme
  * n minut
  * s andra
* värde: hello antalet enheter som du vill tooadd. Det kan vara positivt (tooget datum i hello framtida) eller negativt (tooget datum i hello tidigare).
* datum: DateTime som representerar toowhich hello datumintervall har lagts till.

**Exempel:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Lägger till tre månader och returnerar ett datetime-värde som representerar ”2001-04-01”.

- - -
### <a name="datefromnum"></a>DateFromNum
**Beskrivning:**  
Hej DateFromNum funktionen konverterar ett värde i Annonsens datum format tooa datum och tid.

**Syntax:**  
`dt DateFromNum(num value)`

**Exempel:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Returnerar ett datetime-värde som representerar 2012-01-01 23:00:00

- - -
### <a name="dncomponent"></a>DNComponent
**Beskrivning:**  
Hej DNComponent funktionen returnerar hello värdet för en angiven DN-komponent från vänster.

**Syntax:**  
`str DNComponent(ref dn, num ComponentNumber)`

* DN: hello referens attributet toointerpret
* ComponentNumber: hello-komponenten i hello DN tooreturn

**Exempel:**  
`DNComponent([dn],1)`  
Om dn är ”cn = Johan, ou =...”, returneras Joe

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Beskrivning:**  
Hej DNComponentRev funktionen returnerar hello värdet för en angiven DN-komponent från höger (hello end).

**Syntax:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* DN: hello referens attributet toointerpret
* ComponentNumber - hello-komponenten i hello DN tooreturn
* Alternativ: DC – Ignorera alla komponenter med ”dc =”

**Exempel:**  
Om dn är ”cn = Joe, ou = Atlanta, ou = GA, ou = US, dc = contoso, dc = com” sedan  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Returnerar båda oss.

- - -
### <a name="error"></a>Fel
**Beskrivning:**  
hello fel funktion är används tooreturn ett anpassat fel.

**Syntax:**  
`void Error(str ErrorMessage)`

**Exempel:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Om hello attributet accountName inte finns genereras ett fel på hello-objekt.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Beskrivning:**  
Hej EscapeDNComponent funktionen tar en komponent i ett unikt namn och hoppas det så att den kan representeras i LDAP.

**Syntax:**  
`str EscapeDNComponent(str value)`

**Exempel:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Säkerställer hello-objekt kan skapas i en LDAP-katalog även om hello displayName attributet innehåller tecken som måste hoppas i LDAP.

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Beskrivning:**  
funktionen för hello FormatDateTime är används tooformat en DateTime tooa sträng med ett bestämt format

**Syntax:**  
`str FormatDateTime(dt value, str format)`

* värde: ett värde i hello DateTime-format
* format: en sträng som representerar hello format tooconvert till.

**Anmärkning:**  
Hej möjliga värden för hello-format finns här: [User-defined datum/tid-format (Format-funktionen)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Exempel:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Resulterar i ”2007-12-25”.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Kan medföra ”20140905081453.0Z”

- - -
### <a name="guid"></a>GUID
**Beskrivning:**  
hello funktionen GUID genererar en ny, slumpmässig GUID

**Syntax:**  
`str GUID()`

- - -
### <a name="iif"></a>IIF
**Beskrivning:**  
hello OOM returnerar ett av en uppsättning möjliga värden baserat på ett angivet villkor.

**Syntax:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* villkor: ett värde eller uttryck som kan utvärderas tootrue eller false.
* värde_om_sant: om hello villkoret utvärderas tootrue, hello returneras värdet.
* värde_om_falskt: om hello villkoret utvärderas toofalse, hello returneras värdet.

**Exempel:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Om hello användare är en intern, returnerar hello-alias för en användare med ”t-” läggs toohello början av det, annars returnerar hello användarens alias som är.

- - -
### <a name="instr"></a>InStr
**Beskrivning:**  
hello funktionen InStr hittar hello första förekomsten av en understräng i en sträng

**Syntax:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* stringcheck: string toobe genomsöks
* stringmatch: string toobe hittades
* Starta: starta position toofind hello delsträngen
* Jämför: i vbTextCompare eller i vbBinaryCompare

**Anmärkning:**  
Returnerar hello positionen där delsträngen hello hittades eller 0 om inte hittas.

**Exempel:**  
`InStr("hello quick brown fox","quick")`  
Evalues too5

`InStr("repEated","e",3,vbBinaryCompare)`  
Utvärderar too7

- - -
### <a name="instrrev"></a>InStrRev
**Beskrivning:**  
hello funktionen InStrRev hittar hello sista förekomsten av en understräng i en sträng

**Syntax:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* stringcheck: string toobe genomsöks
* stringmatch: string toobe hittades
* Starta: starta position toofind hello delsträngen
* Jämför: i vbTextCompare eller i vbBinaryCompare

**Anmärkning:**  
Returnerar hello positionen där delsträngen hello hittades eller 0 om inte hittas.

**Exempel:**  
`InStrRev("abbcdbbbef","bb")`  
Returnerar 7

- - -
### <a name="isbitset"></a>IsBitSet
**Beskrivning:**  
hello funktionen IsBitSet tester om lite är eller inte

**Syntax:**  
`bool IsBitSet(num value, num flag)`

* värde: ett numeriskt värde som är evaluated.flag: ett numeriskt värde som har hello bit toobe utvärderas

**Exempel:**  
`IsBitSet(&HF,4)`  
Returnerar SANT eftersom ”4”-biten är aktiverad i hello hexadecimalt värde ”F”

- - -
### <a name="isdate"></a>IsDate
**Beskrivning:**  
Om hello uttryck kan utvärderas som en DateTime-typ. sedan hello funktionen IsDate utvärderar tooTrue.

**Syntax:**  
`bool IsDate(var Expression)`

**Anmärkning:**  
Använda toodetermine om CDate() kan genomföras.

- - -
### <a name="iscert"></a>IsCert
**Beskrivning:**  
Returnerar true om hello rådata kan serialiseras till .NET X509Certificate2 certifikatobjekt.

**Syntax:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: Byte-matris representation av ett X.509-certifikat. hello byte-matris kan vara kodad binär (DER) eller Base64-kodad X.509-data.
- - -
### <a name="isempty"></a>IsEmpty
**Beskrivning:**  
Om hello attribut finns i hello CS eller MV men utvärderar tooan tom sträng, utvärderar hello IsEmpty funktionen tooTrue.

**Syntax:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Beskrivning:**  
Om hello strängen kan vara konverterade tooa GUID, utvärderas hello IsGuid funktionen tootrue.

**Syntax:**  
`bool IsGuid(str GUID)`

**Anmärkning:**  
Ett GUID som har definierats som en sträng som följer någon av dessa mönster: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx eller {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Använda toodetermine om CGuid() kan genomföras.

**Exempel:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Om hello StrAttribute har en GUID-format, returnera en a binär representation, annars returnerar Null.

- - -
### <a name="isnull"></a>IsNull
**Beskrivning:**  
Om hello uttrycket utvärderas tooNull, returnerar hello IsNull funktionen SANT.

**Syntax:**  
`bool IsNull(var Expression)`

**Anmärkning:**  
Ett null-värde uttrycks för ett attribut av hello hello-attributet saknas.

**Exempel:**  
`IsNull([displayName])`  
Returnerar True om hello attributet inte finns i hello CS eller MV.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Beskrivning:**  
Om hello uttryck är null eller en tom sträng, returnerar hello IsNullOrEmpty funktionen SANT.

**Syntax:**  
`bool IsNullOrEmpty(var Expression)`

**Anmärkning:**  
För ett attribut utvärderar detta tooTrue om hello-attribut saknas eller finns men är en tom sträng.  
hello inversen till den här funktionen kallas IsPresent.

**Exempel:**  
`IsNullOrEmpty([displayName])`  
Returnerar True om hello attributet finns inte eller är en tom sträng i hello CS eller MV.

- - -
### <a name="isnumeric"></a>IsNumeric
**Beskrivning:**  
hello funktionen IsNumeric returnerar ett booleskt värde som anger om ett uttryck kan utvärderas som tal av typen.

**Syntax:**  
`bool IsNumeric(var Expression)`

**Anmärkning:**  
Använda toodetermine om CNum() kan vara lyckade tooparse hello uttryck.

- - -
### <a name="isstring"></a>IsString
**Beskrivning:**  
Om hello uttrycket kan vara utvärderade tooa strängen typ, och sedan hello IsString evaluerar tooTrue.

**Syntax:**  
`bool IsString(var expression)`

**Anmärkning:**  
Använda toodetermine om CStr() kan vara lyckade tooparse hello uttryck.

- - -
### <a name="ispresent"></a>IsPresent
**Beskrivning:**  
Om hello uttrycket utvärderas tooa sträng som inte är Null och inte är tom, hello du IsPresent funktionen returnerar true.

**Syntax:**  
`bool IsPresent(var expression)`

**Anmärkning:**  
hello inversen till den här funktionen kallas IsNullOrEmpty.

**Exempel:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Objekt
**Beskrivning:**  
hello funktionen Item returnerar ett objekt från ett med flera värden strängattribut.

**Syntax:**  
`var Item(mvstr attribute, num index)`

* attributet: flervärdesattribut
* index: indexet tooan objekt i flera värden hello-sträng.

**Anmärkning:**  
hello funktionen Item används tillsammans med hello innehåller funktionen eftersom hello senare funktionen returnerar hello index tooan objektet i hello flervärdesattribut.

Genererar ett fel om index är utanför intervallet.

**Exempel:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Returnerar hello primära e-postadress.

- - -
### <a name="itemornull"></a>ItemOrNull
**Beskrivning:**  
Hej ItemOrNull funktionen returnerar ett objekt från ett med flera värden strängattribut.

**Syntax:**  
`var ItemOrNull(mvstr attribute, num index)`

* attributet: flervärdesattribut
* index: indexet tooan objekt i flera värden hello-sträng.

**Anmärkning:**  
Hej ItemOrNull funktionen är användbart tillsammans med hello innehåller funktionen eftersom hello senare funktionen returnerar hello index tooan objektet i hello flervärdesattribut.

Om index är utanför intervallet, returnerar du värdet Null.

- - -
### <a name="join"></a>Slå ihop
**Beskrivning:**  
hello koppling funktionen en sträng med flera värden och returnerar en enstaka sträng med angiven avgränsare infogas mellan varje element.

**Syntax:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* attributet: flervärdesattribut som innehåller strängar toobe ansluten.
* en avgränsare: någon sträng, används tooseparate hello delsträngar i hello returnerade sträng. Om det utelämnas används hello blanksteg (””) används. Om en avgränsare för en tom sträng (””) eller något, alla objekt i listan hello sammanfogas med ingen avgränsare.

**Kommentarer**  
Det finns paritet mellan hello koppling och dela funktioner. hello funktionen Join tar en matris med strängar och slår ihop dem med hjälp av en avgränsare sträng, tooreturn en enskild textsträng. hello dela funktionen en sträng och skiljer den på hello avgränsaren, tooreturn en matris med strängar. Viktigaste skillnaden är dock att kopplingen kan sammanfoga strängar med valfri avgränsare sträng, dela kan endast separata strängar som använder en avgränsare för en enstaka tecken.

**Exempel:**  
`Join([proxyAddresses],",")`  
Kan returnera ”:SMTP:john.doe@contoso.com,smtp:jd@contoso.com”

- - -
### <a name="lcase"></a>LCase
**Beskrivning:**  
hello funktionen LCase konverterar alla tecken i strängen toolower fall.

**Syntax:**  
`str LCase(str value)`

**Exempel:**  
`LCase("TeSt")`  
Returnerar ”test”.

- - -
### <a name="left"></a>vänster
**Beskrivning:**  
hello vänstra funktionen returnerar ett angivet antal tecken från hello till vänster i en sträng.

**Syntax:**  
`str Left(str string, num NumChars)`

* sträng: hello sträng tooreturn tecken från
* NumChars: ett tal som identifierar hello antal tecken tooreturn från hello början (vänster) av sträng

**Anmärkning:**  
En sträng som innehåller hello första numChars tecken i strängen:

* Om numChars = 0, returneras en tom sträng.
* Om numChars < 0, returnerar Indatasträngen.
* Om strängen är null returnera tom sträng.

Om strängen innehåller färre tecken än hello antal som anges i numChars, returneras en sträng identiska toostring (som är, som innehåller alla tecken i parameter 1).

**Exempel:**  
`Left("John Doe", 3)`  
Returnerar ”Joh”.

- - -
### <a name="len"></a>Len
**Beskrivning:**  
hello funktionen längd returnerar antalet tecken i en sträng.

**Syntax:**  
`num Len(str value)`

**Exempel:**  
`Len("John Doe")`  
Returnerar 8

- - -
### <a name="ltrim"></a>LTrim
**Beskrivning:**  
hello LTrim funktionen tar bort inledande blanksteg från en sträng.

**Syntax:**  
`str LTrim(str value)`

**Exempel:**  
`LTrim(" Test ")`  
Returnerar ”Test”

- - -
### <a name="mid"></a>Mid
**Beskrivning:**  
Hej Mid funktionen returnerar ett angivet antal tecken från en angiven position i en sträng.

**Syntax:**  
`str Mid(str string, num start, num NumChars)`

* sträng: hello sträng tooreturn tecken från
* Starta: ett tal som identifierar hello startposition i strängen tooreturn tecken från
* NumChars: ett tal som identifierar hello antalet tooreturn tecken från position i strängen

**Anmärkning:**  
Returnera numChars tecken med början från början position i strängen.  
En sträng som innehåller numChars tecken från början position i strängen:

* Om numChars = 0, returneras en tom sträng.
* Om numChars < 0, returnerar Indatasträngen.
* Om start > Hej strängens längd, returnera Indatasträngen.
* Om starta < = 0, returnera Indatasträngen.
* Om strängen är null returnera tom sträng.

Om det inte numChar tecken returneras i strängen position, så många tecken som möjligt.

**Exempel:**  
`Mid("John Doe", 3, 5)`  
Returnerar ”hn gör”.

`Mid("John Doe", 6, 999)`  
Returnerar ”Berg”

- - -
### <a name="now"></a>Nu
**Beskrivning:**  
hello nu returnerar funktionen DateTime anger hello aktuellt datum och tid, enligt tooyour datorns datum och klockslag.

**Syntax:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Beskrivning:**  
Hej NumFromDate funktionen returnerar ett datum i Annonsens datumformat.

**Syntax:**  
`num NumFromDate(dt value)`

**Exempel:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Returnerar 129699324000000000

- - -
### <a name="padleft"></a>padLeft
**Beskrivning:**  
Hej PadLeft fungerar vänster Pad en sträng tooa som angetts med hjälp av angivna utfyllnadstecknet längd.

**Syntax:**  
`str PadLeft(str string, num length, str padCharacter)`

* sträng: hello sträng toopad.
* längd: ett heltal som representerar hello önskad strängens längd.
* padCharacter: en sträng som består av ett enskilt tecken toouse som hello pad tecken

**Anmärkning:**

* Om hello längden på strängen är mindre än längden, är padCharacter upprepade gånger tillagda toohello början (vänster) av strängen förrän den har en lika lång toolength.
* padCharacter kan vara ett blanksteg, men den kan inte vara ett null-värde.
* Om hello längden på strängen är lika tooor som är större än längden, returneras strängen oförändrat.
* Om strängen har en längd som är större än eller lika toolength, returneras en identisk toostring sträng.
* Om hello längden på strängen är mindre än längden, önskade en ny sträng av hello för längd returneras som innehåller strängen fylls ut med en padCharacter.
* Om strängen är null, returneras hello en tom sträng.

**Exempel:**  
`PadLeft("User", 10, "0")`  
Returnerar ”000000User”.

- - -
### <a name="padright"></a>PadRight
**Beskrivning:**  
Hej PadRight fungerar höger Pad en sträng tooa som angetts med hjälp av angivna utfyllnadstecknet längd.

**Syntax:**  
`str PadRight(str string, num length, str padCharacter)`

* sträng: hello sträng toopad.
* längd: ett heltal som representerar hello önskad strängens längd.
* padCharacter: en sträng som består av ett enskilt tecken toouse som hello pad tecken

**Anmärkning:**

* Om hello längden på strängen är mindre än längden, är padCharacter upprepade gånger tillagda toohello slutet (höger) av strängen förrän den har en lika lång toolength.
* padCharacter kan vara ett blanksteg, men den kan inte vara ett null-värde.
* Om hello längden på strängen är lika tooor som är större än längden, returneras strängen oförändrat.
* Om strängen har en längd som är större än eller lika toolength, returneras en identisk toostring sträng.
* Om hello längden på strängen är mindre än längden, önskade en ny sträng av hello för längd returneras som innehåller strängen fylls ut med en padCharacter.
* Om strängen är null, returneras hello en tom sträng.

**Exempel:**  
`PadRight("User", 10, "0")`  
Returnerar ”User000000”.

- - -
### <a name="pcase"></a>PCase
**Beskrivning:**  
Hej PCase funktionen konverterar hello första tecknet i varje blankstegsavgränsad ord i strängen tooupper fall och alla andra tecken konverteras toolower fallet.

**Syntax:**  
`String PCase(string)`

**Anmärkning:**

* Den här funktionen innehåller för närvarande inte rätt skiftläge tooconvert ett ord som är helt versaler, till exempel en förkortning.

**Exempel:**  
`PCase("TEsT")`  
Returnerar ”Test”.

`PCase(LCase("TEST"))`  
Returnerar ”Test”

- - -
### <a name="randomnum"></a>RandomNum
**Beskrivning:**  
Hej RandomNum funktionen returnerar ett slumptal mellan ett visst intervall.

**Syntax:**  
`num RandomNum(num start, num end)`

* Starta: antalet identifierande hello nedre gräns hello slumpmässigt värde toogenerate
* End: ett tal identifierande hello övre gränsen för hello slumpmässigt värde toogenerate

**Exempel:**  
`Random(100,999)`  
Returnera 734.

- - -
### <a name="removeduplicates"></a>RemoveDuplicates
**Beskrivning:**  
Hej RemoveDuplicates funktionen en sträng med flera värden och kontrollera att varje värde är unikt.

**Syntax:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Exempel:**  
`RemoveDuplicates([proxyAddresses])`  
Returnerar ett språkoberoende proxyAddress attribut där alla dubblettvärden har tagits bort.

- - -
### <a name="replace"></a>Ersätt
**Beskrivning:**  
hello Ersätt funktionen ersätter alla förekomster av en sträng tooanother sträng.

**Syntax:**  
`str Replace(str string, str OldValue, str NewValue)`

* sträng: en sträng tooreplace som värden i.
* OldValue: hello sträng toosearch för och tooreplace.
* NewValue: hello sträng tooreplace till.

**Anmärkning:**  
hello funktionen identifierar hello följa särskilda monikrar:

* \n – ny rad
* \r – vagnretur
* \t – fliken

**Exempel:**  
`Replace([address],"\r\n",", ")`  
Ersätter CRLF med ett kommatecken och ett blanksteg och leda för ”en Microsoft sätt, Redmond, WA, USA”

- - -
### <a name="replacechars"></a>ReplaceChars
**Beskrivning:**  
Hej ReplaceChars funktionen ersätter alla förekomster av tecken hittades i hello ReplacePattern sträng.

**Syntax:**  
`str ReplaceChars(str string, str ReplacePattern)`

* sträng: en sträng tooreplace tecken.
* ReplacePattern: en sträng som innehåller en ordlista med tooreplace tecken.

hello-formatet är {källa1}: {target1}, {källa2}: {target2}, {källan}, {targetN} där källan är hello tecken och mål för toofind hello sträng tooreplace med.

**Anmärkning:**

* hello funktionen tar för varje förekomst av definierade källor och ersätter dem med hello mål.
* hello källan måste vara exakt ett tecken för (unicode).
* hello källa vara inte tomt eller längre än ett tecken (tolkningsfel).
* hello mål kan ha flera tecken, till exempel ö:oe, β:ss.
* hello mål kan vara tomt som anger att hello tecken ska tas bort.
* hello-källa är skiftlägeskänsligt och måste vara en exakt matchning.
* hello, (semikolonavgränsad) och: (kolon) är reserverade tecken som inte kan ersättas med den här funktionen.
* Blanksteg och andra vit tecken i hello ReplacePattern strängen ignoreras.

**Exempel:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Returnerar Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Returnerar ”ONeil”, hello markering är definierad toobe tas bort.

- - -
### <a name="right"></a>Höger
**Beskrivning:**  
hello rätt funktion returnerar ett angivet antal tecken från hello rätt (end) i en sträng.

**Syntax:**  
`str Right(str string, num NumChars)`

* sträng: hello sträng tooreturn tecken från
* NumChars: ett tal som identifierar hello antal tecken tooreturn från hello slutpunkt (höger) av sträng

**Anmärkning:**  
NumChars tecken som ska returneras från hello senaste position i strängen.

En sträng som innehåller hello senaste numChars tecken i strängen:

* Om numChars = 0, returneras en tom sträng.
* Om numChars < 0, returnerar Indatasträngen.
* Om strängen är null returnera tom sträng.

Om strängen innehåller färre tecken än hello nummer anges i NumChars, returneras en identisk toostring sträng.

**Exempel:**  
`Right("John Doe", 3)`  
Returnerar ”Berg”.

- - -
### <a name="rtrim"></a>RTrim
**Beskrivning:**  
hello RTrim funktionen tar bort avslutande blanksteg från en sträng.

**Syntax:**  
`str RTrim(str value)`

**Exempel:**  
`RTrim(" Test ")`  
Returnerar ”Test”.

- - -
### <a name="select"></a>Välj
**Beskrivning:**  
Processen för alla värden i en flervärdesfält attribut (eller utdata för ett uttryck) baserat på funktionen som anges.

**Syntax:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* objektet: representerar ett element i hello flervärdesattribut
* attributet: hello flervärdesattribut
* uttryck: ett uttryck som returnerar en mängd med värden
* villkor: en funktion som kan bearbeta ett objekt i hello attribut

**Exempel:**  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
Returnera alla hello-värden i hello flervärdesattribut Annantelefon när bindestreck (-) har tagits bort.

- - -
### <a name="split"></a>Dela
**Beskrivning:**  
hello dela funktionen en sträng som avgränsas med en avgränsare och gör det en sträng med flera värden.

**Syntax:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* värde: sträng med en avgränsare tecken tooseparate hello.
* en avgränsare: enkel tecken toobe används som hello avgränsare.
* gränsen: maximala antalet värden som kan returnera.

**Exempel:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Returnerar en sträng som har flera värden med 2 element användbart för hello proxyAddress attribut.

- - -
### <a name="stringfromguid"></a>GUIDFromString
**Beskrivning:**  
hello GUIDFromString funktionen tar ett binärt GUID och konverterar den tooa sträng

**Syntax:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Beskrivning:**  
Hej StringFromSid funktionen konverterar en bytematris som innehåller en security identifier tooa sträng.

**Syntax:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Växel
**Beskrivning:**  
hello växelfunktionen är tooreturn används ett värde baserat på utvärderade villkoren.

**Syntax:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* uttryck: Variant-uttryck som du vill använda tooevaluate.
* värde: värdet toobe returneras om hello motsvarande uttryck är True.

**Anmärkning:**  
hello växeln funktionsargument listan består av par med uttryck och värden. hello uttryck utvärderas från vänster tooright och hello-värde som är associerade med hello första uttryck tooevaluate tooTrue returneras. Om hello delar inte är korrekt paras ihop, inträffar ett fel under körning.

Om Uttr1 är SANT returnerar växeln value1. Om uttryck-1 är False, men uttryck-2 är sant, returnerar växeln värdet 2 och så vidare.

Växeln returnerar en ingenting om:

* Ingen hälsningspaket uttryck är True.
* hello första SANT uttrycket har ett motsvarande värde som är Null.

Växeln utvärderar alla uttryck, även om den returnerar endast en av dem. Därför bör du titta på för oönskade sidoeffekter. Hello utvärderingen av ett uttryck som resulterar i en division med noll-fel, inträffar ett fel.

Värdet kan också vara hello fel funktion som returnerar en anpassad sträng.

**Exempel:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Returnerar hello-språk som talas i vissa större orter, annars returneras ett fel.

- - -
### <a name="trim"></a>Rensa
**Beskrivning:**  
Rensa hello-funktionen tar bort inledande och avslutande blanksteg från en sträng.

**Syntax:**  
`str Trim(str value)`  

**Exempel:**  
`Trim(" Test ")`  
Returnerar ”Test”.

`Trim([proxyAddresses])`  
Tar bort inledande och avslutande blanksteg för varje värde i hello proxyAddress attribut.

- - -
### <a name="ucase"></a>UCase
**Beskrivning:**  
hello funktionen UCase konverterar alla tecken i strängen tooupper fall.

**Syntax:**  
`str UCase(str string)`

**Exempel:**  
`UCase("TeSt")`  
Returnerar ”TEST”.

- - -
### <a name="where"></a>där

**Beskrivning:**  
Returnerar en delmängd av värden från ett med flera värden attribut (eller utdata för ett uttryck) som baserat på specifika villkor.

**Syntax:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* objektet: representerar ett element i hello flervärdesattribut
* attributet: hello flervärdesattribut
* villkor: ett uttryck som kan utvärderas tootrue eller false
* uttryck: ett uttryck som returnerar en mängd med värden

**Exempel:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Returvärden hello certifikat i hello flervärdesattribut userCertificate som inte har upphört att gälla.

- - -
### <a name="with"></a>med
**Beskrivning:**  
hello med funktionen ger ett sätt toosimplify ett komplext uttryck med hjälp av en variabel toorepresent ett deluttryck som visas en eller flera gånger i hello komplext uttryck.

**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`  
* variabel: representerar hello underuttryck.
* underuttryck: deluttryck som representeras av variabeln.
* complexExpression: ett komplext uttryck.

**Exempel:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
Har samma funktioner som:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
Som returnerar endast återstående certifikat värden i hello userCertificate attribut.


- - -
### <a name="word"></a>Word
**Beskrivning:**  
hello funktion returnerar ett ord som ingår i en sträng, baserat på parametrarna som beskriver hello avgränsare toouse och hello word nummer tooreturn.

**Syntax:**  
`str Word(str string, num WordNumber, str delimiters)`

* sträng: hello sträng tooreturn ett ord från.
* WordNumber: ett tal som identifierar vilka word-numret ska returnera.
* avgränsare: en sträng som representerar hello delimiter(s) som ska använda tooidentify ord

**Anmärkning:**  
Varje sträng med tecken i strängen avgränsade med hello hello tecken i avgränsare identifieras som ord:

* Om number < 1, returnerar tom sträng.
* Om strängen är null returnerar tom sträng.

Om strängen innehåller mindre än antalet ord eller strängen innehåller inte några ord som identifieras av avgränsare, returneras en tom sträng.

**Exempel:**  
`Word("hello quick brown fox",3," ")`  
Returnerar ”Jansson”

`Word("This,string!has&many separators",3,",!&#")`  
Returnerar ”har”

## <a name="additional-resources"></a>Ytterligare resurser
* [Förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Connect Sync: Anpassa synkroniseringsalternativ](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
