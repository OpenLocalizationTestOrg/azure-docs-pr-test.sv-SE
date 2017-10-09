---
title: aaaUpgrade PhoneFactor tooAzure MFA-Server | Microsoft Docs
description: "Kom igång med Azure MFA-Server när du uppgraderar från hello äldre phonefactor agent."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.openlocfilehash: 15b7b8517929c30f66e6a39cd44c69d12d25c6d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hello-phonefactor-agent-tooazure-multi-factor-authentication-server"></a>Uppgradera hello PhoneFactor Agent tooAzure Multi-Factor Authentication-servern
tooupgrade hello PhoneFactor Agent v5.x och äldre tooAzure Multi-Factor Authentication-servern måste du avinstallera hello PhoneFactor Agent och kopplade komponenter först. Hej Multi-Factor Authentication-servern och dess anknutna komponenter kan installeras.

## <a name="uninstall-hello-phonefactor-agent"></a>Avinstallera hello PhoneFactor-Agent

1. Säkerhetskopiera först hello PhoneFactor-datafilen. hello standardinstallationsplatsen är C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.

2. Om hello Användarportalen är installerad:
  1. Navigera toohello installationsmappen och säkerhetskopiera hello web.config-filen. hello standardinstallationsplatsen är C:\inetpub\wwwroot\PhoneFactor.

  2. Om du har lagt till anpassade teman toohello portalen kan du säkerhetskopiera din egen mapp under hello C:\inetpub\wwwroot\PhoneFactor\App_Themes katalog.

  3. Avinstallera hello Användarportalen antingen via hello PhoneFactor Agent (endast tillgängligt om har installerats på hello samma server som hello PhoneFactor Agent) eller via Windows-program och funktioner.

3. Om hello Mobilappwebbtjänsten installeras:

  1. Gå toohello installationsmappen och säkerhetskopiera hello web.config-filen. hello standardinstallationsplatsen är C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.

  2. Avinstallera hello Mobilappwebbtjänsten via Windows-program och funktioner.

4. Om hello webbtjänst-SDK är installerat kan du avinstallera den via hello PhoneFactor-Agent eller via Windows-program och funktioner.

5. Avinstallera hello PhoneFactor Agent via Windows-program och funktioner.

## <a name="install-hello-multi-factor-authentication-server"></a>Installera hello Multi-Factor Authentication-servern

hello installationssökväg hämtas från registret hello från hello tidigare PhoneFactor Agent-installation så att den ska installeras i hello samma plats (till exempel C:\Program Files\PhoneFactor). Nya installationer har en annan standardinstallationssökväg (t.ex. c:\Program\Microsoft Files\Multi-Factor Authentication Server). Hej datafilen har lämnat hello tidigare PhoneFactor Agent bör uppgraderas under installationen, så att dina användare och inställningar bör fortfarande hello det nya Multi-Factor Authentication-servern när du har installerat.

1. Om du uppmanas aktivera hello Multi-Factor Authentication-servern och se till att den är tilldelad toohello rätt replikeringsgrupp.

2. Om hello webbtjänst-SDK tidigare har installerats, installerar hello ny webbtjänst-SDK via hello användargränssnittet för multi-Factor Authentication-servern.

  Hej standard katalognamnet är nu **MultiFactorAuthWebServiceSdk** i stället för **PhoneFactorWebServiceSdk**. Om du vill toouse hello tidigare namn, måste du ändra hello namnet på hello virtuell katalog under installationen. Annars, om du tillåter hello installera toouse hello nya standardnamnet du har toochange hello URL i alla program som referens hello webbtjänst-SDK (till exempel hello Användarportalen och Mobilappwebbtjänsten) toopoint på hello rätt plats.

