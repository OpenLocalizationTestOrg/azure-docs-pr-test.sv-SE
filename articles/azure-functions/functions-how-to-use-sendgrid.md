---
title: aaaHow toouse SendGrid i Azure Functions | Microsoft Docs
description: Visar hur toouse SendGrid i Azure Functions
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
ms.openlocfilehash: a0ffdae04e5924c773d2d26427626fc1f570f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-sendgrid-in-azure-functions"></a>Hur toouse SendGrid i Azure Functions

## <a name="sendgrid-overview"></a>Översikt över SendGrid

Azure Functions stöder SendGrid utdata bindningar tooenable funktioner toosend e-postmeddelanden med några få rader med kod och ett SendGrid-konto.

toouse hello SendGrid API i en Azure-funktion, måste du ha en [SendGrid konto](http://SendGrid.com). Du måste dessutom ha en SendGrid API-nyckel. Logga in tooyour SendGrid-kontot och klickar på **inställningar** sedan **API-nyckel** toogenerate en API-nyckel. Behåll den här nyckeln som är tillgängliga när du använder den i ett kommande steg.

Du är nu redo toocreate en app i Azure-funktion.

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app 

Azure funktionen appar är behållare för en eller flera Azure functions. Azure functions är just - funktionen. Varje Azure-funktion är bundet tooone trigger, vilket är en händelse som orsakar hello funktionen toorun.
Varje funktion kan innehålla valfritt antal indata eller utdata bindningar. Bindningar är tjänster som du kan använda i en funktion. SendGrid är ett utgående bindning du kan använda toosend e-post. 

1. Logga in toohello Azure-portalen och [skapa ett Azure-Funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) eller öppna en befintlig funktionsapp. 
2. Skapa en Azure-funktion. tookeep den enkla, välja en manuell utlösare och C#. 

 ![Skapa en Azure-funktion](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>Konfigurera SendGrid för användning i en app i Azure-funktion

Du måste lagra din SendGrid API-nyckel som en app inställningen toouse den i en funktion. Hej ApiKey fältet är inte din faktiska SendGrid API-nyckel, men en app som du du definiera som representerar din faktiska API-nyckel. Lagra din nyckel för det här sättet är för säkerhet, eftersom det är separat från någon kod eller filer som kan kontrolleras i källkodskontroll.

- Skapa en **AppSettings** nyckeln i appen funktionen **programinställningar**.

 ![Skapa en Azure-funktion](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>Konfigurera SendGrid utdata bindningar

SendGrid är tillgänglig som en Azure funktionen utdatabindning. toocreate en SendGrid utdatabindning:

1. Gå toohello **integrera** fliken hello-funktionen i hello Azure-portalen.
2. Klicka på **nya utdata** toocreate en SendGrid utdatabindning.
3. Fyll i hello **API-nyckel** och **meddelandet parameternamnet** egenskaper. Om du vill kan du ange hello andra egenskaper nu eller code dem i stället. De här inställningarna kan användas som standard.

 ![Konfigurera SendGrid utdata bindningar](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Om du lägger till en bindning tooa funktion skapas en fil med namnet **function.json** i mappen för din funktion. Den här filen innehåller alla hello samma information som du ser i hello Azure funktionen **integrera** fliken, men i Json-format. Inställningen hello **ApiKey**, **meddelandet**, och **från** fält skapa hello följande poster i hello **function.json** fil: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Om du vill kan du ändra den här filen själv direkt.

Nu när du har skapat och konfigurerat hello Funktionsapp och funktionen, kan du skriva hello kod toosend ett e-postmeddelande.

## <a name="write-code-that-creates-and-sends-email"></a>Skriva kod som skapar och skickar e-post

Hej SendGrid API innehåller alla hello-kommandon du behöver toocreate och skicka ett e-postmeddelande.  

- Ersätt hello koden i hello-funktionen med hello följande kod:

```cs
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
    message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    // change tooemail of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

Meddelande hello första raden innehåller hello ```#r``` direktiv som refererar till hello SendGrid sammansättning. Sedan kan du använda en ```using``` instruktionen toomore enkelt komma åt hello objekt i namnutrymmet. I hello koden skapar instanser av ```Mail```, ```Personalization```, och ```Content``` objekt från hello SendGrid API som ska utgöra hello e-post. När du går tillbaka hello-meddelande, ger den SendGrid. 

hello funktionens signaturen innehåller också en extra out-parameter av typen ```Mail``` med namnet ```message```. Både indata och utdata bindningar express sig själva som funktionsparametrar i kod. 

2. Testa din kod genom att klicka på **Test** och ange ett meddelande i hello **Begärandetext** fältet och sedan på hello **kör** knappen.

 ![Testa din kod](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Kontrollera e-tooverify som skickats SendGrid hello e-post. Det ska gå toohello adress i hello kod från steg 1 och innehålla hello-meddelande från hello **Begärandetext**.

## <a name="next-steps"></a>Nästa steg
Den här artikeln har visat hur toouse hello SendGrid service toocreate och skicka e-post. toolearn mer information om hur du använder Azure Functions i dina appar finns hello följande avsnitt: 

- [Metodtips för Azure Functions](functions-best-practices.md) innehåller vissa bästa praxis toouse när du skapar Azure Functions.

- [Azure Functions för utvecklare](functions-reference.md) programmerare om att koda funktioner och definiera utlösare och bindningar.

- [Testa Azure Functions](functions-test-a-function.md) beskriver olika verktyg och tekniker för att testa funktioner.