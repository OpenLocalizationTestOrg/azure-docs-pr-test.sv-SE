---
title: "aaaMigrate din MySQL-databas med dump och återställa i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskrivs två vanliga sätt tooback upp och återställa databaser i din Azure-databas för MySQL, med hjälp av verktyg som mysqldump, MySQL arbetsstationen och PHPMyAdmin."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: d05d483ff53483df8e005eae2d9a4f8190e8f663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-mysql-database-tooazure-database-for-mysql-using-dump-and-restore"></a>Migrera din MySQL-databas tooAzure databas för MySQL med dump och återställning
Den här artikeln beskrivs två vanliga sätt tooback upp och återställa databaser i din Azure-databas för MySQL
- Dump och återställning från hello kommandoraden (med mysqldump) 
- Dump och Återställ med hjälp av PHPMyAdmin 

## <a name="before-you-begin"></a>Innan du börjar
toostep via den här hur tooguide måste toohave:
- [Skapa Azure-databas för MySQL - server i Azure-portalen](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) kommandoradsverktyg som installeras på en dator.
- MySQL-arbetsstationen [MySQL arbetsstationen hämta](https://dev.mysql.com/downloads/workbench/), Toad, Navicat eller andra tredje parts MySQL verktyget toodo dump och återställa kommandon.

## <a name="use-common-tools"></a>Använd gemensamma verktyg
Använd gemensamma verktyg och verktyg som MySQL arbetsstationen, mysqldump, Toad eller Navicat tooremotely ansluta och återställa data till Azure-databasen för MySQL. Använd dessa verktyg på klientdatorn med en internet-anslutning tooconnect toohello Azure-databas för MySQL. Använd en SSL-krypterad anslutning Metodtips för säkerhet, se även [Konfigurera SSL-anslutning i Azure-databas för MySQL](concepts-ssl-connection-security.md). Du behöver inte toomove hello dump tooany särskilda molnet platsen när du migrerar tooAzure databas för MySQL. 

## <a name="common-uses-for-dump-and-restore"></a>Vanliga användningsområden för dump och återställning
Du kan använda MySQL-verktyg, till exempel mysqldump och mysqlpump toodump och Läs in databaser i en MySQL-databas på Azure i flera vanliga scenarier. I annat fall kan du använda hello [importera och exportera](concepts-migrate-import-export.md) närmar sig i stället.

- Använd database Dumpar när du migrerar hello hela databasen. Denna rekommendation innehåller när du flyttar en stor mängd MySQL data eller när du vill toominimize avbrott i tjänsten för live-webbplatser eller program. 
-  Kontrollera att alla tabeller i databasen hello använder hello InnoDB lagringsmotorn vid inläsning av data till Azure-databas för MySQL. Azure-databas för MySQL stöder endast InnoDB lagringsmotorn och därför stöd inte för alternativa lagring motorer. Om dina tabeller har konfigurerats med andra motorer för lagring, omvandlas till hello InnoDB motorn format innan migreringen tooAzure databas för MySQL.
   Till exempel om du har en WordPress eller Webbappen med hjälp av hello MyISAM tabeller, först konvertera dessa tabeller genom att migrera till InnoDB format innan du återställer tooAzure databas för MySQL. Använd hello-satsen `ENGINE=InnoDB` tooset hello används när du skapar en ny tabell och sedan överföra hello data till hello kompatibel tabellen framför hello återställning. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- tooavoid eventuella kompatibilitetsproblem, kontrollera hello samma version av MySQL används på hello käll- och system när dumpning databaser. Till exempel om den befintliga MySQL-servern är version 5.7 migrera sedan du tooAzure databas för MySQL konfigurerad toorun version 5.7. Hej `mysql_upgrade` kommandot fungerar inte i en Azure-databas för MySQL-server och stöds inte. Om du behöver tooupgrade mellan olika versioner av MySQL först dump eller exportera lägre version databasen till en senare version av MySQL i din egen miljö. Kör sedan `mysql_upgrade`, innan du försöker migrera till en Azure-databas för MySQL.

## <a name="performance-considerations"></a>Saker att tänka på gällande prestanda
toooptimize prestanda, vidta meddelande för dessa överväganden när dumpning stora databaser:
-   Använd hello `exclude-triggers` alternativet i mysqldump när dumpning databaser. Utelämna utlöser från dumpfiler tooavoid hello utlösaren kommandon startar under hello återställning av data. 
-   Undvika hello `single-transaction` alternativet i mysqldump när dumpning mycket stora databaser. Dumpning många tabeller i en enda transaktion medför extra lagring och minne resurser toobe används under återställning och kan orsaka fördröjningar prestanda eller begränsningar för resursen.
-   Använd Flervärde infogningar vid inläsning med SQL toominimize instruktionen körning av arbetet när dumpning databaser. När du använder dumpfiler som genereras av verktyget mysqldump är Flervärde infogningar aktiverade som standard. 
-  Använd hello `order-by-primary` alternativet i mysqldump när dumpning databaser, så att hello data skripta i primär nyckel ordning.
-   Använd hello `disable-keys` alternativet i mysqldump när dumpning data, toodisable sekundärnyckelbegränsningar innan belastningen. Inaktivera främmande nycklar kontroller ger prestandavinster. Aktivera hello begränsningar och verifiera hello data när belastningen hello tooensure referensintegritet.
-   Använd partitionerade tabeller när det är lämpligt.
-   Läsa in data parallellt. Undvik att för mycket parallellitet som skulle orsaka toohit en resursgräns och övervaka resurser med hjälp av hello-mätvärden som är tillgängliga i hello Azure-portalen. 
-   Använd hello `defer-table-indexes` alternativet i mysqlpump när dumpning databaser, så att skapandet av index uppstår efter tabeller data har lästs in.

## <a name="create-a-backup-file-from-hello-command-line-using-mysqldump"></a>Skapa en säkerhetskopia från hello kommandoraden med hjälp av mysqldump
tooback in en befintlig MySQL-databas på hello lokala lokal server eller i en virtuell dator som kör hello följande kommando: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

hello parametrar tooprovide är:
- [uname] Användarnamn för databasen 
- [pass] hello lösenord för databasen (Observera att det finns inget utrymme mellan -p och hello lösenord) 
- [dbname] hello namnet på databasen 
- [backupfile.sql] hello filnamn för säkerhetskopiering av databas 
- [--välja] hello mysqldump alternativet 

Till exempel tooback upp en databas med namnet 'testdb' på servern MySQL med hello användarnamn 'testuser' och med inga lösenord tooa filen testdb_backup.sql använder hello följande kommando. hello kommandot säkerhetskopierar hello `testdb` databas till en fil med namnet `testdb_backup.sql`, som innehåller alla hello SQL-instruktioner behövs toore-skapa hello-databas. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
tooselect specifika tabeller i din databas tooback upp listan hello tabellnamn avgränsade med blanksteg. Till exempel tooback endast tabell1 och tabell2 tabeller från hello 'testdb' följa det här exemplet: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

tooback in mer än en databas på en gång, Använd hello--databasen växlar och databasnamn för listan hello avgränsade med blanksteg. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Du bör använda hello--alternativ för alla databaser tooback upp alla hello databaser i hello server i taget.
```
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-hello-target-azure-database-for-mysql-server"></a>Skapa en databas på hello mål Azure-databas för MySQL-server
Skapa en tom databas på målet hello Azure-databas för MySQL-server där du vill toomigrate hello data. Använd ett verktyg som MySQL arbetsstationen, Toad eller Navicat toocreate hello-databas. hello-databasen kan ha samma namn som hello-databas som är innesluten hello dumpade data hello eller du kan skapa en databas med ett annat namn.

tooget ansluten, leta upp hello anslutningsinformationen hello sidan med egenskaper i Azure-databas för MySQL.
![Hitta hello anslutningsinformationen i hello Azure-portalen](./media/concepts-migrate-dump-restore/1_server-properties-name-login.png)

Lägg till hello anslutningsinformation till MySQL-arbetsstationen.
![MySQL-arbetsstationen anslutningssträngen](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Återställ MySQL-databas med hjälp av kommandoradsverktyget eller MySQL arbetsstationen
Du kan använda hello mysql kommando eller MySQL arbetsstationen toorestore hello data till specifika hello nyligen skapade databasen från hello dumpfilen när du har skapat hello måldatabasen.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Återställa hello data i hello nyligen skapade databasen på målet hello Azure-databas för MySQL-servern i det här exemplet.
```bash
$ mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exportera med PHPMyAdmin
tooexport, kan du använda hello vanliga verktyget phpMyAdmin, som du redan har installerat lokalt i din miljö. tooexport MySQL-databasen med hjälp av PHPMyAdmin:
- Öppna phpMyAdmin.
- Välj din databas. Klicka på hello databasnamnet i hello listan hello vänster. 
- Klicka på hello **exportera** länk. En ny sida visas tooview hello dump av databasen.
- Klicka på hello i hello Export område, **Markera alla** länka toochoose hello tabeller i databasen. 
- Klicka på hello SQL alternativ område, hello lämpliga alternativ. 
- Klicka på hello **Spara som filen** alternativet hello motsvarande komprimering alternativ och klicka sedan på hello **Gå** knappen. En dialogruta visas återkommande erbjudanden du toosave hello filen lokalt.

## <a name="import-using-phpmyadmin"></a>Importera med hjälp av PHPMyAdmin
Importera databasen är liknande tooexporting. Hello följande åtgärder:
- Öppna phpMyAdmin. 
- På hello phpMyAdmin installationen klickar du på **Lägg till** tooadd din Azure-databas för MySQL-servern. Ange hello anslutningsinformationen och inloggningsinformation.
- Skapa en korrekt namngiven databas och markera den hello till vänster i hello-skärmen. toorewrite hello befintlig databas och klicka på hello databasens namn, markera alla hello kryssrutorna bredvid hello tabellnamn Välj **släppa** toodelete hello befintliga tabeller. 
- Klicka på hello **SQL** länk tooshow hello sida där du kan skriva i SQL-kommandon eller ladda upp din SQL-fil. 
- Använd hello **Bläddra** knappen toofind hello-databasfil. 
- Klicka på hello **Gå** knappen tooexport hello säkerhetskopiering, köra hello SQL-kommandon och återskapa din databas.

## <a name="next-steps"></a>Nästa steg
[Ansluta program tooAzure databas för MySQL](./howto-connection-string.md)
