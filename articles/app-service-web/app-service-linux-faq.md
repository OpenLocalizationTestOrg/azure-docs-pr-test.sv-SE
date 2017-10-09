---
title: "aaaAzure App Service Web App på Linux vanliga frågor och svar | Microsoft Docs"
description: "Azure App Service Webbapp på Linux vanliga frågor och svar."
keywords: "Azure apptjänst, webbprogram, vanliga frågor och svar, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: c7798d9144d936eecdc0e191fc870b0ee0b220c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-on-linux-faq"></a>Azure App Service Webbapp på Linux vanliga frågor och svar

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Hello versionen av webbprogram på Linux arbetar vi med att lägga till funktioner och göra förbättringar tooour plattform. Här är några vanliga frågor (FAQ) som våra kunder har efterfrågat oss via hello senast månader.
Om du har en fråga, kommentera hello artikeln och vi kommer att svara så snart som möjligt.

## <a name="built-in-images"></a>Inbyggda bilder

**F:** jag vill toofork hello inbyggda Docker-behållare som hello plattform tillhandahåller. Var hittar jag dessa filer?

**S:** hittar du alla Docker-filer på [GitHub](https://github.com/azure-app-service). Du hittar alla behållare i Docker på [Docker-hubb](https://hub.docker.com/u/appsvc/).

**F:** vilka är hello förväntade värden för hello startfil avsnitt när jag konfigurera hello runtime stack?

**S:** för Node.Js som du anger hello PM2 konfigurationsfilen eller skriptfilen. Ange namnet på din kompilerade DLL-filen för .NET Core. För Ruby, kan du ange hello Ruby skript som du vill tooinitialize din app med.

## <a name="management"></a>Hantering

**F:** vad som händer när jag trycka hello omstart av-knappen i hello Azure-portalen?

**S:** detta är hello motsvarighet till Docker omstart.

**F:** kan jag använda SSH (Secure Shell) tooconnect toohello app behållaren virtuell dator (VM)?

**S:** Ja, du kan göra att kontrollera hello följande artikel via hello SCM-webbplatsen för mer information [SSH-stöd för webbprogrammet på Linux](./app-service-linux-ssh-support.md)

**F:** jag vill toocreate ett Linux App Service-plan från SDK eller en ARM-mall, hur kan jag göra detta?

**S:** måste tooset hello `reserved` för hello app service för`true`.

## <a name="continuous-integrationdeployment"></a>Kontinuerlig integration/distribution

**F:** webbappen fortfarande använder en gammal Docker behållare avbildning när jag har uppdaterat hello avbildningen på Docker-hubben. Stöder kontinuerlig integration/distribution av anpassade behållare?

**S:** tooset in kontinuerlig integration/distribution för Azure-behållare registret eller DockerHub bilder genom att kontrollera hello följande artikel [kontinuerlig distribution med Azure Web App på Linux](./app-service-linux-ci-cd.md). Du kan uppdatera hello behållare genom att stoppa och starta ditt webbprogram för privata register. Eller kan du ändra eller lägga till en dummy tillämpningsinställningen tooforce en uppdatering av din behållare.

**F:** stöder du fristående datorn?

**S:** Ja.

**F:** kan du använda **webbdistribution** toodeploy webbappen?

**S:** Ja, du behöver tooset en app inställningen kallas `WEBSITE_WEBDEPLOY_USE_SCM` för`false`.

## <a name="language-support"></a>Språkstöd

**F:** stöder du Okompilerade .NET Core appar?

**S:** Ja.

**F:** stöder du Composer som en beroende-hanterare för PHP-appar?

**S:** Ja. Under en Git-distribution bör Kudu identifiera som du distribuerar ett PHP-program (tack toohello förekomsten av en composer.json-fil) och utlöser en composer installation för dig.

## <a name="custom-containers"></a>Anpassade behållare

**F:** jag använder egna anpassade container. Min app finns i hello `\home\` directory, men jag kan inte hitta Mina filer när jag söker hello innehåll med hjälp av hello [SCM plats](https://github.com/projectkudu/kudu) eller en FTP-klient. Var finns Mina filer?

**S:** vi montera en SMB-resurs toohello `\home\` directory. Det åsidosätter allt innehåll som redan finns där.

**F:** jag använder egna anpassade container. Jag vill inte hello plattform toomount en SMB-resurs toohello `\home\`.

**S:** kan du göra det genom att ange hello `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app inställning för`false`.

**F:** mitt anpassade container tar en lång tid toostart och hello plattform omstart hello behållare innan den har slutförts startas.

**S:** kan du konfigurera hello tid hello plattform ska vänta innan du startar om din behållare. Detta kan göras genom att ange hello `WEBSITES_CONTAINER_START_TIME_LIMIT` app inställningen toohello det önskade värdet i sekunder. hello standardvärdet är 230 sekunder och Hej max är 600 sekunder.

**F:** vad är hello format för privata registret serverns url?

**S:** måste tooprovide hello fullständig registret url inklusive `http://` eller `https://`.

**F:** vad är hello format för hello avbildningsnamn i privata registret?

**S:** måste tooadd hello fullständiga namn inklusive hello privata registret url (t.ex. myacr.azurecr.IO/DotNet:Latest)

**F:** jag vill tooexpose mer än en port på min anpassade container avbildningen. Är som möjligt?

**S:** för närvarande, som inte stöds.

**F:** kan jag använda min egen storage?

**S:** för närvarande som inte stöds.

**F:** det går inte att bläddra mitt anpassade container filen system eller som kör processer från hello SCM plats. Varför är?

**S:** hello SCM webbplatsen körs i en separat behållare. Du kan inte kontrollera hello filsystem eller kör processer på hello appbehållare.

**F:** mitt anpassade container lyssnar tooa port än port 80. Hur kan jag konfigurera min app tooroute hello begäranden toothat port?

**S:** har vi automatisk identifiering av port, du kan också ange ett program ange kallas **WEBSITES_PORT**, och ge den hello värdet för hello förväntades portnummer. Tidigare hello plattform med `PORT` app inställningen genom att vi planerar toodeprecate hello används den här appen inställningen och flytta toousing `WEBSITES_PORT` exklusivt.

**F:** behöver jag tooimplement HTTPS i min anpassade container.

**S:** Nej, hello plattformen hanterar HTTPS-avslutning vid hello delade frontends.

## <a name="pricing-and-sla"></a>Priser och SLA

**F:** vad är hello priser när du använder hello förhandsversion?

**S:** debiteras du hälften hello antal timmar som appen körs med hello normal priser Azure App Service. Det innebär att du får 50 procent rabatt på normala priser för Azure App Service.

## <a name="other"></a>Annat

**F:** vad är hello stöds tecken i programnamn inställningar?

**S:** du kan bara använda A-Z, a-z, 0-9 och hello understreck för programinställningar.

**F:** där du kan begära det nya funktioner?

**S:** kan du skicka din idé på hello [Web Apps Feedbackforum](https://aka.ms/webapps-uservoice). Lägg till ”[Linux]” toohello titeln på din idé.

## <a name="next-steps"></a>Nästa steg
* [Vad är Azure Web App på Linux?](app-service-linux-intro.md)
* [SSH-stöd för Azure Web App på Linux](./app-service-linux-ssh-support.md)
* [Skapa mellanlagringsmiljöer i Azure App Service](./web-sites-staged-publishing.md)
* [Kontinuerlig distribution med Azure-Webbapp på Linux](./app-service-linux-ci-cd.md)
