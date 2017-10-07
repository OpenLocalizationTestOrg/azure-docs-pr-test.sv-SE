---
title: "aaaAzure behållaren registret webhooks | Microsoft Docs"
description: "Azure-behållaren registret webhooks"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: "Docker behållare ACR"
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.openlocfilehash: adc2afec486007e2d54cd689e6f7ef8b1098db06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a>Med hjälp av Azure-behållare registret webhooks - Azure-portalen

En Azure-behållaren registret lagrar och hanterar privata Docker behållare bilder, liknande toohello sätt Docker hubb lagrar offentliga Docker-avbildningar. Du kan använda webhooks tootrigger händelser när vissa åtgärder utföras i en av dina databaser i registret. Webhooks kan svara tooevents på hello registret nivå eller vara begränsad ned tooa databas taggen. 

Mer bakgrund och begrepp finns hello [översikt](./container-registry-intro.md).

## <a name="prerequisites"></a>Krav 

- Azure-behållaren hanteras registret - skapa en hanterad behållaren registret i din Azure-prenumeration. Till exempel använda hello Azure-portalen eller hello Azure CLI 2.0. 
- Docker CLI - tooset din lokala dator som en Docker-värden och åtkomst hello Docker CLI-kommandon, installera Docker-motorn. 

## <a name="create-webhook-azure-portal"></a>Skapa webhook Azure-portalen

1. Logga in toohello Azure-portalen och navigera toohello registret som du vill toocreate webhooks. 

2. Välj hello ”Webhooks” flik i hello behållaren bladet. 

3. Välj ”Lägg till” Hej webhook bladet verktygsfältet. 

4. Fullständig hello *skapa Webhook* formulär med hello följande information:

| Värde | Beskrivning |
|---|---|
| Namn | hello-namn som du vill toogive toohello webhooken. Det kan bara innehålla gemena bokstäver och siffror och mellan 5 och 50 tecken. |
| URI för tjänsten | hello URI där hello webhook ska skicka efter meddelanden. |
| Egna rubriker | Huvuden som du vill använda toopass tillsammans med hello POST-begäran. De måste vara i ”nyckel: värde” format. |
| Utlösaråtgärder | Åtgärder som utlöser hello webhooken. För närvarande webhooks kan utlösas av push och/eller ta bort åtgärder tooan avbildningen. |
| Status | hello status för hello webhook när den har skapats. Den är aktiverad som standard. |
| Omfång | hello scope på som hello webhook fungerar. Som standard är hello omfång för alla händelser i hello registret. Det kan anges för en databas eller en tagg hello formatet ”databas: taggen”. |

Exempelformulär för webhook:

![DC/OS-GRÄNSSNITTET](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a>Skapa webhook Azure CLI

en webhook med hjälp av toocreate hello Azure CLI, Använd hello [az acr webhook skapa](/cli/azure/acr/webhook#create) kommando.

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Test-webhook

### <a name="azure-portal"></a>Azure Portal

Tidigare toousing hello webhook behållaren image push- och borttagningsåtgärder, kan testas med hello **Ping** knappen. När du skickar hello Ping toohello en allmän post-begäran anges slutpunkt och loggar hello svar. Det här är användbart tooverify som hello webhook har ställts in korrekt.

1. Välj hello webhook som du vill tootest. 
2. Välj hello ”pinga” åtgärd i hello översta verktygsfältet. 
3. Kontrollera hello förfrågan och svar.

### <a name="azure-cli"></a>Azure CLI

tootest en ACR webhook med hello Azure CLI, använder hello [az acr webhook ping](/cli/azure/acr/webhook#ping) kommando.

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

toosee hello resultat kan du använda hello [az acr webhook lista-händelser](/cli/azure/acr/webhook#list-events) kommando. 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Ta bort webhooken

### <a name="azure-portal"></a>Azure Portal

Varje webhook kan tas bort genom att välja hello webhook och hello ta borttagningsknappen hello Azure-portalen.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```