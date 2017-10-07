---
title: "aaaCreate en funktionsapp från hello Azure Portal | Microsoft Docs"
description: "Skapa en ny funktionsapp i Azure App Service från hello-portalen."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c531fc71c798edf22e25a5f4b79c15413809dc86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-from-hello-azure-portal"></a>Skapa en funktionsapp från hello Azure-portalen

Azure funktionen appar använder hello Azure App Service-infrastruktur. Det här avsnittet beskrivs hur du toocreate en funktionsapp i hello Azure-portalen. En funktionsapp är hello-behållare som är värd för hello körningen av enskilda funktioner. När du skapar en funktionsapp i hello värd programtjänstplanen kan appen funktionen använda alla hello-funktioner i Apptjänst.

## <a name="create-a-function-app"></a>Skapa en funktionsapp

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

När du skapar en funktionsapp, ange en giltig **appnamn**, som kan innehålla endast bokstäver, siffror och bindestreck. Understreck (**_**) är inte tillåtna.

Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Namnet på ditt lagringskonto måste vara unikt i Azure. 

När hello funktionsapp har skapats kan skapa du enskilda funktioner i en eller flera olika språk. Skapa funktioner [med hjälp av hello portal](functions-create-first-azure-function.md#create-function), [kontinuerlig distribution](functions-continuous-deployment.md), eller av [Överför med FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Service-planer

Azure Functions har två olika serviceplaner: förbrukning planerings- och App Service-plan. hello förbrukning plan tilldelar automatiskt datorkraft när koden körs, skala ut som behövs toohandle belastning och skalar sedan i när koden inte körs. Hej programtjänstplanen ger din funktion app tooall hello lokaler av App Service. Du måste välja serviceplanen när funktionen appen skapas och den för närvarande inte kan ändras. Mer information finns i [väljer du ett Azure-funktioner som värd för planen](functions-scale.md).

Om du planerar toorun JavaScript-funktioner på en apptjänstplan, bör du välja en plan med färre kärnor. Mer information finns i hello [JavaScript-referens för funktioner](functions-reference-node.md#choose-single-core-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Lagringskraven för kontot

När du skapar en funktionsapp i App Service, måste du skapa eller länka tooa allmänna Azure Storage-konto som har stöd för lagring av Blob, köer och tabellen. Internt använder funktioner lagring för åtgärder som hanterar utlösare och loggning funktionen körningar. Vissa storage-konton stöder inte köer och tabeller som endast blob storage-konton, Azure Premium-lagring och allmänna lagringskonton med ZRS replikering. Dessa konton filtreras av från hello-bladet Storage-konto när du skapar en funktionsapp.

>[!NOTE]
>När du använder hello förbrukning värd plan lagras konfigurationsfilerna funktionen kod och bindningen i Azure File storage i hello huvudsakliga storage-konto. När du tar bort hello huvudsakliga storage-konto raderas innehållet och kan inte återställas.

toolearn mer om lagringskontotyper finns [introduktion till hello Azure Storage-tjänster](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



