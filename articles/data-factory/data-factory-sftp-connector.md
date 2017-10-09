---
title: "aaaMove data från SFTP-servern med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från en lokal eller en molnet SFTP-server med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jingwang
ms.openlocfilehash: b976289e2c1b1899634bb5565b375499077fa554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a><span data-ttu-id="a211c-103">Flytta data från en SFTP-server med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a211c-103">Move data from an SFTP server using Azure Data Factory</span></span>
<span data-ttu-id="a211c-104">Den här artikeln beskrivs hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en på lokalt/i molnet SFTP server tooa stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a211c-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises/cloud SFTP server tooa supported sink data store.</span></span> <span data-ttu-id="a211c-105">Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med kopiera aktivitet och hello lista över datakällor som stöds som källor/sänkor.</span><span class="sxs-lookup"><span data-stu-id="a211c-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="a211c-106">Data factory stöder för närvarande endast flytta data från ett SFTP server tooother lagrar, men inte för att flytta data från andra data lagras tooan SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-106">Data factory currently supports only moving data from an SFTP server tooother data stores, but not for moving data from other data stores tooan SFTP server.</span></span> <span data-ttu-id="a211c-107">Den stöder både lokalt och molntjänster SFTP-servrar.</span><span class="sxs-lookup"><span data-stu-id="a211c-107">It supports both on-premises and cloud SFTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="a211c-108">Kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="a211c-108">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="a211c-109">Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och använda hello aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a211c-109">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="a211c-110">Scenarier som stöds och typer av autentisering</span><span class="sxs-lookup"><span data-stu-id="a211c-110">Supported scenarios and authentication types</span></span>
<span data-ttu-id="a211c-111">Du kan använda den här SFTP connector toocopy data från **både molnet SFTP-servrar och lokala SFTP servrar**.</span><span class="sxs-lookup"><span data-stu-id="a211c-111">You can use this SFTP connector toocopy data from **both cloud SFTP servers and on-premises SFTP servers**.</span></span> <span data-ttu-id="a211c-112">**Grundläggande** och **SshPublicKey** autentiseringstyper som stöds när du ansluter toohello SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-112">**Basic** and **SshPublicKey** authentication types are supported when connecting toohello SFTP server.</span></span>

<span data-ttu-id="a211c-113">När du kopierar data från en lokal SFTP-server, måste du installera en Data Management Gateway i hello lokala miljö/Azure VM.</span><span class="sxs-lookup"><span data-stu-id="a211c-113">When copying data from an on-premises SFTP server, you need install a Data Management Gateway in hello on-premises environment/Azure VM.</span></span> <span data-ttu-id="a211c-114">Se [Data Management Gateway](data-factory-data-management-gateway.md) information om hello gateway.</span><span class="sxs-lookup"><span data-stu-id="a211c-114">See [Data Management Gateway](data-factory-data-management-gateway.md) for details on hello gateway.</span></span> <span data-ttu-id="a211c-115">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner för att ställa in hello gateway och använder den.</span><span class="sxs-lookup"><span data-stu-id="a211c-115">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway and using it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a211c-116">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a211c-116">Getting started</span></span>
<span data-ttu-id="a211c-117">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en SFTP-datakälla med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="a211c-117">You can create a pipeline with a copy activity that moves data from an SFTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="a211c-118">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a211c-118">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="a211c-119">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="a211c-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

- <span data-ttu-id="a211c-120">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a211c-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a211c-121">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a211c-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> <span data-ttu-id="a211c-122">JSON-exempel toocopy data från SFTP server tooAzure Blob Storage, finns [JSON-exempel: kopiera data från SFTP server tooAzure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a211c-122">For JSON samples toocopy data from SFTP server tooAzure Blob Storage, see [JSON Example: Copy data from SFTP server tooAzure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) section of this article.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a211c-123">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a211c-123">Linked service properties</span></span>
<span data-ttu-id="a211c-124">hello följande tabell innehåller en beskrivning för JSON-element specifika tooFTP länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="a211c-124">hello following table provides description for JSON elements specific tooFTP linked service.</span></span>

