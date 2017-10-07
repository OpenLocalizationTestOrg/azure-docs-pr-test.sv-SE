---
title: aaaHow toouse Azure table storage med hello WebJobs SDK
description: "Lär dig hur toouse Azure table storage med hello WebJobs-SDK. Skapa tabeller, lägga till entiteter tootables och läsa befintliga tabeller."
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
ms.openlocfilehash: 8e28c69df4a934646add9e50c6de28e76dca1636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-with-hello-webjobs-sdk"></a>Hur toouse Azure table storage med hello WebJobs SDK
## <a name="overview"></a>Översikt
Den här guiden innehåller C#-kodexempel som visar hur tooread och skriva Azure storage-tabeller med hjälp av [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.

hello handboken förutsätts att du vet [hur toocreate ett Webbjobb projekt i Visual Studio med anslutning strängar som punkt tooyour lagringskonto](websites-dotnet-webjobs-sdk-get-started.md) eller för[flera lagringskonton](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Vissa hello kodstycken som visar hello `Table` attribut som används i funktioner som är [kallas manuellt](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), det vill säga inte med någon av hello utlösaren attribut. 

## <a id="ingress"></a>Hur tooadd entiteter tooa tabell
tooadd entiteter tooa tabell, Använd hello `Table` attribut med ett `ICollector<T>` eller `IAsyncCollector<T>` parametern där `T` anger hello schemat hello entiteter som du vill tooadd. hello Attributkonstruktorn använder en strängparameter som anger hello namnet på hello tabell. 

hello följande kodexempel lägger till `Person` entiteter tooa tabell med namnet *ingång*.

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

Vanligtvis hello typ som du använder med `ICollector` härleds från `TableEntity` eller implementerar `ITableEntity`, men det behöver inte. Något av följande hello `Person` klasserna arbete med hello koden som visas i föregående hello `Ingress` metod.

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

Om du vill toowork direkt med hello Azure storage-API, kan du lägga till en `CloudStorageAccount` Metodsignaturen för parametern toohello.

## <a id="monitor"></a>Realtidsövervakning
Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller hello WebJobs SDK instrumentpanelen övervakning realtidsdata. Hej **anrop loggen** avsnittet berättar om hello funktionen fortfarande körs.

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

Hej **anrop information** sidan rapporterar hello funktionens förlopp (antal entiteter som skrivs) medan den körs och ger dig en möjlighet tooabort den. 

![Ingång funktionen körs](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

När funktionen hello är klar, hello **anrop information** sidan rapporterar hello antalet rader som har skrivits.

![Ingång funktionen klar](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <a id="multiple"></a>Hur tooread flera enheter från en tabell
tooread en tabell, använder hello `Table` attribut med ett `IQueryable<T>` parametern skriver `T` härleds från `TableEntity` eller implementerar `ITableEntity`.

hello följande kodexempel läser och loggar alla rader från hello `Ingress` tabell:

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

### <a id="readone"></a>Hur tooread en enda entitet från en tabell
Det finns en `Table` Attributkonstruktorn med två ytterligare parametrar som kan du ange hello partitionsnyckel och radnyckel när du vill toobind tooa tabell entitet.

hello följande kodexempel läser en tabellrad för en `Person` entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:  

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


Hej `Person` klassen i det här exemplet har inte tooimplement `ITableEntity`.

## <a id="storageapi"></a>Hur toouse hello .NET Storage API direkt toowork med en tabell
Du kan också använda hello `Table` attribut med ett `CloudTable` objekt för större flexibilitet när du arbetar med en tabell.

hello följande kod exempel används en `CloudTable` objekt tooadd en enda entitet toohello *ingång* tabell. 

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

Mer information om hur toouse hello `CloudTable` objekt, se [hur toouse Table Storage från .NET](../cosmos-db/table-storage-how-to-use-dotnet.md). 

## <a id="queues"></a>Närliggande ämnen som omfattas av hello köer hur-tooarticle
Information om hur toohandle tabell bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tootable bearbetning, se [hur toouse Azure kö lagring med hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Avsnitt som beskrivs i artikeln inkludera hello följande:

* Async-funktion
* Flera instanser
* Korrekt avslutning
* Använda WebJobs-SDK-attribut i hello brödtext
* Ange hello SDK-anslutningssträngar i kod
* Ange värden för WebJobs SDK konstruktorparametrarna i koden
* Utlös en funktion manuellt
* Skriva loggar

## <a id="nextsteps"></a>Nästa steg
Den här guiden har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure-tabeller. Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).

