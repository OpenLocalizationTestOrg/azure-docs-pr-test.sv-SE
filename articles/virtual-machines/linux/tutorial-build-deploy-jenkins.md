---
title: "aaaCI/CD från Jenkins tooAzure virtuella datorer med Team Services | Microsoft Docs"
description: "Ställ in kontinuerlig integration (KO) och kontinuerlig distribution (CD) av en Node.js-app med Jenkins tooAzure virtuella datorer från versionshantering i Visual Studio Team Services VSTS () eller Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/15/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: 400ae34cbdf45da65351811c0ff6ff5d61ef862c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-toolinux-vms-using-jenkins-and-team-services"></a>Distribuera din app tooLinux virtuella datorer med hjälp av Jenkins och Team Services

Kontinuerlig integration (KO) och kontinuerlig distribution (CD) är en pipeline som du kan bygga, släppa och distribuera din kod. Team Services innehåller en fullständig, komplett funktionalitet uppsättning av verktyg för automatisering av CI/CD för distribution tooAzure. Jenkins är ett populära 3 parts CI/CD-server-baserade verktyg som också ger CI/CD-automatisering. Du kan använda båda tillsammans toocustomize hur du levererar ditt molnapp eller tjänst.

I den här kursen använder du Jenkins toobuild en **Node.js-webbapp**, och Visual Studio Team Services toodeploy den tooa [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) som innehåller virtuella Linux-datorer.

Du kommer att:

> [!div class="checklist"]
> * Skapa din app i Jenkins
> * Konfigurera Jenkins för integrering med Team Services
> * Skapa en distributionsgrupp för hello Azure virtuella datorer
> * Skapa en definition av versionen som konfigurerar hello virtuella datorer och distribuerar hello app

## <a name="before-you-begin"></a>Innan du börjar

