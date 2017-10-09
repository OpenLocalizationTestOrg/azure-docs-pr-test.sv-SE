---
title: aaaConnector versionshistorik | Microsoft Docs
description: "Det här avsnittet listar alla versioner av hello kopplingar för Forefront Identity Manager (FIM) och Microsoft Identity Manager (MIM)"
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.openlocfilehash: 3522f17c30e46542eaa367ecdefdfd2fc47f71a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connector-version-release-history"></a>Versionshistorik för anslutningsappen
hello kopplingar för Forefront Identity Manager (FIM) och Microsoft Identity Manager (MIM) uppdateras ofta.

> [!NOTE]
> Det här avsnittet finns bara på FIM och MIM. Följande kopplingar stöds inte för installation på Azure AD Connect. Utgiven kopplingar är förinstallerat på AADConnect när du uppgraderar toospecified Build.

Det här avsnittet listas alla versioner av hello kopplingar som har släppts.

Relaterade länkar:

* [Hämta senaste kopplingar](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Allmän LDAP Connector](active-directory-aadconnectsync-connector-genericldap.md) refererar dokumentationen
* [Allmän SQL Connector](active-directory-aadconnectsync-connector-genericsql.md) refererar dokumentationen
* [Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) refererar dokumentationen
* [PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) refererar dokumentationen
* [Kopplingen för Lotus Domino](active-directory-aadconnectsync-connector-domino.md) refererar dokumentationen


## <a name="116040-aadconnect-pending-release"></a>1.1.604.0 (AADConnect väntande versionen)


### <a name="fixed-issues"></a>Fast problem:

* Allmän Web Services:
  * Ett problem som förhindrar att ett SOAP-projekt som skapas när det finns två eller fler slutpunkter har åtgärdats.
* Allmän SQL:
  * Hello driften av import hello GSQL inte konvertera tid korrekt när sparade tooconnector utrymme. hello datum och tid standardformat för anslutarplats för hello GSQL har ändrats från ”åååå-MM-dd: ssZ' too'yyyy-MM-dd: ssZ '.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Fast problem:

* Allmän Web Services:
  * Hej Wsconfig verktyget konverterades inte korrekt hello Json-matris från ”exempelbegäran” för hello metod för REST-tjänst. Detta orsakade problem med serialisering denna Json-matris för hello REST-begäran.
  * Web Service Connector Configuration Tool stöder inte användning av diskutrymme symboler i JSON-attributnamn 
    * Ett mönster för ersättning kan läggas till manuellt toohello WSConfigTool.exe.config fil, t.ex.```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```

* Lotus Notes:
  * När hello alternativet **Tillåt anpassade certifiers för enheter i organisationen/organiserad** inaktiveras hello anslutningen misslyckas under export (uppdatering) när hello export flöda alla attribut är exporterade tooDomino men när hello Exportera en KeyNotFoundException returneras tooSync. 
    * Detta inträffar eftersom hello Byt namn på åtgärden misslyckas när den försöker toochange DN (användarnamn attribut) genom att ändra en hello attribut nedan:  
      - Efternamn
      - Förnamn
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - organisationsenhet
      - altcommonname

  * När **Tillåt anpassade certifiers för enheter i organisationen/organiserad** alternativ är aktiverat, men krävs certifiers är fortfarande tom sedan KeyNotFoundException inträffar.

### <a name="enhancements"></a>Förbättringar av:

* Allmän SQL:
  * **Scenario: gjorts implementerat:** ”*” funktionen
  * **Lösningsbeskrivning av:** ändras tillvägagångssätt för [med flera värden referens attribut hantering](active-directory-aadconnectsync-connector-genericsql.md).


### <a name="fixed-issues"></a>Fast problem:

* Allmän Web Services:
  * Kan inte importera serverkonfigurationen om WebService koppling finns
  * WebService-anslutningen fungerar inte med flera webbtjänster

* Allmän SQL:
  * Inga objekt av typen listas för enstaka värde refererade attribut
  * Deltaimport för ändringsspårning strategi tar bort objekt när värdet tas bort från Flervärde tabell
  * OverflowException i GSQL connector med DB2 på AS / 400

Lotus:
  * Tillagda alternativet tooenable\disable söker organisationsenheter innan du öppnar GlobalParameters sidan

## <a name="114430"></a>1.1.443.0

Utgiven: 2017 mars

### <a name="enhancements"></a>Förbättringar

* Allmän SQL:</br>
  **Scenariot Symptom:** det är en välkänd begränsning med hello SQL-anslutningen där vi endast tillåta en referenstyp tooone objekt och kräver korsreferens med medlemmar. </br>
  **Lösningsbeskrivning av:** hello bearbetningssteg för referenser fanns ”*” alternativet är valt kan alla kombinationer av objekt av typen returneras tillbaka toohello Synkroniseringsmotorn.

>[!Important]
- Detta skapar många platshållare
- Det är obligatoriskt toomake att hello namn är unikt mellan objekttyper.


