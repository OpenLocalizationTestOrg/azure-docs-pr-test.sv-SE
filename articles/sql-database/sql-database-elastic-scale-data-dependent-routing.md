---
title: aaaData beroende routning med Azure SQL Database | Microsoft Docs
description: "Hur toouse hello ShardMapManager klassen i .NET-appar för data-beroende routning, en funktion i delat databaser i Azure SQL Database"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 34014508ae01905686791fe096bb275cb84f53b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-dependent-routing"></a>Databeroende routning
**Data beroende routning** är hello möjlighet toouse hello data i en fråga tooroute hello begäran tooan lämplig databas. Detta är ett grundläggande mönster när du arbetar med delat databaser. hello begärandekontexten kan också vara används tooroute hello begäran, särskilt om hello horisontell partitionering nyckeln inte är en del av hello fråga. Varje specifik fråga eller en transaktion i ett program som använder data beroende routning är begränsad tooaccessing en enskild databas per begäran. För hello Azure SQL Database elastisk verktyg, den här routning sker med hello  **[ShardMapManager klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  i ADO.NET-program.

hello program behöver inte tootrack olika anslutningssträngar eller DB platser som är associerade med olika datasegment i hello delat miljö. I stället hello [Fragmentera kartan Manager](sql-database-elastic-scale-shard-map-management.md) öppnas anslutningar toohello rätt databaser vid behov, baserat på hello data i hello Fragmentera kartan och hello värdet för hello horisontell partitionering nyckel som är hello mål för hello programbegäran. hello nyckeln är vanligtvis hello *customer_id*, *tenant_id*, *date_key*, eller vissa specifika identifierare som är en grundläggande parameter för begäran om hello databas). 

Mer information finns i [skala ut SQL Server med Data beroende routning](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-hello-client-library"></a>Hämta hello-klientbibliotek
tooget hello klass, installera hello [klientbibliotek för elastisk databas](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Med hjälp av en ShardMapManager i ett beroende routning program
Program ska initiera hello **ShardMapManager** under initieringen med hello factory anropet  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. I detta exempel både en **ShardMapManager** och en specifik **ShardMap** som den innehåller har initierats. Det här exemplet visar hello GetSqlShardMapManager och [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metoder.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-hello-shard-map"></a>Använd lägsta privilegium autentiseringsuppgifter som är möjliga för att hämta hello Fragmentera karta
Om ett program inte manipulering hello Fragmentera kartan själva, hello autentiseringsuppgifter i hello fabriksmetoden bör ha bara läsbehörighet på hello **globala Fragmentera kartan** databas. Autentiseringsuppgifterna är vanligtvis skiljer sig från autentiseringsuppgifterna tooopen anslutningar toohello Fragmentera kartan manager. Se även [autentiseringsuppgifter används tooaccess hello-klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-hello-openconnectionforkey-method"></a>Anropa hello OpenConnectionForKey metod
Hej  **[ShardMap.OpenConnectionForKey metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  returnerar en ADO.Net-anslutning som är redo för att utfärda kommandon toohello lämplig databas baserat på hello värdet för hello **nyckeln**parameter. Fragmentera informationen cachelagras i hello programmet hello **ShardMapManager**, så dessa begäranden inte normalt innefattar en databassökning mot hello **globala Fragmentera kartan** databas. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* Hej **nyckeln** parametern används som en nyckel för sökning i hello Fragmentera kartan toodetermine hello lämplig databas för hello-begäran. 
* Hej **connectionString** är används toopass endast hello användarautentiseringsuppgifter för hello önskad anslutning. Inget databasnamn eller servernamn ingår i detta *connectionString* eftersom hello metoden avgör hello databasen och servern med hjälp av hello **ShardMap**. 
* Hej  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  ska anges för**ConnectionOptions.Validate** om en miljö där Fragmentera mappar kan ändras och rader kan flytta tooother databaser som en resultatet av delade eller merge-åtgärder. Detta innebär en kortfattad fråga toohello lokala Fragmentera karta på hello måldatabasen (inte toohello globala Fragmentera mappar) innan hello anslutning levereras toohello program. 

Om hello validering mot hello lokala Fragmentera mappningen misslyckas (som anger att hello cache är felaktig) hello Fragmentera kartan Manager att söka hello globala Fragmentera kartan tooobtain hello nya rätt värde för hello-sökning, hello cachen uppdateras och hämta och returnera hello lämplig databasanslutning. 

Använd **ConnectionOptions.None** endast när Fragmentera mappningsändringar inte förväntas när ett program är online. I så fall kan hello cachelagrade värden antas tooalways vara korrekt, och hello extra fram och åter validering anropet toohello måldatabasen kan hoppas på ett säkert sätt. Som minskar databastrafik. Hej **connectionOptions** kan också anges via ett värde i en konfiguration filen tooindicate om ändringar för horisontell partitionering är förväntat eller inte under en viss tidsperiod.  

Det här exemplet används hello-värdet för ett integer-nyckeln **CustomerID**med hjälp av en **ShardMap** objekt med namnet **customerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect toohello shard for that customer ID. No need toocall a SqlConnection 
    // constructor followed by hello Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

Hej **OpenConnectionForKey** metoden returnerar en ny redan öppen anslutning toohello rätt databas. Anslutningar som används i det här sättet fortfarande dra full nytta av ADO.Net anslutningspooler. Så länge transaktioner och begäranden kan betjänas av en Fragmentera samtidigt, bör detta vara hello endast ändringar som krävs i ett program som redan använder ADO.Net. 

Hej  **[OpenConnectionForKeyAsync metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  finns även om programmet gör Använd asynkron programmering med ADO.Net. Sitt beteende är hello data beroende routning motsvarar ADO. Net's  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metod.

## <a name="integrating-with-transient-fault-handling"></a>Integrera med tillfälligt fel hantering
Bästa praxis för att utveckla program i molnet hello åtkomst är tooensure som fångas av hello app tillfälliga problem och som hello operations har gjorts flera gånger innan ge ett fel. Tillfälligt fel hantering för molnprogram beskrivs på [hantering av tillfälliga fel](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Tillfälligt fel hantering kan samexistera naturligt med hello Data beroende routning mönster. hello krav är tooretry hello hela data åtkomstbegäran inklusive hello **med** block som erhålls hello data beroende routning anslutning. hello-exemplet ovan kan skrivas på följande sätt (Observera markerade ändring). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Exempel – data beroende routning med tillfälligt fel hantering
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect toohello shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Paket som är nödvändiga tooimplement tillfälligt fel hantering laddas ned automatiskt när du skapar hello elastisk databas exempelprogrammet. Paket finns också separat på [Enterprise Library - tillfälliga fel hanterar programmet Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). Använd version 6.0 eller senare. 

## <a name="transactional-consistency"></a>Transaktionskonsekvens
Transaktionell egenskaper är garanterat för alla åtgärder lokala tooa Fragmentera. Till exempel köra transaktioner som har skickats via data beroende routning inom hello omfång hello mål Fragmentera för hello-anslutning. För tillfället finns inga funktioner för att ta med flera anslutningar i en transaktion och det finns därför inga transaktionella garantier för åtgärder som utförs i shards.

## <a name="next-steps"></a>Nästa steg
toodetach en Fragmentera eller tooreattach Fragmentera, se [med hello RecoveryManager klassen toofix Fragmentera kartan problem](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

