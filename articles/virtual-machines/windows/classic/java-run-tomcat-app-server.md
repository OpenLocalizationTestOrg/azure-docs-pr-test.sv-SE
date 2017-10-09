---
title: "aaaRun Java-programservern på en klassisk Azure-VM | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen och visar hur toocreate Windows virtuell dator och konfigurera den toorun Apache Tomcat-programserver."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 2d9f586c9f628d3738522b320996b95b078d7454
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-java-application-server-on-a-virtual-machine-created-with-hello-classic-deployment-model"></a>Hur toorun Java-programservern på en virtuell dator skapas med hello klassiska distributionsmodellen
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. En Resource Manager mallen toodeploy en webapp med Java 8 och Tomcat, se [här](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).

Du kan använda en virtuell dator tooprovide serverfunktioner med Azure. Exempelvis kan en virtuell dator som körs i Azure vara konfigurerade toohost Java-programservern, till exempel Apache Tomcat.

När du har slutfört den här guiden kommer du ha en förståelse av hur toocreate en virtuell dator körs på Azure och konfigurera den toorun Java-programservern. Du lär dig och utföra hello följande uppgifter:

* Hur toocreate en virtuell dator som har en Java Development Kit (JDK) redan har installerats.
* Hur tooremotely inloggningen tooyour virtuella datorn.
* Hur tooinstall Java-programservern--Apache Tomcat--på den virtuella datorn.
* Hur toocreate en slutpunkt för den virtuella datorn.
* Hur tooopen en hello-port i brandväggen för programservern.

hello slutföra Installationsresultat i Tomcat som körs på en virtuell dator.

