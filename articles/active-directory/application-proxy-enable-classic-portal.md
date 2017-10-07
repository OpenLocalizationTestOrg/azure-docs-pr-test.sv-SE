---
title: aaaEnable Azure AD Application Proxy i hello klassiska portal | Microsoft Docs
description: "Aktivera Application Proxy i hello klassiska Azure-portalen och installera hello kopplingar för hello omvänd proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 8be9416a61993e1b46a20152e172c5133e54c0a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-application-proxy-in-hello-classic-portal-and-download-connectors"></a>Aktivera Application Proxy i hello klassiska portalen och hämta kopplingar
Den här artikeln vägleder dig genom hello steg tooenable Microsoft Azure AD Application Proxy för din molnkatalog i Azure AD.

Om du är bekant med vad Application Proxy kan hjälpa dig, lär du dig mer om [hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Krav för Application Proxy
Innan du kan aktivera och använda Application Proxy-tjänster, måste du toohave:

* En [Basic- eller Premium-prenumeration på Microsoft Azure AD](active-directory-editions.md) och en Azure AD-katalog som du är en global administratör för.
* En server som kör Windows Server 2012 R2 eller 2016, där du kan installera hello Application Proxy Connector. hello-servern skickar begäranden toohello Application Proxy-tjänster i hello molnet, och måste därför ett HTTP eller HTTPS-anslutning toohello program som du publicerar.
  * För enkel inloggning tooyour publicerade program, den här datorn måste vara ansluten till domänen i hello samma AD-domän som hello-program som du publicerar. Mer information finns i [enkel inloggning med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
* Om din organisation använder proxy servrar tooconnect toohello internet, läsa [fungerar med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md) mer information om hur tooconfigure dem.

## <a name="open-your-ports"></a>Öppna portar

tooprepare din miljö för Azure AD Application Proxy måste du först tooenable kommunikation tooAzure datacenter. Om det finns en brandvägg i hello sökväg, kontrollera att den är öppen så att anslutningsverktyget kan göra HTTPS (TCP) hello begär toohello Application Proxy.

1. Öppna hello följande portar för**utgående** trafik:

   | Portnummer | Hur den används |
   | --- | --- |
   | 80 | Hämta certifikatåterkallning listor över återkallade under validering av hello SSL-certifikat |
   | 443 | All utgående kommunikation med hello Application Proxy-tjänsten |

   Om brandväggen tillämpar trafik enligt toooriginating användare, öppna dessa portar för trafik som kommer från Windows-tjänsterna körs som Nätverkstjänst.

   > [!IMPORTANT]
   > hello tabellen visar hello portkrav för koppling versioner 1.5.132.0 och senare. Om du fortfarande har en äldre version av anslutningen måste du också tooenable hello följande portar: 5671, 8080, 9090, 9091, 9350, 9352 och 10100 – 10120.
   >
   >Information om hur du uppdaterar din kopplingar toohello senaste versionen finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md#automatic-updates).

2. Om din brandvägg eller proxyserver kan vitlistning av DNS kan du godkända anslutningar toomsappproxy.net och servicebus.windows.net. Om inte, du behöver tooallow åtkomst toohello [Azure DataCenter-IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653), som uppdateras varje vecka.

3. Använd hello [Azure AD Application Proxy Connector portar Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify att din connector kan nå hello Application Proxy-tjänsten. Se till att hello centrala USA region och hello region närmaste tooyou har alla gröna bockmarkeringarna minimum. Utöver det kan innebär mer gröna bockmarkeringarna större flexibilitet.

## <a name="enable-application-proxy-in-azure-ad"></a>Aktivera Application Proxy i Azure AD
1. Logga in som administratör i hello [klassiska Azure-portalen](https://manage.windowsazure.com/).
2. Gå tooActive Directory och välj hello katalog där du vill tooenable Application Proxy.

    ![Active Directory – ikon](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Välj **konfigurera** från hello katalogsidan och rulla ned för**Application Proxy**.
4. Växla **aktivera Programproxytjänster för den här katalogen** för**aktiverad**.

    ![Aktivera Application Proxy](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Välj **Hämta nu**. Hej **Azure AD Application Proxy Connector hämta** öppnas. Läs och acceptera licensvillkoren för hello och klicka på **hämta** toosave hello Windows Installer-fil (.exe) för hello-anslutningen.

## <a name="install-and-register-hello-connector"></a>Installera och registrera hello Connector
1. Kör **AADApplicationProxyConnectorInstaller.exe** på hello servern du förberett bl.a toohello krav.
2. Följ instruktionerna för hello i hello guiden tooinstall.
3. Du kan ange tooregister hello koppling med hello programproxyn för din Azure AD-klient under installationen.

   * Ange dina autentiseringsuppgifter som global Azure AD-administratör. Autentiseringsuppgifterna för klienten som du är global administratör för kan skilja sig från dina Microsoft Azure-autentiseringsuppgifter.
   * Kontrollera att Hej administratör som registrerar hello kopplingen har hello samma katalog som du har aktiverat hello Application Proxy-tjänsten. Till exempel om hello klientdomänen är contoso.com, Hej administratör vara admin@contoso.com eller ett annat alias för domänen.
   * Om **Förbättrad säkerhetskonfiguration** har angetts för**på** på hello server hello registreringsskärmen blockeras. tooallow åtkomst, hello anvisningar i hello felmeddelande. Kontrollera att Förbättrad säkerhetskonfiguration i Internet Explorer är inaktiverat.
   * Om registreringen av anslutningsappen inte lyckas så gå till [Felsöka programproxyn](active-directory-application-proxy-troubleshoot.md).  
4. När hello installationen är klar läggs två nya tjänster tooyour server:

   * **Microsoft AAD Application Proxy Connector** upprättar anslutningarna.

     * **Microsoft AAD Application Proxy Connector Updater** är en tjänst för automatiska uppdateringar. Med jämna mellanrum söker efter nya versioner av hello connector och uppdaterar hello connector efter behov.

     ![Application Proxy Connector-tjänster – skärmbild](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Klicka på **Slutför** i hello installationsfönstret.

Information om anslutningsappar finns i avsnittet [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md) (Förstå anslutningsappar för Azure AD Application Proxy).

Om du vill upprätthålla hög tillgänglighet bör du distribuera minst två anslutningsappar. toodeploy flera kopplingar, Upprepa steg 2 och 3. Varje anslutningsapp måste registreras separat.

Om du vill toouninstall hello koppling avinstallerar du både hello kopplingstjänsten och hello uppdateringstjänsten. Starta om datorn toofully ta bort hello tjänsten.

## <a name="next-steps"></a>Nästa steg
Du är nu redo för[publicera program med programproxy](active-directory-application-proxy-publish.md).

Om du har program som finns i separata nätverk eller olika platser, kan du använda anslutningstjänsten grupper tooorganize hello olika kopplingar till logiska enheter. Läs mer i [Arbeta med programproxyanslutningar](active-directory-application-proxy-connectors.md).
