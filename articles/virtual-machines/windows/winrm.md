---
title: "aaaSet konfigurerar WinRM-åtkomst för en virtuell dator i Azure | Microsoft Docs"
description: "Konfigurera WinRM-åtkomst för användning med en Azure-dator som skapats i hello Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9718e85b-d360-4621-90b8-0b0b84a21208
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/16/2016
ms.author: kasing
ms.openlocfilehash: 23d1d3a3065cbd8e4036be085c6d835cae36caae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="3ca5a-103">Konfigurera WinRM-åtkomst för virtuella datorer i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3ca5a-103">Setting up WinRM access for Virtual Machines in Azure Resource Manager</span></span>
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a><span data-ttu-id="3ca5a-104">WinRM i Azure Service Management vs Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3ca5a-104">WinRM in Azure Service Management vs Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* <span data-ttu-id="3ca5a-105">En översikt över hello Azure Resource Manager finns [artikel](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="3ca5a-105">For an overview of hello Azure Resource Manager, please see this [article](../../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="3ca5a-106">Skillnader mellan Azure Service Management och Azure Resource Manager finns det [artikel](../../resource-manager-deployment-model.md)</span><span class="sxs-lookup"><span data-stu-id="3ca5a-106">For differences between Azure Service Management and Azure Resource Manager, please see this [article](../../resource-manager-deployment-model.md)</span></span>

<span data-ttu-id="3ca5a-107">hello är viktiga skillnader i hur du konfigurerar WinRM-konfigurationen mellan två hello-stackar hur hello certifikat installeras på hello VM.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-107">hello key difference in setting up WinRM configuration between hello two stacks is how hello certificate gets installed on hello VM.</span></span> <span data-ttu-id="3ca5a-108">I hello Azure Resource Manager-stacken modelleras hello certifikat som resurser som hanteras av hello Key Vault-Resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-108">In hello Azure Resource Manager stack, hello certificates are modeled as resources managed by hello Key Vault Resource Provider.</span></span> <span data-ttu-id="3ca5a-109">Därför hello användaren måste tooprovide sina egna certifikat och överföra tooa Key Vault innan den används i en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-109">Therefore, hello user needs tooprovide their own certificate and upload it tooa Key Vault before using it in a VM.</span></span>

<span data-ttu-id="3ca5a-110">Här följer hello stegen tootake tooset upp en virtuell dator med WinRM-anslutningen</span><span class="sxs-lookup"><span data-stu-id="3ca5a-110">Here are hello steps you need tootake tooset up a VM with WinRM connectivity</span></span>

1. <span data-ttu-id="3ca5a-111">Skapa en Key Vault-lösning</span><span class="sxs-lookup"><span data-stu-id="3ca5a-111">Create a Key Vault</span></span>
2. <span data-ttu-id="3ca5a-112">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="3ca5a-112">Create a self-signed certificate</span></span>
3. <span data-ttu-id="3ca5a-113">Ladda upp din självsignerat certifikat tooKey valvet</span><span class="sxs-lookup"><span data-stu-id="3ca5a-113">Upload your self-signed certificate tooKey Vault</span></span>
4. <span data-ttu-id="3ca5a-114">Hämta hello URL för ditt självsignerade certifikat i hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="3ca5a-114">Get hello URL for your self-signed certificate in hello Key Vault</span></span>
5. <span data-ttu-id="3ca5a-115">Referera till URL: en självsignerade certifikat när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3ca5a-115">Reference your self-signed certificates URL while creating a VM</span></span>

## <a name="step-1-create-a-key-vault"></a><span data-ttu-id="3ca5a-116">Steg 1: Skapa en Key Vault</span><span class="sxs-lookup"><span data-stu-id="3ca5a-116">Step 1: Create a Key Vault</span></span>
<span data-ttu-id="3ca5a-117">Du kan använda hello nedan kommandot toocreate hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="3ca5a-117">You can use hello below command toocreate hello Key Vault</span></span>

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a><span data-ttu-id="3ca5a-118">Steg 2: Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="3ca5a-118">Step 2: Create a self-signed certificate</span></span>
<span data-ttu-id="3ca5a-119">Du kan skapa ett självsignerat certifikat med hjälp av PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="3ca5a-119">You can create a self-signed certificate using this PowerShell script</span></span>

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter hello certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-toohello-key-vault"></a><span data-ttu-id="3ca5a-120">Steg 3: Överför din självsignerat certifikat toohello Key Vault</span><span class="sxs-lookup"><span data-stu-id="3ca5a-120">Step 3: Upload your self-signed certificate toohello Key Vault</span></span>
<span data-ttu-id="3ca5a-121">Innan du laddar upp hello certifikat toohello Key Vault skapade i steg 1, måste tooconverted i ett format hello Microsoft.Compute-resursprovidern kan förstå.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-121">Before uploading hello certificate toohello Key Vault created in step 1, it needs tooconverted into a format hello Microsoft.Compute resource provider will understand.</span></span> <span data-ttu-id="3ca5a-122">hello PowerShell-skriptet nedan kan du göra det</span><span class="sxs-lookup"><span data-stu-id="3ca5a-122">hello below PowerShell script will allow you do that</span></span>

