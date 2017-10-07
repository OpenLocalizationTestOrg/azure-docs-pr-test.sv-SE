---
title: aaaOverview Logic Apps kopplingar | Microsoft Docs
description: "Översikt över kopplingar som kan användas i en logikapp"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: ca8dab2e-9b69-4b1e-865d-1facd9f0cdac
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan
ms.openlocfilehash: dc4cac4c0dfaa2f9fd218ffc04414fa8e52a91d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-in-a-logic-app"></a>Med hjälp av anslutningar i en logikapp
Kopplingar ger snabb åtkomst tooevents, data och åtgärder för alla tjänster, protokoll och plattformar.  hello fullständig lista över kopplingar som har stöd för Logic Apps kan [finns här](apis-list.md).  Kopplingar kan användas som en utlösare eller en åtgärd i en logikapp och kan kräva en konfigurerad *anslutning* toouse (till exempel: Auktorisera en Twitter konto tooaccess eller efter åt dig).

## <a name="basics"></a>Grundläggande inställningar
Kopplingar är värdbaserade tjänster som du kan komma åt som en del av en logik app toointegrate med andra tjänster som Dynamics, Azure, Salesforce, [och mer](apis-list.md).  De distribueras och hanteras av Microsoft, så att du kan bygga arbetsflöden integrering med skala, dataflöde och säkerhet som tagit hand om.  Du kan lägga till en koppling tooa logikapp genom att söka och välja en koppling åtgärd eller utlösa **visa Microsoft hanterade API: er**.

![Åtgärd-menyn för att välja utlösare][1]

Varje koppling åtgärd eller utlösare har en uppsättning egenskaper tooconfigure.  Du kan klicka på hello info knappar toolearn mer om åtgärden eller referera till dokumentationen [toolearn mer](apis-list.md).

Om du vill toointegrate med en tjänst eller API som inte är ännu en koppling, du kan också utöka logikappar via en [anpassad koppling](../logic-apps/logic-apps-create-api-app.md) eller bara anropa direkt toohello tjänsten via ett protokoll som HTTP.

## <a name="triggers"></a>Utlösare
Vissa kopplingar har en utlösare, vilket innebär att en händelse från kopplingen ska utlösa en logikapp som skickas i alla data som en del av hello utlösare.  En utlösare är alltid hello första steget i en logikapp.  Populära utlösare är åtgärder som:

* Återkommande - köras varje timme
* När en HTTP-förfrågan tas emot
* När ett objekt läggs tooa kön
* När ett e-postmeddelande tas emot

Vissa utlösare utlöses hello instant en händelse sker via en avisering toohello logikapp och andra måste ett intervall som konfigurerats på hur ofta hello logikapp kontrollerar hello tjänst för en händelse (upp tooevery 15 sekunder).  

När en händelse har tagits emot hello logikapp kör ska utlösas och hello åtgärder i hello arbetsflöde startar.  Du kan också kan tooaccess några data från hello utlösa i hela arbetsflödet hello (för exempel hello 'på en ny tweet' utlösaren skickar hello tweet till hello kör).

## <a name="actions"></a>Åtgärder
De flesta kopplingar har en eller flera åtgärder som kan köras som en del av hello arbetsflöde.  Åtgärder är alla åtgärder som inträffar efter hello kör har utlösts från en utlösare.  tooadd en åtgärd klickar du på hello **nytt steg** knappen och Sök efter hello önskad toouse koppling.  När markerat (och när du har konfigurerat någon [anslutningar](#connections) som kan krävas) visas hello åtgärd kort som du kan konfigurera.  Du kan välja data från föregående steg genom att klicka på någon av hello token för utdata eller ange i någon annan konfiguration efter behov.

![Konfigurera en koppling åtgärd][2]

## <a name="connections"></a>Anslutningar
De flesta kopplingar kräver tooconfigure en *anslutning* innan du kan använda hello anslutningen.  En *anslutning* är alla inloggning eller anslutningskonfiguration behövs tooaccess hello connector.  För kopplingar som använder OAuth, skapa en anslutning innebär att du loggar in hello tjänst (till exempel Office 365, Salesforce eller GitHub) där åtkomst-token kan krypteras och lagras på ett säkert sätt i en Azure hemliga butik.  Övriga kopplingar (till exempel FTP och SQL) kräver en anslutning som innehåller konfiguration som serveradress, användarnamn och lösenord.  Konfigurationsdetaljerna anslutningen också krypteras och lagras på ett säkert sätt.  Kommer att kunna tooaccess hello-tjänsten för så länge hello-tjänsten tillåter.  För Azure Active Directory OAuth-anslutningar (till exempel Office 365 och Dynamics) kan vi fortsätta toorefresh hello åtkomst-token under obestämd tid.  Andra tjänster kan ange begränsningar för hur länge vi kan använda en token utan den håller på att uppdateras.  I allmänhet vissa åtgärder som att ändra ett lösenord kommer att upphäva alla åtkomsttoken.  

Anslutningar kan visas och hanteras i Azure genom att klicka på **Bläddra** och välja **API anslutningar**.  Från hello API anslutningar resurs kan du visa, redigera, uppdatera eller att återauktorisera alla anslutningar som du har skapat.

## <a name="next-steps"></a>Nästa steg
* [Skapa din första logiska app](../logic-apps/logic-apps-create-a-logic-app.md)
* [Läs vanliga användningsområden och exempel på logikappar](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Kom igång med enterprise integration-utlösare och åtgärder](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png
