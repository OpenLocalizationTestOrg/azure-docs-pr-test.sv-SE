---
title: aaaUnderstand Azure AD Application Proxy kopplingar | Microsoft Docs
description: Beskriver hello grunderna om Azure AD Application Proxy-kopplingar.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 294cb26803ef7cf8be9f3af0678d6d2e64f6cc14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Förstå Azure AD Application Proxy-kopplingar

Kopplingar är vad gör att Azure AD Application Proxy är möjligt. De är snabb och enkel toodeploy och underhåll och super kraftfulla. Den här artikeln beskriver vilka kopplingar är hur de fungerar och några förslag på hur toooptimize din distribution. 

## <a name="what-is-an-application-proxy-connector"></a>Vad är en Application Proxy connector

Kopplingar är förenklad agenter som sitter lokalt och underlätta hello utgående anslutning toohello Application Proxy-tjänsten. Kopplingar måste installeras på en Windows-Server som har åtkomst toohello backend-programmet. Du kan sortera kopplingar i kopplingen grupper med varje grupp som hanterar trafik toospecific program. Kopplingar belastningen automatiskt, och kan hjälpa toooptimize nätverksinfrastrukturen. 

## <a name="requirements-and-deployment"></a>Krav och distribution

toodeploy Application Proxy, behöver du minst en koppling, men vi rekommenderar två eller fler för större flexibilitet. Installera hello connector på en Windows Server 2012 R2 eller 2016-dator. hello connector måste toobe kan toocommunicate med hello Application Proxy-tjänsten samt hello lokalt program som du publicerar. 

Läs mer om hello nätverkskraven för hello-anslutningsservern [komma igång med Application Proxy och installera en koppling](active-directory-application-proxy-enable.md).

## <a name="maintenance"></a>Underhåll
hello hand kopplingar och hello tjänsten tar om alla aktiviteter som hello hög tillgänglighet. De kan läggas till eller tas bort dynamiskt. Varje gång en ny begäran kommer är routade tooone hello kopplingar som är tillgänglig. Om en koppling inte är tillgänglig, kan den inte svarar toothis trafik.

hello kopplingar är tillståndslösa och har några konfigurationsdata på hello-datorn. hello endast data lagras är hello inställningarna för att ansluta hello service och dess certifikat för serverautentisering. När de ansluter toohello service hämtar alla konfigurationsdata för hello som krävs och uppdatera den varje par minuter.

Kopplingar också avsöka hello server toofind reda på om det finns en nyare version av hello-anslutningen. Om en sådan finns, uppdateras hello kopplingar.

Du kan övervaka dina kopplingar från hello-dator som körs på, med hello händelseloggen och prestandaräknare. Eller så kan du visa deras status hello Application Proxy-sidan för hello Azure-portalen:

 ![AzureAD Application Proxy kopplingar](./media/application-proxy-understand-connectors/app-proxy-connectors.png)

Du har inte toomanually ta bort kopplingar som inte används. När du kör en koppling förblir den aktiv eftersom det ansluter toohello service. Oanvända anslutningar märks _inaktiva_ och tas bort efter 10 dagar av inaktivitet. Om du vill toouninstall en koppling, avinstallerar du dock både hello kopplingstjänsten och hello uppdateringstjänsten från hello-servern. Starta om datorn toofully ta bort hello tjänsten.

## <a name="automatic-updates"></a>Automatiska uppdateringar

Azure AD innehåller automatiska uppdateringar för alla hello kopplingar som du distribuerar. Kopplingar uppdateras automatiskt så länge hello Application Proxy Connector Updater-tjänsten körs. Om du inte ser hello Connector Updater-tjänsten på servern måste du för[installerar om din anslutning](active-directory-application-proxy-enable.md) tooget eventuella uppdateringar. 

Om du inte vill toowait för en automatisk uppdatering toocome tooyour koppling, kan du utföra en manuell uppgradering. Gå toohello [connector hämtningssidan](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download) på hello server där din koppling finns och välj **hämta**. Den här processen som aktiveras av en uppgradering för hello lokal connector. 

För klienter med flera kopplingar mål hello automatiska uppdateringar en anslutning i taget i varje grupp tooprevent driftavbrott. 

Driftstopp uppstår när din anslutningstjänst uppdateringar om:  
- Du kan bara ha en anslutning. tooavoid denna driftstopp och förbättra hög tillgänglighet rekommenderar vi att du installerar ytterligare en koppling och [skapar du en koppling grupp](active-directory-application-proxy-connectors-azure-portal.md).  
- En koppling hade hello mitten av en transaktion när hello update började. Även om hello inledande transaktion har gått förlorade måste webbläsaren ska automatiskt försök hello åtgärden eller kan du uppdatera sidan. När hello-begäran skickas, är hello trafik dirigeras tooa säkerhetskopiering connector.

