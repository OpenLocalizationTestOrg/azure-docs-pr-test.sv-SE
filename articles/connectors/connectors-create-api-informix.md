---
title: aaaAdd hello Informix connector i dina Logic Apps | Microsoft Docs
description: "Översikt över Informix koppling med REST API-parametrar"
services: 
documentationcenter: 
author: gplarsen
manager: anneta
editor: 
tags: connectors
ms.assetid: ca2393f0-3073-4dc2-8438-747f5bc59689
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: 7a163e2ebf00fa3109b93e34845d922c2174a48d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-informix-connector"></a>Kom igång med hello Informix-koppling
Microsoft-anslutaren för Informix ansluter Logic Apps tooresources lagras i en IBM Informix-databas. hello Informix koppling inkluderar en Microsoft client toocommunicate tooremote Informix server-datorer i ett TCP/IP-nätverk. Detta omfattar molntjänster databaser, till exempel IBM Informix för Windows körs i Azure virtualisering och lokala databaser med hjälp av hello lokala datagateway. Se hello [stöds listan](connectors-create-api-informix.md#supported-informix-platforms-and-versions) IBM Informix-plattformar och versioner (i det här avsnittet).

hello connector stöder hello följande åtgärder:

* Lista databastabeller
* Läs en rad med väljer
* Läsa alla rader med väljer
* Lägga till en rad med INSERT
* Ändra en rad med hjälp av UPDATE
* Ta bort en rad med DELETE

Det här avsnittet visar hur toouse hello kopplingen i en logik app tooprocess databasen åtgärder.

toolearn mer om Logic Apps finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Tillgängliga åtgärder
Den här anslutningen stöder hello följande logik app åtgärder:

* Getables
* GetRow
* GetRows
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Lista tabeller
Skapa en logikapp för någon åtgärd består av många steg utförs via hello Microsoft Azure-portalen.

Inom hello logikapp kan du lägga till en åtgärd toolist tabeller i en Informix-databas. Den här åtgärden instruerar hello connector tooprocess en Informix schema-instruktionen som `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `InformixgetTables`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**.  
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **informix** i hello **Sök efter fler åtgärder** redigera och välj sedan **Informix - Get-tabeller (förhandsgranskning)**.
   
   ![](./media/connectors-create-api-informix/InformixconnectorActions.png)  
6. I hello **Informix - Get-tabeller** configuration-fönstret, Välj **kryssrutan** tooenable **Anslut via lokala datagateway**. Observera att ändra hello inställningar från molnet tooon lokala.
   
   * Ange värde för **Server**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `ibmserver01:9089`.
   * Ange värde för **databasen**. Skriv till exempel `nwind`.
   * Välj värde för **autentisering**. Välj exempelvis **grundläggande**.
   * Ange värde för **användarnamn**. Skriv till exempel `informix`.
   * Ange värde för **lösenord**. Skriv till exempel `Password1`.
   * Välj värde för **Gateway**. Välj exempelvis **datagateway01**.
7. Välj **skapa**, och välj sedan **spara**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)
8. I hello **InformixgetTables** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
9. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_tables**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla en lista över tabeller.
   
   ![](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

## <a name="create-hello-connections"></a>Skapa hello-anslutningar
Den här anslutningen stöder anslutningar toodatabase lokalt och i molnet hello med hello följande anslutningsegenskaper. 

| Egenskap | Beskrivning |
| --- | --- |
| server |Krävs. Accepterar ett strängvärde som representerar en TCP/IP-adress eller ett alias i IPv4- eller IPv6-format, följt (-semikolonavgränsad) av ett TCP/IP-portnummer. |
| Databasen |Krävs. Accepterar ett strängvärde som representerar en DRDA relationell databas namn (RDBNAM). Informix accepterar en 128 byte-sträng (databas kallas en IBM Informix databasnamnet (dbname)). |
| Autentisering |Valfri. Tillåter lista objekt, grundläggande eller Windows (kerberos). |
| användarnamn |Krävs. Accepterar ett strängvärde. |
| lösenord |Krävs. Accepterar ett strängvärde. |
| Gateway |Krävs. Accepterar ett lista objekt-värde som representerar hello lokala data gateway har definierats tooLogic appar i hello lagringsgruppen. |

## <a name="create-hello-on-premises-gateway-connection"></a>Skapa hello lokala gateway-anslutningen
Den här anslutningen har åtkomst till en lokal Informix-databas med hjälp av hello lokala datagateway. Se gateway avsnitt för mer information. 

1. I hello **Gateways** configuration-fönstret, Välj **kryssrutan** tooenable **Anslut via gateway**. Se hello inställningar ändras från molnet tooon lokala.
2. Ange värde för **Server**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `ibmserver01:9089`.
3. Ange värde för **databasen**. Skriv till exempel `nwind`.
4. Välj värde för **autentisering**. Välj exempelvis **grundläggande**.
5. Ange värde för **användarnamn**. Skriv till exempel `informix`.
6. Ange värde för **lösenord**. Skriv till exempel `Password1`.
7. Välj värde för **Gateway**. Välj exempelvis **datagateway01**.
8. Välj **skapa** toocontinue. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorOnPremisesDataGatewayConnection.png)

## <a name="create-hello-cloud-connection"></a>Skapa hello molnet anslutning
Den här anslutningen har åtkomst till ett moln Informix-databas. 

1. I hello **Gateways** configuration rutan, lämna hello **kryssrutan** (oanvända) har inaktiverats **Anslut via gateway**. 
2. Ange värde för **anslutningsnamn**. Skriv till exempel `hisdemo2`.
3. Ange värde för **Informix servernamn**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `hisdemo2.cloudapp.net:9089`.
4. Ange värde för **Informix databasnamnet**. Skriv till exempel `nwind`.
5. Ange värde för **användarnamn**. Skriv till exempel `informix`.
6. Ange värde för **lösenord**. Skriv till exempel `Password1`.
7. Välj **skapa** toocontinue. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Hämta alla rader med väljer
Du kan skapa en logik app åtgärd toofetch alla rader i hello Informix tabell. Den här åtgärden instruerar hello connector tooprocess ett Informix SELECT-uttryck som `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn** (t.ex.) ”**InformixgetRows**”), **prenumeration**, **resursgruppen**, **plats**, och **Apptjänstplan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **informix** i hello **Sök efter fler åtgärder** redigera och välj sedan **Informix - Get-rader (förhandsgranskning)** .
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**.
7. I hello **anslutningar** configuration-fönstret, Välj **Skapa nytt**. 
   
    ![](./media/connectors-create-api-informix/InformixconnectorNewConnection.png)
