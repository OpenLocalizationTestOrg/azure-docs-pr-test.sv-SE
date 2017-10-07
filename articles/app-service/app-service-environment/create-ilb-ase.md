---
title: "aaaCreate och använda en intern belastningsutjämnare med Azure Apptjänst-miljön"
description: "Information om hur toocreate och använda en isolerad internet Azure Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: d019ca6f231c3acfdab4cd380db375a076802f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Skapa och använda en intern belastningsutjämnare med en Apptjänst-miljö #

 Azure Apptjänst-miljön är en distribution av Azure App Service till ett undernät i Azure-nätverk (VNet). Det finns två sätt toodeploy Apptjänst-miljö (ASE): 

- Med en VIP på en extern IP-adress, som kallas ofta för en extern ASE.
- Med en VIP på en intern IP-adress kallas ofta för en ILB ASE eftersom hello intern slutpunkt är en intern belastningsutjämnare (ILB). 

Den här artikeln beskrivs hur du toocreate en ILB ASE. En översikt över hello ASE finns [introduktion tooApp miljöer][Intro]. hur toocreate en extern ASE Se toolearn [skapa en extern ASE][MakeExternalASE].

## <a name="overview"></a>Översikt ##

Du kan distribuera en ASE med en internet-tillgänglig slutpunkt eller en IP-adress på ditt VNet. tooset hello IP-adress tooa VNet-adress, hello ASE måste distribueras med en ILB. När du distribuerar din ASE med en ILB, måste du ange:

-   En egen domän som du använder när du skapar dina appar.
-   hello-certifikat som används för HTTPS.
-   DNS-hantering för din domän.

Du kan i RETUR göra saker som:

-   Värden intranätsprogram på ett säkert sätt i hello moln som du kommer åt via en plats-till-plats eller Azure ExpressRoute VPN.
-   Värden appar i hello moln som inte listas i offentliga DNS-servrar.
-   Skapa isolerad internet backend-appar som din frontend appar på ett säkert sätt kan integreras med.

### <a name="disabled-functionality"></a>Inaktiverade funktioner ###

Det finns vissa saker som du kan göra när du använder en ILB ASE:

-   Använd IP-baserade SSL.
-   Tilldela IP-adresser toospecific appar.
-   Köpa och använda ett certifikat med en app via hello Azure-portalen. Du kan skaffa certifikat från en certifikatutfärdare och använda dem med dina appar. Du kan inte hämta dem via hello Azure-portalen.

## <a name="create-an-ilb-ase"></a>Skapa en ILB ASE ##

toocreate en ILB ASE:

1. Välj i hello Azure-portalen, **ny** > **webb + mobilt** > **Apptjänstmiljö**.

2. Välj din prenumeration.

3. Välj eller skapa en Resursgrupp.

4. Välj eller skapa ett VNet.

5. Om du väljer ett existerande VNet, måste toocreate ett undernät toohold hello ASE. Kontrollera att tooset ett undernät storlek tillräckligt stor tooaccommodate eventuell tillväxt i din ASE. Vi rekommenderar en storlek på `/25`, som har 128-adresser och kan hantera en ASE maximala storlek. hello minsta storlek som du kan välja är en `/28`. När infrastrukturen måste vara storleken skalade tooa Maximalt 11 instanser.

    * Utöver hello standard högst 100 instanser i App Service-planer.

    * Skala nära 100 men med snabbare frontend skalning.

6. Välj **virtuell nätverksplats** > **konfiguration av virtuellt nätverk**. Ange hello **VIP typen** för**internt**.

7. Ange ett domännamn. Den här domänen är hello som används för appar som har skapats i den här ASE. Det finns vissa begränsningar. Det går inte att vara:

    * NET   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename&gt;. p.azurewebsites.net

   hello domännamn för appar och hello domännamn som används av din ASE får inte överlappa varandra. För en ILB ASE med hello domännamn _contoso.com_, du kan inte använda anpassade domännamn för dina appar som:

    * www.contoso.com

    * ABCD.def.contoso.com

    * ABCD.contoso.com

   Om du vet hello anpassade domännamn för dina appar, välja en domän för hello ILB ASE som inte står i konflikt med de anpassade domännamn. I det här exemplet kan du använda något som liknar *contoso internal.com* på din ASE hello domän eftersom som inte står i konflikt med anpassade domännamn som slutar i *. contoso.com*.

