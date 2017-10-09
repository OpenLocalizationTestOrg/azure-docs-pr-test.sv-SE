---
title: "aaaHow tooopen hello brandväggsportar krävs för ett program med Application Proxy | Microsoft Docs"
description: "Ta reda på vilka portar tooopen för hello Azure AD Application Proxy toowork korrekt"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cdc7badb7c15591689a3bfd6bb26da182b00fb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-hello-firewall-ports-required-for-an-application-proxy-application"></a>Hur tooopen hello brandväggsportar som krävs för ett program med Application Proxy

toosee en fullständig lista över hello krävs portar och hello funktionerna i varje port finns hello förutsättningar i hello [Application Proxy dokumentationen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable). Observera att Application Proxy endast använder utgående portar.

Du kan också kontrollera om du har alla hello krävs portar öppna genom att öppna hello [anslutningsverktyget portar Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) från ditt lokala nätverk. Flera gröna bockmarkeringarna innebär större flexibilitet. 

## <a name="app-proxy-regions"></a>Appen Proxy regioner

Vi arbetar på ett sätt som du vet vilken av dessa regioner toolet måste toobe grön du. Kontrollera att alla är för tillfället. Centrala USA krävs oavsett vilken region som du befinner dig i.

toomake att hello verktyget ger du hello rätt resultat måste du kontrollera att:

-   Öppna hello-verktyget i en webbläsare från hello server där du har installerat hello Connector.

-   Kontrollera att alla proxyservrar och brandväggar tillämpliga tooyour Connector är också används toothis sidan. Detta kan göras i Internet Explorer genom att gå för**inställningar**  - &gt; **Internetalternativ**  - &gt; **anslutningar**  - &gt; **Lan-inställningar**. På den här sidan kan se du hello fältet ”Använd en Proxy-Server för ditt LAN”. Välj den här rutan och placera hello proxyadress i hello ”adress” fält.

## <a name="next-steps"></a>Nästa steg
[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
