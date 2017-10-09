---
title: aaaControlling Azure web app trafik med Azure Traffic Manager
description: "Den här artikeln innehåller översiktsinformation för Azure Traffic Manager när det gäller tooAzure webbprogram."
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: a93d4c9370046d54e401e36e7b495af8b711a2aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Styr Azure-webbapptrafik med Azure Traffic Manager
> [!NOTE]
> Den här artikeln innehåller översiktsinformation för Microsoft Azure Traffic Manager när det gäller tooAzure App Service Web Apps. Mer information om Azure Traffic Manager själva hittar genom att besöka hello länkar hello slutet av den här artikeln.
> 
> 

## <a name="introduction"></a>Introduktion
Du kan använda Azure Traffic Manager toocontrol hur begäranden från webbklienter är distribuerade tooweb appar i Azure App Service. Om web app slutpunkter som läggs till tooa Azure Traffic Manager-profil, Azure Traffic Manager håller reda på hello status för dina webbappar (körs, stoppas eller borttagna) så att den kan avgöra vilken av dessa slutpunkter ska ta emot trafik.

## <a name="load-balancing-methods"></a>Läsa in belastningsutjämning metoder
Azure Traffic Manager använder tre olika belastningen belastningsutjämning metoder. Dessa beskrivs i hello följande lista som de gäller tooAzure webbprogram.

* **Redundans**: Om du har web app kloner i olika regioner kan du använder den här metoden tooconfigure en web app tooservice all webbtrafik klienten och konfigurera en annan webbapp i en annan region-tooservice trafik i case hello första webbapp otillgänglig.
* **Resursallokering**: Om du har web app kloner i olika regioner, kan du använda den här metoden toodistribute trafiken jämnt över hello web apps i olika regioner.
* **Prestanda**: hello prestanda metod distribuerar trafik baserat på hello kortaste förfluten tid tooclients. hello prestanda metoden kan användas för webbprogram inom hello samma region eller i olika regioner.

## <a name="web-apps-and-traffic-manager-profiles"></a>Webbprogram och Traffic Manager-profiler
tooconfigure hello kontroll för webbtrafik app du skapa en profil i Azure Traffic Manager som använder en av hello tre metoder som beskrivs ovan för belastningsutjämning och Lägg sedan till hello slutpunkter (i det här fallet webbprogram) som du vill toocontrol trafik toohello profil. Din web appstatus (körs, stoppas eller borttagna) är regelbundet meddelas toohello profilen så att Azure Traffic Manager kan dirigera trafik i enlighet med detta.

När du använder Azure Traffic Manager med Azure, Kom ihåg hello följande punkter:

* För web appdistributioner för endast inom hello ger samma region, Web Apps redan redundans och resursallokering funktioner utan hänsyn tooweb app-läge.
* För distributioner i Hej samma region som använder Web Apps tillsammans med en annan Azure-molntjänst, du kan kombinera båda typer av slutpunkter tooenable hybridmoln.
* Du kan bara ange en web app slutpunkt per region i en profil. När du väljer en webbapp som en slutpunkt för en region hello återstående webbprogram i regionen att vara tillgänglig för val för den här profilen.
* hello web app slutpunkter som du anger i en Azure Traffic Manager-profilen visas under hello **domännamn** avsnittet på hello konfigurationssidan för hello webbprogrammet i hello profil, men kommer inte att konfigurera det.
* När du har lagt till en profil för web app tooa hello **Webbadress** på hello instrumentpanelen i hello webbplats visa appens Portalsida hello domänen URL för hello webbprogram om du har skapat en. Annars hello Traffic Manager-profilens URL visas (till exempel `contoso.trafficmgr.com`). Både hello direkt domännamnet för hello webbprogram och hello Traffic Manager-URL: en kommer att visas på konfigurationssidan hello webbprogram under hello **domännamn** avsnitt.
* Dina egna domännamn fungerar som förväntat, men i tillägg tooadding dem tooyour webbappar, måste du även konfigurera din DNS-mappning toopoint toohello Traffic Manager-URL. Mer information om hur tooset upp en anpassad domän för en webbapp i Azure finns [konfigurera ett anpassat domännamn för en Azure-webbplats](app-service-web-tutorial-custom-domain.md).
* Du kan bara lägga till webbprogram som är i läget standard eller premium tooa Azure Traffic Manager-profilen.

## <a name="next-steps"></a>Nästa steg
En konceptuell och teknisk översikt av Azure Traffic Manager finns i [Traffic Manager-översikt](../traffic-manager/traffic-manager-overview.md).

Mer information om hur du använder Traffic Manager med Web Apps finns i blogginläggen för hello [med hjälp av Azure Traffic Manager med Azure webbplatser](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) och [Azure Traffic Manager kan nu integrera med Azure webbplatser](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

