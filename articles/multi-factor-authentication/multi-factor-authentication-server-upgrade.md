---
title: uppgradering av aaaAzure MFA-Server | Microsoft Docs
description: Anvisningar och riktlinjer tooupgrade hello Azure Multi-Factor Authentication-servern tooa nyare version.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: aaa8d400e0e5f1c6be3a6d22cde6dd893ef4d546
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-toohello-latest-azure-multi-factor-authentication-server"></a>Uppgradera toohello senaste Azure Multi-Factor Authentication-servern

Den här artikeln vägleder dig genom hello processen med att uppgradera Azure Multi-Factor Authentication (MFA) Server v6.0 eller högre. Om du behöver tooupgrade en gammal version av hello PhoneFactor Agent Se för[uppgradera hello PhoneFactor Agent tooAzure servern för flerfunktionsautentisering](multi-factor-authentication-get-started-server-upgrade.md).

Om du inte uppgradera från v6.x eller äldre toov7.x eller senare kan ändra alla komponenter från .NET 2.0 too.NET 4.5. Alla komponenter måste också Microsoft Visual C++ 2015 Redistributable Update 1 eller högre. Hej MFA-Server installer installerar både hello x86 och x64 versioner av dessa komponenter om de inte redan har installerats. Om hello Användarportalen och Mobilappwebbtjänsten körs på separata servrar, måste tooinstall paketen innan du uppgraderar komponenterna. Du kan söka efter hello senaste Microsoft Visual C++ 2015 Redistributable uppdateringen på hello [Microsoft Download Center](https://www.microsoft.com/en-us/download/). 

## <a name="install-hello-latest-version-of-azure-mfa-server"></a>Installera hello senaste versionen av Azure MFA-Server

1. Använd hello-instruktioner i [Download hello Azure Multi-Factor Authentication-servern](multi-factor-authentication-get-started-server.md#download-the-azure-multi-factor-authentication-server) tooget hello senaste versionen av hello Azure MFA-Server.
2. Gör en säkerhetskopia av hello MFA-serverns datafil som finns i C:\Program program\multi-Factor Authentication Server\Data\PhoneFactor.pfdata (antagande om hello installera standardplatsen) på din MFA-huvudservern.
3. Om du kör flera servrar för hög tillgänglighet, ändra hello klientsystem som autentiseras toohello MFA-servern så att de slutar skickar trafik toohello servrar som uppgraderas. Om du använder en belastningsutjämnare, ta bort en MFA-Server från belastningsutjämnaren hello gör hello uppgradering och Lägg sedan till hello servern tillbaka till hello servergruppen.
4. Kör hello nya installationsprogrammet på varje MFA-Server. Uppgradera först underordnade servrar eftersom de kan läsa hello gamla datafilen replikeras av hello master. 

  Du behöver inte toouninstall din aktuella MFA-servern innan du kör installationsprogrammet för hello. hello installer utför en uppgradering på plats. hello installationssökväg hämtas från registret hello från hello tidigare installation, så installeras i hello samma plats (till exempel C:\Program program\multi-Factor Authentication-servern). 
  
5. Om du inte ange tooinstall en Microsoft Visual C++ 2015 Redistributable uppdateringspaketet kan acceptera uppmaningen om hello. Båda hello x86 och x64 versioner av hello-paket installeras.
5. Om du använder hello webbtjänst-SDK, uppmanas du tooinstall hello ny webbtjänst-SDK. När du installerar Hej ny webbtjänst-SDK, se till att det virtuella katalognamnet hello matchar hello tidigare installerade virtuell katalog (till exempel MultiFactorAuthWebServiceSdk).
6. Upprepa steg hello på alla underordnade servrar. Uppgradera en av hello underordnade toobe hello ny mall och sedan uppgradera hello gamla huvudservern. 

## <a name="upgrade-hello-user-portal"></a>Uppgradera hello Användarportalen

1. Gör en säkerhetskopia av hello web.config-fil som finns i hello virtuella katalogen för hello installationsplats för Användarportalen (till exempel C:\inetpub\wwwroot\MultiFactorAuth). Gör en säkerhetskopia av hello App_Themes\Default mappen även om några ändringar har gjorts toohello standardtemat. Det är bättre toocreate en kopia av hello standardmapp och skapa ett nytt tema än toochange hello standardtemat.
2. Om hello Användarportalen körs på samma server som hello andra MFA-Server-komponenter, hello hello uppmaning MFA serverinstallation tooupdate hello Användarportalen. Acceptera uppmaningen om hello och installera hello Användarportalen uppdatering. Kontrollera att hello katalognamnet matchar hello tidigare installerade virtuell katalog (till exempel MultiFactorAuth).
3. Om hello Användarportalen finns på en egen server, kopiera hello MultiFactorAuthenticationUserPortalSetup64.msi fil från hello installationsplats för en av hello MFA-servrar och placera den på hello Användarportalen webbserver. Kör installationsprogrammet för hello. 

  Om ett fel inträffar anger ”Microsoft Visual C++ 2015 Redistributable uppdatering 1 eller senare krävs”, hämta och installera hello senaste uppdateringspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/). Installera båda hello x86 och x64 versioner.

4. Jämför hello web.config säkerhetskopiorna i steg 1 med hello nya web.config-filen när hello uppdaterats Användarportalen programvara är installerad. Om det finns inga nya attribut i hello nya web.config, kopiera filen web.config för säkerhetskopiering till hello virtuell katalog toooverwrite hello ny. Ett annat alternativ är toocopy och klistra in hello appSettings värden och hello webbtjänst-SDK URL från hello säkerhetskopian till hello nya web.config.

Om du har hello Användarportalen på flera servrar, upprepa hello installation på dem alla. 


## <a name="upgrade-hello-mobile-app-web-service"></a>Uppgradera hello Mobilappwebbtjänsten

1. Gör en säkerhetskopia av hello web.config-fil som finns i hello virtuella katalogen för hello Mobilappwebbtjänsten installationsplatsen (till exempel C:\inetpub\wwwroot\app eller C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService).
2. Kopiera hello MultiFactorAuthenticationMobileAppWebServiceSetup64.msi fil från hello installera platsen för hello MFA-servrar och placera den på hello Mobilapp registreringsserver.
3. Kör installationsprogrammet för hello. 

  Om det uppstår ett fel som anger att Microsoft Visual C++ 2015 Redistributable uppdatering 1 eller senare krävs, hämta och installera hello senaste uppdateringspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/). Installera båda hello x86 och x64 versioner.

