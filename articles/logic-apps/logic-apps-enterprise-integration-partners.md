---
title: "aaaCreate partners för business-to-business (B2B) meddelanden - Azure Logic Apps | Microsoft Docs"
description: "Lär dig hur tooadd partners tooyour integrering konto med hello Enterprise-Integrationspaket och Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8dc70a8f441fcf228ed178029dcdbac940d794b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Lägg till eller uppdatera partner i business-to-business-avtalen i arbetsflödet

Partners är enheter som deltar i business-to-business (B2B) transaktioner och utbyte av meddelanden mellan varandra. Innan du kan skapa partners som representerar du och en annan organisation i dessa transaktioner, måste du både dela information som identifierar och validerar meddelanden som skickas av varandra. När du diskutera dessa uppgifter och är redo toostart business-relation, kan du skapa partners i din integrering konto toorepresent du båda.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Vilka roller har partners i integration kontot?

toodefine information om hello-meddelanden som utbyts mellan partner kan du skapa avtal mellan dessa partner. Men innan du kan skapa ett avtal, du måste ha lagt till minst två parter tooyour integrering konto. Din organisation måste vara en del av hello avtal som hello **värden partner**. Hej andra partnern eller **gäst partner** representerar hello organisation som utbyter meddelanden med din organisation. hello gäst partner kan vara ett annat företag eller även en avdelning i din organisation.

När du lägger till dessa tillverkare, skapar du ett avtal.

Ta emot och skicka inställningarna går från hello synsätt av hello Hosted Partner. Hello får till exempel inställningar i ett avtal avgöra hur hello finns partner tar emot meddelanden som skickas från en gäst-partner. På samma sätt anger hello skicka inställningarna på hello avtal hur hello finns kontopartner skickar meddelanden toohello gäst partner.

## <a name="how-toocreate-a-partner"></a>Hur toocreate en partner?

1. Välj i hello Azure-portalen, **Bläddra**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Skriv i sökrutan för hello filter **integrering**och välj **Integrationskonton** i hello resultatlistan.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Välj hello integration konto där du vill tooadd dina partners.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Välj hello **Partners** panelen.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Hello Partners bladet välj **Lägg till**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Ange ett namn för din partner och välj sedan en **kvalificerare**. Du kan använda en **värdet** toohelp identifiera dokument som finns i dina appar.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. toosee hello förloppet för ditt partner-processen, Välj hello *bell* meddelandeikonen.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. tooconfirm som din nya partner har lagts till, Välj hello **Partners** panelen.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    När du har valt hello Partners panelen visas också nytillagda partners i hello Partners bladet.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-tooedit-existing-partners-in-your-integration-account"></a>Hur tooedit befintliga partners i ditt konto för integrering

1. Välj hello **Partners** panelen.
2. När hello Partners blad öppnas, Välj hello-partner som du vill tooedit.
3. På hello **uppdatering Partner** panelen, gör ändringarna.
4. När du är klar väljer **spara**, eller toocancel ändringarna, Välj **Ignorera**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-toodelete-a-partner"></a>Hur toodelete en partner

1. Välj hello **Partners** panelen.
2. Välj hello-partner som du vill toodelete efter hello Partner blad öppnas.
3. Välj **ta bort**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Nästa steg
* [Mer information om avtalen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  

