---
title: "aaaDistributed transaktioner mellan databaser för molnet"
description: "Översikt över elastisk databastransaktioner med Azure SQL-databas"
services: sql-database
documentationcenter: 
author: torsteng
manager: jhubbard
editor: torsteng
ms.assetid: e14df7a3-7788-4cfb-bcd1-7ad6433ef1f9
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: sql-database
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 9a18f2749108d337bf115b3dc21dd0c7828dd9c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="distributed-transactions-across-cloud-databases"></a>Distribuerade transaktioner över molndatabaser
Elastisk databastransaktioner för Azure SQL Database (SQL DB) kan du toorun transaktioner som sträcker sig över flera databaser i SQL-databas. Elastisk databastransaktioner för SQL-databas som är tillgängliga för .NET-program med hjälp av ADO .NET och integrera med hello bekant programmering med hello [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) klasser. tooget hello-biblioteket, se [.NET Framework 4.6.1 (Webbinstallationsprogram)](https://www.microsoft.com/download/details.aspx?id=49981).

Lokalt krävs ett sådant scenario vanligtvis kör Microsoft Distributed Transaction Coordinator (MSDTC). Eftersom MSDTC inte är tillgänglig för Platform as a Service program i Azure, hello möjlighet toocoordinate distribuerade transaktioner har nu direkt integrerats i SQL-databas. Program kan ansluta tooany SQL-databas toolaunch distribuerade transaktioner och hello databaserna kommer transparent samordna hello distribuerade transaktion som visas i följande bild hello. 

  ![Distribuerade transaktioner med Azure SQL Database med transaktioner för elastisk databas ][1]

## <a name="common-scenarios"></a>Vanliga scenarier
Elastisk databastransaktioner för SQL DB Aktivera program toomake ändringar toodata lagras i flera olika SQL-databaser. hello preview fokuserar på klientsidan development upplevelser i C# och .NET. En serversidan upplevelse med T-SQL är planerad för ett senare tillfälle.  
Elastisk databas transaktioner mål hello följande scenarier:

* Flera databasprogram i Azure: med det här scenariot data är lodrätt partitionerad över flera databaser i SQL-databas så att olika typer av data som finns på olika databaser. Vissa åtgärder kräver ändringar toodata som är kvar i två eller flera databaser. hello programmet använder elastisk transaktioner toocoordinate hello databasändringar i databaser och kontrollera odelbarhet.
* Delat databasprogram i Azure: med det här scenariot använder hello datanivå hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md) eller self delning toohorizontally partition hello data över flera databaser i SQL-databas. Framträdande användningsfall är hello måste tooperform ändringar för ett delat program för flera innehavare när ändringar span innehavare. Se exempelvis en överföring från en klient tooanother, både som finns på olika databaser. Andra fall är detaljerade horisontell partitionering tooaccommodate kapacitetsbehov för en stor klient som i sin tur vanligtvis innebär att vissa atomiska åtgärder måste toostretch över flera databaser som används för hello samma klient. Ett tredje fall är atomiska uppdateringar tooreference data som replikeras över databaser. Atomic, överförda, operationer längs dessa rader kan nu koordineras över flera databaser med hjälp av hello preview.
  Elastisk databastransaktioner använder två faser tooensure transaktion odelbarhet över databaser. Det är passar bra för transaktioner som rör färre än 100 databaser samtidigt i en enda transaktion. Dessa begränsningar tillämpas inte, men en förvänta prestanda och slutförandefrekvenser för elastisk databas transaktioner toosuffer när överskrider gränserna.

## <a name="installation-and-migration"></a>Installation och migrering
hello funktioner för elastisk databastransaktioner i SQL DB tillhandahålls via uppdateringar toohello .NET-biblioteken System.Data.dll och System.Transactions.dll. hello DLL: er Se till att tvåfasincheckning används vid behov tooensure odelbarhet. toostart utveckla program med elastiska databastransaktioner, installera [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) eller en senare version. När du kör på en tidigare version av .NET framework hello transaktioner misslyckas toopromote tooa distribuerade transaktioner och ett undantag aktiveras.

