---
title: aaaGeneric SQL Connector | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfigure Microsoft allmän SQL-anslutningen."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2eab8f0894e83ab4738b9f2deb05b03cdc9a9d43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-technical-reference"></a>Teknisk referens för allmän SQL Connector
Den här artikeln beskriver hello allmän SQL-anslutningen. hello artikeln gäller toohello följande produkter:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Måste använda snabbkorrigering 4.1.3671.0 eller senare [KB3092178](https://support.microsoft.com/kb/3092178).

För MIM2016 och FIM2010R2 hello Connector är tillgänglig för hämtning från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

toosee den här kopplingen i åtgärden, se hello [allmänna SQL Connector stegvisa](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) artikel.

## <a name="overview-of-hello-generic-sql-connector"></a>Översikt över hello allmän SQL-koppling
hello allmän SQL-anslutningen kan du toointegrate hello synkroniseringstjänsten med ett databassystem som erbjuder ODBC-anslutning.  

Ur en övergripande som hello följande funktioner stöds av hello aktuella versionen av hello-anslutningen:

| Funktion | Support |
| --- | --- |
| Anslutna datakällan |hello Connector stöds med alla 64-bitars ODBC-drivrutiner. Den har testats med hello följande: <li>Microsoft SQL Server och SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 11 & 10 g</li><li>MySQL 5.x</li> |
| Scenarier |<li>Livscykelhantering för objektet</li><li>Lösenordshantering</li> |
| Åtgärder |<li>Fullständig Import och Deltaimport, Export</li><li>För Export: Lägga till, ta bort uppdateringen, och Ersätt</li><li>Ange lösenord, ändra lösenord</li> |
| Schemat |<li>Dynamisk identifiering av objekt och attribut</li> |

### <a name="prerequisites"></a>Krav
Innan du använder hello koppling, kontrollera att du har hello följande på hello synkroniseringsserver:

* 4.5.2 för Microsoft .NET Framework eller senare
* 64-bitars ODBC-drivrutiner för klienten

### <a name="permissions-in-connected-data-source"></a>Behörigheter i anslutna datakällan
toocreate eller utföra hello stöds aktiviteter i generiska SQL-anslutningen måste du ha:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Portar och protokoll
Hello-portar som krävs för hello ODBC-drivrutinen toowork dokumentationen hello databasleverantörens.

## <a name="create-a-new-connector"></a>Skapa en ny koppling
tooCreate en allmän SQL-koppling i **synkroniseringstjänsten** Välj **hanteringsagenten** och **skapa**. Välj hello **generiskt SQL (Microsoft)** koppling.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Anslutning
hello anslutningen använder ett ODBC DSN-fil för anslutningen. Skapa hello DSN en fil med hjälp av **ODBC-datakällor** hittades i hello start-menyn under **Administrationsverktyg**. I hello Administrationsverktyg, skapar en **fil-DSN** så att den kan ges toohello koppling.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

hello är anslutning hello först när du skapar en ny koppling för allmän SQL. Du måste först tooprovide hello följande information:

* DSN filsökväg
* Autentisering
  * Användarnamn
  * Lösenord

hello-databasen ska ha stöd för en av dessa autentiseringsmetoder:

* **Windows-autentisering**: hello autentisera databasen använder hello Windows-autentiseringsuppgifter tooverify hello användare. hello användarnamn/lösenord anges är används tooauthenticate med hello-databas. Det här kontot måste behörigheter toohello databas.
* **SQL-autentisering**: hello autentisera databasen använder hello användarnamn/lösenord definieras en hello anslutningen skärmen tooconnect toohello databas. Om du sparar hello namn/lösenord för användaren i hello DSN-fil har prioritet hello autentiseringsuppgifterna på anslutningen hello-skärmen.
* **Azure SQL Database authentication**: Mer information finns i [ansluta tooSQL databasen med hjälp av Azure Active Directory Authentication](../../sql-database/sql-database-aad-authentication.md).

**DN är fästpunkt**: Om du väljer det här alternativet används också hello DN som hello fästpunktsattributet. Det kan användas för en enkel implementering men har även hello följande begränsningar:

* Anslutningen stöder endast en objekttyp. Därför hello hänvisningar attribut kan bara referera till samma objekttyp.

**Exporttyp av: Objekt-Ersätt**: under exporten när endast vissa attribut har ändrats, hello hela objektet med alla attribut som exporteras och ersätter hello befintligt objekt.

### <a name="schema-1-detect-object-types"></a>Schemat 1 (identifiera objekttyper)
På den här sidan kommer tooconfigure hur hello Connector är pågående toofind hello olika objekttyper i hello-databasen.

Varje objekttyp visas som en partition och konfigurerat ytterligare på **konfigurera partitioner och hierarkier**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objekt av typen identifieringsmetod**: hello Connector stöder dessa identifieringsmetoder för typ av objekt.

* **Fast värde**: du anger hello lista över typer av objekt med en kommaavgränsad lista. Till exempel: `User,Group,Department`.  
  ![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
* **Tabellen/vyn/lagrad procedur**: Ange hello namnet på hello tabellen/vyn/lagrad procedur och hello kolumnnamn som visar hello lista över objekttyper. Om du använder en lagrad procedur kan också ange parametrar för den hello format **[Name]: [riktning]: [värde]**. Ange varje parameter på en separat rad (Använd Ctrl + Retur tooget en ny rad).  
  ![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
* **SQL-frågan**: det här alternativet kan du tooprovide en SQL-fråga som returnerar en kolumn med objekttyper, till exempel `SELECT [Column Name] FROM TABLENAME`. hello returnerade kolumnen måste vara av typen sträng (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schemat 2 (identifiera attributtyper)
På den här sidan ska tooconfigure hur hello attributnamn och typer ska toobe upptäcktes. hello konfigurationsalternativ visas för varje objekttyp som identifierats på hello föregående sida.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Attributet typen identifieringsmetod**: hello Connector stöder dessa identifieringsmetoder för attributet typ med de identifierade objekt i schemat 1.

* **Tabellen/vyn/lagrad procedur**: Ange hello namnet på hello tabellen/vyn/lagrad procedur som ska använda toofind hello attributnamn. Om du använder en lagrad procedur kan också ange parametrar för den hello format **[Name]: [riktning]: [värde]**. Ange varje parameter på en separat rad (Använd Ctrl + Retur tooget en ny rad). toodetect hello attributnamn i ett flervärdesattribut, ange en kommaavgränsad lista med tabeller eller vyer. Flervärdesattribut scenarier stöds inte när överordnade och underordnade tabellen har samma kolumnnamn.
* **SQL-frågan**: det här alternativet kan du tooprovide en SQL-fråga som returnerar en kolumn med attributnamn, till exempel `SELECT [Column Name] FROM TABLENAME`. hello returnerade kolumnen måste vara av typen sträng (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schemat 3 (definiera fästpunkt och DN)
Den här sidan kan du tooconfigure fästpunkten och DN-attribut för varje identifierad objekttyp. Du kan välja flera attribut toomake hello fästpunkt unikt.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

* Flera värden och booleskt attribut visas inte.
* Samma attribut kan inte använda för DN och ankaret, såvida inte **DN är fästpunkt** är markerat på sidan för hello-anslutning.
* Om **DN är fästpunkt** väljs hello anslutningen på sidan den här sidan kräver endast hello DN-attribut. Det här attributet används också som hello fästpunkt attribut.

  ![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schemat 4 (definiera attributtyp, referens och riktning)
Den här sidan kan du tooconfigure hello attributtyp, till exempel heltal, binary eller booleskt värde och riktning för varje attribut. Alla attribut från sidan **schema 2** visas inklusive attribut med flera värden.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

* **DataType**: används toomap hello typen toothose attributtyper känd av hello Synkroniseringsmotorn. hello standardvärdet är toouse hello samma typ som identifierats i hello SQL-schemat, men DateTime och referens är inte enkelt kan upptäckas. För de, behöver du toospecify **DateTime** eller **referens**.
* **Riktning**: du kan ange hello attributet riktning tooImport, exportera eller ImportExport. ImportExport är som standard.

![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Information:

* Om en attributtyp inte kan upptäckas av hello anslutningen använder hello strängdatatypen.
* **Kapslade tabeller** kan betraktas som en kolumn databastabeller. Oracle lagrar hello rader i en kapslad tabell i bokstavsordning. När du hämtar hello kapslad tabell till en variabel för PL/SQL har hello rader angetts i följd nedsänkt text som börjar på 1. Som ger åtkomst till matrisliknande tooindividual rader.
* **VARRYS** stöds inte i hello-koppling.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schemat 5 (definiera partition för referensattribut)
På den här sidan kan konfigurera du för alla referensattribut vilken partition (objekttyp) ett attribut refererar till.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Om du använder **DN är fästpunkt**, måste du använda samma objekttypen hello en refererar du från hello. Du kan inte referera till en annan objekttyp.

>[!NOTE]
Från och med hello mars 2017 uppdatering är nu ett alternativ för ”*” när det här alternativet är valt och sedan alla möjliga medlemstyper kommer att importeras.

![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/any-option.png)

>[!IMPORTANT]
 Från och med kan 2017 hello ”*” aka **något alternativ** har ändrats toosupport importera och exportera flödet. Om du vill toouse det här alternativet bör med flera värden tabellen/vyn har ett attribut som innehåller hello objekttyp.

![](./media/active-directory-aadconnectsync-connector-genericsql/any-02.png)

 </br> Om ”*” är markerad och sedan måste du även ange hello namnet på hello kolumn med hello objekttyp.</br> ![](./media/active-directory-aadconnectsync-connector-genericsql/any-03.png)

Efter importen visas något liknande toohello bilden nedan:

  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Globala parametrar
hello globala parametrar är används tooconfigure Deltaimport, datum/tid-format och lösenordsmetod.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)



hello generiska SQL Connector stöder följande metoder för Deltaimport hello:

* **Utlösaren**: se [genererar Delta vyer med hjälp av utlösare](https://technet.microsoft.com/library/cc708665.aspx).
* **Vattenstämpel**: en generisk metod som kan användas med en databas. hello vattenstämpel frågan är ifylld baserat på hello databasleverantör. En kolumn för vattenstämpeln måste finnas på varje tabell/vy som används. Den här kolumnen måste spåra INSERT och Update toohello tabeller som och dess beroende (med flera värden eller underordnad) tabeller. hello klockorna synkroniseringstjänsten och hello databasservern måste synkroniseras. Om inte, kan vissa poster i hello Deltaimport utelämnas.  
  Begränsning:
  * Vattenstämpel strategi har inte för ta bort stödobjekt.
* **Ögonblicksbild**: (fungerar endast med Microsoft SQL Server) [genererar Delta vyer med hjälp av ögonblicksbilder](https://technet.microsoft.com/library/cc720640.aspx)
* **Ändringsspårning**: (fungerar endast med Microsoft SQL Server) [om ändringsspårning](https://msdn.microsoft.com/library/bb933875.aspx)  
  Begränsningar:
  * Fästpunkten & DN-attribut måste vara en del av primärnyckeln för hello markerade objekt i hello tabell.
  * SQL-fråga stöds inte under Import och Export med ändringsspårning.

**Ytterligare parametrar**: Ange hello databasen serverns tidszon som anger där databasservern finns. Det här värdet används toosupport hello olika format för datum och tid attribut.

hello Connector lagrar alltid datum och tid i UTC-format. toobe kan toocorrectly konvertera hello datum och tider, hello tidszon hello-databasservern och hello-format som används måste anges. hello format anges i .net-format.

Varje gång Datumattribut måste tillhandahållas toohello Connector i UTC-tidsformat för under exporten.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Lösenord Configuration**: hello connector ger funktioner för synkronisering av lösenord och stöder och ändra lösenord.

hello Connector erbjuder två metoder toosupport Lösenordssynkronisering:

* **Lagrade proceduren**: den här metoden kräver två lagrade procedurer toosupport Set & Ändra lösenord. Ange alla parametrar för Lägg till och ändra hello lösenord igen i **ange lösenord SP** och **ändra lösenord SP** parametrar som respektive per exemplet nedan.
  ![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
* **Lösenord tillägget**: den här metoden kräver att lösenord tilläggs-DLL (du behöver tooprovide hello DLL-namn som implementerar hello [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) interface). Lösenordet tilläggsammanfattningen måste placeras i tilläggsmappen så att hello connector kan läsa in hello DLL vid körning.
  ![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Du har också tooenable hello lösenordshantering på hello **konfigurera tillägget** sidan.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier
Hello partitioner och hierarkier på sidan Välj alla typer av objekt. Varje objekt är en egen partition.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Du kan också åsidosätta hello värden som definieras på hello **anslutning** eller **globala parametrar** sidan.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfigurera ankare
Den här sidan är skrivskyddat eftersom hello fästpunkt har redan definierats. hello valda fästpunktsattributet läggs alltid med hello objektet typen tooensure förblir det unika över objekttyper.

![ankare](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurera kör steg-Parameter
Dessa steg konfigureras på hello körningsprofiler på hello Connector. De här konfigurationerna hello verkligt arbete för att importera och exportera data.

### <a name="full-and-delta-import"></a>Fullständig och Deltaimport
Generisk SQL Connector-stöd fullständig och Deltaimport på följande sätt:

* Tabell
* Visa
* Lagrad procedur
* SQL-fråga

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabellen/vyn**  
tooimport med flera värden attribut för ett objekt har tooprovide hello CSV-tabell/visningsnamn i **namn multivärdes tabellvyer** och respektive kopplingsvillkor i hello **kopplingsvillkor**med hello den överordnade tabellen.

Exempel: Du tooimport hello medarbetare objektet och alla dess med flera värden attribut. Det finns två tabeller, namngivna medarbetare (huvudtabell) och avdelning (med flera värden).
Hej du följande:

* Typen **medarbetare** i **SP-tabell/vy**.
* Avdelning i **namn multivärdes tabellvyer**.
* Skriv hello kopplingsvillkor mellan anställda och avdelning i **kopplingsvillkor**, till exempel `Employee.DEPTID=Department.DepartmentID`.
  ![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Lagrade procedurer**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

* Om du har mycket data bör tooimplement sidbrytning med din lagrade procedurer.
* För den lagrade proceduren toosupport sidbrytningen måste tooprovide Index för Start och slut Index. Se: [effektivt växling till stora mängder Data](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexoch @EndIndex ersätts under körning med respektive värdet för sidstorlek konfigurerats på **konfigurera steg** sidan. Till exempel, när hello koppling hämtar första sidan och hello sidstorleken anges 500, i en sådan situation @StartIndex skulle värdet vara 1 och @EndIndex 500. Dessa värden ökar när koppling hämtar de efterföljande sidorna och ändra hello @StartIndex & @EndIndex värde.
* tooexecute parametriserade lagrade proceduren, ange hello parametrar i `[Name]:[Direction]:[Value]` format. Ange varje parameter på en separat rad (tryck på Ctrl + Retur tooget en ny rad).
* Allmän SQL kopplingen stödjer också importen från länkade servrar i Microsoft SQL Server. Om information ska hämtas från en tabell i länkade servern bör tabell ges i hello format:`[ServerName].[Database].[Schema].[TableName]`
* Allmän SQL Connector stöder endast de objekt som har liknande struktur (både alias namn och datatyp) mellan köra steg information och schema-identifiering. Om hello markerade objekt från schemat och informationen i kör steget skiljer sig och sedan SQL-anslutningen är toosupport den här typen av scenarier.

**SQL-fråga**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

* Flera resultatmängder frågor som inte stöds.
* SQL-frågan stöder hello sidbrytning och ange start-indexet och End indexet som en variabel toosupport sidbrytning.

### <a name="delta-import"></a>Deltaimport
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta importera konfigurationen kräver vissa ytterligare konfiguration jämfört med fullständig Import.

* Om du väljer hello utlösare eller ögonblicksbild metoden tootrack deltaändringar och ange sedan Historiktabellen eller en ögonblicksbild av databasen i **Historiktabellen eller ögonblicksbild databasnamnet** rutan.
* Du måste också tooprovide kopplingsvillkor mellan historiktabellen och den överordnade tabellen, till exempel`Employee.ID=History.EmployeeID`
* tootrack hello transaktionen på hello överordnad tabell från hello historiktabellen, måste du ange hello kolumnnamn som innehåller information om åtgärden hello (Lägg till/Uppdatera/ta bort).
* Om du väljer vattenstämpel tootrack deltaändringar och ange sedan hello kolumnnamn som innehåller hello åtgärden information i **vattenstämplar markera kolumnnamnet**.
* Hej **ändra Typattributet** krävs för hello ändra typ. Den här kolumnen mappar en ändring som inträffar i hello primära tabellen eller Flervärde tabell tooa ändra typen i hello delta-vyn. Den här kolumnen kan innehålla hello Modify_Attribute ändra typen för attributet nivå ändra eller lägga till, ändra, eller ta bort ändra typ av en ändring på objektnivå. Om det är något annat än hello standardvärdet Lägg till, ändra eller ta bort och du kan definiera dessa värden med hjälp av det här alternativet.

### <a name="export"></a>Exportera
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Stöd för allmän SQL Connector Export med fyra metoder som stöds, till exempel:

* Tabell
* Visa
* Lagrad procedur
* SQL-fråga

**Tabellen/vyn**  
Om du väljer hello tabellen/vyn alternativet genererar hello connector hello respektive frågor toodo hello Export.

**Lagrade procedurer**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Om du väljer alternativet för hello lagrade proceduren kräver Export tre olika lagrade procedurer tooperform Insert/Update/Delete åtgärder.

* **Lägg till SP namnet**: den här SP körs om objekt kommer tooconnector för infogning i hello respektive tabell.
* **Uppdatera SP namnet**: den här SP körs om objekt kommer tooconnector för uppdatering i hello respektive tabell.
* **Ta bort SP namn**: den här SP körs om objekt kommer tooconnector för borttagning i hello respektive tabell.
* Filattributet från hello-schemat som används som en parameter värdet toohello lagrad procedur. Till exempel `EmployeeName: INPUT: @EmployeeName` (EmployeeName väljs i hello connector schemat och hello koppling ersätter hello respektive värde medan export)
* toorun parametriserade lagrade proceduren, ange parametrar i `[Name]:[Direction]:[Value]` format. Ange varje parameter på en separat rad (tryck på Ctrl + Retur tooget en ny rad).

**SQL-fråga**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Om du väljer hello SQL frågealternativet kräver Export tre olika frågor tooperform Insert/Update/Delete-åtgärder.

* **Infoga frågan**: den här frågan körs om objekt kommer tooconnector för infogning i hello respektive tabell.
* **Uppdatera fråga**: den här frågan körs om objekt kommer tooconnector för uppdatering i hello respektive tabell.
* **Ta bort frågan**: den här frågan körs om objekt kommer tooconnector för borttagning i hello respektive tabell.
* Filattributet från hello-schemat som används som en parameter toohello frågan värde, till exempel`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Felsökning
* Information om hur tooenable loggning tootroubleshoot hello koppling finns hello [hur tooEnable ETW-spårning för kopplingar](http://go.microsoft.com/fwlink/?LinkId=335731).