## <a name="creating-connector-groups"></a>Skapa grupper för anslutningstjänsten

Kopplingen grupper kan tooassign specifika kopplingar tooserve specifika program. Du kan gruppera ett antal kopplingar och sedan tilldela varje tooa programgrupp. 

Kopplingen grupper gör det enklare toomanage stora distributioner. De kan också förbättra svarstiden för klienter som har program som finns i olika regioner, eftersom du kan skapa grupper för plats-baserad connector tooserve endast lokala program. 

toolearn mer om koppling grupper finns [publicera program på separata nätverk och platser med hjälp av grupper för anslutningstjänsten](active-directory-application-proxy-connectors-azure-portal.md).

## <a name="security-and-networking"></a>Säkerhet och nätverk

Kopplingar kan installeras var som helst på hello-nätverk som tillåter dem. toosend begäranden toohello Application Proxy-tjänsten. Vad är viktigt har hello datorn kör hello anslutningstjänsten även åtkomst tooyour appar. Du kan installera kopplingar inuti företagsnätverket eller på en virtuell dator som körs i molnet hello. Kopplingar kan köras inom en demilitariserad zon (DMZ), men det är inte nödvändigt eftersom all trafik är utgående så nätverket förblir säkra.

Kopplingar kan bara skicka utgående förfrågningar. hello utgående trafik skickas toohello Application Proxy-tjänsten och toohello publicerade program. Du har inte tooopen ingående portar eftersom trafiken flödar båda sätten när en session har upprättats. Du inte har tooset in belastningsutjämning mellan hello kopplingar eller konfigurera inkommande åtkomst via dina brandväggar. 

Mer information om hur du konfigurerar utgående brandväggsregler finns [fungerar med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md).

