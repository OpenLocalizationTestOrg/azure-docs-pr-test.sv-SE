---
title: "Skapa och hantera Azure-databas för MySQL-servern med hjälp av Azure portal | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-server och hantera servern med hjälp av Azure-portalen."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4605518b6955d9943e76c25df2d4105a6a94433d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Skapa och hantera Azure-databas för MySQL-servern med hjälp av Azure-portalen
Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-server och hantera servern med hjälp av Azure-portalen. Server management innehåller visa serverinformation och databaser, återställa lösenord och ta bort servern.

## <a name="log-in-to-the-azure-portal"></a>Logga in på Azure Portal
Logga in på [Azure-portalen](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Skapa en Azure Database för MySQL-server
Följ dessa steg om du vill skapa en Azure-databas för MySQL-server med namnet ”mysqlserver4demo”

1-Klicka **ny** knapp hittades i det övre vänstra hörnet i Azure-portalen.

2 – Välj **databaser** ny sida och välj **Azure-databas för MySQL** från sidan databaser.

> En Azure-databas för MySQL-server har skapats med en definierad uppsättning [beräkning och lagring](./concepts-compute-unit-and-storage.md) resurser. Databasen har skapats i en Azure-resursgrupp och i Azure-databasen för MySQL-servern.

![Skapa nya-server](./media/howto-create-manage-server-portal/create-new-server.png)

3 - fylla i Azure-databasen för MySQL formulär med följande information:

| **Formulärfält** | **Fältbeskrivning** |
|----------------|-----------------------|
| *Servernamn* | Azure-mysql (servernamnet är globalt unik) |
| *Prenumeration* | MySQLaaS (Välj listrutan) |
| *Resursgrupp* | MyResource (skapa en ny resursgrupp eller använda en befintlig) |
| *Inloggning för serveradministratör* | myadmin (konfigurera namn på administratörskonto) |
| *Lösenord* | kontolösenord för installationsprogrammet admin |
| *Bekräfta lösenord* | bekräfta lösenord för administratörskonto |
| *Plats* | Norra Europa (Välj mellan Norra Europa och västra USA) |
| *Version* | 5.6 (Välj Azure-databas för MySQL server-version) |

4-Klicka **prisnivå** att ange tjänstenivå för tjänstnivå och prestandanivå för den nya servern. Compute-enhet kan konfigureras mellan 50 och 100 i grundläggande nivån, 100 och 200 i standardnivån, och lagringsutrymme kan läggas till baserat på inkluderade belopp. Den här guiden Ta väljer vi 50 beräknings-enhet och 50GB. Klicka på **OK** att spara ditt val.
![Skapa-server--prisnivån](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5-Klicka **skapa** att etablera servern. Etableringen tar några minuter.

> Markera alternativet **Fäst på instrumentpanelen** för att enkelt kunna spåra dina distributioner.
> [!NOTE]
> Även om kommer du stöd för lagring, upp till 1 000 GB i grundläggande nivån och 10000 i standardnivån för Public Preview är maximalt lagringsutrymme fortfarande begränsat till 1 000 GB tillfälligt. 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Uppdatera en Azure-databas för MySQL-server
När nya servern är etablerad användare har 2 alternativ för att redigera en befintlig server: återställa administratörslösenord eller skala upp eller ned servern genom att ändra beräknings-enheter.

### <a name="change-the-administrator-user-password"></a>Ändra administratörslösenordet för användaren
1 - på servern **översikt** bladet, klickar du på **Återställ lösenord** att fylla i ett lösenord indata och bekräftelsen fönster.

2 - Ange nya lösenord och bekräfta lösenordet i fönstret enligt nedan: ![Återställ lösenord](./media/howto-create-manage-server-portal/reset-password.png)

3-Klicka **OK** att spara det nya lösenordet.

### <a name="scale-updown-by-changing-compute-units"></a>Skala upp eller ned genom att ändra Compute-enheter

1 - på server-bladet under **inställningar**, klickar du på **prisnivå** att öppna bladet nivå priser för Azure-databas för MySQL-servern.

2-Följ steg 4 i **skapa en Azure-databas för MySQL server** ändra Compute enheter i samma prisnivå.

## <a name="delete-an-azure-database-for-mysql-server"></a>Ta bort en Azure-databas för MySQL-server

1 - på servern **översikt** bladet, klickar du på **ta bort** kommandot för att öppna bladet om du tar bort bekräftelse.

2 - Skriv rätt servernamn i indata i bladet för dubbla bekräftelse.

3-Klicka **ta bort** knappen igen för att bekräfta åtgärden tar bort och vänta tills ”ta bort lyckades” popup på meddelandefältet.

## <a name="list-the-azure-database-for-mysql-databases"></a>Visa en lista med Azure-databas för MySQL-databaser
På servern **översikt** bladet Bläddra nedåt tills du ser ikonen längst ned i databasen. Alla databaser visas i tabellen. Klicka på **ta bort** kommandot för att öppna bladet om du tar bort bekräftelse.

![Visa-databaser](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Visa detaljer för en Azure-databas för MySQL-server
Klicka på **egenskaper** under **inställningar** på servern öppnas bladet den **egenskaper** bladet. Sedan kan du visa alla detaljerad information om servern.

## <a name="next-steps"></a>Nästa steg

[Snabbstart: Skapa Azure-databas för MySQL-server med hjälp av Azure portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