| <span data-ttu-id="a211c-125">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a211c-125">Property</span></span> | <span data-ttu-id="a211c-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a211c-126">Description</span></span> | <span data-ttu-id="a211c-127">Krävs</span><span class="sxs-lookup"><span data-stu-id="a211c-127">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a211c-128">typ</span><span class="sxs-lookup"><span data-stu-id="a211c-128">type</span></span> | <span data-ttu-id="a211c-129">hello Typegenskapen måste anges för`Sftp`.</span><span class="sxs-lookup"><span data-stu-id="a211c-129">hello type property must be set too`Sftp`.</span></span> |<span data-ttu-id="a211c-130">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-130">Yes</span></span> |
| <span data-ttu-id="a211c-131">värden</span><span class="sxs-lookup"><span data-stu-id="a211c-131">host</span></span> | <span data-ttu-id="a211c-132">Namn eller IP-adressen för hello SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-132">Name or IP address of hello SFTP server.</span></span> |<span data-ttu-id="a211c-133">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-133">Yes</span></span> |
| <span data-ttu-id="a211c-134">port</span><span class="sxs-lookup"><span data-stu-id="a211c-134">port</span></span> |<span data-ttu-id="a211c-135">Port på vilken hello SFTP servern lyssnar.</span><span class="sxs-lookup"><span data-stu-id="a211c-135">Port on which hello SFTP server is listening.</span></span> <span data-ttu-id="a211c-136">hello standardvärdet är: 21</span><span class="sxs-lookup"><span data-stu-id="a211c-136">hello default value is: 21</span></span> |<span data-ttu-id="a211c-137">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-137">No</span></span> |
| <span data-ttu-id="a211c-138">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a211c-138">authenticationType</span></span> |<span data-ttu-id="a211c-139">Ange autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="a211c-139">Specify authentication type.</span></span> <span data-ttu-id="a211c-140">Tillåtna värden: **grundläggande**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="a211c-140">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="a211c-141">Se för[använder grundläggande autentisering](#using-basic-authentication) och [med hjälp av SSH autentisering med offentlig nyckel](#using-ssh-public-key-authentication) respektive avsnitt på fler egenskaper och JSON-exempel.</span><span class="sxs-lookup"><span data-stu-id="a211c-141">Refer too[Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="a211c-142">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-142">Yes</span></span> |
| <span data-ttu-id="a211c-143">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="a211c-143">skipHostKeyValidation</span></span> | <span data-ttu-id="a211c-144">Ange om tooskip värd viktiga validering.</span><span class="sxs-lookup"><span data-stu-id="a211c-144">Specify whether tooskip host key validation.</span></span> | <span data-ttu-id="a211c-145">Nej.</span><span class="sxs-lookup"><span data-stu-id="a211c-145">No.</span></span> <span data-ttu-id="a211c-146">Hej standardvärdet: false</span><span class="sxs-lookup"><span data-stu-id="a211c-146">hello default value: false</span></span> |
| <span data-ttu-id="a211c-147">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="a211c-147">hostKeyFingerprint</span></span> | <span data-ttu-id="a211c-148">Ange hello fingeravtryck hello värden nyckel.</span><span class="sxs-lookup"><span data-stu-id="a211c-148">Specify hello finger print of hello host key.</span></span> | <span data-ttu-id="a211c-149">Ja om hello `skipHostKeyValidation` toofalse anges.</span><span class="sxs-lookup"><span data-stu-id="a211c-149">Yes if hello `skipHostKeyValidation` is set toofalse.</span></span>  |
| <span data-ttu-id="a211c-150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a211c-150">gatewayName</span></span> |<span data-ttu-id="a211c-151">Namnet på hello Data Management Gateway tooconnect tooan lokalt SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-151">Name of hello Data Management Gateway tooconnect tooan on-premises SFTP server.</span></span> | <span data-ttu-id="a211c-152">Ja om du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-152">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="a211c-153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="a211c-153">encryptedCredential</span></span> | <span data-ttu-id="a211c-154">Krypterade autentiseringsuppgifter tooaccess hello SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-154">Encrypted credential tooaccess hello SFTP server.</span></span> <span data-ttu-id="a211c-155">Genereras automatiskt när du anger grundläggande autentisering (användarnamn och lösenord) eller SshPublicKey autentisering (användarnamn + privat sökväg eller innehåll) i Kopiera guiden eller hello ClickOnce popup-dialogruta.</span><span class="sxs-lookup"><span data-stu-id="a211c-155">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="a211c-156">Nej.</span><span class="sxs-lookup"><span data-stu-id="a211c-156">No.</span></span> <span data-ttu-id="a211c-157">Gäller bara när du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-157">Apply only when copying data from an on-premises SFTP server.</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="a211c-158">Med grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="a211c-158">Using basic authentication</span></span>

