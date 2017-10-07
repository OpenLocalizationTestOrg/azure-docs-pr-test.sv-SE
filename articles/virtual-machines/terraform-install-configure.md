---
title: aaaInstall och konfigurera Terraform tooprovision virtuella datorer och annan infrastruktur i Azure | Microsoft Docs
description: "Lär dig hur tooinstall och konfigurera Terraform toocreate Azure resurser"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 803f51a6f5357417b96264ba713791408f9935b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-terraform-tooprovision-vms-and-other-infrastructure-into-azure"></a>Installera och konfigurera Terraform tooprovision virtuella datorer och annan infrastruktur till Azure 
Den här artikeln beskriver hello nödvändiga åtgärder tooinstall och konfigurera Terraform tooprovision resurser, t.ex virtuella datorer i Azure. Får du lära dig hur toocreate och använda Azure autentiseringsuppgifter tooenable Terraform tooprovision molnresurser att på ett säkert sätt.

HashiCorp Terraform ger ett enkelt sätt toodefine och distribuera moln-infrastruktur med hjälp av en anpassad templating språk som kallas HashiCorp configuration språk (LISTAN). Den här anpassade språket är [enkelt toowrite och enkelt toounderstand](terraform-create-complete-vm.md). Dessutom med hello `terraform plan` kommandot kan du visualisera hello ändringar tooyour infrastruktur innan du utför dem. Följ dessa steg toostart Terraform med Azure.

