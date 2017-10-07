---
title: "aaaSchemas för XML-verifiering - Azure Logic Apps | Microsoft Docs"
description: "Validera XML-dokument med scheman för Logikappar i Azure och Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 87cf92741e10ff7cccd260f27442909e34928903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-hello-enterprise-integration-pack"></a>Validera XML med scheman för Logikappar i Azure och hello Enterprise-Integrationspaket

Scheman bekräfta att hello XML-dokument som du får är giltiga och har hello förväntade data i ett fördefinierat format. Scheman kan också hjälpa Validera meddelanden som utbyts i ett B2B-scenario.

## <a name="add-a-schema"></a>Lägger till ett schema

1. Välj i hello Azure-portalen, **fler tjänster**.

    ![Azure-portalen ”fler tjänster”](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Skriv i sökrutan för hello filter **integrering**, och välj **Integrationskonton** från hello resultatlistan.

    ![Filtrera sökrutan](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Välj hello **integrering konto** där du vill att tooadd hello schemat.

    ![Lista över integrationskonton](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Välj hello **scheman** panelen.

    ![Exempel integration kontot, ”scheman”](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Lägga till en schemafil som är mindre än 2 MB

1. I hello **scheman** bladet som öppnas (från hello som föregående steg IT-avdelning), väljer **Lägg till**.

    ![Scheman bladet ”Lägg till”](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Ange ett namn för schemat. Överför hello schemafilen genom att välja hello mappen ikonen nästa toohello **schemat** rutan. När hello överföringen är klar väljer du **OK**.

    ![Skärmbild av ”Lägg till schemat” med ”liten fil” markerat](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-too8-mb-maximum"></a>Lägga till en schemafil som är större än 2 MB (upp too8 MB maximalt)

De här stegen variera beroende på åtkomstnivå för hello blob-behållare: **offentliga** eller **ingen anonym åtkomst**.

**toodetermine på den här åtkomstnivån**

1.  Öppna **Azure Lagringsutforskaren**. 

2.  Under **Blobbbehållare**väljer hello blob-behållare som du vill använda. 

3.  Välj **säkerhet**, **åtkomstnivå**.

Om hello blob säkerhetsåtkomstnivå **offentliga**, Följ dessa steg.

![Azure Lagringsutforskaren med ”Blob-behållare”, ”säkerhet” och ”offentliga” markerat](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Överför hello schemat tooyour storage-konto och kopiera hello URI.

    ![Storage-konto med URI markerat](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. I **Lägg till Schema**väljer **stora filer**, och ange hello URI i hello **innehåll URI** textruta.

    ![Scheman med knappen ”Lägg till” och ”stora filer” markerat](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Om hello blob säkerhetsåtkomstnivå **ingen anonym åtkomst**, Följ dessa steg.

![Azure Lagringsutforskaren med ”Blob-behållare”, ”säkerhet” och ”ingen anonym åtkomst” markerat](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Överför hello schemat tooyour storage-konto.

    ![Lagringskonto](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Generera en signatur för delad åtkomst för hello-schemat.

    ![Storage-konto med delad åtkomst signaturer fliken markerat](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. I **Lägg till Schema**väljer **stora filer**, och ange hello delad åtkomstsignatur URI i hello **innehåll URI** textruta.

    ![Scheman med knappen ”Lägg till” och ”stora filer” markerat](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. I hello **scheman** bladet för ditt konto integration nytillagda schemat ska visas.

    ![Kontot integrering med ”scheman” och hello nya schemat markerat](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Redigera scheman

1. Välj hello **scheman** panelen.

2. Efter hello **scheman** blad öppnas, Välj hello schema som du vill tooedit.

3. På hello **scheman** bladet välj **redigera**.

    ![Scheman bladet](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Välj hello schemafilen som du vill tooedit och sedan välja **öppna**.

    ![Öppna schemat filen tooedit](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Azure visas ett meddelande som hello schema har överförts.

## <a name="delete-schemas"></a>Ta bort scheman

1. Välj hello **scheman** panelen.

2. Efter hello **scheman** blad öppnas väljer hello schema som du vill använda toodelete.

3. På hello **scheman** bladet välj **ta bort**.

    ![Scheman bladet](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. tooconfirm som du vill toodelete hello valts schemat, Välj **Ja**.

    ![Schemat bekräftelse-meddelande](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    I hello **scheman** bladet hello schemat listan uppdateras och innehåller inte längre hello-schema som du har tagit bort.

    ![Din integrering konto med ”scheman” markerat](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om hello enterprise-integrationspaket").  