4. Jämför hello web.config-filen som har säkerhetskopierats i steg 1 med hello nya web.config-filen när hello uppdateras Mobilappwebbtjänsten programvara har installerats. Om det finns inga nya attribut i hello nya web.config, kan du kopiera dina sparade web.config till hello virtuell katalog och skriver över hello ny. Ett annat alternativ är toocopy och klistra in hello appSettings värden och hello webbtjänst-SDK URL från hello säkerhetskopian till hello nya web.config.

Om du har hello Mobilappwebbtjänsten på flera servrar, upprepa hello installation på dem alla. 

## <a name="upgrade-hello-ad-fs-adapters"></a>Uppgradera hello AD FS-kort


### <a name="if-mfa-runs-on-different-servers-than-ad-fs"></a>Om MFA körs på olika servrar än AD FS

Dessa anvisningar gäller endast om du kör servern för flerfunktionsautentisering separat från AD FS-servrarna. Om båda tjänsterna körs hello samma servrar, hoppa över det här avsnittet och gå toohello installationssteg. 

1. Spara en kopia av hello MultiFactorAuthenticationAdfsAdapter.config fil som har registrerats i AD FS eller exportera hello-konfigurationen med hjälp av följande PowerShell-kommando hello: `Export-AdfsAuthenticationProviderConfigurationData -Name [adapter name] -FilePath [path tooconfig file]`. hello kortets namn är ”WindowsAzureMultiFactorAuthentication” eller ”AzureMfaServerAuthentication” beroende på hello-version som installerats tidigare.
2. Kopiera följande filer från hello MFA-Server installation plats toohello AD FS-servrarna hello:

  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config

