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
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="892b6-104">Använda Robomongo med en Azure-Cosmos-DB: API för MongoDB-konto</span><span class="sxs-lookup"><span data-stu-id="892b6-104">Use Robomongo with an Azure Cosmos DB: API for MongoDB account</span></span>
<span data-ttu-id="892b6-105">tooconnect tooan Azure Cosmos DB: API för MongoDB-konto med hjälp av Robomongo, måste du:</span><span class="sxs-lookup"><span data-stu-id="892b6-105">tooconnect tooan Azure Cosmos DB: API for MongoDB account using Robomongo, you must:</span></span>

* <span data-ttu-id="892b6-106">Hämta och installera [Robomongo](https://robomongo.org/)</span><span class="sxs-lookup"><span data-stu-id="892b6-106">Download and install [Robomongo](https://robomongo.org/)</span></span>
* <span data-ttu-id="892b6-107">Har Azure Cosmos-DB: API för MongoDB konto [anslutningssträngen](connect-mongodb-account.md) information</span><span class="sxs-lookup"><span data-stu-id="892b6-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="connect-using-robomongo"></a><span data-ttu-id="892b6-108">Ansluta med Robomongo</span><span class="sxs-lookup"><span data-stu-id="892b6-108">Connect using Robomongo</span></span>
<span data-ttu-id="892b6-109">tooadd Azure Cosmos-DB: API: et för MongoDB konto toohello Robomongo MongoDB-anslutningar, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="892b6-109">tooadd your Azure Cosmos DB: API for MongoDB account toohello Robomongo MongoDB Connections, perform hello following steps.</span></span>

1. <span data-ttu-id="892b6-110">Hämta Azure Cosmos-DB: API för MongoDB anslutning kontoinformation använder hello instruktioner [här](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="892b6-110">Retrieve your Azure Cosmos DB: API for MongoDB account connection information using hello instructions [here](connect-mongodb-account.md).</span></span>

    ![Skärmbild av bladet för hello anslutning sträng](./media/mongodb-robomongo/connectionstringblade.png)
2. <span data-ttu-id="892b6-112">Kör *Robomongo.exe*</span><span class="sxs-lookup"><span data-stu-id="892b6-112">Run *Robomongo.exe*</span></span>

3. <span data-ttu-id="892b6-113">Klicka på hello anslutning under **filen** toomanage anslutningar.</span><span class="sxs-lookup"><span data-stu-id="892b6-113">Click hello connection button under **File** toomanage your connections.</span></span> <span data-ttu-id="892b6-114">Klicka på **skapa** i hello **MongoDB anslutningar** fönster som öppnas hello **anslutningsinställningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="892b6-114">Then, click **Create** in hello **MongoDB Connections** window, which will open up hello **Connection Settings** window.</span></span>

4. <span data-ttu-id="892b6-115">I hello **anslutningsinställningar** fönster, Välj ett namn.</span><span class="sxs-lookup"><span data-stu-id="892b6-115">In hello **Connection Settings** window, choose a name.</span></span> <span data-ttu-id="892b6-116">Leta sedan upp hello **värden** och **Port** från informationen i steg 1 och ange dem till **adress** och **Port**respektive .</span><span class="sxs-lookup"><span data-stu-id="892b6-116">Then, find hello **Host** and **Port** from your connection information in Step 1 and enter them into **Address** and **Port**, respectively.</span></span>

    ![Skärmbild som visar hello Robomongo hantera anslutningar](./media/mongodb-robomongo/manageconnections.png)
5. <span data-ttu-id="892b6-118">På hello **autentisering** klickar du på **utför autentisering**.</span><span class="sxs-lookup"><span data-stu-id="892b6-118">On hello **Authentication** tab, click **Perform authentication**.</span></span> <span data-ttu-id="892b6-119">Ange sedan databasen (standard är *Admin*), **användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="892b6-119">Then, enter your Database (default is *Admin*), **User Name** and **Password**.</span></span>
<span data-ttu-id="892b6-120">Båda **användarnamn** och **lösenord** finns i informationen i steg 1.</span><span class="sxs-lookup"><span data-stu-id="892b6-120">Both **User Name** and **Password** can be found in your connection information in Step 1.</span></span>

    ![Skärmbild som visar hello Robomongo fliken för autentisering](./media/mongodb-robomongo/authentication.png)
6. <span data-ttu-id="892b6-122">På hello **SSL** markerar **Använd SSL-protokollet**, ändra hello **autentiseringsmetod** för**ett självsignerat certifikat**.</span><span class="sxs-lookup"><span data-stu-id="892b6-122">On hello **SSL** tab, check **Use SSL protocol**, then change hello **Authentication Method** too**Self-signed Certificate**.</span></span>

    ![Skärmbild som visar hello Robomongo SSL-fliken](./media/mongodb-robomongo/SSL.png)
7. <span data-ttu-id="892b6-124">Klicka slutligen på **Test** tooverify att du har kan tooconnect **spara**.</span><span class="sxs-lookup"><span data-stu-id="892b6-124">Finally, click **Test** tooverify that you are able tooconnect, then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="892b6-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="892b6-125">Next steps</span></span>
* <span data-ttu-id="892b6-126">Utforska Azure Cosmos DB: API för MongoDB [exempel](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="892b6-126">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