8. I hello **Gateways** configuration rutan, lämna hello **kryssrutan** (oanvända) har inaktiverats **Anslut via gateway**.
   
   * Ange värde för **anslutningsnamn**. Skriv till exempel `HISDEMO2`.
   * Ange värde för **Informix servernamn**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `HISDEMO2.cloudapp.net:9089`.
   * Ange värde för **Informix databasnamnet**. Skriv till exempel `NWIND`.
   * Ange värde för **användarnamn**. Skriv till exempel `informix`.
   * Ange värde för **lösenord**. Skriv till exempel `Password1`.
9. Välj **skapa** toocontinue.
   
    ![](./media/connectors-create-api-informix/InformixconnectorCloudConnection.png)
10. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
11. Alternativt, Välj **visa avancerade alternativ** toospecify frågealternativ.
12. Välj **Spara**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsTableName.png)
13. I hello **InformixgetRows** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
14. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla en lista med rader.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Lägga till en rad med INSERT
Du kan skapa en logik app åtgärd tooadd en rad i en Informix-tabell. Den här åtgärden instruerar hello connector tooprocess en Informix INSERT-instruktion som `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `InformixinsertRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **informix** i hello **Sök efter fler åtgärder** redigera och välj sedan **Informix - infogningsraden (förhandsgranskning)**.
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** configuration fönstret Välj tooselect en anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**, typen `Area 99999`, och skriv `102` för **REGIONID**. 
10. Välj **Spara**.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowValues.png)
11. I hello **InformixinsertRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
12. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla hello ny rad.
    
    ![](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Hämta en rad med väljer
Du kan skapa en logik app åtgärd toofetch en rad i en Informix-tabell. Den här åtgärden som instruerar hello connector tooprocess instruktionen Informix väljer där `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `InformixgetRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **informix** i hello **Sök efter fler åtgärder** redigera och välj sedan **Informix - Get-rader (förhandsgranskning)** .
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** konfigurationer fönstret Välj tooselect en befintlig anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**. 
10. Alternativt, Välj **visa avancerade alternativ** toospecify frågealternativ.
11. Välj **Spara**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowValues.png)
12. I hello **InformixgetRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
13. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla rad.
    
    ![](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Ändra en rad med hjälp av UPDATE
Du kan skapa en logik app åtgärd toochange en rad i en Informix-tabell. Den här åtgärden instruerar hello connector tooprocess en Informix UPDATE-instruktion som `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `InformixupdateRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **informix** i hello **Sök efter fler åtgärder** redigera och välj sedan **Informix - raden uppdateringen (förhandsversion)**.
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** konfigurationer fönstret Välj tooselect en befintlig anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**, typen `Updated 99999`, och skriv `102` för **REGIONID**. 
10. Välj **Spara**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowValues.png)
11. I hello **InformixupdateRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
12. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla hello ny rad.
    
    ![](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Ta bort en rad med DELETE
Du kan skapa en logik app åtgärd tooremove en rad i en Informix-tabell. Den här åtgärden instruerar hello connector tooprocess en Informix DELETE-instruktion som `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `InformixdeleteRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **informix** i hello **Sök efter fler åtgärder** redigera och välj sedan **Informix - ta bort rad (förhandsgranskning)**.
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** konfigurationer rutan, Välj en befintlig anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-informix/InformixconnectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**. 
10. Välj **Spara**. 
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowValues.png)
11. I hello **InformixdeleteRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
12. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla hello bort raden.
    
    ![](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="supported-informix-platforms-and-versions"></a>Informix-plattformar som stöds och versioner
Den här anslutningen stöder hello efter IBM Informix versioner när du konfigurerade toosupport distribuerade relationella Database Architecture (DRDA)-klientanslutningar.

* IBM Informix 12.1
* IBM Informix 11,7

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/informix/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).

