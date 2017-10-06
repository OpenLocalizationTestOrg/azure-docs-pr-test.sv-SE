---
title: "aaaHow tooCreate en ILB ASE med hjälp av Azure Resource Manager-mallar | Microsoft Docs"
description: "Lär dig hur ladda toocreate en intern belastningsutjämnare ASE med hjälp av Azure Resource Manager-mallar."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.openlocfilehash: 16db20eccc232ccc73107fcc8291de180fb2a323
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-ilb-ase-using-azure-resource-manager-templates"></a>Hur tooCreate en ILB ASE med hjälp av Azure Resource Manager-mallar

> [!NOTE] 
> Den här artikeln handlar om hello Apptjänstmiljö v1. Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
>

## <a name="overview"></a>Översikt
Apptjänstmiljöer kan skapas med ett internt virtuellt nätverk-adress i stället för en offentlig VIP.  Den här interna adressen tillhandahålls av en Azure komponent som kallas hello intern belastningsutjämnare (ILB).  En ILB ASE kan skapas med hello Azure-portalen.  Det kan även skapas med hjälp av automation via Azure Resource Manager-mallar.  Den här artikeln går igenom hello stegen och syntax behövs toocreate en ILB ASE med Azure Resource Manager-mallar.

Det finns tre steg ingår i automatisera skapandet av en ILB ASE:

