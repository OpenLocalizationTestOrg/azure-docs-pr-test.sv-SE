---
title: aaaDeploy dina Azure IoT Suite anslutna factory gateway | Microsoft Docs
description: "Hur toodeploy en gateway på Windows- eller Linux tooenable anslutning toohello anslutna factory förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 72436dec60eda0de20143f362fe740b0c4412f36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-hello-connected-factory-preconfigured-solution"></a>Distribuera en gateway på Windows- eller Linux för hello anslutna factory förkonfigurerade lösningen

hello obligatorisk programvara toodeploy en gateway för hello anslutna factory förkonfigurerade lösningen består av två komponenter:

* Hej *OPC Proxy* upprättar en anslutning tooIoT hubb. Hej *OPC Proxy* sedan väntar på kommando- och meddelanden från hello integrerad OPC webbläsare som körs i hello anslutna factory lösning portal.
* Hej *OPC Publisher* ansluter tooexisting lokalt OPC UA servrar och vidarebefordrar telemetri från dem tooIoT hubb.

Båda komponenterna är öppen källkod och är tillgängliga som källa på GitHub och Docker-behållare:

| GitHub | DockerHub |
| ------ | --------- |
| [OPC Publisher][lnk-publisher-github] | [OPC Publisher][lnk-publisher-docker] |
| [OPC-Proxy][lnk-proxy-github] | [OPC-Proxy][lnk-proxy-docker] |

Ingen offentlig IP-adress eller tomrum i brandväggen för hello gateway krävs för någon komponent. använda endast utgående portarna 443, 5671 och 8883 hello OPC-Proxy och OPC utgivare.

