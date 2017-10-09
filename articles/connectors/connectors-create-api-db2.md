---
title: aaaAdd hello DB2-koppling i dina Logic Apps | Microsoft Docs
description: "Översikt över DB2-koppling med REST API-parametrar"
services: 
documentationcenter: 
author: gplarsen
manager: erikre
editor: 
tags: connectors
ms.assetid: 1c6b010c-beee-496d-943a-a99e168c99aa
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/26/2016
ms.author: plarsen; ladocs
ms.openlocfilehash: d836c61231d3c9cfdb30ff9c40a48b1718f3ffb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-db2-connector"></a>Kom igång med hello DB2-koppling
Microsoft-anslutaren för DB2 ansluter Logic Apps tooresources lagras i en IBM DB2-databas. Denna koppling inkluderar en Microsoft client toocommunicate med fjärråtkomst DB2 server-datorer i ett TCP/IP-nätverk. Detta omfattar molntjänster databaser, till exempel IBM Bluemix dashDB eller IBM DB2 för Windows körs i Azure virtualisering och lokala databaser med hjälp av hello lokala datagateway. Se hello [stöds listan](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2-plattformar och versioner (i det här avsnittet).

hello DB2-koppling har stöd för följande databasåtgärder hello:

* Lista databastabeller
* Läs en rad med väljer
* Läsa alla rader med väljer
* Lägga till en rad med INSERT
* Ändra en rad med hjälp av UPDATE
* Ta bort en rad med DELETE

Det här avsnittet visar hur toouse hello kopplingen i en logik app tooprocess databasen åtgärder.

toolearn mer om Logic Apps finns [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="available-actions"></a>Tillgängliga åtgärder
hello DB2-koppling stöder hello följande logik app åtgärder:

* GetTables
* GetRow
* GetRows
* InsertRow
* UpdateRow
* DeleteRow

## <a name="list-tables"></a>Lista tabeller
Skapa en logikapp för någon åtgärd består av många steg utförs via hello Microsoft Azure-portalen.

Inom hello logikapp kan du lägga till en åtgärd toolist tabeller i en DB2-databas. hello åtgärd instruerar hello connector tooprocess en DB2 schema-instruktionen, som `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `Db2getTables`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och sedan ange hello **Intervall** tootype **7**.  
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger `db2` i hello **Sök efter fler åtgärder** redigera och välj sedan **DB2 - Get-tabeller (förhandsgranskning)**.
   
   ![](./media/connectors-create-api-db2/Db2connectorActions.png)  
6. I hello **DB2 - Get-tabeller** configuration-fönstret, Välj **kryssrutan** tooenable **Anslut via lokala datagateway**. Observera att ändra hello inställningar från molnet tooon lokala.
   
   * Ange värde för **Server**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `ibmserver01:50000`.
   * Ange värde för **databasen**. Skriv till exempel `nwind`.
   * Välj värde för **autentisering**. Välj exempelvis **grundläggande**.
   * Ange värde för **användarnamn**. Skriv till exempel `db2admin`.
   * Ange värde för **lösenord**. Skriv till exempel `Password1`.
   * Välj värde för **Gateway**. Välj exempelvis **datagateway01**.
7. Välj **skapa**, och välj sedan **spara**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)
8. I hello **Db2getTables** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
9. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_tables**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla en lista över tabeller.
   
   ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-hello-connections"></a>Skapa hello-anslutningar
Den här anslutningen stöder anslutningar toodatabases finns lokalt och i molnet hello med hello följande anslutningsegenskaper. 

| Egenskap | Beskrivning |
| --- | --- |
| server |Krävs. Accepterar ett strängvärde som representerar ett TCP/IP-adress eller ett alias i IPv4- eller IPv6-format, följt (-semikolonavgränsad) av ett TCP/IP-portnummer. |
| Databasen |Krävs. Accepterar ett strängvärde som representerar en DRDA relationell databas namn (RDBNAM). DB2 för z/OS accepterar en sträng med 16 byte (databas kallas en IBM DB2 för z/OS-plats). DB2 för i5/OS accepterar en 18 byte-sträng (databas kallas en IBM DB2 för i relationell databas). DB2 för LUW accepterar en 8 byte-sträng. |
| Autentisering |Valfri. Tillåter lista objekt, grundläggande eller Windows (kerberos). |
| användarnamn |Krävs. Accepterar ett strängvärde. DB2 för z/OS accepterar en 8 byte-sträng. DB2 för i accepterar en 10 byte-sträng. DB2 för Linux eller UNIX accepterar en 8 byte-sträng. DB2 för Windows accepterar en 30-byte-sträng. |
| lösenord |Krävs. Accepterar ett strängvärde. |
| Gateway |Krävs. Accepterar ett lista objekt-värde som representerar hello lokala data gateway har definierats tooLogic appar i hello lagringsgruppen. |

## <a name="create-hello-on-premises-gateway-connection"></a>Skapa hello lokala gateway-anslutningen
Den här anslutningen kan komma åt en lokala DB2-databas med hello lokala gateway. Se gateway avsnitt för mer information. 

1. I hello **Gateways** configuration-fönstret, Välj **kryssrutan** tooenable **Anslut via gateway**. Observera att ändra hello inställningar från molnet tooon lokala.
2. Ange värde för **Server**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `ibmserver01:50000`.
3. Ange värde för **databasen**. Skriv till exempel `nwind`.
4. Välj värde för **autentisering**. Välj exempelvis **grundläggande**.
5. Ange värde för **användarnamn**. Skriv till exempel `db2admin`.
6. Ange värde för **lösenord**. Skriv till exempel `Password1`.
7. Välj värde för **Gateway**. Välj exempelvis **datagateway01**.
8. Välj **skapa** toocontinue. 
   
    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-hello-cloud-connection"></a>Skapa hello molnet anslutning
Den här anslutningen kan komma åt en molnet DB2-databas. 

1. I hello **Gateways** configuration rutan, lämna hello **kryssrutan** (oanvända) har inaktiverats **Anslut via gateway**. 
2. Ange värde för **anslutningsnamn**. Skriv till exempel `hisdemo2`.
3. Ange värde för **DB2 servernamn**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `hisdemo2.cloudapp.net:50000`.
4. Ange värde för **DB2 databasnamnet**. Skriv till exempel `nwind`.
5. Ange värde för **användarnamn**. Skriv till exempel `db2admin`.
6. Ange värde för **lösenord**. Skriv till exempel `Password1`.
7. Välj **skapa** toocontinue. 
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Hämta alla rader med väljer
Du kan definiera en logik app åtgärd toofetch alla rader i en DB2-tabell. Detta instruerar hello connector tooprocess en DB2 SELECT-instruktion som `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `Db2getRows`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger `db2` i hello **Sök efter fler åtgärder** redigera och välj sedan **DB2 - Get-rader (förhandsgranskning)**.
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**.
7. I hello **anslutningar** configuration-fönstret, Välj **Skapa nytt**. 
   
    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
8. I hello **Gateways** configuration rutan, lämna hello **kryssrutan** (oanvända) har inaktiverats **Anslut via gateway**.
   
   * Ange värde för **anslutningsnamn**. Skriv till exempel `HISDEMO2`.
   * Ange värde för **DB2 servernamn**, i hello form av adress eller alias kolon portnummer. Skriv till exempel `HISDEMO2.cloudapp.net:50000`.
   * Ange värde för **DB2 databasnamnet**. Skriv till exempel `NWIND`.
   * Ange värde för **användarnamn**. Skriv till exempel `db2admin`.
   * Ange värde för **lösenord**. Skriv till exempel `Password1`.
9. Välj **skapa** toocontinue.
   
    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)
10. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
11. Alternativt, Välj **visa avancerade alternativ** toospecify frågealternativ.
12. Välj **Spara**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)
13. I hello **Db2getRows** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
14. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla en lista med rader.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Lägga till en rad med INSERT
Du kan definiera en logik app åtgärd tooadd en rad i en DB2-tabell. Den här åtgärden instruerar hello connector tooprocess en DB2 INSERT-instruktion som `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `Db2insertRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **db2** i hello **Sök efter fler åtgärder** redigera och välj sedan **DB2 - infogningsraden (förhandsgranskning)**.
6. I hello **DB2 - infogningsraden (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** configuration rutan, Välj en anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**, typen `Area 99999`, och skriv `102` för **REGIONID**. 
10. Välj **Spara**.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
11. I hello **Db2insertRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
12. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla hello ny rad.
    
    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Hämta en rad med väljer
Du kan definiera en logik app åtgärd toofetch en rad i en DB2-tabell. Den här åtgärden som instruerar hello connector tooprocess instruktionen DB2 väljer där `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn** (t.ex.) ”**Db2getRow**”), **prenumeration**, **resursgruppen**, **plats**, och **Apptjänstplan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista. 
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **db2** i hello **Sök efter fler åtgärder** redigera och välj sedan **DB2 - Get-rader (förhandsgranskning)**.
6. I hello **hämta rader (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** konfigurationer rutan, Välj en befintlig anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**. 
10. Alternativt, Välj **visa avancerade alternativ** toospecify frågealternativ.
11. Välj **Spara**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)
12. I hello **Db2getRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
13. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla rad.
    
    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Ändra en rad med hjälp av UPDATE
Du kan definiera en logik app åtgärd toochange en rad i en DB2-tabell. Den här åtgärden instruerar hello connector tooprocess en DB2 UPDATE-instruktion som `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `Db2updateRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista.
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** anger **db2** i hello **Sök efter fler åtgärder** redigera och välj sedan **DB2 - raden uppdateringen (förhandsversion)**.
6. I hello **DB2 - raden uppdateringen (förhandsversion)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** konfigurationer fönstret Välj tooselect en befintlig anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**, typen `Updated 99999`, och skriv `102` för **REGIONID**. 
10. Välj **Spara**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)
11. I hello **Db2updateRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
12. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla hello ny rad.
    
    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Ta bort en rad med DELETE
Du kan definiera en logik app åtgärd tooremove en rad i en DB2-tabell. Den här åtgärden instruerar hello connector tooprocess en DB2 DELETE-instruktion som `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Skapa en logikapp
1. I hello **Azure startar board**väljer  **+**  (plustecknet) **webb + mobilt**, och sedan **Logikapp**.
2. Ange hello **namn**, som `Db2deleteRow`, **prenumeration**, **resursgruppen**, **plats**, och **App Tjänsten Plan**. Välj **PIN-kod toodashboard**, och välj sedan **skapa**.

### <a name="add-a-trigger-and-action"></a>Lägg till en utlösare och åtgärd
1. I hello **Logic Apps Designer**väljer **tom LogicApp** i hello **mallar** lista. 
2. I hello **utlösare** väljer **återkommande**. 
3. I hello **återkommande** utlösning, Välj **redigera**väljer **frekvens** listrutan tooselect **dag**, och välj sedan  **Intervallet** tootype **7**. 
4. Välj hello **+ nytt steg** och välj sedan **lägga till en åtgärd**.
5. I hello **åtgärder** väljer **db2** i hello **Sök efter fler åtgärder** redigera och välj sedan **DB2 - ta bort rad (förhandsgranskning)**.
6. I hello **DB2 - ta bort rad (förhandsgranskning)** åtgärd, Välj **ändra anslutningen**. 
7. I hello **anslutningar** konfigurationer rutan, Välj en befintlig anslutning. Välj exempelvis **hisdemo2**.
   
    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)
