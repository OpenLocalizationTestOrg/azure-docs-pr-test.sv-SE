---
title: "aaaAS2 meddelanden för B2B enterprise integration - Azure Logic Apps | Microsoft Docs"
description: "Exchange AS2-meddelanden för B2B enterprise integrering med Azure Logikappar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 23f9d74dd0ad807e9cdaedb320d60496cfef9bc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>Exchange AS2-meddelanden för enterprise-integrering med logic apps

Innan du kan utbyta AS2-meddelanden för Logikappar i Azure, måste du skapa ett AS2-avtal och spara avtalet i ditt konto för integrering. Här följer hello instruktioner för hur toocreate ett AS2-avtal.

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* En [integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md) som redan har definierat och som är associerade med din Azure-prenumeration
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som definieras i ditt konto för integrering och konfigurerad med hello AS2-kvalificerare under redan **Business identiteter**

> [!NOTE]
> När du skapar ett avtal måste hello innehållet i hello fil matcha hello avtalstyp.    

När du [skapa ett konto för integrering](../logic-apps/logic-apps-enterprise-integration-accounts.md) och [lägga till partners](logic-apps-enterprise-integration-partners.md), du kan skapa ett AS2-avtal genom att följa dessa steg.

## <a name="create-an-as2-agreement"></a>Skapa ett AS2-avtal

