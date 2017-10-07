---
title: "aaaConfigure SSL för en tjänst i molnet (klassisk) | Microsoft Docs"
description: "Lär dig hur toospecify en HTTPS-slutpunkt för en webbroll och hur tooupload en SSL-certifikatet toosecure ditt program."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: a1ca031b98af49d371977a208ed24f6dc8ea2ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfigurera SSL för ett program i Azure
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
> * [Klassisk Azure-portal](cloud-services-configure-ssl-certificate.md)
> 
> 

Secure Socket Layer (SSL)-kryptering är hello vanligaste metoden för att skydda data som skickas över hello internet. Arbetet beskrivs hur toospecify en HTTPS-slutpunkt för en webbroll och hur tooupload en SSL-certifikatet toosecure ditt program.

> [!NOTE]
> hello procedurer i det här steget gäller tooAzure molntjänster; App-tjänster, se [detta](../app-service-web/web-sites-configure-ssl-certificate.md) artikel.
> 
> 

Den här aktiviteten använder en Produktionsdistribution. Information om hur du använder en fristående distribution har angetts hello slutet av det här avsnittet.

Läs [detta](cloud-services-how-to-create-deploy.md) artikeln först om du inte har skapat en tjänst i molnet.

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Steg 1: Hämta ett SSL-certifikat
tooconfigure SSL för ett program måste du först tooget ett SSL-certifikat som har signerats av en certifikatet (CA), en betrodd tredje part som utfärdar certifikat för detta ändamål. Om du inte redan har en, måste tooobtain något på ett företag som säljer SSL-certifikat.

hello certifikat måste uppfylla följande krav för SSL-certifikat i Azure hello:

* hello certifikatet måste innehålla en privat nyckel.
* hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.
* hello måste certifikatets ämnesnamn matcha hello domän används tooaccess hello-Molntjänsten. Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello cloudapp.net domän. Du måste skaffa en anpassad domän namnet toouse när komma åt tjänsten. När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domänen namnet som används för tooaccess ditt program. Om ditt domännamn är till exempel **contoso.com** du vill begära ett certifikat från Certifikatutfärdaren för ***. contoso.com** eller **www.contoso.com**.
* hello certifikat måste använda minst 2048-bitars kryptering.

För testning, kan du [skapa](cloud-services-certs-create.md) och använda ett självsignerat certifikat. Ett självsignerat certifikat har autentiserats inte via en Certifikatutfärdare och kan använda hello cloudapp.net domän som hello Webbadress. Till exempel hello följande uppgift använder ett självsignerat certifikat i vilka hello nätverksnamn (CN) används i hello certifikat är **sslexample.cloudapp.net**.

Därefter måste du ha information om hello certifikat i tjänstdefinitionen och konfigurationsfiler för tjänsten.

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a>Steg 2: Ändra hello service definition och configuration-filer
Programmet måste vara konfigurerade toouse hello certifikat och en HTTPS-slutpunkt måste läggas till. Därför hello tjänstdefinitionen och service configuration-filer måste toobe uppdateras.

1. Öppna i din utvecklingsmiljö hello tjänstdefinitionsfilen (CSDEF) och lägga till en **certifikat** avsnitt i hello **WebRole** avsnittet och inkludera hello följande information den certifikatet (och mellanliggande certifikat):
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                        storeLocation="LocalMachine" 
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by hello CA root, you
            must include all hello intermediate certificates
            here. You must list them here, even if they are
            not bound tooany endpoints. Failing toolist any of
            hello intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```
   
   Hej **certifikat** avsnittet definierar hello namnet på vår certifikat, dess plats och hello namnet på hello store där den finns.
   
   Behörigheter (`permisionLevel` attributet) kan vara set tooone av hello följande värden:
   
   | Behörighetsvärdet | Beskrivning |
   | --- | --- |
   | limitedOrElevated |**(Standard)**  Alla processer som rollen har åtkomst till hello privat nyckel. |
   | upphöjd |Endast utökade processer kan komma åt hello privat nyckel. |
2. Lägg till i din tjänstdefinitionsfilen en **InputEndpoint** element i hello **slutpunkter** avsnittet tooenable HTTPS:
   
    ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. Lägg till i din tjänstdefinitionsfilen en **bindning** element i hello **platser** avsnitt. Det här avsnittet lägger till en HTTPS-bindningen toomap endpoint tooyour plats:
   
    ```xml   
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```
   
   Alla hello nödvändiga ändringar toohello tjänstdefinitionsfilen har slutförts, men du behöver fortfarande tooadd hello certifikatinformationen till hello tjänstkonfigurationsfilen.
4. Lägg till i din tjänstekonfigurationsfil (CSCFG) ServiceConfiguration.Cloud.cscfg, en **certifikat** avsnitt i hello **rollen** avsnittet, ersätter hello tumavtrycket exempelvärde nedan med som ditt certifikat:
   
    ```xml   
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate" 
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

