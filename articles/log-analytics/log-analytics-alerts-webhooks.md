---
title: "aaaWebhook Aviseringsåtgärd provet i OMS Log Analytics | Microsoft Docs"
description: "En av hello åtgärder du kan köra i svaret tooa logganalys aviseringen är en * webhook *, vilket gör att du tooinvoke en extern process via en HTTP-begäran. Den här artikeln beskriver hur ett exempel på hur du skapar en webhook-åtgärd i en Log Analytics-avisering med Slack."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: e60bdc4922347073d572c2e4719461b13e8e7d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-toosend-message-tooslack"></a>Skapa en avisering webhook-åtgärd i OMS logganalys toosend meddelandet tooSlack
En av hello åtgärder du kan köra i svaret tooa [logganalys avisering](log-analytics-alerts.md) är en *webhook*, vilket gör att du tooinvoke en extern process via en HTTP-begäran.  Du kan läsa om information om aviseringar och webhooks i [aviseringar i logganalys](log-analytics-alerts.md)

I den här artikeln går vi igenom ett exempel på hur du skapar en webhook-åtgärd i en Log Analytics-avisering med Slack som är en meddelandetjänst.

> [!NOTE]
> Du måste ha en Slack konto toocomplete det här exemplet.  Du kan registrera dig för ett kostnadsfritt konto på [slack.com](http://slack.com).
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a>Steg 1 – Aktivera webhooks i Slack
1. Logga in tooSlack på [slack.com](http://slack.com).
2. Välj en kanal i hello **kanaler** avsnitt i hello till vänster.  Detta är hello kanal som hello-meddelande skickas till.  Du kan välja något av hello standard kanaler som **allmänna** eller **slumpmässiga**.  I ett produktionsscenario, skulle du förmodligen skapa en särskild kanal som **criticalservicealerts**. <br>
   
   ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. Klicka på **lägga till en app eller en anpassad integrering** tooopen hello programkatalogen.
4. Typen *webhooks* till hello sökrutan och väljer sedan **inkommande WebHooks**. <br>
   
   ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. Klicka på **installera** nästa tooyour Teamnamn.
6. Klicka på **lägga till konfigurationen**.
7. Välj hello hello kanal du kommer toouse för det här exemplet och klicka sedan på **Lägg till inkommande WebHooks integrering**.  
8. Kopiera hello **Webhooksadressen**.  Du kommer att klistra in det i hello varningskonfigurationen. <br>
   
    ![Slack-kanaler](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Steg 2 – Skapa varningsregeln i logganalys
1. [Skapa en aviseringsregel](log-analytics-alerts.md) med hello följande inställningar.
   * Fråga:```    Type=Event EventLevelName=error ```
   * Sök efter den här aviseringen var: 5 minuter
   * hello antalet resultat är: större än 10
   * Under den här tidsperioden: 60 minuter
   * Välj **Ja** för **Webhook** och **nr** för hello andra åtgärder.
2. Klistra in hello Slack URL till hello **Webhooksadressen** fältet.
3. Välj alternativet för hello för**omfattar en anpassad JSON-nyttolast**.
4. Slack förväntar sig en nyttolast som har formaterats i JSON med en parameter med namnet *text*.  Detta är hello text som visas i hello-meddelande skapas.  Du kan använda en eller flera av hello avisering parametrar med hello  *#*  symbol exempelvis som i följande exempel hello.
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds hello over threshold of #thresholdvalue ."
    }
    ```
   
    ![exempel JSON-nyttolast](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. Klicka på **spara** toosave hello varningsregel.
6. Vänta tillräckligt med tid för en avisering toobe skapas och kontrollera Slack för ett meddelande som kommer att vara liknande toohello följande.
   
   ![exempel webhook i Slack](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a>Avancerade webhook nyttolasten för Slack
Stor utsträckning kan du anpassa inkommande meddelanden med Slack. Mer information finns i [inkommande Webhooks](https://api.slack.com/incoming-webhooks) på hello Slack-webbplatsen. Följande är en mer komplex nyttolast toocreate ett omfattande meddelande med formatering:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link tooSearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


Detta skulle generera ett meddelande i Slack liknande toohello följande.

![exempel på meddelande i Slack](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Sammanfattning
Med den här varningsregeln på plats har du ett meddelande som skickas tooSlack varje gång hello villkoret är uppfyllt.  

Detta är bara ett exempel på en åtgärd som du kan skapa i svaret tooan avisering.  Du kan skapa en webhook-åtgärd som anropar en annan extern tjänst, en runbook åtgärd toostart en runbook i Azure Automation eller en e-åtgärd toosend tooyourself en e-post eller andra mottagare.   

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om andra [Varna åtgärder i logganalys](log-analytics-alerts-actions.md) inklusive andra åtgärder.


