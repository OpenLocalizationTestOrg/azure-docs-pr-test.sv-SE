---
title: "aaaContinuous bygg- och -integrering för Azure Service Fabric Linux Java-programmet med hjälp av Jenkins | Microsoft Docs"
description: "Skapa och integrera kontinuerligt för ditt Java-program för Linux med hjälp av Jenkins"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: 15da2cb8c759233219369ea889550da93748129f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-jenkins-toobuild-and-deploy-your-linux-java-application"></a>Använda Jenkins toobuild och distribuera Linux Java-programmet
Jenkins är ett populärt verktyg för kontinuerlig integrering och distribution av appar. Här är hur toobuild och distribuera Azure Service Fabric-program med hjälp av Jenkins.

## <a name="general-prerequisites"></a>Allmänna krav
- Du måste ha Git installerat lokalt. Du kan installera hello lämplig Git-version från [hello Git hämtar sidan](https://git-scm.com/downloads), baserat på ditt operativsystem. Om du är ny tooGit mer information om den från hello [Git dokumentationen](https://git-scm.com/docs).
- Har hello Service Fabric-Jenkins plugin praktiska. Du kan ladda ned det från [Service Fabric-nedladdningar](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Konfigurera Jenkins i ett Service Fabric-kluster

Du kan konfigurera Jenkins i eller utanför ett Service Fabric-kluster. hello de följande avsnitten visar hur tooset den i ett kluster när du använder ett Azure storage kontot toosave hello tillstånd hello behållaren instans.

### <a name="prerequisites"></a>Krav
1. Ha ett Service Fabric Linux-kluster redo. En Service Fabric-klustret redan skapat från hello Azure-portalen har Docker installerad. Om du kör hello kluster lokalt, kontrollera om Docker är installerat med kommandot hello ``docker info``. Om den inte är installerad kan du installera den i enlighet med detta med hjälp av hello följande kommandon:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Har hello Service Fabric-behållarprogram distribueras på hello klustret med hjälp av hello följande steg:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
cd service-fabric-java-getting-started/Services/JenkinsDocker/
```

3. Du behöver hello ansluta alternativet information om hello Azure storage-filresurs, där du vill att toopersist hello tillstånd hello Jenkins behållare instans. Om du använder hello Microsoft Azure-portalen för hello detsamma, kontrollera gör hello - skapa ett Azure storage-konto säg ``sfjenkinsstorage1``. Skapa en **filresurs** under detta lagringskonto säger ``sfjenkins``. Klicka på **Anslut** för hello-filresursen och Observera hello värden den visas under **ansluter från Linux**, säg detta skulle se ut så här -
```sh
sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
```

4. Uppdatera hello platshållare för värden i hello ```setupentrypoint.sh``` skriptet med detaljer om motsvarande azure-lagring.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
Ersätt ``[REMOTE_FILE_SHARE_LOCATION]`` med hello värdet ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` hello utdata från hello ansluta i punkt 3 ovan.
Ersätt ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` med hello värdet ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` från punkt 3 ovan.

5. Anslut toohello kluster och installera programmet hello.
```azurecli
sfctl cluster select --endpoint http://PublicIPorFQDN:19080   # cluster connect command
bash Scripts/install.sh
```
Detta installerar en Jenkins behållare på hello klustret och kan övervakas med hjälp av hello Service Fabric Explorer.

### <a name="steps"></a>Steg
1. Från din webbläsare går för``http://PublicIPorFQDN:8081``. Det ger hello hello inledande admin lösenord toosign i sökvägen. Du kan fortsätta toouse Jenkins som administratörsanvändare. Eller du kan skapa och ändra hello användare när du har loggat in med hello inledande administratörskonto.

   > [!NOTE]
   > Kontrollera att hello 8081 port har angetts som hello programmet endpoint port när du skapar hello klustret.
   >

2. Hämta hello behållaren instans-ID genom att använda ``docker ps -a``.
3. Secure Shell (SSH) inloggning toohello behållare och klistra in hello sökväg som visades i hello Jenkins portal. Om exempelvis i hello portal visas hello sökvägen `PATH_TO_INITIAL_ADMIN_PASSWORD`, kör hello följande:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Konfigurera GitHub toowork med Jenkins, med hjälp av hello anvisningarna i [Generera en ny SSH-nyckel och lägga till den toohello SSH-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    * Använd hello instruktioner från GitHub toogenerate hello SSH-nyckel och tooadd hello SSH key toohello GitHub-konto som är värd för databasen.
    * Kör hello-kommandon som nämns i hello föregående länk i hello Jenkins Docker shell (och inte på din värd).
    * toosign i toohello Jenkins shell från värden, Använd hello följande kommando:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Konfigurera Jenkins utanför ett Service Fabric-kluster

Du kan konfigurera Jenkins i eller utanför ett Service Fabric-kluster. Hej följande avsnitt visas hur tooset den utanför ett kluster.

### <a name="prerequisites"></a>Krav
Du måste toohave Docker installerad. hello följande kommandon kan vara används tooinstall Docker från hello terminal:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Nu när du kör ``docker info`` i hello terminal, bör visas i hello utdata som hello Docker-tjänsten körs.

### <a name="steps"></a>Steg
  1. Hämta hello Service Fabric Jenkins behållare avbildningen:``docker pull raunakpandya/jenkins:v1``
  2. Kör hello behållaren avbildningen:``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Hämta hello-ID för hello behållaren image-instansen. Du kan visa alla hello Docker-behållare med hello kommando``docker ps –a``
  4. Logga in på toohello Jenkins portal med hello följande steg:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Om behållar-ID:t är 2d24a73b5964 ska du använda 2d24.
    * Den här lösenord krävs för att logga in toohello Jenkins instrumentpanelen från portalen, vilket är``http://<HOST-IP>:8080``
    * När du loggar in för hello första gången, du kan skapa ditt eget konto och använda det för framtida eller du kan fortsätta toouse hello-administratörskonto. När du skapar en användare måste toocontinue med.
  5. Konfigurera GitHub toowork med Jenkins, med hjälp av hello anvisningarna i [Generera en ny SSH-nyckel och lägga till den toohello SSH-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
        * Använd hello instruktioner från GitHub toogenerate hello SSH-nyckel och tooadd hello SSH key toohello GitHub-konto som är värd för hello-databasen.
        * Kör hello-kommandon som nämns i hello föregående länk i hello Jenkins Docker shell (och inte på din värd).
      * toosign i toohello Jenkins shell från värden, Använd hello följande kommandon:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Se till att hello kluster eller en dator där hello Jenkins behållare bilden finns har en offentlig IP-adress. Detta gör att hello Jenkins instans tooreceive meddelanden från GitHub.

## <a name="install-hello-service-fabric-jenkins-plug-in-from-hello-portal"></a>Installera hello Service Fabric-Jenkins plugin från hello-portalen

1. Gå för``http://PublicIPorFQDN:8081``
2. Hej Jenkins instrumentpanelen, Välj **hantera Jenkins** > **hantera plugin-program** > **Avancerat**.
Här kan du ladda upp ett plugin-program. Välj **Välj fil**, och välj sedan hello **serviceFabric.hpi** fil som du hämtade under krav. När du väljer **överför**, Jenkins installeras automatiskt hello plugin-programmet. Tillåt en omstart om det begärs.

## <a name="create-and-configure-a-jenkins-job"></a>Skapa och konfigurera ett Jenkins-jobb

1. Skapa en **ny post** från instrumentpanelen.
2. Ange ett namn (till exempel **MyJob**). Välj **free-style project** (freestyle-projekt) och klicka på **OK**.
3. Gå hello jobbet sidan och klicka på **konfigurera**.

   a. Under hello Allmänt under **GitHub projekt**, ange URL för GitHub. Den här URL-värdar hello Service Fabric Java-program som du vill toointegrate med hello Jenkins kontinuerlig integration, kontinuerlig distribution (CI/CD) flöda (till exempel ``https://github.com/sayantancs/SFJenkins``).

   b. Under hello **källa kod Management** väljer **Git**. Ange hello databasen URL som är värd för hello Service Fabric Java-program som du vill toointegrate med hello Jenkins CI/CD-flöde (till exempel ``https://github.com/sayantancs/SFJenkins.git``). Dessutom kan du ange vilka gren toobuild (till exempel **/master-**).
4. Konfigurera din *GitHub* (som är värd hello databasen) så att den kan tootalk tooJenkins. Använd hello följande steg:

   a. Gå tooyour GitHub-lagringsplatsen sidan. Gå för**inställningar** > **integreringar och tjänster**.

   b. Välj **lägga till tjänsten**, typen **Jenkins**, och välj hello **Jenkins GitHub-plugin-programmet**.

   c. Ange din Jenkins-webhooksadress (som standard ska den vara ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klicka på **Lägg till/Uppdatera tjänsten**.

   d. En test-händelse skickas tooyour Jenkins instans. Du bör se en grön bock av hello webhook i GitHub och skapar ditt projekt.

   e. Under hello **Skapa utlösare** väljer som skapa alternativ. I det här exemplet vill tootrigger en version när vissa push toohello databasen sker. Därför väljer du **GitHub hook trigger for GITScm polling** (GitHub-hookutlösare för GITScm-avsökning). (Tidigare det här alternativet anropades **bygga när en ändring pushas tooGitHub**.)

   f. Under hello **Skapa avsnittet**, hello listrutan **Lägg till build steg**, Välj alternativet för hello **anropa Gradle skript**. I hello widget som medföljer, ange hello sökväg för**rot skapa skriptet** för ditt program. Den hämtar build.gradle från hello-sökvägen och fungerar därför. Om du skapar ett projekt med namnet ``MyActor`` (med hello Eclipse plugin-program eller Yeoman generator) ska innehålla hello rot build skriptet ``${WORKSPACE}/MyActor``. Se följande skärmbild ett exempel på hur det ser ut hello:

    ![Service Fabric Jenkins Build-åtgärd][build-step]

   g. Från hello **efter Build åtgärder** listrutan, Välj **distribuera Service Fabric-projekt**. Här behöver du tooprovide klustret information där hello Jenkins kompileras Service Fabric-programmet distribueras. Du kan också ange ytterligare programinformation används toodeploy hello program. Se följande skärmbild ett exempel på hur det ser ut hello:

    ![Service Fabric Jenkins Build-åtgärd][post-build-step]

   > [!NOTE]
   > här hello-klustret kan vara detsamma som hello en värd hello Jenkins program, om du använder Service Fabric toodeploy hello Jenkins behållare.
   >

## <a name="next-steps"></a>Nästa steg
GitHub och Jenkins har nu konfigurerats. Överväg att göra vissa exempel ändras i din ``MyActor`` projekt i hello databasen exempelvis https://github.com/sayantancs/SFJenkins. Push-ändringar-tooa remote ``master`` gren (eller valfri gren som du har konfigurerat toowork med). Detta utlöser hello Jenkins jobbet ``MyJob``, som du har konfigurerat. Hämtar hello ändringar från GitHub, skapar dem och distribuerar hello programmet toohello klusterslutpunkten du angav i efter build-åtgärder.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png