8. Välj **OK**, och välj sedan **skapa**.

    ![ASE-generering][1]

På hello **virtuellt nätverk** bladet finns en **virtuella nätverkskonfigurationen** alternativet. Du kan använda den tooselect en extern VIP eller ett internt VIP. hello standardvärdet är **externa**. Om du väljer **externa**, din ASE använder en internet-tillgängliga VIP. Om du väljer **internt**, din ASE har konfigurerats med en ILB på en IP-adress inom ditt VNet.

När du har valt **internt**, hello möjlighet tooadd flera IP-adresser tooyour ASE tas bort. Du måste i stället tooprovide hello domän hello ASE. I en ASE med en extern VIP används hello namnet på hello ASE i hello domän för appar som har skapats i den ASE.

Om du ställer in **VIP typen** för**internt**, namnet på din ASE används inte i hello domän för hello ASE. Du ange uttryckligen hello domän. Om din domän är *contoso.corp.net* och du skapar en app i den ASE med namnet *timereporting*, hello URL för appen är timereporting.contoso.corp.net.


## <a name="create-an-app-in-an-ilb-ase"></a>Skapa en app i en ILB ASE ##

Du skapar en app i en ASE ILB i hello samma sätt som du skapar en app i en ASE normalt.

1. Välj i hello Azure-portalen, **ny** > **webb + mobilt** > **Web** eller **Mobile** eller  **API-App**.

2. Ange namn på hello hello-appen.

3. Välj hello prenumeration.

4. Välj eller skapa en Resursgrupp.

5. Välj eller skapa en apptjänstplan. Om du vill toocreate en ny programtjänstplan, Välj din ASE som hello plats. Välj hello arbetspool där du vill att din App Service-plan toobe skapas. När du skapar hello App Service-plan, Välj din ASE som hello plats och hello arbetspool. När du anger hello namnet på hello app har hello domän under appnamnet ersatts av hello domän för ditt ASE.

6. Välj **Skapa**. Om du vill hello app tooappear på instrumentpanelen, väljer den **PIN-kod toodashboard** kryssrutan.

    ![Skapa en App Service-plan][2]

    Under **appnamn**, hello domännamn är uppdaterade tooreflect hello domän för ditt ASE.

## <a name="post-ilb-ase-creation-validation"></a>Post ILB ASE skapa validering ##

En ILB ASE är något annorlunda än hello icke - ILB ASE. Som redan anges måste toomanage egna DNS. Du har också tooprovide ditt eget certifikat för HTTPS-anslutningar.

När du har skapat din ASE visar hello domännamn hello domän du angav. Ett nytt objekt visas i den **inställningen** menyn kallas **ILB certifikat**. Hej ASE skapas med ett certifikat som inte anger hello ILB ASE domän. Om du använder hello ASE med certifikatet om webbläsaren det är ogiltigt. Det här certifikatet gör det enklare tootest HTTPS, men du måste tooupload egna certifikat som är bundet tooyour ILB ASE domän. Det här steget är nödvändigt oavsett om certifikatet är självsignerat eller förvärvas från en certifikatutfärdare.

![ILB ASE domännamn][3]

ILB-ASE behöver ett giltigt SSL-certifikat. Använda interna certifikatutfärdare, köpa ett certifikat från en extern utfärdare eller använda ett självsignerat certifikat. Oavsett hello källa för hello SSL-certifikat måste hello följande certifikat-attribut toobe konfigurerats korrekt:

