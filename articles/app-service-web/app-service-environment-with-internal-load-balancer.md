---
title: "aaaCreating och använda en intern belastningsutjämnare med en Apptjänst-miljö | Microsoft Docs"
description: "Skapa och använda en ASE med en ILB"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 20799f260993d6e81499408e5b547a2e21430174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Med hjälp av en intern belastningsutjämnare med en Apptjänst-miljö

> [!NOTE] 
> Den här artikeln handlar om hello Apptjänstmiljö v1. Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
>

hello App Service miljö (ASE)-funktionen är ett alternativ för Premium-tjänsten i Azure App Service som levererar en utökad konfiguration-funktion som inte är tillgänglig i hello flera innehavare stämplar. Hej ASE funktion i stort sett distribuerar hello Azure App Service i ditt virtuella Azure-Network(VNet). toogain en bättre förståelse av hello funktioner som erbjuds av Apptjänstmiljöer läsa hello [vad är en Apptjänstmiljö] [ WhatisASE] dokumentation. Om du inte vet hello fördelarna med att du arbetar i ett VNet läsa hello [Azure Virtual Network vanliga frågor och svar][virtualnetwork]. 

## <a name="overview"></a>Översikt
En ASE kan distribueras med en tillgänglig slutpunkt för internet eller en IP-adress på ditt VNet. I ordning tooset hello IP-adress tooa VNet-adress måste toodeploy din ASE med en intern Balancer(ILB) belastningen. När din ASE har konfigurerats med ett ILB anger du:

* en egen domän eller underdomän. toomake enkelt dokumentet förutsätter underdomän men du kan konfigurera det alls. 
* hello-certifikat som används för HTTPS
* DNS-hantering för din underdomänen. 

Du kan i RETUR göra saker som:

* värden intranätsprogram som affärsprogram på ett säkert sätt i hello molnet vilka åtkomst via en plats tooSite eller ExpressRoute VPN
* värden appar i hello moln som inte listas i offentliga DNS-servrar
* Skapa internet isolerade backend-appar som frontend-appar på ett säkert sätt kan integreras med

#### <a name="disabled-functionality"></a>Inaktiverade funktioner
Det finns vissa saker som du kan göra när du använder en ILB ASE. Detta omfattar:

* med hjälp av IPSSL
* tilldela IP-adresser toospecific appar
* köpa och använder ett certifikat med en app hello-portalen. Naturligtvis fortfarande kan du hämta certifikat direkt med en certifikatutfärdare och använda den med dina appar, inte via hello Azure-portalen.

## <a name="creating-an-ilb-ase"></a>Skapa en ILB ASE
Skapa en ILB ASE skiljer sig inte mycket från att skapa en ASE normalt. En ingående diskussion om hur du skapar en ASE läsa [hur tooCreate en Apptjänstmiljö][HowtoCreateASE]. hello processen toocreate är en ILB ASE hello samma mellan skapa ett virtuellt nätverk under skapande av ASE eller välja ett befintligt virtuellt nätverk. toocreate en ILB ASE: 

1. I hello Azure portal väljer **Ny -> webb + mobil -> Apptjänstmiljö**
2. Välj din prenumeration
3. Välj eller skapa en resursgrupp
4. Välj eller skapa ett virtuellt nätverk
5. Skapa ett undernät om du väljer ett virtuellt nätverk
6. Välj **virtuell nätverksplats konfiguration av virtuellt nätverk ->** och ange hello VIP typen tooInternal
7. Ange underdomänsnamn på (det vara hello underdomäner som används för appar som skapats i den här ASE)
8. Klicka på Ok och skapa sedan

![][1]

Inom hello virtuellt nätverksblad finns ett alternativ för konfiguration av virtuellt nätverk. Detta kan du välja mellan en extern VIP eller interna VIP. hello standardvärdet är extern. Om du har satts tooExternal använder din ASE en tillgänglig VIP för internet. Om du väljer internt konfigureras din ASE med en ILB på en IP-adress inom ditt VNet. 