Efter installationen kan du använda hello distributed transaction API: er i System.Transactions med anslutningar tooSQL DB. Om du har befintliga MSDTC-program med hjälp av dessa API: er kan bara återskapa dina befintliga program för .NET 4.6 när du har installerat hello 4.6.1 Framework. Om dina projekt riktar .NET 4.6, används de automatiskt hello uppdateras DLL-filer från hello nya Framework-version och distribuerade transaktioner API-anrop i kombination med anslutningar tooSQL DB ska lyckas.

Kom ihåg att elastisk databastransaktioner inte behöver installera MSDTC. Elastisk databastransaktioner hanteras i stället direkt efter och inom SQL-databas. Detta förenklar avsevärt scenarier för en distribution av MSDTC är inte nödvändigt toouse distribuerade transaktioner med SQL-databas. Avsnitt 4 förklarar vi i detalj hur toodeploy elastisk databastransaktioner och hello krävs .NET framework tillsammans med dina molntjänster program tooAzure.

## <a name="development-experience"></a>Utvecklingsarbetet
### <a name="multi-database-applications"></a>Flera databasprogram
hello använder följande exempelkod hello bekant programmering med .NET System.Transactions. hello TransactionScope klassen upprättar en omgivande transaktion i .NET. (En ”omgivande transaktion” är ett som finns i den aktuella tråden för hello.) Alla anslutningar öppnas i hello TransactionScope delta i hello transaktioner. Om olika databaser delta är hello transaktionen automatiskt utökade tooa distribuerad transaktion. hello resultatet av hello transaktionen styrs genom att ange hello scope toocomplete tooindicate ett genomförande.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Delat databasprogram
Elastisk databastransaktioner för SQL DB stöder också koordinera distribuerade transaktioner där du använder hello OpenConnectionForKey metod för hello elastisk databas-biblioteket tooopen klientanslutningar för en skaländras ut nivån. Överväg att fall där du behöver tooguarantee transaktionskonsekvens för ändringar i flera nyckelvärden för olika horisontell partitionering. Anslutningar toohello shards värd hello nyckelvärden för olika horisontell partitionering är asynkrona med OpenConnectionForKey. I vanliga fall hello vara hello anslutningar toodifferent shards så att säkerställa transaktionella garantier kräver en distribuerad transaktion. hello följande kodexempel illustrerar den här metoden. Den förutsätter att en variabel med namnet shardmap används toorepresent en Fragmentera mappa från hello klientbibliotek för elastisk databas:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Installation av .NET för Azure-molntjänster
Azure tillhandahåller flera erbjudanden toohost .NET-program. En jämförelse av olika hello-erbjudanden som är tillgängliga i [jämförelse mellan Azure App Service, molntjänster och virtuella datorer](../app-service-web/choose-web-site-cloud-service-vm.md). Om hello gästoperativsystemet för hello erbjudande är mindre än .NET 4.6.1 som krävs för elastiska transaktioner, måste tooupgrade hello gäst OS too4.6.1. 