* **Ämne**: det här attributet måste anges too*.your-rot-domain-här.
* **Alternativt ämnesnamn**: det här attributet måste innehålla både **.your-rot-domain-här* och **.scm.your-rot-domain-här*. SSL-anslutningar toohello SCM/Kudu-plats som är associerade med varje app använder en adress i formatet hello *your-app-name.scm.your-root-domain-here*.

Konvertera/spara hello SSL-certifikatet som en .pfx-fil. hello .pfx-filen måste innehålla alla mellanliggande och root certifikat. Skydda den med ett lösenord.

Om du vill toocreate ett självsignerat certifikat, kan du använda hello PowerShell-kommandon här. Vara säker på att toouse ILB ASE domännamnet i stället för *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

hello-certifikat som genereras av dessa PowerShell-kommandon flaggas av webbläsare eftersom hello certifikatet inte har skapats av en certifikatutfärdare i din webbläsare förtroendekedja för. tooget ett certifikat som har förtroende för din webbläsare, skaffa den från en kommersiell certifikatutfärdare i din webbläsare förtroendekedja för. 

![Ställ in ILB certificate][4]

tooupload egna certifikat och testa åtkomst:

1. När du har skapat hello ASE gå toohello ASE UI. Välj **ASE** > **inställningar** > **ILB certifikat**.

2. tooset hello ILB certifikat, Välj hello certifikatets PFX-fil och ange hello lösenord. Det här steget tar några tid tooprocess. Det visas ett meddelande om att en överföringen pågår.

3. Hämta hello ILB-adressen för din ASE. Välj **ASE** > **egenskaper** > **virtuella IP-adressen**.

4. Skapa en webbapp i din ASE efter hello ASE skapas.

5. Skapa en virtuell dator om du inte har något i det virtuella nätverket.

    > [!NOTE] 
    > Försök inte toocreate den här virtuella datorn i hello samma undernät som hello ASE eftersom den kommer att misslyckas eller orsaka problem.
    >
    >

6. Ange hello DNS för din ASE-domän. Du kan använda jokertecken med din domän i din DNS. toodo några enkla tester, redigera hello hosts-filen på din Virtuella tooset hello web app name toohello VIP IP-adress:

    a. Om din ASE har hello domännamn _. ilbase.com_ och du skapar hello webbprogram med namnet _mytestapp_, riktar på _mytestapp.ilbase.com_. Sedan ställer du in _mytestapp.ilbase.com_ tooresolve toohello ILB-adress. (För Windows hello hosts-filen finns på _C:\Windows\System32\drivers\etc\_.)

    b. tootest web distribution publicering eller åtkomst toohello, avancerad konsol, skapa en post för _mytestapp.scm.ilbase.com_.

7. Använda en webbläsare på den virtuella datorn och gå till http://mytestapp.ilbase.com. (Eller toowhatever din webbprogrammets namn är med domänen.)

8. Använda en webbläsare på den virtuella datorn och gå till https://mytestapp.ilbase.com. Om du använder ett självsignerat certifikat kan acceptera hello bristande säkerhet.

    hello IP-adressen för din ILB anges under **IP-adresser**. Den här listan har också hello IP-adresser som används av hello externa VIP och för av inkommande hanteringstrafik.

    ![ILB IP-adress][5]

### <a name="web-jobs-functions-and-hello-ilb-ase"></a>Web jobb, funktioner och hello ILB ASE

Web jobb och funktioner stöds i en ASE ILB men hello portal toowork med dem, måste du ha åtkomst toohello SCM nätverksplats.  Detta innebär att din webbläsare måste antingen vara på en värd som är antingen i eller ansluten toohello virtuellt nätverk.  

När du använder Azure Functions på en ILB ASE kan få du ett felmeddelande som säger ”vi inte kan tooretrieve dina funktioner just nu. Försök igen senare ”. Det här felet uppstår eftersom hello funktioner UI utnyttjar hello SCM plats över HTTPS och hello rotcertifikat är inte i hello webbläsare förtroendekedja för. Web jobb har liknande problem. tooavoid det här problemet kan du göra något av följande hello:

