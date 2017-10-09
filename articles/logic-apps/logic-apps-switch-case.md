---
title: "aaaSwitch uttryck för olika åtgärder i Azure Logic Apps | Microsoft Docs"
description: "Välj olika åtgärder tooperform i logikappar utifrån uttrycken med hjälp av en switch-sats"
services: logic-apps
keywords: Switch-instruktionen
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 09ed7e4a752003aba157e9156bf4dc89ef86f5ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Utför olika åtgärder i logikappar med en switch-sats

När du redigerar ett arbetsflöde har ofta tootake olika åtgärder baserat på hello-värdet för ett objekt eller ett uttryck. Du kanske till exempel din logik app toobehave på olika sätt beroende på hello statuskod för en HTTP-begäran eller ett alternativ som valts i ett e-postmeddelande.

Du kan använda en switch-instruktionen tooimplement dessa scenarier. Din logikapp kan utvärdera en token eller ett uttryck och välj hello fallet med hello samma värde tooexecute hello anges åtgärder. Endast ett fall ska matcha hello switch-instruktionen.

> [!TIP]
> Precis som alla programmeringsspråk stöder switch-satser endast likheten operatörer. Om du behöver andra relationsoperatorer använder du en villkorssatsen, till exempel ”större än”.
> tooensure deterministisk körningsbeteende fall måste innehålla ett unikt och statiska värde i stället för dynamiska token eller uttryck.

## <a name="prerequisites"></a>Krav

- En aktiv Azure-prenumeration. Om du inte har en aktiv Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/), eller försök [Logic Apps för kostnadsfri](https://tryappservice.azure.com/).
- [Grundläggande kunskaper om logikappar](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-tooyour-workflow"></a>Lägga till ett switch-instruktionen tooyour arbetsflöde

tooshow som så här fungerar en switch-instruktionen i det här exemplet skapar en logikapp att övervakar filer upp tooDropbox. Hello nya filer som överförs hello logikapp skickar e-post tooan godkännare som väljer om tootransfer tooSharePoint dessa filer. hello appen använder en switch-sats som utför olika åtgärder baserat på hello-värde som hello godkännare väljer.

1. Skapa en logikapp och välj den här utlösaren: **Dropbox - när en fil skapas**.

   ![Använda Dropbox - när en fil skapas utlösare](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. Lägg till den här åtgärden under hello utlösare: **Outlook.com - Skicka godkännande e-post**

   > [!TIP]
   > Logikappar stöder även skicka e-scenarier för godkännande från ett Office 365 Outlook-konto.

   - Om du inte har en befintlig anslutning, uppmanas du toocreate en.
   - Fyll i hello krävs. Till exempel under **till**, ange hello e-postadress för att skicka e-post för hello godkännare.
   - Under **användaralternativ**, ange `Approve, Reject`.

   ![Konfigurera anslutning](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Lägg till en switch-sats.

   - Välj **+ nytt steg** > **... Flera** > **lägga till ett switch-fall**. 
   - Nu vi vill tooselect hello åtgärd tooperform baserat på hello `SelectedOptions` utdata från hello *skicka e-post för godkännande* åtgärd. 
   Du hittar det här fältet i hello **lägga till dynamiskt innehåll** väljare.
   - Använd *fall 1* toohandle när hello godkännare väljer `Approve`.
     - Om det godkänns, kopiera hello ursprungliga filen tooSharePoint Online med hello [ **SharePoint Online - skapa filen** åtgärd](../connectors/connectors-create-api-sharepointonline.md).
     - Lägg till en annan åtgärd hello case toonotify användare att en ny fil är tillgänglig i SharePoint.
   - Lägg till en annan case toohandle när användaren väljer `Reject`.
     - Skicka ett e-postmeddelande som informerar andra godkännare att hello filen avvisas och ingen ytterligare åtgärd krävs om avvisas.
   - `SelectedOptions`ger endast två alternativ, så vi kan lämna hello **standard** fallet är tom.

   ![Switch-instruktionen](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > En switch-instruktionen måste minst ett fall i tillägg toohello standardfallet.

4. Ta bort hello ursprungliga filen har överförts tooDropbox genom att lägga till den här åtgärden efter hello switch-instruktionen: **Dropbox - ta bort filen**

5. Spara din logikapp. Testa din app genom att överföra en fil tooDropbox. Du bör få ett godkännande e-postmeddelande inom kort. Välj ett alternativ och Observera hello beteende.

   > [!TIP]
   > Kolla hur för[övervaka dina logikappar](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-hello-code-behind-switch-statements"></a>Förstå hello koden bakom switch-satser

Nu när du har skapat en logikapp med hjälp av en switch-sats, vi titta på hello kod definition bakom hello switch-instruktionen.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* `"Switch"`är hello namn hello switch-instruktionen, som du kan byta namn för läsbarhet. 
* `"type": "Switch"`Anger att hello har en switch-sats. 
* `"expression"`hello godkännare valda alternativ i det här exemplet och utvärderas mot varje fall som deklarerats senare i hello definition. 
* `"cases"`kan innehålla valfritt antal fall. För varje fall `"Case *"` är hello standardnamnet hello fall där du kan byta namn för läsbarhet. 
`"case"`Anger hello case etiketten hello växeln uttryck används för jämförelse, och måste vara ett konstant och unikt värde. Om inget av hello fall matchar hello switch-uttryck, åtgärder under `"default"` utförs.

## <a name="get-help"></a>Få hjälp

tooask frågor, besvara frågor och se vilka andra användare i Azure Logic Apps gör finns hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

- Lär dig hur för[Lägg till villkor](logic-apps-use-logic-app-features.md)
- Lär dig mer om [fel och undantagshantering](logic-apps-exception-handling.md)
- Utforska mer [arbetsflöde språk funktioner](logic-apps-author-definitions.md)