1. Första hello grundläggande ASE skapas i ett virtuellt nätverk med ett internt belastningsutjämnaradress i stället för en offentlig VIP.  Som en del av det här steget måste tilldelas ett namn för skogsrotsdomänen toohello ILB ASE.
2. När hello ILB ASE skapas ett SSL-certifikat har överförts.  
3. hello är överförda SSL-certifikat uttryckligen tilldelad toohello ILB ASE som dess ”default” SSL-certifikat.  SSL-certifikatet som ska användas för SSL-trafik tooapps på hello ILB ASE när hello appar adresseras med hello vanliga rot domän tilldelats toohello ASE (t.ex. https://someapp.mycustomrootcomain.com)

## <a name="creating-hello-base-ilb-ase"></a>Skapa hello Base ILB ASE
Ett exempel Azure Resource Manager-mall och dess associerade parametrar-fil finns på GitHub [här][quickstartilbasecreate].

De flesta hello parametrar i hello *azuredeploy.parameters.json* filen är vanliga toocreating båda ILB ASEs samt ASEs bunden tooa offentliga VIP.  hello listan nedan anrop out-parametrar för särskilda anteckning eller som är unika, när du skapar en ILB ASE:

* *interalLoadBalancingMode*: Ange too3, vilket innebär att båda HTTP/HTTPS-trafik på portarna 80/443, i de flesta fall och hello kontrolldata/channel-portar har lyssnat tooby hello FTP-tjänsten på hello ASE, bundna tooan ILB allokeras virtuellt nätverk intern adress.  Om den här egenskapen anges i stället too2, och sedan endast hello FTP-tjänsten relaterade portar (både kontroll- och kanaler) kommer att bindas tooan ILB-adress, medan hello HTTP/HTTPS-trafiken ska finnas kvar på hello offentliga VIP.
* *dnsSuffix*: den här parametern anger hello standard-rotdomän som ska tilldelas toohello ASE.  I hello offentliga variationen i Azure App Service, hello standard rotdomänen för alla webbappar är *azurewebsites.net*.  Men eftersom en ILB ASE interna tooa kundens virtuella nätverk, inte blir meningsfullt toouse hello offentliga tjänstens standard rotdomänen.  I stället ska en ILB ASE ha en standard-rotdomän som passar för användning i ett företags interna virtuella nätverk.  En hypotetiska Contoso Corporation kan till exempel använda en standard rotdomän *intern contoso.com* för appar som är avsedda tooonly vara matchas och tillgänglig Contosos virtuellt nätverk. 
* *ipSslAddressCount*: den här parametern är automatiskt som standard tooa värdet 0 i hello *azuredeploy.json* filen eftersom ILB ASEs bara ha en enda ILB-adress.  Inga explicit SSL-IP-adresser för en ILB ASE och därför hello SSL-IP-adresspool för ett ILB ASE måste anges toozero, annars uppstår ett etablering fel. 

En gång hello *azuredeploy.parameters.json* filen har fyllts i för en ILB ASE, hello ILB ASE kan skapas med hjälp av hello följande kodavsnitt i Powershell.  Ändra hello filen sökvägar toomatch där hello Azure Resource Manager template-filerna finns på din dator.  Kom ihåg toosupply egna värden för hello Azure Resource Manager distributionsnamnet och resursgruppens namn.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Om hello Azure Resource Manager skickas mallen tar några timmar för hello ILB ASE toobe skapas.  När hello skapa är klar visas hello ILB ASE i hello portal UX i hello lista över Apptjänstmiljöer för hello-prenumeration som utlöste hello distribution.

## <a name="uploading-and-configuring-hello-default-ssl-certificate"></a>Överför och konfigurerar hello ”standard” SSL-certifikat
En gång hello ILB ASE skapas, ett SSL-certifikat ska vara associerat med hello ASE som hello ”default” SSL-certifikat används för att fastställa tooapps för SSL-anslutningar.  Fortsätter med hello hypotetiska Contoso Corporation exempelvis om hello ASES standard DNS-suffix är *intern contoso.com*, sedan en anslutning för*https://some-random-app.internal-contoso.com*kräver ett SSL-certifikat som gäller för **.internal contoso.com*. 

Det finns en mängd olika sätt tooobtain ett giltigt SSL-certifikat inklusive interna CA: er, köpa ett certifikat från en extern utfärdare och använda ett självsignerat certifikat.  Oavsett hello källa för hello SSL-certifikat måste hello följande certifikat-attribut toobe konfigurerats korrekt:

* *Ämne*: det här attributet måste anges för **.your-rot-domain-here.com*
* *Alternativt ämnesnamn*: det här attributet måste innehålla både **.your-rot-domain-here.com*, och **.scm.your-rot-domain-here.com*.  Hej för hello andra posten beror på att SSL-anslutningar toohello SCM/Kudu-plats som är associerade med varje app kommer att göras med en adress i formatet hello *your-app-name.scm.your-root-domain-here.com*.

Med ett giltigt SSL-certifikat i hand krävs två ytterligare förberedande åtgärder.  hello SSL-certifikat måste toobe konverteras/sparas som en .pfx-fil.  Kom ihåg att hello .pfx-fil måste alla mellanliggande tooinclude och rotcertifikat och även måste toobe skyddas med ett lösenord.

Hello gällande .pfx-fil måste toobe konverteras till en base64-sträng eftersom hello SSL-certifikatet kommer att överföras med en Azure Resource Manager-mall.  Eftersom Azure Resource Manager-mallar är textfiler, måste toobe konverteras till en base64-sträng, så den kan inkluderas som en parameter för hello mallen hello .pfx-fil.

hello Powershell kodfragmentet nedan visar ett exempel på Generera ett självsignerat certifikat, exporterar hello certifikat som en .pfx-fil, konvertera hello .pfx-fil till en base64-kodad sträng och sedan spara hello base64-kodade sträng tooa separat fil.  Hej Powershell-koden för base64-kodning bygger på hello [Powershell-skript blogg][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

När hello SSL-certifikatet har genererats och konverterade tooa base64-kodad sträng hello Azure Resource Manager-mall för exempel på GitHub för [Konfigurera SSL-certifikat för hello standard] [ configuringDefaultSSLCertificate] kan användas.

Hej parametrar i hello *azuredeploy.parameters.json* filen visas nedan:

* *appServiceEnvironmentName*: hello namnet på hello ILB ASE som konfigureras.
* *existingAseLocation*: Text sträng som innehåller hello Azure-region där hello ILB ASE har distribuerats.  Till exempel: ”södra centrala USA”.
* *pfxBlobString*: hello based64 kodad strängrepresentation av hello .pfx-fil.  Använder hello kodstycke som visades tidigare, skulle du kopiera hello sträng som finns i ”exportedcert.pfx.b64” och klistra in den i som hello värde för hello *pfxBlobString* attribut.
* *lösenordet*: hello lösenord som används för toosecure hello .pfx-fil.
* *certificateThumbprint*: hello certifikatets tumavtryck.  Om du hämtar värdet från Powershell (t.ex. *$certificate. Tumavtryck för* från hello tidigare kodstycke), kan du använda hello-värde som-är.  Men om du kopierar hello värdet från hello Windows certifikat dialogrutan Spara toostrip ut hello extra blanksteg.  Hej *certificateThumbprint* ska se ut ungefär: AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *certificateName*: ett eget strängidentifierare väljer själv används tooidentity hello certifikat.  hello namnet används som en del av hello Unik identifierare för Azure Resource Manager för hello *Microsoft.Web/certificates* entitet som representerar hello SSL-certifikat.  hello namnet **måste** avslutas med hello följande suffix: \_yourASENameHere_InternalLoadBalancingASE.  Det här suffixet används av hello portalen som en indikator på att hello certifikat används för att skydda en ASE ILB-aktiverade.

Ett förkortat exempel på *azuredeploy.parameters.json* visas nedan:

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

En gång hello *azuredeploy.parameters.json* filen har fyllts i, hello standard SSL-certifikat kan konfigureras med hello följande kodavsnitt i Powershell.  Ändra hello filen sökvägar toomatch där hello Azure Resource Manager template-filerna finns på din dator.  Kom ihåg toosupply egna värden för hello Azure Resource Manager distributionsnamnet och resursgruppens namn.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Om hello Azure Resource Manager skickas mallen tar ungefär 40 minuter minuter per ASE frontend tooapply hello ändring.  Till exempel med en standard tar storlek ASE med två framför-ends hello mallen runt en timme och 20 minuter toocomplete.  Medan hello mallen körs hello ASE inte kan tooscaled.  

När hello mallen har slutförts appar på hello ILB ASE kan nås över HTTPS och hello anslutningar säkras med hello standard SSL-certifikat.  hello standard SSL-certifikat används när apparna på hello ILB ASE behandlas med en kombination av hello programnamn plus hello standard värdnamn.  Till exempel *https://mycustomapp.internal-contoso.com* använder hello standard SSL-certifikat för **.internal contoso.com*.

Dock precis som appar körs på hello offentlig tjänst med flera klienter, kan utvecklare också konfigurera anpassade värdnamn för enskilda appar och sedan konfigurera unika SNI SSL-certifikatbindningar för enskilda appar.  

## <a name="getting-started"></a>Komma igång
tooget igång med Apptjänstmiljöer, se [introduktion tooApp-miljö](app-service-app-service-environment-intro.md)

Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

