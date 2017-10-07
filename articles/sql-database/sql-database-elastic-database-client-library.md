---
title: aaaBuilding skalbara molnresurser databaser | Microsoft Docs
description: "Skapa skalbara .NET databasen appar med hello klientbibliotek för elastisk databas"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 1f11c52d-13c1-4994-b9b1-5b1ae2f9255f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: ca34eff2078c0700dee1bc587f264bbfca979eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="building-scalable-cloud-databases"></a>Skapa skalbara molndatabaser
Skala ut databaser kan enkelt utföras med hjälp av skalbara verktyg och funktioner för Azure SQL Database. I synnerhet kan du använda hello **klientbibliotek för elastisk databas** toocreate och hantera databaser som skalats ut. Den här funktionen kan du enkelt utvecklar delat program med hundratals, eller till och med tusentals – för Azure SQL-databaser. [Elastiska jobb](sql-database-elastic-jobs-powershell.md) kan sedan använda toohelp förenklar hanteringen av dessa databaser.

tooinstall hello-biblioteket, gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="documentation"></a>Dokumentation
1. [Kom igång med elastiska databasverktyg](sql-database-elastic-scale-get-started.md)
2. [Elastiska databasfunktioner](sql-database-elastic-scale-introduction.md)
3. [Karthantering för shard](sql-database-elastic-scale-shard-map-management.md)
4. [Migrera befintliga databaser tooscale ut](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Databeroende routning](sql-database-elastic-scale-data-dependent-routing.md)
6. [Flera Fragmentera frågor](sql-database-elastic-scale-multishard-querying.md)
7. [Lägga till en Fragmentera med hjälp av verktyg för elastisk databas](sql-database-elastic-scale-add-a-shard.md)
8. [Program med flera klienter med elastiska Databasverktyg och säkerhet på radnivå](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [Uppgradera klienten biblioteket appar](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Översikt över elastisk frågor](sql-database-elastic-query-overview.md)
11. [Ordlista för verktyg i elastiska databaser](sql-database-elastic-scale-glossary.md)
12. [Klientbibliotek för elastisk databas med Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [Klientbibliotek för elastisk databas med Dapper](sql-database-elastic-scale-working-with-dapper.md)
14. [Dela merge tool](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Prestandaräknare för karthanteraren för shard](sql-database-elastic-database-client-library.md) 
16. [Vanliga frågor och svar för elastiska Databasverktyg](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Klientfunktioner
Skala ut program med hjälp av *horisontell partitionering* presenterar utmaningar för både hello developer samt Hej administratör. hello klientbiblioteket förenklar hello hanteringsuppgifter genom att tillhandahålla verktyg som gör att båda utvecklare och administratörer hantera databaser som skalats ut. Det finns många databaser som kallas ”shards”, toomanage i ett typexempel. Kunder som är samordnad i hello samma databas och det finns en databas per kund (en enskild klient schemat). hello klientbiblioteket innehåller följande funktioner:

- **Fragmentera kartan Management**: en särskild databas kallas hello ”Fragmentera kartan manager” skapas. Hantering av Fragmentera karta är hello möjligheten för ett program toomanage metadata om dess delar. Utvecklare kan använda den här funktionen tooregister databaser som delar, beskriver mappningar av enskilda horisontell partitionering nycklar eller nyckelintervall toothose databaser och underhålla dessa metadata som hello tal och sammansättning av databaser utvecklas tooreflect kapacitet ändringar. Utan hello klientbibliotek för elastisk databas behöver du toospend mycket tid skriva hello management kod när du implementerar horisontell partitionering. Mer information finns i [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).

- **Data beroende routning**: Anta en begäran kommer till hello program. Hello programmet måste baserat på hello horisontell partitionering nyckelvärdet för hello begäran toodetermine hello rätt databas baserat på hello nyckel/värde. Sedan öppnas en toohello databasen tooprocess hello anslutningsbegäran. Data beroende routning kan hello tooopen anslutningar med ett enda enkelt anrop till hello Fragmentera mappning av programmet hello. Data beroende routning har ett annat område av infrastrukturkod som omfattas nu av funktioner i hello klientbibliotek för elastisk databas. Mer information finns i [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md).
- **Flera Fragmentera frågor (MSQ)**: flera Fragmentera frågar fungerar när en begäran omfattar flera (eller alla) delar. En fråga med flera Fragmentera kör hello samma T-SQL-kod på alla delar eller en uppsättning shards. hello resultat från hello deltar shards slås samman till en övergripande resultatmängden med hjälp av UNION ALL semantik. hello funktioner som exponeras via hello klientbiblioteket hanterar många aktiviteter, inklusive: anslutningshanteringen, tråd management, hantering av fel och mellanresultat bearbetning. MSQ kan fråga dig toohundreds av shards. Mer information finns i [flera Fragmentera frågar](sql-database-elastic-scale-multishard-querying.md).

I allmänhet kunder som använder elastisk Databasverktyg förvänta dig tooget alla T-SQL-funktioner när du skickar in Fragmentera lokala åtgärder som skillnad från toocross Fragmentera åtgärder som har sina egna semantik.

## <a name="next-steps"></a>Nästa steg
Försök hello [exempelapp](sql-database-elastic-scale-get-started.md) som visar hello klientfunktioner. 

tooinstall hello-biblioteket, gå för[klientbibliotek för elastisk databas](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Instruktioner om hur du använder hello dela merge tool finns hello [översikt över dela merge tool](sql-database-elastic-scale-overview-split-and-merge.md).

[Klientbibliotek för elastisk databas är nu öppna ursprung!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Använd [elastisk frågor](sql-database-elastic-query-overview.md).

hello bibliotek är tillgängligt som programvara med öppen källkod på [GitHub](https://github.com/Azure/elastic-db-tools). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png

