---
title: "aaaImport och exportera i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskrivs vanliga sätt tooimport och export-databaser i Azure-databas för MySQL, med hjälp av verktyg som MySQL-arbetsstationen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: e2c036773d975df2eea2a59d166ea10567218a41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>Migrera MySQL-databas genom att importera och exportera
Den här artikeln beskrivs två vanliga metoder tooimporting och exporterar data tooan Azure-databas för MySQL-servern genom att använda MySQL-arbetsstationen. 

## <a name="before-you-begin"></a>Innan du börjar
toostep via den här hur tooguide behöver du:
- En Azure-databas för MySQL-server genom att följa [skapa en Azure-databas för MySQL-server med hjälp av Azure portal](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL-arbetsstationen [ned](https://dev.mysql.com/downloads/workbench/), eller en annan MySQL verktyget tooimport och export.

## <a name="use-common-tools"></a>Använd gemensamma verktyg
Använd gemensamma verktyg, till exempel MySQL arbetsstationen, Toad eller Navicat tooremotely ansluta och importera eller exportera data till Azure-databas för MySQL. 

Använd dessa verktyg på klientdatorn med en Internet-anslutning tooconnect tooAzure databas för MySQL. Använd en SSL-krypterad anslutning Metodtips för säkerhet, enligt beskrivningen i [Konfigurera SSL-anslutning i Azure-databas för MySQL](concepts-ssl-connection-security.md).

Du inte behöver toomove importen och exportera tooany särskilda molnet platsen när du migrerar tooAzure databas för MySQL. 

## <a name="create-a-database-on-hello-azure-database-for-mysql-server"></a>Skapa en databas på hello Azure-databas för MySQL-server
Skapa en tom databas på hello Azure-databas för MySQL-server där du vill toomigrate hello data. Använd ett verktyg som MySQL arbetsstationen, Toad eller Navicat toocreate hello-databas. hello-databasen kan ha hello samma namn som hello-databas som innehåller hello dumpade data, eller om du kan skapa en databas med ett annat namn.

tooget ansluten, leta upp hello anslutningsinformationen på hello **egenskaper** rutan i Azure för MySQL-databas.

![Hitta hello anslutningsinformationen i hello Azure-portalen](./media/concepts-migrate-import-export/1_server-properties-name-login.png)

Lägg till hello anslutning information tooMySQL arbetsstationen.

![MySQL-arbetsstationen anslutningssträngen](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-toouse-import-and-export-techniques-instead-of-a-dump-and-restore"></a>Bestämma när toouse importera och exportera metoder i stället för en dump och återställning
Använd MySQL verktyg tooimport och exportera databaser till Azure MySQL-databas i hello följande scenarier. I annat fall kan du dra nytta av hello [dump och återställa](concepts-migrate-dump-restore.md) närmar sig i stället. 

- När du behöver tooselectively välja några tabeller tooimport från en befintlig MySQL-databas till Azure MySQL-databas, bästa toouse hello importera och exportera tekniken.  På så sätt, kan du utelämna onödiga tabeller från hello migrering toosave tid och resurser. Till exempel använda hello `--include-tables` eller `--exclude-tables` växel med [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) och hello `--tables` växel med [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables).
- När du flyttar hello databasobjekt än tabeller, skapa de explicit. Omfattar begränsningar (primärnyckel, sekundärnyckel, index), vyer, funktioner, procedurer, utlösare och eventuella andra databasobjekt som du vill toomigrate.
- När du migrerar data från externa datakällor än en MySQL-databas, skapa flat-filer och importera dem med hjälp av [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html).

Kontrollera att alla tabeller i databasen hello hello InnoDB lagringsmotorn när du läser in data till Azure-databas för MySQL. Azure-databas för MySQL stöder endast hello InnoDB lagringsmotorn, därför inte stöd för alternativa lagring motorer. Om dina tabeller kräver alternativa lagring motorer, vara säker på att tooconvert dem toouse hello InnoDB motorn format innan hello migrering tooAzure för MySQL-databas. 

Till exempel om du har en WordPress- eller web app som använder hello MyISAM motorn först konvertera hello tabeller genom att migrera hello data till InnoDB tabeller. Återställ sedan tooAzure databas för MySQL. Använd hello-satsen `ENGINE=INNODB` tooset hello för att skapa en tabell och därefter överföra hello data i hello-kompatibel tabellen innan hello migreringen. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>Rekommendationer för import och export
-   Skapa grupperade index och primärnycklar innan data läses in. Läsa in data i primär nyckel ordning. 
-   Fördröjning skapandet av sekundärindex förrän data har lästs in. Skapa alla sekundärindex efter inläsning. 
-   Inaktivera sekundärnyckelbegränsningar före inläsning. Inaktivera främmande nycklar kontroller ger betydande prestandaförbättringar. Aktivera hello begränsningar och verifiera hello data när belastningen hello tooensure referensintegritet.
-   Läsa in data parallellt. Undvik att för mycket parallellitet som skulle orsaka toohit en resursgräns och övervaka resurser med hjälp av hello-mätvärden som är tillgängliga i hello Azure-portalen. 
-   Använd partitionerade tabeller när det är lämpligt.

## <a name="import-and-export-by-using-mysql-workbench"></a>Importera och exportera genom att använda MySQL-arbetsstationen
Det finns två sätt tooexport och importera data i MySQL-arbetsstationen. Var och en har ett annat syfte. 

### <a name="table-data-export-and-import-wizards-from-hello-object-browsers-context-menu"></a>Tabelldata exportera och importera guider hello objektet webbläsarens snabbmenyn
![MySQL-arbetsstationen guider på snabbmenyn för webbläsaren hello-objekt](./media/concepts-migrate-import-export/p1.png)

hello guider för tabelldata stöd importera och exportera åtgärder med hjälp av CSV- och JSON-filer. De omfattar flera konfigurationsalternativ som avgränsare, kolumnen och val av kodning. Du kan göra guiderna mot lokal eller fjärransluten anslutna MySQL-servrar. åtgärd för import av hello innehåller tabeller, kolumner och mappning. 

Du kan komma åt dessa guider hello objektet webbläsarens snabbmenyn genom att högerklicka på en tabell. Välj antingen **Data exportera Tabellguiden** eller **guiden Importera tabell-Data**. 

#### <a name="table-data-export-wizard"></a>Tabellguiden Data Export
hello exporteras följande exempel hello tabell tooa CSV-fil: 
1. Högerklicka på hello tabell med hello databasen toobe exporteras. 
2. Välj **tabell guiden exportera affärsdata**. Välj hello kolumner toobe exporteras, rad förskjutningen (eventuella) och (om sådan finns). 
3. På hello **väljer data för export** klickar du på **nästa**. Välj hello sökväg, CSV eller JSON filtyp. Välj också hello radbrytningstecken, metod i den omslutande strängar och fältavgränsaren. 
4. På hello **väljer utdata filplats** klickar du på **nästa**. 
5. På hello **exportera data** klickar du på **nästa**.

#### <a name="table-data-import-wizard"></a>Guiden Importera tabell-Data
hello importerar följande exempel hello tabell från en CSV-fil:
1. Högerklicka på hello tabell med hello databasen toobe importeras. 
2. Bläddra tooand Välj hello CSV-filen toobe importerats och klickar sedan på **nästa**. 
3. Välj hello måltabellen (ny eller befintlig) och markerar eller avmarkerar hello **trunkeringen registret före import** kryssrutan. Klicka på **Nästa**.
4. Välj kodning och hello kolumner toobe importeras och klicka sedan på **nästa**. 
5. På hello **dataimport** klickar du på **nästa**. hello guiden importerar hello data i enlighet med detta.

### <a name="sql-data-export-and-import-wizards-from-hello-navigator-pane"></a>SQL-data exportera och importera guider i hello Navigator rutan
Använd en guiden tooexport eller importera SQL genereras från MySQL arbetsstationen eller genereras från hello mysqldump kommandot. Komma åt dessa guider från hello **Navigator** fönstret eller genom att välja **Server** hello huvudmenyn. Välj sedan **dataexporten** eller **dataimporten**. 

#### <a name="data-export"></a>Export av data
![MySQL-arbetsstationen data exportera med hello Navigator fönstret](./media/concepts-migrate-import-export/p2.png)

Du kan använda hello **dataexporten** fliken tooexport MySQL-data. 
1. Markera varje schema som du vill tooexport, du kan också välja specifika schemat objekt/tabeller från varje schema och generera hello export. Konfigurationsalternativ inkluderar export tooa projektmappen eller fristående SQL-fil, dump lagrade rutiner och händelser eller hoppa över tabelldata. 
 
   Du kan också använda **exportera resultatuppsättning** tooexport ett visst resultat som i hello SQL editor tooanother format, till exempel CSV, JSON, HTML- och XML. 
3. Välj hello databasen objekt tooexport och konfigurera hello relaterade alternativ.
4. Klicka på **uppdatera** tooload hello aktuella objekt.
5. Du kan också öppna hello **avancerade alternativ** fliken toorefine hello exportåtgärden. Lägg exempelvis till tabellen lås, Använd Ersätt i stället för insert-satser och citatidentifierare med backtick tecken.
6. Klicka på **starta exportera** toobegin hello exporten.


#### <a name="data-import"></a>Import av data
![MySQL-arbetsstationen dataimporten använder Management Navigator](./media/concepts-migrate-import-export/p3.png)

Du kan använda hello **dataimporten** fliken tooimport eller återställa data som har exporterats från hello data exportåtgärden eller hello mysqldump kommando. 
1. Välj hello projektmappen eller fristående SQL-filen, Välj hello schema tooimport till, eller välj **ny** toodefine ett nytt schema. 
2. Klicka på **starta importera** toobegin hello importen.

## <a name="next-steps"></a>Nästa steg
Som en annan metod för migrering, läsa [migrera din MySQL-databas med dump och återställa i Azure-databas för MySQL](concepts-migrate-dump-restore.md). 
