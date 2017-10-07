---
title: "aaaFix SQL-anslutningsfel tillfälligt fel | Microsoft Docs"
description: "Lär dig hur tootroubleshoot, diagnostisera och förhindra att ett fel vid anslutning till SQL eller ett tillfälligt fel i Azure SQL Database. "
keywords: "SQL-anslutning, anslutningssträngen, problem med nätverksanslutningen, tillfälligt fel, anslutningsfel"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: d225e610b9e88170ab53ca16d615bd07220603cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Felsöka, diagnostisera och förhindra SQL-anslutningsfel och tillfälliga fel för SQL Database
Den här artikeln beskriver hur tooprevent, Felsök, diagnostisera och minska anslutningsfel och tillfälliga fel som ditt klientprogram påträffar när det interagerar med Azure SQL Database. Lär dig hur tooconfigure försök logik, skapa hello anslutningssträngen och justera andra inställningar.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tillfälliga fel tillfälligt
Ett tillfälligt fel - också tillfälliga fel – har en underliggande orsaken som kommer snart att lösas automatiskt. En tillfällig tillfälliga fel beror på när hello Azure system snabbt skiftar maskinvara resurser toobetter belastningsutjämna olika arbetsbelastningar. De flesta av dessa händelser för omkonfiguration Slutför ofta i mindre än 60 sekunder. Under det angivna tidsintervallet omkonfiguration kanske anslutningen utfärdar tooAzure SQL-databas. Program som ansluter tooAzure SQL-databas ska vara inbyggda tooexpect felen tillfälligt hantera dem genom att implementera försök logiken i koden i stället för att visa dem toousers som programfel.

