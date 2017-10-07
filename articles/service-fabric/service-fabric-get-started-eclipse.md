---
title: "aaaAzure Service Fabric-plugin-program för Eclipse | Microsoft Docs"
description: "Kom igång med hello Service Fabric-plugin-programmet för Eclipse."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2016
ms.author: saysa
ms.openlocfilehash: 4ba5a28a6282387249a2bd4e62314e891ff04162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Service Fabric-plugin-program för utveckling av Java-program i Eclipse
Eclipse är en av hello används mest integrerad utvecklingsmiljöer (IDEs) för Java-utvecklare. I den här artikeln beskriver vi hur tooset in Eclipse development environment toowork med Azure Service Fabric. Lär dig hur tooinstall hello Service Fabric-plugin-program, skapa ett Service Fabric-program och distribuera din Service Fabric application tooa lokal eller fjärransluten Service Fabric-kluster i Eclipse Neon.

## <a name="install-or-update-hello-service-fabric-plug-in-in-eclipse-neon"></a>Installera eller uppdatera hello Service Fabric-plugin-programmet i Eclipse Neon
Du kan installera ett Service Fabric-plugin-program i Eclipse. hello plugin-programmet kan underlätta hello processen att skapa och distribuera Java-tjänster.

1.  Se till att du har hello senaste versionen av Eclipse Neon och hello senaste versionen av Buildship (1.0.17 eller senare) installerat:
    -   toocheck hello versioner av installerade komponenter i Eclipse Neon blir för**hjälp** > **installationsinformationen**.
    -   tooupdate Buildship, se [Eclipse Buildship: Eclipse plugin-program för Gradle][buildship-update].
    -   toocheck för och installera uppdateringar för Eclipse Neon gå för**hjälp** > **söka efter uppdateringar**.

2.  tooinstall hello Service Fabric-plugin-programmet i Eclipse Neon gå för**hjälp** > **installera ny programvara**.
  1.    I hello **arbeta med** ange **http://dl.microsoft.com/eclipse**.
  2.    Klicka på **Lägg till**.

         ![Service Fabric-plugin-program för Eclipse Neon][sf-eclipse-plugin-install]
  3.    Välj hello Service Fabric-plugin-program och klicka sedan på **nästa**.
  4.    Slutför hello installationssteg och acceptera licensvillkoren för hello.

Kontrollera att du har hello senaste versionen om du redan har hello Service Fabric plugin-program installerat. Gå toocheck efter tillgängliga uppdateringar för**hjälp** > **installationsinformationen**. Välj Service Fabric i hello lista över installerade plugin-program, och klicka sedan på **uppdatering**. Tillgängliga uppdateringar installeras.