## <a name="install-terraform"></a>Installera Terraform
tooinstall Terraform, [hämta](https://www.terraform.io/downloads.html) hello-paket som är lämpliga för ditt operativsystem i en separat installationskatalogen. hello innehåller en enda körbar fil som bör du också definiera en global sökväg. För instruktioner om hur tooset hello sökväg för Linux och Mac, gå för[den här webbsidan](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). För instruktioner om hur tooset hello sökväg på Windows, gå för[den här webbsidan](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). tooverify installationen kör hello `terraform` kommando. Du bör se en lista över tillgängliga alternativ för Terraform som utdata.

Du måste sedan tooallow Terraform åtkomst tooyour Azure-prenumeration tooperform infrastruktur etablering.

## <a name="set-up-terraform-access-tooazure"></a>Ställ in Terraform åtkomst tooAzure
tooenable Terraform tooprovision resurser i Azure, behöver du toocreate två entiteter i Azure Active Directory (AD Azure): en Azure AD-program och en Azure AD-tjänstens huvudnamn. Sedan kan du använda dessa enheter identifierare i Terraform skript. Ett huvudnamn för tjänsten är en lokal instans av en global Azure AD-program. Ett huvudnamn för tjänsten kan detaljerade lokal åtkomstkontroll tooglobal resurser.

Det finns flera sätt toocreate en Azure AD-program och en Azure AD-tjänstens huvudnamn. hello enklaste och snabbaste vägen idag är toouse Azure CLI 2.0 som [du kan hämta och installera på Windows, Linux eller en Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Du kan också använda PowerShell eller Azure CLI 1.0 toocreate hello nödvändiga säkerhetsinfrastruktur. hello-instruktionerna som följer visar hur tooconfigure Terraform för Azure med hjälp av alla dessa metoder.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Använda Azure CLI 2.0 (för Windows, Linux och Mac-användare) 
När du hämtar och installerar hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), logga in tooadminister din Azure-prenumeration genom att utfärda hello följande kommando:

```
az login
```

>[!NOTE]
>Om du använder hello Kina, Azure Tyskland eller Azure Government-moln, måste toofirst konfigurera hello Azure CLI toowork med det molnet. Du kan göra detta genom att köra hello följande:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Om du har flera Azure-prenumerationer, deras information returneras av hello `az login` kommando. Ange hello `SUBSCRIPTION_ID` returneras miljö variabeln toohold hello värdet för hello `id` från hello prenumeration du vill toouse. 

Ange hello prenumeration som du vill toouse för den här sessionen.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Fråga hello konto tooget hello prenumerations-ID och klient-ID-värden.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Därefter måste du skapa separata autentiseringsuppgifter för Terraform.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

AppId, lösenordet, sp_name och klient returneras. Anteckna hello appId och lösenord.

tooconfirm dina autentiseringsuppgifter (tjänstens huvudnamn), öppna ett nytt gränssnitt och kör följande kommandon hello. Ersätt hello returnerade värden för sp_name, lösenord och klientorganisations:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>Använd PowerShell (för Windows-användare) 
toouse Windows datorn toowrite och kör din Terraform skript och toouse PowerShell för konfigurationsåtgärder konfigurera din dator med hello rätt PowerShell-verktyg. 

1. Installera PowerShell verktygen genom att följa stegen hello i [installera och konfigurera Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. Hämta och köra hello [azure setup.ps1 skriptet](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) från hello PowerShell-konsolen.

3. toorun hello azure setup.ps1 skript, hämta och köra hello `./azure-setup.ps1 setup` från hello PowerShell-konsolen. Logga sedan in tooyour Azure-prenumeration med administratörsbehörighet.

4. Ange ett programnamn (godtycklig sträng, krävs) när du tillfrågas. Du kan också ange ett starkt lösenord när du uppmanas. Om du inte anger ett lösenord, genereras ett starkt lösenord med hjälp av .NET-bibliotek för säkerhet.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Använda Azure CLI 1.0 (för Linux- eller Mac-användare)
tooget igång med Terraform på Linux-datorer eller Mac-datorer med Azure CLI 1.0, installera hello rätt bibliotek på din dator.  

1. Installera Azure xPlat CLI-verktygen genom att följa stegen hello i [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

2. Hämta och installera en JSON-processor genom att följa instruktionerna hello i [hämta jq](https://stedolan.github.io/jq/download/).

3. Hämta och köra hello [azure setup.sh skriptet](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash-skript från hello-konsolen.

4. toorun hello azure setup.sh skript, hämta och köra hello `./azure-setup setup` från hello-konsolen. Logga sedan in tooyour Azure-prenumeration med administratörsbehörighet.
 
5. Ange ett programnamn (godtycklig sträng, krävs) när du tillfrågas. Du kan också ange ett starkt lösenord när du uppmanas. Om du inte anger ett lösenord, genereras ett starkt lösenord med hjälp av .NET-bibliotek för säkerhet.

Alla hello föregående skript skapa en Azure AD-program och tjänstens huvudnamn. hello tjänstens huvudnamn hämtar deltagare eller ägare behörighet på hello prenumeration. På grund av hello hög nivå av åtkomst, bör du alltid skydda hello säkerhetsinformation som genereras av dessa skript. Anteckna alla fyra typer av säkerhetsinformation som tillhandahålls av dessa skript: appId, lösenord och PRENUMERATIONSID tenant_id.

## <a name="set-environment-variables"></a>Uppsättning miljövariabler
När du skapar och konfigurerar en Azure AD-tjänstens huvudnamn måste toolet Terraform vet hello klient-ID, prenumerations-ID, klient-ID och klientens hemliga toouse. Du kan göra det genom att bädda in dessa värden i skripten Terraform, enligt beskrivningen i [skapa grundläggande infrastruktur med hjälp av Terraform](terraform-create-complete-vm.md). Alternativt kan kan du ange hello följande miljövariabler (och därmed undvika incheckning av misstag eller dela dina autentiseringsuppgifter):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Du kan använda det här exemplet shell-skript tooset dessa variabler:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Dessutom, om du använder Terraform med Azure i Kina eller antingen Azure Government eller Azure Tyskland måste tooset hello miljövariabeln korrekt.

## <a name="next-steps"></a>Nästa steg
Du har nu installerats Terraform och konfigurerat Azure autentiseringsuppgifter så att du kan starta distribution av infrastrukturen i din Azure-prenumeration. Lär dig sedan hur för[skapa infrastruktur med Terraform](terraform-create-complete-vm.md).