För tjänster i Azure-App, uppgraderar du toohello gäst-OS inte stöds för närvarande. Azure virtuella datorer bara logga in hello VM och kör installationsprogrammet för hello hello senaste .NET Framework. För Azure Cloud Services behöver du tooinclude hello installation av en nyare version av .NET i hello Start uppgifter för din distribution. hello koncept och steg finns dokumenterade i [installera .NET på en rolltjänst för molnet](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Observera att hello installer för .NET 4.6.1 kan kräva mer tillfällig lagring under hello startprogram process på Azure-molntjänster än hello installationsprogram för .NET 4.6. tooensure en lyckad installation, du behöver tooincrease tillfällig lagring för din Azure-molntjänst i filen ServiceDefinition.csdef i hello LocalResources avsnitt och hello miljöinställningar av din startaktivitet enligt hello följande Exempel:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Transaktioner över flera servrar
Elastisk databastransaktioner stöds mellan olika logiska servrar i Azure SQL Database. När transaktioner mellan logisk server gränser, måste hello deltagande servrar först toobe som anges i en relation för ömsesidig kommunikation. När hello kommunikation relationen har upprättats hello någon databas i någon av hello två servrar som kan delta i elastisk transaktioner med databaser från andra servrar. Med transaktioner som sträcker sig över mer än två logiska servrar, behöver en relation för kommunikation toobe på plats för alla typer av logiska servrar.

Använd följande PowerShell-cmdlets toomanage kommunikation mellan servrar relationer för elastiska databastransaktioner hello:

* **Nya AzureRmSqlServerCommunicationLink**: Använd den här cmdlet-toocreate en ny relation för kommunikation mellan två logiska servrar i Azure SQL DB. hello relationen är symmetriska vilket innebär att båda servrarna kan initiera transaktioner med hello annan server.
* **Get-AzureRmSqlServerCommunicationLink**: Använd den här cmdlet tooretrieve befintlig kommunikation relationer och deras egenskaper.
* **Ta bort AzureRmSqlServerCommunicationLink**: Använd den här cmdlet-tooremove en befintlig relation för kommunikation. 

## <a name="monitoring-transaction-status"></a>Övervaka status för transaktioner
Använd dynamiska hanteringsvyer (av DMV: er) i SQL DB toomonitor status och förlopp för pågående elastisk databas-transaktioner. Alla av DMV: er relaterade tootransactions är relevanta för distribuerade transaktioner i SQL-databas. Du kan hitta hello motsvarande lista över av DMV: er här: [transaktion relaterade dynamiska hanteringsvyer och funktioner (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Dessa av DMV: er är särskilt användbar:

* **sys.DM\_tran\_active\_transaktioner**: Visar aktiva transaktioner just nu och deras status. hello UOW (arbetsenheten)-kolumn kan identifiera hello olika underordnade transaktioner som tillhör toohello samma distribuerade transaktionen. Alla transaktioner inom samma distribuerade transaktioner utföra hello hello samma UOW värde. Se hello [DMV dokumentationen](https://msdn.microsoft.com/library/ms174302.aspx) för mer information.
* **sys.DM\_tran\_databasen\_transaktioner**: tillhandahåller ytterligare information om transaktioner, till exempel placeringen av hello transaktion i hello-loggen. Se hello [DMV dokumentationen](https://msdn.microsoft.com/library/ms186957.aspx) för mer information.
* **sys.DM\_tran\_Lås**: innehåller information om hello lås som för tillfället hålls av en pågående transaktioner. Se hello [DMV dokumentationen](https://msdn.microsoft.com/library/ms190345.aspx) för mer information.

## <a name="limitations"></a>Begränsningar
hello som för närvarande följande begränsningar gäller tooelastic databastransaktioner i SQL-databas:

* Endast transaktioner mellan databaser i SQL-databas stöds. Andra [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) resursproviders och databaser utanför SQL-databas kan inte ingå i elastisk databastransaktioner. Det innebär att elastisk databastransaktioner inte sträcka ut över lokala SQL Server och Azure SQL-databaser. Fortsätt toouse MSDTC för distribuerade transaktioner lokalt. 
* Endast klienten koordineras transaktioner från ett .NET-program stöds. Serversidan stöd för T-SQL, till exempel BÖRJA DISTRIBUTED TRANSACTION är planerade men ännu inte tillgänglig. 
* Transaktioner över WCF-tjänster stöds inte. Du har till exempel metod en WCF-tjänst som utför en transaktion. Omslutande hello anrop inom ett transaktionsomfång misslyckas som en [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Nästa steg
För frågor kan du nå toous på hello [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) och för funktionsbegäranden, Lägg till dem toohello [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



