---
title: "aaaDesign första Azure SQL database - C# | Microsoft Docs"
description: "Lär dig toodesign din första Azure SQL-databas och ansluta tooit med ett C#-program med hjälp av ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 07/31/2017
ms.author: genemi;carlrab
ms.openlocfilehash: 8161de24bff1ec2fa307efa93adab2bd1b761fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Utforma en Azure SQL database och ansluta med C & #x23; och ADO.NET

Azure SQL Database är en relationell databas-som-en tjänst (DBaaS) i hello Microsoft Cloud (”Azure”). I kursen får du lära dig hur toouse hello Azure-portalen och ADO.NET med Visual Studio för att: 

> [!div class="checklist"]
> * Skapa en databas i hello Azure-portalen
> * Konfigurera en brandväggsregel på servernivå i hello Azure-portalen
> * Ansluta toohello databasen med ADO.NET och Visual Studio
> * Skapa tabeller med ADO.NET
> * Infoga, uppdatera och ta bort data med ADO.NET 
> * Fråga efter data ADO.NET

Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Krav

En installation av [Visual Studio Community 2017, Visual Studio Professional 2017 eller Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- hello following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- hello following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig grundläggande uppgifter som skapar en databas och tabeller, läsa in fråga efter data och återställningspunkt hello databasen tooa tidigare tidpunkt. Du har lärt dig att:
> [!div class="checklist"]
> * Skapa en databas
> * Konfigurera en brandväggsregel
> * Ansluta toohello databasen med [Visual Studio och C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Skapa tabeller
> * Infoga, uppdatera och ta bort data
> * Frågedata

Nästa självstudiekurs toolearn toohello om att migrera dina data i förväg.

> [!div class="nextstepaction"]
>[Migrera din SQL Server-databasen tooAzure SQL-databas](sql-database-migrate-your-sql-server-database.md)

