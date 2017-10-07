---
title: "aaaRDG och Azure MFA-Server med hjälp av RADIUS | Microsoft Docs"
description: "Det här är hello Azure Multi-Factor authentication sida som hjälper distribuera Gateway för fjärrskrivbord (RD) och Azure Multi-Factor Authentication-servern med hjälp av RADIUS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: fd280e9b6ff90c82927cffe593c4f1fda7047842
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Fjärrskrivbordsgateway och Azure Multi-Factor Authentication Server med RADIUS
Gateway för fjärrskrivbord (RD) använder ofta hello lokala tjänster NPS (Network Policy) tooauthenticate användare. Den här artikeln beskriver hur tooroute RADIUS-förfrågningar ut från hello Remote Desktop Gateway (via hello lokala NPS) toohello Multi-Factor Authentication-servern. hello kombination av Azure MFA och fjärrskrivbordsgateway innebär att användarna kan komma åt sina arbetsmiljöer från valfri plats när du utför stark autentisering. 

Eftersom Windows-autentisering för terminal services inte stöds för Server 2012 R2, kan du använda fjärrskrivbordsgateway och RADIUS-toointegrate med MFA-servern. 

Installera hello Azure Multi-Factor Authentication-servern på en separat server, vilken proxyservrar hello RADIUS begäran tillbaka toohello NPS på hello servern för fjärrskrivbordsgateway. När NPS validerar hello användarnamn och lösenord, returnerar ett svar toohello Multi-Factor Authentication-servern. Hej MFA-servern utför hello andra faktor autentisering och returnerar resultatet toohello gateway.

## <a name="prerequisites"></a>Krav

- En domänansluten Azure MFA-server. Om du inte har något installerat redan gör hello i [komma igång med hello Azure Multi-Factor Authentication-servern](multi-factor-authentication-get-started-server.md).
- En fjärrskrivbordsgateway som autentiserar med Network Policy Services.

## <a name="configure-hello-remote-desktop-gateway"></a>Konfigurera hello servern för fjärrskrivbordsgateway
Konfigurera hello RD Gateway toosend RADIUS-autentisering tooan Azure Multi-Factor Authentication-servern. 

1. I hanteraren för fjärrskrivbordsgateway, högerklicka på hello servernamn och välj **egenskaper**.
2. Gå toohello **auktoriseringsprincip för fjärrskrivbordsanslutning** fliken och markera **centrala server som kör NPS**. 
3. Lägga till en eller flera Azure Multi-Factor Authentication-servrar som RADIUS-servrar genom att ange hello namn eller IP-adressen för varje server. 
4. Skapa en delad hemlighet för varje server.

## <a name="configure-nps"></a>Konfigurera NPS
hello fjärrskrivbordsgateway använder NPS toosend hello RADIUS-begäran tooAzure Multi-Factor Authentication. tooconfigure NPS först kan du ändra hello timeout-inställningar tooprevent hello RD Gateway avbryts innan hello tvåstegsverifiering har slutförts. Sedan kan uppdatera du NPS tooreceive RADIUS-autentiseringar från MFA-servern. Använd följande procedur tooconfigure NPS hello:

### <a name="modify-hello-timeout-policy"></a>Ändra hello timeout-princip

1. Öppna hello i NPS **RADIUS-klienter och Server** -menyn i vänster kolumn och markera hello **fjärranslutna RADIUS-servergrupper**. 
2. Välj hello **TS GATEWAY-SERVERGRUPP**. 
3. Gå toohello **belastningsutjämning** fliken. 
4. Ändra både hello **antal sekunder utan svar innan begäran anses som utelämnats** och hello **antalet sekunder mellan begäranden när servern anses vara otillgänglig** toobetween 30 och 60 sekunder. (Om du hittar den hello-servern gör timeout under autentiseringen fortfarande kan du återvända hit. öka hello antal sekunder)
5. Gå toohello **konto och autentisering** och kontrollerar att hello RADIUS-portar har angetts för att matcha hello portarna som hello Multi-Factor Authentication-servern lyssnar på.

### <a name="prepare-nps-tooreceive-authentications-from-hello-mfa-server"></a>Förbereda NPS tooreceive autentiseringar från hello MFA-Server

1. Högerklicka på **RADIUS-klienter** under RADIUS-klienter och servrar i vänster kolumn och markera hello **ny**.
2. Lägg till hello Azure Multi-Factor Authentication-servern som en RADIUS-klient. Välj ett eget namn och ange en delad hemlighet.
3. Öppna hello **principer** -menyn i vänster kolumn och markera hello **principer för anslutningsbegäran**. Nu kan du se en princip med namnet TS GATEWAY AUTHORIZATION POLICY (TS GATEWAY-AUKTORISERINGSPRINCIP) som skapades när Fjärrskrivbordsgateway konfigurerades. Den här principen vidarebefordrar RADIUS-begäranden toohello Multi-Factor Authentication-servern.
4. Högerklicka på **TS Gateway-auktoriseringsprincipen** och välj **Duplicera princip**. 
5. Öppna hello ny princip och gå toohello **villkor** fliken.
6. Lägg till ett villkor som matchar hello eget namn för klient med hello egna namn som i steg 2 för hello Azure Multi-Factor Authentication Server RADIUS-klient. 
7. Gå toohello **inställningar** fliken och markera **autentisering**.
8. Ändra hello autentiseringsprovider för**autentisera begäranden på den här servern**. Den här principen ser till att när NPS tar emot en RADIUS-begäran från hello Azure MFA-Server, hello-autentisering sker lokalt skickas i stället för en RADIUS-begäran tillbaka toohello Azure Multi-Factor Authentication-servern, vilket skulle resultera i en loop-villkoret. 
9. tooprevent en loop-villkor, kontrollera att hello ny princip sorterade ovan hello ursprungliga principen i hello **principer för anslutningsbegäran** fönstret.

## <a name="configure-azure-multi-factor-authentication"></a>Konfigurera Azure Multi-Factor Authentication

hello Azure Multi-Factor Authentication-servern är konfigurerad som en RADIUS-proxy mellan fjärrskrivbordsgateway och NPS.  Den bör installeras på en domänansluten server som är separat från hello RD Gateway-servern. Använd hello följa proceduren tooconfigure hello Azure Multi-Factor Authentication-servern.

1. Öppna hello Azure Multi-Factor Authentication-servern och välj hello ikon för RADIUS-autentisering. 
2. Kontrollera hello **aktivera RADIUS-autentisering** kryssrutan.
3. På fliken för hello klienter säkerställa hello portar matchar det som har konfigurerats i NPS och välj sedan **Lägg till**.
4. Lägg till hello RD Gateway-serverns IP-adress, namn (valfritt) och en delad hemlighet. hello delade hemliga behov toobe hello samma på både hello Azure Multi-Factor Authentication-servern och fjärrskrivbordsgateway.
3. Gå toohello **mål** fliken och väljer hello **RADIUS-servrar** knappen.
4. Välj **Lägg till** och ange hello IP-adress, delad hemlighet och portar för hello NPS-servern. Om du inte använder en central NPS är hello RADIUS-klienten och RADIUS-mål hello samma. hello delad hemlighet måste matcha hello en inställning i hello RADIUS-klienten avsnitt i hello NPS-servern.

![RADIUS-autentisering](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="next-steps"></a>Nästa steg

- Integrera Azure MFA och [IIS-webbappar](multi-factor-authentication-get-started-server-iis.md)

- Få svar i hello [Azure Multi-Factor Authentication vanliga frågor och svar](multi-factor-authentication-faq.md)
