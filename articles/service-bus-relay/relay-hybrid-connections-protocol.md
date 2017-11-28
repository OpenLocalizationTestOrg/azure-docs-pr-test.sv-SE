---
title: aaaAzure Relay Hybridanslutningar protokollet guiden | Microsoft Docs
description: "Guide för Azure Relay Hybridanslutningar-protokollet."
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: 
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm;clemensv
ms.openlocfilehash: 2d145d919d606ae4722b063e1baf39fb845a600a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Azure Relay Hybridanslutningar protokoll
Azure Relay är en av hello viktiga kapaciteten pelare hello Azure Service Bus-plattform. hello nya *Hybridanslutningar* möjligheterna för vidarebefordran är en säker, öppna protokoll utvecklingen baserat på http- och WebSockets. Det ersätter hello förstnämnda lika med namnet *BizTalk-tjänst* funktion som har byggt på protokoll. hello integrering av Hybridanslutningar i Azure App Service fortsätter toofunction som-är.

Hybridanslutningar möjliggör dubbelriktad, binär dataström kommunikation mellan två nätverksprogram då ena eller båda parterna kan finnas bakom NAT och brandväggar. Den här artikeln beskriver hello klientsidan interaktioner med hello Hybridanslutningar relay för att ansluta klienter i lyssnaren och avsändaren roller och hur lyssnare ta emot nya anslutningar.

## Interaktion modellen
Hej Hybridanslutningar relay ansluter två parter genom att tillhandahålla en rendezvous-punkt i hello Azure-molnet både parter kan identifiera och ansluta toofrom sina egna nätverk perspektiv. Den tidpunkten och rendezvous kallas ”Hybridanslutning” i den här och övrig dokumentation i hello API: er och även i hello Azure-portalen. Hej kallas Hybridanslutningar tjänstslutpunkten är tooas hello ”tjänst” hello resten av den här artikeln. hello interaktion modellen leans på hello nomenklaturen upprättas av många andra nätverksfunktioner API: er.

Det finns en lyssnare som först anger beredskap toohandle inkommande anslutningar och därefter godkänner dem när de tas emot. Hej på andra sidan finns det en anslutande klienter som ansluter till hello lyssnare, väntar på att anslutningen toobe accepteras för att upprätta en dubbelriktad kommunikation sökväg.
”Anslut” ”lyssna” och ”accepterar” är hello samma villkor som du hittar i de flesta socket API: er.

Alla vidarebefordrande kommunikation har någon av parterna utgående anslutningar upprättas mot en tjänstslutpunkt som gör att-lyssnaren ”hello” också ”klient” talspråkliga används och kan även orsakas av andra terminologi överlagringar. hello exakt terminologi vi därför använda Hybridanslutningar är följande:

hello program på båda sidor av en anslutning kallas ”klienter”, eftersom de är klienter toohello service. hello SA klienten som väntar och accepterar anslutningar är ”lyssnaren” eller toobe i hello ”lyssnare roll”. hello-klient som initierar en ny anslutning till en lyssnare via hello tjänsten kallas hello ”avsändare”, eller finns i ”avsändaren roll”.

### Lyssnare interaktioner
hello-lyssnare har fyra interaktioner med hello-tjänst. alla överföring uppgifter beskrivs nedan i avsnittet för hello-referens.

#### Lyssna
tooindicate beredskap toohello tjänsten att en lyssnare är klar tooaccept anslutningar, skapar ett utgående WebSocket-anslutningen. hello anslutning handskakning innebär hello namnet på en Hybridanslutning som konfigurerats på hello Relay namnområde och en säkerhetstoken som ger hello ”lyssna” till att namnet.
När hello WebSocket accepteras av hello service hello registreringen är klar och hello upprätta web WebSocket sparas alive som hello ”kontrollkanal” för att aktivera alla efterföljande interaktioner. hello-tjänsten tillåter upp too25 samtidiga lyssnare på en Hybrid-anslutning. Om det finns två eller flera aktiva lyssnare, balanseras inkommande anslutningar mellan dem i slumpmässig ordning. rättvis fördelning är inte säkert.

#### Acceptera
När en avsändare öppnas en ny anslutning på hello tjänsten väljer hello-tjänsten och meddelar en hello aktiva lyssnare på hello Hybridanslutning. Det här meddelandet skickas toohello lyssnare över hello öppna kontrollkanal som ett JSON-meddelande som innehåller hello-URL för hello WebSocket-slutpunkt som hello lyssnare måste ansluta toofor hello anslutning ska godkännas.