När du har valt internt hello möjlighet tooadd flera IP-adresser tooyour ASE tas bort och i stället måste tooprovide hello underdomän hello ASE. I en ASE med en extern VIP hello används namnet på hello ASE i hello underdomän för appar som har skapats i den ASE. Om din ASE anropades ***contosotest*** och din app i den ASE anropades ***MinTest*** hello underdomän skulle vara hello format ***contosotest.p.azurewebsites.net*** och hello URL: en för appen skulle vara ***mytest.contosotest.p.azurewebsites.net***. Om du ställer in hello VIP typen tooInternal används ASE-namnet inte i hello underdomänen för hello ASE. Du ange uttryckligen hello underdomänen. Om din underdomän ***contoso.corp.net*** och du har gjort en app i den ASE med namnet ***timereporting*** sedan hello URL appen skulle bli ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Appar i en ILB ASE
Skapa en app i en ASE ILB är hello samma som att skapa en app i en ASE normalt. 

1. I hello Azure portal väljer **Ny -> webb + mobil -> webb** eller **Mobile** eller **API-App**
2. Ange namnet på appen
3. Välj en prenumeration
4. Välj eller skapa resursgrupp
5. Välj eller skapa App Service Plan(ASP). Om att skapa en ny ASP sedan väljer din ASE som hello plats och väljer hello arbetspool du din ASP toobe som skapats i. När du skapar hello ASP väljer du din ASE som hello plats och hello arbetspool. När du anger hello namnet på hello-app som du ser den hello underdomänen under har appnamnet ersatts av hello underdomän för din ASE. 
6. Välj Skapa. Du bör välja hello **PIN-kod toodashboard** kryssrutan om du vill att hello app tooshow på instrumentpanelen. 

![][2]

Under hello appen hämtar namn hello underdomän uppdaterade tooreflect hello underdomän till din ASE. 

## <a name="post-ilb-ase-creation-validation"></a>Bokför ILB ASE skapa validering
En ILB ASE är något annorlunda än hello icke - ILB ASE. Anges som redan behöver du toomanage egna DNS och du har också tooprovide ditt eget certifikat för HTTPS-anslutningar. 

När du har skapat din ASE ser du att hello underdomän visar hello underdomän du angav och det finns ett nytt objekt i hello **inställningen** menyn kallas **ILB certifikat**. Hej ASE skapas med ett självsignerat certifikat som gör det enklare tootest HTTPS. hello portalen får du veta att du behöver tooprovide egna certifikat för HTTPS, men detta är toodrive toohave ett certifikat som går med din underdomänen. 

![][3]

Om du försöker bara saker och inte vet hur toocreate ett certifikat, du kan använda hello IIS MMC konsolen programmet toocreate en self signerade certifikat. När den har skapats kan du exportera det som en .pfx-fil och överför den i hello ILB certifikat Användargränssnittet. När du åtkomst till en plats som skyddas med ett självsignerat certifikat, din webbläsare får en varning om att hello platsen du ansluter till inte är säkra på grund av toohello oförmåga toovalidate hello certifikat. Om du vill tooavoid den varning som du behöver ett felaktigt signerade certifikat som matchar din underdomän och har en förtroendekedja för som identifieras i din webbläsare.

![][6]

Om du vill tootry hello flöda med dina egna certifikat och testa både HTTP och HTTPS åtkomst tooyour ASE:

1. Öppnar tooASE Användargränssnittet när ASE skapas **ASE -> Inställningar -> ILB-certifikat**
2. Ange ILB certifikat genom att välja certifikatets pfx-fil och ange lösenord. Det här steget tar en liten när tooprocess och hello-meddelande som en skalning pågår kommer att visas.
3. Hämta hello ILB-adressen för din ASE (**ASE -> Egenskaper för virtuell IP-adress ->**)
4. Skapa en webbapp i ASE när den har skapats 
5. Skapa en virtuell dator om du inte har något i det virtuella nätverket (hello inte i samma undernät som hello ASE eller saker break)
6. Ange DNS för din underdomänen. Du kan använda jokertecken med din underdomän i din DNS eller om du vill toodo några enkla tester redigera hello hosts-filen på din Virtuella tooset web app name tooVIP IP-adress. Om din ASE hade hello underdomännamn. ilbase.com och du har gjort hello web app mytestapp så att skulle åtgärdas på mytestapp.ilbase.com därefter uppsättning som i hosts-filen. (På Windows hello hosts-filen är på C:\Windows\System32\drivers\etc\)
7. Använda en webbläsare på den virtuella datorn och toohttp://mytestapp.ilbase.com (eller vad din webbprogrammets namn är med ditt underdomän)
8. Använda en webbläsare på den virtuella datorn och toohttps://mytestapp.ilbase.com behöver tooaccept hello bristande säkerhet om du använder ett självsignerat certifikat. 

hello IP-adressen för din ILB anges i dina egenskaper som hello virtuell IP-adress

![][4]

## <a name="using-an-ilb-ase"></a>Med hjälp av en ILB ASE
#### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
En ILB ASE aktiverar isolering av nätverk för dina appar som hello appar inte är tillgänglig eller är även känt av hello internet. Detta är utmärkt för intranätplatser, till exempel affärsprogram. När du behöver toorestrict åtkomst även kan mer du fortfarande använda Network Security Groups(NSGs) toocontrol åtkomst på hello nätverksnivån. 

Om du vill begränsa åtkomsten för toouse NSG: er toofurther måste du toomake att du inte dela hello kommunikation måste den hello ASE i ordning toooperate. Även om hello HTTP/HTTPS-åtkomst endast via hello används ILB av hello ASE hello ASE fortfarande är beroende av resursen utanför hello virtuella nätverk. toosee vilka nätverksåtkomst är fortfarande krävs titt på hello informationen i hello dokument på [styra inkommande trafik tooan Apptjänstmiljö] [ ControlInbound] och hello dokumentet [nätverk Konfigurationsinformation för Apptjänst-miljöer med ExpressRoute][ExpressRoute]. 

tooconfigure dina NSG: er som du behöver tooknow hello IP-adress som ska användas av Azure toomanage din ASE. IP-adressen är också hello utgående IP-adress från din ASE om den gör det internet-begäranden. hello utgående IP-adressen för din ASE förblir statisk för din ASE hello livslängd. Om du tar bort och återskapa din ASE får du en ny IP-adress. toofind IP-adressen för Gå**Inställningar -> Egenskaper** och hitta hello **utgående IP-adress**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Hantering av allmänna ILB ASE
Hantera en ILB ASE är i stort sett hello samma som att hantera en ASE normalt. Du behöver tooscale upp din worker pooler toohost mer ASP instanser och skala upp din klientdelen servrar toohandle ökade mängder HTTP/HTTPS-trafik. Allmän information om hur du hanterar hello konfigurationen av en ASE läsa hello dokumentet på [konfigurerar en Apptjänstmiljö][ASEConfig]. 

hello ytterligare hantering objekt är certifikathantering och DNS-hantering. Du behöver tooobtain och överför hello-certifikat som används för HTTPS efter ILB ASE har skapats och Ersätt den innan den upphör. Eftersom Azure äger hello basdomän kan vi ge certifikat för ASEs med en extern VIP. Eftersom hello underdomäner som används av en ILB ASE kan vara något, behöver du tooprovide egna certifikat för HTTPS. 

#### <a name="dns-configuration"></a>DNS-konfiguration
När du använder en extern VIP hello DNS hanteras av Azure. Alla appar som skapats i en ASE läggs till automatiskt tooAzure DNS som är en offentlig DNS. I en ASE ILB har du toomanage egna DNS. För en given underdomän, till exempel contoso.corp.net måste toocreate DNS A registrerar den punkt tooyour ILB-adressen för:

    * 
    publicera *.SCM ftp 


## <a name="getting-started"></a>Komma igång
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

tooget igång med Apptjänstmiljöer, se [introduktion tooApp miljöer][WhatisASE]

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