* Allmän LDAP:</br>
 **Scenario:** när endast några behållare har markerats i en specifik partition, sedan hello Sök fortfarande görs i hela partition. Specifika filtreras som synkroniseringstjänsten, men inte av MA vilket kan leda till försämrade prestanda. </br>

 **Lösningsbeskrivning av:** ändras GLDAP connector kod toomake det möjligt gå igenom alla behållare och söka efter objekt i var och en av dem, i stället för att söka i hela hello-partitionen.


* Lotus Domino:

  **Scenario:** Domino mail borttagning stöd för en person tas bort vid en export. </br>
  **Lösning:** konfigurerbara e borttagning stöd för en person tas bort vid en export.

### <a name="fixed-issues"></a>Fast problem:
* Allmän Web Services:
 * När du ändrar hello-tjänstens URL i SAP wsconfig projekt via webbtjänsten konfigurationsverktyget sedan händer hello följande fel: hittade inte en del av hello sökväg

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Allmän LDAP:
 * GLDAP Connector kan inte se alla attribut i AD LDS
 * Guiden radbrytningar när inga UPN-attribut identifieras från hello LDAP-directory-schemat
 * Delta importen misslyckas med identifiering av fel som inte finns vid fullständig import när attributet ”objectclass” inte är markerat
 * En ”konfigurera partitioner och hierarkier” konfigurationssidan visar inte alla objekt som är vilken typ som är lika toohello partition för nya servrar i hello generisk  
LDAP-MA. De visade endast objekt från RootDSE partition.


* Allmän SQL:
 * Korrigering för allmän SQL vattenstämpel Deltaimport flervärdesattribut inte importera programfel
 * När du exporterar deleted\added värdena för flervärdesattribut, men de är inte deleted\added i datakällan.  


* Lotus Notes:
 * Ett fält ”Full” visas i hello metaversum korrekt men när du exporterar tooNotes hello värde för hello-attributet är Null eller tom.
 * Korrigering för Certifier dubblettfel
 * När hello objekt utan data är markerad på hello kopplingen för Lotus Domino med andra objekt sedan felmeddelande vi hello identifiering när du utför en fullständig Import.
 * När Deltaimport körs på hello kopplingen för Lotus Domino hello slutet av den kör, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service ibland returnerar ett programfel.
 * Gruppen medlemskap övergripande fungerar bra och underhålls, förutom när du kör hello export tootry tooremove en användare från medlemskap det visas som slutförd med en uppdatering, men hello användaren inte komma bort från medlemskap i Lotus Notes.
 * Ett möjlighet toochoose läge exporten som ”Lägg till objekt längst ned” lades till i configuration GUI för Lotus MA tooappend nya objekt längst ned under hello export för attribut med flera värden.
 * Anslutningen läggs hello behövs logik toodelete hello filen från hello e-postmappen och ID-valvet.
 * Ta bort medlemskap som inte fungerar för mellan NAB medlem.
 * Värden ska vara har tagits bort från flervärdesattribut

## <a name="111170"></a>1.1.117.0
Utgiven: 2016 mars

**Ny koppling**  
Inledande versionen av hello [allmänna SQL Connector](active-directory-aadconnectsync-connector-genericsql.md).

**Nya funktioner:**

* Allmän LDAP Connector:
  * Stöd för Deltaimport med Isode har lagts till.
* Web Services Connector:
  * Uppdaterade hello csEntryChangeResult aktivitet och setImportErrorCode aktivitet tooallow objektet nivån fel toobe returnerade tillbaka toohello Synkroniseringsmotorn.
  * Uppdaterade hello SAP6 och SAP6User mallar toouse hello nya objekt nivån fel funktioner.
* Lotus Domino-anslutningen:
  * Du måste en certifier per adressbok för exporten. Du kan nu använda hello samma lösenord för alla certifiers toomake hello management enklare.

**Fast problem:**

* Allmän LDAP Connector:
  * För IBM Tivoli DS identifierades vissa referensattribut på rätt sätt.
  * Mellanslag i hello början och slutet av strängar för öppna LDAP under en Deltaimport har trunkerats.
  * För Novell och NetIQ export som flytta ett objekt mellan organisationsenheter-behållare och hello samma tid som har bytt namn till hello objekt misslyckades.
* Web Services Connector:
  * Om hello webbtjänsten har flera slutpunkter för samma bindning, sedan hello Connector inte korrekt identifiera dessa slutpunkter.
* Lotus Domino-anslutningen:
  * Export av hello fullName attributet tooa e-post i databasen fungerar inte.
  * Export som både läggas till och ta bort medlemmen från en grupp bara exporterade hello lägga till medlemmar.
  * Om ett Notes-dokument är ogiltig (hello attributet isValid ange toofalse), hello anslutningen misslyckas.

## <a name="older-releases"></a>Äldre versioner
Före mars 2016 publicerades hello kopplingar som supportfrågor.

**Allmän LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597 September 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015 mars
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534 januari 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419 September 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014 mars

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419 September 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419 September 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597 September 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015 mars
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712 2014 augusti
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003 februari 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, oktober 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534 2013 augusti

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