hello-URL kan och måste användas direkt av hello lyssnare utan någon extra arbete.
hello kodad information är endast giltigt för en kort tidsperiod i grunden för så länge som hello avsändare villigt toowait för hello anslutning toobe upprätta slutpunkt till slutpunkt, men in tooa högst 30 sekunder. hello-URL kan endast användas för en lyckade anslutningsförsöket. Så snart som hello WebSocket-anslutning med hello rendezvous URL upprättas all ytterligare aktivitet på den här WebSocket vidarebefordrande från och toohello avsändaren, utan några åtgärder eller tolkning av hello-tjänsten.

#### Förnya
hello säkerhetstoken som måste använda tooregister hello-lyssnare och underhålla kontrollkanalen kan gälla när hello lyssnare är aktiv. hello token upphör att gälla påverkar inte pågående anslutningar, men den orsakar hello kontrollen kanal toobe bort av hello-tjänsten på eller strax efter hello nu har upphört att gälla. Hej ”förnya” åtgärden är en JSON-meddelande som hello lyssnare kan skicka tooreplace hello token som är associerat med hello kontrollkanal hello kontrollkanal kan underhållas under långa perioder.

#### Pinga
Om hello kontrollkanal hålls inaktiv för länge mellanhand på hello sätt, till exempel belastningen kan belastningsutjämnare eller NAT-enheter släppa hello TCP-anslutning. Hej ”pinga” åtgärden förhindrar som genom att skicka en liten mängd data på hello-kanal som påminner alla på hello nätverksväg hello anslutningen är avsedd toobe alive och den fungerar också som en ”live” test för hello-lyssnaren. Om hello ping misslyckas hello kontrollkanal ska betraktas som inte kan användas och hello lyssnare borde återansluta.

### Avsändaren interaktion
hello avsändaren har bara en enskild interaktion med hello-tjänsten: ansluter den.

#### Anslut
Hej ”ansluta” åtgärden öppnar en WebSocket på hello-tjänsten, tillhandahåller hello namnet på hello Hybridanslutning och (valfritt, men måste som standard) en säkerhetstoken som ger ”skicka” behörighet i hello frågesträngen. hello-tjänsten sedan samverkar med hello lyssnare i hello sätt som beskrivs ovan och hello lyssnare skapar en rendezvous-anslutning som är kopplad till den här WebSocket. När hello WebSocket har godkänts, är alla ytterligare interaktioner på den WebSocket med en lyssnare för anslutna.

### Interaktion sammanfattning
hello resultatet av den här interaktionen modellen är hello avsändaren klienten kommer utanför handskakning med en ”ren” WebSocket som är anslutna tooa lyssnare och som behöver ingen ytterligare preambles eller förberedelser. Den här modellen ger praktiskt taget alla befintliga WebSocket-klienten implementering tooreadily dra nytta av hello Hybridanslutningar tjänsten genom att ange en korrekt byggda URL till sina lager för WebSocket-klienten.

WebSocket som hello lyssnare hämtar via acceptera interaktionen hello rendezvous-anslutning är också rensa och kan överlämnas tooany befintliga WebSocket-serverimplementering med vissa minimal extra abstraktion som skiljer mellan ”acceptera” åtgärder i sina framework lokala nätverket lyssnare och Hybridanslutningar remote accepterar ”” åtgärder.

## Referens för protokollet

Det här avsnittet beskrivs hello information om hello protokollet interaktioner som beskrivs ovan.

Alla WebSocket-anslutningar görs på port 443 som en uppgradering från HTTPS 1.1, som representeras ofta av vissa WebSocket framework eller API. Beskrivningen här sparas implementering neutral, utan tyder på ett specifikt ramverk.

### Lyssnarprotokollet
hello-lyssnarprotokollet består av två anslutning gester och tre meddelandeåtgärder.

#### Lyssnare kontrollanslutningen kanal
hello kontrollkanal öppnas med att skapa en WebSocket-anslutning till:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

Hej `namespace-address` är hello fullständigt kvalificerade domännamnet för hello Azure Relay namnområde att värdar hello Hybridanslutning vanligtvis av hello formulär `{myname}.servicebus.windows.net`.

