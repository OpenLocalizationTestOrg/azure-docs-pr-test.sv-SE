---
title: aaaUse hello SharePoint Server-anslutningen i dina Logic Apps | Microsoft Docs
description: "Komma igång med hello hello SharePoint Server-anslutningen i dina Logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3b814f42611e4971ff5c94ae3b021829217911dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-connector"></a>Kom igång med hello SharePoint-koppling
hello SharePoint Connector ger ett sätt toowork med listor i SharePoint.

Kom igång genom att skapa en logikapp; Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-toosharepoint"></a>Skapa en anslutning tooSharePoint
toouse Hej SharePoint Connector, du först skapa en **anslutning** ange hello information för dessa egenskaper: 

| Egenskap | Krävs | Beskrivning |
| --- | --- | --- |
| Token |Ja |Ange autentiseringsuppgifter för SharePoint |

tooconnect för**SharePoint**, ange din identitet (användarnamn och lösenord, autentiseringsuppgifter för smartkort, etc.) tooSharePoint. När du har autentiserats, kan du fortsätta toouse hello SharePoint connector i din logikapp. 

Följ dessa steg toosign i SharePoint toocreate hello-anslutning när på hello designer av din logikapp, **anslutning** för användning i din logikapp:

1. Ange SharePoint i sökrutan hello och vänta tills hello Sök tooreturn alla poster med SharePoint i hello namn:   
   ![Konfigurera SharePoint][1]  
2. Välj **SharePoint - när en fil har skapats**   
3. Välj **logga in tooSharePoint**:   
   ![Konfigurera SharePoint][2]    
4. Ge din SharePoint-autentiseringsuppgifter toosign i tooauthenticate SharePoint   
   ![Konfigurera SharePoint][3]     
5. När hello autentisering har slutförts kommer du att omdirigerade tooyour logik app toocomplete den genom att konfigurera SharePoint- **när en fil skapas** dialogrutan.          
   ![Konfigurera SharePoint][4]  
6. Du kan sedan lägga till andra utlösare och åtgärder som du behöver toocomplete din logikapp.   
7. Spara ditt arbete genom att välja **spara** hello menyraden ovan.  

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sharepoint/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
