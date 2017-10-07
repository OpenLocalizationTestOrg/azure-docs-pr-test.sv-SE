---
title: "aaaGet igång med Autoskala i Azure | Microsoft Docs"
description: "Lär dig hur tooscale resurs i Azure."
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 6b3c3f4529018dcaf9691c538fec63dfbb3cea06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-autoscale-in-azure"></a>Kom igång med Autoskala i Azure
Den här artikeln beskriver hur tooset in Autoskala inställningarna för din resurs i hello Microsoft Azure-portalen.

Azure övervakaren Autoskala gäller endast toovirtual machine-skalningsuppsättningar, cloud services, Azure App Service-planer och apptjänstmiljöer. 

## <a name="discover-hello-autoscale-settings-in-your-subscription"></a>Identifiera hello Autoskala inställningarna i din prenumeration
Du kan identifiera alla hello resurser som autoskalning kan tillämpas i Azure-Monitor. Använd följande steg för en stegvis genomgång hello:

1. Öppna hello [Azure-portalen.][1]
2. Klicka på hello Azure ikonen i hello till vänster.
  ![Öppna Azure Övervakare][2]
3. Klicka på **Autoskala** tooview alla hello resurser för vilka Autoskala kan användas tillsammans med deras aktuella Autoskala status.
  ![Identifiera Autoskala i Azure-Monitor][3]

Du kan använda hello filterfönstret hello översta tooscope ned hello listan tooselect resurser i en viss resursgrupp, specifika resurstyper eller en viss resurs.

Du kommer märka hello aktuella standardinstansantalet och hello Autoskala status för varje resurs. hello Autoskala status kan vara:

- **Inte konfigurerad**: du har inte aktiverat Autoskala ännu för den här resursen.
- **Aktiverad**: du har aktiverat Autoskala för den här resursen.
- **Inaktiverad**: du har inaktiverat Autoskala för den här resursen.

## <a name="create-your-first-autoscale-setting"></a>Skapa din första autoskalningsinställning

Låt oss nu gå igenom en enkla steg för steg-toocreate din första autoskalningsinställning.

1. Öppna hello **Autoskala** bladet i Azure-Monitor och välj en resurs som du vill tooscale. (hello följande använda en App Service-plan som är associerade med ett webbprogram. Du kan [skapa din första ASP.NET-webbapp i Azure på fem minuter.] [4])
2. Observera att hello aktuella standardinstansantalet är 1. Klicka på **aktivera Autoskala**.
  ![Skalinställningen för ny webbapp][5]
3. Ange ett namn för hello skala inställningen och klicka sedan på **lägga till en regel**. Observera hello regeln skalningsalternativ som öppnas som en kontext fönstret hello höger. Som standard anger hello alternativet tooscale din instans räkna med 1 om hello CPU-procent av hello resurs överskrider 70 procent. Lämna till sina standardvärden och klicka på **Lägg till**.
  ![Skapa skalinställningen för en webbapp][6]
4. Nu har du skapat din första skalningsregeln. Observera att hello UX rekommenderar bästa praxis och att ”bör toohave minst en skalan i regeln”. toodo så:
  
    a. Klicka på **lägga till en regel**. 

    b. Ange **operatorn** för**mindre än**.

    c. Ange **tröskelvärdet** för**20**.

    d. Ange **åtgärden** för**minska antalet av**.

   Du bör nu ha en skalinställningen att skala ut/skalar i baserat på CPU-användning.
   ![Skala baserat på CPU][8]
5. Klicka på **Spara**.

Grattis! Du har nu skapat din första skala inställningen tooautoscale ditt webbprogram baserat på CPU-användning.

> [!NOTE] 
> hello likadant är tillämpliga tooget igång med en virtuell dator skala rolltjänst för uppsättningen eller molnet.

## <a name="other-considerations"></a>Andra överväganden
### <a name="scale-based-on-a-schedule"></a>Skala enligt ett schema
Dessutom tooscale baserat på CPU, du kan ange nivå på olika sätt för särskilda dagar i veckan hello.

1. Klicka på **Lägg till ett villkor för skala**.
2. Ange hello skala läge och hello regler är samma hello som hello standardtillståndet.
3. Välj **Upprepa särskilda dagar** för hello schema.
4. Välj hello dagar och hello Starttid/Sluttid för när hello skala villkor ska tillämpas.

![Skala villkor baserat på schema][9]
### <a name="scale-differently-on-specific-dates"></a>Skala annorlunda på specifika datum
Dessutom tooscale baserat på CPU, du kan ange nivå på olika sätt för specifika datum.

1. Klicka på **Lägg till ett villkor för skala**.
2. Ange hello skala läge och hello regler är samma hello som hello standardtillståndet.
3. Välj **ange start-/ slutdatum** för hello schema.
4. Välj hello start/slutdatum och hello Starttid/Sluttid för när hello skala villkor ska tillämpas.

![Skala villkor baserat på datum][10]

### <a name="view-hello-scale-history-of-your-resource"></a>Visa historiken för hello skala för din resurs
När din resurs skalas upp eller ned loggas en händelse i aktivitetsloggen för hello. Du kan visa hello skala historiken för din resurs för hello senaste 24 timmarna genom att växla toohello **kör historik** fliken.

![Kör historik][11]

Om du vill tooview hello fullständig skala historik (för upp too90 dagar), Välj **Klicka här toosee mer**. hello aktivitetsloggen öppnas med Autoskala markerat för din resurs och kategori.

### <a name="view-hello-scale-definition-of-your-resource"></a>Visa hello skala definition av din resurs
Autoskala är en Azure Resource Manager-resurs. Du kan visa hello skala definition i JSON genom att växla toohello **JSON** fliken.

![Skala definition][12]

Du kan göra ändringar i JSON direkt, om det behövs. Dessa ändringar visas när du har sparat.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Inaktivera Autoskala och skala dina instanser manuellt
Det kan finnas tillfällen när du vill toodisable din aktuella skalinställningen och skala resursen manuellt.

Klicka på hello **inaktivera Autoskala** knappen hello överst.
![Inaktivera Autoskala][13]

> [!NOTE] 
> Det här alternativet inaktiverar konfigurationen. Men kan du hämta tillbaka tooit när du har aktiverat Autoskala igen. 

Du kan nu ange hello antalet instanser som du vill tooscale toomanually.

![Ange manuell skala][14]

Du kan alltid gå tillbaka tooAutoscale genom att klicka på **aktivera Autoskala** och sedan **spara**.

## <a name="next-steps"></a>Nästa steg
- [Skapa en aktivitet loggen avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Skapa en aktivitet loggen avisering toomonitor alla misslyckade Autoskala skala-/ skalbar åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