hello frågealternativ sträng parameter är som följer.

| Parameter | Krävs | Beskrivning |
| --- | --- | --- |
| `sb-hc-action` |Ja |För hello lyssnare rollen hello parametern måste vara **sb-hc-action = lyssna** |
| `{path}` |Ja |hello URL-kodade namnområdessökväg av hello förkonfigurerade Hybridanslutning tooregister den här lyssnaren på. Det här uttrycket är tillagda toohello fast `$hc/` sökvägsdelen. |
| `sb-hc-token` |Ja\* |hello lyssnare måste ange en giltig, URL-kodade Service Bus delad åtkomst-Token för hello namnområde eller Hybrid som ger hello **lyssna** rätt. |
| `sb-hc-id` |Nej |Den här klienten har angett valfritt ID aktiverar diagnostikspårning för slutpunkt till slutpunkt. |

Om hello WebSocket-anslutningen misslyckas på grund av toohello Hybridanslutning sökväg inte registreras eller en ogiltig eller saknas token eller några andra fel tillhandahålls hello fel feedback med hjälp av hello vanlig HTTP 1.1 status feedback modellen. Statusbeskrivningen innehåller ett fel för spårnings-id som kan överföras till Azure supporttekniker:

| Kod | Fel | Beskrivning |
| --- | --- | --- |
| 404 |Det gick inte att hitta |Hej Hybridanslutning sökväg är ogiltig eller hello grundläggande Webbadress är felaktig. |
| 401 |Behörighet saknas |hello säkerhetstoken är saknad eller skadad eller ogiltig. |
| 403 |Tillåts inte |hello säkerhetstoken är inte giltig för den här sökvägen för den här åtgärden. |
| 500 |Internt fel |Något gick fel i hello-tjänsten. |

Om hello WebSocket-anslutningen avsiktligt stängs av hello-tjänsten efter att det ursprungligen har ställts in hello orsaken till detta så överförs med hjälp av lämplig WebSocket-protokollet felkoden tillsammans med ett beskrivande felmeddelande som innehåller också en spårning -ID. hello stängs inte tjänsten ner kontrollkanalen utan ett feltillstånd. En ren avstängning är klient kontrolleras.

| WS-Status | Beskrivning |
| --- | --- |
| 1001 |hello Hybridanslutning sökvägen har tagits bort eller inaktiverats. |
| 1008 |hello säkerhets-token har upphört att gälla, därför hello auktoriseringsprincip överskrids. |
| 1011 |Något gick fel i hello-tjänsten. |

### Acceptera handskakning
Hej ”accepterar”-meddelande skickas av hello service toohello lyssnare över tidigare upprättad kontrollkanalen som ett JSON-meddelande i en WebSocket ram. Det finns inga toothis svarsmeddelandet.

hello-meddelande innehåller en JSON-objekt med namnet ”accepterar”, som definierar hello följande egenskaper just nu:

* **adress** – hello URL: en sträng toobe används för att upprätta hello WebSocket toothe service tooaccept en inkommande anslutning.
* **ID** – hello Unik identifierare för den här anslutningen. Om hello-ID har angetts av hello avsändaren klienten är hello avsändaren angivna värdet, annars är det ett värde som genereras av systemet.
* **connectHeaders** – alla HTTP-huvuden som har angetts toohello Relay endpoint hello avsändaren, vilket även innefattar hello WebSocket-s-protokollet och WebSocket-s-tillägg-rubriker.

#### Acceptera meddelandet

```json
{                                                           
    "accept" : {
        "address" : "wss://168.61.148.205:443/$hc/{path}?..."    
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",  
        "connectHeaders" : {                                         
            "Host" : "...",                                                
            "Sec-WebSocket-Protocol" : "...",                              
            "Sec-WebSocket-Extensions" : "..."                             
        }                                                            
     }                                                         
}
```

hello adress URL anges i hello JSON meddelandet används av hello lyssnare upprätta hello WebSocket för att godkänna eller avvisa hello avsändaren socket.

#### Acceptera hello socket
tooaccept, hello lyssnare upprättar en WebSocket-adress för anslutning toohello tillhandahålls.

Om hello ”accepterar” meddelande utför en `Sec-WebSocket-Protocol` rubrik som är det förväntas att hello-lyssnaren accepterar bara hello WebSocket om den har stöd för protokollet. Dessutom anger hello-huvud som hello WebSocket har upprättats.

