---
title: aaaContinuous levereras cloud services med Visual Studio Online | Microsoft Docs
description: "Lär dig hur tooset in kontinuerlig leverans för Azure cloud appar utan att spara diagnostik lagring viktiga toohello service configuration-filer"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: dc87d049e46daf8b8a26ee4450ebd9b7910f287b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-tooazure-using-visual-studio-online"></a>Securely spara Cloud Services Diagnostics Lagringsnyckel och installationsprogrammet kontinuerlig integrering och distribution tooAzure med hjälp av Visual Studio Online
 Det är en gemensam praxis tooopen källa projekt Nuförtiden. Spara programmet hemligheter i konfigurationsfiler finns inte längre säker hantering som säkerhetsproblem exponeras från hemligheter hamnar från offentliga källa kontroller. Lagra hemlighet som oformaterad text i en fil i en kontinuerlig Integration pipeline inte är säker delade antingen eftersom servrar kan vara resurser på hello molnmiljö. Den här artikeln förklarar hur Visual Studio och Visual Studio Online minskar hello säkerhetsfrågor under utveckling och kontinuerlig Integration process.

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>Ta bort nyckeln hemlig för diagnostik lagring i konfigurationsfilen för projektet
Cloud Services-tillägget för diagnostik kräver Azure storage för att spara diagnostik resultatet. Tidigare hello lagringsanslutningssträng har angetts i hello molntjänster (.cscfg) konfigurationsfiler och gick att checka in toosource kontroll. Vi ändrat hello beteende tooonly store en partiell anslutningssträngen med hello nyckel ersättas med en token i hello senaste Azure SDK-versionen. hello följande steg beskriver hur hello nya molntjänster verktygsuppsättning fungerar:

### <a name="1-open-hello-role-designer"></a>1. Öppna hello rollen designer
* Dubbelklicka på eller högerklicka på en molntjänster rollen tooopen roll designer

![Öppna rollen designer][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2. Under avsnittet för diagnostik nytt ”inte ta bort nyckeln för säkerhetslagring hemliga från projektet” har lagts till
* Om du använder hello lokala lagringsemulatorn den här kryssrutan är inaktiverad eftersom det inte finns några hemliga toomanage för hello lokala anslutningssträngen, som är UseDevelopmentStorage = true.

![Lokal lagring emulatorn anslutningssträngen är inte hemliga][1]

* Om du skapar ett nytt projekt som standard är den här kryssrutan avmarkerad. Detta resulterar i hello lagring viktiga avsnitt i anslutningssträngen för lagring av hello som valts som ersätts med en token. hello värdet för hello-token kommer att hittas under hello den aktuella användarens centrala AppData mapp, till exempel: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService

> Observera hello user\AppData mappen är åtkomst kontrolleras av användarens inloggning och anses vara en säker plats toostore development hemligheter.
> 
> 

![Nyckeln för säkerhetslagring sparas under användarprofil][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3. Välj ett lagringskonto för diagnostik
* Välj ett lagringskonto från hello dialogrutan startas genom att klicka på ”...” Hej till. Observera hur anslutningssträngen för lagring av hello genereras inte hello lagringskontonyckel.
* Till exempel: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)

### <a name="4----debugging-hello-project"></a>4.    Felsökning hello-projekt
* F5 toostart felsökning i Visual Studio. Allt bör fungera i hello samma sätt som tidigare.
  ![Starta felsökning lokalt][3]

### <a name="5-publish-project-from-visual-studio"></a>5. Publicera projekt från Visual Studio
* Starta hello dialogrutan Publicera och fortsätt inloggningen instruktioner toopublish hello applicaion tooAzure.

### <a name="6-additional-information"></a>6. Ytterligare information
> Obs: hello inställningar panelen i hello rollen designer förblir eftersom den är för tillfället. Om du vill toouse hello hemliga hanteringsfunktionen för diagnostik gå toohello konfigurationer fliken.
> 
> 

![Lägg till-inställningar][4]

> Obs: Om aktiverad, hello Application Insights nyckeln lagras som klartext. hello-nyckeln används endast för Överför innehållet så att inga känsliga data är i fara komprometteras.
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>Skapa och publicera en Cloud Services-projekt med hjälp av Visual Studio online uppgiftsmallar
* hello följande steg visar hur toosetup kontinuerlig Integration för molntjänster projekt med Visual Studio online uppgifter:
  ### <a name="1----obtain-a-vso-account"></a>1.    Hämta ett VSO konto
* [Skapa Visual Studio Online-konto] [ Create Visual Studio Online account] om du inte redan har en
* [Skapa grupprojekt] [ Create team project] i Visual Studio-onlinekonto

### <a name="2----setup-source-control-in-visual-studio"></a>2.    Konfigurera källkontroll i Visual Studio
* Ansluta tooa grupprojekt

![Ansluta tooteam-projekt][5]

![Välj team projekt tooconnect till][6]

* Lägg till ditt projekt toosource kontroll

![Lägga till projektet toosource kontroll][7]

![Mappa kontrollen projektmappen tooa källa][8]

* Kontrollera i projektet från Explorer-teamet

![Kontrollera i projektet toosource kontroll][9]

### <a name="3----configure-build-process"></a>3.    Konfigurera skapandeprocess
* Bläddra tooyour grupprojekt och Lägg till en ny build-process mallar

![Lägg till en ny version][10]

* Välj build-aktivitet

![Lägga till en build-aktivitet][11]

![Välj mall för Visual Studio-Skapa aktivitet][12]

* Redigera build uppgiften indata. Anpassa hello build parametrar enligt tooyour måste du

![Konfigurera build-aktivitet][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* Konfigurera build variabler

![Konfigurera build variabler][14]

* Lägg till en aktivitet tooupload build släpp

![Välj publicera build släpp aktivitet][15]

![Konfigurera publicera build släpp aktivitet][16]

* Kör hello build

![Ny version av kön][17]

![Visa build sammanfattning][18]

* Om hello build lyckas visas en liknande toobelow resultat

![Skapa resultat][19]

### <a name="4----configure-release-process"></a>4.    Konfigurera Release-processen
* Skapa en ny version

![Skapa ny version][20]

* Välj hello uppgift för distribution av Azure Cloud Services

![Välj Azure Cloud Services uppgift för distribution][21]

* Som hello lagringskontonyckel inte är markerat i toosource kontrollen behöver vi toospecify hello hemlig nyckel för att ställa in diagnostik tillägg. Expandera hello **avancerade alternativ för att skapa nya tjänsten** avsnittet och redigera hello **diagnostik Lagringskontonycklar** parametern indata. Den här indata använder flera rader med nyckel/värde-par i hello-format för **[RoleName]:$(StorageAccountKey)**

> Obs: om din diagnostik storage-konto är under hello samma prenumeration som där du ska publicera hello molntjänster programmet, du inte tooenter hello nyckel i hello distribution uppgiften indata; hello distribution hämtar programmässigt hello storage-informationen från prenumerationen
> 
> 

![Konfigurera Cloud Services distribution aktivitet][22]

* Använd hemlighet Skapa variabler toosave Lagringsnycklar. indata för en variabel som hemlighet klickar du på hello låsikon hello höger på hello toomask variabler

![Spara lagringsnycklar i hemlighet Skapa variabler][23]

* Skapa en version och distribuera ditt projekt tooAzure

![Skapa ny version][24]

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du anger diagnostik tillägg för Azure Cloud Services, se [aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png