<span data-ttu-id="a211c-159">toouse grundläggande autentisering, ange `authenticationType` som `Basic`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:</span><span class="sxs-lookup"><span data-stu-id="a211c-159">toouse basic authentication, set `authenticationType` as `Basic`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="a211c-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a211c-160">Property</span></span> | <span data-ttu-id="a211c-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a211c-161">Description</span></span> | <span data-ttu-id="a211c-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="a211c-162">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a211c-163">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a211c-163">username</span></span> | <span data-ttu-id="a211c-164">Användare som har åtkomst till toohello SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="a211c-164">User who has access toohello SFTP server.</span></span> |<span data-ttu-id="a211c-165">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-165">Yes</span></span> |
| <span data-ttu-id="a211c-166">lösenord</span><span class="sxs-lookup"><span data-stu-id="a211c-166">password</span></span> | <span data-ttu-id="a211c-167">Lösenordet för hello (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="a211c-167">Password for hello user (username).</span></span> | <span data-ttu-id="a211c-168">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-168">Yes</span></span> |

#### <a name="example-basic-authentication"></a><span data-ttu-id="a211c-169">Exempel: Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="a211c-169">Example: Basic authentication</span></span>
```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="a211c-170">Exempel: Grundläggande autentisering med krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a211c-170">Example: Basic authentication with encrypted credential</span></span>

```JSON
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
      }
}
```

### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="a211c-171">Med hjälp av autentisering med SSH offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="a211c-171">Using SSH public key authentication</span></span>

<span data-ttu-id="a211c-172">toouse SSH autentisering med offentlig nyckel, ange `authenticationType` som `SshPublicKey`, och ange följande egenskaper utöver hello SFTP connector generiska som introducerades i hello sista avsnittet hello:</span><span class="sxs-lookup"><span data-stu-id="a211c-172">toouse SSH public key authentication, set `authenticationType` as `SshPublicKey`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="a211c-173">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a211c-173">Property</span></span> | <span data-ttu-id="a211c-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a211c-174">Description</span></span> | <span data-ttu-id="a211c-175">Krävs</span><span class="sxs-lookup"><span data-stu-id="a211c-175">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a211c-176">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a211c-176">username</span></span> |<span data-ttu-id="a211c-177">Användare som har åtkomst till toohello SFTP-servern</span><span class="sxs-lookup"><span data-stu-id="a211c-177">User who has access toohello SFTP server</span></span> |<span data-ttu-id="a211c-178">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-178">Yes</span></span> |
| <span data-ttu-id="a211c-179">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="a211c-179">privateKeyPath</span></span> | <span data-ttu-id="a211c-180">Ange absolut sökväg toohello fil för privat nyckel som gateway kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="a211c-180">Specify absolute path toohello private key file that gateway can access.</span></span> | <span data-ttu-id="a211c-181">Ange antingen hello `privateKeyPath` eller `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="a211c-181">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="a211c-182">Gäller bara när du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="a211c-182">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="a211c-183">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="a211c-183">privateKeyContent</span></span> | <span data-ttu-id="a211c-184">En serialiserad textsträng hello privata nyckel innehåll.</span><span class="sxs-lookup"><span data-stu-id="a211c-184">A serialized string of hello private key content.</span></span> <span data-ttu-id="a211c-185">hello guiden Kopiera kan läsa hello-fil för privat nyckel och extrahera hello privata nyckel innehållet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a211c-185">hello Copy Wizard can read hello private key file and extract hello private key content automatically.</span></span> <span data-ttu-id="a211c-186">Om du använder någon annan verktyget/SDK, Använd hello privateKeyPath egenskapen i stället.</span><span class="sxs-lookup"><span data-stu-id="a211c-186">If you are using any other tool/SDK, use hello privateKeyPath property instead.</span></span> | <span data-ttu-id="a211c-187">Ange antingen hello `privateKeyPath` eller `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="a211c-187">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="a211c-188">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="a211c-188">passPhrase</span></span> | <span data-ttu-id="a211c-189">Ange hello pass frasen/lösenord toodecrypt hello privata nyckel om hello nyckelfilen skyddas av ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="a211c-189">Specify hello pass phrase/password toodecrypt hello private key if hello key file is protected by a pass phrase.</span></span> | <span data-ttu-id="a211c-190">Ja om hello privata nyckeln skyddas av ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="a211c-190">Yes if hello private key file is protected by a pass phrase.</span></span> |