3. Redigera hello Register-MultiFactorAuthenticationAdfsAdapter.ps1 skript genom att lägga till `-ConfigurationFilePath [path]` toohello slutet av hello `Register-AdfsAuthenticationProvider` kommando. Ersätt *[sökväg]* med hello fullständig sökväg toohello MultiFactorAuthenticationAdfsAdapter.config exporterade filen eller hello-konfigurationsfilen i hello föregående steg. 

  Kontrollera hello attribut i hello nya MultiFactorAuthenticationAdfsAdapter.config toosee om de matchar hello gammal konfigurationsfil. Om några attribut har lagts till eller tas bort i hello ny version, kopiera hello attributvärden från hello gamla configuration file toohello ny eller ändra hello gamla configuration file toomatch.

### <a name="install-new-ad-fs-adapters"></a>Installera nya AD FS-kort

> [!IMPORTANT] 
> Användarna kommer inte att nödvändiga tooperform tvåstegsverifiering under steg 3 – 8 av det här avsnittet. Om du har AD FS som konfigurerats i flera kluster, kan du ta bort, uppgradering och återställa varje kluster i hello servergrupp oberoende av hello andra kluster tooavoid driftstopp.

1. Ta bort några AD FS-servrar från hello servergruppen. Uppdatera servrarna när hello andra fortfarande körs.
2. Installera hello nya AD FS-adaptern på varje server som tas bort från hello AD FS-servergrupp. Om hello MFA-servern är installerad på varje AD FS-servern, kan du uppdatera via hello MFA-administratören UX. Uppdatera annars genom att köra sedan filerna MultiFactorAuthenticationAdfsAdapterSetup64.msi. 

  Om ett fel inträffar anger ”Microsoft Visual C++ 2015 Redistributable uppdatering 1 eller senare krävs”, hämta och installera hello senaste uppdateringspaketet från hello [Microsoft Download Center](https://www.microsoft.com/download/). Installera båda hello x86 och x64 versioner.

3. Gå för**AD FS** > **autentiseringsprinciper** > **Multifaktoråtkomstkontroll certifikatautentisering**. Avmarkera **WindowsAzureMultiFactorAuthentication** eller **AzureMFAServerAuthentication** (beroende på hello aktuell version installerad). 

  När det här steget har slutförts är tvåstegsverifiering via MFA-servern inte tillgänglig i det här AD FS-klustret förrän du har slutfört steg 8.

4. Avregistrera hello äldre version av hello AD FS-adaptern genom att köra hello Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-skript. Se till att hello *-namnet* parameter (”WindowsAzureMultiFactorAuthentication” eller ”AzureMFAServerAuthentication”) som matchar hello-namnet som visas i steg 3. Detta gäller tooall servrar i hello samma AD FS-klustret eftersom det inte finns en central konfiguration.
5. Registrera hello nya AD FS-adaptern genom att köra hello Register-MultiFactorAuthenticationAdfsAdapter.ps1 PowerShell-skript. Detta gäller tooall servrar i hello samma AD FS-klustret eftersom det inte finns en central konfiguration.
6. Starta om hello AD FS-tjänsten på varje server som tas bort från hello AD FS-servergrupp.
7. Lägg till hello uppdateras servrar tillbaka toohello AD FS-grupp och ta bort hello servrar från hello servergruppen.
8. Gå för**AD FS** > **autentiseringsprinciper** > **Multifaktoråtkomstkontroll certifikatautentisering**. Kontrollera **AzureMfaServerAuthentication**.
9. Upprepa steg 2 tooupdate hello servrar som har nu tagits bort från hello AD FS-grupp och starta om hello AD FS-tjänsten på dessa servrar.
10. Lägg till dessa servrar till hello AD FS-servergrupp.

## <a name="next-steps"></a>Nästa steg

- Exempel på [avancerade scenarier med Azure Multi-Factor Authentication och tredje parts VPN-anslutningar](multi-factor-authentication-advanced-vpn-configurations.md)

- [Synkronisera MFA-Server med Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)

- [Konfigurera Windows-autentisering](multi-factor-authentication-get-started-server-windows.md) för dina program
