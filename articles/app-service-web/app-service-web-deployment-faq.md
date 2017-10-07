---
title: "aaaDeployment vanliga frågor och svar för Azure-webbappar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om distribution för funktionen för hello Web Apps i Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 566e1d7028e678f9679200f436118d27dfb07079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Distribution och svar för Web Apps i Azure

Den här artikeln innehåller svar toofrequently vanliga frågor och svar om distributionsproblem för hello [funktionen Web Apps i Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>Det är bara komma igång med App Service web apps. Hur jag för att publicera min kod?

Här följer några alternativ för att publicera web app koden:

*   Distribuera med Visual Studio. Om du har hello Visual Studio-lösning, högerklicka hello webbapprojektet och välj **publicera**.
*   Distribuera med hjälp av en FTP-klient. I hello Azure-portalen, hämta hello publiceringsprofil för hello webbprogrammet som du vill toodeploy din kod. Därefter kan du överföra hello filer too\site\wwwroot med hjälp av hello samma Publicera profil FTP-autentiseringsuppgifter.

Mer information finns i [distribuera din app tooApp tjänsten](web-sites-deploy.md).

## <a name="i-see-an-error-message-when-i-try-toodeploy-from-visual-studio-how-do-i-resolve-this"></a>Jag får ett felmeddelande när jag försöker toodeploy från Visual Studio. Hur lösa problemet?

Om du ser följande meddelande hello du kanske använder en äldre version av hello SDK ”: fel under distributionen för resurs 'YourResourceName' i resursgruppen 'YourResourceGroup': MissingRegistrationForLocation: hello prenumerationen har inte registrerats för hello resurstyp 'komponenter' hello plats 'Centrala USA'. Registrera för den här providern i ordning toohave toothis plats ”. 

tooresolve det här felet, uppgradera toohello [senaste SDK: N](https://azure.microsoft.com/downloads/). Om du ser det här meddelandet och du har hello senaste SDK: N, begär support.

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-tooapp-service"></a>Hur distribuerar ett ASP.NET-program från Visual Studio tooApp tjänsten?
<a id="deployasp"></a>

hello kursen [skapa din första ASP.NET-webbapp i Azure på fem minuter](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-get-started/) visar hur toodeploy ett ASP.NET web application tooa webbapp i App Service med hjälp av Visual Studio 2015.

## <a name="what-are-hello-different-types-of-deployment-credentials"></a>Vad är hello olika typer av autentiseringsuppgifter för distribution?

Apptjänst stöder två typer av autentiseringsuppgifter för lokal Git och FTP-/ S-distributionen. Mer information om hur autentiseringsuppgifter för distribution av tooconfigure finns [konfigurera autentiseringsuppgifter för distribution för Apptjänst](app-service-deployment-credentials.md).

## <a name="what-is-hello-file-or-directory-structure-of-my-app-service-web-app"></a>Vad är hello filen eller katalogen strukturen för webbappen Apptjänst?

Information om hello filstruktur för din App Service-appen finns [filen strukturen i Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure).

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-hello-disk-when-i-try-tooftp-my-files"></a>Hur löser jag ”FTP fel 550 - det finns inte är tillräckligt med utrymme på disken för hello” när jag försöker tooFTP Mina filer?

Om du ser det här meddelandet är det troligt att du kör i en diskkvot i hello service-plan för webbappen. Du kan behöva tooscale in tooa högre tjänstnivån baserat på dina behov av diskutrymme. Mer information om priser planer och gränserna finns [priser för Apptjänst](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>Hur ställer jag in kontinuerlig distribution för webbprogrammet min App Service?

Du kan ställa in kontinuerlig distribution från flera resurser, inklusive Visual Studio Team Services, OneDrive, GitHub, Bitbucket, Dropbox och andra Git-databaser. Dessa alternativ är tillgängliga i hello-portalen. [Kontinuerlig distribution tooApp Service](app-service-continuous-deployment.md) är en bra genomgång som förklarar hur tooset in kontinuerlig distribution.

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>Hur felsöker problem med kontinuerlig distribution från GitHub och Bitbucket?

Hur du undersöker problem med kontinuerlig distribution från GitHub eller Bitbucket finns [undersöker kontinuerlig distribution](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment).

## <a name="i-cant-ftp-toomy-site-and-publish-my-code-how-do-i-resolve-this"></a>Jag kan inte FTP-platsen toomy och publicera min kod. Hur lösa problemet?

problem med tooresolve FTP:

1. Kontrollera att du anger rätt hello-värdnamn och autentiseringsuppgifter. Detaljerad information om olika typer av autentiseringsuppgifter och hur toouse dem, se [distributionsbehörigheterna](https://github.com/projectkudu/kudu/wiki/Deployment-credentials).
2. Kontrollera att hello FTP portar inte blockeras av en brandvägg. hello portar ska ha de här inställningarna:
    * Anslutningsport för FTP-kontroll: 21
    * FTP-dataporten för anslutning: 989 10001 10300

## <a name="how-do-i-publish-my-code-tooapp-service"></a>Hur jag för att publicera min kod tooApp tjänsten?

hello Azure Quickstart är utformad toohelp som du distribuerar en app med hello distribution stacken och metod du föredrar. Gå toouse hello Snabbstart, i hello Azure-portalen för**inställningar** > **Appdistribution**.

## <a name="why-does-my-app-sometimes-restart-after-deployment-tooapp-service"></a>Varför min app ibland starta om efter distribution tooApp tjänsten?

toolearn om hello omständigheter som en programdistribution kan resultera i en omstart finns [distribution kontra runtime problem](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts"). Eftersom artikeln hello distribuerar Apptjänst toohello wwwroot-mappen. Den startar aldrig direkt om din app.

## <a name="how-do-i-integrate-visual-studio-team-services-code-with-app-service"></a>Hur integreras Visual Studio Team Services kod med App Service?

Har du två alternativ för att använda kontinuerlig distribution med Visual Studio Team Services:

*   Använd ett Git-projekt. Anslut via App Service med hjälp av hello distributionsalternativ för den lagringsplatsen.
*   Använd ett Team Foundation Version kontrollen (TFVC)-projekt. Distribuera med hello build-agent för Apptjänst.

Koden kontinuerlig distribution för båda alternativen beror på befintliga developer arbetsflöden och checka in procedurer. Mer information finns i följande artiklar: 

*   [Implementera kontinuerlig distribution för din app tooan Azure-webbplats](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [Konfigurera ett Visual Studio Team Services-konto så att den kan distribuera tooa webbprogram](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-toodeploy-my-app-tooapp-service"></a>Hur använder jag FTP eller FTPS toodeploy min app tooApp tjänsten?

Information om hur du använder FTP eller FTPS toodeploy din web app tooApp tjänsten, finns i [distribuera din app tooApp tjänsten med hjälp av FTP/S](app-service-deploy-ftp.md).
