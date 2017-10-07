---
title: "aviseringar i realtid för aaaAzure CDN | Microsoft Docs"
description: Aviseringar i realtid i Microsoft Azure CDN. Meddelanden om hello prestanda hello slutpunkter i CDN-profilen med realtid aviseringar.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 269c90437088bbc41bf899a8c02749e8e6f3006c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Aviseringar i realtid i Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Översikt
Det här dokumentet förklarar aviseringar i realtid i Microsoft Azure CDN. Den här funktionen innehåller meddelanden i realtid om hello prestanda hello slutpunkter i CDN-profilen.  Du kan konfigurera e-post eller HTTP-varningar baserat på:

* Bandbredd
* Statuskoder
* Cache-status
* Anslutningar

## <a name="creating-a-real-time-alert"></a>Skapa en avisering om realtid
1. I hello [Azure Portal](https://portal.azure.com), bläddra tooyour CDN-profilen.
   
    ![CDN-profilbladet](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. Klicka på hello hello CDN-profilbladet **hantera** knappen.
   
    ![CDN-profilbladet hantera knappen](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    hello CDN-hanteringsportalen öppnas.
3. Hovra över hello **Analytics** fliken och sedan hovra över hello **realtid Stats** utfällbar.  Klicka på **Aviserinar**.
   
    ![CDN-hanteringsportalen](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    hello lista över befintliga aviseringen konfigurationer (eventuella) visas.
4. Klicka på hello **Lägg till avisering** knappen.
   
    ![Aviseringen knappen Lägg till](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Ett formulär för att skapa en ny avisering visas.
   
    ![Ny avisering formulär](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Om du vill att den här aviseringen toobe aktiv när du klickar på **spara**, kontrollera hello **avisering aktiverad** kryssrutan.
6. Ange ett beskrivande namn för aviseringen i hello **namn** fältet.
7. I hello **medietyp** listrutan **HTTP stort objekt**.
   
    ![Medietyp med HTTP-stort objekt som valts](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > Du måste välja **HTTP stort objekt** som hello **medietyp**.  hello andra alternativ används inte av **Azure CDN från Verizon**.  Fel tooselect **HTTP Large Object** kommer aviseringen toonever utlösas.
   > 
   > 
8. Skapa en **uttryck** toomonitor genom att välja en **mått**, **operatorn**, och **utlösa värdet**.
   
   * För **mått**, Välj hello typ av villkor som önskade övervakade.  **Mbit/s bandbredd** hello mängden bandbreddsanvändning i megabit per sekund.  **Totalt antal anslutningar** är hello antalet samtidiga HTTP-anslutningar tooour kant-servrar.  Definitioner av hello olika cache status och statuskoder finns [Azure CDN Cache-statuskoder](https://msdn.microsoft.com/library/mt759237.aspx) och [Azure CDN HTTP-statuskoder](https://msdn.microsoft.com/library/mt759238.aspx)
   * **Operatorn** hello matematisk operator som fastställer hello förhållandet mellan hello mått och hello utlösaren värde.
   * **Utlösa värdet** är hello tröskelvärde som måste vara uppfyllda innan ett meddelande skickas.
     
     I hello exemplet nedan visar hello-uttryck som jag har skapat jag vill att ett meddelande när hello antalet 404 statuskoder är större än 25 toobe.
     
     ![Realtid avisering exempeluttryck](./media/cdn-real-time-alerts/cdn-expression.png)
9. För **intervall**, ange hur ofta du vill att hello-uttryck som utvärderas.
10. I hello **meddela på** listrutan när du vill toobe meddelas när hello uttrycket är sant.
    
    * **Condition-Start** anger att en avisering skickas när hello angivna villkor först har identifierats.
    * **Condition-End** anger att en avisering skickas när hello angetts villkoret kan inte längre hittas. Det här meddelandet kan bara aktiveras när våra nätverksövervakning systemet upptäckte att hello som angetts tillstånd inträffade.
    * **Kontinuerlig** anger att ett meddelande ska skickas varje gång som hello kontrollsystem för nätverk identifierar hello angivna villkor. Tänk på att hello nätverksövervakning system kommer bara kontrollera en gång per intervall för hello angivna villkor.
    * **Villkoret Start- och slutdatum** anger att ett meddelande ska skickas hello första gången som hello angivet villkor har identifierats och igen när hello villkoret kan inte längre hittas.
11. Om du vill tooreceive meddelanden via e-post, kontrollera hello **Avisera via e-post** kryssrutan.  
    
    ![Meddela via e-formulär](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    I hello **till** anger hello e-postadress du där du vill att meddelanden skickas. För **ämne** och **brödtext**, kan du lämna hello standard och du kan anpassa hello-meddelande med hello **tillgängliga nyckelord** lista toodynamically Infoga aviseringsdata när hello-meddelande skickas.
    
    > [!NOTE]
    > Du kan testa hello e-postmeddelande genom att klicka på hello **testmeddelande** knappen, men endast efter hello varningskonfigurationen har sparats.
    > 
    > 
12. Om du vill att meddelanden toobe anslås tooa webbservern Kontrollera hello **Avisera av HTTP Post** kryssrutan.
    
    ![Meddela via HTTP Post-formulär](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    I hello **Url** anger hello-URL du där du vill att hello HTTP-meddelande skickat. I hello **huvuden** textruta ange hello HTTP-huvuden toobe skickas i hello-begäran.  För **brödtext** du kan anpassa hello-meddelande med hello **tillgängliga nyckelord** lista toodynamically Infoga aviseringsdata när hello-meddelande skickas.  **Huvuden** och **brödtext** standard tooan XML-nyttolasten liknande toohello exemplet nedan.
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > Du kan testa hello HTTP Post-meddelanden genom att klicka på hello **testmeddelande** knappen, men endast efter hello varningskonfigurationen har sparats.
    > 
    > 
13. Klicka på hello **spara** knappen toosave avisering konfigurationen.  När du har markerat **avisering aktiverad** i steg 5 aviseringen är aktiv.

## <a name="next-steps"></a>Nästa steg
* Analysera [realtid statistik i Azure CDN](cdn-real-time-stats.md)
* Gräva djupare med [avancerade http-rapporter](cdn-advanced-http-reports.md)
* Analysera [användningsmönster](cdn-analyze-usage-patterns.md)