Använd hello [Azure AD Application Proxy Connector portar Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify att din connector kan nå hello Application Proxy-tjänsten. Se till att hello centrala USA region och hello region närmaste tooyou har alla gröna bockmarkeringarna minimum. Utöver det kan innebär mer gröna bockmarkeringarna större flexibilitet. 

## <a name="performance-and-scalability"></a>Prestanda och skalbarhet

Skala för hello Application Proxy-tjänsten är transparent, men skalan är en faktor för kopplingar. Du måste toohave tillräckligt med kopplingar toohandle högtrafik. Dock behöver du inte tooconfigure belastningsutjämning eftersom alla kopplingar i en grupp för anslutningen automatiskt belastningsutjämna.

Eftersom kopplingar är tillståndslös påverkas inte av hello antal användare eller sessioner. I stället svara de toohello antalet begäranden och deras nyttolastens storlek. Med standard webbtrafik, kan en genomsnittlig virtuell dator hantera några tusen begäranden per sekund. hello specifik kapacitet beror på hello exakt datorn egenskaper. 

prestanda för hello kopplingen bundna av processor- och nätverk. CPU-prestanda krävs för SSL-kryptering och dekryptering, men nätverk är viktiga tooget snabb anslutning toohello program hello online-tjänsten i Azure.

Minnet är däremot mindre problem för kopplingar. hello onlinetjänst hand tar om mycket hello bearbetning och alla oautentiserad trafik. Allt som kan göras i hello molnet görs i hello molnet. 

En annan faktor som påverkar prestanda är hello kvaliteten på hello nätverk mellan hello kopplingar, inklusive: 

* **Hej onlinetjänst**: långsam eller lång tidsfördröjning anslutningar toohello Application Proxy-tjänst i Azure påverkan hello prestanda för kopplingen. Anslut din organisation tooAzure med Express Route hello bästa prestanda. I annat fall har teamet nätverk, se till att anslutningar tooAzure hanteras så effektivt som möjligt. 
* **Hej backend program**: I vissa fall finns proxy mellan hello koppling och hello backend-program som kan långsamma eller förhindra anslutningar. tootroubleshoot det här scenariot, öppna en webbläsare från hello-anslutningsservern och försök tooaccess hello program. Om du kör hello kopplingar i Azure men hello program lokalt, kanske hello upplevelse inte användarna förväntar dig.
* **Hej domänkontrollanter**: om hello kopplingar utför enkel inloggning med hjälp av Kerberos-begränsad delegering kan de kontakta hello domänkontrollanter innan du skickar hello begäran toohello backend. hello kopplingar har en cache med Kerberos-biljetter, men i en miljö med upptagen hello svarstiderna hos hello domänkontrollanter kan påverka prestanda. Det här problemet är vanligare för kopplingar som körs i Azure men kommunicerar med domänkontrollanter som finns lokalt. 

Mer information om hur du optimerar nätverket finns [nätverk topologiöverväganden när du använder Azure Active Directory Application Proxy](application-proxy-network-topology-considerations.md).

## <a name="domain-joining"></a>Ansluta till domänen

Kopplingar kan köras på en dator som inte är ansluten till domänen. Om du vill använda enkel inloggning (SSO) tooapplications som använder integrerad autentisering IWA (Windows) måste dock en domänansluten dator. I det här fallet hello connector datorer måste vara domänanslutna tooa som kan utföra [Kerberos](https://web.mit.edu/kerberos) begränsad delegering åt hello användare för hello publicerade program.

Kopplingar kan också vara domänansluten toodomains eller skogar som har ett partiellt förtroende eller endast tooread-domänkontrollanter.

## <a name="connector-deployments-on-hardened-environments"></a>Kopplingen distributioner på strikt miljöer

Vanligtvis är enkla connector distribution och kräver ingen särskild konfiguration. Det finns emellertid vissa unika villkor som ska övervägas:

* Organisationer som begränsar hello utgående trafik måste [öppna portar som krävs](active-directory-application-proxy-enable.md#open-your-ports).
* FIPS-kompatibla datorer kan vara nödvändiga toochange deras konfiguration tooallow hello connector processer toogenerate och lagra ett certifikat.
* Organisationer som låser deras miljö baserat på hello processer som problemet hello nätverk begäranden har toomake till att båda connector-tjänster är aktiverade tooaccess alla krävs portar och IP-adresser.
* I vissa fall utgående vidarebefordra proxyservrar Bryt hello dubbelriktat certifikatautentisering och orsaka hello kommunikation toofail.

## <a name="connector-authentication"></a>Kopplingen autentisering

tooprovide en säker tjänst kopplingar tooauthenticate mot hello-tjänsten, och hello-tjänsten har tooauthenticate mot hello-anslutningen. Den här autentiseringen görs med hjälp av klient- och servercertifikat när hello kopplingar initierar hello-anslutningen. Det här sättet hello administratörens användarnamn och lösenord lagras inte på hello connector datorn.

hello-certifikat som används är särskilda toohello Application Proxy-tjänsten. De skapas under hello första registreringen och automatiskt förnyas hello kopplingar varje par månader. 

Om en anslutning inte är ansluten toohello tjänsten för flera månader, certifikat kan vara inaktuell. I det här fallet avinstallera och installera hello connector tootrigger registrering. Du kan köra hello följande PowerShell-kommandon:

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-hello-hood"></a>Under huven hello

Kopplingar är baserade på Windows Server Web Application Proxy, så de har de flesta hello samma hanteringsverktyg, inklusive Windows-händelseloggar

 ![Hantera händelseloggar med hello Loggboken](./media/application-proxy-understand-connectors/event-view-window.png)

och Windows-prestandaräknare. 

 ![Lägg till räknare toohello koppling med hello Prestandaövervakaren](./media/application-proxy-understand-connectors/performance-monitor.png)

hello kopplingar har både admin och sessionen loggar. Hej administratörsloggar innehåller viktiga händelser och deras fel. hello sessionsloggar innehåller alla hello transaktioner och information om bearbetning. 

toosee hello loggar gå toohello Loggboken, öppna hello **visa** -menyn och aktivera **visa analytiska loggar och felsökningsloggar**. Sedan kan aktivera dem toostart samla händelser. Dessa loggar visas inte i Web Application Proxy i Windows Server 2012 R2 som hello kopplingar är baserade på en senare version.

Du kan undersöka hello tjänstens i fönstret hello hello tillstånd. hello connector består av två Windows-tjänster: hello faktiska koppling och hello Update. Båda måste köra alla hello tid.

 ![AzureAD tjänster lokalt](./media/application-proxy-understand-connectors/aad-connector-services.png)

## <a name="next-steps"></a>Nästa steg


* [Publicera program på separata nätverk och platser med hjälp av connector-grupper](active-directory-application-proxy-connectors-azure-portal.md)
* [Arbeta med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md)
* [Felsöka Application Proxy och connector](active-directory-application-proxy-troubleshoot.md)
* [Hur toosilently installerar hello Azure AD Application Proxy Connector](active-directory-application-proxy-silent-installation.md)

