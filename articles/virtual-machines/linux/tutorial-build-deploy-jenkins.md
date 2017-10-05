---
title: "CI/CD: N från Jenkins för virtuella Azure-datorer med Team Services | Microsoft Docs"
description: "Ställ in kontinuerlig integration (KO) och kontinuerlig distribution (CD) av en Node.js-app med Jenkins till Azure virtuella datorer från versionshantering i Visual Studio Team Services VSTS () eller Microsoft Team Foundation Server (TFS)"
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
ms.openlocfilehash: a40e26a8681df31fad664e4d1df4c1513311900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-team-services"></a>Distribuera din app till Linux virtuella datorer med hjälp av Jenkins och Team Services

Kontinuerlig integration (KO) och kontinuerlig distribution (CD) är en pipeline som du kan bygga, släppa och distribuera din kod. Team Services innehåller en fullständig, komplett funktionalitet uppsättning av verktyg för automatisering av CI/CD för distribution till Azure. Jenkins är ett populära 3 parts CI/CD-server-baserade verktyg som också ger CI/CD-automatisering. Du kan använda båda tillsammans för att anpassa hur du levererar ditt molnapp eller tjänst.

I den här självstudiekursen kommer du använda Jenkins för att skapa en **Node.js-webbapp**, och Visual Studio Team Services för att distribuera den till en [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) som innehåller virtuella Linux-datorer.

Du kommer att:

> [!div class="checklist"]
> * Skapa din app i Jenkins
> * Konfigurera Jenkins för integrering med Team Services
> * Skapa en distributionsgrupp för de virtuella Azure-datorerna
> * Skapa en definition av versionen som konfigurerar de virtuella datorerna och distribuerar appen

## <a name="before-you-begin"></a>Innan du börjar