1.  Logga in toohello [Azure-portalen](http://portal.azure.com "Azure-portalen").  

2.  Välj hello vänstra menyn **fler tjänster**. Skriv i sökrutan hello **integrering** som filter. Markera i hello resultat **Integrationskonton**.

    > [!TIP]
    > Om du inte ser **fler tjänster**, du kanske tooexpand hello menyn först. Överst hello hello komprimerad-menyn, Välj **menyn Visa**.

    ![Fler tjänster, filtret på ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. I hello **Integrationskonton** bladet som öppnas väljer hello integration konto där du vill toocreate hello avtal.
Om du inte ser några integrationskonton [skapa en första](../logic-apps/logic-apps-enterprise-integration-accounts.md "om integrationskonton").  

    ![Välj hello integration konto där toocreate hello avtal](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Välj hello **avtal** panelen. Om du inte har ett avtal sida vid sida, Lägg till hello panelen först.

    ![Välj ikonen ”avtal”](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. I hello avtal bladet som öppnas väljer du **Lägg till**.

    ![Välj ”Lägg till”](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. Under **Lägg till**, ange en **namn** för ditt avtal. För **avtalstyp**väljer **AS2**. Välj hello **värden Partner**, **Värdidentiteten**, **gäst Partner**, och **gäst identitet** för ditt avtal.

    ![Ange avtalsuppgifter](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Egenskap | Beskrivning |
    | --- | --- |
    | Namn |Namnet på hello avtal |
    | Avtalstyp | Ska vara AS2 |
    | Värd-Partner |Ett avtal måste både värden och gästen partner. hello värden partner representerar hello organisation som konfigurerar hello avtal. |
    | Värdidentiteten |En identifierare för hello värden partner |
    | Gästen Partner |Ett avtal måste både värden och gästen partner. hello gäst partner representerar hello organisation som har att göra affärer med hello värden partner. |
    | Gästen identitet |En identifierare för hello gäst partner |
    | Ta emot inställningarna |Dessa egenskaper gäller tooall har tagits emot av ett avtal. |
    | Skicka inställningar |Dessa egenskaper gäller tooall meddelanden som skickas av ett avtal. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Konfigurera hur ditt avtal handtag mottagna meddelanden

Nu när du har angett hello avtal egenskaper kan konfigurera du hur detta avtal identifierar och hanterar inkommande meddelanden som tagits emot från din partner via det här avtalet.

1.  Under **Lägg till**väljer **tar emot inställningar**.
Konfigurera dessa egenskaper baserat på ditt avtal med hello-partner som utbyter meddelanden med dig. Egenskapen finns i hello tabellen i det här avsnittet.

    ![Konfigurera ”ta emot inställningar”](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. Alternativt kan du kan åsidosätta hello egenskaper för inkommande meddelanden genom att välja **åsidosätta meddelandeegenskaper**.

3. toorequire alla inkommande meddelanden toobe signerad, Välj **meddelandet måste vara signerade**. Från hello **certifikat** väljer du en befintlig [gäst partner offentliga certifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md) för att validera hello signaturen på hälsningsmeddelande. Eller skapa hello certifikat, om du inte har någon.

4.  toorequire alla inkommande meddelanden toobe krypteras, Välj **meddelandet ska krypteras**. Från hello **certifikat** väljer du en befintlig [värden partner privat certifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md) för att dekryptera inkommande meddelanden. Eller skapa hello certifikat, om du inte har någon.

5. toorequire meddelanden toobe komprimerade, Välj **meddelandet ska komprimeras**.

6. Välj toosend en synkron disposition meddelanden (MDN) för mottagna meddelanden **skicka MDN**.

7. toosend registrerat MDNs för mottagna meddelanden, Välj **Skicka signerat MDN**.

8. toosend asynkron MDNs för mottagna meddelanden, Välj **skicka asynkront MDN**.

9. När du är klar gör att toosave dina inställningar genom att välja **OK**.

Nu ditt avtal är klar toohandle inkommande valda meddelanden som uppfyller tooyour inställningar.

| Egenskap | Beskrivning |
| --- | --- |
| Åsidosätt meddelandeegenskaper |Anger egenskaper i mottagna meddelanden som kan åsidosättas. |
| Meddelandet ska vara signerat |Kräver meddelanden toobe har signerats digitalt. Konfigurera hello gäst partner offentliga certifikat för signaturverifiering.  |
| Meddelandet ska vara krypterat |Kräver meddelanden toobe krypteras. Icke-krypterade meddelanden avvisas. Konfigurera hello värden partner privata certifikat för att dekryptera hälsningsmeddelande.  |
| Meddelandet ska vara komprimerat |Kräver meddelanden toobe komprimeras. Icke-komprimerade meddelanden avvisas. |
| MDN Text |hello standard meddelandet disposition meddelande (MDN) toobe skickas toohello som skickade meddelandet. |
| Skicka MDN |Kräver MDNs toobe skickas. |
| Skicka signerat MDN |Kräver MDNs toobe signerade. |
| MIC algoritm |Välj hello algoritmen toouse för signering av meddelanden. |
| Skicka asynkront MDN | Kräver meddelanden toobe skickas asynkront. |
| URL: EN | Ange hello URL där toosend hello MDNs. |

## <a name="configure-how-your-agreement-sends-messages"></a>Konfigurera hur ditt avtal skickar meddelanden

Du kan konfigurera hur detta avtal identifierar och hanterar utgående meddelanden som du skickar tooyour partners via det här avtalet.

1.  Under **Lägg till**väljer **skicka inställningar**.
Konfigurera dessa egenskaper baserat på ditt avtal med hello-partner som utbyter meddelanden med dig. Egenskapen finns i hello tabellen i det här avsnittet.

    ![Ange egenskaper för hello ”skicka inställningar”](./media/logic-apps-enterprise-integration-agreements/agreement-51.png)

2. toosend signerade meddelanden tooyour partner, Välj **aktivera Meddelandesignering**. För att signera hälsningsmeddelande i hello **MIC algoritmen** listan, Välj hello *partner privata värdcertifikatet MIC algoritmen*. Och i hello **certifikat** väljer du en befintlig [värden partner privat certifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. toosend krypterade meddelanden toohello partner, Välj **aktivera meddelandekryptering**. För att kryptera hälsningsmeddelande i hello **krypteringsalgoritm** listan, Välj hello *gäst partner offentliga certifikat algoritmen*.
Och i hello **certifikat** väljer du en befintlig [gäst partner offentliga certifikat](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. toocompress hello-meddelande, Välj **aktivera komprimering av meddelandet**.

5. toounfold hello HTTP innehållstyp-sidhuvud till en enda rad väljer **vika HTTP-huvuden**.

6. tooreceive synkron MDNs för hello skickade meddelanden, Välj **begära MDN**.

7. tooreceive registrerat MDNs för hello skickade meddelanden, Välj **begäran signerade MDN**.

8. tooreceive asynkron MDNs för hello skickade meddelanden, Välj **begära asynkron MDN**. Om du väljer det här alternativet måste du ange hello URL för där toosend hello MDNs.

9. toorequire-oavvislighet för mottagandet, Välj **aktivera NRR**.  

10. toospecify algoritmen format toouse i hello MIC eller logga in hello utgående huvuden hello AS2-meddelande eller MDN, Välj **SHA2 algoritmen format**.  

11. När du är klar gör att toosave dina inställningar genom att välja **OK**.

Ditt avtal är nu redo toohandle utgående meddelanden som uppfyller tooyour valda inställningar.

| Egenskap | Beskrivning |
| --- | --- |
| Aktivera Meddelandesignering |Kräver att alla meddelanden som skickas från hello avtal toobe signerade. |
| MIC algoritm |hello algoritmen toouse för signering av meddelanden. Konfigurerar hello partner privata värdcertifikatet MIC algoritmen för signering hälsningsmeddelande. |
| Certifikat |Välj hello certifikat toouse för signering av meddelanden. Konfigurerar hello värden partner privata certifikat för signering hälsningsmeddelande. |
| Aktivera kryptering av meddelanden |Kräver kryptering av alla meddelanden som skickas från det här avtalet. Konfigurerar hello gäst partner offentliga certifikat algoritm för kryptering av hälsningsmeddelande. |
| Krypteringsalgoritm |Hej toouse för kryptering algoritm för kryptering av meddelanden. Konfigurerar hello gäst partner offentliga certifikat för kryptering hälsningsmeddelande. |
| Certifikat |certifikatet toouse tooencrypt hälsningsmeddelande. Konfigurerar hello gäst partner privata certifikat för kryptering hälsningsmeddelande. |
| Aktivera komprimering för meddelandet |Kräver komprimering av alla meddelanden som skickas från det här avtalet. |
| Vika HTTP-huvuden |Placerar hello HTTP-huvudet på en enda rad. |
| Begäran MDN |Kräver en MDN för alla meddelanden som skickas från det här avtalet. |
| Begäran signerade MDN |Kräver att alla MDNs som skickas toothis avtal toobe signerade. |
| Asynkron MDN för begäran |Kräver asynkron MDNs toobe skickas toothis avtal. |
| URL: EN |Ange hello URL där toosend hello MDNs. |
| Aktivera NRR |Kräver oavvislighet mottogs (NRR) attributet kommunikation som ger bevis hello data togs emot som åtgärdas. |
| SHA2 algoritmen format |Välj algoritm format toouse i hello MIC eller logga in hello utgående huvuden hello AS2-meddelande eller MDN |

## <a name="find-your-created-agreement"></a>Hitta din skapade avtal

1.  När du är klar med inställningen alla dina avtal egenskaper på hello **Lägg till** bladet välj **OK** toofinish skapar ditt avtal och returnerar tooyour integrering konto bladet.

    Ditt nya avtal nu visas i din **avtal** lista.

2.  Du kan också visa dina avtal i ditt Kontoöversikt för integrering. Välj på ditt kontoblad integration **översikt**och välj hello **avtal** panelen. 

    ![Välj ”avtal” panelen tooview alla avtal](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/as2/). 

## <a name="next-steps"></a>Nästa steg
* [Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