> [!NOTE]
> <span data-ttu-id="a211c-191">SFTP-anslutningen har endast stöd för OpenSSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="a211c-191">SFTP connector only support OpenSSH key.</span></span> <span data-ttu-id="a211c-192">Kontrollera att din nyckelfilen är hello rätt format.</span><span class="sxs-lookup"><span data-stu-id="a211c-192">Make sure your key file is in hello proper format.</span></span> <span data-ttu-id="a211c-193">Du kan använda Putty verktyget tooconvert från .ppk tooOpenSSH format.</span><span class="sxs-lookup"><span data-stu-id="a211c-193">You can use Putty tool tooconvert from .ppk tooOpenSSH format.</span></span>

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a><span data-ttu-id="a211c-194">Exempel: SshPublicKey autentisering med hjälp av privat nyckel filsökväg</span><span class="sxs-lookup"><span data-stu-id="a211c-194">Example: SshPublicKey authentication using private key filePath</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "mygateway"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="a211c-195">Exempel: SshPublicKey autentisering med hjälp av privat nyckel innehåll</span><span class="sxs-lookup"><span data-stu-id="a211c-195">Example: SshPublicKey authentication using private key content</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="a211c-196">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a211c-196">Dataset properties</span></span>
<span data-ttu-id="a211c-197">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a211c-197">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a211c-198">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="a211c-198">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="a211c-199">Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="a211c-199">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="a211c-200">Den innehåller information som är specifik toohello dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="a211c-200">It provides information that is specific toohello dataset type.</span></span> <span data-ttu-id="a211c-201">Hej typeProperties avsnittet för en dataset av typen **filresursen** datamängden har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a211c-201">hello typeProperties section for a dataset of type **FileShare** dataset has hello following properties:</span></span>

| <span data-ttu-id="a211c-202">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a211c-202">Property</span></span> | <span data-ttu-id="a211c-203">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a211c-203">Description</span></span> | <span data-ttu-id="a211c-204">Krävs</span><span class="sxs-lookup"><span data-stu-id="a211c-204">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a211c-205">folderPath</span><span class="sxs-lookup"><span data-stu-id="a211c-205">folderPath</span></span> |<span data-ttu-id="a211c-206">Sökvägen toohello undermapp.</span><span class="sxs-lookup"><span data-stu-id="a211c-206">Sub path toohello folder.</span></span> <span data-ttu-id="a211c-207">Använda escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="a211c-207">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="a211c-208">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="a211c-208">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="a211c-209">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="a211c-209">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="a211c-210">Ja</span><span class="sxs-lookup"><span data-stu-id="a211c-210">Yes</span></span> |
| <span data-ttu-id="a211c-211">fileName</span><span class="sxs-lookup"><span data-stu-id="a211c-211">fileName</span></span> |<span data-ttu-id="a211c-212">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="a211c-212">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="a211c-213">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="a211c-213">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="a211c-214">Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="a211c-214">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="a211c-215">Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="a211c-215">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="a211c-216">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-216">No</span></span> |
| <span data-ttu-id="a211c-217">fileFilter</span><span class="sxs-lookup"><span data-stu-id="a211c-217">fileFilter</span></span> |<span data-ttu-id="a211c-218">Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="a211c-218">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="a211c-219">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="a211c-219">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="a211c-220">Exempel 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="a211c-220">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="a211c-221">Exempel 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="a211c-221">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="a211c-222">fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="a211c-222">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="a211c-223">Den här egenskapen stöds inte med HDFS.</span><span class="sxs-lookup"><span data-stu-id="a211c-223">This property is not supported with HDFS.</span></span> |<span data-ttu-id="a211c-224">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-224">No</span></span> |
| <span data-ttu-id="a211c-225">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="a211c-225">partitionedBy</span></span> |<span data-ttu-id="a211c-226">partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="a211c-226">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="a211c-227">Till exempel folderPath som innehåller parametrar för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="a211c-227">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="a211c-228">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-228">No</span></span> |
| <span data-ttu-id="a211c-229">Format</span><span class="sxs-lookup"><span data-stu-id="a211c-229">format</span></span> | <span data-ttu-id="a211c-230">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="a211c-230">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="a211c-231">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a211c-231">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="a211c-232">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a211c-232">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="a211c-233">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="a211c-233">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="a211c-234">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-234">No</span></span> |
| <span data-ttu-id="a211c-235">Komprimering</span><span class="sxs-lookup"><span data-stu-id="a211c-235">compression</span></span> | <span data-ttu-id="a211c-236">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="a211c-236">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="a211c-237">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="a211c-237">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="a211c-238">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="a211c-238">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="a211c-239">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="a211c-239">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="a211c-240">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-240">No</span></span> |
| <span data-ttu-id="a211c-241">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="a211c-241">useBinaryTransfer</span></span> |<span data-ttu-id="a211c-242">Ange om använder binära överföringsläge.</span><span class="sxs-lookup"><span data-stu-id="a211c-242">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="a211c-243">True för en binär och FALSKT ASCII.</span><span class="sxs-lookup"><span data-stu-id="a211c-243">True for binary mode and false ASCII.</span></span> <span data-ttu-id="a211c-244">Standardvärde: True.</span><span class="sxs-lookup"><span data-stu-id="a211c-244">Default value: True.</span></span> <span data-ttu-id="a211c-245">Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer.</span><span class="sxs-lookup"><span data-stu-id="a211c-245">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="a211c-246">Nej</span><span class="sxs-lookup"><span data-stu-id="a211c-246">No</span></span> |