8. I hello **tabellnamn** listan, Välj hello **NEDPIL**, och välj sedan **området**.
9. Ange värden för alla obligatoriska kolumner (se röd asterisk). Skriv till exempel `99999` för **AREAID**. 
10. Välj **Spara**. 
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)
11. I hello **Db2deleteRow** bladet inom hello **alla körningar** listan **sammanfattning**, Välj hello första listas objekt (senaste kör).
12. I hello **logikapp kör** bladet väljer **kör information**. Inom hello **åtgärd** väljer **Get_rows**. Se hello värde för **Status**, som borde vara **lyckades**. Välj hello **indata länken** tooview hello indata. Välj hello **utdata länken**, och visa hello matar ut; som ska innehålla hello bort raden.
    
    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="supported-db2-platforms-and-versions"></a>DB2-plattformar som stöds och versioner
Den här anslutningen har stöd för följande IBM DB2-plattformar och versioner samt IBM DB2 kompatibla produkter (t.ex. IBM Bluemix dashDB) som har stöd för distribuerade relationella Database Architecture (DRDA) SQL åtkomst Manager (SQLAM) version 10 och 11 hello:

* IBM DB2 för z/OS 11,1
* IBM DB2 för z/OS 10.1
* IBM DB2 för i 7.3
* IBM DB2 för i 7.2
* IBM DB2 för i 7.1
* IBM DB2 för LUW 11
* IBM DB2 för LUW 10.5

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/db2/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md). Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).