- Lägg till hello certifikatarkivet tooyour betrott certifikat. Då hämtas kant och Internet Explorer.
- Använd Chrome och webbplatsen toohello SCM först, godkänna hello obetrodda certifikat och sedan toohello portal.
- Använda ett kommersiella certifikat som finns i din webbläsare förtroendekedja för.  Är det bästa alternativet för hello.  

## <a name="dns-configuration"></a>DNS-konfiguration ##

När du använder en extern VIP hanteras hello DNS av Azure. Alla appar som skapats i en ASE läggs automatiskt tooAzure DNS, vilket är en offentlig DNS. I en ASE ILB, måste du hantera din egen DNS. För en viss domän som _contoso.net_, behöver du toocreate DNS A-poster i din DNS punkt tooyour ILB adressen för:

- *. contoso.net
- *. scm.contoso.net

Om din ILB ASE domän används för flera saker utanför den här ASE behöva toomanage DNS på grundval av per programnamn. Den här metoden en utmaning eftersom du behöver tooadd namnen på alla nya app i din DNS när du skapar den. Därför rekommenderar vi att du använder en särskild domän.

## <a name="publish-with-an-ilb-ase"></a>Publicera med en ILB ASE ##

Det finns två slutpunkter för varje app som har skapats. Du har på en ILB ASE  *&lt;appnamn >.&lt; ILB ASE domän >* och  *&lt;appnamn > .scm.&lt; ILB ASE domän >*. 

hello SCM platsnamn tar toohello Kudu-konsolen som kallas hello **avancerade portal**, inom hello Azure-portalen. Hej Kudu-konsolen kan du visa miljövariabler, utforska hello disk, använda en konsol och mycket mer. Mer information finns i [Kudu-konsol för Azure App Service][Kudu]. 

Hej multitenant Apptjänst och en extern ASE är det enkel inloggning mellan hello Azure-portalen och hello Kudu-konsolen. För hello ILB ASE behöver du dock toouse din publishing autentiseringsuppgifter toosign in hello Kudu-konsolen.

Internet-baserade CI system, t.ex GitHub och Visual Studio Team Services fungerar inte med en ILB ASE eftersom hello publishing slutpunkten inte komma åt internet. Du måste i stället toouse en CI-system som använder en pull-modell, till exempel Dropbox.

hello publishing slutpunkter för appar i en ASE ILB använda hello domän som hello ILB ASE har skapats med. Den här domänen visas hello app publiceringsprofilen och i hello app portalbladet (**översikt** > **Essentials** och även **egenskaper**). Om du har en ILB ASE med hello underdomän *contoso.net* och en app med namnet *MinTest*, använda *mytest.contoso.net* för FTP och *mytest.scm.contoso.net*  för web-distribution.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Koppla en ILB ASE med en Brandvägg-enhet ##

Azure Apptjänst innehåller många säkerhetsåtgärder som skyddar hello system. De kan också hjälpa toodetermine om en app har över. hello bästa skydd för ett webbprogram är toocouple en Värdplattformen, till exempel Azure App Service med en brandvägg för webbaserade program (Brandvägg). Eftersom hello ILB ASE har ett isolerat nätverk programslutpunkten, är den lämplig för sådan användning.

Mer om hur tooconfigure din ILB ASE med en Brandvägg enhet, se toolearn [konfigurera brandväggar för webbaserade program med App Service-miljö][ASEWAF]. Den här artikeln visar hur toouse Barracuda virtuell utrustning med din ASE. Ett annat alternativ är toouse Azure Application Gateway. Programgateway använder hello OWASP core regler toosecure alla program som placeras bakom det. Mer information om Application Gateway finns [introduktion toohello Azure web application firewall][AppGW].

## <a name="get-started"></a>Kom igång ##

Alla artiklar och hur tooinstructions för ASEs är tillgängliga i den [viktigt för apptjänstmiljöer][ASEReadme].

* tooget igång med ASEs, se [introduktion tooApp miljöer][Intro].
* Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

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
