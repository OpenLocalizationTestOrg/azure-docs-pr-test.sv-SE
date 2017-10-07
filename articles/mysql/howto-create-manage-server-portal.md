---
title: "aaaCreate och hantera Azure-databas för MySQL-servern med hjälp av Azure portal | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-servern och hanterar hello servern hello Azure-portalen."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c532df43b3d2124d7bd34875b32d52357f162af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Skapa och hantera Azure-databas för MySQL-servern med hjälp av Azure-portalen
Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-servern och hanterar hello servern hello Azure-portalen. Server management innehåller visa serverinformation och databaser, återställa lösenord och ta bort hello-server.

## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen
Logga in toohello [Azure-portalen](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Skapa en Azure Database för MySQL-server
Följ dessa steg toocreate en Azure-databas för MySQL-server med namnet ”mysqlserver4demo”

1-Klicka **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2 – Välj **databaser** hello ny sida och välj **Azure-databas för MySQL** från hello databaser sida.

> En Azure-databas för MySQL-server har skapats med en definierad uppsättning [beräkning och lagring](./concepts-compute-unit-and-storage.md) resurser. hello-databasen har skapats i en Azure-resursgrupp och i Azure-databasen för MySQL-servern.

![Skapa nya-server](./media/howto-create-manage-server-portal/create-new-server.png)

3 - fylla hello Azure-databas för MySQL formulär med hello följande information:

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

4-Klicka **prisnivå** toospecify hello tjänstnivå och prestandanivå servicenivå för den nya servern. Compute-enhet kan konfigureras mellan 50 och 100 i grundläggande nivån, 100 och 200 i standardnivån, och lagringsutrymme kan läggas till baserat på inkluderade belopp. Den här guiden Ta väljer vi 50 beräknings-enhet och 50GB. Klicka på **OK** toosave valet.
![Skapa-server--prisnivån](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5-Klicka **skapa** tooprovision hello server. Etableringen tar några minuter.

> Kontrollera hello **PIN-kod toodashboard** alternativet tooallow enkel spårning av dina distributioner.
> [!NOTE]
> Även om too1000GB i grundläggande nivån och 10000GB i Standard nivå ha stöd för lagring, för Public Preview är hello maximalt lagringsutrymme fortfarande begränsad too1000GB tillfälligt. 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Uppdatera en Azure-databas för MySQL-server
När nya servern är etablerad användare har 2 alternativ tooedit en befintlig server: återställa administratörslösenord eller skala upp eller ned hello server genom att ändra hello beräknings-enheter.

### <a name="change-hello-administrator-user-password"></a>Ändra Hej administratör användarens lösenord
1 - på hello server **översikt** bladet, klickar du på **Återställ lösenord** toopopulate ett lösenord indata och bekräftelsen fönster.

2 - Ange nytt lösenord och bekräfta hello lösenord i hello fönster enligt nedan: ![Återställ lösenord](./media/howto-create-manage-server-portal/reset-password.png)

3-Klicka **OK** toosave hello nytt lösenord.

### <a name="scale-updown-by-changing-compute-units"></a>Skala upp eller ned genom att ändra Compute-enheter

1 - på hello serverblad under **inställningar**, klickar du på **prisnivå** tooopen hello priser nivå bladet för hello Azure-databas för MySQL-servern.

2-Följ steg 4 i **skapa en Azure-databas för MySQL server** toochange Compute enheter i hello samma prisnivån.

## <a name="delete-an-azure-database-for-mysql-server"></a>Ta bort en Azure-databas för MySQL-server

1 - på hello server **översikt** bladet, klickar du på **ta bort** kommando knappen tooopen hello raderas bekräftelse bladet.

2-typen hello rätt servernamn i textrutan för hello bladet dubbla bekräftelse.

3-Klicka **ta bort** knappen igen tooconfirm bort åtgärd och vänta tills ”ta bort lyckades” popup på hello meddelandefältet.

## <a name="list-hello-azure-database-for-mysql-databases"></a>Lista hello Azure-databas för MySQL-databaser
På hello server **översikt** bladet Bläddra nedåt tills du ser hello databasen panelen på hello längst ned. Alla hello databaser visas i hello tabell. Klicka på **ta bort** kommando knappen tooopen hello raderas bekräftelse bladet.

![Visa-databaser](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Visa detaljer för en Azure-databas för MySQL-server
Klicka på **egenskaper** under **inställningar** på hello servern öppnas bladet hello **egenskaper** bladet. Sedan kan du visa alla detaljerad information om hello-server.

## <a name="next-steps"></a>Nästa steg

[Snabbstart: Skapa Azure-databas för MySQL-server med hjälp av Azure portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
