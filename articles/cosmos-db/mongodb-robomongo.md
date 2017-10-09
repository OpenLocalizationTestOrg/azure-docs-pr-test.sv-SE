---
title: "aaaUse Robomongo för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig hur toouse Robomongo med en Azure-Cosmos-DB: API för MongoDB-konto"
keywords: robomongo
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
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 3018243e904015426dc69a69b26fb53421e1fe4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Använda Robomongo med en Azure-Cosmos-DB: API för MongoDB-konto
tooconnect tooan Azure Cosmos DB: API för MongoDB-konto med hjälp av Robomongo, måste du:

* Hämta och installera [Robomongo](https://robomongo.org/)
* Har Azure Cosmos-DB: API för MongoDB konto [anslutningssträngen](connect-mongodb-account.md) information

## <a name="connect-using-robomongo"></a>Ansluta med Robomongo
tooadd Azure Cosmos-DB: API: et för MongoDB konto toohello Robomongo MongoDB-anslutningar, utföra hello följande steg.

1. Hämta Azure Cosmos-DB: API för MongoDB anslutning kontoinformation använder hello instruktioner [här](connect-mongodb-account.md).

    ![Skärmbild av bladet för hello anslutning sträng](./media/mongodb-robomongo/connectionstringblade.png)
2. Kör *Robomongo.exe*

3. Klicka på hello anslutning under **filen** toomanage anslutningar. Klicka på **skapa** i hello **MongoDB anslutningar** fönster som öppnas hello **anslutningsinställningar** fönster.

4. I hello **anslutningsinställningar** fönster, Välj ett namn. Leta sedan upp hello **värden** och **Port** från informationen i steg 1 och ange dem till **adress** och **Port**respektive .

    ![Skärmbild som visar hello Robomongo hantera anslutningar](./media/mongodb-robomongo/manageconnections.png)
5. På hello **autentisering** klickar du på **utför autentisering**. Ange sedan databasen (standard är *Admin*), **användarnamn** och **lösenord**.
Båda **användarnamn** och **lösenord** finns i informationen i steg 1.

    ![Skärmbild som visar hello Robomongo fliken för autentisering](./media/mongodb-robomongo/authentication.png)
6. På hello **SSL** markerar **Använd SSL-protokollet**, ändra hello **autentiseringsmetod** för**ett självsignerat certifikat**.

    ![Skärmbild som visar hello Robomongo SSL-fliken](./media/mongodb-robomongo/SSL.png)
7. Klicka slutligen på **Test** tooverify att du har kan tooconnect **spara**.

## <a name="next-steps"></a>Nästa steg
* Utforska Azure Cosmos DB: API för MongoDB [exempel](mongodb-samples.md).
