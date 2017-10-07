---
title: "aaaCreate en Azure Apptjänst-miljö med hjälp av en Resource Manager-mall"
description: "Förklarar hur toocreate en extern eller ILB Azure App Service-miljö med hjälp av en Resource Manager-mall"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: c8aeedee675a6e931169b725ee916cc7fa8f762f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Skapa en ASE med hjälp av en Azure Resource Manager-mall

## <a name="overview"></a>Översikt
Azure Apptjänst-miljöer (ASEs) kan skapas med en internet-tillgänglig slutpunkt eller en slutpunkt på en intern adress i Azure-nätverk (VNet). När de skapas med en intern slutpunkt tillhandahålls denna slutpunkt av en Azure komponent som kallas en intern belastningsutjämnare (ILB). Hej ASE på en intern IP-adress kallas en ILB ASE. Hej ASE med en offentlig slutpunkt kallas för en extern ASE. 

En ASE kan skapas med hjälp av hello Azure-portalen eller en Azure Resource Manager-mall. Den här artikeln beskriver hur hello steg och syntax som du behöver en extern ASE eller ILB ASE toocreate med Resource Manager-mallar. toolearn hur toocreate en ASE i hello Azure-portalen, se [göra en extern ASE] [ MakeExternalASE] eller [gör en ILB ASE][MakeILBASE].

När du skapar en ASE i hello Azure-portalen kan du skapa ditt VNet på hello samma tid eller välj en befintlig VNet toodeploy till. När du skapar en ASE från en mall måste du starta med: 

* Hanteraren för virtuella nätverk.
* Ett undernät i det virtuella nätverket. Vi rekommenderar en ASE undernät storlek på `/25` med 128 adresser tooaccomodate framtida tillväxt. När hello ASE har skapats kan ändra du inte hello storlek.
* hello resurs-ID från ditt VNet. Du kan hämta den här informationen från hello Azure-portalen under Egenskaper för virtuellt nätverk.
* hello-prenumeration som du vill toodeploy till.
* hello-plats som du vill toodeploy till.

tooautomate genereringen av din ASE:

1. Skapa hello ASE från en mall. Om du skapar en extern ASE är du klar efter det här steget. Om du skapar en ILB ASE, finns det några fler saker toodo.

2. När du har skapat din ILB ASE upp ett SSL-certifikat som matchar din ILB ASE-domän.