(hello föregående exempel använder **sha1** för hello tumavtrycket algoritmen. Ange hello lämpligt värde för din certifikatets tumavtryck algoritm.)

Nu när hello service definitions- och konfigurationsfiler har uppdaterats, paketera din distribution för uppladdning av tooAzure. Om du använder **cspack**, Använd inte den **/generateConfigurationFile** flagga som som skriver över certifikatinformationen du infogas.

## <a name="step-3-upload-a-certificate"></a>Steg 3: Överför certifikat
Distributionspaketet har uppdaterats toouse hello certifikat och en HTTPS-slutpunkt har lagts till. Nu kan du överföra hello och certifikat tooAzure med hello klassiska Azure-portalen.

1. Logga in toohello [klassiska Azure-portalen][Azure classic portal]. 
2. Klicka på **molntjänster** hello vänstra navigeringsfönstret.
3. Klicka på hello önskad tjänst i molnet.
4. Klicka på hello **certifikat** fliken.
   
    ![Klicka på fliken för hello-certifikat](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Klicka på hello **överför** knappen.
   
    ![Ladda upp](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Ange hello **filen**, **lösenord**, klicka på **Slutför** (hello bock).

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a>Steg 4: Anslut toohello rollinstansen med hjälp av HTTPS
Nu när distributionen är igång och körs i Azure, kan du ansluta tooit med hjälp av HTTPS.

1. I hello klassiska Azure-portalen, Välj distributionen och sedan på hello länken under **Webbadress**.
   
   ![Fastställa Webbadress][2]
2. Ändra hello länken toouse i webbläsaren, **https** i stället för **http**, och besök hello-sidan.
   
   > [!NOTE]
   > Om du använder ett självsignerat certifikat när du bläddrar tooan HTTPS-slutpunkt som är kopplad till hello självsignerat certifikat kan du se ett certifikatfel i hello webbläsare. Använder ett certifikat som signerats av en betrodd certifikatutfärdare åtgärdar problemet. i hello tiden kan du ignorera hello felet. (Ett annat alternativ är tooadd hello självsignerat certifikat toohello användarens certifikat från betrodd utfärdare certifikatarkiv.)
   > 
   > 
   
   ![Webbplats för SSL-exempel][3]

Om du vill toouse SSL för en fristående distribution i stället för en Produktionsdistribution, måste du först toodetermine hello URL som används för hello mellanlagring av distribution. Distribuera service toohello fristående molnmiljön utan ett certifikat eller information om certifikat. Distribution kan du bestämma hello GUID-baserad URL, vilket visas i hello klassiska Azure-portalen **Webbadress** fältet. Skapa ett certifikat med hello vanliga namn (CN) lika toohello GUID-baserad URL (till exempel **32818777 6e77-4ced-a8fc-57609d404462.cloudapp.net**). Använd hello Azure klassiska portal tooadd hello certifikat tooyour mellanlagras tjänst i molnet. Lägg sedan till hello information tooyour CSDEF- och CSCFG certifikatfiler, paketera programmet och uppdatera mellanlagrade toouse hello nya distributionspaketet.

## <a name="next-steps"></a>Nästa steg
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure.md).
* Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name.md).
* [Hantera din molntjänst](cloud-services-how-to-manage.md).

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
