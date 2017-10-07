---
title: aaaPush Docker bild tooprivate Azure registret | Microsoft Docs
description: "Push och pull Docker-avbildningar tooa privata behållare registret i Azure med hjälp av hello Docker CLI"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a81a6f4bfcb23642a89ac7631348d40e2f4911a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="push-your-first-image-tooa-private-docker-container-registry-using-hello-docker-cli"></a>Push din första bilden tooa privata Docker behållare registret med hjälp av hello Docker CLI
En Azure-behållaren registret lagrar och hanterar privata [Docker](http://hub.docker.com) behållare bilder, liknande toohello sätt [Docker-hubb](https://hub.docker.com/) lagrar offentliga Docker-avbildningar. Du använder hello [Docker-kommandoradsgränssnittet](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) för [inloggning](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), och andra åtgärder på din behållare registret.

Mer bakgrund och begrepp finns [hello översikt](container-registry-intro.md)



## <a name="prerequisites"></a>Krav
* **Azure-behållarregister** – Skapa ett behållarregister i din Azure-prenumeration. Till exempel använda hello [Azure-portalen](container-registry-get-started-portal.md) eller hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **Docker CLI** -tooset din lokala dator som en Docker-värden och åtkomst hello Docker CLI-kommandon, installera [Docker-motorn](https://docs.docker.com/engine/installation/).

## <a name="log-in-tooa-registry"></a>Logga in tooa registret
Kör `docker login` toolog i tooyour behållare registret med din [registret autentiseringsuppgifter](container-registry-authentication.md).

hello följande exempel skickar hello-ID och lösenord för ett Azure Active Directory [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md). Du kan till exempel har tilldelat en service principal tooyour registret för ett automation-scenario.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Kontrollera att toospecify hello registret fullständigt kvalificerade namn (gemener). I det här exemplet är det `myregistry.azurecr.io`.

## <a name="steps-toopull-and-push-an-image"></a>Steg toopull och push en avbildning
hello följer exempel hämtningar hello Nginx-avbildning från hello offentliga Docker hubb registret taggar för privat Azure-behållaren registret, skickar den tooyour registret, därefter hämtar den igen.

**1. Hämta hello Docker officiella bild för Nginx**

Första pull hello offentliga Nginx bild tooyour lokal dator.

```
docker pull nginx
```
**2. Starta hello Nginx-behållaren**

hello följande kommando startar hello lokala Nginx-behållaren interaktivt på port 8080, så att du toosee utdata från Nginx. Det tar bort hello kör behållaren en gång stoppades.

```
docker run -it --rm -p 8080:80 nginx
```

Bläddra för[http://localhost: 8080](http://localhost:8080) tooview hello kör behållare. Du kan se en skärm liknande toohello efter.

![Nginx på lokal dator](./media/container-registry-get-started-docker-cli/nginx.png)

toostop hello körs behållare genom att trycka på [CTRL] + [C].

**3. Skapa ett alias till hello bild i registret**

hello följande kommando skapar ett alias till hello bild med ett fullständigt kvalificerad sökväg tooyour register. Det här exemplet anger hello `samples` namnområde tooavoid oreda i hello rot hello registret.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

**4. Push hello avbildningen tooyour registret**

```
docker push myregistry.azurecr.io/samples/nginx
```

**5. Pull hello-avbildning från registret**

```
docker pull myregistry.azurecr.io/samples/nginx
```

**6. Starta hello Nginx-behållaren från registret**

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Bläddra för[http://localhost: 8080](http://localhost:8080) tooview hello kör behållare.

toostop hello körs behållare genom att trycka på [CTRL] + [C].

**7. (Valfritt) Ta bort hello-avbildningar**

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a>Gränser för samtidiga anrop
I vissa situationer kan anrop som körs samtidigt resultera i fel. hello följande tabell innehåller hello gränserna för samtidiga anrop med ”Push” och ”Pull” åtgärder på Azure-behållaren registret:

| Åtgärd  | Gräns                                  |
| ---------- | -------------------------------------- |
| PULL       | Konfigurera samtidiga too10 hämtar per registret |
| PUSH       | Konfigurera too5 samtidiga push-meddelanden per registret |

## <a name="next-steps"></a>Nästa steg
Nu när du vet hello grunderna är klar toostart med hjälp av registret! Till exempel börja distribuera behållaren bilder tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) klustret.
