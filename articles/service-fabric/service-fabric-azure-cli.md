---
title: "aaaGetting igång med Azure Service Fabric XPlat CLI"
description: "Komma igång med Azure Service Fabric XPlat CLI"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: c3ec8ff3-3b78-420c-a7ea-0c5e443fb10e
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: e4baa30536b4d8668d8efad301ed8210eb9c0335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-xplat-cli-toointeract-with-a-service-fabric-cluster"></a>Hej XPlat CLI toointeract med ett Service Fabric-kluster

Du kan interagera med Service Fabric-kluster från Linux-datorer med hjälp av hello XPlat CLI på Linux.

hello första steget är att hämta hello senaste versionen av hello CLI från hello git rep och ange den i din sökväg med hjälp av hello följande kommandon:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Den stöder, du kan ange hello namn av hello kommandot tooobtain hello hjälp för kommandot för varje kommando.
Automatisk komplettering stöds för hello-kommandon. Till exempel hello efter kommandot får du hjälp för alla kommandon som programmet hello. 

```sh
 azure servicefabric application 
```

Du kan filtrera hello hjälp tooa specifika kommandot ytterligare som följande exempel visar hello:

```sh
 azure servicefabric application  create
```

tooenable automatisk komplettering i hello CLI kör hello följande kommandon:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Hej efter kommandona Anslut toohello klustret och visar hello av noderna i klustret hello:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

toouse namngivna parametrar och hitta vad de är, kan du skriva--hjälpa efter kommandot. Exempel:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

När du ansluter flera datorer tooa-kluster från en dator **som inte ingår i klustret hello**, använda hello följande kommando:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Ersätt hello PublicIPorFQDN taggen med hello verkliga IP eller FQDN efter behov. När du ansluter flera datorer tooa-kluster från en dator **som ingår i klustret hello**, använda hello följande kommando:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Du kan använda PowerShell eller CLI toointeract med Linux Service Fabric-kluster skapas med hello Azure-portalen.

> [!WARNING]
> Dessa kluster är inte säker, därför du kanske öppnar din som visas genom att lägga till hello offentliga IP-adressen i hello klustermanifestet.

## <a name="using-hello-xplat-cli-tooconnect-tooa-service-fabric-cluster"></a>Med hjälp av hello XPlat CLI tooconnect tooa Service Fabric-kluster

hello följande Azure CLI-kommandona beskrivs hur tooconnect tooa skydda klustret. information om hello certifikat måste matcha ett certifikat på hello klusternoder.

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

Om ditt certifikat har certifikatutfärdare (CA), måste tooadd hello--ca-certifikat-sökvägsparameter som hello följande exempel: 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

Om du har flera certifikatutfärdare kan du använda ett kommatecken som hello avgränsare.

Om ditt eget namn i hello certifikatet inte matchar hello Anslutningens slutpunkt, kan du använda parametern hello `--strict-ssl-false` toobypass hello verifieringen enligt hello följande kommando:

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

Om du vill att tooskip hello CA verifiering, kan du lägga till hello--avvisa obehörig false parametern enligt hello följande kommando: 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

När du har anslutit bör du kunna toorun toointeract andra CLI-kommandon med hello klustret.

## <a name="deploying-your-service-fabric-application"></a>Distribuera programmet Service Fabric

Kör följande kommandon toocopy, registrera och starta hello service fabric programmet hello:

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a>Uppgradera ditt program

hello-processen är liknande toohello [processer i Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Skapa, kopiera, registrera och skapa programmet från projektet rotkatalog. Om din instans av programmet heter `fabric:/MySFApp`, och hello typ är MySFApp, hello kommandon blir som följer:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Se hello ändra tooyour programmet och återskapa hello ändrade tjänsten.  Uppdateringen hello ändrade tjänstens manifestfil (ServiceManifest.xml) med hello uppdaterade versioner hello Service (och koden eller Config eller Data efter behov). Även ändra hello programmets manifest (ApplicationManifest.xml) med hello uppdatera versionsnumret för programmet hello och hello ändrade tjänsten.  Nu kan kopiera och registrera ditt uppdaterade program med hjälp av hello följande kommandon:

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Du kan nu starta uppgradering av programmet hello med hello följande kommando:

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

Nu kan du övervaka uppgradering av programmet hello med SFX. Om några minuter skulle hello-programmet har uppdaterats. Du kan också prova en uppdaterad app med ett fel och kontrollera hello automatisk återställning i service fabric.

## <a name="converting-from-pfx-toopem-and-vice-versa"></a>Konvertera från PFX tooPEM och vice versa

Du kan behöva tooinstall ett certifikat i din lokala dator (med Windows eller Linux) tooaccess säker kluster som kan finnas i en annan miljö. Till exempel vid åtkomst till en skyddad Linux-kluster från en Windows-dator och vice versa kanske du måste tooconvert ditt certifikat från PFX tooPEM och vice versa. 

tooconvert från en PEM-filen tooa PFX-fil, Använd hello följande kommando:

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

tooconvert från en PFX-filen tooa PEM-fil, Använd hello följande kommando:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Se för[OpenSSL dokumentationen](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) mer information.

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a>Felsökning


### <a name="copying-of-hello-application-package-does-not-succeed"></a>Kopiering av hello programpaket lyckas inte

Kontrollera om `openssh` är installerad. Ubuntu Desktop har inte installerats. Installera den med hjälp av hello följande kommando:

```sh
sudo apt-get install openssh-server openssh-client**
```

Om hello problemet kvarstår kan du prova att inaktivera PAM för ssh genom att ändra hello `sshd_config` fil med hello följande kommandon:

```sh
sudo vi /etc/ssh/sshd_config
#Change hello line with UsePAM toohello following: UsePAM no
sudo service sshd reload
```

Om hello problemet kvarstår kan du försöka ökande hello antal ssh sessioner genom att köra följande kommandon hello:

```sh
sudo vi /etc/ssh/sshd\_config
# Add hello following toolines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

Använder nycklar för ssh-autentisering (som skillnad från toopasswords) ännu stöds inte (eftersom hello-plattformen använder ssh toocopy paket), så Använd lösenordsautentisering i stället.

## <a name="next-steps"></a>Nästa steg

[Ställ in hello utvecklingsmiljö och distribuera ett Service Fabric application tooa Linux-kluster.](service-fabric-get-started-linux.md)

## <a name="related-articles"></a>Relaterade artiklar

* [Kom igång med Service Fabric och Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
