---
title: aaaPowerShell Connector | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfigure Microsoft Windows PowerShell Connector."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 44ff6b1f53283000b72e15f861e0f86c21afe12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-powershell-connector-technical-reference"></a>Teknisk referens för Windows PowerShell Connector
Den här artikeln beskriver hello Windows PowerShell Connector. hello artikeln gäller toohello följande produkter:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Måste använda snabbkorrigering 4.1.3671.0 eller senare [KB3092178](https://support.microsoft.com/kb/3092178).

För MIM2016 och FIM2010R2 hello Connector är tillgänglig för hämtning från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-hello-powershell-connector"></a>Översikt över hello PowerShell Connector
hello PowerShell Connector kan du toointegrate hello synkroniseringstjänsten med externa system som tillhandahåller Windows PowerShell-baserade API: er. hello koppling innehåller en brygga mellan hello funktionerna i hello anropsbaserade extensible connectivity hanteringsagenten 2 (ECMA2) framework och Windows PowerShell. Mer information om hello ECMA framework finns hello [Extensible Connectivity 2.2 Management Agent referens](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Krav
Innan du använder hello koppling, kontrollera att du har hello följande på hello synkroniseringsserver:

* 4.5.2 för Microsoft .NET Framework eller senare
* Windows PowerShell 2.0, 3.0 eller 4.0

hello körningsprincipen på hello synkroniseringstjänsten servern måste vara konfigurerade tooallow hello connector toorun Windows PowerShell-skript. Om inte är digitalt signerade hello skript hello connector körs, konfigurera hello körningsprincipen genom att köra det här kommandot:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Skapa en ny koppling
Du måste ange en serie med Windows PowerShell-skript som kör hello åtgärder som begärs av hello synkroniseringstjänsten toocreate en Windows PowerShell-koppling i hello synkroniseringstjänsten. Hello-skript som du måste implementera varierar beroende på hello datakälla du ansluta tooand hello funktioner som du behöver. Det här avsnittet ger en översikt över hello skript som kan implementeras och när de är obligatoriska.

hello utformad Windows PowerShell connector är toostore hello skript i hello synkroniseringstjänsten databasen. Även om det är möjligt toorun skript som är lagrade på hello filsystemet är enklare tooinsert hello brödtexten för varje skript direkt i toohello kopplingskonfiguration.

tooCreate en PowerShell-koppling i **synkroniseringstjänsten** Välj **hanteringsagenten** och **skapa**. Välj hello **PowerShell (Microsoft)** koppling.

![Skapa koppling](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Anslutning
Ange konfigurationsparametrar för att ansluta tooa fjärrsystemet. Dessa värden lagras av hello-synkroniseringstjänsten och blir tillgängliga tooyour Windows PowerShell-skript när hello connector körs på ett säkert sätt.

![Anslutning](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Du kan konfigurera hello följande parametrar för anslutningen:

**Anslutning**

| Parameter | Standardvärde | Syfte |
| --- | --- | --- |
| Server |<Blank> |Namn på servern som hello kopplingen ska ansluta till. |
| Domän |<Blank> |Domänen för hello autentiseringsuppgifter toostore för användning när hello connector körs. |
| Användare |<Blank> |Användarnamnet för hello autentiseringsuppgifter toostore för användning när hello connector körs. |
| Lösenord |<Blank> |Lösenordet för hello autentiseringsuppgifter toostore för användning när hello connector körs. |
| Personifiera Connector-kontot |False |Om värdet är true körs hello-synkroniseringstjänsten hello Windows PowerShell-skript i hello kontext hello autentiseringsuppgifterna. När det är möjligt bör den hello **$Credentials** parametern överförs tooeach skript används i stället för personifiering. För mer information om ytterligare behörigheter som krävs toouse det här alternativet, se [ytterligare konfiguration för personifiering](#additional-configuration-for-impersonation). |
| Läs in användarprofil vid personifiering |False |Instruerar tooload Windows hello användarprofil hello connector autentiseringsuppgifter under personifieringen. Om hello personifierade användaren har en central användarprofil, laddar inte hello connector hello centrala profil. Mer information om ytterligare behörigheter som krävs toouse den här parametern, finns [ytterligare konfiguration för personifiering](#additional-configuration-for-impersonation). |
| Inloggningstyp vid personifiering |Ingen |Inloggningstyp under personifieringen. Mer information finns i hello [dwLogonType] [ dw] dokumentation. |
| Signerade skript |False |Om värdet är true verifierar hello Windows PowerShell connector att varje skript har en giltig digital signatur. Om värdet är false, se till att hello synkroniseringstjänsten serverns Windows PowerShell-körningsprincipen RemoteSigned eller obegränsat. |

**Allmänna modulen**  
hello-anslutningen kan du toostore delade Windows PowerShell-modulen i hello konfiguration. När hello connector körs ett skript, extrahera hello Windows PowerShell-modulen är toohello filsystem så att den kan importeras till varje skript.

Hello vanliga modul är extraherade toohello connector MAData-mappen för Import, Export och synkronisering av lösenords-skript. För schemat, verifiering, hierarkin och Partition identifiering av skript är hello allmänna modulen extraherade toohello % TEMP %-mappen. I båda fallen kan extrahera hello allmänna modulen skriptet heter enligt toohello vanliga skript Modulnamn inställningen.

tooload en modul kallas FIMPowerShellConnectorModule.psm1 från hello MAData mappen, använder hello följande instruktion:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

tooload en modul anropas FIMPowerShellConnectorModule.psm1 från hello % TEMP %-mappen, använder hello följande instruktion:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Verifieringen av parametern**  
hello verifieringsskriptet är en valfri Windows PowerShell-skript som kan använda tooensure att kopplingen konfigurationsparametrar som tillhandahålls av Hej administratör är giltiga. Validera server, autentiseringsuppgifter för anslutning och anslutningen parametrar är vanliga användningsområden för hello verifieringsskriptet. hello anropas verifieringsskriptet efter hello följande flikar och dialogrutor ändras:

* Anslutning
* Globala parametrar
* Partitionen konfiguration

hello verifieringsskriptet tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Hej konfigurationsfliken eller en dialogruta som utlöste hello begäran. |
| ConfigParameters |[KeyedCollection] [ keyk] [sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |

hello verifieringsskriptet ska returnera en enda ParameterValidationResult objektet toohello pipeline.

**Identifiering av schemat**  
hello schemat identifieringsskript är obligatoriskt. Det här skriptet returnerar hello objekttyper, attribut och attributet begränsningar som hello synkroniseringstjänsten använder när du konfigurerar regler för flödet av attribut. hello schemat identifieringsskriptet körs under anslutning skapas och fylls hello connector schemat. Det används också av hello uppdatera Schema-åtgärden i hello Synchronization Service Manager.

hello schemat identifieringsskript tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection] [ keyk] [sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |

hello skriptet måste returnera en enda [schemat] [ schema] objekt toohello pipeline. hello schemaobjekt består av [SchemaType] [ schemaT] objekt som representerar objekttyper (till exempel: användare och grupper). hello SchemaType objekt innehåller en samling [SchemaAttribute] [ schemaA] objekt som representerar hello attribut (till exempel: Förnamn, efternamn och postadress) av typen hello.

**Ytterligare parametrar**  
Dessutom toohello standard konfigurationsinställningar, du kan definiera ytterligare anpassade konfigurationsinställningar som är specifika toohello instans av hello Connector. Dessa parametrar kan anges på hello koppling, partition, eller kör steg nivåer och nås från hello relevanta Windows PowerShell-skript. Anpassade inställningar som kan lagras i hello synkroniseringstjänsten databasen i oformaterad text eller kan vara krypterat. hello synkroniseringstjänsten automatiskt krypterar och dekrypterar säker konfigurationsinställningar vid behov.

toospecify anpassade konfigurationsinställningar, separat hello namn för varje parameter med kommatecken (,).

tooaccess anpassade konfigurationsinställningar från ett skript, måste du suffix hello namn med ett understreck ( \_ ) och hello omfattning hello-parametern (Global, Partition eller RunStep). Till exempel tooaccess hello globala FileName-parameter, använda det här kodstycket:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Funktioner
hello funktioner fliken hello Management Agent Designer definierar hello beteende och funktionerna i hello-anslutningen. hello val som görs på den här fliken kan inte ändras när hello-koppling har skapats. Den här tabellen visar hello kapacitetsinställningarna.

![Funktioner](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Funktion | Beskrivning |
| --- | --- |
| [Huvudnamnet format][dnstyle] |Anger om hello connector stöder unika namn och därför vad format. |
| [Exportera typ][exportT] |Anger hello typ av objekt som presenteras toohello exportskriptet. <li>AttributeReplace – innehåller hello fullständig uppsättning värden för ett flervärdesattribut när hello attribut ändras.</li><li>AttributeUpdate – innehåller endast hello går tooa flervärdesattribut när hello attribut ändras.</li><li>MultivaluedReferenceAttributeUpdate - innehåller en fullständig uppsättning värden för icke-referens med flera värden attribut och endast går för flera värden referensattribut.</li><li>ObjectReplace – innehåller alla attribut för ett objekt när någon attributändringar</li> |
| [Databasnormalisering][DataNorm] |Instruerar hello synkroniseringstjänsten toonormalize fästpunkt attribut innan de tillhandahålls tooscripts. |
| [Bekräftelse av objekt][oconf] |Konfigurerar hello väntande import beteende i hello synkroniseringstjänsten. <li>Normal – standardbeteendet som förväntar sig alla exporterade ändringar toobe bekräftas via import</li><li>NoDeleteConfirmation – finns när ett objekt tas bort, det inga väntande import genereras.</li><li>NoAddAndDeleteConfirmation – finns när ett objekt skapas eller tas bort, det inga väntande import genereras.</li> |
| Använda DN som ankare |Om hello unika namn format anges tooLDAP, är också hello fästpunktsattributet för hello anslutningsplatsen hello unika namn. |
| Samtidiga åtgärder av flera kopplingar |När alternativet är markerat kan flera Windows PowerShell-kopplingar köras samtidigt. |
| Partitioner |När alternativet är markerat hello connector har stöd för flera partitioner och identifiering av partition. |
| Hierarki |När alternativet är markerat stöder hello anslutningen en hierarkisk struktur för LDAP-format. |
| Aktivera Import |När alternativet är markerat importerar hello kopplingen data via skript för import. |
| Aktivera Deltaimport |När alternativet är markerat kan begära hello connector går från hello Importera skript. |
| Aktivera Export |När alternativet är markerat exporterar hello connector data via export skript. |
| Aktivera fullständig Export |När alternativet är markerat exportera hello skript stöd exportera hello hela anslutningsplatsen. Det här alternativet Aktivera exportera måste också vara markerad toouse. |
| Ingen referensvärden i första Export skickas |När alternativet är markerat exporteras referensattribut i ett andra steg i exporten. |
| Aktivera objektet Byt namn |När alternativet är markerat kan du ändra unika namn. |
| Ta bort-lägga till som ersätter |När alternativet är markerat, delete-Lägg till åtgärder som exporteras som en enda ersättning. |
| Aktivera Lösenordsåtgärder |När alternativet är markerat stöds skript för synkronisering av lösenord. |
| Aktivera Export av lösenord i första steget |När alternativet är markerat exporteras lösenord under etablering när hello objekt skapas. |

### <a name="global-parameters"></a>Globala parametrar
hello globala parametrar-fliken i hello Management Agent Designer kan du tooconfigure hello Windows PowerShell-skript som körs av hello-anslutningen. Du kan också konfigurera globala värden för anpassade konfigurationsinställningar som definierats hello anslutning på fliken.

**Identifiering av partition**  
En partition är en separat namnområde inom ett delat schema. Till exempel är alla domäner i Active Directory en partition inom en skog. En partition är hello logisk gruppering för import och export åtgärder. Importera och exportera ha partition som en kontext och alla åtgärder som sker i den här kontexten. Partitioner är avsedda toorepresent en hierarki i LDAP. hello unika namnet för en partition används i import tooverify att returneras alla objekt som är inom en partition hello omfång. hello unika partitionsnamnet används också under etableringen från hello metaversum toohello connector utrymme toodetermine hello partition ett objekt ska associeras med under exporten.

hello partition identifieringsskript tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |

hello skriptet måste returnera en antingen en enskild [Partition] [ part] objekt eller en lista [T] för Partition objekt toohello pipeline.

**Hierarkiidentifiering**  
hello hierarkin identifieringsskript används bara när hello kapaciteten för unika namn format är LDAP. hello skript är används tooallow du toobrowse och välj en uppsättning behållare som anses i eller utanför importera och exportera åtgärder. hello skriptet ska endast ange en lista över noder som är direkt underordnade hello rot angivna toohello nodskript.

hello hierarkin identifieringsskript tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| ParentNode |[HierarchyNode][hn] |hello rotnoden hello hierarkin under vilka hello skript ska returnera direkt underordnade objekt. |

hello skriptet måste returnera en antingen ett enda underordnat HierarchyNode objekt eller en lista [T] för underordnade HierarchyNode objekt toohello pipeline.

#### <a name="import"></a>Importera
Kopplingar som har stöd för importåtgärder måste implementera tre skript.

**Påbörja Import**  
hello börja importera skript körs hello början av ett kör importsteg. Under det här steget kan du upprätta en anslutning toohello källsystemet och göra förberedande steg innan du importerar data från hello anslutna system.

hello börja importera skriptet tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informerar hello skript om hello typ av import kör (delta eller fullständig) partition, hierarkin, vattenstämpel och förväntade storlek. |
| Typer |[Schemat][schema] |Schemat för hello anslutningsplatsen som importeras. |

hello skriptet måste returnera en enda [OpenImportConnectionResults] [ oicres] objekt toohello pipeline, till exempel:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Importera Data**  
hello importera Dataskript anropas av hello anslutningen förrän hello skript anger att det finns inga fler data tooimport. hello Windows PowerShell connector har en sidstorlek 9 999 objekt. Om skriptet returnerar mer än 9 999 objekt som ska importeras, du måste ha stöd för sidindelning. hello koppling visar kallas för en anpassad egenskap som du kan använda tooa store en vattenstämpel så att varje gång hello importera Dataskript, skriptet fortsätter med att importera objekt där den avbröts.

hello importera Dataskript tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Innehåller hello vattenstämpel (CustomData) som kan användas vid växlingsbara import och delta-import. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informerar hello skript om hello typ av import kör (delta eller fullständig) partition, hierarkin, vattenstämpel och förväntade storlek. |
| Typer |[Schemat][schema] |Schemat för hello anslutningsplatsen som importeras. |

hello importera Dataskript måste skriva en lista [[CSEntryChange][csec]] objektet toohello pipeline. Den här samlingen består av CSEntryChange attribut som representerar alla objekt som importeras. Den här samlingen bör ha en fullständig uppsättning CSEntryChange objekt som har alla attribut för varje objekt under en fullständig Import-körning. Hej CSEntryChange objekt under en Deltaimport ska antingen innehålla hello attributet nivån går för varje objekt tooimport eller en fullständig representation av hello-objekt som har ändrats (Ersättningsläge).

**End-Import**  
Vid hello slutsats om hello importera kör, körs hello slutet Importera skript. Det här skriptet ska utföra någon rensning nödvändiga åtgärder (till exempel Stäng anslutningar toosystems och svara toofailures).

hello slutet Importera skript tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informerar hello skript om hello typ av import kör (delta eller fullständig) partition, hierarkin, vattenstämpel och förväntade storlek. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informerar hello skript om hello orsak hello importera avslutades. |

hello skriptet måste returnera en enda [CloseImportConnectionResults] [ cicres] objekt toohello pipeline, till exempel:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exportera
Identiska toohello import-arkitektur hello Connector och kopplingar som har stöd för Export måste implementera tre skript.

**Börja exportera**  
hello börjar exportera skript körs hello början av ett export kör steg. Det här steget kan du upprätta en anslutning toohello källsystemet och genomföra alla förberedande steg innan du exporterar data toohello anslutna system.

hello börjar exportera skriptet tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informerar hello skript om hello typ av export kör (delta eller fullständig) partition, hierarkin och förväntade storlek. |
| Typer |[Schemat][schema] |Schemat för hello anslutningsplatsen som exporteras. |

hello skript ska inte returnera några utdata toohello pipeline.

**Exportera Data**  
hello synkroniseringstjänsten anropar hello exportera Data skript så många gånger som är nödvändiga tooprocess alla väntande exporter. Om hello anslutningsplatsen har flera väntande exporter än hello kopplingens sidstorlek, hello hello exportera Dataskript kan anropas flera gånger och eventuellt flera gånger för samma objekt.

Hej exportskriptet data tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| CSEntries |IList[CSEntryChange][csec] |Lista över alla hello anslutningsplatsen objekt med väntande exporter toobe som bearbetats under det här steget. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informerar hello skript om hello typ av export kör (delta eller fullständig) partition, hierarkin och förväntade storlek. |
| Typer |[Schemat][schema] |Schemat för hello anslutningsplatsen som exporteras. |

hello data exportskriptet måste returnera en [PutExportEntriesResults] [ peeres] objekt toohello pipeline. Det här objektet behöver inte tooinclude resultatet information för varje exporterad koppling om ett fel eller en ändring toohello fästpunktsattributet inträffar. Till exempel tooreturn en PutExportEntriesResults objektet toohello pipeline:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**End-Export**  
Kör på hello slutsats hello exporten, hello slutet exporterar skriptet toorun. Det här skriptet ska utföra någon rensning nödvändiga åtgärder (till exempel Stäng anslutningar toosystems och svara toofailures).

hello slutet exportskriptet tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informerar hello skript om hello typ av export kör (delta eller fullständig) partition, hierarkin och förväntade storlek. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Informerar hello skript om hello orsak hello export avslutades. |

hello skript ska inte returnera några utdata toohello pipeline.

#### <a name="password-synchronization"></a>Lösenordssynkronisering
Windows PowerShell-kopplingar kan användas som mål för ändringar/lösenordsåterställning.

hello lösenord skriptet tar emot hello följande parametrar från hello-anslutningen:

| Namn | Datatyp | Beskrivning |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][sträng, [ConfigParameter][cp]] |Tabell över konfigurationsparametrar för hello Connector. |
| Autentiseringsuppgift |[PSCredential][pscred] |Innehåller alla autentiseringsuppgifter som angetts av administratören hello hello anslutning på fliken. |
| Partition |[Partition][part] |Katalogpartition som hello CSEntry finns i. |
| CSEntry |[CSEntry][cse] |Kopplingen utrymme post för hello-objekt som är tog emot en lösenordsändring eller återställning. |
| Åtgärdstyp |Sträng |Anger om hello åtgärden är en återställning (**SetPassword**) eller en ändring (**ChangePassword**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Flaggor som anger hello avsedda beteende för återställning av lösenord. Den här parametern är endast tillgänglig om OperationType är **SetPassword**. |
| Gammalt lösenord |Sträng |Fylls i med hello objektets gamla lösenord för ändring av lösenord. Den här parametern är endast tillgänglig om OperationType är **ChangePassword**. |
| Nytt lösenord |Sträng |Fylls i med hello objektet nytt lösenord som ska ange för hello skript. |

Hej lösenord skript är inte förväntade tooreturn alla resultat toohello Windows PowerShell-pipeline. Om ett fel uppstår i hello lösenord skript, utlösa hello skript en av följande undantag tooinform hello synkroniseringstjänsten om hello problemet hello:

* [PasswordPolicyViolationException] [ pwdex1] – genereras om hello lösenordet inte uppfyller hello lösenordsprincip i hello anslutna system.
* [PasswordIllFormedException] [ pwdex2] – genereras om hello lösenord inte är godkänd för hello anslutna system.
* [PasswordExtension] [ pwdex3] – utlöstes andra fel i hello lösenord skript.

## <a name="sample-connectors"></a>Exempel kopplingar
En fullständig översikt över hello tillgängliga exempel kopplingar, se [Connector för Windows PowerShell Connector provtagning][samp].

## <a name="other-notes"></a>Anmärkningar
### <a name="additional-configuration-for-impersonation"></a>Ytterligare konfiguration för personifiering
Bevilja hello-användare som är personifierad hello följande behörigheter på hello synkroniseringstjänst för servern:

Läsbehörighet toohello följande registernycklar:

* HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
* HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Environment

toodetermine hello säkerhetsidentifierare (SID) för hello synkroniseringstjänsten tjänstkonto som kör hello följande PowerShell-kommandon:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Läsbehörighet toohello följande systemmappar:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Ersätt hello namnet hello Windows PowerShell Connector för platshållaren hello {ConnectorName}.

## <a name="troubleshooting"></a>Felsökning
* Information om hur tooenable loggning tootroubleshoot hello koppling finns hello [hur tooEnable ETW-spårning för kopplingar](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
