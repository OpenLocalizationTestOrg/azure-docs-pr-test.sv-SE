---
title: "aaaCreate, länkar, ta bort eller flytta integration konton i Azure logikappar | Microsoft Docs"
description: "Hur toocreate en integration konto och länka det tooyour logikappar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: fda6c91723b3e3624ee176df112ba8b6c9800273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-integration-account"></a>Vad är en integrering konto?

Ett konto för integrering tillåter enterprise integration apparna toomanage artefakter, inklusive scheman, kartor, certifikat, partners och avtal. Alla integration appar som du skapar använder en integration konto tooaccess dessa scheman, kartor, certifikat och så vidare.

## <a name="create-an-integration-account"></a>Skapa ett konto för integrering

1.  Logga in toohello [Azure-portalen](http://portal.azure.com "Azure-portalen"). Välj hello vänstra menyn **fler tjänster**.

    ![Välj ”fler tjänster”](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. I sökrutan hello skriver du ”integration” för filtret. Markera i hello resultat **Integrationskonton**.

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Överst hello på hello sidan, Välj **Lägg till**.

    ![Välj Lägg till](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Namnge ditt konto för integrering och välj hello Azure-prenumeration som du vill toouse. Du kan antingen skapa en ny **resursgruppen** eller välj en befintlig resursgrupp. Välj en **plats** som värd för ditt konto för integrering och ett **prisnivån**. 

    När du är klar kan du välja **skapa**.

    ![Ange information för ditt konto för integrering](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure tillhandahåller kontot integration hello valt plats, vilket bör vara klart inom 1 minut.

5. Uppdatera hello-sidan. Du ser ditt nya konto för integrering i listan.

    ![Det nya kontot på integrering visas](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Därefter länka hello integration konto som du skapade tooyour logikapp. 

## <a name="link-an-integration-account-tooa-logic-app"></a>Länka en integration konto tooa logikapp

toogive dina logic apps komma åt toomaps, scheman, avtal och andra artefakter i kontot integration länken hello integration konto tooyour logikapp.

### <a name="prerequisites"></a>Krav

* Ett konto för integrering
* En logikapp

> [!NOTE] 
> Kontrollera appen integration konto och logik i hello *samma Azure-plats* innan du börjar.


1. Välj din logikapp i hello Azure-portalen och kontrollera platsen för din logikapp.

    ![Välj din logikapp, Kontrollera platsen](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Under **inställningar**väljer **integrering konto**.

    ![Välj ”Integration konto”](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Från hello **välja ett konto för integrering** listan, Välj hello integration konto som du vill toolink tooyour logikapp. toofinish länkar, Välj **spara**.

    ![Välj kontot integrering](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Du får ett meddelande som visar din integrering kontot är länkade tooyour logikapp och att alla artefakter i ditt konto för integrering är nu tillgängliga tooyour logikapp.

    ![Din logikapp länkas tooyour integrering konto](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Nu när ditt konto integration är länkade tooyour logikapp, kan du använda hello B2B-kopplingar i dina logic apps. Några vanliga B2B-kopplingar innehåller XML-verifiering och koda/avkoda flat-fil.  

## <a name="delete-your-integration-account"></a>Ta bort ditt konto för integrering

1. Välj **fler tjänster**.

    ![Välj ”fler tjänster”](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. I sökrutan hello skriver du ”integration” för filtret. Markera i hello resultat **Integrationskonton**.

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Välj hello integration konto som du vill toodelete.

    ![Välj integration konto toodelete](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. Välj på menyn hello **ta bort**.

    ![Välj ”Ta bort”](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. Bekräfta ditt val toodelete hello integration konto.

## <a name="move-your-integration-account"></a>Flytta integration-kontot

toomove en integration konto tooanother Azure-prenumerationen eller resursen grupp, Följ dessa steg.

> [!IMPORTANT]
> När du flyttar ett integration konto måste du uppdatera alla skript toouse hello ny resurs-ID.

1. Välj **fler tjänster**.

    ![Välj ”fler tjänster”](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. I sökrutan hello skriver du ”integration” för filtret. Markera i hello resultat **Integrationskonton**.

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Välj hello integration konto som du vill toomove. Under **inställningar**, Välj **egenskaper**.

    ![Välj integration konto toomove. Under inställningar, välj Egenskaper](./media/logic-apps-enterprise-integration-accounts/move.png)

5. Ändra hello resursgrupp eller Azure-prenumeration som är kopplad till ditt konto för integrering.

    ![Välj Ändra resursgrupp eller ändra prenumerationen](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Nästa steg
* [Mer information om avtalen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  

