---
title: aaaManage ett Azure DB som Cosmos-konto via hello Azure Portal | Microsoft Docs
description: "Lär dig hur toomanage Azure Cosmos-DB-kontot via hello Azure-portalen. Hitta en vägledning om hur du använder hello Azure Portal tooview, kopiera, ta bort och åtkomst."
keywords: Azure-portalen documentdb, azure, Microsoft azure
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: 77ad953cf558a519674be08ad913a12202f69496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-an-azure-cosmos-db-account"></a>Hur toomanage ett Azure DB som Cosmos-konto
Lär dig hur tooset globalt konsekvensfel, arbeta med nycklar och ta bort ett Azure DB som Cosmos-konto i hello Azure-portalen.

## <a id="consistency"></a>Hantera Azure Cosmos DB konsekvenskontroll inställningar
Att välja rätt konsekvensnivå för hello beror på hello semantiken för ditt program. Du bör du bekanta dig med hello tillgängliga konsekvensnivåer i Azure Cosmos-databasen genom att läsa [med konsekvenskontroll nivåer toomaximize tillgänglighet och prestanda i Azure Cosmos DB][consistency]. Azure Cosmos-DB ger konsekvens, tillgänglighet och prestanda garantier, på alla nivåer för konsekvenskontroll som är tillgängliga för ditt konto. Konfigurera ditt konto med en konsekvenskontroll nivå av starka kräver att dina data är begränsat tooa enda Azure-region och globalt inte tillgängliga. På andra sidan hello, hello Avslappnad konsekvensnivåer - avgränsas föråldrad, session eller eventuell aktivera du tooassociate valfritt antal Azure-regioner med ditt konto. hello följande enkla steg visar hur tooselect hello standardnivån för konsekvens för ditt konto. 

### <a name="toospecify-hello-default-consistency-for-an-azure-cosmos-db-account"></a>toospecify hello standardkonsekvensen för ett konto i Azure Cosmos DB
1. I hello [Azure-portalen](https://portal.azure.com/), komma åt ditt konto i Azure Cosmos DB.
2. I hello-kontoblad klickar du på **standard konsekvenskontroll**.
3. I hello **standard konsekvenskontroll** bladet, Välj hello nya konsekvensnivå och klickar på **spara**.
    ![Standard konsekvenskontroll session][5]

## <a id="keys"></a>Visa, kopiera och återskapa åtkomstnycklar
När du skapar ett konto i Azure Cosmos DB genererar hello tjänsten två master åtkomstnycklar som kan användas för autentisering när hello Azure Cosmos DB konto används. Genom att tillhandahålla två åtkomstnycklar kan Azure Cosmos DB du tooregenerate hello nycklarna utan avbrott tooyour Azure DB som Cosmos-konto. 

I hello [Azure-portalen](https://portal.azure.com/), åtkomst hello **nycklar** bladet hello resurs-menyn på hello **Azure Cosmos DB konto** bladet tooview, kopiera och generera hello åtkomstnycklar som är används tooaccess Azure DB som Cosmos-konto.

![Skärmbild av Azure Portal, bladet nycklar](./media/manage-account/keys.png)

> [!NOTE]
> Hej **nycklar** bladet innehåller också primära och sekundära anslutningssträngar som kan använda tooconnect tooyour konto från hello [Datamigreringsverktyg](import-data.md).
> 
> 

Skrivskyddade nycklar är också tillgängliga på det här bladet. Läsning och frågor är skrivskyddade åtgärder stund skapar, tar bort, inte och ersätter.

### <a name="copy-an-access-key-in-hello-azure-portal"></a>Kopiera en snabbtangent i hello Azure-portalen
På hello **nycklar** bladet, klickar du på hello **kopiera** knappen toohello höger i hello nyckel som du vill att toocopy.

![Visa och kopiera snabbtangent i hello bladet nycklar i Azure Portal](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Återskapa åtkomstnycklar
Du bör ändra hello nycklar tooyour Azure Cosmos DB åtkomstkonto regelbundet toohelp skydda dina anslutningar. Två åtkomstnycklar tilldelas tooenable du toomaintain anslutningar toohello Azure DB som Cosmos-konto med hjälp av ena åtkomstnyckeln medan du återskapar hello andra snabbtangent.

> [!WARNING]
> Återskapar åtkomstnycklarna påverkar alla program som är beroende av hello aktuella nyckeln. Alla klienter som använder hello viktiga tooaccess hello Azure Cosmos DB åtkomstkonto måste vara uppdaterade toouse hello ny nyckel.
> 
> 

Om du har program eller molntjänster med hjälp av hello Azure Cosmos DB konto förlorar du hello anslutningar om du återskapa nycklar, såvida inte du lanserar dina nycklar. hello beskriver följande steg hello processen ingår i rullande dina nycklar.

1. Uppdatera hello snabbtangent i ditt program kod tooreference hello sekundära åtkomstnyckeln för hello Azure DB som Cosmos-konto.
2. Återskapa hello primära åtkomstnyckeln för ditt Azure DB som Cosmos-konto. I hello [Azure Portal](https://portal.azure.com/), komma åt ditt konto i Azure Cosmos DB.
3. I hello **Azure Cosmos-DB konto** bladet, klickar du på **nycklar**.
4. På hello **nycklar** bladet hello generera knappen Klicka **Ok** tooconfirm som du vill toogenerate en ny nyckel.
    ![Återskapa åtkomstnycklar](./media/manage-account/regenerate-keys.png)
5. När du har verifierat hello nya nyckeln är tillgänglig för användning uppdatera (cirka 5 minuter efter återskapandet), hello snabbtangent i ditt program kod tooreference hello nya primärnyckeln.
6. Återskapa hello sekundära åtkomstnyckeln.
   
    ![Återskapa åtkomstnycklar](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Det kan ta flera minuter innan en nyligen skapade nyckel kan använda tooaccess Azure DB som Cosmos-konto.
> 
> 

## <a name="get-hello--connection-string"></a>Hämta hello anslutningssträngen
tooretrieve anslutningen sträng, hello följande: 

1. I hello [Azure-portalen](https://portal.azure.com), komma åt ditt konto i Azure Cosmos DB.
2. Hello resurs menyn klickar du på **nycklar**.
3. Klicka på hello **kopiera** knappen Nästa toohello **primära anslutningssträngen** eller **sekundära anslutningssträngen** rutan. 

Om du använder hello anslutningssträngen i hello [Migreringsverktyget för Azure Cosmos DB databasen](import-data.md), Lägg till hello databas namnet toohello slutet av hello anslutningssträngen. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>Ta bort ett Azure DB som Cosmos-konto
tooremove en Azure-Cosmos-DB kontot från hello Azure-portalen som du inte längre använder, högerklicka på hello kontonamn och klicka på **ta bort kontot**.

![Hur toodelete en Cosmos-databas med Azure-konto i hello Azure-portalen](./media/manage-account/deleteaccount.png)

1. I hello [Azure-portalen](https://portal.azure.com/), komma åt hello Azure Cosmos DB-konto som du vill toodelete.
2. På hello **Azure Cosmos DB konto** bladet, högerklicka på hello-kontot och klicka sedan på **ta bort konto**. 
3. Ange hello Azure Cosmos DB konto namnet tooconfirm som du vill toodelete hello konto på hello resulterande bekräftelse-bladet.
4. Klicka på hello **ta bort** knappen.

![Hur toodelete en Cosmos-databas med Azure-konto i hello Azure-portalen](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Nästa steg
Lär dig hur för[komma igång med ditt konto i Azure Cosmos DB](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
