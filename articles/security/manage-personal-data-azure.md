---
title: aaaManage personliga data i Microsoft Azure | Microsoft Docs
description: information om hur toocorrect, uppdatera, ta bort och exportera personliga data i Azure Active Directory och Azure SQL Database
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 032f70d32377cb9395cb2c35c27dad05001537c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a>Hantera personliga data i Microsoft Azure

Den här artikeln innehåller råd om hur toocorrect, uppdatera, ta bort och exportera personliga data i Azure Active Directory och Azure SQL Database.

## <a name="scenario"></a>Scenario

En Dublin-baserade företaget tillhandahåller en arbetsuppgift för avancerad mål bröllop Irland och hälsningsmeddelande för både en lokal och kundbas. De har kontor, kunder, anställda och leverantörer som finns runt hello world toofully service hello handelsplatser de erbjuder.

Hello företagets håller reda på svar som inkluderar Matallergier och dietetisk inställningar mellan många andra objekt. Bröllop gäster kan registrera för olika aktiviteter som horseback barnet, surfa, beredskapsbåten bilar, osv., och även interagera med varandra på en central webbsida under hello månader som leder fram toohello händelse. hello företagets samlar in personlig information från anställda, leverantörer, kunder och bröllop gäster. På grund av hello internationella natur hello business hello företag måste vara kompatibel med flera nivåer av förordning.

## <a name="problem-statement"></a>Problembeskrivning

- Data-administratörer måste vara kan toocorrect stämmer personlig information och uppdatera ofullständiga eller ändra personlig information.

- Behovet av data-administratörer måste vara kan toodelete personlig information på hello-begäran för en registrerade.

- Data administratörer behöver tooexport data och tillhandahålla tooa registrerade i ett vanligt, strukturerade format på sin begäran.

## <a name="company-goals"></a>Företagets mål

- Felaktiga eller ofullständiga kund, Bröllop gäst, medarbetare och personlig information om leverantören måste korrigeras eller uppdateras i Azure Active Directory och Azure SQL Database.

- Personlig information måste tas bort i Azure Active Directory och Azure SQL Database på hello-begäran för en registrerade.

- Personliga data måste exporteras i formatet vanliga, strukturerade på hello-begäran för en registrerade.

## <a name="solutions"></a>Lösningar

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Azure Active Directory: åtgärda/korrigera felaktiga eller ofullständiga personliga data och radera/ta bort personliga data/användarprofiler

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) är Microsofts molnbaserade, flera innehavare katalog och identity management-tjänsten.
Du kan korrigera, uppdatera eller ta bort kund- och användarprofiler och arbete användarinformation som innehåller dina personliga data, till exempel en användares namn, arbete rubrik, adress eller telefonnummer i din [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) miljö med hjälp av hello [Azure-portalen](https://portal.azure.com/).

Du måste logga in med ett konto som är en global administratör för hello-katalogen.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Hur jag korrigera eller uppdatera användarprofil och arbete information i Azure Active Directory?

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.

2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

    ![Media/image1.png](media/manage-personal-data-azure/image001.png)

3. På hello **användare och grupper** bladet väljer **användare**.

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. På hello **användare och grupper – användare** bladet Välj en användare hello listan, och på hello bladet för hello vald användare Markera **profil** tooview hello användarprofilsinformation som behöver rättas toobe eller uppdaterade.

    ![Media/image3.png](media/manage-personal-data-azure/image005.png)

5. Korrigera eller uppdatera hello information och markera i kommandofältet hello **spara.**

6.  På hello bladet för valda hello-användaren väljer **fungerar Info** tooview arbete användarinformation som behöver toobe åtgärdat eller uppdateras.

    ![Media/image4.png](media/manage-personal-data-azure/image007.png)

7. Korrigera eller uppdatera hello användarinformation för arbete och markera i kommandofältet hello **spara.**

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Hur tar jag bort en användarprofil i Azure Active Directory?

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.

2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

    ![](media/manage-personal-data-azure/image001.png)

3. På hello **användare och grupper** bladet väljer **användare**.

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. På hello **användare och grupper – användare** bladet Välj en användare hello listan.

    ![Media/image3.png](media/manage-personal-data-azure/image007.png)

5. På hello bladet för valda hello-användaren väljer **översikt**, och välj sedan i kommandofältet hello **ta bort**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>SQL Database: åtgärda/korrigera felaktiga eller ofullständiga personuppgifter. Radera/ta bort personuppgifter. Exportera personliga data 

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) är en databas i molnet som gör att utvecklare kan skapa och hantera program.

Personliga data uppdateras i [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) med standard SQL-frågor och kan också tas bort. Dessutom kan personuppgifter exporteras från SQL-databas med olika metoder, inklusive hello Azure SQL Server-import och export-guiden och i olika format, inklusive en BACPAC-fil.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Hur jag korrigera, uppdatera eller ta bort personliga data i SQL-databas?

toolearn hur toocorrect eller uppdatera personliga data i SQL-databas finns hello [uppdatering (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [uppdatera Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [uppdatera med vanliga tabelluttryck](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), eller [ Uppdatera skriva Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentation.

toolearn hur toodelete personliga data i SQL-databas finns hello [ta bort (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentation.

#### <a name="how-do-i-export-personal-data-tooa-bacpac-file-in-sql-database"></a>Hur exporterar personuppgifter tooa BACPAC filen i SQL-databasen?

En BACPAC-fil innehåller hello SQL-databas data och metadata och är en zip-fil med filnamnstillägget BACPAC. Detta kan göras med hjälp av hello [Azure-portalen](https://portal.azure.com/), hello SQLPackage kommandoradsverktyg, SQL Server Management Studio (SSMS) eller PowerShell.

toolearn hur tooexport tooa BACPAC datafilen, besök hello [exportera en Azure SQL database tooa BACPAC fil](https://docs.microsoft.com/azure/sql-database/sql-database-export) som innehåller detaljerade anvisningar för varje metod som anges ovan.

Hur exporterar personliga data från SQL-databas med hello SQL Server importera och exportera guiden?

Den här guiden hjälper dig att kopiera data från en källa tooa mål. För en introduktion toohello guiden, inklusive hur tooget den behörighetsinformation och hur tooget med hello-verktyget finns hello [importera och exportera Data med hello SQL Server importera och exportera guiden](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) webbsida.

En översikt över stegen för hello guiden finns hello [steg i hello SQL Server importera och exportera guiden](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) webbsida.

## <a name="next-steps"></a>Nästa steg:

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

