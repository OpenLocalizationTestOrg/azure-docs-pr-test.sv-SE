---
title: "Använd Azure-tabellagring med WebJobs SDK:n"
description: "Lär dig mer om att använda Azure table storage med WebJobs SDK. Skapa tabeller, lägga till enheter i tabeller och läsa befintliga tabeller."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 451432cc-c780-4310-85d3-84f44fe48afe
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 13cfc788c14d714df7022ce003d34691cf73d121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-with-the-webjobs-sdk"></a>Använd Azure-tabellagring med WebJobs SDK:n
## <a name="overview"></a>Översikt
Den här guiden innehåller C#-kodexempel som visar hur du läser och skriver Azure storage-tabeller med hjälp av [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.

Handboken förutsätter att du vet [hur du skapar ett Webbjobb-projekt i Visual Studio med anslutning strängar som pekar på ditt lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller [flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Vissa av koden kodavsnitt visas den `Table` attribut som används i funktioner som är [kallas manuellt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), det vill säga inte genom att använda ett av attributen för utlösare. 

## <a id="ingress"></a>Lägga till entiteter i en tabell
Om du vill lägga till enheter i en tabell, använder den `Table` attribut med ett `ICollector<T>` eller `IAsyncCollector<T>` parametern där `T` anger schemat för de enheter som du vill lägga till. Attributkonstruktorn använder en strängparameter som anger namnet på tabellen. 

Följande kodexempel lägger till `Person` enheter till en tabell med namnet *ingång*.

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() { 
                        PartitionKey = "Test", 
                        RowKey = i.ToString(), 
                        Name = "Name" }
                    );
            }
        }

Typen du använder vanligtvis med `ICollector` härleds från `TableEntity` eller implementerar `ITableEntity`, men det behöver inte. Något av följande `Person` klasserna arbete med koden som visas i den föregående `Ingress` metod.

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

Om du vill arbeta direkt med Azure storage API, kan du lägga till en `CloudStorageAccount` parameter till Metodsignaturen.

## <a id="monitor"></a>Realtidsövervakning
Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller WebJobs SDK instrumentpanelen övervakning realtidsdata. Den **anrop loggen** avsnittet berättar om funktionen fortfarande körs.

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

Den **anrop information** sidan rapporterar funktionens status (antal entiteter som skrivs) medan den körs och ger dig en möjlighet att avbryta den. 

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

När funktionen har slutförts i **anrop information** sidan rapporterar antalet rader som har skrivits.

![Ingång funktionen klar](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <a id="multiple"></a>Läsa flera enheter från en tabell
Läs en tabell genom att använda den `Table` attribut med ett `IQueryable<T>` parametern skriver `T` härleds från `TableEntity` eller implementerar `ITableEntity`.

Följande kodexempel läser och loggar alla rader från den `Ingress` tabellen:

        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}", 
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <a id="readone"></a>Läsa en enda entitet från en tabell
Det finns en `Table` Attributkonstruktorn med två ytterligare parametrar som kan du ange partitionsnyckel och radnyckel när du vill binda till en enskild tabell entitet.

Följande kodexempel läser en tabellrad för en `Person` entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


Den `Person` klassen i det här exemplet behöver inte implementera `ITableEntity`.

## <a id="storageapi"></a>Hur du använder .NET Storage API direkt för att arbeta med en tabell
Du kan också använda den `Table` attribut med ett `CloudTable` objekt för större flexibilitet när du arbetar med en tabell.

I följande kod exempel används en `CloudTable` objekt att lägga till en enda entitet till den *ingång* tabell. 

        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

Mer information om hur du använder den `CloudTable` objekt, se [använda Table Storage från .NET](../cosmos-db/table-storage-how-to-use-dotnet.md). 

## <a id="queues"></a>Närliggande ämnen som omfattas av den här artikeln köer
Information om hur du hanterar tabell bearbetning som utlöses av ett kömeddelande eller WebJobs-SDK-scenarier som inte är specifika för bearbetning av tabellen finns [använda Azure queue storage med WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Följande: avsnitt beskrivs i artikeln

* Async-funktion
* Flera instanser
* Korrekt avslutning
* Använda WebJobs-SDK-attribut i brödtexten för en funktion
* Ange anslutningssträngar SDK i kod
* Ange värden för WebJobs SDK konstruktorparametrarna i koden
* Utlös en funktion manuellt
* Skriva loggar

## <a id="nextsteps"></a>Nästa steg
Den här guiden tillhandahåller kodexempel som visar hur du hanterar vanliga scenarier för att arbeta med Azure-tabeller. Mer information om hur du använder Azure WebJobs och WebJobs-SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).

