---
title: "aaaUse mongoimport och mongorestore med hello Azure Cosmos DB API för MongoDB | Microsoft Docs"
description: "Lär dig hur toouse mongoimport och mongorestore tooimport data tooan API för MongoDB-konto"
keywords: mongoimport mongorestore
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 921354bc7b09a076a73e0cbf5e4aabcc9e83d5a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: Importera MongoDB-data 

toomigrate data från MongoDB tooan Azure DB som Cosmos-konto för användning av hello API för MongoDB, måste du:

* Hämta antingen *mongoimport.exe* eller *mongorestore.exe* från hello [MongoDB Download Center](https://www.mongodb.com/download-center).
* Hämta din [API: et för MongoDB-anslutningssträng](connect-mongodb-account.md).

Om du importerar data från MongoDB och planera toouse med hello Azure Cosmos DB bör du använda hello [datamigreringsverktyget](import-data.md) tooimport data.

Den här kursen ingår hello följande uppgifter:

> [!div class="checklist"]
> * Hämta din anslutningssträng
> * Importera MongoDB-data med hjälp av mongoimport
> * Importera MongoDB-data med hjälp av mongorestore

## <a name="prerequisites"></a>Krav

* Öka genomflödet: hello varaktighet för din datamigrering beror på hello mängden genomströmning som du angett för samlingar. Vara säker på att tooincrease hello genomströmning för större migrering av data. När du har slutfört hello migreringen, minska hello genomströmning toosave kostnaderna. Mer information om ökar genomströmningen i hello [Azure-portalen](https://portal.azure.com), se [prestandanivåer och prisnivåerna i Azure Cosmos DB](performance-levels.md).

* Aktivera SSL: Azure Cosmos-DB har stränga säkerhetskrav och standarder. Vara säker på att tooenable SSL när du kommunicerar med ditt konto. hello procedurer i hello resten av hello artikel innefattar hur tooenable SSL för mongoimport och mongorestore.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Hitta din Anslutningssträngsinformation (värd, port, användarnamn och lösenord)

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänster, klicka på hello **Azure Cosmos DB** post.
2. I hello **prenumerationer** rutan, Välj namnet på ditt konto.
3. I hello **anslutningssträngen** bladet, klickar du på **anslutningssträngen**.  
hello högra fönstret innehåller alla hello information du behöver toosuccessfully ansluta tooyour-konto.

    ![Anslutningen sträng bladet](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-toohello-api-for-mongodb-by-using-mongoimport"></a>Importera data toohello API: et för MongoDB med mongoimport

tooimport data tooyour Azure DB som Cosmos-konto, använder hello följande mall. Fyll i *värden*, *användarnamn*, och *lösenord* med hello-värden som är specifika tooyour konto.  

Mall:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Exempel:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-toohello-api-for-mongodb-by-using-mongorestore"></a>Importera data toohello API: et för MongoDB med mongorestore

toorestore data tooyour API: et för MongoDB-konto, använder hello följande mall tooexecute hello import. Fyll i *värden*, *användarnamn*, och *lösenord* med hello-värden som är specifika tooyour konto.

Mall:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Exempel:

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Guide för en lyckad migrering

1. Skapa och skala dina samlingar:
        
    * Standard etablerar Azure Cosmos DB en ny MongoDB-samling med 1 000 frågeenheter (RUs). Innan du börjar hello migrering med hjälp av mongoimport, mongorestore eller mongomirror skapa alla samlingar från hello [Azure-portalen](https://portal.azure.com) eller från MongoDB-drivrutiner och verktyg. Om samlingen är större än 10 GB, kontrollerar du att toocreate en [delat/partitionerad samling](partition-data.md) med en lämplig Fragmentera nyckel.

    * Från hello [Azure-portalen](https://portal.azure.com), öka genomflödet av dina samlingar från 1 000 RUs för en enskild partition samling och 2 500 RUs för en delat samling för hello migrering. Med hello högre genomströmning, kan du undvika begränsning och migrera på kortare tid. Med varje timme fakturering i Azure Cosmos-databasen, kan du minska hello genomströmning omedelbart efter hello migrering toosave kostnader.

2. Beräkna hello ungefärliga RU kostnad för att skriva ett enskilt dokument:

    a. Ansluta tooyour Azure Cosmos DB MongoDB-databas från hello MongoDB-gränssnittet. Du hittar anvisningar i [ansluta en MongoDB programmet tooAzure Cosmos DB](connect-mongodb-account.md).
    
    b. Kör ett Exempelkommando för insert genom att använda ett av dokumenten exempel hello MongoDB Shell:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Kör ```db.runCommand({getLastRequestStatistics: 1})``` och du får ett svar som detta:
     
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```
        
    d. Anteckna hello begäran kostnad.
    
3. Avgör hello svarstid från din dator toohello Azure Cosmos DB Molntjänsten:
    
    a. Aktivera utförlig loggning från hello MongoDB-gränssnittet med hjälp av det här kommandot:```setVerboseShell(true)```
    
    b. Köra en enkel fråga mot databasen hello: ```db.coll.find().limit(1)```. Du får ett svar som detta:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. Ta bort hello infogas dokument innan hello migrering tooensure att det inte finns några dubbletter dokument. Du kan ta bort dokument med hjälp av det här kommandot:```db.coll.remove({})```

5. Beräkna hello ungefärliga *batchSize* och *numInsertionWorkers* värden:

    * För *batchSize*, division hello totalt etablerats RUs av hello RUs förbrukad från dina dokument Skriv i steg 3.
    
    * Om hello beräknade *batchSize* < = 24, använder den som din *batchSize* värde.
    
    * Om hello beräknade *batchSize* > 24, ange hello *batchSize* värdet too24.
    
    * För *numInsertionWorkers*, Använd den här formeln: *numInsertionWorkers = (dataflöde * fördröjning i sekunder) / (batchstorlek * används RUs för en enskild skrivning)*.
        
    |Egenskap|Värde|
    |--------|-----|
    |batchSize| 24 |
    |RUs etablerats | 10000 |
    |Svarstid | 0.100 s |
    |RU debiteras för 1 doc-skrivning | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 RUs x 0,1 s) / (24 x 10 RUs) = 4.1666*

6. Kör hello slutliga migrering kommando:

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a>Nästa steg

Du kan fortsätta toohello nästa kurs och lära dig hur tooquery MongoDB-data med hjälp av Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Hur tooquery MongoDB data?](../cosmos-db/tutorial-query-mongodb.md)
