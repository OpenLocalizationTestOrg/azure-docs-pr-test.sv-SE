---
title: "aaaCreate en Jenkins server på Azure"
description: "Installera Jenkins på en virtuell dator i Azure Linux från hello Jenkins lösningsmall och skapa ett exempelprogram för Java."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 82ab2ac52594acba131414b449b608978591d4b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-hello-azure-portal"></a>Skapa en Jenkins-server på en Azure Linux-VM från hello Azure-portalen

Den här snabbstarten visar hur tooinstall [Jenkins](https://jenkins.io) på en Ubuntu Linux VM hello verktyg och toowork plugin-program som har konfigurerats med Azure. När du är klar körs en Jenkins-server i Azure och skapar en Java-exempelapp från [GitHub](https://github.com).

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration
* Åtkomst tooSSH på kommandoraden för din dator (till exempel hello Bash shell eller [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-jenkins-vm-from-hello-solution-template"></a>Skapa hello Jenkins VM från hello lösningsmall

Öppna hello [marketplace-avbildning för Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) i webbläsaren och välj **hämta IT nu** från hello vänster sida av hello-sidan. Granska hello priser information och välj **Fortsätt**och välj **skapa** tooconfigure hello Jenkins server i hello Azure-portalen. 
   
![Dialogruta i Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

I hello **Konfigurera grundläggande inställningar** fliken, Fyll i hello följande fält:

![Konfigurera grundläggande inställningar](./media/install-jenkins-solution-template/ap-basic.png)

* Använd **Jenkins** som **namn**.
* Ange ett **användarnamn**. hello användarnamn måste uppfylla [specifika krav](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
* Välj **lösenord** som hello **autentiseringstyp** och ange ett lösenord. hello lösenordet måste innehålla en versal bokstav, en siffra och ett specialtecken.
* Använd **myJenkinsResourceGroup** för hello **resursgruppen**.
* Välj hello **östra USA** [Azure-region](https://azure.microsoft.com/regions/) från hello **plats** listrutan.

Välj **OK** tooproceed toohello **konfigurera ytterligare alternativ** fliken. Ange ett unikt namn tooidentify hello Jenkins domänserver och välj **OK**.

![Ställ in ytterligare alternativ](./media/install-jenkins-solution-template/ap-addtional.png)  

 När valideringen lyckas, välja **OK** igen från hello **sammanfattning** fliken. Välj slutligen **inköp** toocreate hello Jenkins VM. När servern är klar kan få du ett meddelande i hello Azure-portalen:   

![Meddelande om att Jenkins är klar](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-toojenkins"></a>Ansluta tooJenkins

Navigera tooyour virtuella datorn (till exempel http://jenkins2517454.eastus.cloudapp.azure.com/) i webbläsaren. Hej Jenkins konsolen inte är tillgänglig via osäker HTTP så anvisningar finns på hello sidan tooaccess hello Jenkins konsolen på ett säkert sätt från datorn med hjälp av en SSH-tunnel.

![Låsa upp Jenkins](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Ställ in hello tunneln med hello `ssh` på hello sida från kommandoraden hello ersätter `username` med hello namnet hello virtuella administratörsanvändare valt tidigare när du ställer in hello virtuell dator från hello lösningsmall.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

När du har startat hello tunnel, navigera toohttp://localhost:8080 / på den lokala datorn. 

Hämta hello initialt lösenord genom att köra följande kommando på kommandoraden hello medan du är ansluten via SSH toohello Jenkins VM hello.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Låsa upp hello Jenkins instrumentpanel för hello första gången du använder den här initialt lösenord.

![Låsa upp Jenkins](./media/install-jenkins-solution-template/jenkins-unlock.png)

Välj **installera plugin-program för föreslagna** på hello nästa sida och sedan skapa en Jenkins admin används tooaccess hello Jenkins instrumentpanel för användare.

![Jenkins är redo!](./media/install-jenkins-solution-template/jenkins-welcome.png)

Hej Jenkins servern är nu redo toobuild kod.

## <a name="create-your-first-job"></a>Skapa ditt första virtuella jobb

Välj **skapa nya jobb** hello Jenkins konsolen namnge den **mySampleApp** och välj **Freestyle projektet**och välj **OK**.

![Skapa ett nytt jobb](./media/install-jenkins-solution-template/jenkins-new-job.png) 

Välj hello **källa kod Management** fliken, aktivera **Git**, och ange följande URL i hello **databasen URL** fält:`https://github.com/spring-guides/gs-spring-boot.git`

![Definiera hello Git repo](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Välj hello **skapa** fliken och markera sedan **Lägg till build steg**, **anropa Gradle skriptet**. Välj **Use Gradle Wrapper** (Använd Gradle-omslutning) och ange `complete` i **Wrapper location** (Omslutningsplats) och `build` för **Tasks** (Uppgifter).

![Använd hello Gradle wrapper toobuild](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Välj **Avancerat..** och ange sedan `complete` i hello **rot skapa skript** fältet. Välj **Spara**.

![Ange avancerade inställningar i hello Gradle wrapper build steg](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-hello-code"></a>Skapa hello kod

Välj **skapa nu** toocompile hello kod och paketet hello sample-appen. När din build är klar väljer du hello **arbetsytan** länk för hello-projekt.

![Bläddra toohello arbetsytan tooget hello JAR-filen från hello build](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Navigera för`complete/build/libs` och kontrollera hello `gs-spring-boot-0.1.0.jar` finns det tooverify att din build lyckades. Din Jenkins servern är nu redo toobuild projekt i Azure.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Lägga till virtuella Azure-datorer som Jenkins-agenter](jenkins-azure-vm-agents.md)
