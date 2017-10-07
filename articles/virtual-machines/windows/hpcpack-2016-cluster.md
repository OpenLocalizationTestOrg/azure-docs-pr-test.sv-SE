---
title: aaaHPC Pack 2016 kluster i Azure | Microsoft Docs
description: "Lär dig hur toodeploy ett HPC Pack 2016 kluster i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3dde6a68-e4a6-4054-8b67-d6a90fdc5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/15/2016
ms.author: danlep
ms.openlocfilehash: 9e21ec70c822a783474b96da77ce925940279abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-hpc-pack-2016-cluster-in-azure"></a>Distribuera ett HPC Pack 2016-kluster i Azure

Följ hello stegen i den här artikeln toodeploy en [Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) kluster i Azure-datorer. HPC Pack är Microsofts ledigt HPC-lösning baserat på Microsoft Azure och Windows Server-teknik och stöder en bred HPC-arbetsbelastning.

Använd någon av hello [Azure Resource Manager-mallar](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 klustret. Du har flera val av klustertopologi med olika antal head klusternoder och antingen Linux eller Windows compute-noder.

## <a name="prerequisites"></a>Krav

### <a name="pfx-certificate"></a>PFX-certifikat

En Microsoft HPC Pack 2016 klustret kräver en Personal Information Exchange (PFX) certifikat toosecure hello kommunikation mellan hello HPC-noder. hello certifikat måste uppfylla följande krav hello:

* Den måste ha en privat nyckel kapacitet för nyckelutbyte
* Nyckelanvändning innehåller digitala signatur och nyckelchiffrering
* Utökad nyckelanvändning innehåller klientautentisering och serverautentisering

Om du inte redan har ett certifikat som uppfyller dessa krav, kan du begära hello certifikat från en certifikatutfärdare. Du kan också använda hello följande kommandon toogenerate hello självsignerat certifikat baserat på hello operativsystem där du kör hello kommando och exportera format hello PFX-certifikat med privat nyckel.

* **För Windows 10 eller Windows Server 2016**, kör hello inbyggda **ny SelfSignedCertificate** PowerShell-cmdlet enligt följande:

  ```PowerShell
  New-SelfSignedCertificate -Subject "CN=HPC Pack 2016 Communication" -KeySpec KeyExchange -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2") -CertStoreLocation cert:\CurrentUser\My -KeyExportPolicy Exportable -NotAfter (Get-Date).AddYears(5)
  ```
* **För operativsystem tidigare än Windows 10 eller Windows Server 2016**, hämta hello [självsignerat certifikat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) från hello Microsoft Script Center. Extrahera innehållet och kör följande kommandon i PowerShell-Kommandotolken hello:

    ```PowerShell 
    Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
  
    New-SelfSignedCertificateEx -Subject "CN=HPC Pack 2016 Communication" -KeySpec Exchange -KeyUsage "DigitalSignature,KeyEncipherment" -EnhancedKeyUsage "Server Authentication","Client Authentication" -StoreLocation CurrentUser -Exportable -NotAfter (Get-Date).AddYears(5)
    ```

### <a name="upload-certificate-tooan-azure-key-vault"></a>Ladda upp certifikatet tooan Azure key vault

Innan du distribuerar hello HPC-kluster, överför hello certifikat tooan [Azure key vault](../../key-vault/index.md) som en hemlig och registrera hello följande information för användning under distributionen av hello: **valvnamnet**,  **Valvet resursgruppen**, **Certifikatwebbadress**, och **certifikattumavtrycket**.

Ett exempel PowerShell-skriptet tooupload hello certifikat följer. Mer information om hur du överför ett certifikat tooan Azure key vault finns [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md).

```powershell
#Give hello following values
$VaultName = "mytestvault"
$SecretName = "hpcpfxcert"
$VaultRG = "myresourcegroup"
$location = "westus"
$PfxFile = "c:\Temp\mytest.pfx"
$Password = "yourpfxkeyprotectionpassword"
#Validate hello pfx file
try {
    $pfxCert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $PfxFile, $Password
}
catch [System.Management.Automation.MethodInvocationException]
{
    throw $_.Exception.InnerException
}
$thumbprint = $pfxCert.Thumbprint
$pfxCert.Dispose()
# Create and encode hello JSON object
$pfxContentBytes = Get-Content $PfxFile -Encoding Byte
$pfxContentEncoded = [System.Convert]::ToBase64String($pfxContentBytes)
$jsonObject = @"
{
"data": "$pfxContentEncoded",
"dataType": "pfx",
"password": "$Password"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
#Create an Azure key vault and upload hello certificate as a secret
$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
$rg = Get-AzureRmResourceGroup -Name $VaultRG -Location $location -ErrorAction SilentlyContinue
if($null -eq $rg)
{
    $rg = New-AzureRmResourceGroup -Name $VaultRG -Location $location
}
$hpcKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $VaultRG -Location $location -EnabledForDeployment -EnabledForTemplateDeployment
$hpcSecret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secret
"hello following Information will be used in hello deployment template"
"Vault Name             :   $VaultName"
"Vault Resource Group   :   $VaultRG"
"Certificate URL        :   $($hpcSecret.Id)"
"Certificate Thumbprint :   $thumbprint"

```


## <a name="supported-topologies"></a>Topologier som stöds

Välj något av hello [Azure Resource Manager-mallar](https://github.com/MsHpcPack/HPCPack2016) toodeploy hello HPC Pack 2016 klustret. Följande är med tre topologier som stöds för klustret. Hög tillgänglighet topologier innehåller flera head klusternoder.

1. Kluster med hög tillgänglighet med Active Directory-domän

    ![Hög tillgänglighet till klustret i AD-domänen](./media/hpcpack-2016-cluster/haad.png)



2. Kluster med hög tillgänglighet utan Active Directory-domän

    ![Kluster med hög tillgänglighet utan AD-domänen](./media/hpcpack-2016-cluster/hanoad.png)

3. Kluster med en enda huvudnod

   ![Kluster med enskild huvudnod](./media/hpcpack-2016-cluster/singlehn.png)


## <a name="deploy-a-cluster"></a>Distribuera ett kluster

toocreate hello kluster, Välj en mall och klicka på **distribuera tooAzure**. I hello [Azure-portalen](https://portal.azure.com), ange parametrar för hello mall enligt beskrivningen i hello följande steg. Varje mall skapas alla Azure-resurser som krävs för hello HPC-klusterinfrastruktur. Resurserna innehåller ett virtuellt Azure-nätverk, offentliga IP-adress, belastningsutjämnare (endast för ett kluster med hög tillgänglighet), nätverksgränssnitt, tillgänglighetsuppsättningar, lagringskonton och virtuella datorer.

### <a name="step-1-select-hello-subscription-location-and-resource-group"></a>Steg 1: Välj hello prenumeration, plats och resursgruppen.

Hej **prenumeration** och hello **plats** måste vara samma som du angav när du har överfört PFX-certifikat (se krav). Vi rekommenderar att du skapar en **resursgruppen** för hello-distribution.

### <a name="step-2-specify-hello-parameter-settings"></a>Steg 2: Ange hello parameterinställningarna

Ange eller ändra värden för hello mallparametrar. Klicka på hello ikonen nästa tooeach parameter för hjälpinformation. Se även hello vägledning för [tillgängliga storlekar på VM](sizes.md).

Ange hello-värden som du antecknade i hello förutsättningar för hello följande parametrar: **valvnamnet**, **valvet resursgruppen**, **Certifikatwebbadress**, och **Certifikattumavtrycket**.

### <a name="step-3-review-legal-terms-and-create"></a>Steg 3. Granska juridiska villkor och skapa
Klicka på **Granska juridiska villkor** tooreview hello villkoren. Om du godkänner villkoren klickar du på **inköp**, och klicka sedan på **skapa** toostart hello distribution.

## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster
1. När hello HPC Pack klustret distribueras går toohello [Azure-portalen](https://portal.azure.com). Klicka på **resursgrupper**, och hitta hello resursgruppen i vilka hello klustret har distribuerats. Du kan hitta hello huvudnod virtuella datorer.

    ![Klustrets huvudnoder hello-portalen](./media/hpcpack-2016-cluster/clusterhns.png)

2. Klicka på en huvudnod (i ett kluster med hög tillgänglighet, klicka på någon av hello huvudnoderna). I **Essentials**, hittar du hello offentlig IP-adress eller fullständigt DNS-namn i hello-klustret.

    ![Anslutningsinställningar för kluster](./media/hpcpack-2016-cluster/clusterconnect.png)

3. Klicka på **Anslut** toolog på tooany av hello huvudnoderna med hjälp av fjärrskrivbord med den angivna administratörsanvändarnamn. Om hello-klustret som du har distribuerat finns i en Active Directory-domän, hello användarnamnet är hello formatet <privateDomainName> \<adminUsername > (till exempel hpc.local\hpcadmin).

## <a name="next-steps"></a>Nästa steg
* Skicka jobb tooyour klustret. Se [skicka jobb tooHPC ett HPC Pack kluster i Azure](hpcpack-cluster-submit-jobs.md) och [hantera ett HPC Pack 2016-kluster i Azure med hjälp av Azure Active Directory](hpcpack-cluster-active-directory.md).

