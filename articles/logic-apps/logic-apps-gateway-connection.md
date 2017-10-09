---
title: "aaaAccess datakällor lokalt för Logikappar i Azure | Microsoft Docs"
description: "Konfigurera hello lokala datagateway så att du kan komma åt datakällor lokalt från logikappar"
keywords: "komma åt data på lokala, dataöverföring, kryptering och datakällor"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 1d3deaac5a095316ce78e224dab0c08559bc2ff2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-hello-on-premises-data-gateway"></a>Komma åt datakällor lokalt från logikappar med hello lokala datagateway

tooaccess datakällor lokalt från dina logic apps ställa in en lokal datagateway logikappar kan använda med kopplingar som stöds. hello gateway fungerar som en brygga som ger snabb överföring och kryptering mellan datakällor lokalt och dina logic apps. hello gateway vidarebefordrar data från lokala datakällor på krypterade kanaler via hello Azure Service Bus. All trafik som kommer från som säkra utgående trafik från hello gateway agent. Lär dig mer om [hur hello datagateway fungerar](logic-apps-gateway-install.md#gateway-cloud-service). 

hello gateway har stöd för anslutningar toothese datakällor lokalt:

*   BizTalk Server 2016
*   DB2  
*   Filsystem
*   Informix
*   MQ
*   MySQL
*   Oracle-databas
*   PostgreSQL
*   SAP-programserver 
*   SAP Message Server
*   SharePoint
*   SQL Server
*   Teradata

Dessa steg visar hur tooset in hello lokala data gateway toowork med logic apps. Läs mer om kopplingar som stöds, [kopplingar för Azure Logikappar](../connectors/apis-list.md). 

Information om hur toouse hello gateway med andra tjänster finns i följande artiklar:

*   [Microsoft Power BI lokala datagateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services lokala datagateway](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow lokala datagateway](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps lokala datagateway](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a>Krav

* Du måste redan ha [installerat hello datagateway på en lokal dator](logic-apps-gateway-install.md).

* När du loggar in toohello Azure-portalen måste du ha toouse hello samma arbets- eller skolkonto som användes för[installera hello lokala datagateway](logic-apps-gateway-install.md#requirements). Din inloggning kontot måste också ha en Azure-prenumeration toouse när du skapar en gateway-resurs i hello Azure-portalen för gatewayinstallationen.

* Gatewayinstallationen kan inte redan begärts av en Azure gateway-resurs. Du kan associera din gateway installation tooonly en Azure gateway-resurs. Anspråk händer när du skapar hello gateway resurs så att hello installation är inte tillgängligt för andra resurser.

## <a name="set-up-hello-data-gateway-connection"></a>Ställ in hello data gateway-anslutningen

### <a name="1-install-hello-on-premises-data-gateway"></a>1. Installera hello lokala datagateway

Om du inte redan gjort följer hello [steg tooinstall hello lokala datagateway](logic-apps-gateway-install.md). Innan du fortsätter med hello andra steg, se till att du har installerat hello datagateway på en lokal dator.

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-hello-on-premises-data-gateway"></a>2. Skapa en Azure-resurs för hello lokala datagateway

När du har installerat hello gateway på en lokal dator måste du skapa din datagateway som en resurs i Azure. Det här steget associerar även din gateway-resurs med din Azure-prenumeration.

1. Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen"). Kontrollera att toouse hello samma Azure arbete eller skola e-postadress används tooinstall hello gateway.

2. Hello vänstra menyn i Azure och välj **ny** > **Enterprise Integration** > **lokala datagateway** som visas här:

   ![Sök efter ”lokala datagateway”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. På hello **skapa anslutningen gateway** bladet och ange följande information toocreate din data gateway-resurs:

    * **Namnet**: Ange ett namn för din gateway-resurs. 

    * **Prenumerationen**: Välj hello Azure-prenumeration tooassociate med gateway-resurs. 
    Den här prenumerationen ska vara hello samma prenumeration som din logikapp.
   
      Hej standardabonnemang baseras på hello Azure-konto som du använde toosign i.

    * **Resursgruppen**: skapa en resursgrupp eller välj en befintlig resursgrupp för att distribuera din gateway-resurs. 
    Resursgrupper kan du hantera relaterade Azure-resurser som en samling.

    * **Plats**: Azure begränsar den här platsen toohello samma region som valts för hello gateway-Molntjänsten under [gatewayinstallationen](logic-apps-gateway-install.md). 

      > [!NOTE]
      > Kontrollera att hello gateway resursplats matchar hello gateway cloud service plats. Annars kan gatewayinstallationen inte visas i hello installerade gateways listan för du tooselect i hello nästa steg.
      > 
      > Du kan använda olika regioner för din gateway-resurs och för din logikapp.

    * **Installationen namnet**: Välj hello-gateway som du tidigare har installerat om gatewayinstallationen av inte redan har markerats. 

    tooadd hello gateway resurs tooyour Azure instrumentpanelen, Välj **PIN-kod toodashboard**. 
    När du är klar väljer **skapa**.

    Exempel:

    ![Ange information toocreate lokala datagateway](./media/logic-apps-gateway-connection/createblade.png)

    toofind eller visa din datagateway när som helst hello Azure vänstra huvudmenyn gå för **fler tjänster** > **Enterprise Integration** > **lokala Data Gateways**.

    ![Gå för ”fler tjänster”, ”Enterprise Integration”, ”lokala Data Gateways”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-toohello-on-premises-data-gateway"></a>3. Ansluta logik app toohello lokala datagateway

Nu när du har skapat din data gateway-resurs och tillhörande din Azure-prenumeration med den här resursen, skapa en anslutning mellan logik appen och hello datagateway.

> [!NOTE]
> Din plats för gateway-anslutningen måste finnas i hello samma region som din logikapp, men du kan använda en datagateway som finns i en annan region.

1. Skapa eller öppna logikappen i logik App Designer i hello Azure-portalen.

2. Lägg till en koppling som har stöd för lokala anslutningar, t.ex. SQL Server.

3. Följande hello ordning som visas, Välj **Anslut via lokala datagateway**, ange ett unikt anslutningsnamn och hello nödvändig information och välj hello data gateway resurs som du vill toouse. När du är klar väljer **skapa**.

   > [!TIP]
   > Ett unikt anslutningsnamn hjälper dig att identifiera anslutningen senare, speciellt när du skapar flera anslutningar. Om tillämpligt, även innehålla hello kvalificerade domännamnet för ditt användarnamn. 

   ![Skapa anslutning mellan logik appen och data gateway](./media/logic-apps-gateway-connection/blankconnection.png)

Grattis, gateway-anslutningen är klar för din app toouse för logik.

## <a name="edit-your-gateway-connection-settings"></a>Redigera anslutningsinställningarna gateway

När du har skapat en gateway-anslutningen för din logikapp, kanske du vill toolater update hello-inställningarna för den specifika anslutningen.

1. toofind hello gateway-anslutningen:

   * Hello logik app bladet under **utvecklingsverktyg**väljer **API anslutningar**. 
   
     Hej **API anslutningar** visar alla API-anslutningar som är associerade med din logikapp, inklusive gateway-anslutningar.

     ![Gå tooyour logikapp, Välj ”API-anslutningar](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Hello Azure vänstra huvudmenyn går för **fler tjänster** > **webb- och Mobile Services** > **API anslutningar** för alla API-anslutningar inklusive gateway-anslutningar som är associerade med din Azure-prenumeration. 

   * Hello Azure vänstra huvudmenyn går för**alla resurser** för alla API-anslutningar, inklusive gateway-anslutningar som är associerade med din Azure-prenumeration.

2. Välj hello gateway-anslutningen som du vill att tooview eller redigera och välj **redigera API-anslutningen**.

   > [!TIP]
   > Om uppdateringarna inte gälla försök [stoppa och starta om Windows hello gateway-tjänsten](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a>Växla eller ta bort dina lokala data gateway-resurs

toocreate en annan gateway-resurs, associera din gateway med en annan resurs eller ta bort hello gateway resurs, kan du ta bort hello gatewayresursen utan att påverka hello gateway-installationen. 

1. Hello Azure vänstra huvudmenyn gå för**alla resurser**. 
2. Hitta och välj din data gateway-resurs.
3. Välj **lokala Data Gateway**, och välj hello resurs verktygsfältet **ta bort**.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Nästa steg

* [Skydda dina Logic Apps](./logic-apps-securing-a-logic-app.md)
* [Vanliga exempel och scenarier för logic apps](./logic-apps-examples-and-scenarios.md)
