---
title: "aaaGetting igång med Azure storage och Visual Studio anslutna tjänster (Webbjobb projekt)"
description: "Hur tooget igång med Azure Table storage i ett Azure WebJobs-projekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 061a6c46-0592-4e5d-aced-ab7498481cde
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 80d9f8d8b493ce612623dfed7e89325fb154a1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-storage-azure-webjob-projects"></a>Komma igång med Azure Storage (Azure Webjobs-projekt)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Översikt
Den här artikeln innehåller C#-kodexempel som visar visa hur toouse hello Azure WebJobs SDK version 1.x med hello Azure table storage-tjänsten. hello kodexempel använda hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.

hello Azure Table storage-tjänst kan du toostore stora mängder strukturerade data. hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet. Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.  Se [komma igång med Azure Table storage med hjälp av .NET](../cosmos-db/table-storage-how-to-use-dotnet.md#create-a-table) för mer information.

Vissa hello kodstycken som visar hello **tabell** attribut som används i funktioner som kallas manuellt, det vill säga inte med någon av hello utlösaren attribut.

## <a name="how-tooadd-entities-tooa-table"></a>Hur tooadd entiteter tooa tabell
tooadd entiteter tooa tabell, Använd hello **tabell** attribut med ett **ICollector<T> ** eller **IAsyncCollector<T> ** parametern där **T** anger hello schemat hello entiteter som du vill tooadd. hello Attributkonstruktorn använder en strängparameter som anger hello namnet på hello tabell.

hello följande kodexempel lägger till **Person** entiteter tooa tabell med namnet *ingång*.

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

Vanligtvis hello typ som du använder med **ICollector** härleds från **TableEntity** eller implementerar **ITableEntity**, men det behöver inte. Något av följande hello **Person** klasserna arbete med hello koden som visas i föregående hello **ingång** metod.

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

Om du vill toowork direkt med hello Azure storage-API, kan du lägga till en **CloudStorageAccount** Metodsignaturen för parametern toohello.

## <a name="real-time-monitoring"></a>Realtidsövervakning
Eftersom dataåtkomstfunktioner ofta bearbetar stora mängder data, innehåller hello WebJobs SDK instrumentpanelen övervakning realtidsdata. Hej **anrop loggen** avsnittet berättar om hello funktionen fortfarande körs.

![Ingång funktionen körs](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

Hej **anrop information** sidan rapporterar hello funktionens förlopp (antal entiteter som skrivs) medan den körs och ger dig en möjlighet tooabort den.

![Ingång funktionen körs](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

När funktionen hello är klar, hello **anrop information** sidan rapporterar hello antalet rader som har skrivits.

![Ingång funktionen klar](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## <a name="how-tooread-multiple-entities-from-a-table"></a>Hur tooread flera enheter från en tabell
tooread en tabell, använder hello **tabell** attribut med ett **IQueryable<T> ** parametern skriver **T** härleds från **TableEntity**eller implementerar **ITableEntity**.

hello följande kodexempel läser och loggar alla rader från hello **ingång** tabell:

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

### <a name="how-tooread-a-single-entity-from-a-table"></a>Hur tooread en enda entitet från en tabell
Det finns en **tabell** Attributkonstruktorn med två ytterligare parametrar som kan du ange hello partitionsnyckel och radnyckel när du vill toobind tooa tabell entitet.

hello följande kodexempel läser en tabellrad för en **Person** entitet baserat på partitionen nyckel- och nyckelvärden togs emot i ett kömeddelande:  

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


Hej **Person** klassen i det här exemplet har inte tooimplement **ITableEntity**.

## <a name="how-toouse-hello-net-storage-api-directly-toowork-with-a-table"></a>Hur toouse hello .NET Storage API direkt toowork med en tabell
Du kan också använda hello **tabell** attribut med en **CloudTable** objekt för större flexibilitet när du arbetar med en tabell.

hello följande kod exempel används en **CloudTable** objekt tooadd en enda entitet toohello *ingång* tabell.

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

Mer information om hur toouse hello **CloudTable** objekt, se [komma igång med Azure Table storage med hjälp av .NET](../storage/storage-dotnet-how-to-use-tables.md).

## <a name="related-topics-covered-by-hello-queues-how-tooarticle"></a>Närliggande ämnen som omfattas av hello köer hur-tooarticle
Information om hur toohandle tabell bearbetning utlöses av ett kömeddelande eller för WebJobs SDK scenarier inte specifika tootable bearbetning, se [komma igång med Azure Queue storage och Visual Studio anslutna tjänster (Webbjobb projekt) ](../storage/vs-storage-webjobs-getting-started-queues.md).

## <a name="next-steps"></a>Nästa steg
Den här artikeln har angett koden exempel som visar hur toohandle vanliga scenarier för att arbeta med Azure-tabeller. Mer information om hur toouse Azure WebJobs och hello WebJobs SDK, se [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).

