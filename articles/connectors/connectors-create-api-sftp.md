---
title: aaaLearn hur toouse hello SFTP connector i dina logic apps | Microsoft Docs
description: "Skapa logikappar med Azure App service. Anslut tooSFTP API toosend och ta emot filer. Du kan utföra olika åtgärder så som att skapa, uppdatera, hämta eller ta bort filer."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/20/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3f50570191c9b9339fe6584b9056b2549512b789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sftp-connector"></a>Kom igång med hello SFTP-koppling
Använd hello SFTP connector tooaccess toosend en SFTP-konto och ta emot filer. Du kan utföra olika åtgärder så som att skapa, uppdatera, hämta eller ta bort filer.  

toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp. Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toosftp"></a>Ansluta tooSFTP
Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service. En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.  

### <a name="create-a-connection-toosftp"></a>Skapa en anslutning tooSFTP
> [!INCLUDE [Steps toocreate a connection tooSFTP](../../includes/connectors-create-api-sftp.md)]
> 
> 

## <a name="use-an-sftp-trigger"></a>Använd en SFTP-utlösare
En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden. [Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

I det här exemplet hello **SFTP - när en fil har lagts till eller ändrats** utlösare är används tooinitiate en logik app arbetsflödet när en fil har lagts till eller ändrats på en SFTP-server. Du också lägga till ett villkor som kontrollerar hello innehållet i hello nya eller ändrade filen och gör en beslut tooextract hello-fil om innehållet visar ska extraheras innan du använder hello innehållet. Slutligen lägger du till en åtgärd tooextract hello innehållet i en fil och placerar hello extraherade innehållet i en mapp på hello SFTP-servern. 

Du kan använda den här utlösaren toomonitor en SFTP-mapp för nya filer som representerar order i ett enterprise-exempel.  Du kan sedan använda åtgärden SFTP-koppling som **hämta filinnehåll**, tooget hello innehållet i hello ordning för vidare bearbetning och lagring i en order-databas.

> [!INCLUDE [Steps toocreate an SFTP trigger](../../includes/connectors-create-api-sftp-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Lägg till ett villkor
> [!INCLUDE [Steps tooadd a condition](../../includes/connectors-create-api-sftp-condition.md)]
> 
> 

## <a name="use-an-sftp-action"></a>Använd en SFTP-åtgärd
En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp. [Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps toocreate an SFTP action](../../includes/connectors-create-api-sftp-action.md)]
> 
> 

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sftpconnector/).

## <a name="more-connectors"></a>Flera kopplingar
Gå tillbaka toohello [API: er listan](apis-list.md).
