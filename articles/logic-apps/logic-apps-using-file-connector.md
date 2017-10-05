---
title: "Ansluta till lokalt filsystem från Azure Logic Apps | Microsoft Docs"
description: "Ansluta till lokalt filsystem från logik app arbetsflödet via lokala datagateway och filsystemet connector"
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
ms.openlocfilehash: f33e7c58103c57e17e4e273caba1ab9b83f0cd2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Ansluta till lokalt filsystem från logikappar med filsystemet connector

Hybrid är cloud centrala för logic apps så för att hantera data och säker åtkomst till lokala resurser, dina logic apps kan använda lokala datagateway. I den här artikeln visar vi hur du ansluter till ett lokalt filsystem med ett enkelt scenario: kopiera en fil som överförs till Dropbox till en filresurs och sedan skicka ett e-postmeddelande.

## <a name="prerequisites"></a>Krav

- Installera och konfigurera senast [lokala datagateway](https://www.microsoft.com/download/details.aspx?id=53127).
- Installera den senaste version av lokala datagatewayen, version 1.15.6150.1 eller senare. [Ansluta till lokala datagateway](http://aka.ms/logicapps-gateway) innehåller stegen. Gatewayen måste installeras på en lokal dator innan du fortsätter med resten av stegen.

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Lägga till utlösare och åtgärder för att ansluta till filsystemet

1. Skapa en logikapp och lägga till den här Dropbox-utlösare: **när en fil skapas** 
2. Välj under utlösare, **nästa steg** > **lägga till en åtgärd**. 
3. I sökrutan anger `file system` så att du kan visa alla stöds åtgärder för kopplingen filsystem.

   ![Sök efter filen connector](media/logic-apps-using-file-connector/search-file-connector.png)

2. Välj den **skapa fil** åtgärd, och skapa en anslutning till filsystemet.

   Om du inte har en befintlig anslutning, uppmanas du att skapa en.

   1. Välj **Anslut via lokala datagateway**. Fler egenskaper visas.
   2. Välj din rotmapp för filsystemet.
      
       > [!NOTE]
       > Rotmappen är den viktigaste överordnade mappen som används för relativa sökvägar för alla fil-relaterade åtgärder. Du kan ange en lokal mapp på datorn där lokala datagateway har installerats eller mappen kan vara en nätverksresurs som datorn kan komma åt.

   3. Ange användarnamn och lösenord för gatewayen.
   4. Välj den gateway som du tidigare har installerat.

       ![Konfigurera anslutning](media/logic-apps-using-file-connector/create-file.png)

3. När du har angett alla detaljer väljer **skapa**. 

   Logic Apps konfigurerar och testar anslutningen, se till att anslutningen inte fungerar korrekt. 
   Om anslutningen har konfigurerats på rätt sätt finns alternativ för den åtgärd som du valde tidigare. 
   Filen system koppling är nu redo för användning.

4. Ange att du vill kopiera filer från Dropbox till rotmappen för din lokala filresurs.

   ![Skapa filen åtgärd](media/logic-apps-using-file-connector/create-file-filled.png)

5. När logikappen kopieras filen, kan du lägga till en Outlook-åtgärd som skickar ett e-postmeddelande så relevanta användarna vet om den nya filen. Ange mottagare, titel och brödtexten i e-postmeddelandet. 

   I den dynamiska innehåll selector du utdata från filen-anslutningen så att du kan lägga till mer information i e-postmeddelandet.

   ![Skicka e-åtgärd](media/logic-apps-using-file-connector/send-email.png)

6. Spara din logikapp. Testa din app genom att överföra en fil till Dropbox. Filen ska kopieras till filresursen lokalt och du får ett e-postmeddelande om åtgärden.

   > [!TIP] 
   > Lär dig hur du [övervaka dina logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Grattis, du har nu en fungerande logikapp som kan ansluta till det lokala filsystemet. Försök att utforska andra funktioner som kopplingen erbjuder:

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

## <a name="view-the-swagger"></a>Visa swagger
Finns det [swagger information](/connectors/fileconnector/). 

## <a name="get-help"></a>Få hjälp

I [Azure Logic Apps-forumet](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) kan du ställa frågor, besvara frågor och se vad andra Azure Logic Apps-användare håller på med.

På [webbplatsen för Azure Logic Apps-användarfeedback](http://aka.ms/logicapps-wish) kan du hjälpa till med att förbättra Azure Logic Apps och anslutningsapparna genom att rösta på förslag eller komma med egna förslag på förbättringar.

## <a name="next-steps"></a>Nästa steg

- [Ansluta till lokala data](../logic-apps/logic-apps-gateway-connection.md) från logikappar
- Lär dig mer om [enterprise integration](../logic-apps/logic-apps-enterprise-integration-overview.md)