> [!NOTE]
> Installera eller uppdatera hello Service Fabric-plugin-programmet är långsam, kanske på grund av tooan Eclipse inställningen. Eclipse samlar in metadata i alla ändringar tooupdate platser som har registrerats med Eclipse-instans. Gå toospeed hello registreringen av söker efter och installera en uppdatering med Service Fabric plugin-programmet för**tillgänglig programvara platser**. Avmarkera kryssrutorna för hello för alla platser förutom hello som pekar toohello Service Fabric-plugin-plats (http://dl.microsoft.com/eclipse/azure/servicefabric).

## <a name="create-a-service-fabric-application-in-eclipse"></a>Skapa ett Service Fabric-program i Eclipse

1.  I Eclipse Neon går för**filen** > **ny** > **andra**. Välj **Service Fabric Project** (Service Fabric Project) och klicka sedan på **Next** (Nästa).

    ![Service Fabric, ny projektsida 1][create-application/p1]

2.  Ange ett namn för ditt projekt och klicka sedan på **Next** (Nästa).

    ![Service Fabric, ny projektsida 2][create-application/p2]

3.  Välj i hello lista över mallar **tjänstmall**. Välj typ av tjänstmall (Actor (Aktör), Stateless (Tillståndslös), Container (Behållare) eller Guest Binary (Gäst, binär)) och klicka sedan på **Next** (Nästa).

    ![Service Fabric, ny projektsida 3][create-application/p3]

4.  Ange tjänstnamnet hello tjänsten information och klicka sedan på **Slutför**.

    ![Service Fabric, ny projektsida 4][create-application/p4]

5. När du skapar din första Service Fabric-projekt i hello **öppna associerade perspektiv** dialogrutan klickar du på **Ja**.

    ![Service Fabric, ny projektsida 5][create-application/p5]

6.  Det nya projektet ser ut så här:

    ![Service Fabric, ny projektsida 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a>Skapa och distribuera ett Service Fabric-program i Eclipse

1.  Högerklicka på det nya Service Fabric-programmet och välj sedan **Service Fabric**.

    ![Service Fabric-snabbmeny][publish/RightClick]

2. Välj hello-alternativ som du vill använda i hello undermenyn:
    -   toobuild hello program utan rensas, klicka på **bygga program**.
    -   toodo en ren version av programmet hello, klickar du på **återskapa programmet**.
    -   tooclean hello tillämpningen av inbyggda artefakter, klickar du på **ren programmet**.

3.  Du kan också välja att distribuera, ta bort eller publicera programmet på den här menyn:
    -   toodeploy tooyour lokala klustret och klicka på **distribuera programmet**.
    -   I hello **publicera programmet** dialogrutan väljer du en profil:
        -  **Local.json**
        -  **Cloud.json**

     Filerna JavaScript Object Notation (JSON) lagrar information (till exempel anslutningens slutpunkter och säkerhetsinformation) som är nödvändiga tooconnect tooyour lokala eller ett kluster i molnet (Azure).

  ![Publicera-menyn för Service Fabric][publish/Publish]

Ett annat sätt toodeploy Service Fabric-program genom att använda Eclipse körs konfigurationer.

  1.    Gå för**kör** > **kör konfigurationer**.
  2.    Under **Gradle projekt**väljer hello **ServiceFabricDeployer** kör konfigurationen.
  3.    I hello högra fönstret på hello **argument** fliken för **publishProfile**väljer **lokala** eller **moln**.  hello standardvärdet är **lokala**. toodeploy tooa remote eller molnet kluster, Välj **moln**.
  4.    tooensure att hello rätt information fylls i hello publicera profiler, redigera **Local.json** eller **Cloud.json** efter behov. Du kan lägga till eller uppdatera information om slutpunkt och säkerhetsreferenser.
  5.    Se till att **Working Directory** pekar toohello program som du vill toodeploy. toochange Hej programmet, klickar du på hello **arbetsytan** och välj hello-program som du vill.
  6.    Klicka på **Apply** (Verkställ) och sedan på **Run** (Kör).

Ditt program skapas och distribueras efter en liten stund. Du kan övervaka hello Distributionsstatus i Service Fabric Explorer.  

## <a name="add-a-service-fabric-service-tooyour-service-fabric-application"></a>Lägg till ett Service Fabric-tjänsten tooyour Service Fabric-program

tooadd ett Service Fabric-tooan befintliga Service Fabric tjänstprogram, hello följande steg:

1.  Högerklicka på hello projekt du tooadd en tjänst, och klickar sedan på **Service Fabric**.

    ![Service Fabric, lägg till tjänst, sida 1][add-service/p1]

2.  Klicka på **lägga till Service Fabric Service**, och fullständig hello uppsättning steg tooadd en toohello service-projekt.
3.  Välj hello tjänstmall du vill tooadd tooyour projektet och klicka sedan på **nästa**.

    ![Service Fabric, lägg till tjänst, sida 2][add-service/p2]

4.  Ange hello tjänstnamn (och annan information som behövs) och klicka sedan på hello **lägga till tjänsten** knappen.  

    ![Service Fabric, lägg till tjänst, sida 3][add-service/p3]

5.  När hello-tjänsten har lagts ut din övergripande projektstruktur liknande toohello följande projekt:

    ![Service Fabric, lägg till tjänst, sida 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>Redigera manifestversioner för Service Fabric Java-programmet

tooedit manifestet versioner, högerklicka på projektet hello finns för**Service Fabric** och välj **redigera Manifest versioner...**  från hello menyn för verben. Hello i guiden kan du uppdatera hello manifest för programmet manifestet, tjänstmanifestet och hello versioner för **kod**, **Config** och **Data** paket.

Om du markerar alternativet hello **automatiskt uppdatera programmet och service versioner** och uppdatera sedan en version, sedan hello manifestet versioner uppdateras automatiskt. toogive ett exempel du först välja hello kryssrutan och sedan uppdatera hello version av **kod** version från 0.0.0 too0.0.1 och klicka på **Slutför**, service manifest version och programmanifestet version kommer att uppdateras automatiskt too0.0.1.

## <a name="upgrade-your-service-fabric-java-application"></a>Uppgradera ditt Service Fabric Java-program

För en uppgraderingsscenariot säger du skapade hello **App1** projekt genom att använda hello Service Fabric-plugin-programmet i Eclipse. Du har distribuerat den med hjälp av plugin-programmet hello-toocreate ett program med namnet **fabric: / App1Application**. hello programtypen är **App1AppicationType**, och hello programmet version 1.0. Nu ska tooupgrade programmet utan att påverka tillgängligheten.

Först gör ändringar tooyour program och sedan återskapa hello ändras service. Uppdatera hello ändrade tjänstens manifestfilen (ServiceManifest.xml) med hello uppdaterade versioner för hello-tjänsten (och koden, Config eller Data som är aktuellt). Dessutom ändra hello programmets manifest (ApplicationManifest.xml) med hello uppdatera versionsnumret för programmet hello och hello ändrade tjänsten.  

tooupgrade ditt program genom att använda Eclipse Neon som du kan skapa en duplicerad kör Konfigurationsprofil. Använd sedan tooupgrade programmet efter behov.

1.  Gå för**kör** > **kör konfigurationer**. Klicka på hello liten pil toohello till vänster i hello vänster, **Gradle projekt**.
2.  Högerklicka på **ServiceFabricDeployer** och välj sedan **Duplicate** (Duplicera). Ange ett nytt namn för den här konfigurationen, till exempel **ServiceFabricUpgrader**.
3.  I hello högra panelen på hello **argument** fliken, ändrar **- Pconfig = 'distribuera'** för**- Pconfig = 'uppgradera'**, och klicka sedan på **tillämpa**.

Den här processen skapar och sparar en Konfigurationsprofil som kör du någon gång tooupgrade kan använda programmet. Hello senaste uppdaterade programmets Typversion får också från hello programmanifestfilen.

uppgradering av programmet hello tar några minuter. Du kan övervaka hello programmet uppgradering i Service Fabric Explorer.

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a>Migrera gamla Service Fabric Java-program toobe används med Maven
Vi har nyligen flyttat Service Fabric Java-bibliotek från Service Fabric Java SDK tooMaven databasen. Medan hello nya program som du skapar med Eclipse genererar senaste uppdaterade projekt (som kommer att kunna toowork med Maven), kan du uppdatera din befintliga Service Fabric tillståndslös eller aktören Java-program som använder hello Service Fabric Java SDK tidigare, toouse hello Service Fabric Java beroenden från Maven. Följ anvisningarna för hello [här](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure äldre programmet fungerar med Maven.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
