---
title: "aaaCreate en extern Azure App Service-miljö"
description: "Förklarar hur toocreate en Apptjänst-miljö när du skapar en app eller en fristående"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: f8619534ddd889ea65063733ac6ec11b206e799c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-external-app-service-environment"></a>Skapa en extern Apptjänst-miljö #

Azure Apptjänst-miljön är en distribution av Azure App Service till ett undernät i Azure-nätverk (VNet). Det finns två sätt toodeploy Apptjänst-miljö (ASE):

- Med en VIP på en extern IP-adress, som kallas ofta för en extern ASE.
- Med hello VIP på en intern IP-adress, kallas ofta för en ILB ASE eftersom hello intern slutpunkt är en intern belastningsutjämnare (ILB).

Den här artikeln beskrivs hur du toocreate en extern ASE. En översikt över hello ASE finns [en introduktion toohello Apptjänstmiljö][Intro]. Mer information om hur toocreate en ILB ASE, se [skapa och använda en ILB ASE][MakeILBASE].

## <a name="before-you-create-your-ase"></a>Innan du skapar din ASE ##

När du skapar din ASE, kan du inte ändra hello följande:

- Plats
- Prenumeration
- Resursgrupp
- Virtuella nätverk som används
- Undernät som används
- Undernätets storlek

> [!NOTE]
> När du väljer ett virtuellt nätverk och ange ett undernät, se till att det är tillräckligt stor tooaccommodate framtida tillväxt. Vi rekommenderar en storlek på `/25` med 128-adresser.
>

## <a name="three-ways-toocreate-an-ase"></a>Tre sätt toocreate en ASE ##

Det finns tre sätt toocreate en ASE:

- **När du skapar en apptjänstplan**. Den här metoden skapar hello ASE och hello App Service-plan i ett enda steg.
- **Som en fristående åtgärd**. Den här metoden skapar en fristående ASE, vilket är en ASE utan något i den. Den här metoden är en mer avancerad processen toocreate en ASE. Du använder den toocreate en ASE med en ILB.
- **En Azure Resource Manager-mall**. Den här metoden är avsedd för avancerade användare. Mer information finns i [skapa en ASE från en mall][MakeASEfromTemplate].

En extern ASE har en offentlig VIP, vilket innebär att alla HTTP/HTTPS-trafik toohello appar i hello ASE träffar en internet-tillgänglig IP-adress. En ASE med en ILB har en IP-adress från hello undernät som används av hello ASE. hello appar i en ASE ILB inte visas direkt toohello internet.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Skapa en ASE och en apptjänstplan tillsammans ##

Hej apptjänstplan är en behållare för appar. När du skapar en app i App Service, Välj eller skapa en apptjänstplan. hello behållaren modellen miljöer håller App Service-planer och programtjänstplaner håller appar.

toocreate en ASE när du skapar en apptjänstplan:

1. I hello [Azure-portalen](https://portal.azure.com/)väljer **ny** > **webb + mobilt** > **Web App**.

    ![Skapa en webbapp][1]

2. Välj din prenumeration. hello app och hello ASE skapas i hello samma prenumerationer.

3. Välj eller skapa en Resursgrupp. Med resursgrupper kan du hantera relaterade Azure-resurser som en enhet. Resursgrupper är också användbara när du har skapat regler för rollbaserad åtkomstkontroll för dina appar. Mer information finns i hello [översikt över Azure Resource Manager][ARMOverview].

4. Välj hello apptjänstplan och välj sedan **Skapa nytt**.

    ![Ny programtjänstplan][2]

5. I hello **plats** listrutan, Välj hello region där du vill att toocreate hello ASE. Om du väljer en befintlig ASE är inte en ny ASE skapas. Hej programtjänstplanen har skapats i hello ASE som du har valt. 

6. Välj **prisnivå**, och välj ett av hello **isolerad** priser SKU: er. Om du väljer en **isolerad** SKU-kort och en plats som inte är en ASE, skapas en ny ASE på denna plats. Välj toostart hello processen toocreate en ASE **Välj**. Hej **isolerad** SKU är endast tillgänglig i tillsammans med en ASE. Du också kan inte använda andra prisnivå SKU i en ASE än **isolerad**.

    ![Val av prisnivå][3]

7. Ange hello namn för din ASE. Det här namnet används i hello adresserbara namn för dina appar. Om hello hello ASE heter _appsvcenvdemo_, hello domännamnet är *. appsvcenvdemo.p.azurewebsites.net*. Om du skapar en app med namnet *mytestapp*, är det adresserbara på mytestapp.appsvcenvdemo.p.azurewebsites.net. Du kan inte använda blanksteg i hello namn. Om du använder versaler är hello domännamnet hello totala gemen version av det här namnet.

    ![Nya programtjänstplanens namn][4]

8. Ange information om din Azure virtuella nätverk. Välj antingen **skapa nya** eller **Välj befintlig**. hello alternativet tooselect ett existerande VNet är bara tillgängligt om du har ett VNet i hello valda regionen. Om du väljer **Skapa nytt**, ange ett namn för hello virtuella nätverk. Ett nytt Resource Manager-VNet med det namnet skapas. Den använder hello adressutrymme `192.168.250.0/23` i hello valda regionen. Om du väljer **Välj befintligt**, måste du:

    a. Välj hello VNet-Adressblock, om du har fler än en.

    b. Ange ett nytt undernätsnamn.

    c. Välj hello storlek för hello undernät. *Kom ihåg tooselect en storlek tillräckligt stor tooaccommodate tillväxt i din ASE.* Vi rekommenderar `/25`, som har 128-adresser och kan hantera en ASE maximala storlek. Vi rekommenderar inte `/28`, till exempel eftersom endast 16 adresser är tillgängliga. Infrastruktur använder minst fem adresser. I en `/28` undernät, som du har lämnat med en maximal skala 11 instanser.

    d. Välj IP-intervall för hello undernät.

9. Välj **skapa** toocreate hello ASE. Den här processen skapar även hello App Service-plan och hello app. Hej ASE, App Service-plan och app är allt under hello samma prenumeration och även i hello samma resursgrupp. Om din ASE måste en separat resursgrupp eller om du behöver en ILB ASE gör hello steg toocreate en ASE ensamt.

## <a name="create-an-ase-by-itself"></a>Skapa en ASE ensamt ##

Om du skapar en ASE fristående har ingenting i den. En tom ASE ådrar fortfarande månadskostnaden för hello-infrastruktur. Följ dessa steg toocreate en ASE med en ILB eller toocreate en ASE i sin egen resursgruppen. När du har skapat din ASE kan du skapa appar i den med hjälp av hello normala processer. Välj din nya ASE som hello plats.

1. Sök hello Azure Marketplace för **Apptjänstmiljö**, eller välj **ny** > **Web Mobile** > **Apptjänst Miljö**. 

2. Ange hello namnet på din ASE. Det här namnet används för hello appar som har skapats i hello ASE. Om hello namn är *mynewdemoase*, hello underdomännamn är *. mynewdemoase.p.azurewebsites.net*. Om du skapar en app med namnet *mytestapp*, är det adresserbara på mytestapp.mynewdemoase.p.azurewebsites.net. Du kan inte använda blanksteg i hello namn. Om du använder versaler är hello domännamnet hello totala gemener versionen av hello namn. Om du använder en ILB ASE-namnet används inte i din underdomänen men i stället anges uttryckligen när ASE skapades.

    ![Namngivning av ASE][5]

3. Välj din prenumeration. Den här prenumerationen är också hello att alla program i hello ASE att använda. Du kan inte försätta en ASE i ett virtuellt nätverk som tillhör en annan prenumeration.

4. Välj eller ange en ny resursgrupp. hello resursgruppens namn används för din ASE måste vara hello samma konto som används för din VNet. Om du väljer ett existerande VNet hello val av resursgrupp för ditt ASE är uppdaterade tooreflect som ditt VNet. *Du kan skapa en ASE med en resursgrupp som skiljer sig från hello VNet resursgrupp om du använder en Resource Manager-mall.* toocreate en ASE från en mall finns [skapa en Apptjänst-miljö från en mall][MakeASEfromTemplate].

    ![Val av resursgrupp][6]

5. Välj din VNet och plats. Du kan skapa ett nytt virtuellt nätverk eller välj ett befintligt virtuellt nätverk: 

    * Om du väljer ett nytt virtuellt nätverk kan du ange ett namn och plats. hello har nya VNet hello adressintervallet 192.168.250.0/23 och ett undernät med namnet standard. hello undernät definieras som 192.168.250.0/24. Du kan bara välja en Resource Manager-VNet. Hej **VIP typen** val avgör om din ASE kan nås direkt från hello internet (externa) eller om den använder en ILB. toolearn mer om dessa alternativ finns [skapa och använda en intern belastningsutjämnare med en Apptjänst-miljö][MakeILBASE]. 

      * Om du väljer **externa** för hello **VIP typen**, kan du välja hur många externa IP-adresser hello systemet skapas med för IP-baserade SSL. 
    
      * Om du väljer **internt** för hello **VIP typen**, måste du ange hello-domän som används av din ASE. Du kan distribuera en ASE till ett virtuellt nätverk som använder offentliga eller privata adressintervall. toouse ett VNet med ett offentligt adressintervall måste toocreate hello VNet i förväg. 
    
    * Om du väljer ett befintligt virtuellt nätverk skapas ett nytt undernät när hello ASE skapas. *Du kan inte använda ett befintligt undernät i hello-portalen. Du kan skapa en ASE med ett befintligt undernät om du använder en Resource Manager-mall.* toocreate en ASE från en mall finns [skapa en Apptjänst-miljö från en mall][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##

Du kan ändå skapa instanser av hello första versionen av Apptjänst-miljö (ASEv1). toostart som bearbetar Sök hello Marketplace för **Apptjänstmiljö v1**. Du skapar hello ASE i hello samma sätt som du skapar hello fristående ASE. När den är klar har två frontwebbservrarna och två personer i din ASEv1. Du måste hantera hello frontwebbservrarna och personer med ASEv1. De läggs inte automatiskt när du skapar din App Service-planer. Hej frontwebbservrarna fungera som hello HTTP/HTTPS-slutpunkter och skicka trafik toohello arbetare. hello arbetare är hello roller som värd för dina appar. Du kan justera hello antalet frontwebbservrarna och personer när du har skapat din ASE. 

toolearn mer om ASEv1, se [introduktion toohello Apptjänstmiljö v1][ASEv1Intro]. Mer information om skalning, hantera och övervaka ASEv1, finns i [hur tooconfigure en Apptjänstmiljö][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png



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
