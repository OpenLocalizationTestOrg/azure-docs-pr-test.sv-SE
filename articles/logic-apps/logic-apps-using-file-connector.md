---
title: "aaaConnect tooon lokala filsystem från Azure Logic Apps | Microsoft Docs"
description: "Ansluta tooon lokala filsystem från logik app arbetsflödet via hello lokala datagateway och filsystemet connector"
keywords: Filsystem
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: LADocs; deli
ms.openlocfilehash: beb5565293def4aba81f63f19e77d7498aac38c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-file-systems-from-logic-apps-with-hello-file-system-connector"></a>Ansluta tooon lokala filsystem från logikappar med hello filsystemet connector

Molnet hybridanslutning är central toologic appar, så toomanage data och säker åtkomst till lokala resurser, dina logic apps kan använda hello lokala datagateway. I den här artikeln visar vi hur tooconnect tooan lokalt filsystem med ett enkelt scenario: kopiera en fil som överförda tooDropbox tooa filresurs och sedan skicka ett e-postmeddelande.

## <a name="prerequisites"></a>Krav

- Installera och konfigurera hello senaste [lokala datagateway](https://www.microsoft.com/download/details.aspx?id=53127).
- Installera hello senaste lokala datagateway, version 1.15.6150.1 eller senare. [Ansluta toohello lokala datagateway](http://aka.ms/logicapps-gateway) visar hello steg. hello gateway måste installeras på en lokal dator innan du kan fortsätta med hello resten av hello steg.

## <a name="add-trigger-and-actions-for-connecting-tooyour-file-system"></a>Lägg till utlösare och åtgärder för att ansluta tooyour filsystem

1. Skapa en logikapp och lägga till den här Dropbox-utlösare: **när en fil skapas** 
2. Välj under hello utlösaren **nästa steg** > **lägga till en åtgärd**. 
3. Skriv i sökrutan hello `file system` så att du kan visa alla stöds åtgärder för hello filsystemet connector.

   ![Sök efter filen connector](media/logic-apps-using-file-connector/search-file-connector.png)

2. Välj hello **skapa fil** åtgärd, och skapa en anslutning tooyour filsystem.

   Om du inte har en befintlig anslutning, kan du ange toocreate en.

   1. Välj **Anslut via lokala datagateway**. Fler egenskaper visas.
   2. Välj din rotmapp för filsystemet.
      
       > [!NOTE]
       > hello rotmappen är hello huvudsakliga överordnade mappen som används för relativa sökvägar för alla fil-relaterade åtgärder. Du kan ange en lokal mapp på hello datorn där hello lokala datagateway har installerats eller hello-mappen kan vara en nätverksresurs som hello datorn kan komma åt.

   3. Ange hello användarnamn och lösenord för hello gateway.
   4. Välj hello-gateway som du tidigare har installerat.

       ![Konfigurera anslutning](media/logic-apps-using-file-connector/create-file.png)

3. När du har angett alla hello information, Välj **skapa**. 

   Logic Apps konfigurerar och testar anslutningen, se till att hello anslutning fungerar korrekt. 
   Om hello anslutningen har konfigurerats på rätt sätt finns alternativ för hello-åtgärd som du valde tidigare. 
   hello filen system koppling är nu redo för användning.

4. Ange att du vill toocopy filer från Dropbox toohello rotmappen för din lokala filresurs.

   ![Skapa filen åtgärd](media/logic-apps-using-file-connector/create-file-filled.png)

5. Lägga till en Outlook-åtgärd som skickar ett e-postmeddelande så relevanta användarna vet om hello ny fil efter din logik app kopior hello-fil. Ange hello mottagare, titel och brödtext hello e-post. 

   I hello dynamiskt innehåll selector du utdata från hello filen anslutningen så att du kan lägga till mer information om toohello e-post.

   ![Skicka e-åtgärd](media/logic-apps-using-file-connector/send-email.png)

6. Spara din logikapp. Testa din app genom att överföra en fil tooDropbox. hello-fil får kopieras toohello lokalt filresursen och du får ett e-postmeddelande om hello-åtgärd.

   > [!TIP] 
   > Lär dig hur för[övervaka dina logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Grattis, du har nu en fungerande logikappen som kan ansluta tooyour lokala filsystem. Försök att utforska andra funktioner som hello connector erbjudanden, till exempel:

- Skapa fil
- Listar filer i mappen
- Lägg till fil
- Ta bort fil
- Hämta innehåll
- Hämta filinnehåll med hjälp av sökväg
- Hämta filens metadata
- Hämta metadata för fil med hjälp av sökväg
- Listar filer i rotmappen
- Uppdatera-filen

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/fileconnector/). 

## <a name="get-help"></a>Få hjälp

tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

- [Ansluta tooon lokala data](../logic-apps/logic-apps-gateway-connection.md) från logikappar
- Lär dig mer om [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)
