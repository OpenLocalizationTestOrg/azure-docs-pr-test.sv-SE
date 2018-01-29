---
title: "Autentisera med Azure-behållaren registret från Azure-Behållarinstanser"
description: "Lär dig att ge åtkomst till bilder i registret privata behållare från Azure-Behållarinstanser med hjälp av en Azure Active Directory-tjänstens huvudnamn."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 00d9632a5d0c42eceee1b412f8963bbadbea651f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Autentisera med Azure-behållaren registret från Azure-Behållarinstanser

Du kan använda en Azure Active Directory (AD Azure) tjänstens huvudnamn för att ge åtkomst till ditt privata behållare register i registret för Azure-behållare.

I den här artikeln får du lära dig att skapa och konfigurera en Azure AD-tjänstens huvudnamn med *pull* behörigheter i registret. Sedan kan starta du en behållare i Azure Container instanser (ACI) som tar emot dess avbildning från din privata registret med tjänstens huvudnamn för autentisering.

## <a name="when-to-use-a-service-principal"></a>När du ska använda ett huvudnamn för tjänsten

Du bör använda ett huvudnamn för tjänsten för autentisering från ACI i **fjärradministrerade scenarier**, till exempel program eller tjänster som skapar behållarinstanser i automatisk eller på annat sätt oövervakat.

Till exempel om du har ett automatiserat skript som körs varje natt och skapar en [uppgiftsbaserade behållaren instans](../container-instances/container-instances-restart-policy.md) för att bearbeta vissa data kan användas för ett huvudnamn för tjänsten med pull (läsare) behörighet för att autentisera till registret. Du kan rotera tjänstens huvudnamn autentiseringsuppgifter eller återkalla helt sin åtkomst utan att påverka andra tjänster och program.

Tjänstens huvudnamn bör också vara används när registret [administratörsanvändare](container-registry-authentication.md#admin-account) är inaktiverad.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Autentisera med tjänstens huvudnamn

Om du vill starta en behållare i Azure Container instanser som använder ett huvudnamn för tjänsten, ange dess ID för `--registry-username`, och lösenordet för `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="next-steps"></a>Nästa steg

I följande artiklar innehåller ytterligare information om hur du arbetar med tjänstens huvudnamn och ACR:

* [Azure Container registret autentisering med tjänstens huvudnamn](container-registry-auth-service-principal.md)
* [Autentisera med Azure-behållaren registret från Azure Container Service (AKS)](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->
