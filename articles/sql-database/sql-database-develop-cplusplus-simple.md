---
title: aaaConnect tooSQL databasen med C och C++ | Microsoft Docs
description: "Använd hello exemplet kod i den här snabbstartsguide toobuild moderna program med C++ och backas upp av en relationsdatabaser i hello moln med Azure SQL Database."
services: sql-database
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: 
ms.assetid: 07d9e0b1-3234-4f17-a252-a7559160a9db
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 03/06/2017
ms.author: edmacauley
ms.openlocfilehash: 9b581424c91bfdd93deb6914212629519a011d8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-database-using-c-and-c"></a>Ansluta tooSQL databasen med C och C++
Det här exemplet syftar C och C++ utvecklare försök tooconnect tooAzure SQL-databas. Den är uppdelad i avsnitt så att du kan gå toohello avsnitt som bäst samlar in ditt intresse. 

## <a name="prerequisites-for-hello-cc-tutorial"></a>Krav för hello C/C++-självstudier
Kontrollera att du har hello följande objekt:

* Ett aktivt Azure-konto. Om du inte har ett kan du registrera dig för en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/). Du måste installera hello C++ språk komponenter toobuild och kör det här exemplet.
* [Visual Studio Linux Development](https://visualstudiogallery.msdn.microsoft.com/725025cf-7067-45c2-8d01-1e0fd359ae6e). Om du utvecklar på Linux, måste du också installera hello Linux för Visual Studio-tillägget. 

## <a id="AzureSQL"></a>Azure SQL Database och SQL Server på virtuella datorer
Azure SQL bygger på Microsoft SQL Server och är utformad tooprovide en hög tillgänglighet, performant och skalbar service. Det finns många fördelar toousing SQL Azure via din egna databas som körs lokalt. Med SQL Azure du inte har tooinstall, konfigurera, underhålla eller hantera din databas, men endast hello innehåll och hello strukturen för din databas. Vanliga saker som vi oroa dig med databaser som feltolerans och redundans är inbyggda i. 

Azure har två alternativ för värd för SQL server-arbetsbelastningar: Azure SQL-databas, databasen som en tjänst och SQLServer på virtuella datorer (VM). Vi kommer inte att hämta information om hello skillnaderna mellan dessa två förutom att Azure SQL-databas är det bästa alternativet för nya molnbaserade program tootake utnyttja hello besparingar och prestandaoptimeringar som molntjänster. Om du funderar på att migrera eller utöka din lokala program toohello moln, kanske SQLServer på virtuella Azure-datorn fungerar ut bättre för dig. tookeep saker enkel för den här artikeln ska vi skapa en Azure SQL database. 

## <a id="ODBC"></a>Tekniker för dataåtkomst: ODBC och OLE DB
Anslutande tooAzure SQL-databas är inte annorlunda och det finns två sätt tooconnect toodatabases: ODBC (Open Database connectivity) och OLE DB (Object Linking och inbäddning databas). Under de senaste åren har Microsoft justerad med [ODBC för åtkomst till interna relationella data](https://blogs.msdn.microsoft.com/sqlnativeclient/2011/08/29/microsoft-is-aligning-with-odbc-for-native-relational-data-access/). ODBC är relativt enkel och även mycket snabbare än OLE DB. hello här endast begränsning är ODBC använder en gamla API C-format. 

## <a id="Create"></a>Steg 1: Skapa din Azure SQL-databas
Se hello [komma igång med](sql-database-get-started-portal.md) toolearn hur toocreate en exempeldatabas.  Du kan också följa detta [korta videon i två minuter](https://azure.microsoft.com/documentation/videos/azure-sql-database-create-dbs-in-seconds/) toocreate en Azure SQL database med hello Azure-portalen.

## <a id="ConnectionString"></a>Steg 2: Hämta anslutningssträngen
När Azure SQL database har etablerats, du behöver toocarry ut hello följande steg toodetermine anslutningsinformationen och lägga till din klientens IP-Adressen för brandväggen åtkomst. 

I [Azure-portalen](https://portal.azure.com/), gå tooyour Azure SQL ODBC databasanslutningssträng med hjälp av hello **visa databasanslutningssträngar** visas som en del av hello översiktsavsnittet för databasen: 

![ODBCConnectionString](./media/sql-database-develop-cplusplus-simple/azureportal.png)

![ODBCConnectionStringProps](./media/sql-database-develop-cplusplus-simple/dbconnection.png)

Kopiera hello innehållet på hello **ODBC (innehåller Node.js) [SQL-autentisering]** sträng. Vi använder den här strängen senare tooconnect från våra kommandoradsverktyget C++ ODBC-tolken. Den här strängen innehåller information som hello drivrutin, server och andra parametrar för anslutningen. 

## <a id="Firewall"></a>Steg 3: Lägg till IP-toohello brandväggen
Gå toohello brandväggen avsnittet för databasservern och Lägg till din [klienten IP toohello brandväggen med hjälp av de här stegen](sql-database-configure-firewall-settings.md) toomake att vi kan upprätta en anslutning: 

![AddyourIPWindow](./media/sql-database-develop-cplusplus-simple/ip.png)

Nu har konfigurerat din Azure SQL-databas och är redo tooconnect från din kod i C++. 

## <a id="Windows"></a>Steg 4: Ansluter från en Windows C/C++-program
Du kan enkelt ansluta tooyour [Azure SQL-databas med hjälp av ODBC i Windows med det här exemplet](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29) som bygger med Visual Studio. hello exempel implementerar en ODBC-kommandoradsverktyget tolk som kan använda tooconnect tooour Azure SQL DB. Det här exemplet krävs antingen en databas namnet fil (DSN) källfil som kommandoradsargument eller hello utförlig anslutningssträngen som vi tidigare kopierade från hello Azure-portalen. Öppna hello egenskapssidan för det här projektet och klistra in anslutningssträngen hello som ett kommandoargument som visas här: 

![DSN Propsfile](./media/sql-database-develop-cplusplus-simple/props.png)

Kontrollera att du anger information om hello rätt autentisering för databasen som en del av den anslutningssträngen för databasen. 

Starta hello programmet toobuild den. Du bör se hello följande fönstret verifiera anslutningen. Du kan även köra vissa grundläggande SQL-kommandon som **Skapa tabell** toovalidate database-anslutningen:

![SQL-kommandon](./media/sql-database-develop-cplusplus-simple/sqlcommands.png)

Du kan också skapa en DSN-fil som guiden hello som startas när inga kommandoargument har angetts. Vi rekommenderar att det här alternativet och försök. Du kan använda den här DSN-filen för automatisering och skydda dina inställningar för autentisering: 

![Skapa DSN-fil](./media/sql-database-develop-cplusplus-simple/datasource.png)

Grattis! Du har nu anslutits tooAzure SQL med C++ och ODBC i Windows. Du kan fortsätta läsa toodo hello samma för Linux-plattformen. 

## <a id="Linux"></a>Steg 5: Anslutning från ett C/C++ för Linux-program
Om du inte har hört hello nyheter ännu, kan Visual Studio du nu toodevelop C++ Linux-program. Du kan läsa om den här nya scenariot i hello [Visual C++ för Linux-utveckling](https://blogs.msdn.microsoft.com/vcblog/2016/03/30/visual-c-for-linux-development/) blogg. toobuild för Linux måste en fjärrdator där Linux-distro körs. Om du inte har någon tillgänglig, kan du ange ett dig snabbt med [Linux Azure virtuella datorer](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

För den här självstudiekursen Låt oss förutsätter att du har en distributionsplats för Ubuntu 16.04 Linux ställa in. hello stegen här bör också tillämpas tooUbuntu 15.10, Red Hat 6 och 7 för Red Hat. 

hello installera följande hello bibliotek som krävs för SQL och ODBC för din distro:

    sudo su
    sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/mssql-ubuntu-test/ xenial main" > /etc/apt/sources.list.d/mssqlpreview.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    apt-get update
    apt-get install msodbcsql
    apt-get install unixodbc-dev-utf16 #this step is optional but recommended*

Starta Visual Studio. Under Verktyg -> Alternativ -> Cross Platform -> Anslutningshanteraren, lägga till en anslutning tooyour Linux: 

![Alternativ](./media/sql-database-develop-cplusplus-simple/tools.png)

När SSH-anslutningen har upprättats kan du skapa en anges projektmallen (Linux): 

![Ny projektmall](./media/sql-database-develop-cplusplus-simple/template.png)

Du kan sedan lägga till en [nya C källfil och Ersätt den med det här innehållet](https://github.com/Microsoft/VCSamples/blob/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29/odbcconnector/odbcconnector.c). Med hello ODBC-API: er SQLAllocHandle, SQLSetConnectAttr och SQLDriverConnect, bör du vara kan tooinitialize och upprätta en anslutning tooyour databas. Precis som med hello Windows ODBC-exemplet behöver du tooreplace hello SQLDriverConnect anrop med hello information från din databas sträng anslutningsparametrar kopieras från hello Azure portal tidigare. 

     retcode = SQLDriverConnect(
        hdbc, NULL, "Driver=ODBC Driver 13 for SQL"
                    "Server;Server=<yourserver>;Uid=<yourusername>;Pwd=<"
                    "yourpassword>;database=<yourdatabase>",
        SQL_NTS, outstr, sizeof(outstr), &outstrlen, SQL_DRIVER_NOPROMPT);

Hej senaste sak toodo innan du kompilerar tooadd **odbc** som biblioteket beroende: 

![Lägga till ODBC som ett inkommande bibliotek](./media/sql-database-develop-cplusplus-simple/lib.png)

toolaunch ditt program ta fram hello Linux-konsolen från hello **felsöka** menyn: 

![Linux-konsolen](./media/sql-database-develop-cplusplus-simple/linuxconsole.png)

Om anslutningen har upprättats bör nu se hello aktuella databasnamnet ut i hello Linux-konsolen: 

![Utdata från Linux-konsolen](./media/sql-database-develop-cplusplus-simple/linuxconsolewindow.png)

Grattis! Du har slutfört hello självstudiekursen och kan nu ansluta tooyour Azure SQL DB från C++ på Windows- och Linux-plattformar.

## <a id="GetSolution"></a>Hämta hello fullständiga C/C++ självstudiekursen lösningen
Du kan hitta hello GetStarted-lösningen som innehåller alla hello prover i den här artikeln på github:

* [ODBC-C++ Windows exempel](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28windows%29), hämta hello Windows C++ ODBC exempel tooconnect tooAzure SQL
* [ODBC-C++ Linux exempel](https://github.com/Microsoft/VCSamples/tree/master/VC2015Samples/ODBC%20database%20sample%20%28linux%29), hämta hello Linux C++ ODBC exempel tooconnect tooAzure SQL

## <a name="next-steps"></a>Nästa steg
* Granska hello [Utvecklingsöversikt för SQL-databas](sql-database-develop-overview.md)
* Mer information om hello [ODBC API-referens](https://docs.microsoft.com/sql/odbc/reference/syntax/odbc-api-reference/)

## <a name="additional-resources"></a>Ytterligare resurser
* [Designmönster för SaaS-program med flera klientorganisationer med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Utforska alla hello [funktionerna i SQL-databas](https://azure.microsoft.com/services/sql-database/)