3. Om hello Användarportalen tidigare har installerats på hello PhoneFactor Agent Server installerar hello nya Multi-Factor Authentication-Användarportalen via hello användargränssnittet för multi-Factor Authentication-servern.

  Hej standard katalognamnet är nu **MultiFactorAuth** i stället för **PhoneFactor**. Om du vill toouse hello tidigare namn, måste du ändra hello namnet på hello virtuell katalog under installationen. Om du tillåter hello installera toouse hello nya standardnamnet bör du annars Klicka hello Användarportalen ikonen i hello Multi-Factor Authentication-servern och uppdatera hello Användarportalens URL på hello-inställningar.

4. Om hello Användarportalen och/eller Mobilappwebbtjänsten tidigare har installerats på en annan server än hello PhoneFactor Agent:

  1. Gå toohello installationsplatsen (till exempel C:\Program Files\PhoneFactor) och kopiera en eller flera installationsprogram toohello annan server. Det finns installationsprogram för 32-bitars och 64-bitars för både hello Användarportalen och Mobilappwebbtjänsten. De heter MultiFactorAuthenticationUserPortalSetupXX.msi och MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

  2. tooinstall hello Användarportalen på hello webbserver, öppna en kommandotolk som administratör och kör MultiFactorAuthenticationUserPortalSetupXX.msi.

    Hej standard katalognamnet är nu **MultiFactorAuth** i stället för **PhoneFactor**. Om du vill toouse hello tidigare namn, måste du ändra hello namnet på hello virtuell katalog under installationen. Om du tillåter hello installera toouse hello nya standardnamnet bör du annars Klicka hello Användarportalen ikonen i hello Multi-Factor Authentication-servern och uppdatera hello Användarportalens URL på hello-inställningar. Befintliga användare måste toobe känner till ny hello-URL.

  3. Gå toohello Användarportalen installationsplatsen (till exempel C:\inetpub\wwwroot\MultiFactorAuth) och redigera hello web.config-filen. Kopiera hello värden i hello appSettings och applicationSettings avsnitt från din ursprungliga web.config-fil som har säkerhetskopierats innan hello uppgraderingen i hello ny web.config-fil. Om hello nya virtuella katalogen standardnamnet har kvar när du installerar hello webbtjänst-SDK, ändra hello URL hello applicationSettings avsnittet toopoint toohello rätt plats. Om alla standardvärden har ändrats i hello tidigare web.config-filen, gäller dessa samma ändringar toohello nya web.config-filen.

  4. tooinstall hello Mobilappwebbtjänsten på hello webbserver, öppna en kommandotolk som administratör och kör hello MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

    Hej standard katalognamnet är nu **MultiFactorAuthMobileAppWebService** i stället för **PhoneFactorPhoneAppWebService**. Om du vill toouse hello tidigare namn, måste du ändra hello namnet på hello virtuell katalog under installationen. Du kanske vill toochoose ett kortare namn toomake det enkelt för slutanvändare tootype i på sina mobila enheter. Om du tillåter hello installera toouse hello nya standardnamnet bör du annars Klicka hello Mobilapp ikonen i hello Multi-Factor Authentication-servern och uppdatera hello Mobile App webbtjänst-URL.

  5. Gå toohello Mobilappwebbtjänsten installationsplatsen (till exempel C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) och redigera hello web.config-filen. Kopiera hello värden i hello appSettings och applicationSettings avsnitt från din ursprungliga web.config-fil som har säkerhetskopierats innan hello uppgraderingen i hello ny web.config-fil. Om hello nya virtuella katalogen standardnamnet har kvar när du installerar hello webbtjänst-SDK, ändra hello URL hello applicationSettings avsnittet toopoint toohello rätt plats. Om alla standardvärden har ändrats i hello tidigare web.config-filen, gäller dessa samma ändringar toohello nya web.config-filen.

## <a name="next-steps"></a>Nästa steg

- [Installera hello användare portal](multi-factor-authentication-get-started-portal.md) för hello Azure Multi-Factor Authentication-servern.

- [Konfigurera Windows-autentisering](multi-factor-authentication-get-started-server-windows.md) för dina program. 
