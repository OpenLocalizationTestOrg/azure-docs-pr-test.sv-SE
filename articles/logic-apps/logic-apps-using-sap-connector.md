---
title: aaaConnect tooan lokal SAP-system i Azure Logic Apps | Microsoft Docs
description: "Ansluta tooan lokal SAP system från logik app-arbetsflödes hello lokala datagateway"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 594ec5fed337398bf931d396684630ee9f907d2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-on-premises-sap-system-from-logic-apps-with-hello-sap-connector"></a>Ansluta tooan lokal SAP system från logikappar med hello SAP connector 

hello lokala datagateway kan du toomanage data och åtkomst till säker resurser som finns lokalt. Det här avsnittet visar hur du kan ansluta logic apps tooan lokal SAP-systemet. I det här exemplet logikappen begär en IDOC över HTTP och skickar tillbaka hello-svar.    

> [!NOTE]
> Aktuella begränsningar: 
> - Din logikapp timeout om alla steg som krävs för hello svar inte slutförs inom hello [tidsgränsen för begäran](./logic-apps-limits-and-config.md). I det här scenariot kan begäranden blockeras. 
> - hello filväljaren visar inte alla tillgängliga hello-fält. I det här scenariot kan du manuellt lägga till sökvägar.

## <a name="prerequisites"></a>Krav

- Installera och konfigurera hello senaste [lokala datagateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 eller senare. [Hur tooconnect toohello lokala datagateway i en logikapp](http://aka.ms/logicapps-gateway) visar hello steg. hello gateway måste installeras på en lokal dator innan du kan fortsätta.

- Hämta och installera hello senaste SAP klientbiblioteket på hello datorn samma där du installerade hello datagateway. Använd någon av följande SAP versioner hello: 
    - SAP-Server
        - En SAP-Server som stöd hello .NET Connector (NCo) 3.0
 
    - SAP-klient
        - SAP .NET Connector (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-tooyour-sap-system"></a>Lägg till utlösare och åtgärder för att ansluta tooyour SAP-system

hello SAP-koppling har åtgärder, men inte utlösare. Har vi toouse en annan utlösare hello början av hello arbetsflöde. 

1. Lägga till hello begäran och svar utlösare och välj sedan **nytt steg**.

2. Välj **lägga till en åtgärd**, och välj sedan hello SAP-koppling genom att skriva `SAP` i sökfältet hello:    

     ![Välj SAP-programserver eller SAP Message Server](media/logic-apps-using-sap-connector/sap-action.png)

3. Välj [ **SAP-programserver** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) eller [ **SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), baserat på din SAP-konfiguration. Om du inte har en befintlig anslutning, kan du ange toocreate en.

   1. Välj **Anslut via lokala datagateway**, och ange hello information för SAP-system:   

       ![Lägg till anslutning sträng tooSAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. Under **Gateway**, Välj en befintlig gateway eller tooinstall en ny gateway, Välj **installera gatewayen**.

        ![Installera en ny gateway](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. När du har angett alla hello information, Välj **skapa**. 
   Logic Apps konfigurerar och testar hello-anslutning, se till att hello anslutning fungerar korrekt.

4. Ange ett namn för din SAP-anslutning.

5. hello olika SAP-alternativ är nu tillgängliga. toofind IDOC kategori, Välj hello-listan. Eller manuellt ange i hello sökväg och välj hello HTTP-svar i hello **brödtext** fält:

     ![SAP-åtgärd](media/logic-apps-using-sap-connector/picture3.png)

6. Lägg till hello åtgärd för att skapa en **HTTP-svar**. hello-svarsmeddelandet ska från hello SAP-utdata.

7. Spara din logikapp. Testa den genom att skicka en IDOC via hello HTTP utlösaren URL. Vänta tills hello svar från hello logikapp efter hello IDOC skickas:   

     > [!TIP]
     > Kolla hur för[övervaka dina Logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Nu när hello SAP-anslutningen läggs tooyour logikapp kan du börja utforska andra funktioner:

- BAPI
- RFC

## <a name="get-help"></a>Få hjälp

tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

- Lär dig hur toovalidate, transformering och andra BizTalk-liknande funktioner i hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Ansluta tooon lokala data](../logic-apps/logic-apps-gateway-connection.md) från logikappar