Om klientprogrammet använder ADO.NET, programmet meddelade om hello tillfälligt fel av hello throw av en **SqlException**. Hej **nummer** egenskapen kan jämföras mot hello lista över tillfälliga fel hello övre delen av avsnittet hello: [SQL-felkoder för SQL Database-klientprogram](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Anslutningen jämfört med kommandot
Du kommer försök hello SQL-anslutning eller upprätta igen, beroende på hello följande:

* **Ett tillfälligt fel inträffar under ett anslutningsförsök**: hello anslutningen ska göras efter att förskjuta för några sekunder.
* **Ett tillfälligt fel inträffar under en SQL-kommandot query**: hello kommandot bör inte göras omedelbart. I stället efter en stund hello anslutningen bör nyligen upprättas. Sedan kan hello-kommandot göras.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Logik för omprövning av tillfälliga fel
Klientprogram som kan uppstår ett tillfälligt fel är mer robusta när de innehåller logik för omprövning.

När ditt program kommunicerar med Azure SQL Database via en 3 part mellanprogram, fråga med hello leverantör om hello mellanprogram innehåller logik för omprövning av tillfälliga fel.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Principer för nya försök
* Ett försök tooopen en anslutning ska göras om hello felet är tillfälligt.
* En SQL SELECT-instruktion som misslyckas med ett tillfälligt fel bör inte göras direkt.
  
  * I stället skapa en ny anslutning och försök sedan hello väljer.
* När en UPDATE SQL-instruktion misslyckas med ett tillfälligt fel bör en ny anslutning upprättas innan hello UPPDATERINGEN igen.
  
  * logik för omprövning av hello måste se till att hello hela Databastransaktionen har slutförts eller hello hela transaktionen återställs.

#### <a name="other-considerations-for-retry"></a>Andra överväganden för nya försök
* En kommandofil som startas automatiskt efter arbetstid och som kommer att slutföras innan morgon har råd toovery tålamod med lång tidsintervall mellan dess nya försök.
* Ett gränssnitt användarprogram bör hänsyn till hello mänsklig tendens toogive upp efter för lång väntan.
  
  * Dock får hello lösning inte vara tooretry några sekunders eftersom principen kan översvämma hello system med förfrågningar.

#### <a name="interval-increase-between-retries"></a>Öka intervall mellan försök
Vi rekommenderar att du fördröjning för 5 sekunder innan din första försök. Försöker igen efter en kortare än 5 sekunder fördröjning risker överväldigande hello-Molntjänsten. För varje efterföljande försök ska hello fördröjning växa exponentiellt, in tooa högst 60 sekunder.

En beskrivning av hello *blockerande period* för klienter som använder ADO.NET finns i [SQL Server anslutning poolning (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Du kan också tooset ett maximalt antal försök innan programmet hello själv avslutas.

#### <a name="code-samples-with-retry-logic"></a>Kodexempel med logik
Kodexempel med logik för omprövning, på en mängd olika programmeringsspråk, finns på:

* [Anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testa din logik
tootest logik för omprövning måste du simulera eller orsaka fel än kan korrigeras medan programmet körs.

##### <a name="test-by-disconnecting-from-hello-network"></a>Testa genom att koppla från hello nätverk
Ett sätt som du kan testa din logik är toodisconnect klientdatorn från hello nätverk när hello programmet körs. hello-felet är:

* **SqlException.Number** = 11001
* Meddelandet: ”värden är inte känd”

Som en del av hello första återförsöket försök, kan programmet korrigera hello felstavat och försök sedan tooconnect.

Det här praktiska toomake, du koppla från datorn från nätverket hello innan du startar programmet. Sedan identifierar programmet en parameter för körning som gör att hello program:

1. Lägg till 11001 tooits lista över fel tooconsider tillfälligt som tillfällig.
2. Försök den första anslutningen som vanligt.
3. Ta bort 11001 hello listan när hello fel har inträffat.
4. Visa ett meddelande om hello tooplug hello dator i hello nätverk.
   * Pausa körning med hjälp av antingen hello **Console.ReadLine** metod eller en dialogruta med en OK-knapp. hello användaren trycker på RETUR hello när hello datorn ansluten till nätverket hello.
5. Försök igen tooconnect, förväntas lyckades.

##### <a name="test-by-misspelling-hello-database-name-when-connecting"></a>Testa genom felstavat hello databasnamnet vid anslutning
Programmet kan ändamålsenligt fel hello användarnamn innan hello första anslutningsförsöket. hello-felet är:

* **SqlException.Number** = 18456
* Meddelandet: ”inloggning misslyckades för användaren 'WRONG_MyUserName'”.

Som en del av hello första återförsöket försök, kan programmet korrigera hello felstavat och försök sedan tooconnect.

Det här praktiska toomake, programmet kände igen en parameter för körning som gör att hello program:

1. Lägg till 18456 tooits lista över fel tooconsider tillfälligt som tillfällig.
2. Lägg till 'WRONG_' toohello användarnamn ändamålsenligt.
3. Ta bort 18456 hello listan när hello fel har inträffat.
4. Ta bort 'WRONG_' från hello användarnamn.
5. Försök igen tooconnect, förväntas lyckades.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection parametrar för anslutningen försök igen
Om klientprogrammet ansluter tootooAzure SQL-databas med hjälp av .NET Framework-klass för hello **System.Data.SqlClient.SqlConnection**, bör du använda .NET 4.6.1 eller senare (eller .NET Core) så att du kan utnyttja funktionen anslutning försök igen. Information om hello funktionen [här](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points toodn632678.aspx, which links tooa downloadable .docx related tooSqlClient and SQL Server 2014.
-->


När du skapar hello [anslutningssträngen](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) för din **SqlConnection** objekt, bör du samordna hello värden mellan hello följande parametrar:

* ConnectRetryCount &nbsp; &nbsp; *(standardvärdet är 1. Intervallet är 0 och 255.)*
* ConnectRetryInterval &nbsp; &nbsp; *(standard är 1 sekund. Intervall är 1 till 60).*
* Timeout för anslutningen &nbsp; &nbsp; *(standardvärdet är 15 sekunder. Intervallet är 0 och 2147483647)*

Mer specifikt bör valda värdena Se hello efter likhet SANT:

* Timeout för anslutningen = ConnectRetryCount * ConnectionRetryInterval

Exempel: om hello antal = 3 och intervall = 10 sekunder timeout för bara 29 sekunder inte skulle helt ger hello system tillräckligt med tid för dess 3 och slutgiltig försök vid anslutning: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Anslutningen jämfört med kommandot
Hej **ConnectRetryCount** och **ConnectRetryInterval** -parametrar kan din **SqlConnection** objekt försök hello anslutningsåtgärden utan uppmanar eller stör din program, till exempel returnera kontrollen tooyour program. hello återförsök kan inträffa i följande situationer hello:

* mySqlConnection.Open metodanrop
* mySqlConnection.Execute metodanrop

Det finns en liten finess. Om ett tillfälligt fel uppstår när din *frågan* körs din **SqlConnection** objekt har inte försök hello åtgärden med anslutning och den verkligen inte köra frågan igen. Dock **SqlConnection** snabbt kontrollerar hello anslutningen innan du skickar frågan för körning. Om hello Snabbkontrollen upptäcker ett anslutningsproblem, **SqlConnection** hello försök ansluta igen. Om hello försök lyckas, skickas frågan för du för körning.

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount kombineras med logik för omprövning av programmet?
Anta att programmet har robust anpassade logik. Det kan försök hello anslutningsåtgärden 4 gånger. Om du lägger till **ConnectRetryInterval** och **ConnectRetryCount** = 3 tooyour anslutningssträngen, ökar hello försök antal too4 * 3 = 12 återförsök. Du kan inte avser sådana ett stort antal återförsök.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-tooazure-sql-database"></a>Anslutningar tooAzure SQL-databas
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Anslutningen: Anslutningssträng
hello anslutningssträngen som är nödvändiga för att ansluta tooAzure SQL-databas är skiljer sig från hello sträng för att ansluta tooMicrosoft SQL Server. Du kan kopiera hello anslutningssträngen för databasen från hello [Azure-portalen](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Anslutning: IP-adress för
Du måste konfigurera hello SQL Database server tooaccept kommunikation från hello IP-adress för hello-dator som är värd för dina klientprogram. Du kan göra detta genom att redigera hello brandväggsinställningar via hello [Azure-portalen](https://portal.azure.com/).

Om du glömmer tooconfigure hello IP-adress kan misslyckas programmet med en praktisk felmeddelande hello IP-adress.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Mer information finns: [så här: konfigurera brandväggsinställningar på SQL-databas](sql-database-configure-firewall-settings.md)

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Anslutning: portar
Vanligtvis behöver du bara tooensure att port 1433 är öppen för utgående kommunikation på hello-dator som värd-klientprogrammet.

Till exempel när klientprogrammet finns på en dator med Windows kan hello Windows-brandväggen på hello värd du tooopen port 1433:

1. Öppna hello Kontrollpanelen
2. &gt;Alla objekt på Kontrollpanelen
3. &gt;Windows-brandväggen
4. &gt;Avancerade inställningar
5. &gt;Utgående regler
6. &gt;Åtgärder
7. &gt;Ny regel

Om klientprogrammet finns på en Azure-dator (VM), bör du läsa:<br/>[Portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Mer bakgrundsinformation om cofiguration portar och IP-adress, se: [Azure SQL Database-brandvägg](sql-database-firewall-configure.md)

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Anslutning: ADO.NET 4.6.1
Om programmet använder ADO.NET-klasser som **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL-databas, rekommenderar vi att du använder .NET Framework version 4.6.1 eller senare.

ADO.NET 4.6.1:

* För Azure SQL-databas finns förbättrad tillförlitlighet när du öppnar en anslutning med hjälp av hello **SqlConnection.Open** metod. Hej **öppna** metoden omfattar nu bästa prestanda försök autentiseringsmekanismer i svaret tootransient fel, för vissa fel inom tidsgränsen för hello anslutning.
* Stöder anslutningspooler. Detta inkluderar en effektiv kontroll hello anslutning objekt som den ger programmet fungerar.

Vi rekommenderar att programmet tillfälligt stänger hello-anslutning när du använder den inte omedelbart när du använder ett anslutningsobjekt från en anslutningspool. Öppna en anslutning inte är dyr hello sättet att skapa en ny anslutning.

Om du använder ADO.NET 4.0 eller tidigare, rekommenderar vi att du uppgraderar toohello senaste ADO.NET.

* Från och med November 2015, kan du [hämta ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostik
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostik: Testa om verktyg kan ansluta
Om programmet misslyckas tooconnect tooAzure SQL-databas, kan en diagnostik tootry tooconnect med ett program. Helst hello verktyget skulle ansluta med hjälp av hello samma bibliotek som används i programmet.

Du kan försöka verktygen på alla Windows-datorer:

* SQL Server Management Studio (ssms.exe) som ansluter med hjälp av ADO.NET.
* SQLCMD.exe som ansluter med hjälp av [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).

När du är ansluten, testa om ett kort SQL SELECT-frågan fungerar.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-hello-open-ports"></a>Diagnostik: Kontrollera hello öppna portar
Anta att du misstänker att anslutningsförsök misslyckas på grund av problem med tooport. Du kan köra ett verktyg som rapporterar hello portkonfigurationer på datorn.

Följande verktyg kan vara användbart på Linux hello:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * (Ändra hello exempel värdet toobe IP-adress.)

På Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) verktyg kan vara till hjälp. Här är ett exempel körning som efterfrågas hello port situationen på en Azure SQL Database-server och som kördes på en bärbar dator:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting tooresolve name tooIP address...
Name resolved too23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostik: Logga din fel
Ett tillfälligt problem diagnostiserats ibland bäst av identifiering av ett allmänt mönster över dagar eller veckor.

Klienten kan hjälpa en diagnos genom att logga alla fel som påträffas. Du kanske kan toocorrelate hello loggposter med Feldata som Azure SQL Database loggar själva internt.

Enterprise-biblioteket 6 (EntLib60) ger hanterad klasser tooassist med loggning:

* [5 - som enkelt som som omfattas av av en logg: med hello loggning programmet Block](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostik: Granska systemloggar för fel
Här följer några Transact-SQL SELECT-satser som frågeloggar av fel och annan information.

| Frågan för logg | Beskrivning |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |Hej [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) vyn innehåller information om hur enskilda händelser, inklusive några som kan orsaka tillfälliga fel eller anslutningsfel.<br/><br/>Helst korrelera hello **starttid** eller **end_time** värden med information om när dina klientprogram stötte på problem.<br/><br/>**Tips:** måste du ansluta toohello **master** databasen toorun detta. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |Hej [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) visa erbjuder aggregerade antal händelsetyper för ytterligare diagnostik.<br/><br/>**Tips:** måste du ansluta toohello **master** databasen toorun detta. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-hello-sql-database-log"></a>Diagnostik: Sök efter problem händelser i loggen för hello SQL-databas
Du kan söka efter poster om problemet händelser i loggen för hello i Azure SQL Database. Försök följande Transact-SQL SELECT-uttryck i hello hello **master** databasen:

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Några returnerade rader från sys.fn_xe_telemetry_blob_target_read_file
Nästa är en returnerad rad kan se ut. hello null-värden som visas är ofta inte null i andra rader.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Enterprise Library 6
Enterprise-biblioteket 6 (EntLib60) är ett ramverk för .NET-klasser som hjälper dig att implementera robust klienter för molntjänster, varav en är hello Azure SQL Database-tjänsten. Du kan hitta avsnitt dedikerade tooeach område där EntLib60 kan hjälpa genom att besöka första:

* [Enterprise Library 6 – April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

Logik för att hantera tillfälliga fel är en plats där EntLib60 kan hjälpa.

* [4 - perseverance, hemligheten för alla framgångar: med hello tillfälliga fel hanterar programmet Block](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> hello källkoden för EntLib60 är tillgängligt för allmänheten [hämta](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft har inga planer toomake ytterligare uppdateringar och underhåll uppdaterar tooEntLib.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 klasser för tillfälliga fel och försök igen
hello följande EntLib60 klasser är mycket användbart för logik. Alla dessa, eller ytterligare under hello namnområde **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*I hello namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

* **RetryPolicy** klass
  
  * **ExecuteAction** metod
* **ExponentialBackoff** klass
* **SqlDatabaseTransientErrorDetectionStrategy** klass
* **ReliableSqlConnection** klass
  
  * **ExecuteCommand** metod

I hello namnområdet **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** klass
* **NeverTransientErrorDetectionStrategy** klass

Här är länkar tooinformation om EntLib60:

* Ledigt [Book hämta: Developer's Guide tooMicrosoft Enterprise Library 2 Edition](http://www.microsoft.com/download/details.aspx?id=41145)
* Bästa praxis: [försök allmänna riktlinjer](../best-practices-retry-general.md) har en utmärkt detaljerad beskrivning av logik.
* Hämta NuGet [Enterprise Library - hantering av tillfälliga fel program block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-hello-logging-block"></a>EntLib60: hello loggning block
* hello loggning block är en mycket flexibelt och konfigurerbara lösning som hjälper dig att:
  
  * Skapa och lagra meddelanden i en mängd olika platser.
  * Kategorisera och filtrera meddelanden.
  * Samla in relevant information som är användbart för felsökning och spårning och för granskning och allmänna loggning krav.
* hello loggning block avlägsnar hello loggning funktioner från hello loggmålet så att hello programkoden är konsekvent, oavsett hello plats och typ för hello mål loggning store.

Mer information finns: [5 – som enkelt som som omfattas av av en logg: med hello loggning programmet Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>Källkoden för EntLib60 IsTransient metod
Nästa från hello **SqlDatabaseTransientErrorDetectionStrategy** klassen är hello C#-källkod för hello **IsTransient** metod. hello källkoden klargör vilka fel ansågs toobe tillfälligt och worthy på försök igen, från och med April 2013.

Ett stort antal **//comment** rader har tagits bort från den här kopian tooemphasize läsbarhet.

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in hello exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // hello service is currently busy. Retry hello request after 10 seconds.
            // Code: (reason code toobe decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode hello reason code from hello error message to
            // determine hello grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach hello decoded values as additional attributes to
            // hello original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // hello instance of SQL Server you attempted tooconnect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Nästa steg
* Felsöka andra vanliga problem med Azure SQL Database anslutningen finns [felsöka anslutningen utfärdar tooAzure SQL-databas](sql-database-troubleshoot-common-connection-issues.md).
* [SQL Server anslutningspoolen (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [*Du försöker* är ett Apache 2.0 licensierad allmänna du försöker biblioteket som skrivits i **Python**, toosimplify hello uppgiften att lägga till försök beteende toojust om något.](https://pypi.python.org/pypi/retrying)

