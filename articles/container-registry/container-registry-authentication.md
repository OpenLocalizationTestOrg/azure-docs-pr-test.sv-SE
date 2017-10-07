---
title: "aaaAuthenticate med en Azure-behållaren register | Microsoft Docs"
description: "Hur toolog i tooan Azure-behållaren registret med hjälp av en Azure Active Directory service principal eller ett administratörskonto"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0b0462e8432b2567689debca322e2426baa7fa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Autentisera med ett privat Docker behållare register
toowork med behållaren bilder i en Azure-behållaren registret du logga in med hello `docker login` kommando. Du kan logga in med antingen en  **[Azure Active Directory-tjänstens huvudnamn](../active-directory/active-directory-application-objects.md)**  eller registret-specifika **administratörskonto**. Den här artikeln innehåller mer information om dessa identiteter.



## <a name="service-principal"></a>Tjänstens huvudnamn

Du kan [tilldela ett huvudnamn för tjänsten](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registret och använda den för grundläggande Docker-autentisering. Du bör använda ett huvudnamn för tjänsten för de flesta scenarier. Ange hello app-ID och lösenord för hello service principal toohello `docker login` kommandot som visas i följande exempel hello:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

När du loggade in cachelagrar Docker hello autentiseringsuppgifter, så du inte behöver tooremember hello app-ID.

> [!TIP]
> Om du vill kan du återskapa hello lösenordet för ett huvudnamn för tjänsten genom att köra hello `az ad sp reset-credentials` kommando.
>


Tjänstens huvudnamn Tillåt [rollbaserad åtkomst](../active-directory/role-based-access-control-configure.md) tooa registret. Tillgängliga roller är:
  * Läsare (pull endast åtkomst).
  * Deltagare (pull och push).
  * Ägaren (pull-, push- och tilldela roller tooother användare).

Anonym åtkomst är inte tillgänglig på Azure-behållare register. Du kan använda för offentliga bilder [Docker-hubb](https://docs.docker.com/docker-hub/).

Du kan tilldela flera tjänstens huvudnamn tooa registret, vilket innebär att du toodefine åtkomst för olika användare eller program. Tjänstens huvudnamn också aktivera ”fjärradministrerade” anslutning tooa registret i utvecklare eller DevOps scenarier, till exempel hello följande exempel:

  * Distribution i behållare från en DC/OS, Docker Swarm och Kubernetes i registret tooorchestration system. Du kan också dra behållaren register toorelated Azure-tjänster som [Behållartjänsten](../container-service/index.yml), [Apptjänst](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), med mera.

  * Kontinuerlig integrering och distribution lösningar (till exempel Visual Studio Team Services eller Jenkins) som bygger avbildningar för behållaren och push-installera dem tooa registret.





## <a name="admin-account"></a>Administratörskonto
Med varje register som du skapar skapas ett administratörskonto automatiskt. Hello-kontot är inaktiverat som standard, men du kan aktivera och hantera hello autentiseringsuppgifter, till exempel via hello [portal](container-registry-get-started-portal.md#manage-registry-settings) eller med hjälp av hello [Azure CLI 2.0 kommandon](container-registry-get-started-azure-cli.md#manage-admin-credentials). Varje administratörskontot tillhandahålls med två lösenord som genereras. hello två lösenord kan du toomaintain anslutningar toohello registret med hjälp av ett lösenord när du återskapar hello andra lösenord. Om hello-kontot är aktiverat du överföra hello användarnamn och antingen lösenord toohello `docker login` kommandot för grundläggande autentisering toohello registret. Exempel:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> hello administratörskonto är utformad för en enskild användare tooaccess hello registret, främst för testning. Det rekommenderas inte tooshare hello administratörsautentiseringsuppgifter konto bland andra användare. Alla användare som visas som en enskild användare toohello registret. Ändra eller inaktivera det här kontot inaktiverar du registret åtkomst för alla användare som använder hello autentiseringsuppgifter.
>


### <a name="next-steps"></a>Nästa steg
* [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md).
* Mer information om autentisering i hello behållaren registret preview finns hello [blogginlägget](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