hello samma gäller toohello `Sec-WebSocket-Extensions` huvud. Om hello framework stöder ett tillägg, ska den in hello huvud toohello serversidan svar av hello krävs `Sec-WebSocket-Extensions` handskakning för hello tillägg.

hello URL måste användas som-är för att fastställa hello acceptera socket, men innehåller följande parametrar:

| Parameter | Krävs | Beskrivning |
| --- | --- | --- |
| `sb-hc-action` |Ja |För att acceptera en socket måste hello-parametern vara`sb-hc-action=accept` |
| `{path}` |Ja |(se följande stycke hello) |
| `sb-hc-id` |Nej |Se föregående beskrivning av **id**. |

`{path}`är hello URL-kodade namnområdessökväg av hello förkonfigurerade Hybridanslutning på vilka tooregister den här lyssnaren. Det här uttrycket är tillagda toothe fast `$hc/` sökvägsdelen. 

Hej `path` uttryck kan utökas med ett suffix och ett stränguttryck för frågan som följer hello registrerade namn efter ett separertratten snedstreck. Detta gör hello avsändaren klienten toopass dispatch argument toohello accepterar lyssnare när det inte är möjligt tooinclude HTTP-huvuden. hello förväntan är att hello-lyssnaren framework analyserar hello fast sökvägsdelen och hello registrerade namnet från sökvägen och gör hello resten eventuellt utan någon fråga strängargument föregås av `sb-`, tillgängliga toohello program för ska om tooaccept hello anslutning.

Mer information finns i hello efter ”avsändaren protokoll” avsnittet.

Om det finns ett fel, kan hello-tjänsten svara på följande sätt:

| Kod | Fel | Beskrivning |
| --- | --- | --- |
| 403 |Tillåts inte |hello URL är inte giltig. |
| 500 |Internt fel |Det uppstod ett fel i hello-tjänsten |

Hello-anslutning har upprättats, stängs hello servern när hello WebSocket när hello avsändaren WebSocket stänger, eller med hello följande status:

| WS-Status | Beskrivning |
| --- | --- |
| 1001 |hello avsändaren klienten stängs hello-anslutning. |
| 1001 |hello Hybridanslutning sökvägen har tagits bort eller inaktiverats. |
| 1008 |hello säkerhets-token har upphört att gälla, därför hello auktoriseringsprincip överskrids. |
| 1011 |Något gick fel i hello-tjänsten. |

#### Avvisa hello socket
Avvisa hello socket när genomför inspektionen ”accepterar” hälsningsmeddelande kräver en liknande handskakning så att hello statuskod och statusbeskrivning kommunicerar orsak till hello nekande kan flöda tillbaka toohello avsändare.

hello protokollet designvalen här är toouse en WebSocket-handskakning (som är utformad tooend i en definierad feltillstånd) så att lyssnare klientimplementeringar toorely kan fortsätta på en WebSocket-klienten och behöver inte använda en extra, utan http-klienten.

tooreject hello socket, hello klienten tar hello adress URI från ”accepterar” hello-meddelande och lägger till två frågan sträng parametrar tooit, enligt följande:

| Param | Krävs | Beskrivning |
| --- | --- | --- |
| statusCode |Ja |Numeriska HTTP-statuskod. |
| StatusDescription |Ja |Mänsklig läsbar orsak hello underkännande. |

hello resulterande URI är sedan används tooestablish WebSocket-anslutningen.

När den har slutförts korrekt, misslyckas denna handskakning avsiktligt med en HTTP-felkod 410, eftersom ingen WebSocket har upprättats. Om något går fel beskrivs hello följande koder hello-fel:

| Kod | Fel | Beskrivning |
| --- | --- | --- |
| 403 |Tillåts inte |hello URL är inte giltig. |
| 500 |Internt fel |Något gick fel i hello-tjänsten. |

### Lyssnare token förnyelse
När hello lyssnare token är om tooexpire, ersätter den den genom att skicka en text ram toohello meddelandetjänst via hello upprätta kontrollkanalen. Meddelandet innehåller en JSON-objekt som kallas `renewToken`, som definierar hello efter egenskapen just nu:

