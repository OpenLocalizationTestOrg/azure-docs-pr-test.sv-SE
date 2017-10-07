---
title: "aaaCommon orsaker till Molntjänsten roller återvinning | Microsoft Docs"
description: "En rolltjänst för molnet plötsligt återanvänds kan medföra betydande driftstopp. Här följer några vanliga problem som orsakar roller toobe återvinns, som kan hjälpa dig att minska driftstopp."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 8fa152b33d2b22a8a02f834d4bc38519b4272f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="common-issues-that-cause-roles-toorecycle"></a>Vanliga problem som orsakar roller toorecycle
Den här artikeln beskrivs några av hello vanliga orsaker till problem med distribution och felsökning tips toohelp de här problemen. En indikation på att det finns ett problem med ett program är när hälsningspaket rollinstansen misslyckas toostart eller den växlar mellan hello initiera upptagen och stoppas.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Runtime-beroenden som saknas
Om en roll i ditt program är beroende av en sammansättning som inte är en del av hello .NET Framework eller hello Azure hanterade biblioteket, måste du uttryckligen inkludera sammansättningen i hello programpaket. Tänk på att andra Microsoft-ramverk inte är tillgängliga på Azure som standard. Om din roll är beroende av dessa ramverk, måste du lägga till dessa sammansättningar toohello programpaket.

Kontrollera hello följande innan du skapar och paketera programmet:

* Om du använder Visual studio gör att hello **kopiera lokala** egenskapen för**SANT** för varje refererar till sammansättningen i projektet som inte är en del av hello Azure SDK eller hello .NET Framework.
* Kontrollera att hello web.config-filen inte refererar till några oanvända sammansättningar i hello kompilering element.
* Hej **Skapa åtgärd** av varje .cshtml fil har angetts för**innehåll**. Detta säkerställer att hello filer visas korrekt i hello paketet och gör andra referensfiler tooappear i hello-paketet.

## <a name="assembly-targets-wrong-platform"></a>Sammansättningen mål fel plattform
Azure är en 64-bitars-miljö. Därför fungerar inte .NET-sammansättningar som kompilerats för ett 32-bitars mål på Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rollen utlöser ett ohanterat undantag vid initiering eller stoppar
Alla undantag som utlöses av hello hello [RoleEntryPoint] -klassen, som innehåller hello [OnStart], [OnStop], och [kör]metoder, är ohanterade undantag. Om det uppstår ett ohanterat undantag i någon av dessa metoder kan hello rollen kommer att återanvändas. Om hello rollen återvinning upprepade gånger kan det utlöser ett undantag när den försöker toostart.

## <a name="role-returns-from-run-method"></a>Rollen som returneras från metoden
Hej [kör] metoden är avsedda toorun på obestämd tid. Om din kod åsidosätter hello [kör] metod det ska vila på obestämd tid. Om hello [kör] metoden returnerar hello rollen återanvänds.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Felaktig DiagnosticsConnectionString-inställning
Om programmet använder Azure-diagnostik, din tjänstekonfigurationsfil måste ange hello `DiagnosticsConnectionString` konfigurationsinställning. Den här inställningen ska ange ett HTTPS-anslutning tooyour storage-konto i Azure.

tooensure som din `DiagnosticsConnectionString` är korrekt innan du distribuerar ditt program paketet tooAzure, kontrollera hello följande:  

* Hej `DiagnosticsConnectionString` inställningen punkter tooa giltigt lagringskonto i Azure.  
  Som standard pekar inställningen toohello emulerade storage-konto, så du måste uttryckligen ändra inställningen innan du distribuerar ditt programpaket. Om du inte ändrar den här inställningen genereras ett undantag när hälsningspaket rollinstansen försöker toostart hello diagnostikövervakare. Detta kan orsaka hello rollen instans toorecycle på obestämd tid.
* hello anslutningssträngen har angetts i hello följande [format](../storage/common/storage-configure-connection-string.md). (hello-protokollet måste anges som HTTPS). Ersätt *MyAccountName* med hello namnet på ditt lagringskonto och *MyAccountKey* med din åtkomstnyckel:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Om du utvecklar programmet med hjälp av Azure-verktyg för Microsoft Visual Studio kan du använda hello egenskapen sidor tooset det här värdet.

## <a name="exported-certificate-does-not-include-private-key"></a>Exporterade certifikatet innehåller ingen privat nyckel
toorun en webbroll under SSL, måste du kontrollera att din exporterade certifikat innehåller hello privata nyckel. Om du använder hello *Windows Certificate Manager* tooexport hello certifikatet vara säker på att tooselect **Ja** för hello **Export hello privata nyckeln** alternativet. hello certifikatet måste exporteras i hello PFX-format, vilket är hello endast format som stöds för närvarande.

## <a name="next-steps"></a>Nästa steg
Visa mer [felsökning artiklar](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) för molntjänster.

Visa mer rollåtervinning scenarier vid [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[kör]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
