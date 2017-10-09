---
title: "aaaAzure AD App Proxy – börja installera connector | Microsoft Docs"
description: "Aktivera Application Proxy i hello Azure-portalen och installera hello kopplingar för hello omvänd proxy."
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
ms.date: 08/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ea79ffa92fa223584be04f49019fd31a0bfd8358
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-application-proxy-and-install-hello-connector"></a>Komma igång med Application Proxy och installera hello-koppling
Den här artikeln vägleder dig genom hello steg tooenable Microsoft Azure AD Application Proxy för din molnkatalog i Azure AD.

Om du inte är medveten om hello fördelar för säkerhet och produktivitet Application Proxy ger tooyour organisation, lär du dig mer om [hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Krav för Application Proxy
Innan du kan aktivera och använda Application Proxy-tjänster, måste du toohave:

* En [Basic- eller Premium-prenumeration på Microsoft Azure AD](active-directory-editions.md) och en Azure AD-katalog som du är en global administratör för.
* En server som kör Windows Server 2012 R2 eller 2016, där du kan installera hello Application Proxy Connector. hello-servern måste toobe kan tooconnect toohello Application Proxy-tjänster i molnet hello och hello lokala program som du publicerar.
  * För enkel inloggning tooyour ska publicerade program med hjälp av Kerberos-begränsad delegering, den här datorn vara ansluten till domänen i hello samma AD-domän som hello-program som du publicerar. Mer information finns i [KCD för enkel inloggning med Application Proxy](active-directory-application-proxy-sso-using-kcd.md).

Om din organisation använder proxy servrar tooconnect toohello internet, läsa [fungerar med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md) mer information om hur tooconfigure dem innan du få igång med Application Proxy.

## <a name="open-your-ports"></a>Öppna portar

tooprepare din miljö för Azure AD Application Proxy måste du först tooenable kommunikation tooAzure datacenter. Om det finns en brandvägg i hello sökväg, kontrollera att den är öppen så att anslutningsverktyget kan göra HTTPS (TCP) hello begär toohello Application Proxy.

1. Öppna hello följande portar för**utgående** trafik:

   | Portnummer | Hur den används |
   | --- | --- |
   | 80 | Hämta certifikatåterkallning listor över återkallade under validering av hello SSL-certifikat |
   | 443 | All utgående kommunikation med hello Application Proxy-tjänsten |

   Om brandväggen tillämpar trafik enligt toooriginating användare, kan du öppna dessa portar för trafik från Windows-tjänster som körs som Nätverkstjänst.

   > [!IMPORTANT]
   > hello tabellen visar hello portkrav för koppling versioner 1.5.132.0 och senare. Om du fortfarande har en äldre version av anslutningen måste du också tooenable hello följande portar i tillägget too80 och 443:5671, 8080 9090 9091 9350, 9352 10100 – 10120.
   >
   >Information om hur du uppdaterar din kopplingar toohello senaste versionen finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md#automatic-updates).

2. Om din brandvägg eller proxyserver kan vitlistning av DNS kan du godkända anslutningar toomsappproxy.net och servicebus.windows.net. Om inte, du behöver tooallow åtkomst toohello [Azure DataCenter-IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653), som uppdateras varje vecka.

3. Microsoft använder fyra adresser tooverify certifikat. Tillåt åtkomst toohello följande URL: er om du inte gjort det för andra produkter:
   * mscrl.microsoft.com:80
   * CRL.microsoft.com:80
   * OCSP.msocsp.com:80
   * www.microsoft.com:80

4. Din anslutningstjänst behöver åtkomst toologin.windows.net och login.microsoftonline.net för hello registreringsprocessen.

5. Använd hello [Azure AD Application Proxy Connector portar Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify att din connector kan nå hello Application Proxy-tjänsten. Se till att hello centrala USA region och hello region närmaste tooyou har alla gröna bockmarkeringarna minimum. Utöver det kan innebär mer gröna bockmarkeringarna större flexibilitet.

## <a name="install-and-register-a-connector"></a>Installera och registrera en koppling
1. Logga in som administratör i hello [Azure-portalen](https://portal.azure.com/).
2. Den aktuella katalogen visas under ditt användarnamn i hello övre högra hörnet. Om du behöver toochange kataloger, väljer du ikonen.
3. Gå för**Azure Active Directory** > **Application Proxy**.

   ![Navigera tooApplication Proxy](./media/active-directory-application-proxy-enable/app_proxy_navigate.png)

4. Välj **hämta Connector**.

   ![Hämta Connector](./media/active-directory-application-proxy-enable/download_connector.png)

5. Kör **AADApplicationProxyConnectorInstaller.exe** på hello servern du förberett bl.a toohello krav.
6. Följ instruktionerna för hello i hello guiden tooinstall. Du kan ange tooregister hello koppling med hello programproxyn för din Azure AD-klient under installationen.

   * Ange dina autentiseringsuppgifter som global Azure AD-administratör. Autentiseringsuppgifterna för klienten som du är global administratör för kan skilja sig från dina Microsoft Azure-autentiseringsuppgifter.
   * Kontrollera att Hej administratör som registrerar hello kopplingen har hello samma katalog som du har aktiverat hello Application Proxy-tjänsten. Till exempel om hello klientdomänen är contoso.com, Hej administratör vara admin@contoso.com eller ett annat alias för domänen.
   * Om **Förbättrad säkerhetskonfiguration** har angetts för**på** på hello server där du installerar hello koppling, kanske inte visas registrering hello-skärmen. tooget åtkomst, hello anvisningar i hello felmeddelande. Kontrollera att Förbättrad säkerhetskonfiguration i Internet Explorer är inaktiverat.

Om du vill upprätthålla hög tillgänglighet bör du distribuera minst två anslutningsappar. Varje anslutningsapp måste registreras separat.

## <a name="test-that-hello-connector-installed-correctly"></a>Testa hello kopplingen installerad

Du kan bekräfta att en ny koppling installerats korrekt genom att söka efter den i antingen hello Azure-portalen eller på servern. 

I hello Azure-portalen, logga in tooyour klient och navigera för**Azure Active Directory** > **Application Proxy**. Alla kopplingar och connector grupper visas på den här sidan. Välj en koppling toosee information om eller flyttas till en annan koppling grupp. 

Kontrollera hello lista över aktiva tjänster för hello koppling och hello connector updater på servern. hello två tjänster ska börja köras omedelbart, men om inte, aktivera dem: 

   * **Microsoft AAD Application Proxy Connector** upprättar anslutningarna.

   * **Microsoft AAD Application Proxy Connector Updater** är en tjänst för automatiska uppdateringar. hello updater söker efter nya versioner av hello connector och uppdateringar hello connector efter behov.

   ![Application Proxy Connector-tjänster – skärmbild](./media/active-directory-application-proxy-enable/app_proxy_services.png)

Information om kopplingar och hur de förblir in toodate finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).


## <a name="next-steps"></a>Nästa steg
Du är nu redo för[publicera program med programproxy](application-proxy-publish-azure-portal.md).

Om du har program som finns i separata nätverk eller olika platser använda anslutningstjänsten grupper tooorganize hello olika kopplingar till logiska enheter. Läs mer i [Arbeta med programproxyanslutningar](active-directory-application-proxy-connectors-azure-portal.md).
