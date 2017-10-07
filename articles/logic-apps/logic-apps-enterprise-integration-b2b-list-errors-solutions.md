---
title: "Logic Apps B2B lista över fel och lösningar: Azure App Service | Microsoft Docs"
description: "Logic Apps B2B lista över fel och lösningar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 0340e2979f1972ba631354e206c93969e55946e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a>Logic Apps B2B lista över fel och lösningar  
Den här artikeln hjälper dig att felsöka problem som kan inträffa i Logic Apps B2B-scenarier och ger förslag på åtgärder för att korrigera felen.


## <a name="agreement-resolution"></a>Avtalet upplösning

### <a name="no-agreement-found"></a>* Inget avtal hittades 

|   |   |  
|---|---|
| Felbeskrivning | Inget avtal med avtal upplösning parametrar hittades|    
| Användaråtgärd | hello avtal bör läggas toohello integrering konto med överenskomna business identiteter.</br> hello business identiteter ska matcha toohello meddelande-ID: n|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a>* Inget avtal hittades med identiteter

|   |   | 
|---|---|
| Felbeskrivning | Inget avtal med identiteter hittades: 'AS2Identity':: 'Partner1' och 'AS2Identity':: 'Partner3'| 
| Användaråtgärd | Ogiltig AS2-från eller AS2-tooconfigured för avtalet. </br> Rätt AS2 meddelandet AS2-från eller AS2-tooheaders eller avtal toomatch AS2-ID: n i AS2-meddelandet huvuden med avtal konfigurationer |
|   |   |     

## <a name="as2"></a>AS2

### <a name="-missing-as2-message-headers"></a>* Saknas AS2-meddelandehuvuden  

|   |   |  
|---|---|
| Felbeskrivning| Ogiltig AS2-huvuden. En av ' AS2-att ' eller ' AS2-från-huvuden är tom| 
| Användaråtgärd | Ett AS2-meddelande togs emot som inte innehåller hello AS2-från eller AS2-tooor båda huvuden. </br> Kontrollera AS2-meddelande AS2-från och AS2-tooheaders och åtgärda dem baserat på konfigurationen av avtalet |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a>* Saknas AS2 meddelandetexten och rubriker    

|   |   |  
|---|---|
| Felbeskrivning| hello begär innehåll är null eller tomt | 
| Användaråtgärd | Ett AS2-meddelande togs emot som inte innehåller hello meddelandetexten |
|  |  | 

### <a name="-as2-message-decryption-failure"></a>* Krypteringsfel för AS2-meddelande

|   |   | 
|---|---|
| Felbeskrivning |  [bearbetas/fel: dekryptering misslyckades] | 
| Användaråtgärd | Lägg till @base64ToBinary tooAS2Message innan du skickar toopartner 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a>* MDN krypteringsfel

|   |   | 
|---|---|
| Felbeskrivning |  [bearbetas/fel: dekryptering misslyckades] | 
| Användaråtgärd | Lägg till @base64ToBinary tooMDN innan du skickar toopartner 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a>* Saknas signeringscertifikat

|   |   |  
|---|---|
| Felbeskrivning| hello har signeringscertifikat inte konfigurerats för AS2 part. </br> AS2-från: partner1 AS2-till: partner2 | 
| Användaråtgärd | Konfigurera inställningar för AS2-avtal med rätt certifikat för signatur |
|  |  | 

## <a name="x12-and-edifact"></a>X12 och EDIFACT

### <a name="-leading-or-trailing-space-found"></a>* Inledande eller avslutande blanksteg hittades    
    
|   |   | 
|---|---|
| Felbeskrivning | Ett fel uppstod vid parsning. Hej Edifact-transaktion med id ' 123456 'som finns i interchange (utan grupp) med id ' 987654', med avsändar-id 'Partner1', mottagaren id 'Partner2' pausas med följande fel: inledande avslutande avgränsare hittades |
| Användaråtgärd | hello avtal inställningar toobe konfigurerad tooallow inledande och avslutande blanksteg. </br> Redigera avtal inställningar tooallow inledande och avslutande blanksteg |
|   |   |

![ge utrymme](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-hello-agreement"></a>* Kontrollen av dubblett har aktiverats i hello avtal

|   |   | 
|---|---| 
| Felbeskrivning | Duplicera kontroll tal |
| Användaråtgärd | Det här felet indikerar emot hälsningsmeddelande har dubbla kontrollen siffror. </br> Korrigera hello kontrollen antalet och skicka hello-meddelande |
|   |   |

### <a name="-missing-schema-in-hello-agreement"></a>* Saknas schemat i hello avtal

|   |   | 
|---|---| 
| Felbeskrivning | Ett fel uppstod vid parsning. Hej X12 transaktion uppsättningen med id '564220001' finns i funktionella grupp med id '56422', i utbyte med id '000056422' med ' 12345678-avsändar-id', mottagaren id ' 87654321 ' pausas med följande fel ”hello-meddelande har ett okänt dokument ty PE och gick inte att tolka tooany hello befintliga scheman som konfigureras i hello avtalet ” |
| Användaråtgärd | Konfigurera schemat i hello avtal inställningar  |
|   |   |

### <a name="-incorrect-schema-in-hello-agreement"></a>* Felaktigt schema i hello avtal

|   |   | 
|---|---| 
| Felbeskrivning | hello-meddelande har en okänd dokumenttyp och gick inte att tolka tooany hello befintliga scheman som konfigureras i hello avtal. |
| Användaråtgärd | Konfigurera rätt schema i hello avtal inställningar  |
|   |   |

## <a name="flat-file"></a>Flat-fil

### <a name="-input-message-with-no-body"></a>* Indatameddelande med ingen brödtext

|   |   | 
|---|---|
| Felbeskrivning | InvalidTemplate. Det går inte tooprocess mallspråksuttryck i indata för åtgärden 'Flat_File_Decoding' på rad '1'- och '1902': ' krävs för egenskapen 'content-förväntar sig ett värde men erhöll null. Sökvägen ''.'. |
| Användaråtgärd | Det här felet indikerar hello inkommande meddelandet inte innehåller en text |
|   |   | 

## <a name="learn-more"></a>Läs mer
[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md)