![Virtuell dator som kör Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a>toocreate en virtuell dator
1. Logga in toohello [Azure-portalen](https://portal.azure.com).  
2. Klicka på **ny**, klickar du på **Compute**, klicka på **se alla** i hello **aktuella appar**.
3. Klicka på **JDK**, klickar du på **JDK 8** i hello **JDK** fönstret.  
   Virtuella bilder som stöder **JDK 6** och **JDK 7** är tillgängliga om du har äldre program som inte är redo toorun i JDK 8.
4. Välj i rutan hello JDK 8 **klassiska**, klicka på **skapa**.
5. I hello **grunderna** bladet:
   1. Ange ett namn för hello virtuella datorn.
   2. Ange ett namn för Hej administratör i hello **användarnamn** fältet. Kom ihåg detta namn och hello tillhörande lösenord som följer i hello nästa fält. Du behöver dem när du loggar in via fjärranslutning toohello virtuella datorn.
   3. Ange ett lösenord i hello **nytt lösenord** fältet och igen i hello **Bekräfta lösenord** fältet. Lösenordet är för hello administratörskonto.
   4. Välj lämplig hello **prenumeration**.
   5. För hello **resursgruppen**, klickar du på **Skapa nytt** och ange hello namnet på en ny resursgrupp. Klicka på **använda befintliga** och välj en av hello tillgängliga resursgrupper.
   6. Välj en plats där hello virtuella dator finns, som **södra centrala USA**.
6. Klicka på **Nästa**.
7. I hello **virtuella bildstorleken** bladet väljer **A1 Standard** eller en annan lämplig bild.
8. Klicka på **Välj**.

9. I hello **inställningar** bladet, klickar du på **OK**. Du kan använda hello standardvärden som tillhandahålls av Azure.  
10. I hello **sammanfattning** bladet, klickar du på **OK**.

## <a name="tooremotely-sign-in-tooyour-virtual-machine"></a>tooremotely inloggning tooyour virtuell dator
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **virtuella datorer (klassisk)**. Om det behövs klickar du på **fler tjänster** i hello nedre vänstra hörnet under hello servicekategorier. Hej **virtuella datorer (klassisk)** posten visas i hello **Compute** grupp.
3. Klicka på hello virtuell dator som du vill toosign i hello namn.
4. När hello virtuella datorn har startat tillåter en meny hello överst i fönstret hello anslutningar.
5. Klicka på **Anslut**.
6. Svara toohello prompter som behövs tooconnect toohello virtuell dator. Normalt spara eller öppna hello RDP-fil som innehåller hello anslutningsinformation. Du kan ha toocopy hello URL-adress: port som hello sista delen av hello första raden i hello RDP-filen och klistra in den i inloggning fjärrprogram.

## <a name="tooinstall-a-java-application-server-on-your-virtual-machine"></a>tooinstall Java-programservern på den virtuella datorn
Du kan kopiera en Java application server tooyour virtuell dator eller installera en Java-programservern via ett installationsprogram.

Den här kursen använder Tomcat som hello Java application server tooinstall.

1. När du är inloggad tooyour virtuella datorn, öppna en webbläsarsession för[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).
2. Dubbelklicka på hello länk för **32-bitars/64-bitars Windows installationsprogram**. Med den här tekniken kan installeras Tomcat som en Windows-tjänst.
3. Välj toorun hello installer.
4. Inom hello **Apache Tomcat installationsprogrammet** , följ hello uppmanas tooinstall Tomcat. Hello enligt den här kursen räcker det bra att acceptera hello standard. När du når hello **hello Slutför installationsguiden för Apache Tomcat** dialogrutan kan du om du vill kontrollera **köra Apache Tomcat** toohave Tomcat start nu. Klicka på **Slutför** toocomplete hello Tomcat installationsprocessen.

## <a name="toostart-tomcat"></a>toostart Tomcat

Du kan starta Tomcat manuellt genom att öppna en kommandotolk på den virtuella datorn körs hello-kommandot **net&nbsp;starta&nbsp;Tomcat8**.

När Tomcat körs kan du komma åt Tomcat genom att ange hello URL <http://localhost: 8080> i hello virtuell dators webbläsare.

toosee Tomcat körs från externa datorer kan du behöver toocreate en slutpunkt och öppnar en port.

## <a name="toocreate-an-endpoint-for-your-virtual-machine"></a>toocreate en slutpunkt för den virtuella datorn
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **virtuella datorer (klassisk)**.
3. Klicka på hello virtuell dator som körs på Java-programservern hello namn.
4. Klicka på **Slutpunkter**.
5. Klicka på **Lägg till**.
6. I hello **lägga till slutpunkten** dialogrutan:
   1. Ange ett namn för slutpunkten hello; till exempel **HttpIn**.
   2. Välj **TCP** för hello-protokollet.
   3. Ange **80** för hello offentlig port.
   4. Ange **8080** för hello privat port.
   5. Välj **inaktiverad** för hello flytande IP-adress.
   6. Lämna hello åtkomstkontrollistan är.
   7. Klicka på hello **OK** knappen tooclose hello dialogrutan och skapa hello slutpunkt.

## <a name="tooopen-a-port-in-hello-firewall-for-your-virtual-machine"></a>tooopen en port i brandväggen hello för den virtuella datorn
1. Logga in tooyour virtuella datorn.
2. Klicka på **starta Windows**.
3. Klicka på **Kontrollpanelen**.
4. Klicka på **System och säkerhet**, klickar du på **Windows-brandväggen**, och klicka sedan på **avancerade inställningar**.
5. Klicka på **regler för inkommande trafik**, och klicka sedan på **ny regel**.  
   ![Ny inkommande regel][NewIBRule]
6. För hello **regeltyp**väljer **Port**, och klicka sedan på **nästa**.  
   ![Ny inkommande regel port][NewRulePort]
7. På hello **protokoll och portar** väljer **TCP**, ange **8080** som hello **specifika lokala portar**, och klicka sedan på **Nästa**.  
  ![Ny inkommande regel][NewRuleProtocol]
8. På hello **åtgärd** väljer **Tillåt hello anslutning**, och klicka sedan på **nästa**.
   ![Ny inkommande Regelåtgärd][NewRuleAction]
9. På hello **profil** kontrollerar du att **domän**, **privata**, och **offentliga** är markerade och klickar sedan på **nästa** .
   ![Ny inkommande regel profil][NewRuleProfile]
10. På hello **namn** skärmen, ange ett namn för hello regel som **HttpIn** (hello Regelnamnet är inte obligatoriska toomatch hello slutpunktsnamn, men), och klicka sedan på **Slutför**.  
    ![Namn på ny inkommande regel][NewRuleName]

Tomcat webbplatsen ska nu visas på en extern webbläsare. Ange en URL i formatet hello i hello webbläsarens adressfönstret  **http://*din\_DNS\_namn*. cloudapp.net**, där ***din\_DNS\_namn*** är hello DNS-namn du angav när du skapade hello virtuell dator.

## <a name="application-lifecycle-considerations"></a>Programmet livscykel överväganden
* Du kan skapa egna exempelwebbappens Arkiv (WAR) och lägga till den toohello **webbappar** mapp. Till exempel skapa ett grundläggande Service (Java JSP sidan) dynamiskt webbprojekt och exportera det som en WAR-fil. Kopiera hello WAR toohello Apache Tomcat **webbappar** mapp på hello virtuell dator, kör det i en webbläsare.
* Som standard när hello Tomcat-tjänsten är installerad, är den inställd toostart manuellt. Du kan växla den toostart automatiskt med hjälp av hello snapin-modulen tjänster. Starta hello snapin-modulen Tjänster genom att klicka på **Windows starta**, **Administrationsverktyg**, och sedan **Services**. Dubbelklicka på hello **Apache Tomcat** och ange **starttyp** för**automatisk**.

    ![Ange en service toostart automatiskt][service_automatic_startup]

    hello är fördelen med Tomcat startar automatiskt att den börjar köras när hello virtuella datorn startas (till exempel efter programuppdateringar som kräver en omstart har installerats).

## <a name="next-steps"></a>Nästa steg
Du kan lära dig om andra tjänster (t.ex Azure Storage, service bus och SQL-databas) som du vill tooinclude med Java-program. Visa information om hello tillgänglig på hello [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from hello "toocreate an ednpoint for your virtual mache" 3/17/2017,
     toouse hello new portal.
6. In hello **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In hello **New endpoint details** dialog box:
-->