* Du behöver åtkomst tooa Jenkins konto. Om du inte har skapat en Jenkins server, se [Jenkins dokumentationen](https://jenkins.io/doc/). 

* Logga in tooyour Team Services-konto (`https://{youraccount}.visualstudio.com`). 
  Du kan få en [kostnadsfritt konto Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Mer information finns i [ansluta tooTeam Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

* Skapa en personlig åtkomsttoken (PATRIK) i ditt Team Services-konto om du inte redan har ett. Jenkins kräver den här informationen tooaccess ditt Team Services-konto.
  Läs [hur skapar jag en personlig åtkomsttoken för Team Services och TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) toolearn hur toogenerate en.

## <a name="get-hello-sample-app"></a>Hämta hello sample-appen

Du behöver en app toodeploy lagras i en Git-lagringsplats.
Den här kursen rekommenderar vi du använder [denna sample-appen som är tillgängliga från GitHub](https://github.com/azooinmyluggage/fabrikam-node).

1. Skapa en duplicering av den här appen och anteckna hello plats (URL) för användning i senare steg i den här kursen.

1. Se hello förgrening **offentliga** toosimplify ansluta tooGitHub senare.

> [!NOTE]
> Mer information finns i [duplicera en repo](https://help.github.com/articles/fork-a-repo/) och [gör en privat databas offentliga](https://help.github.com/articles/making-a-private-repository-public/).

> [!NOTE]
> hello app har skapats med [Yeoman](http://yeoman.io/learning/index.html); används **Express**, **bower**, och **grunt**; och har några **npm**paket som beroenden.
> Hej exempelapp innehåller en uppsättning [Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) som har använt toodynamically skapar hello virtuella datorer för distribution på Azure. Dessa mallar används av uppgifter i hello [Team Services släpper definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).
> hello Huvudmall skapar en nätverkssäkerhetsgrupp, en virtuell dator och ett virtuellt nätverk. Den tilldelas en offentlig IP-adress och öppnar inkommande port 80. Det ger också en tagg som används av hello distribution för väljer hello datorer tooreceive hello distribution.
>
> hello exemplet innehåller också ett skript som konfigurerar Nginx och distribuerar hello app. Den körs på varje hello virtuella datorer. Mer specifikt installerar hello skriptet nod och Nginx PM2; konfigurerar Nginx och PM2; Därefter startar hello nod app.

## <a name="configure-jenkins-plugins"></a>Konfigurera Jenkins plugin-program

Först måste du konfigurera två Jenkins plugin-program för **NodeJS** och **integrering med Team Services**.

1. Öppna Jenkins-konto och välj **hantera Jenkins**.

1. I hello **hantera Jenkins** väljer **hantera plugin-program**.

1. Filter hello listan toolocate hello **NodeJS** plugin-program och installera den utan omstart.

   ![Lägga till hello NodeJS plugin-programmet tooJenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Filter hello listan toofind hello **Team Foundation Server** plugin-program och installera den. (Det här plugin-programmet fungerar för både Team Services och Team Foundation Server.) Det är inte nödvändigt att starta om Jenkins.

## <a name="configure-jenkins-build-for-nodejs"></a>Konfigurera Jenkins build för Node.js

Skapa ett nytt build-projekt i Jenkins, och konfigurerar det enligt följande:

1. I hello **allmänna** och anger ett namn för ditt build-projekt.

1. I hello **källa kod Management** väljer **Git** och ange hello information om hello databasen och hello gren med Appkod.

   ![Lägg till en lagringsplatsen tooyour version](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Om databasen inte är offentlig väljer **Lägg till** och ange autentiseringsuppgifter tooconnect tooit.

1. I hello **Skapa utlösare** väljer **avsökning SCM** och ange hello schema `H/03 * * * *` toopoll hello Git-lagringsplats för ändringar i alla tre minuter. 

1. I hello **kompileringsmiljö** väljer **ange nod &amp; npm bin / mappen SÖKVÄGEN** och ange `NodeJS` för hello Node JS-Installation-värde. Lämna **npmrc filen** inställd på ”Använd systemstandard”.

1. I hello **skapa** ange hello kommandot `npm install` tooensure alla beroenden är uppdaterade.

## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurera Jenkins för integrering med Team Services

1. I hello **efter build åtgärder** fliken för **filer tooarchive**, ange `**/*` tooinclude alla filer.

1. För **utlösa version i TFS/Team Services**, ange hello fullständiga URL: en för ditt konto (som `https://your-account-name.visualstudio.com`), hello projektnamnet, ett namn för hello versionen definition (skapas senare), och hello autentiseringsuppgifter tooconnect tooyour konto.
   Du behöver ditt användarnamn och hello PATRIK som du skapade tidigare. 

   ![Konfigurera Jenkins efter build-åtgärder](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. Spara hello build-projekt.

## <a name="create-a-jenkins-service-endpoint"></a>Skapa en Jenkins tjänstslutpunkt

En tjänstslutpunkt kan Team Services tooconnect tooJenkins.

1. Öppna hello **Services** i Team Services, öppna hello **nya tjänstslutpunkten** listan, och välj **Jenkins**.

   ![Lägg till en Jenkins slutpunkt](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Ange ett namn som du ska använda toorefer toothis anslutningen.

1. Ange hello URL-Adressen till din Jenkins och skalstreck hello **accepterar ej betrodda certifikat för SSL** alternativet.

1. Ange hello användarnamn och lösenord för kontot för Jenkins.

1. Välj **verifiera anslutning** toocheck som hello information är korrekt.

1. Välj **OK** toocreate hello tjänstslutpunkten.

## <a name="create-a-deployment-group"></a>Skapa en distributionsgrupp

Du behöver en [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) för innehålla hello virtuella datorer.

1. Öppna hello **versioner** för hello **skapa &amp; versionen** hubb och sedan öppna hello **distributionsgrupper** , och välj **+ ny**.

1. Ange ett namn för hello distributionsgruppen och en valfri beskrivning.
   Välj **skapa**.

hello Azure Resource distribution aktivitet skapas och registrerar hello VMs när den körs med hello Azure Resource Manager-mall.
Du inte behöver toocreate och registrera hello virtuella datorer själv.

## <a name="create-a-release-definition"></a>Skapa en definition för versionen

En definition av versionen anger hello processen Team Services körs toodeploy hello app.
toocreate hello versionen definition i Team Services:

1. Öppna hello **versioner** för hello **skapa &amp; släpper** NAV, öppna hello  **+**  listrutan i hello listan över definitioner av versionen och välj Hej **skapa versionen definition**. 

1. Välj hello **tom** mall och välj **nästa**.

1. I hello **artefakter** klickar du på **länka en artefakt** och välj **Jenkins**. Välj Jenkins tjänst endpoint-anslutningen. Sedan hello Jenkins källa jobb och **skapa**. 

1. I nya hello släpper definition, Välj **+ Lägg till aktiviteter** och lägga till en **Azure Resource distribution** aktivitet toohello standardmiljö.

1. Välj hello nedrullningsbara pilen Nästa toohello **+ Lägg till aktiviteter** länkar och Lägg till en grupp fas toohello distributionsdefinitionen.

   ![Lägga till en distribution grupp fas](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. Öppna hello i hello uppgiften katalogen **Utility** och lägger till en instans av hello **kommandoskript** aktivitet.

1. hello parametrar mall som används i hello Azure Resource distribution uppgiften anger hello admin lösenord som används för tooconnect toohello virtuella datorer.
   Du ge det här lösenordet hello variabeln **$(adminpassword)**:
   
   - Öppna hello **variabler** fliken och i hello **variabler** ange hello namnet `adminpassword`.

   - Ange hello administratörslösenord.

   - Välj hello ”hänglås” ikonen nästa toohello värdet textruta tooprotect hello lösenord. 

## <a name="configure-hello-azure-resource-group-deployment-task"></a>Konfigurera hello Azure Resource distribution aktivitet

Hej **Azure Resource distribution** uppgift är används toocreate hello distributionsgruppen. Konfigurera enligt följande:

* **Azure-prenumeration:** Välj en anslutning hello listan under **tillgängliga Azure Service anslutningar**. 
  Om inga anslutningar visas väljer du **hantera**väljer **nya tjänstslutpunkten** sedan **Azure Resource Manager**, och följ anvisningarna för hello.
  Returnera tooyour versionen definition, uppdatera hello **AzureRM prenumeration** listan och väljer hello-anslutning som du skapade.

* **Resursgruppen**: Ange ett namn på hello resursgrupp som du skapade tidigare.

* **Plats**: Välj en region för hello-distribution.

  ![Skapa en ny resursgrupp](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **Platsen mall**:`URL of hello file`

* **Mallen länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Mallen parametrar länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Åsidosätt mallparametrar**: en lista över hello åsidosätta värden, till exempel: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Infoga egna specifika värden för hello {platshållare}. 

* **Aktivera krav**:`Configure with Deployment Group agent`

* **TFS/VSTS endpoint**: Välj **Lägg till** i hello ”Lägg till ny Team Foundation Server-teamets Services anslutning” dialogrutan Välj **Token för autentisering**. Ange ett namn för hello anslutningen och hello URL för ditt team projekt. Generera och ange sedan en [personlig åtkomst-Token (PATRIK)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) tooauthenticate hello anslutning tooyour team projekt.

  ![Skapa en personlig åtkomsttoken](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Grupprojekt**: Välj det aktuella projektet.

* **Distributionsgruppen**: Ange hello samma gruppnamn för distribution som du använde för hello **resursgruppen** parameter.

hello standardinställningarna för hello Azure Resource distribution aktivitet är toocreate eller uppdatera en resurs och toodo så inkrementellt. hello uppgift skapar hello VMs hello första gången körs, och därefter bara uppdatera dem.

## <a name="configure-hello-shell-script-task"></a>Konfigurera hello kommandoskript aktivitet

Hej **kommandoskript** uppgift är används tooprovide hello konfiguration för ett skript toorun på varje server tooinstall Node.js och starta hello app. Konfigurera enligt följande:

* **Script sökvägen**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Ange fungerande Directory**:`Checked`

* **Arbetskatalog**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-hello-release-definition"></a>Byt namn på och spara hello versionen definition

1. Redigera hello namn hello versionen toohello namnet på definitionen av du angav i den **efter build åtgärder** fliken hello build i Jenkins. Jenkins kräver det här namnet toobe kan tootrigger en ny version när hello källa artefakter uppdateras.

1. Du kan också ändra hello namnet på hello miljö genom att klicka på hello namn. 

1. Välj **spara**, och välj **OK**.

## <a name="start-a-manual-deployment"></a>Starta en manuell distribution

1. Välj **+ versionen** och välj **skapa versionen**.

1. Välj hello build du slutfört i hello markerade nedrullningsbara listrutan och välj **skapa**.

1. Välj hello versionen länk i hello popup-meddelande. Till exempel ”: versionen **Release 1** har skapats”.

1. Öppna hello **loggar** fliken toowatch hello versionen konsolens utdata.

1. Öppna hello URL i webbläsaren och en av hello servrar du har lagt till tooyour distributionsgruppen. Ange till exempel`http://{your-server-ip-address}`

## <a name="start-a-cicd-deployment"></a>Starta en CI/CD-distribution

1. I hello versionen definition, avmarkerar hello **aktiverad** kryssrutan i hello **alternativ för åtkomstkontroll** hello inställningarna för hello Azure Resource distribution aktivitet.
   För framtida distributioner toohello befintliga distributionsgruppen, behöver inte toore-köra den här åtgärden.

1. Gå toohello Git-lagerkälla och ändra hello innehållet i hello **h1** rubriken i hello filen [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. Genomför ändringarna.

1. Efter några minuter ser du en ny utgåva som skapats i hello **versioner** i Team Services eller TFS. Öppna hello versionen toosee hello distribution äger rum. Grattis!

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen automatiserad hello distribution av en app tooAzure med Jenkins bygg- och Team Services för versionen. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa din app i Jenkins
> * Konfigurera Jenkins för integrering med Team Services
> * Skapa en distributionsgrupp för hello Azure virtuella datorer
> * Skapa en definition av versionen som konfigurerar hello virtuella datorer och distribuerar hello app

Avancera toohello nästa självstudiekurs toolearn mer om hur toodeploy ett ljus (Linux, Apache, MySQL och PHP) stacken.

> [!div class="nextstepaction"]
> [Distribuera LAMP-stack](tutorial-lamp-stack.md)