* Du behöver åtkomst till ett konto för Jenkins. Om du inte har skapat en Jenkins server, se [Jenkins dokumentationen](https://jenkins.io/doc/). 

* Logga in på ditt Team Services-konto (`https://{youraccount}.visualstudio.com`). 
  Du kan få en [kostnadsfritt konto Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Mer information finns i [Anslut till Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

* Skapa en personlig åtkomsttoken (PATRIK) i ditt Team Services-konto om du inte redan har ett. Jenkins kräver den här informationen för att få åtkomst till ditt Team Services-konto.
  Läs [hur skapar jag en personlig åtkomsttoken för Team Services och TFS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) att lära dig hur du skapar en.

## <a name="get-the-sample-app"></a>Hämta sample-appen

Du måste distribuera en app lagras i en Git-lagringsplats.
Den här kursen rekommenderar vi du använder [denna sample-appen som är tillgängliga från GitHub](https://github.com/azooinmyluggage/fabrikam-node).

1. Skapa en duplicering av den här appen och Anteckna platsen (URL) för användning i senare steg i den här kursen.

1. Kontrollera förgrening **offentliga** att förenkla ansluter till GitHub senare.

> [!NOTE]
> Mer information finns i [duplicera en repo](https://help.github.com/articles/fork-a-repo/) och [gör en privat databas offentliga](https://help.github.com/articles/making-a-private-repository-public/).

> [!NOTE]
> Appen har skapats med [Yeoman](http://yeoman.io/learning/index.html); används **Express**, **bower**, och **grunt**; och har några **npm** paket som beroenden.
> Exempelappen innehåller en uppsättning [Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) som används för att dynamiskt skapa virtuella datorer för distribution på Azure. Dessa mallar används av uppgifter i den [Team Services släpper definition](https://www.visualstudio.com/docs/build/actions/work-with-release-definitions).
> Den huvudsakliga mallen skapar en nätverkssäkerhetsgrupp, en virtuell dator och ett virtuellt nätverk. Den tilldelas en offentlig IP-adress och öppnar inkommande port 80. Det ger också en tagg som används av distributionsgruppen för att välja datorer för att ta emot distributionen.
>
> Exemplet innehåller också ett skript som konfigurerar Nginx och distribuerar appen. Den körs på var och en av de virtuella datorerna. Mer specifikt installerar skriptet nod, Nginx och PM2; konfigurerar Nginx och PM2; Därefter startar appen nod.

## <a name="configure-jenkins-plugins"></a>Konfigurera Jenkins plugin-program

Först måste du konfigurera två Jenkins plugin-program för **NodeJS** och **integrering med Team Services**.

1. Öppna Jenkins-konto och välj **hantera Jenkins**.

1. I den **hantera Jenkins** väljer **hantera plugin-program**.

1. Filtrera listan för att hitta den **NodeJS** plugin-program och installera den utan omstart.

   ![Att lägga till plugin-programmet för NodeJS Jenkins](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)

1. Filtrera listan efter den **Team Foundation Server** plugin-program och installera den. (Det här plugin-programmet fungerar för både Team Services och Team Foundation Server.) Det är inte nödvändigt att starta om Jenkins.

## <a name="configure-jenkins-build-for-nodejs"></a>Konfigurera Jenkins build för Node.js

Skapa ett nytt build-projekt i Jenkins, och konfigurerar det enligt följande:

1. I den **allmänna** och anger ett namn för ditt build-projekt.

1. I den **källa kod Management** väljer **Git** och ange information om databasen och grenen med Appkod.

   ![Lägg till en repo i din version](media/tutorial-build-deploy-jenkins/jenkins-git.png)

   > [!NOTE]
   > Om databasen inte är offentlig väljer **Lägg till** och ange autentiseringsuppgifter för att ansluta till den.

1. I den **Skapa utlösare** väljer **avsökning SCM** och ange schemat `H/03 * * * *` avsöker Git-lagringsplats för ändringar i alla tre minuter. 

1. I den **kompileringsmiljö** väljer **ange nod &amp; npm bin / mappen SÖKVÄGEN** och ange `NodeJS` för Installation av JS-värdet. Lämna **npmrc filen** inställd på ”Använd systemstandard”.

1. I den **skapa** ange kommandot `npm install` att se till att alla beroenden är uppdaterade.

## <a name="configure-jenkins-for-team-services-integration"></a>Konfigurera Jenkins för integrering med Team Services

1. I den **efter build åtgärder** fliken för **filer att arkivera**, ange `**/*` att inkludera alla filer.

1. För **utlösa version i TFS/Team Services**, ange en fullständig Webbadress för ditt konto (som `https://your-account-name.visualstudio.com`), projektnamnet, ett namn för versionen definitionen (skapas senare) och autentiseringsuppgifterna för att ansluta till ditt konto.
   Du behöver ditt användarnamn och PATRIK som du skapade tidigare. 

   ![Konfigurera Jenkins efter build-åtgärder](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)

1. Spara build-projektet.

## <a name="create-a-jenkins-service-endpoint"></a>Skapa en Jenkins tjänstslutpunkt

En tjänstslutpunkt kan Team Services att ansluta till Jenkins.

1. Öppna den **Services** i Team Services, öppna den **nya tjänstslutpunkten** listan, och välj **Jenkins**.

   ![Lägg till en Jenkins slutpunkt](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)

1. Ange ett namn som du använder för att referera till den här anslutningen.

1. Ange Webbadressen till din Jenkins server och skalstreck den **accepterar ej betrodda certifikat för SSL** alternativet.

1. Ange användarnamn och lösenord för kontot för Jenkins.

1. Välj **verifiera anslutning** att kontrollera att informationen är korrekt.

1. Välj **OK** att skapa tjänsteslutpunkt.

## <a name="create-a-deployment-group"></a>Skapa en distributionsgrupp

Du behöver en [distributionsgruppen](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) som innehåller de virtuella datorerna.

1. Öppna den **versioner** för den **skapa &amp; versionen** hubben, öppna den **distributionsgrupper** , och välj **+ ny**.

1. Ange ett namn för distributionsgruppen och en valfri beskrivning.
   Välj **skapa**.

Aktiviteten Azure Resource distribution skapar och registrerar de virtuella datorerna när den körs med hjälp av Azure Resource Manager-mall.
Du behöver inte skapa och registrera de virtuella datorerna själv.

## <a name="create-a-release-definition"></a>Skapa en definition för versionen

En definition av versionen anger processen Team Services kommer att köras om du vill distribuera appen.
Skapa definition för versionen i Team Services:

1. Öppna den **versioner** för den **skapa &amp; släpper** hubben, öppna den  **+**  listrutan i listan över definitioner av versionen och välj  **Skapa en definition för versionen**. 

1. Välj den **tom** mall och välj **nästa**.

1. I den **artefakter** klickar du på **länka en artefakt** och välj **Jenkins**. Välj Jenkins tjänst endpoint-anslutningen. Sedan väljer du det Jenkins källa och välj **skapa**. 

1. I den nya versionen definition, Välj **+ Lägg till aktiviteter** och lägga till en **Azure Resource distribution** aktivitet för standard-miljö.

1. Välj nedrullningsbara pilen bredvid den **+ Lägg till aktiviteter** länkar och Lägg till en grupp fas i distributionen till definition.

   ![Lägga till en distribution grupp fas](media/tutorial-build-deploy-jenkins/deployment-group-phase-in-release-definition.png) 

1. Öppna i katalogen aktivitet i **Utility** och lägger till en instans av den **kommandoskript** aktivitet.

1. Parametrarna-mallen som används i aktiviteten Azure Resource distribution anger administratörslösenordet som används för att ansluta till de virtuella datorerna.
   Du kan ange lösenord med variabeln **$(adminpassword)**:
   
   - Öppna den **variabler** fliken och i den **variabler** ange namnet `adminpassword`.

   - Ange administratörens lösenord.

   - Välj hänglåsikonen ”” bredvid textrutan värdet för att skydda lösenordet. 

## <a name="configure-the-azure-resource-group-deployment-task"></a>Konfigurera aktiviteten Azure Resource distribution

Den **Azure Resource distribution** aktivitet används för att skapa distributionsgruppen. Konfigurera enligt följande:

* **Azure-prenumeration:** Välj en anslutning i listan under **tillgängliga Azure Service anslutningar**. 
  Om inga anslutningar visas väljer du **hantera**väljer **nya tjänstslutpunkten** sedan **Azure Resource Manager**, och följ anvisningarna.
  Gå tillbaka till din versionen definition, uppdatera det **AzureRM prenumeration** listan och väljer den anslutning som du skapade.

* **Resursgruppen**: Ange ett namn för resursgruppen som du skapade tidigare.

* **Plats**: Välj en region för distributionen.

  ![Skapa en ny resursgrupp](media/tutorial-build-deploy-jenkins/provision-web-server.png)

* **Platsen mall**:`URL of the file`

* **Mallen länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.json`

* **Mallen parametrar länken**:`{your-git-repo}/ARM-Templates/UbuntuWeb1.parameters.json`

* **Åsidosätt mallparametrar**: en lista över åsidosättningen värden, till exempel: `-location {location} -virtualMachineName {machine] -virtualMachineSize Standard_DS1_v2 -adminUsername {username} -virtualNetworkName fabrikam-node-rg-vnet -networkInterfaceName fabrikam-node-websvr1 -networkSecurityGroupName fabrikam-node-websvr1-nsg -adminPassword $(adminpassword) -diagnosticsStorageAccountName fabrikamnodewebsvr1 -diagnosticsStorageAccountId Microsoft.Storage/storageAccounts/fabrikamnodewebsvr1 -diagnosticsStorageAccountType Standard_LRS -addressPrefix 172.16.8.0/24 -subnetName default -subnetPrefix 172.16.8.0/24 -publicIpAddressName fabrikam-node-websvr1-ip -publicIpAddressType Dynamic`.<br />Lägga till egna specifika värden för {platshållare}. 

* **Aktivera krav**:`Configure with Deployment Group agent`

* **TFS/VSTS endpoint**: Välj **Lägg till** och i dialogrutan ”Lägg till ny Team Foundation Server-teamets Services anslutning” Välj **Token för autentisering**. Ange ett namn för anslutningen och Webbadressen för ditt team projekt. Generera och ange sedan en [personlig åtkomst-Token (PATRIK)]( https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) att autentisera anslutningen till team projekt.

  ![Skapa en personlig åtkomsttoken](media/tutorial-build-deploy-jenkins/create-a-pat.png)

* **Grupprojekt**: Välj det aktuella projektet.

* **Distributionsgruppen**: Ange namnet på samma distribution som du använde i den **resursgruppen** parameter.

Standardinställningarna för aktiviteten Azure Resource distribution är att skapa eller uppdatera en resurs och gör inkrementellt. Uppgiften skapar de virtuella datorerna första gången körs, och därefter bara uppdatera dem.

## <a name="configure-the-shell-script-task"></a>Konfigurera aktiviteten Shell-skript

Den **kommandoskript** aktivitet används för att ange konfigurationen för ett skript köras på varje server för att installera Node.js och starta appen. Konfigurera enligt följande:

* **Script sökvägen**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`

* **Ange fungerande Directory**:`Checked`

* **Arbetskatalog**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
   
## <a name="rename-and-save-the-release-definition"></a>Byt namn på och spara versionen definition

1. Redigera namnet på definitionen versionen till det namn du angav i den **efter build åtgärder** för versionen i Jenkins. Jenkins kräver det här namnet för att kunna utlösa en ny version när artefakter källa har uppdaterats.

1. Du kan också ändra namnet på miljön genom att klicka på namnet. 

1. Välj **spara**, och välj **OK**.

## <a name="start-a-manual-deployment"></a>Starta en manuell distribution

1. Välj **+ versionen** och välj **skapa versionen**.

1. Välj build du slutfört i den markerade listrutan och välj **skapa**.

1. Välj länken versionen i popup-meddelande. Till exempel ”: versionen **Release 1** har skapats”.

1. Öppna den **loggar** fliken kan du titta på utmatningen versionen.

1. Öppna URL-Adressen till en av de servrar som du har lagt till i gruppen för distribution i din webbläsare. Ange till exempel`http://{your-server-ip-address}`

## <a name="start-a-cicd-deployment"></a>Starta en CI/CD-distribution

1. I definitionen versionen avmarkera den **aktiverad** kryssrutan i den **alternativ för åtkomstkontroll** i inställningarna för distribution av Azure Resource grupp-aktivitet.
   För framtida distributioner i den befintliga distributionsgruppen behöver inte köra aktiviteten igen.

1. Gå till Git-lagerkälla och ändra innehållet i den **h1** rubriken i filen [app/views/index.jade](https://github.com/azooinmyluggage/fabrikam-node/blob/master/app/views/index.jade).

1. Genomför ändringarna.

1. Efter några minuter ser du en ny utgåva som skapats i den **versioner** i Team Services eller TFS. Öppna versionen om du vill se distributionen äger rum. Grattis!

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen automatiserad distribution av en app på Azure med hjälp av Jenkins bygg- och Team Services för versionen. Du har lärt dig hur till:

> [!div class="checklist"]
> * Skapa din app i Jenkins
> * Konfigurera Jenkins för integrering med Team Services
> * Skapa en distributionsgrupp för de virtuella Azure-datorerna
> * Skapa en definition av versionen som konfigurerar de virtuella datorerna och distribuerar appen

Gå till nästa kurs vill veta mer om hur du distribuerar ett ljus (Linux, Apache, MySQL och PHP) stacken.

> [!div class="nextstepaction"]
> [Distribuera LAMP-stack](tutorial-lamp-stack.md)