* **token** – en giltig, URL-kodade Service Bus delade åtkomsttoken för namnområdet eller Hybridanslutning som ger hello **lyssna** rätt.

#### renewToken meddelande

```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Om token hello-validering misslyckas åtkomst nekas och hello Molntjänsten stängs hello kontrollkanal WebSocket med ett fel. Annars finns inget svar.

| WS-Status | Beskrivning |
| --- | --- |
| 1008 |hello säkerhets-token har upphört att gälla, därför hello auktoriseringsprincip överskrids. |

## Avsändaren protokoll
hello avsändaren protokollet är effektivt identiska toohello sätt som en lyssnare har upprättats.
hello målet är maximalt genomskinlighet för hello slutpunkt till slutpunkt WebSocket. hello adress att koppla toois hello desamma hello lyssnare, men hello ”åtgärd” skiljer sig och token måste en annan behörighet:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

Hej *namnområde adress* är hello fullständigt kvalificerade domännamnet för hello Azure Relay namnområde att värdar hello Hybridanslutning vanligtvis av hello formulär `{myname}.servicebus.windows.net`.

hello begäran kan innehålla godtycklig extra HTTP-rubriker, inklusive programdefinierade de. Alla angivna huvuden flödet toohello lyssnare och finns på hello `connectHeader` objekt av hello **acceptera** kontroll.

hello frågealternativ sträng parametern är följande:

| Param | Krävs? | Beskrivning |
| --- | --- | --- |
| `sb-hc-action` |Ja |För hello avsändaren roll hello-parametern måste vara `action=connect`. |
| `{path}` |Ja |(se följande stycke hello) |
| `sb-hc-token` |Ja\* |hello lyssnare måste ange en giltig, URL-kodade Service Bus delad åtkomst-Token för hello namnområde eller Hybrid som ger hello **skicka** rätt. |
| `sb-hc-id` |Nej |Ett valfritt ID som möjliggör slutpunkt till slutpunkt diagnostikspårning och görs tillgängliga toohello lyssnare under hello acceptera handskakning. |

Hej `{path}` är hello URL-kodade namnområdessökväg av hello förkonfigurerade Hybridanslutning på vilka tooregister den här lyssnaren. Hej `path` uttryck kan utökas med ett suffix och en fråga sträng uttryck toocommunicate ytterligare. Om hello Hybridanslutning är registrerad under hello sökvägen `hyco`, hello `path` uttrycket kan vara `hyco/suffix?param=value&...` följt av hello frågan string-parametrar som anges här. En fullständig uttryck kan sedan vara följande:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

Hej `path` uttryck som skickas med toohello lyssnare i hello adress URI finns i ”accepterar” kontroll hälsningsmeddelande.

Om hello WebSocket-anslutningen misslyckas på grund av toohello Hybridanslutning sökvägen inte registreras, en ogiltig eller saknas token eller några andra fel tillhandahålls hello fel feedback med hjälp av hello vanlig HTTP 1.1 status feedback modellen. Statusbeskrivningen innehåller ett fel spårnings-ID som kan överföras till Azure-supportpersonal:

| Kod | Fel | Beskrivning |
| --- | --- | --- |
| 404 |Det gick inte att hitta |Hej Hybridanslutning sökväg är ogiltig eller hello grundläggande Webbadress är felaktig. |
| 401 |Behörighet saknas |hello säkerhetstoken är saknad eller skadad eller ogiltig. |
| 403 |Tillåts inte |hello säkerhetstoken är inte giltigt för den här sökvägen och för den här åtgärden. |
| 500 |Internt fel |Något gick fel i hello-tjänsten. |

Om hello WebSocket-anslutningen stängs avsiktligt av hello-tjänsten när den ursprungligen har ställts in, hello orsaken till detta så överförs med hjälp av lämplig WebSocket-protokollet felkoden tillsammans med ett beskrivande felmeddelande som innehåller också en spårnings-ID: t.

| WS-Status | Beskrivning |
| --- | --- |
| 1000 |hello lyssnare Stäng hello socket. |
| 1001 |hello Hybridanslutning sökvägen har tagits bort eller inaktiverats. |
| 1008 |hello säkerhets-token har upphört att gälla, därför hello auktoriseringsprincip överskrids. |
| 1011 |Något gick fel i hello-tjänsten. |

## Nästa steg
* [Vanliga frågor och svar om Relay](relay-faq.md)
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)