hello steg i den här artikeln visar hur toodeploy en gateway med Docker på antingen [Windows](#windows-deployment) eller [Linux](#linux-deployment). hello gateway kan toohello anslutna factory förkonfigurerade lösning.

> [!NOTE]
> hello gateway-programvara som körs i hello dockerbehållare är [Azure IoT kant].

## <a name="windows-deployment"></a>Windows-distribution

> [!NOTE]
> Om du ännu inte har en gateway-enhet, rekommenderar Microsoft att du köper en kommersiell gateway från någon av våra partners. Besök hello [Azure IoT-enhet katalogen] för gateway-enheter som är kompatibla med hello anslutet factory lösning. Följ instruktionerna för hello som medföljer hello enheten tooset in hello gateway. Du kan också använda hello följande instruktioner toomanually ställa in ett av dina befintliga gatewayer.

### <a name="install-docker"></a>Installera Docker

Installera [Docker för Windows] på din Windows-baserade gateway-enhet. Välj en enhet på din värd datorn tooshare med Docker under Windows Docker-installationen. följande skärmbild visar dela hello D enheten på datorn Windows hello:

![Installera Docker][img-install-docker]

Skapa en mapp med namnet **docker** i hello rot hello delad enhet.
Du kan också utföra det här steget när du har installerat docker från hello **inställningar** menyn.

### <a name="configure-hello-gateway"></a>Konfigurera hello-gateway

1. Du behöver hello **iothubowner** anslutningssträngen för dina Azure IoT Suite anslutna factory distribution toocomplete hello gateway-distribution. I hello [Azure-portalen], navigera tooyour IoT-hubb i hello resursgruppen skapade när du distribuerade hello anslutna factory lösning. Klicka på **principer för delad åtkomst** tooaccess hello **iothubowner** anslutningssträngen:

    ![Hitta hello anslutningssträngen för IoT-hubb][img-hub-connection]

    Kopiera hello **anslutningssträngen--primärnyckel** värde.

1. Konfigurera hello gateway för din IoT-hubb genom att köra hello två gateway moduler **när** från en kommandotolk med:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  är hello toogive tooyour OPC UA Publisher hello format **utgivare.&lt; fullständigt kvalificerade domännamnet&gt;**. Om exempelvis factory nätverket kallas **myfactorynetwork.com**, hello **ApplicationName** värdet är **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  är hello **iothubowner** anslutningssträngen som du kopierade i föregående steg i hello. Den här anslutningssträngen används endast i det här steget, du behöver i hello följande steg:

    Du använder hello mappas D:\\docker-mappen (hello `-v` argumentet) senare toopersist hello två X.509-certifikat med hello gateway moduler.

### <a name="run-hello-gateway"></a>Köra hello gateway

1. Starta om hello gateway med hello följande kommandon:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Av säkerhetsskäl hello två X.509-certifikat som är kvar i hello D:\\docker mapp innehåller hello privat nyckel. Begränsa toohello autentiseringsuppgifter toothis mapp (vanligtvis **administratörer**) du använder toorun hello dockerbehållare. Högerklicka på hello D:\\docker-mappen, Välj **egenskaper**, sedan **säkerhet**, och sedan **redigera**. Ge **administratörer** fullständig kontroll och ta bort alla andra:

    ![Bevilja behörigheter tooDocker resursen][img-docker-share]

1. Kontrollera nätverksanslutningen. Ange hello-kommando från en kommandotolk `ping publisher.<your fully qualified domain name>` tooping din gateway. Lägg till hello IP-adress och namnet på gateway toohello hosts-filen på din gateway om hello mål kan inte nås. hello hosts-filen finns i hello **Windows\\System32\\drivrutiner\\etc** mapp.

1. Försök därefter tooconnect toohello utgivaren med hjälp av en lokal OPC UA klient på hello gateway. Hej OPC UA endpoint URL: en är `opc.tcp://publisher.<your fully qualified domain name>:62222`. Om du inte har en OPC UA-klient, hämtar och använder en [öppen källkod OPC UA klienten].

1. När du har slutfört dessa lokala tester, bläddra toohello **ansluta din egen OPC UA Server** sida i hello anslutna factory lösning portal. Ange hello publisher slutpunkts-URL (`tcp://publisher.<your fully qualified domain name>:62222`) och klicka på **Anslut**. Du får en certifikatvarning om och klicka sedan på **Fortsätt.** Nästa du får ett felmeddelande som hello hello UA webbklienten inte litar på utgivaren. tooresolve det här felet, kopiera hello **UA webbklienten** certifikat från hello **D:\\docker\\avvisade certifikat\\certifikat** mappen toohello **D:\\docker\\UA program\\certifikat** mapp på hello gateway. Du behöver inte toorestart för hello gateway. Upprepa det här steget. Nu kan du ansluta toohello gateway hello molnet och du är redo tooadd OPC UA servrar toohello lösning.

### <a name="add-your-opc-ua-servers"></a>Lägga till OPC UA-servrar

1. Bläddra toohello **ansluta din egen OPC UA Server** sida i hello anslutna factory lösning portal. Följ samma steg som i föregående avsnitt tooestablish hello förtroende mellan hello anslutna factory portal och hello OPC UA server hello. Det här steget upprättas ömsesidigt förtroende av hello certifikat från hello ansluten factory portal och hello OPC UA server och skapar en anslutning.

1. Bläddra hello OPC UA noder trädet serverns OPC UA, högerklicka på hello OPC noder och välj **publicera**. Hello OPC UA server för publishing toowork sätt och hello publisher måste finnas på hello samma nätverk. Med andra ord om hello fullständigt kvalificerade domännamnet för hello utgivaren är **publisher.mydomain.com** sedan hello fullständigt kvalificerade domännamnet för hello OPC UA server måste vara, till exempel **myopcuaserver.mydomain.com**. Om din konfiguration skiljer sig, kan du manuellt lägga till noder toohello publishesnodes.json fil hittades i hello **D:\\docker** mapp. Hej publishesnodes.json automatiskt genereras på hello först lyckad publicering av en OPC-nod.

1. Telemetri nu flödar från hello gateway-enhet. Du kan visa hello telemetri i hello **Factory platser** vy över hello anslutna factory portal under **nya Factory**.

## <a name="linux-deployment"></a>Linux-distribution

> [!NOTE]
> Om du ännu inte har en gateway-enhet, rekommenderar Microsoft att du köper en kommersiell gateway från någon av våra partners. Besök hello [Azure IoT-enhet katalogen] för gateway-enheter som är kompatibla med hello anslutet factory lösning. Följ instruktionerna för hello som medföljer hello enheten tooset in hello gateway. Du kan också använda hello följande instruktioner toomanually ställa in ett av dina befintliga gatewayer.

[Installera Docker] på Linux-gateway-enheten.

### <a name="configure-hello-gateway"></a>Konfigurera hello-gateway

1. Du behöver hello **iothubowner** anslutningssträngen för dina Azure IoT Suite anslutna factory distribution toocomplete hello gateway-distribution. I hello [Azure-portalen], navigera tooyour IoT-hubb i hello resursgruppen skapade när du distribuerade hello anslutna factory lösning. Klicka på **principer för delad åtkomst** tooaccess hello **iothubowner** anslutningssträngen:

    ![Hitta hello anslutningssträngen för IoT-hubb][img-hub-connection]

    Kopiera hello **anslutningssträngen--primärnyckel** värde.

1. Konfigurera hello gateway för din IoT-hubb genom att köra hello två gateway moduler **när** från ett gränssnitt med:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  är hello namnet på hello OPC UA hello Programgateway skapar hello format **utgivare.&lt; fullständigt kvalificerade domännamnet&gt;**. Till exempel **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  är hello **iothubowner** anslutningssträngen som du kopierade i föregående steg i hello. Den här anslutningssträngen används endast i det här steget, du behöver i hello följande steg:

    Du använder hello **/ delade** mapp (hello `-v` argumentet) senare toopersist hello två X.509-certifikat med hello gateway moduler.

### <a name="run-hello-gateway"></a>Köra hello gateway

1. Starta om hello gateway med hello följande kommandon:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 –-rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Av säkerhetsskäl hello två X.509-certifikat som är kvar i hello **/ delade** mapp innehåller hello privat nyckel. Begränsa åtkomst toothis mappen toohello autentiseringsuppgifter du använder toorun hello dockerbehållare. tooset hello behörigheter för **rot** endast använda hello `chmod` shell kommandot på hello-mappen.

1. Kontrollera nätverksanslutningen. Ange hello-kommando från en shell `ping publisher.<your fully qualified domain name>` tooping din gateway. Lägg till hello IP-adress och namnet på gateway tooyour hosts-filen på din gateway om hello mål kan inte nås. hello hosts-filen finns i hello **/etc** mapp.

1. Försök därefter tooconnect toohello utgivaren med hjälp av en lokal OPC UA klient på hello gateway. Hej OPC UA endpoint URL: en är `opc.tcp://publisher.<your fully qualified domain name>:62222`. Om du inte har en OPC UA-klient, hämtar och använder en [öppen källkod OPC UA klienten].

1. När du har slutfört dessa lokala tester, bläddra toohello **ansluta din egen OPC UA Server** sida i hello anslutna factory lösning portal. Ange hello publisher slutpunkts-URL (`tcp://publisher.<your fully qualified domain name>:62222`) och klicka på **Anslut**. Du får en certifikatvarning om och klicka sedan på **Fortsätt.** Nästa du får ett felmeddelande som hello hello UA webbklienten inte litar på utgivaren. tooresolve det här felet, kopiera hello **UA webbklienten** certifikat från hello **/delad/Avvisad certifikat/certifikat** mappen toohello **/shared/UA program/certifikat** mapp på hello gateway. Du behöver inte toorestart för hello gateway. Upprepa det här steget. Nu kan du ansluta toohello gateway hello molnet och du är redo tooadd OPC UA servrar toohello lösning.

### <a name="add-your-opc-ua-servers"></a>Lägga till OPC UA-servrar

1. Bläddra toohello **ansluta din egen OPC UA Server** sida i hello anslutna factory lösning portal. Följ samma steg som i föregående avsnitt tooestablish hello förtroende mellan hello anslutna factory portal och hello OPC UA server hello. Det här steget upprättas ömsesidigt förtroende av hello certifikat från hello ansluten factory portal och hello OPC UA server och skapar en anslutning.

1. Bläddra hello OPC UA noder trädet serverns OPC UA, högerklicka på hello OPC noder och välj **publicera**. Hello OPC UA server för publishing toowork sätt och hello publisher måste finnas på hello samma nätverk. Med andra ord om hello fullständigt kvalificerade domännamnet för hello utgivaren är **publisher.mydomain.com** sedan hello fullständigt kvalificerade domännamnet för hello OPC UA server måste vara, till exempel **myopcuaserver.mydomain.com**. Om din konfiguration skiljer sig, kan du manuellt lägga till noder toohello publishesnodes.json fil hittades i hello **/ delade** mapp. Hej publishesnodes.json genereras automatiskt på hello först lyckad publicering av en OPC-nod.

1. Telemetri nu flödar från hello gateway-enhet. Du kan visa hello telemetri i hello **Factory platser** vy över hello anslutna factory portal under **nya Factory**.

## <a name="next-steps"></a>Nästa steg

toolearn mer om hello arkitektur av hello anslutna fabriken förkonfigurerade lösningen, se [anslutna factory förkonfigurerade lösningen genomgången][lnk-walkthrough].

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Docker för Windows]: https://www.docker.com/docker-windows
[Azure IoT-enhet katalogen]: https://catalog.azureiotsuite.com/?q=opc
[Azure-portalen]: http://portal.azure.com/
[öppen källkod OPC UA klienten]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Installera Docker]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IoT kant]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy