---
title: "aaaHow toouse hello SendGrid e-posttjänst (Node.js) | Microsoft Docs"
description: "Lär dig hur skicka e-post med hello SendGrid e-posttjänst på Azure. Kodexempel som skrivits med hello Node.js API."
services: 
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: 
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: fd617b6aaa656e7b5dd51c51ebb0db1e848450f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-nodejs"></a>Hur tooSend e-post med hjälp av SendGrid från Node.js
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med SendGrid e-tjänsten på Azure. hello exempel skrivs med hello Node.js API. hello beskrivs scenarier där **konstruera e-post**, **skicka e-post**, **lägga till bilagor**, **med hjälp av filter**, och **uppdatera egenskaperna för**. Mer information om SendGrid och skicka e-post finns hello [nästa steg](#next-steps) avsnitt.

## <a name="what-is-hello-sendgrid-email-service"></a>Vad är hello SendGrid e-posttjänst?
SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering. Vanliga Användningsscenarier för SendGrid är:

* Automatiskt skickar kvitton toocustomers
* Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden
* Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider
* Generera rapporter toohelp identifiera trender
* Vidarebefordran av kundfrågor
* E-postaviseringar från ditt program

Mer information finns i [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>Skapa ett SendGrid-konto
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-nodejs-module"></a>Referens hello SendGrid Node.js-modul
Hej SendGrid-modul för Node.js kan installeras via hello nod Pakethanteraren (npm) med hjälp av hello följande kommando:

    npm install sendgrid

Efter installationen kan kräva du hello modulen i ditt program med hjälp av hello följande kod:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

Hej SendGrid modulen exporterar hello **SendGrid** och **e-post** funktioner.
**SendGrid** ansvarar för att skicka e-post via Web API medan **e-post** kapslar in ett e-postmeddelande.

## <a name="how-to-create-an-email"></a>Så här: skapa ett e-postmeddelande
Skapa ett e-postmeddelande med hello SendGrid modulen innebär att först skapa ett e-postmeddelande hello e-funktionen, och skickar den med hjälp av hello SendGrid-funktionen. hello följande är ett exempel på hur du skapar ett nytt meddelande hello e-funktionen:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

Du kan också ange ett HTML-meddelande för klienter som stöder genom att ange hello HTML-egenskapen. Exempel:

    html: This is a sample <b>HTML<b> email message.

Ange egenskaper för båda hello text och html ger korrekt användning av textinnehåll för klienter som inte stöder HTML-meddelanden.

Mer information om alla egenskaper som stöds av hello e-funktionen finns [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Så här: skicka ett e-postmeddelande
När du har skapat ett e-postmeddelande med hello e-funktionen kan du skicka den via hello webb-API som tillhandahålls av SendGrid. 

### <a name="web-api"></a>Webb-API
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> Medan hello exemplen ovan visas skicka i ett e-objektet och motringning funktionen måste anropa du också direkt hello skicka funktionen genom att ange egenskaper för e-post direkt. Exempel:  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a>Så här: Lägg till en bifogad fil
Bifogade filer kan läggas till tooa meddelandet genom att ange hello filnamn och sökväg i hello **filer** egenskapen. hello som följande exempel visar skicka bifogade filer:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used toospecify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> När du använder hello **filer** egenskapen hello-filen måste vara tillgängligt via [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Om hello-fil som du vill tooattach finns i Azure Storage, exempelvis en Blob-behållare, måste du först kopiera hello fillagring toolocal eller tooan Azure enheten innan den kan skickas som en bifogad fil med hello **filer** egenskapen.
> 
> 

## <a name="how-to-use-filters-tooenable-footers-and-tracking"></a>Så här: filter tooEnable sidfötter och spårning
SendGrid ger ytterligare e-postfunktioner via hello filter. Dessa är inställningar som kan läggas till tooan e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera klickar du på spårning, Google analytics, prenumerationen spårning och så vidare. En fullständig lista över filter finns [filterinställningar][Filter Settings].

Filter kan vara tillämpade tooa meddelande med hjälp av hello **filter** egenskapen.
Varje filter som anges av ett hash-värde som innehåller filter-specifika inställningar.
hello följande exempel visar hello sidfoten och på Spåra filter:

### <a name="footer"></a>Sidfot
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a>Klicka på spårning
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a>Så här: uppdatera egenskaper för e-post
Vissa egenskaper för e-post kan skrivas över med  **ange*egenskapen*** eller läggas till med hjälp av  **lägga till*egenskapen***. Du kan till exempel lägga till ytterligare mottagare med hjälp av

    email.addTo('jeff@contoso.com');

eller ange ett filter med hjälp av

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

Mer information finns i [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>Så här: använda ytterligare SendGrid tjänster
SendGrid erbjuder webbaserade API: er som du kan använda tooleverage ytterligare SendGrid funktioner från din Azure-program. Fullständig information finns i hello [SendGrid API-dokumentationen][SendGrid API documentation].

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.

* SendGrid Node.js modulen databasen: [sendgrid nodejs][sendgrid-nodejs]
* SendGrid API-dokumentationen: <https://sendgrid.com/docs>
* SendGrid specialerbjudande för Azure-kunder: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions
[transaktionella e-postleverans]: https://sendgrid.com/transactional-email
