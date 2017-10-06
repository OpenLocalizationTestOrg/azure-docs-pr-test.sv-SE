---
title: installera aaaProblem hello Application Proxy Agent Connector | Microsoft Docs
description: "Hur tootroubleshoot problem som du kan hello står inför när du installerar Application Proxy Agent Connector"
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
ms.openlocfilehash: 07ac366a429083af0c9b87aa9df9cf3876132b90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="problem-installing-hello-application-proxy-agent-connector"></a>Problem med att installera hello Application Proxy Agent Connector

Microsoft AAD Application Proxy Connector är en intern domän-komponent som använder utgående anslutningar tooestablish hello anslutningen från hello molnet tillgängliga endpoint toohello interna domänen.

## <a name="general-problem-areas-with-connector-installation"></a>Allmänna problemområden med kopplingsinstallationen

När hello installationen av en anslutning misslyckas, är hello orsaken oftast en av hello följande områden:

1.  **Anslutningen** – toocomplete en lyckad installation hello nya connector måste tooregister och upprätta förtroende för framtida egenskaper. Detta görs genom att ansluta toohello AAD Application Proxy-Molntjänsten.

2.  **Upprätta förtroende** – hello ny koppling skapar ett självsignerat certifikat och registrerar toohello Molntjänsten.

3.  **Autentisering av Hej administratör** – under installationen hello måste användaren ange administratörsautentiseringsuppgifter toocomplete hello Connector-installationen.

## <a name="verify-connectivity-toohello-cloud-application-proxy-service-and-microsoft-login-page"></a>Kontrollera anslutningen toohello moln Application Proxy-tjänsten och Microsoft Login-sida

**Mål:** verifiera att hello connector datorn kan ansluta toohello AAD Application Proxy registrering slutpunkt som Microsoft-inloggningssidan.

1.  Öppna en webbläsare och gå toohello följande webbsida: <https://aadap-portcheck.connectorporttest.msappproxy.net> , och kontrollera att hello anslutningen tooCentral USA och östra USA datacenter med portarna 9090 och 9091 fungerar.

2.  Om någon av dessa portar inte lyckas (om du inte har en grön bock), verifiera att hello brandväggen eller backend-proxy har \*. msappproxy.net med portarna 9090 och 9091 definierats korrekt.

3.  Öppna en webbläsare (separat flik) och gå toohello följande webbsida: <https://login.microsoftonline.com>, se till att du loggar in toothat sidan.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Kontrollera att dator-och backend-stöd för Application Proxy betrodda certifikat

**Mål:** Kontrollera att hello connector datorn, backend-proxy och brandväggen stöder hello-certifikat som skapas av hello connector för framtida förtroende.

>[!NOTE]
>hello-koppling försöker toocreate ett SHA512 certifikat som stöds av TLS1.2. Om datorn hello eller hello backend brandväggar och proxyservrar inte stöder TLS1.2 hello installationen misslyckas.
>
>

**tooresolve hello problem:**

1.  Kontrollera hello datorn stöder TLS1.2 – TLS 1.2 ska ha stöd för alla Windows-versioner när 2012 R2. Om datorn connector är från en version av 2012 R2 eller tidigare, kontrollerar du att hello följande KBs är installerade på datorn hello: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Kontakta administratören för nätverket och be tooverify att hello backend proxy- och brandväggsinställningarna inte blockerar SHA512 för utgående trafik.

## <a name="verify-admin-is-used-tooinstall-hello-connector"></a>Kontrollera admin används tooinstall hello-koppling

**Mål:** kontrollerar du att hello-användaren som försöker tooinstall hello connector är en administratör med rätt autentiseringsuppgifter. För närvarande måste hello användaren vara en global administratör för hello installation toosucceed.

**tooverify hello autentiseringsuppgifterna är korrekta:**

Ansluta för<https://login.microsoftonline.com> och använda hello samma autentiseringsuppgifter. Kontrollera att hello inloggningen är klar. Du kan kontrollera hello användarroll genom att gå för**Azure Active Directory**  - &gt; **användare och grupper**  - &gt; **alla användare**. 

Välj ditt användarkonto, sedan ”Directory roll” hello resulterande menyn. Verifiera den valda rollen hello är ”Global administratör”. Om du tooaccess någon hello sidor längs de här stegen kan är du inte en global administratör.

## <a name="next-steps"></a>Nästa steg
[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
