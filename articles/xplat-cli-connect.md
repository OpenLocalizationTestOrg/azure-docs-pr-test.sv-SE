---
title: "aaaLog i tooAzure från hello CLI | Microsoft Docs"
description: "Ansluta tooyour Azure-prenumeration från hello Azure-kommandoradsgränssnittet (Azure CLI) för Mac, Linux och Windows"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 42682c00c8dea78b2c624e640379716d1d4d7a2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-in-tooazure-from-hello-azure-cli"></a>Logga in tooAzure från hello Azure CLI
hello Azure CLI är öppen källkod, plattformsoberoende kommandon för att arbeta med Azure-resurser. Den här artikeln beskriver hello olika sätt tooprovide din Azure-konto autentiseringsuppgifter tooconnect hello Azure CLI tooyour Azure-prenumeration:

* Kör hello `azure login` CLI kommandot tooauthenticate via Azure Active Directory. Den här metoden ger du åtkomst till tooCLI kommandon i både [kommandot lägen](#cli-command-modes). När du kör kommandot hello utan ytterligare alternativ `azure login` efterfrågar toocontinue logga in interaktivt i en webbportal. För ytterligare `azure login` kommandot Alternativ, se hello scenarier i den här artikeln eller Skriv `azure login --help`.
* Om du bara behöver toouse Azure Service Management läge CLI-kommandona (rekommenderas inte för de flesta nya distributioner) kan du hämta och installera en publicera inställningsfil på datorn.

Om du inte redan har installerat hello CLI, se [installera hello Azure CLI](cli-install-nodejs.md). Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](http://azure.microsoft.com/free/) på ett par minuter.

Bakgrundsinformation om annat konto identiteter och Azure-prenumerationer finns [hur Azure-prenumerationer är associerade med Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Scenario 1: azure-inloggning med interaktiv inloggning
Med vissa konton hello CLI kräver att du toorun `azure login` och fortsätt sedan hello inloggningen med en webbläsare via en webbportal, en process som kallas *interaktiv inloggning*. En vanlig orsak är när du har ett arbets- eller skolkonto konto (kallas även en *organisationskonto*) som har ställts in toorequire flerfunktionsautentisering. Använd också interaktiv inloggning med ditt Microsoft-konto när du vill toouse kommandon för Resource Manager-läge.

Interaktiv inloggning är enkel: typen `azure login` --utan alternativ--som visas i följande exempel hello:

```
azure login
```                                                                                             

hello utdata visas ungefär hello följande:

```         
info:    Executing command login
info:    toosign in, use a web browser tooopen hello page http://aka.ms/devicelogin. Enter hello code XXXXXXXXX tooauthenticate.
```
Kopiera hello kod erbjuds tooyou i hello kommandoutdata och öppna en webbläsare toohttp://aka.ms/devicelogin eller andra sidan om anges. (Du kan öppna en webbläsare på hello samma dator eller på en annan dator eller enhet.) Ange hello kod och är du tillfrågas tooenter hello användarnamn och lösenord för hello identitet du vill toouse. När den här processen är klar slutför hello-kommandogränssnittet hello inloggningen. Det kan se ut ungefär så:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> Med interaktiv inloggning utförs autentisering och auktorisering med Azure Active Directory. Om du använder en Microsoft-kontoidentitet hello inloggningen har åtkomst till din Azure Active Directory-standarddomän. (Om du har registrerat dig för ett kostnadsfritt Azure-konto, Azure Active Directory skapas automatiskt en standarddomän för ditt konto.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Scenario 2: azure logga in med ett användarnamn och lösenord
Använd hello `azure login` med hello användarnamn (`-u`) parametern tooauthenticate när du vill toouse ett arbets- eller Skol-konto som inte kräver flerfunktionsautentisering. Du uppmanas kommandoraden hello hello lösenord (eller alternativt kan du skicka hello lösenord som en ytterligare hello-parameter `azure login` kommandot). hello skickas följande exempel hello användarnamnet för ett organisationskonto:

    azure login -u myUserName@contoso.onmicrosoft.com

Du kan sedan uppmanas tooenter ditt lösenord:

    info:    Executing command login
    Password: *********

hello inloggningen sedan är klar.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Om det här är din första gången logga in med autentiseringsuppgifterna du uppmanas tooverify du vill toocache en autentiseringstoken. Det här meddelandet kan också uppstå om du tidigare använt hello `azure logout` kommando (beskrivs senare i artikeln hello). toobypass varningen för automatiseringsscenarier kör `azure login` med hello `-q` parameter.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Scenario 3: azure logga in med ett huvudnamn för tjänsten
Om du skapar ett huvudnamn för tjänsten för ett Active Directory-program och hello tjänstens huvudnamn har behörighet för din prenumeration, kan du använda hello `azure login` kommandot tooauthenticate hello tjänstens huvudnamn. Beroende på ditt scenario kan du ange autentiseringsuppgifter för hello av hello tjänstens huvudnamn som explicit parametrar av hello `azure login` kommando. Till exempel skickar hello följande kommando hello tjänstens huvudnamn och Active Directory klient-ID:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Du kan sedan ange tooprovide hello lösenord. Du kan också ange hello autentiseringsuppgifter via en CLI-kod i skript eller ett program, eller använda ett certifikat tooauthenticate hello tjänstens huvudnamn icke-interaktivt för automatiseringsscenarier. Mer information och exempel finns [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Scenario 4: Använda en publicera inställningsfil
Om du bara behöver toouse hello Azure Service Management läge CLI-kommandona (till exempel toodeploy virtuella Azure-datorer i hello klassiska distributionsmodellen) kan du ansluta med en fil med inställningar i Publicera. Den här metoden installerar ett certifikat på den lokala datorn som du kan använda tooperform hanteringsaktiviteter för så länge hello prenumeration och hello certifikat är giltiga.

* **inställningsfilen för publicering av toodownload hello** för ditt konto, se till att hello CLI är i Service Management-läge genom att skriva `azure config mode asm`. Kör följande kommando hello:

        azure account download

Detta öppnar din standardwebbläsare och du uppmanas toosign i toohello [klassiska Azure-portalen](https://manage.windowsazure.com). När du loggar in, en `.publishsettings` filhämtningar. Notera var filen sparas.

> [!NOTE]
> Om ditt konto är kopplat till flera Azure Active Directory-klienter måste du ange tooselect som Active Directory som du vill toodownload en publiceringsinställningarna filen för.
>
>

När valts med hjälp av hello hämtningssidan, eller genom att besöka hello klassiska Azure-portalen, blir hello valda Active Directory hello-standard som används av hello klassiska portalen och hämtningssidan. När ett standardvärde har upprättats kan du se hello text '**Klicka här tooreturn toohello sidan**' hello överst i hello hämtningssidan. Använd hello som länk tooreturn toohello på sidan.

* **inställningsfilen för publicering av tooimport hello**kör hello följande kommando:

        azure account import <path tooyour .publishsettings file>

> [!IMPORTANT]
> När du har importerat dina Publiceringsinställningar, bör du ta bort hello `.publishsettings` fil. Det krävs inte längre av hello Azure CLI och utgör en säkerhetsrisk eftersom det skulle kunna använda toogain åtkomst tooyour prenumeration.
>
>

## <a name="cli-command-modes"></a>CLI kommandolägen
hello Azure CLI har två kommandolägen för att arbeta med Azure-resurser med olika uppsättningar:

* **Resource Manager-läget** – för att arbeta med Azure-resurser i hello Resource Manager-distributionsmodellen. tooset det här läget kör `azure config mode arm`.
* **Service Management-läge** – för att arbeta med Azure-resurser i hello klassiska distributionsmodellen. tooset det här läget kör `azure config mode asm`.

När installeras är hello aktuella versionen av hello CLI finns i Resource Manager-läget.

> [!NOTE]
> hello Resource Manager och Service Management-läge kan inte anges samtidigt. Det vill säga resurser som skapats i ett läge kan inte hanteras från hello andra läge.
>
>

## <a name="multiple-subscriptions"></a>Flera prenumerationer
Om du har flera Azure-prenumerationer, beviljar ansluta tooAzure åtkomst tooall prenumerationer som är kopplade till dina autentiseringsuppgifter. En prenumeration är valt som standard hello och används av hello Azure CLI när du utför åtgärder. Du kan visa hello prenumerationer, inklusive hello aktuell standard prenumeration, med hello `azure account list` kommando. Det här kommandot returnerar informationen liknande toohello följande:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

I föregående lista hello, hello **aktuella** visar hello aktuell standard prenumeration som Azure-sub-1. toochange hello standard prenumeration, Använd hello `azure account set` kommando och ange hello-prenumeration som du vill toobe hello standard. Exempel:

    azure account set Azure-sub-2

Detta ändrar hello standard prenumeration tooAzure-sub-2.

> [!NOTE]
> Ändra hello standardabonnemang träder i kraft omedelbart och är en global ändring; nya Azure CLI-kommandona om du kör dem från hello samma kommandoradsverktyget instans eller en annan instans använder hello standard prenumeration.
>
>

Om du vill toouse en icke-standard-prenumeration med hello Azure CLI, men inte vill toochange hello aktuell standard, kan du använda hello `--subscription` alternativet för hello kommando och ange hello namn för hello prenumeration gärna toouse för hello åtgärden.

När du är ansluten tooyour Azure-prenumeration kan börja du använda hello Azure CLI-kommandon toowork med Azure-resurser.

## <a name="storage-of-cli-settings"></a>Lagring av CLI-inställningar
Om du loggar in med hello `azure login` kommando eller importera Publiceringsinställningar, din CLI profil och loggar lagras i en `.azure` directory finns i din `user` directory. Din `user` directory skyddas av operativsystemet. Vi rekommenderar dock att du vidta ytterligare åtgärder tooencrypt din `user` directory. Du kan göra det i hello följande sätt:

* Ändra hello katalogegenskaper eller använda BitLocker för Windows.
* På Mac aktivera FileVault för hello directory.
* På Ubuntu, funktionen hello krypterade Home directory. Andra Linux-distributioner erbjuder liknande funktioner.

## <a name="logging-out"></a>Logga ut
toolog ut Använd hello följande kommando:

    azure logout -u <username>

Om hello prenumerationer som är kopplade till autentiseras endast hello kontot med Active Directory, logga ut borttagningar hello prenumerationsinformation från hello lokal profil. Men om även en publicera inställningsfil importerades för hello prenumerationer loggar endast tar bort Active Directory relaterad information från hello lokal profil.

## <a name="next-steps"></a>Nästa steg
* toouse Azure CLI-kommandon finns i [Azure CLI-kommandona i Resource Manager-läget](virtual-machines/azure-cli-arm-commands.md) och [Azure CLI-kommandon i Service Management-läge](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* toolearn mer om hello Azure CLI, ladda ned källkoden, rapportera problem, eller bidra toohello projekt, besök hello [GitHub-lagringsplatsen för hello Azure CLI](https://github.com/azure/azure-xplat-cli).
* Om du får problem med att använda hello Azure CLI eller Azure, gå hello [Azure forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
