---
title: 'Azure-portalen: SQL-databas dynamisk datamaskning | Microsoft Docs'
description: "Hur tooget igång med SQL Database dynamisk datamaskering i hello Azure-portalen"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 5c5f74682a15d42eceb78c3ca0705401e1ac0ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-hello-azure-portal"></a>Kom igång med SQL Database dynamisk datamaskering med hello Azure-portalen

Det här avsnittet beskrivs hur du tooimplement [dynamisk datamaskning](sql-database-dynamic-data-masking-get-started.md) med hello Azure-portalen. Du kan också implementeras dynamiska datamaskering med [Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) eller hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-hello-azure-portal"></a>Konfigurera dynamisk datamaskning för databasen med hello Azure-portalen
1. Starta hello Azure-portalen på [https://portal.azure.com](https://portal.azure.com).
2. Navigera toohello inställningsbladet för hello-databas som innehåller hello känsliga data som du vill toomask.
3. Klicka på hello **dynamisk Datamaskering** panelen som startar hello **dynamisk Datamaskering** configuration-bladet.
   
   * Du kan också bläddra ned toohello **Operations** avsnittet och klicka på **dynamisk Datamaskering**.
     
     ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. I hello **dynamisk Datamaskering** configuration bladet kan du se vissa databaskolumner som hello rekommendationer motorn har flaggats för maskering. Order tooaccept hello rekommendationer, klicka bara på **Lägg till Mask** för en eller flera kolumner och en mask skapas baserat på hello standardtypen för den här kolumnen. Du kan ändra hello maskering funktionen genom att klicka på hello maskningsregel och redigera hello maskering fältet format tooa annat valfritt format. Vara säker på att tooclick **spara** toosave dina inställningar.
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. tooadd en mask för alla kolumner i databasen, hello överst i hello **dynamisk Datamaskering** configuration bladet och klickar på **Lägg till Mask** tooopen hello **Lägg till regel för maskering** konfiguration av bladet
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Välj hello **schemat**, **tabell** och **kolumnen** toodefine hello anges fält som ska maskeras.
7. Välj en **maskering cellformatet** hello listan över känsliga data maskering kategorier.
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Klicka på **spara** i hello datamaskning bladet tooupdate hello regeluppsättning av maskering regler i hello dynamisk datamaskering princip.
9. Typen hello SQL-användare eller AAD identiteter som ska uteslutas från maskering och ha åtkomst toohello avmaskerad känsliga data. Detta bör vara en semikolonavgränsad lista med användare. Observera att användare med administratörsbehörighet alltid har åtkomst till toohello ursprungliga omaskerat data.
   
    ![Navigeringsfönstret](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > toomake det så hello programnivå kan visa känsliga data för privilegierad användare, lägga till hello SQL-användaren eller AAD-identitet hello program använder tooquery hello databas. Vi rekommenderar starkt att den här listan innehåller ett minimalt antal Privilegierade användare toominimize exponering av hello känsliga data.
   > 
   > 
10. Klicka på **spara** i hello datamaskning bladet toosave hello nya eller uppdaterade maskering Konfigurationsprincip.


## <a name="next-steps"></a>Nästa steg

* En översikt över dynamisk datamaskning finns [dynamisk datamaskning](sql-database-dynamic-data-masking-get-started.md).
* Du kan också implementeras dynamiska datamaskering med [Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) eller hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).
