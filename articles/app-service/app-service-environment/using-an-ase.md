---
title: "aaaUse en Azure Apptjänst-miljö"
description: "Hur toocreate, publicera och skala appar i Azure Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 30c89e384efc07c560254856c0ca7d4eb4b1f010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-app-service-environment"></a>Använd en Apptjänst-miljö #

## <a name="overview"></a>Översikt ##

Azure Apptjänst-miljön är en distribution av Azure App Service till ett undernät i kundens virtuella Azure-nätverket. Det består av:

- **Främre ends**: hello frontwebbservrarna är där HTTP/HTTPS avslutas i en Apptjänst-miljö (ASE).
- **Anställda**: hello arbetare är hello-resurser som är värdar för dina appar.
- **Databasen**: hello databasen innehåller information som definierar hello-miljö.
- **Lagring**: hello lagring är används toohost hello kunden publicerade appar.

> [!NOTE]
> Det finns två versioner av Apptjänstmiljö: ASEv1 och ASEv2. Du måste hantera hello resurser innan du kan använda dem i ASEv1. toolearn hur tooconfigure och hantera ASEv1, se [konfigurerar du en Apptjänst-miljö v1][ConfigureASEv1]. hello resten av den här artikeln fokuserar på ASEv2.
>
>

Du kan distribuera en ASE (ASEv1 och ASEv2) med en extern eller intern VIP för åtkomst till appen. hello-distribution med en extern VIP kallas vanligtvis en extern ASE. hello interna versionen kallas hello ILB ASE eftersom den använder en intern belastningsutjämnare (ILB). toolearn mer om hello ILB ASE finns [skapa och använda en ILB ASE][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>Skapa en webbapp i en ASE ##

toocreate ett webbprogram i en ASE du använda hello samma process som när du skapar den som vanligt, men med några mindre skillnader. När du skapar en ny programtjänstplan:

- I stället för att välja en geografisk plats i vilka toodeploy appen, Välj en ASE som din plats.
- Alla programtjänstplaner som skapats i en ASE måste finnas i en isolerad prisnivån.

Om du inte har en ASE, du kan skapa en genom att följa instruktionerna hello i [skapa en Apptjänst-miljö][MakeExternalASE].

toocreate ett webbprogram i en ASE:

1. Välj **nya** > **webb + mobilt** > **Webbapp**.

2. Ange ett namn för hello webbprogrammet. Om du redan har valt en apptjänstplan i en ASE visar hello domännamn för hello app hello ASE hello domännamn.

    ![Val av nätverksnamn för Web app][1]

3. Välj en prenumeration.

4. Ange ett namn för en ny resursgrupp eller välj **använda befintliga** och välj en hello nedrullningsbara listan.

5. Välj en befintlig programtjänstplan i din ASE eller skapa en ny genom att följa dessa steg:

    a. Välj **skapa nya**.

    b. Ange hello namn för din programtjänstplan.

    c. Välj din ASE i hello **plats** listrutan.

    d. Välj en **isolerad** prisnivån. Välj **Välj**.

    e. Välj **OK**.
    
    ![Isolerade prisnivåer][2]

6. Välj **Skapa**.

## <a name="how-scale-works"></a>Hur skala fungerar ##

Varje App Service-appen körs i en apptjänstplan. hello behållaren modellen är miljöer håller App Service-planer och programtjänstplaner håller appar. När du skalar en app du skalar hello App Service-plan och skala därför alla hello appar i hello samma plan.

I ASEv2, när du skalar en apptjänstplan läggs hello behövs infrastruktur automatiskt. Det finns en tidsfördröjning tooscale operations medan hello infrastruktur har lagts till. ASEv1, måste infrastruktur hello behövs läggas innan du kan skapa eller skala upp din programtjänstplan. 

I hello multitenant Apptjänst skalning är vanligtvis omedelbar eftersom en pool av resurser är tillgängliga toosupport den. Det finns ingen sådan buffert i en ASE och resurser vid behov.

Du kan skala upp too100 instanser i en ASE. De 100 instanserna kan finnas i en enda App Service-plan eller fördelade på flera programtjänstplaner.

## <a name="ip-addresses"></a>IP-adresser ##

Apptjänsten hello möjlighet tooallocate en dedicerad IP-adress tooan app. Den här funktionen är tillgänglig när du har konfigurerat IP-baserade SSL, enligt beskrivningen i [binda en befintlig anpassad SSL-certifikat tooAzure webbappar][ConfigureSSL]. Men i en ASE finns viktiga undantag. Du kan inte lägga till ytterligare IP adresser toobe som används för en IP-baserade SSL i en ASE ILB.

I ASEv1 behöver du tooallocate hello IP-adresser som resurser innan du kan använda dem. I ASEv2 använder du dem från din app precis som i hello multitenant Apptjänst. Det finns alltid en ledig adress i ASEv2 too30 IP-adresser. Varje gång läggs du använder någon annan så att en adress alltid är tillgängliga för användning. Fördröjning är krävs tooallocate en annan IP-adress, vilket förhindrar att lägga till IP-adresser i snabb följd.

## <a name="front-end-scaling"></a>Frontend skalning ##

I ASEv2, när du skalar upp din App Service-planer arbetare läggs automatiskt toosupport dem. Varje ASE skapas med två frontwebbservrarna. Dessutom skala hello frontwebbservrarna automatiskt ut med en hastighet av en klientdel för varje 15 instanser i App Service-planer. Till exempel om du har 15 instanser måste ha du tre frontwebbservrarna. Om du skalar too30 instanser, finns det fyra front slutar och så vidare.

Det här antalet frontwebbservrarna bör vara mer än tillräckligt för de flesta scenarier. Du kan dock skala ut i en snabbare takt. Du kan ändra hello förhållandet tooas lite som en klientdel för varje fem instanser. Det finns en avgift för att ändra hello förhållandet. Mer information finns i [priser för Azure App Service][Pricing].

Frontend resurser är hello HTTP/HTTPS-slutpunkt för hello ASE. Minnesanvändningen per klientdelen är konsekvent cirka 60 procent med hello frontend standardkonfigurationen. Kundens arbetsbelastningar körs inte på en klientdel. hello är avgörande för en klientdel med avseende tooscale hello CPU som styrs i första hand av HTTPS-trafik.

## <a name="app-access"></a>Åtkomst till appen ##

I en extern ASE skiljer hello-domän som används när du skapar appar sig från hello multitenant Apptjänst. Den omfattar hello namnet på hello ASE. Mer information om hur toocreate en extern ASE finns [skapa en Apptjänst-miljö][MakeExternalASE]. hello domännamn i en extern ASE ser ut som *.&lt; asename&gt;. p.azurewebsites.net*. Om din ASE heter exempelvis _externa ase_ och du är värd för en app som kallas _contoso_ i den ASE du når den på följande URL: er hello:

- Contoso.external ase.p.azurewebsites.net
- Contoso.SCM.external ase.p.azurewebsites.net

hello URL contoso.scm.external-ase.p.azurewebsites.net är används tooaccess hello Kudu-konsolen eller för att publicera en app med hjälp av web distribuera. Mer information om hello Kudu-konsolen finns [Kudu-konsol för Azure App Service][Kudu]. Hej Kudu-konsolen ger dig en webbgränssnittet för felsökning, överför filer, redigera filer och mycket mer.

I en ILB ASE bestämmer du hello domän vid tidpunkten för distribution. Mer information om hur toocreate en ILB ASE, se [skapa och använda en ILB ASE][MakeILBASE]. Om du anger domännamnet för hello _ilb ase.info_, hello appar i den ASE använda denna domän under skapande av app. För hello app med namnet _contoso_, hello URL: er är:

- Contoso.ilb ase.info
- Contoso.SCM.ilb ase.info

## <a name="publishing"></a>Publicering ##

Precis som med hello multitenant Apptjänst i en ASE kan du publicera med:

- Web-distribution.
- FTP.
- Kontinuerlig integrering.
- Dra och släpp i hello Kudu-konsolen.
- IDE-miljö, till exempel för Visual Studio, Eclipse eller IntelliJ IDEA.

Med en extern ASE hello dessa publiceringsalternativ alla fungerar samma. Mer information finns i [distribution i Azure App Service][AppDeploy]. 

hello största skillnaden med publicering är med avseende tooan ILB ASE. Med en ILB ASE hello publishing slutpunkter alla bara är tillgängliga via hello ILB. Hej ILB finns i en privat IP-adress i hello ASE undernät i hello virtuellt nätverk. Om du inte har network access toohello ILB kan du publicera alla appar på den ASE. Enligt beskrivningen i [skapa och använda en ILB ASE][MakeILBASE], behöver du tooconfigure DNS för hello appar i hello system. Som innehåller hello SCM slutpunkt. Om de inte är rätt definierad, kan du publicera. Din IDEs måste också toohave network access toohello ILB i ordning toopublish direkt tooit.

Internet-baserade CI system, t.ex GitHub och Visual Studio Team Services fungerar inte med en ILB ASE eftersom hello publishing slutpunkt inte är tillgänglig Internet. Du måste i stället toouse en CI-system som använder en pull-modell, till exempel Dropbox.

hello publishing slutpunkter för appar i en ASE ILB använda hello domän som hello ILB ASE har skapats med. Du kan se den i hello app publiceringsprofilen och hello app portalbladet (i **översikt** > **Essentials** och även i **egenskaper**). 

## <a name="pricing"></a>Prissättning ##

hello priser SKU kallas **isolerad** skapades för ASEv2. Alla programtjänstplaner som finns i ASEv2 finns i hello isolerad priser SKU. Isolerade priser för Apptjänst-planen kan variera per region. 

Dessutom toohello pris för din Apptjänst-planer finns det en platt för ASE sig själv. hello debiteras inte ändras med hello storlek på din ASE och betalar för hello ASE infrastruktur på en standard skalning av ytterligare 1 frontend för varje 15 App Service-plan instanser.  

Om hello standard skala antalet 1 klientdelen för varje 15 App Service-plan instanser inte är tillräckligt snabbt, kan du justera hello förhållandet som framför-servrar har lagts till eller hello storleken på hello framför-servrar.  När du justerar hello förhållande eller storlek, betalar för hello frontend kärnor inte skulle att lägga till som standard.  

Till exempel om du ändrar hello skala förhållandet too10 en klientdel har lagts till för varje 10 instanser i App Service-planer. hello fasta avgifter omfattar en skala andel en klientdel för varje 15 instanser. Med en skala förhållandet mellan 10 betala en avgift för hello tredje klientdelen som har lagts till för hello 10 App Service-plan instanser. Du behöver inte toopay för den när du når 15 instanser eftersom den har lagts till automatiskt.

Om du justeras hello storleken på hello framför ends too2 kärnor men inte ändra hello förhållandet sedan du betalar för hello extra kärnor.  En ASE skapas med 2 framför-upphör kommer det även under hello automatisk skalning tröskelvärde du ska betala för extra 2 kärnor om du har ökat hello storlek too2 core framför-servrar.

Mer information finns i [priser för Azure App Service][Pricing].

## <a name="delete-an-ase"></a>Ta bort en ASE ##

toodelete en ASE: 

1. Använd **ta bort** hello överst i hello **Apptjänstmiljö** bladet. 

2. Ange hello namnet på din ASE tooconfirm som du vill toodelete den. När du tar bort en ASE kan du ta bort alla hello innehållsfiler i den. 

    ![Ta bort ASE][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