3. hello har överförda SSL-certifikatet tilldelats toohello ILB ASE som dess ”default” SSL-certifikat.  Det här certifikatet används för SSL-trafik tooapps på hello ILB ASE när de använder hello vanliga rotdomänen som är tilldelade toohello ASE (till exempel https://someapp.mycustomrootcomain.com).


## <a name="create-hello-ase"></a>Skapa hello ASE
En Resource Manager-mall som skapar en ASE och dess associerade parametrar-fil är tillgänglig [i ett exempel] [ quickstartasev2create] på GitHub.

Om du vill toomake en ILB ASE kan använda dessa Resource Manager-mall [exempel][quickstartilbasecreate]. De tillgodose toothat användningsfall. De flesta hello parametrar i hello *azuredeploy.parameters.json* filen är den vanliga toohello skapandet av ILB ASEs och externa ASEs. hello följande lista visar parametrarna för särskild anmärkning eller som är unika, när du skapar en ILB ASE:

* *interalLoadBalancingMode*: Ange too3, vilket innebär att båda HTTP/HTTPS-trafik på portarna 80/443, i de flesta fall och hello kontrolldata/channel-portar har lyssnat tooby hello FTP-tjänsten på hello ASE kommer att vara bundna tooan ILB-allokerade virtuella nätverk intern adress. Om den här egenskapen anges too2 endast hello FTP tjänstrelaterade portar (både kontroll- och kanaler) är bundna tooan ILB-adress. Det finns kvar på hello offentliga VIP hello HTTP/HTTPS-trafik.
* *dnsSuffix*: den här parametern anger hello standard rotdomänen som är tilldelade toohello ASE. I hello offentliga variationen i Azure App Service, hello standard rotdomänen för alla webbappar är *azurewebsites.net*. Eftersom en ILB ASE är interna tooa kundens virtuella nätverk, går det inte att meningsfullt toouse hello offentliga tjänstens standard rotdomänen. I stället ska en ILB ASE ha en standard-rotdomän som passar för användning i ett företags interna virtuella nätverk. Contoso Corporation kan till exempel använda en standard rotdomän *intern contoso.com* för appar som är avsedda toobe matchas och tillgänglig endast Contosos virtuellt nätverk. 
* *ipSslAddressCount*: den här parametern används automatiskt standardvärden tooa värdet 0 i hello *azuredeploy.json* filen eftersom ILB ASEs bara ha en enda ILB-adress. Det finns inga explicit SSL-IP-adresser för en ILB ASE. Därför måste hello SSL-IP-adresspool för ett ILB ASE anges toozero. Annars uppstår ett etablering. 

Efter hello *azuredeploy.parameters.json* filen är ifylld, skapa hello ASE med hjälp av hello PowerShell kodstycke. Ändra hello sökvägar toomatch hello Resource Manager-mallfil filplatser på din dator. Kom ihåg toosupply egna värden för hello Resource Manager distributionsnamn och hello resursgruppens namn:

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Det tar ungefär en timme för hello ASE toobe skapas. Sedan visas hello ASE i hello-portal i hello lista över ASEs för hello-prenumeration som utlöste hello-distribution.

## <a name="upload-and-configure-hello-default-ssl-certificate"></a>Överför och konfigurerar hello ”default” SSL-certifikat
Ett SSL-certifikat måste vara kopplad till hello ASE som hello ”default” SSL-certifikat som används tooestablish SSL-anslutningar tooapps. Om DNS-suffixet för hello ASES standard *intern contoso.com*, en anslutning toohttps://some-random-app.internal-contoso.com kräver ett SSL-certifikat som gäller för **.internal contoso.com* . 

Skaffa ett giltigt SSL-certifikat med hjälp av interna certifikatutfärdare, köpa ett certifikat från en extern utfärdare eller med ett självsignerat certifikat. Oavsett hello källa för hello SSL-certifikat, måste hello följande certifikat attribut vara korrekt konfigurerade:

* **Ämne**: det här attributet måste anges för **.your-rot-domain-here.com*.
* **Alternativt ämnesnamn**: det här attributet måste innehålla både **.your-rot-domain-here.com* och **.scm.your-rot-domain-here.com*. SSL-anslutningar toohello SCM/Kudu-plats som är associerade med varje app använder en adress i formatet hello *your-app-name.scm.your-root-domain-here.com*.

Med ett giltigt SSL-certifikat i hand krävs två ytterligare förberedande åtgärder. Konvertera/spara hello SSL-certifikatet som en .pfx-fil. Kom ihåg att hello .pfx-filen måste innehålla alla mellanliggande och root certifikat. Skydda den med ett lösenord.

hello .pfx-fil måste toobe konverteras till en base64-sträng eftersom hello SSL-certifikat har överförts av en Resource Manager-mall. Eftersom Resource Manager-mallar är textfiler, måste hello .pfx-fil konverteras till en base64-sträng. Det här sättet kan den inkluderas som en parameter för hello mallen.

Använd hello följande PowerShell-kodstycke till:

* Generera ett självsignerat certifikat.
* Exportera hello certifikatet som en .pfx-fil.
* Konvertera hello .pfx-fil till en base64-kodad sträng.
* Spara hello base64-kodad sträng tooa separat fil. 

Den här PowerShell-koden för base64-kodning bygger på hello [PowerShell-skript blogg][examplebase64encoding]:

        $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

        $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
        $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

        $fileName = "exportedcert.pfx"
        Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

        $fileContentBytes = get-content -encoding byte $fileName
        $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
        $fileContentEncoded | set-content ($fileName + ".b64")

När hello SSL-certifikatet har genererats och konverteras tooa base64-kodad sträng använder hello exempel Resource Manager-mall [konfigurera hello standard SSL-certifikat] [ quickstartconfiguressl] på GitHub. 

Hej parametrar i hello *azuredeploy.parameters.json* fil finns här:

* *appServiceEnvironmentName*: hello namnet på hello ILB ASE som konfigureras.
* *existingAseLocation*: Text sträng som innehåller hello Azure-region där hello ILB ASE har distribuerats.  Till exempel: ”södra centrala USA”.
* *pfxBlobString*: hello based64-kodad sträng som innehåller hello .pfx-fil. Använd hello kodstycke som visades tidigare och kopiera hello sträng som finns i ”exportedcert.pfx.b64”. Klistra in den i som hello värde för hello *pfxBlobString* attribut.
* *lösenordet*: hello lösenord som används för toosecure hello .pfx-fil.
* *certificateThumbprint*: hello certifikatets tumavtryck. Om du hämtar värdet från PowerShell (till exempel *$certificate. Tumavtryck för* från hello tidigare kodstycke), kan du använda hello-värde som är. Om du kopierar hello värdet från dialogrutan för hello Windows certifikat, Kom ihåg toostrip ut hello extra blanksteg. Hej *certificateThumbprint* ska se ut ungefär AF3143EB61D43F6727842115BB7F17BBCECAECAE.
* *certificateName*: ett eget strängidentifierare väljer själv används tooidentity hello certifikat. hello namnet används som en del av hello Unik identifierare för Resource Manager för hello *Microsoft.Web/certificates* entitet som representerar hello SSL-certifikat. hello namnet *måste* avslutas med hello följande suffix: \_yourASENameHere_InternalLoadBalancingASE. hello Azure-portalen använder det här suffixet som en indikator på att hello certifikat används toosecure en ASE ILB-aktiverade.

Ett förkortat exempel på *azuredeploy.parameters.json* visas här:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Efter hello *azuredeploy.parameters.json* filen fylls i, konfigurera hello standard SSL-certifikat med hjälp av hello PowerShell kodstycke. Ändra hello filen sökvägar toomatch där hello Resource Manager template-filerna finns på din dator. Kom ihåg toosupply egna värden för hello Resource Manager distributionsnamn och hello resursgruppens namn:

     $templatePath="PATH\azuredeploy.json"
     $parameterPath="PATH\azuredeploy.parameters.json"

     New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Det tar ungefär 40 minuter per ASE klientdelen tooapply hello ändring. Att till exempel för en standardstorlek ASE som använder två frontwebbservrarna, hello mallen tar runt en timme och 20 minuter toocomplete. Medan hello mallen körs hello ASE går inte att skala.  

Appar på hello ILB ASE kan nås via HTTPS när hello mallen har slutförts. hello anslutningar skyddas med hjälp av hello standard SSL-certifikat. hello standard SSL-certifikat används när apparna på hello ILB ASE åtgärdas genom att använda en kombination av hello programnamn plus hello standardnamnet för värden. Till exempel https://mycustomapp.internal-contoso.com använder hello standard SSL-certifikat för **.internal contoso.com*.

Utvecklare kan dock precis som appar som körs på hello offentliga multitenant tjänsten, konfigurera anpassade värdnamn för enskilda appar. De kan också konfigurera unika SNI SSL-certifikatbindningar för enskilda appar.

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##
Apptjänst-miljö har två versioner: ASEv1 och ASEv2. hello föregående information har baserat på ASEv2. Det här avsnittet visar hello av skillnaderna mellan ASEv1 och ASEv2.

I ASEv1 hanterar alla hello resurser manuellt. Som innehåller hello frontwebbservrarna, personal och IP-adresser som används för IP-baserade SSL. Innan du kan skala ut din programtjänstplan, måste du skalar upp hello arbetspool som du vill toohost den.

ASEv1 använder en annan prisnivå modell från ASEv2. ASEv1 betalar du för varje kärna allokerade. Som innehåller kärnor som används för frontwebbservrarna eller personer som inte är värd för alla arbetsbelastningar. I ASEv1 är hello maximal skala standardstorlek en ASE 55 Totalt antal värdar. Som innehåller arbetare och frontwebbservrarna. En fördel tooASEv1 är kan distribueras i ett klassiskt virtuellt nätverk och ett virtuellt nätverk för hanteraren för filserverresurser. toolearn mer om ASEv1, se [Apptjänstmiljö v1 introduktion][ASEv1Intro].

toocreate en ASEv1 med hjälp av en Resource Manager-mall finns [skapar en ILB ASE v1 med Resource Manager-mall][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
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
[ILBASEv1Template]: ../../app-service-web/app-service-app-service-environment-create-ilb-ase-resourcemanager.md