```
$fileName = "<Path toohello .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-hello-url-for-your-self-signed-certificate-in-hello-key-vault"></a><span data-ttu-id="3ca5a-123">Steg 4: Hämta hello URL för ditt självsignerade certifikat i hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="3ca5a-123">Step 4: Get hello URL for your self-signed certificate in hello Key Vault</span></span>
<span data-ttu-id="3ca5a-124">Hej Microsoft.Compute-resursprovidern måste en URL toohello hemlighet i hello Key Vault vid etablering hello VM.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-124">hello Microsoft.Compute resource provider needs a URL toohello secret inside hello Key Vault while provisioning hello VM.</span></span> <span data-ttu-id="3ca5a-125">Detta aktiverar hello Microsoft.Compute resource provider toodownload hello hemlighet och skapa hello motsvarande certifikat på hello VM.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-125">This enables hello Microsoft.Compute resource provider toodownload hello secret and create hello equivalent certificate on hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="3ca5a-126">hello-URL för hello hemlighet måste tooinclude hello versionen också.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-126">hello URL of hello secret needs tooinclude hello version as well.</span></span> <span data-ttu-id="3ca5a-127">En exempel-URL som ser ut som nedan https://contosovault.vault.azure.net:443/hemligheter/contososecret/01h9db0df2cd4300a20ence585a6s7ve</span><span class="sxs-lookup"><span data-stu-id="3ca5a-127">An example URL looks like below https://contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve</span></span>
> 
> 

#### <a name="templates"></a><span data-ttu-id="3ca5a-128">Mallar</span><span class="sxs-lookup"><span data-stu-id="3ca5a-128">Templates</span></span>
<span data-ttu-id="3ca5a-129">Du kan hämta hello länken toohello URL i hello mallen med hjälp av hello under</span><span class="sxs-lookup"><span data-stu-id="3ca5a-129">You can get hello link toohello URL in hello template using hello below code</span></span>

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a><span data-ttu-id="3ca5a-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3ca5a-130">PowerShell</span></span>
<span data-ttu-id="3ca5a-131">Du kan hämta den här URL-Adressen med hello nedan PowerShell-kommando</span><span class="sxs-lookup"><span data-stu-id="3ca5a-131">You can get this URL using hello below PowerShell command</span></span>

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a><span data-ttu-id="3ca5a-132">Steg 5: Referera självsignerade certifikat-URL när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="3ca5a-132">Step 5: Reference your self-signed certificates URL while creating a VM</span></span>
#### <a name="azure-resource-manager-templates"></a><span data-ttu-id="3ca5a-133">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="3ca5a-133">Azure Resource Manager Templates</span></span>
<span data-ttu-id="3ca5a-134">När du skapar en virtuell dator via mallar, hämtar hello certifikatet refereras i hello hemligheter och hello winRM avsnitt enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="3ca5a-134">While creating a VM through templates, hello certificate gets referenced in hello secrets section and hello winRM section as below:</span></span>

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of hello Key Vault containing hello secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for hello certificate you got in Step 4>",
                  "certificateStore": "<Name of hello certificate store on hello VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for hello certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

<span data-ttu-id="3ca5a-135">En exempelmall för hello ovan finns här på [201 vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="3ca5a-135">A sample template for hello above can be found here at [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)</span></span>

<span data-ttu-id="3ca5a-136">Källkoden för den här mallen finns på [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span><span class="sxs-lookup"><span data-stu-id="3ca5a-136">Source code for this template can be found on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)</span></span>

#### <a name="powershell"></a><span data-ttu-id="3ca5a-137">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3ca5a-137">PowerShell</span></span>
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-toohello-vm"></a><span data-ttu-id="3ca5a-138">Steg 6: Ansluta toohello VM</span><span class="sxs-lookup"><span data-stu-id="3ca5a-138">Step 6: Connecting toohello VM</span></span>
<span data-ttu-id="3ca5a-139">Innan du kan ansluta toohello VM som du behöver toomake att datorn är konfigurerad för fjärrhantering med WinRM.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-139">Before you can connect toohello VM you'll need toomake sure your machine is configured for WinRM remote management.</span></span> <span data-ttu-id="3ca5a-140">Starta PowerShell som administratör och kör hello nedan kommandot toomake att du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-140">Start PowerShell as an administrator and execute hello below command toomake sure you're set up.</span></span>

    Enable-PSRemoting -Force

> [!NOTE]
> <span data-ttu-id="3ca5a-141">Du kan behöva toomake till hello WinRM-tjänsten körs om hello ovan inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="3ca5a-141">You might need toomake sure hello WinRM service is running if hello above does not work.</span></span> <span data-ttu-id="3ca5a-142">Du kan göra det med`Get-Service WinRM`</span><span class="sxs-lookup"><span data-stu-id="3ca5a-142">You can do that using `Get-Service WinRM`</span></span>
> 
> 

<span data-ttu-id="3ca5a-143">När hello installationen är klar kan du ansluta toohello VM med hjälp av hello nedan kommando</span><span class="sxs-lookup"><span data-stu-id="3ca5a-143">Once hello setup is done, you can connect toohello VM using hello below command</span></span>

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
