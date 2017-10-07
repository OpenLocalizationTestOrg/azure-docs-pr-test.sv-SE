---
title: "aaaDeploy split-kopplingstjänsten | Microsoft Docs"
description: "Dela och slå samman med elastiska Databasverktyg"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6fef641cbc1e73ce34a851c53298a072dade8f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-split-merge-service"></a>Distribuera en tjänst för att dela/sammanslå
hello dela dokument med verktyget kan du flytta data mellan delat databaser. Se [flytta data mellan databaser som skalats ut moln](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-hello-split-merge-packages"></a>Hämta hello dela Merge-paket
1. Hämta hello senaste NuGet-versionen från [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).
2. Öppna en kommandotolk och navigera toohello katalogen där du hämtade nuget.exe. hello hämtningen innehåller PowerShell commmands.
3. Hämta hello senaste dela Merge-paketet i hello aktuell katalog med hello nedan kommando:
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

hello filerna placeras i en katalog med namnet **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** där *x.x.xxx.x* visar hello versionsnumret. Hitta hello delade dokument Service-filer i hello **content\splitmerge\service** underkatalog, och hello delade dokument PowerShell-skript (och nödvändiga klient DLL-filer) i hello **content\splitmerge\powershell** underkatalog.

## <a name="prerequisites"></a>Krav
1. Skapa en Azure SQL DB-databas som ska användas som hello delade dokument status databas. Gå toohello [Azure-portalen](https://portal.azure.com). Skapa en ny **SQL-databas**. Namnge hello databas och skapa en ny administratör och lösenord. Vara säker på att toorecord hello namn och lösenord för senare användning.
2. Se till att din Azure SQL DB-server gör att Azure-tjänster tooconnect tooit. I hello-portalen i hello **brandväggsinställningar**, se till att hello **och ge åtkomst tooAzure tjänster** inställningen för**på**. Klicka på hello ”spara”-ikonen.
   
   ![Tillåtna tjänster][1]
3. Skapa ett Azure Storage-konto som ska användas för diagnostik utdata. Gå toohello Azure-portalen. Klicka på vänster hello-fältet **ny**, klickar du på **Data + lagring**, sedan **lagring**.
4. Skapa en Azure-molntjänst som innehåller delning-kopplingstjänsten.  Gå toohello Azure-portalen. Klicka på vänster hello-fältet **ny**, sedan **Compute**, **Molntjänsten**, och **skapa**. 

## <a name="configure-your-split-merge-service"></a>Konfigurera din delade kopplingstjänsten
### <a name="split-merge-service-configuration"></a>Dela kopplingstjänsten konfiguration
1. Skapa en kopia av hello i hello mappen dit du hämtade hello delade dokument sammansättningar **ServiceConfiguration.Template.cscfg** som levererades tillsammans med **SplitMergeService.cspkg** och byta namn på det. **ServiceConfiguration.cscfg**.
2. Öppna **ServiceConfiguration.cscfg** i en textredigerare som Visual Studio som validerar indata till exempel hello format för certifikattumavtryck.
3. Skapa en ny databas eller välj en befintlig databas tooserve som hello status databas för delade Merge-operationer och hämta hello anslutningssträngen för databasen. 
   
   > [!IMPORTANT]
   > För tillfället hello status databasen måste använda hello latinska sortering (SQL\_Latin1\_allmänna\_CP1\_CI\_AS). Mer information finns i [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).
   >

   Med Azure SQL DB är hello anslutningssträngen vanligtvis hello formuläret:
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. Ange den här anslutningssträngen i hello cscfg-filen i båda hello **SplitMergeWeb** och **SplitMergeWorker** roll avsnitt i hello ElasticScaleMetadata inställningen.
5. För hello **SplitMergeWorker** roll, ange en giltig anslutning sträng tooAzure lagring för hello **WorkerRoleSynchronizationStorageAccountConnectionString** inställningen.

### <a name="configure-security"></a>Konfigurera säkerhet
För detaljerade anvisningar tooconfigure hello säkerheten för hello-tjänsten, referera toohello [delade dokument säkerhetskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).

Hello enligt en enkel testdistributionen för den här självstudiekursen utföra en minimal uppsättning configuration steg tooget hello tjänst igång. Dessa steg aktiverar endast hello en/datorkontot toocommunicate med hello-tjänsten körs.

### <a name="create-a-self-signed-certificate"></a>Skapa ett självsignerat certifikat
Skapa en ny katalog och från den här katalogen execute hello följande kommando med hjälp av en [Developer kommandotolk för Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) fönster:

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

Du uppmanas att ange lösenord tooprotect hello privat nyckel. Ange ett starkt lösenord och bekräfta. Du uppmanas sedan hello lösenord toobe används en gång efter som. Klicka på **Ja** på hello slutet tooimport den toohello betrodda certifikatutfärdare myndigheter rotarkivet.

### <a name="create-a-pfx-file"></a>Skapa en PFX-fil
Kör följande kommando från hello hello samma fönster där makecert utfördes; Använd hello samma lösenord som du använt toocreate hello certifikat:

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-hello-client-certificate-into-hello-personal-store"></a>Importera hello klientcertifikatet till hello datorarkivet
1. Dubbelklicka i Utforskaren, **MyCert.pfx**.
2. I hello **guiden Importera certifikat** Välj **aktuell användare** och på **nästa**.
3. Bekräfta hello sökväg och klicka på **nästa**.
4. Ange hello lösenord, lämna **inkludera alla utökade egenskaper** markerad och klicka på **nästa**.
5. Lämna **väljer automatiskt hello certifikatarkiv [...]**  markerad och klicka på **nästa**.
6. Klicka på **Slutför** och **OK**.

### <a name="upload-hello-pfx-file-toohello-cloud-service"></a>Överför hello PFX-filen toohello Molntjänsten
1. Gå toohello [Azure Portal](https://portal.azure.com).
2. Välj **molntjänster**.
3. Välj hello molntjänst som du skapade ovan för hello dela/kopplingstjänsten.
4. Klicka på **certifikat** på hello översta menyn.
5. Klicka på **överför** i hello nedre fältet.
6. Välj hello PFX-filen och ange hello samma lösenord som ovan.
7. Kopiera hello certifikatets tumavtryck från hello ny post i listan hello när klar.

### <a name="update-hello-service-configuration-file"></a>Uppdatera hello-tjänstkonfigurationsfil
Klistra in tumavtrycket för certifikatet hello kopieras över till hello tumavtrycksvärde/attribut för de här inställningarna.
För hello worker-rollen:
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

För hello webbroll:

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Observera att för Produktionsdistribution separat certifikat ska användas för hello Certifikatutfärdaren för kryptering, hello servercertifikat och klientcertifikat. Detaljerad information om detta finns i [säkerhetskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).

## <a name="deploy-your-service"></a>Distribuera din tjänst
1. Gå toohello [Azure-portalen](https://manage.windowsazure.com).
2. Klicka på hello **molntjänster** hello vänster och välj hello molntjänst som du skapade tidigare.
3. Klicka på **instrumentpanelen**.
4. Välj hello mellanlagring miljö och klicka sedan på **ladda upp en ny fristående distribution**.
   
   ![Mellanlagring][3]
5. Ange en distributionsetiketten hello i dialogrutan. Klicka på 'Från lokala' för både 'Paketet ”och” Configuration ”och välj hello **SplitMergeService.cspkg** fil- och .cscfg-filen som du tidigare har konfigurerat.
6. Se till att kryssrutan hello **distribuera även om en eller flera roller innehåller en enda instans** är markerad.
7. Nådde hello tick-knappen i hello nedre högra toobegin hello distribution. Förväntat tootake toocomplete för några minuter.

   ![Ladda upp][4]

## <a name="troubleshoot-hello-deployment"></a>Felsöka hello-distribution
Om din webbroll misslyckas toocome online, är det troligt problem med hello säkerhetskonfiguration. Kontrollera att hello SSL har konfigurerats enligt beskrivningen ovan.

Om arbetsrollen misslyckas toocome online, men din webbroll lyckas, är det troligen inte att ansluta toohello status databasen som du skapade tidigare.

* Kontrollera att det stämmer hello anslutningssträngen i din .cscfg.
* Kontrollera att hello-servern och databasen finns och att hello användar-id och lösenord är korrekta.
* Hello anslutningssträngen bör vara hello formatet för Azure SQL DB:

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* Kontrollera att servernamnet hello inte börjar med **https://**.
* Se till att din Azure SQL DB-server gör att Azure-tjänster tooconnect tooit. toodo, öppna https://manage.windowsazure.com, klickar du på ”SQL-databaser på hello vänster, klickar du på” servrar ”hello överst och markera din server. Klicka på **konfigurera** på hello uppifrån och se till att hello **Azure Services** inställningen för ”Ja”. (Se krav för hello avsnittet hello överst i den här artikeln).

## <a name="test-hello-service-deployment"></a>Testa hello tjänstdistribution
### <a name="connect-with-a-web-browser"></a>Ansluta med en webbläsare
Fastställa hello webbslutpunkten för din delade kopplingstjänsten. Du hittar det i hello klassiska Azure-portalen genom att gå toohello **instrumentpanelen** Molntjänsten och söker **Webbadress** hello höger. Ersätt **http://** med **https://** eftersom hello standardsäkerhetsinställningarna inaktivera hello HTTP-slutpunkt. Läs in hello sidan för denna URL i webbläsaren.

### <a name="test-with-powershell-scripts"></a>Testa med PowerShell-skript
hello distribution och din miljö kan testas genom att köra hello ingår exempel PowerShell-skript.

hello skriptfiler ingår är:

1. **SetupSampleSplitMergeEnvironment.ps1** -ställer in en test datanivå för delade/Merge (se tabellen nedan ger en detaljerad beskrivning)
2. **ExecuteSampleSplitMerge.ps1** -kör teståtgärder på hello test data tjänstnivån (se tabellen nedan ger en detaljerad beskrivning)
3. **GetMappings.ps1** - översta exempel på skript som skriver ut hello hello Fragmentera mappningar aktuella tillstånd.
4. **ShardManagement.psm1** -helper-skript som omsluter hello ShardManagement API
5. **SqlDatabaseHelpers.psm1** -helper-skript för att skapa och hantera SQL-databaser
   
   <table style="width:100%">
     <tr>
       <th>PowerShell-fil</th>
       <th>Steg</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1.    Skapar en Fragmentera kartan manager-databas</td>
     </tr>
     <tr>
       <td>2.    Skapar 2 Fragmentera databaser.
     </tr>
     <tr>
       <td>3.    Skapar en Fragmentera karta för dessa databasen (ta bort alla befintliga Fragmentera mappar på de databaserna som). </td>
     </tr>
     <tr>
       <td>4.    Skapar en liten exempeltabell i båda hello shards och fylls hello tabell i ett hello delar.</td>
     </tr>
     <tr>
       <td>5.    Deklarerar hello SchemaInfo för hello delat tabellen.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell-fil</th>
       <th>Steg</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1.    Skickar en delad begäran toohello dela kopplingstjänsten web frontend som delar upp halva hello data från hello första Fragmentera toohello andra Fragmentera.</td>
     </tr>
     <tr>
       <td>2.    Omröstningar hello web frontend för hello dela status för tjänstbegäran och väntar tills hello begäran har slutförts.</td>
     </tr>
     <tr>
       <td>3.    Skickar en sammanslagning begäran toohello dela kopplingstjänsten web frontend som flyttar hello data från hello andra Fragmentera tillbaka toohello första Fragmentera.</td>
     </tr>
     <tr>
       <td>4.    Omröstningar hello web frontend för hello merge begärandestatus och väntar tills hello begäran har slutförts.</td>
     </tr>
   </table>
   
## <a name="use-powershell-tooverify-your-deployment"></a>Använd PowerShell tooverify distributionen
1. Öppna ett nytt fönster i PowerShell och navigera toohello katalogen där du hämtade hello delade dokument paketet och gå sedan till hello ”powershell” katalog.
2. Skapa en Azure SQL database-server (eller välj en befintlig server) där hello Fragmentera kartan manager och shards kommer att skapas.
   
   > [!NOTE]
   > Hej SetupSampleSplitMergeEnvironment.ps1 skript skapar de här databaserna på hello samma server med tookeep hello standardskript enkla. Detta är inte en begränsning för hello dela kopplingstjänsten sig själv.
   >
   
   En SQL-autentisering-inloggning med läsning och skrivning åtkomst toohello DBs som behövs för hello dela kopplade service toomove data och uppdatera hello Fragmentera mappar. Eftersom hello dela kopplingstjänsten körs i molnet hello, stöder den för närvarande inte integrerad autentisering.
   
   Kontrollera att hello Azure SQL server är konfigurerat tooallow åtkomst från hello IP-adressen för hello-dator som kör dessa skript. Du hittar den här inställningen under hello Azure SQL-server / configuration / tillåtna IP-adresser.
3. Köra hello SetupSampleSplitMergeEnvironment.ps1 skriptet toocreate hello exempel miljö.
   
   Kör skriptet kommer att radera alla befintliga Fragmentera management kartdata strukturer på hello Fragmentera kartan manager-databasen och hello delar. Det kan vara användbart toorerun hello skript om du vill initiera toore hello Fragmentera kartan eller delar.
   
   Kommandorad:

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. Köra hello Getmappings.ps1 tooview hello skriptmappningar som för närvarande finns i hello exempel miljö.
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. Köra hello ExecuteSampleSplitMerge.ps1 skriptet tooexecute en split-åtgärd (halva hello data flyttas på hello första Fragmentera toohello andra Fragmentera) och en merge-operation (hello data flyttas tillbaka till hello första Fragmentera). Om du har konfigurerat SSL och vänstra hello http-slutpunkten inaktiverad du kontrollera att du använder hello https:// slutpunkt i stället.
   
   Kommandorad:

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   Om du får hello nedan fel är troligen ett problem med din webbslutpunkten certifikat. Försök att ansluta toohello webbslutpunkten med din favorit webbläsare och kontrollera om det finns fel på certifikatet.
   
     ```
     Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLSsecure channel.
     ```
   
   Om det lyckades hello utdata ska se ut så hello nedan:
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. Experimentera med andra datatyper! Alla dessa skript tar en valfri parameter för - ShardKeyType som gör att du toospecify hello nyckeltyp. hello standardvärdet är Int32, men du kan också ange Int64, Guid eller Binary.

## <a name="create-requests"></a>Startförfrågan
hello-tjänsten kan användas med hjälp av hello webbgränssnittet eller genom att importera och använda hello SplitMerge.psm1 PowerShell-modul som kommer att skicka din begäran via hello webbroll.

hello-tjänsten kan flytta data i både delat tabeller och referenstabeller. Ett delat tabellen har en nyckelkolumn för horisontell partitionering och har olika raddata på varje Fragmentera. Är inte en referenstabell delat så att den innehåller hello samma rad data på varje Fragmentera. För referenstabeller är användbart för data som inte ändras ofta och använda tooJOIN med delat tabeller i frågor.

Du måste deklarera hello delat tabeller och referenstabeller som du vill flytta toohave i ordning tooperform en delad merge-operation. Detta åstadkoms med hello **SchemaInfo** API. Detta API är i hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namnområde.

1. För varje delat tabell skapar en **ShardedTableInfo** -objektet som beskriver hello tabell överordnade schemanamn (valfritt, standardvärden för ”dbo”) hello tabellnamn och hello kolumnnamnet i den tabell som innehåller hello horisontell partitionering nyckel.
2. För varje tabell, skapar du en **ReferenceTableInfo** -objektet som beskriver hello tabell överordnade schemanamn (valfritt, standardvärden för ”dbo”) och hello tabellnamn.
3. Lägg till hello ovan TableInfo objekt tooa nya **SchemaInfo** objekt.
4. Hämta en referens tooa **ShardMapManager** objektet och anropet **GetSchemaInfoCollection**.
5. Lägg till hello **SchemaInfo** toohello **SchemaInfoCollection**, tillhandahåller hello Fragmentera mappningsnamn.

Ett exempel på detta kan ses i hello SetupSampleSplitMergeEnvironment.ps1 skript.

hello dela kopplingstjänsten skapar inte hello måldatabasen (eller schemat för alla tabeller i databasen hello) för dig. De måste vara skapats i förväg innan du skickar en begäran toohello tjänst.

## <a name="troubleshooting"></a>Felsökning
Hello under meddelandet kan visas när du kör hello exempel powershell-skript:

   ```
   Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLS secure channel.
   ```

Det här felet betyder att SSL-certifikatet inte har konfigurerats korrekt. Följ hello anvisningarna i avsnittet ”ansluta med en webbläsare'.

Om du inte kan skicka begäranden visas här:

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

I det här fallet Kontrollera konfigurationsfilen i viss hello inställning för **WorkerRoleSynchronizationStorageAccountConnectionString**. Det här felet indikerar vanligtvis att hello arbetsrollen inte kunde initiera hello metadata-databasen vid första användning har. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

