---
title: "aaaAzure behållaren registret databaser | Microsoft Docs"
description: "Hur toouse Azure Container registret databaser för Docker bilder"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.openlocfilehash: 448fb812f537c9502041ce5fb372b0681a9dac4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-powershell"></a>Skapa en privat Docker behållare registret med hjälp av hello Azure PowerShell
Använda kommandon i [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate ett register för behållaren och hantera dess inställningar från Windows-datorn. Du kan också skapa och hantera behållare register med hello [Azure-portalen](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), eller programmässigt med hello behållaren registret [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Bakgrund och begrepp finns [hello översikt](container-registry-intro.md)
* En fullständig lista över cmdlets som stöds finns i [Azure Container registret Management-Cmdlets](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).


## <a name="prerequisites"></a>Krav
* **Azure PowerShell**: tooinstall och kom igång med Azure PowerShell, se hello [Installationsinstruktioner](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). Logga in tooyour Azure-prenumeration genom att köra `Login-AzureRMAccount`. Mer information finns i [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).
* **Resursgrupp**: Skapa en [resursgrupp](../azure-resource-manager/resource-group-overview.md#resource-groups) innan du skapar ett behållarregister eller använd en befintlig resursgrupp. Kontrollera att resursgruppen hello finns på en plats där hello behållaren Registry-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/). toocreate en resursgrupp med hjälp av Azure PowerShell finns [hello PowerShell-referens](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).
* **Lagringskontot** (valfritt): skapa en standard Azure [lagringskonto](../storage/common/storage-introduction.md) tooback hello behållaren registret i hello samma plats. Om du inte anger ett storage-konto när du skapar ett register med `New-AzureRMContainerRegistry`, hello kommando skapar en åt dig. toocreate en storage-konto med PowerShell, se [hello PowerShell-referens](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount). Premium-lagring stöds inte för närvarande.
* **Tjänstens huvudnamn** (valfritt): när du skapar ett register med PowerShell som standard den har inte konfigurerats för åtkomst. Beroende på dina behov kan du tilldela en befintlig Azure Active Directory service principal tooa registret eller skapa och tilldela en ny. Alternativt kan du aktivera hello registret admin-användarkonto. Avsnitten hello senare i den här artikeln. Läs mer om registeråtkomst [autentisera med hello behållaren registret](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Skapa ett behållarregister
Kör hello `New-AzureRMContainerRegistry` kommandot toocreate ett register för behållaren.

> [!TIP]
> När du skapar ett register anger du ett globalt unikt domännamn på den översta nivån, som endast innehåller bokstäver och siffror. hello registret i hello exempel heter `MyRegistry`, men i stället använda ett unikt namn för din egen.
>
>

Hej följande kommando använder hello minimal parametrar toocreate behållare registret `MyRegistry` i hello resursgruppen `MyResourceGroup` i hello södra centrala USA plats:

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* `-StorageAccountName` är valfritt. Om inte anges skapas ett lagringskonto med ett namn som består av hello registret namn och en tidsstämpel i hello angivna resursgruppen.

## <a name="assign-a-service-principal"></a>Tilldela ett tjänstobjekt
Använd PowerShell-kommandon tooassign ett Azure Active Directory [tjänstens huvudnamn](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registret. hello tjänstens huvudnamn i dessa exempel tilldelas hello ägarrollen, men du kan tilldela [andra roller](../active-directory/role-based-access-control-configure.md) om du vill.

### <a name="create-a-service-principal"></a>Skapa ett huvudnamn för tjänsten
I följande kommando hello, skapas ett nytt huvudnamn för tjänsten. Ange ett starkt lösenord med hello `-Password` parameter.

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a>Tilldela en ny eller befintlig tjänstens huvudnamn
Du kan tilldela en ny eller en befintlig service principal tooa registret. tooassign den ägare roll åtkomst toohello registret, kör ett kommando liknande toohello följande exempel:

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-toohello-registry-with-hello-service-principal"></a>Logga in toohello registret med hello tjänstens huvudnamn
När du tilldelar hello service principal toohello registret, kan du logga in med hello följande kommando:

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a>Hantera administratörsautentiseringsuppgifter
Ett administratörskonto skapas automatiskt för varje behållarregister och är inaktiverat som standard. hello följande exempel visar PowerShell-kommandon toomanage hello administratörsautentiseringsuppgifter för behållaren registret.

### <a name="obtain-admin-user-credentials"></a>Hämta administratörsautentiseringsuppgifter
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Aktivera en administratörsanvändare för ett befintligt register
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Inaktivera en administratörsanvändare för ett befintligt register
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a>Nästa steg
* [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md)
