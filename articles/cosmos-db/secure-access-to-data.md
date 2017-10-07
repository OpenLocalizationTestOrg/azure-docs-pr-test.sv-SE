---
title: "aaaLearn hur toosecure åt toodata i Azure Cosmos DB | Microsoft Docs"
description: "Lär dig mer om access control begrepp i Azure Cosmos-databasen, inklusive huvudnycklar, skrivskyddade nycklar, användare och behörigheter."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: fef7f8e14b488f6ceab0f2aa279a1e99d4416f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-access-tooazure-cosmos-db-data"></a>Skydda åtkomst tooAzure Cosmos DB data
Den här artikeln innehåller en översikt över att skydda åtkomst toodata lagras i [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos-DB använder två typer av nycklar tooauthenticate användare och ger åtkomst till tooits data och resurser. 

|Nyckeltyp|Resurser|
|---|---|
|[Huvudnycklar](#master-keys) |Används för administrativa resurser: databasen konton, databaser, användare och behörigheter|
|[Resurs-token](#resource-tokens)|Används för programresurser: samlingar, dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er|

<a id="master-keys"></a>

## <a name="master-keys"></a>Huvudnycklar 

Huvudnycklar ange åtkomst toohello alla hello administrativa resurser för hello konto. Huvudnycklar:  
- Ge åtkomst tooaccounts, databaser, användare och behörigheter. 
- Det går inte att vara används tooprovide detaljerade åtkomst toocollections och dokument.
- Skapas under hello skapandet av ett konto.
- Genereras när som helst.

Varje konto består av två huvudnycklar: en primärnyckel och sekundärnyckel. hello syftet med dubbla nycklar är så att du kan återskapa eller återställa nycklar, vilket ger kontinuerlig tooyour åtkomstkonto och data. 

Det finns två skrivskyddade nycklar i tillägg toohello två huvudnycklar för hello Cosmos-DB-kontot. Nycklarna skrivskyddat tillåter endast läsåtgärder på hello-konto. Skrivskyddade nycklar ger inte tillgång tooread behörigheter resurser.

Primära, sekundära, skrivskyddad och skrivskyddad huvudnycklar kan hämtas och återskapas med hello Azure-portalen. Instruktioner finns i [visa, kopiera och generera åtkomstnycklar](manage-account.md#keys).

![Åtkomstkontroll (IAM) i hello Azure portal – visar NoSQL database-säkerhet](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

hello är rotera din huvudnyckeln enkel. Navigera toohello Azure portal tooretrieve sekundärnyckeln, och sedan ersätta primärnyckel med din sekundärnyckeln i ditt program och rotera hello primärnyckel i hello Azure-portalen.

![Huvudnyckeln rotation i hello Azure portal – visar NoSQL database-säkerhet](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-toouse-a-master-key"></a>Code exempel toouse en huvudnyckel

hello illustrerar följande kodexempel hur toouse en Cosmos-DB kontot slutpunkt och huvudnyckeln tooinstantiate en DocumentClient och skapa en databas. 

```csharp
//Read hello Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from hello Azure portal on hello Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access tooyour DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a>Resurs-token

Token för resursen ger åtkomst toohello resurser i en databas. Resurs-token:
- Ge åtkomst toospecific samlingar, partitionsnycklar, dokument, bifogade filer, lagrade procedurer, utlösare och UDF: er.
- Skapas när en [användare](#users) beviljas [behörigheter](#permissions) tooa viss resurs.
- Återskapas när en resurs behörighet är efterlevts på av POST, hämta eller skicka anrop.
- Använd en hash-resurs token speciellt konstruerade för hello användare, resurser och behörighet.
- Är Tidsbundna med en anpassningsbara giltighetsperiod. hello standard giltigt timespan är en timme. Livslängd för token, men kan uttryckligen anges, in tooa maximalt fem timmar.
- Ange en säker alternativa toogiving ut hello huvudnyckel. 
- Aktivera klienter tooread, skriva och ta bort resurser i hello Cosmos DB konto enligt toohello behörighet de tilldelats.

Du kan använda en resurs-token (genom att skapa Cosmos-DB-användare och behörigheter) när du vill tooprovide åtkomst tooresources i Cosmos-DB kontot tooa klient som inte är betrott med hello huvudnyckel.  

Cosmos DB resurs token är en säker alternativ som gör att klienter tooread, skriva och ta bort resurser i Cosmos-DB-kontot enligt toohello behörigheter som du har beviljat och utan behov av antingen master eller läsa endast nyckel.

Här är en typisk designmönstret där resurs token kan begärs, skapas och levereras tooclients:

1. En tjänst halva nivå ställs in tooserve en mobilprogram tooshare Användarfoton. 
2. tjänst för hello halva nivå har hello huvudnyckeln för hello Cosmos-DB-konto.
3. hello foto-app är installerad på slutanvändarens mobila enheter. 
4. Du loggar in upprättar hello foto-app hello identitet hello användare med tjänst för hello halva nivå. Den här mekanismen för identitet etablering är rent upp toohello program.
5. När hello identitet har upprättats tjänstbegäranden hello halva nivå behörigheter baserat på hello identitet.
6. hello halva nivå tjänsten skickar en resurs token tillbaka toohello phone app.
7. hello phone-app fortsätta toouse hello resurs token toodirectly åtkomst Cosmos DB resurser med hello behörigheter har definierats av hello resurs token och för hello-intervall som tillåts av hello resurs token. 
8. När hello resurs token upphör att gälla, får ett 401 obehörig undantag efterföljande förfrågningar.  Nu hello telefonapp återupprättar hello identitet och begär en ny resurs-token.

    ![Azure DB Cosmos-resurs tokens arbetsflöde](./media/secure-access-to-data/resourcekeyworkflow.png)

Resurs-token generering och hantering hanteras av hello interna Cosmos DB klientbibliotek; Om du använder REST måste du skapa hello begäran/autentiseringshuvuden. Mer information om hur du skapar autentiseringshuvuden för REST finns [åtkomstkontroll på Cosmos DB resurser](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources) eller hello [källkoden för våra SDK](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Ett exempel på en tjänst på mellannivå används toogenerate eller broker resurs-token, finns hello [ResourceTokenBroker app](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Användare
Cosmos DB användare har associerats med en Cosmos-DB-databas.  Varje databas kan innehålla noll eller flera Cosmos-DB-användare.  Hej följande exempel visas hur toocreate en Cosmos-DB användarresurs.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Varje Cosmos-DB-användare har en PermissionsLink-egenskap som kan vara används tooretrieve hello lista över [behörigheter](#permissions) som är associerade med hello användare.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>Behörigheter
En Cosmos-DB behörighet resurs är associerad med en Cosmos-DB-användare.  Varje användare kan innehålla noll eller flera Cosmos-DB-behörigheter.  En behörighet resurs ger åtkomst tooa säkerhetstoken som hello användarnas behov när du försöker tooaccess en resurs för specifika program.
Det finns två tillgängliga åtkomstnivåer som kan tillhandahållas av en behörighet:

* Alla: hello användaren har fullständig behörighet för hello resurs.
* Läs: hello användare kan endast läsa hello innehållet i hello resurs men kan inte utföra skrivåtgärder-, update- eller delete-åtgärder på hello resursen.

> [!NOTE]
> Användaren i ordning toorun Cosmos DB lagrade procedurer hello måste ha hello alla behörighet på hello samling i vilka hello lagrade proceduren körs.
> 
> 

### <a name="code-sample-toocreate-permission"></a>Koden exempel toocreate behörighet

hello följande kodexempel visar hur toocreate en behörighet resurs läsa hello resurs token av hello behörighet för resursen och koppla hello behörigheter till hello [användaren](#users) skapade ovan.

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = documentCollection.SelfLink,
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

Om du har angett en partitionsnyckel för samlingen, sedan hello-behörighet för insamling, dokument och bilaga resurser måste också innehålla hello ResourcePartitionKey i tillägget toohello ResourceLink.

### <a name="code-sample-tooread-permissions-for-user"></a>Koden exempel tooread behörigheter för användare

tooeasily hämta alla behörighet resurser som är associerade med en viss användare, Cosmos DB tillgängliggör en behörighet för varje användarobjekt.  hello följande kodavsnitt visar hur tooretrieve hello behörigheten som är associerad med hello användaren som skapade ovan, skapa en behörighetslista och initiera en ny Dokumentklient hello användarens räkning.

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a>Nästa steg
* toolearn mer om Cosmos DB databassäkerhet finns [Cosmos DB: databasen säkerhet](database-security.md).
* toolearn om hur du hanterar master och skrivskyddade nycklar, se [hur toomanage ett konto i Azure Cosmos DB](manage-account.md#keys).
* toolearn hur tooconstruct Azure Cosmos DB auktorisering token Se [åtkomstkontroll på Azure Cosmos DB resurser](https://docs.microsoft.com/rest/api/documentdb/access-control-on-documentdb-resources).
