---
title: "aaaContinuous distribution för Azure Functions | Microsoft Docs"
description: "Använd kontinuerlig distribution för Azure App Service toopublish Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 28c44f737dad3feab3cf54f7dd42b6a978d0617e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-for-azure-functions"></a>Löpande distribution för Azure Functions
Azure Functions gör det enkelt toodeploy appen funktionen med hjälp av Apptjänst kontinuerlig integration. Functions kan integreras med BitBucket, Dropbox, GitHub eller Visual Studio Team Services VSTS (). Detta gör att ett arbetsflöde där Funktionskoden uppdateringar genom att använda en av dessa integrerade tjänster tooAzure för distribution av utlösare. Om du är ny tooAzure funktioner, starta med [översikt över Azure Functions](functions-overview.md).

Kontinuerlig distribution är ett bra alternativ för projekt där flera och ofta återkommande bidrag integreras. Du kan även Underhåll källkontrollen på koden funktioner. följande distribution källor hello stöds:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Extern lagringsplats (Git eller ett)
* [Lokal Git-lagringsplats](../app-service-web/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

Distributioner konfigureras på grundval av per funktion app. Kontinuerlig distribution har aktiverats åtkomst toofunction koden i hello portal anges när för*skrivskyddad*.

## <a name="continuous-deployment-requirements"></a>Krav för kontinuerlig distribution

Du måste ha din distributionskälla konfigurerad och funktioner koden i hello distributionskälla innan du kan ställa in kontinuerlig distribution. Varje funktion bor i en namngiven underkatalog där hello katalognamnet är hello namnet på hello-funktion i en viss funktion app-distribution.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a>Konfigurera kontinuerlig distribution
Använd den här proceduren tooconfigure kontinuerlig distribution för en befintlig funktionsapp. Dessa steg visar integrering med en GitHub-databas, men liknande steg gäller för Visual Studio Team Services eller andra deployment services.

1. I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsalternativ**. 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment.png)
 
2. I hello **distributioner** bladet och klickar på **installationsprogrammet**.
 
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. I hello **distributionskälla** bladet, klickar du på **Välj källa**, fyll sedan i hello information för din valda distributionskälla och klickar på **OK**.
   
    ![Välj distributionskälla](./media/functions-continuous-deployment/choose-deployment-source.png)

När kontinuerlig distribution har konfigurerats, alla ändringar i din distributionskälla är kopierade toohello funktionsapp och en fullständig webbplatsdistribution utlöses. hello plats är omdistribueras när filer i hello källan uppdateras.

## <a name="deployment-options"></a>Distributionsalternativ

hello följande är några vanliga distributionsscenarier:

- [Skapa en fristående distribution](#staging)
- [Flytta befintliga funktioner toocontinuous distribution](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Skapa en fristående distribution

Funktionen appar stöder inte ännu distributionsplatser. Du kan dock fortfarande hantera separata mellanlagring och produktion distributioner med hjälp av kontinuerlig integrering.

Hej processen tooconfigure och arbeta med en fristående distribution vanligtvis ser ut så här:

1. Skapa två funktionen appar i din prenumeration, en för hello kod och en för Förproduktion. 

2. Skapa en distributionskälla om du inte redan har ett. Det här exemplet används [GitHub].

3. Om appen produktion funktionen fullständig hello föregående steg i **ställa in kontinuerlig distribution** och ange hello distribution gren toohello mastergrenen av GitHub-lagringsplatsen.
   
    ![Välj distributionen gren](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Upprepa det här steget för hello mellanlagring funktionsapp, men välja hello mellanlagring gren i stället i GitHub-lagringsplatsen. Om din distribution-källan inte stöder förgrening, använder du en annan mapp.
    
5. Göra uppdateringar tooyour koden i hello mellanlagring branch eller mapp och sedan kontrollera att ändringarna återspeglas i hello mellanlagring av distribution.

6. Sammanfoga ändringar från hello mellanlagring gren till mastergrenen hello efter tester. Den här kopplingen utlöser distribution toohello funktionen produktionsprogrammet. Om din distribution-källan inte stöder filialer, kan du skriva över hello filer i hello produktion mappen med hello filer från hello mellanlagring mapp.

<a name="existing"></a>
### <a name="move-existing-functions-toocontinuous-deployment"></a>Flytta befintliga funktioner toocontinuous distribution
När du har befintliga funktioner som du har skapat och underhålls i hello portal toodownload måste din befintliga fungera kodfiler med hjälp av FTP- eller hello lokal Git-lagringsplats innan du kan ställa in kontinuerlig distribution som beskrivs ovan. Du kan göra detta i hello App Service-inställningar för din funktionsapp. När filerna har hämtats överför du dem. tooyour valt kontinuerlig distributionskälla.

> [!NOTE]
> När du har konfigurerat kontinuerlig integration kommer du inte längre att kunna tooedit källfiler i hello Functions-portalen.

- [Så här: Konfigurera autentiseringsuppgifter för distribution](#credentials)
- [Så här: hämta filer med FTP](#downftp)
- [Så här: hämta filer med hjälp av hello lokal Git-lagringsplats](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Så här: Konfigurera autentiseringsuppgifter för distribution
Innan du kan hämta filer från din funktion med FTP- eller lokal Git-lagringsplats, måste du konfigurera autentiseringsuppgifter tooaccess hello platsen. Autentiseringsuppgifter anges vid hello funktionen app-nivå. Använd hello följande steg tooset autentiseringsuppgifter för distribution i hello Azure-portalen:

1. I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsbehörigheterna**.
   
    ![Ange autentiseringsuppgifter för lokal distribution](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Ange ett användarnamn och lösenord och klicka sedan på **spara**. Nu kan du använda dessa autentiseringsuppgifter tooaccess funktionen appen från FTP- eller hello inbyggda Git-lagringsplatsen.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Så här: hämta filer med FTP

1. I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **egenskaper**, kopiera hello värden för **FTP/distribution användaren**, **Värdnamn för FTP-**, och **FTPS värdnamn**.  

    **FTP-/ Distributionsanvändare användaren** måste anges som visas i hello portal, inklusive hello programnamn, tooprovide rätt kontext för hello FTP-servern.
   
    ![Hämta information om distribution](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Använd hello anslutningsinformation du samlade in tooconnect tooyour app från FTP-klient och hämta hello källfilerna för dina funktioner.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Så här: hämta filer med hjälp av en lokal Git-lagringsplats

1. I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsalternativ**. 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment.png)
 
2. I hello **distributioner** bladet och klickar på **installationsprogrammet**.
 
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. I hello **distributionskälla** bladet, klickar du på **lokal Git-lagringsplats** och klicka sedan på **OK**.

3. I **plattformsfunktioner**, klickar du på **egenskaper** och anteckna värdet för hello för Git-URL. 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Klona hello-databasen på den lokala datorn med hjälp av Kommandotolken Git-medveten eller ditt favoritprogram Git-verktyget. Hej Git klonade kommandot ser ut så här:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Hämta filer från din funktion app toohello klon på den lokala datorn, som i följande exempel hello:
   
        git pull origin master
   
    Om det krävs ange din [konfigurerat autentiseringsuppgifter för distribution](#credentials).  

[GitHub]: https://github.com/