> [!NOTE]
> <span data-ttu-id="a211c-247">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="a211c-247">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="a211c-248">Med hjälp av egenskapen partionedBy</span><span class="sxs-lookup"><span data-stu-id="a211c-248">Using partionedBy property</span></span>
<span data-ttu-id="a211c-249">Du kan ange en dynamisk folderPath filnamn för tidsdata serien med partitionedBy som nämns i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="a211c-249">As mentioned in hello previous section, you can specify a dynamic folderPath, filename for time series data with partitionedBy.</span></span> <span data-ttu-id="a211c-250">Du kan göra det med hello Data Factory makron och hello systemvariabeln SliceStart, SliceEnd som visar hello logiska tidsperiod för en viss datasektorn.</span><span class="sxs-lookup"><span data-stu-id="a211c-250">You can do so with hello Data Factory macros and hello system variable SliceStart, SliceEnd that indicate hello logical time period for a given data slice.</span></span>

<span data-ttu-id="a211c-251">toolearn om tid serien datauppsättningar schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och utförande](data-factory-scheduling-and-execution.md), och [skapar Pipelines](data-factory-create-pipelines.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="a211c-251">toolearn about time series datasets, scheduling, and slices, See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="a211c-252">Exempel 1:</span><span class="sxs-lookup"><span data-stu-id="a211c-252">Sample 1:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="a211c-253">I det här exemplet {segment} ersätts med hello värde för Data Factory systemvariabel SliceStart hello-format (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="a211c-253">In this example {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="a211c-254">Hej SliceStart refererar toostart tiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="a211c-254">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="a211c-255">hello folderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="a211c-255">hello folderPath is different for each slice.</span></span> <span data-ttu-id="a211c-256">Exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.</span><span class="sxs-lookup"><span data-stu-id="a211c-256">Example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="a211c-257">Exempel 2:</span><span class="sxs-lookup"><span data-stu-id="a211c-257">Sample 2:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="a211c-258">I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.</span><span class="sxs-lookup"><span data-stu-id="a211c-258">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="a211c-259">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a211c-259">Copy activity properties</span></span>
<span data-ttu-id="a211c-260">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a211c-260">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a211c-261">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a211c-261">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="a211c-262">Medan hello egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a211c-262">Whereas, hello properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="a211c-263">För kopieringsaktiviteten varierar hello Typegenskaper beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a211c-263">For Copy activity, hello type properties vary depending on hello types of sources and sinks.</span></span>

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="a211c-264">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="a211c-264">Supported file and compression formats</span></span>
<span data-ttu-id="a211c-265">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="a211c-265">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-sftp-server-tooazure-blob"></a><span data-ttu-id="a211c-266">JSON-exempel: Kopiera data från SFTP server tooAzure blob</span><span class="sxs-lookup"><span data-stu-id="a211c-266">JSON Example: Copy data from SFTP server tooAzure blob</span></span>
<span data-ttu-id="a211c-267">hello följande exempel innehåller exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a211c-267">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a211c-268">De visar hur toocopy från SFTP datakällan tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a211c-268">They show how toocopy data from SFTP source tooAzure Blob Storage.</span></span> <span data-ttu-id="a211c-269">Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a211c-269">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a211c-270">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="a211c-270">This sample provides JSON snippets.</span></span> <span data-ttu-id="a211c-271">Stegvisa instruktioner för att skapa datafabriken hello inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="a211c-271">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="a211c-272">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="a211c-272">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="a211c-273">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="a211c-273">hello sample has hello following data factory entities:</span></span>

* <span data-ttu-id="a211c-274">En länkad tjänst av typen [sftp](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a211c-274">A linked service of type [sftp](#linked-service-properties).</span></span>
* <span data-ttu-id="a211c-275">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a211c-275">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="a211c-276">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a211c-276">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="a211c-277">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a211c-277">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="a211c-278">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a211c-278">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a211c-279">hello exemplet kopierar data från en SFTP server tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="a211c-279">hello sample copies data from an SFTP server tooan Azure blob every hour.</span></span> <span data-ttu-id="a211c-280">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a211c-280">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="a211c-281">**SFTP länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="a211c-281">**SFTP linked service**</span></span>

<span data-ttu-id="a211c-282">Det här exemplet använder hello grundläggande autentisering med användarnamn och lösenord i klartext.</span><span class="sxs-lookup"><span data-stu-id="a211c-282">This example uses hello basic authentication with user name and password in plain text.</span></span> <span data-ttu-id="a211c-283">Du kan också använda något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="a211c-283">You can also use one of hello following ways:</span></span>

* <span data-ttu-id="a211c-284">Grundläggande autentisering med krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a211c-284">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="a211c-285">Autentisering med SSH offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="a211c-285">SSH public key authentication</span></span>

<span data-ttu-id="a211c-286">Se [FTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="a211c-286">See [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON

{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver",
            "port": 22,
            "authenticationType": "Basic",
            "username": "myuser",
            "password": "mypassword",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "mygateway"
        }
    }
}
```
<span data-ttu-id="a211c-287">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="a211c-287">**Azure Storage linked service**</span></span>

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="a211c-288">**SFTP inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="a211c-288">**SFTP input dataset**</span></span>

<span data-ttu-id="a211c-289">Den här datamängden refererar toohello SFTP mappen `mysharedfolder` och `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="a211c-289">This dataset refers toohello SFTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="a211c-290">hello pipeline kopierar hello filen toohello mål.</span><span class="sxs-lookup"><span data-stu-id="a211c-290">hello pipeline copies hello file toohello destination.</span></span>

<span data-ttu-id="a211c-291">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a211c-291">Setting "external": "true" informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "SFTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "SftpLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="a211c-292">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="a211c-292">**Azure Blob output dataset**</span></span>

<span data-ttu-id="a211c-293">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a211c-293">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a211c-294">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a211c-294">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="a211c-295">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="a211c-295">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="a211c-296">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="a211c-296">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="a211c-297">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="a211c-297">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="a211c-298">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a211c-298">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource** and **sink** type is set too**BlobSink**.</span></span>

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00Z",
        "end": "2017-02-20T19:00:00Z"
    }
}
```

## <a name="performance-and-tuning"></a><span data-ttu-id="a211c-299">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="a211c-299">Performance and Tuning</span></span>
<span data-ttu-id="a211c-300">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="a211c-300">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a211c-301">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a211c-301">Next Steps</span></span>
<span data-ttu-id="a211c-302">Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a211c-302">See hello following articles:</span></span>

* <span data-ttu-id="a211c-303">[Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a211c-303">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
