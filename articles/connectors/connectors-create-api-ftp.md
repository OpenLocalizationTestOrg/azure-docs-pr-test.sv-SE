---
title: aaaLearn hur toouse hello FTP-anslutningen i logikappar | Microsoft Docs
description: "Skapa logikappar med Azure App service. Ansluta tooFTP server toomanage dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i FTP-servern."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a7020df2005ebb34fc569627ae0516b8528cc7a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-ftp-connector"></a>Kom igång med hello FTP-anslutningen
Använd hello FTP-anslutningen toomonitor, hantera och skapa filer på en FTP-server. 

toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp. Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooftp"></a>Ansluta tooFTP
Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service. En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.  

### <a name="create-a-connection-tooftp"></a>Skapa en anslutning tooFTP
> [!INCLUDE [Steps toocreate a connection tooFTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Använda en FTP-utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!IMPORTANT]
> hello FTP-anslutningen kräver en FTP-server som kan nås från hello Internet och är konfigurerad toooperate med PASSIVT läge. Hello FTP-anslutningen är också **inte kompatibel med implicit FTPS (FTP över SSL)**. hello FTP-anslutningen har endast stöd för explicit FTPS (FTP över SSL).  
> 
> 

I det här exemplet ska jag visa hur toouse hello **FTP - när en fil har lagts till eller ändrats** utlöser tooinitiate en logik app arbetsflödet när en fil har lagts till eller ändrats på en FTP-server. Du kan använda den här utlösaren toomonitor en FTP-mapp för nya filer som representerar order från kunder i ett enterprise-exempel.  Du kan sedan använda en åtgärd för FTP-anslutningen som **hämta filinnehåll** tooget hello innehållet i hello ordning för vidare bearbetning och lagring i order-databas.

1. Ange *ftp* i hello sökrutan på hello logic apps designer väljer du sedan hello **FTP - när en fil har lagts till eller ändrats** utlösare   
   ![FTP-utlösarbild 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   Hej **när en fil har lagts till eller ändrats** kontrollen öppnas  
   ![Bild 2 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Välj hello **...**  finns på hello höger sida av hello kontroll. Då öppnas hello mappen väljarkontrollen  
   ![Bild 3 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Välj hello  **>**  (HÖGERPIL) och bläddra toofind hello mappen som du vill toomonitor för nya eller ändrade filer. Välj hello mapp och notera hello mapp visas nu i hello **mappen** kontroll.  
   ![Bild 4 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Din logikapp har nu konfigurerats med en utlösare som börjar en körning av hello andra utlösare och åtgärder i hello arbetsflödet när en fil ändras eller skapas i hello viss FTP-mapp. 

> [!NOTE]
> Det måste innehålla minst en utlösare och en åtgärd för en logik app toobe funktionella. Hello åtgärderna i hello nästa avsnitt tooadd en åtgärd.  
> 
> 

## <a name="use-a-ftp-action"></a>Använda en FTP-åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

Nu när du har lagt till en utlösare, följer du dessa steg tooadd en åtgärd som kommer att få hello innehållet i hello nya eller ändrade filen som hittas med hello utlösare.    

1. Välj **+ nytt steg** tooadd hello hello åtgärd tooget hello innehållet i hello fil på hello FTP-server  
2. Välj hello **lägga till en åtgärd** länk.  
   ![Bild 1 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Ange *FTP* toosearch för alla åtgärder relaterade tooFTP.
4. Välj **FTP - hämta filinnehåll** som hello åtgärd tootake när en ny eller ändrad fil hittas i hello FTP-mappen.      
   ![Bild 2 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-2.png)  
   Hej **hämta filinnehåll** styra öppnas. **Obs**: du kommer att tillfrågas tooauthorize din logik app tooaccess FTP-servern konto om du inte har gjort det tidigare.  
   ![Bild 3 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Välj hello **filen** kontrollen (hello tomt utrymme finns under **filen***). Här kan kan du använda någon av hello olika egenskaper från hello nya eller ändrade filen som hittas på hello FTP-servern.  
6. Välj hello **filen innehåll** alternativet.  
   ![Bild 4 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. hello kontroll uppdateras, som anger att hello **FTP - hämta filinnehåll** åtgärd får hello *filen innehåll* hello nya eller ändrade filen på hello FTP-servern.      
   ![Bild 5 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Spara ditt arbete och lägger till en fil toohello FTP-mappen tootest arbetsflödet.    

Nu hello logikapp har konfigurerats med en utlösare toomonitor en mapp på en FTP-servern och initiera hello arbetsflödet när den hittar en ny fil eller en fil på hello FTP-servern. 

Hej logikapp har också konfigurerats med en åtgärd tooget hello innehållet i hello nya eller ändrade filen.

Nu kan du lägga till en annan åtgärd som hello [SQL Server - infogningsraden](connectors-create-api-sqlazure.md) åtgärd tooinsert hello innehållet i hello nya eller ändrade filen till en SQL-databastabell.  

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md)

