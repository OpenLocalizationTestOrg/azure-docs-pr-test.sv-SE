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
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Konfigurera WinRM-åtkomst för virtuella datorer i Azure Resource Manager
## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM i Azure Service Management vs Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

* En översikt över hello Azure Resource Manager finns [artikel](../../azure-resource-manager/resource-group-overview.md)
* Skillnader mellan Azure Service Management och Azure Resource Manager finns det [artikel](../../resource-manager-deployment-model.md)

hello är viktiga skillnader i hur du konfigurerar WinRM-konfigurationen mellan två hello-stackar hur hello certifikat installeras på hello VM. I hello Azure Resource Manager-stacken modelleras hello certifikat som resurser som hanteras av hello Key Vault-Resursprovidern. Därför hello användaren måste tooprovide sina egna certifikat och överföra tooa Key Vault innan den används i en virtuell dator.

Här följer hello stegen tootake tooset upp en virtuell dator med WinRM-anslutningen

1. Skapa en Key Vault-lösning
2. Skapa ett självsignerat certifikat
3. Ladda upp din självsignerat certifikat tooKey valvet
4. Hämta hello URL för ditt självsignerade certifikat i hello Key Vault
5. Referera till URL: en självsignerade certifikat när du skapar en virtuell dator

## <a name="step-1-create-a-key-vault"></a>Steg 1: Skapa en Key Vault
Du kan använda hello nedan kommandot toocreate hello Key Vault

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Steg 2: Skapa ett självsignerat certifikat
Du kan skapa ett självsignerat certifikat med hjälp av PowerShell-skript

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter hello certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-toohello-key-vault"></a>Steg 3: Överför din självsignerat certifikat toohello Key Vault
Innan du laddar upp hello certifikat toohello Key Vault skapade i steg 1, måste tooconverted i ett format hello Microsoft.Compute-resursprovidern kan förstå. hello PowerShell-skriptet nedan kan du göra det

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

## <a name="step-4-get-hello-url-for-your-self-signed-certificate-in-hello-key-vault"></a>Steg 4: Hämta hello URL för ditt självsignerade certifikat i hello Key Vault
Hej Microsoft.Compute-resursprovidern måste en URL toohello hemlighet i hello Key Vault vid etablering hello VM. Detta aktiverar hello Microsoft.Compute resource provider toodownload hello hemlighet och skapa hello motsvarande certifikat på hello VM.

> [!NOTE]
> hello-URL för hello hemlighet måste tooinclude hello versionen också. En exempel-URL som ser ut som nedan https://contosovault.vault.azure.net:443/hemligheter/contososecret/01h9db0df2cd4300a20ence585a6s7ve
> 
> 

#### <a name="templates"></a>Mallar
Du kan hämta hello länken toohello URL i hello mallen med hjälp av hello under

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
Du kan hämta den här URL-Adressen med hello nedan PowerShell-kommando

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Steg 5: Referera självsignerade certifikat-URL när du skapar en virtuell dator
#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-mallar
När du skapar en virtuell dator via mallar, hämtar hello certifikatet refereras i hello hemligheter och hello winRM avsnitt enligt nedan:

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

En exempelmall för hello ovan finns här på [201 vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Källkoden för den här mallen finns på [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell
    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-toohello-vm"></a>Steg 6: Ansluta toohello VM
Innan du kan ansluta toohello VM som du behöver toomake att datorn är konfigurerad för fjärrhantering med WinRM. Starta PowerShell som administratör och kör hello nedan kommandot toomake att du har konfigurerat.

    Enable-PSRemoting -Force

> [!NOTE]
> Du kan behöva toomake till hello WinRM-tjänsten körs om hello ovan inte fungerar. Du kan göra det med`Get-Service WinRM`
> 
> 

När hello installationen är klar kan du ansluta toohello VM med hjälp av hello nedan kommando

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
