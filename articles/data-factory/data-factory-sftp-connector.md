---
title: "Flytta data från SFTP-servern med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från en lokal eller molnet SFTP server med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 3a73311342489af031ed2ea1489e56292ebf2e09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-sftp-server-using-azure-data-factory"></a><span data-ttu-id="515c9-103">Flytta data från en SFTP-server med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="515c9-103">Move data from an SFTP server using Azure Data Factory</span></span>
<span data-ttu-id="515c9-104">Den här artikeln beskrivs hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en på lokalt/i molnet SFTP-server till ett dataarkiv som stöds sink.</span><span class="sxs-lookup"><span data-stu-id="515c9-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from an on-premises/cloud SFTP server to a supported sink data store.</span></span> <span data-ttu-id="515c9-105">Den här artikeln bygger på den [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning kopieringsaktiviteten och listan över datakällor som stöds som källor/sänkor.</span><span class="sxs-lookup"><span data-stu-id="515c9-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="515c9-106">Data factory stöder för närvarande endast flytta data från en SFTP-server till andra databaser, men inte för att flytta data från andra datalager till en SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="515c9-106">Data factory currently supports only moving data from an SFTP server to other data stores, but not for moving data from other data stores to an SFTP server.</span></span> <span data-ttu-id="515c9-107">Den stöder både lokalt och molntjänster SFTP-servrar.</span><span class="sxs-lookup"><span data-stu-id="515c9-107">It supports both on-premises and cloud SFTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="515c9-108">Kopieringsaktiviteten tar inte bort källfilen när den har kopierats till målet.</span><span class="sxs-lookup"><span data-stu-id="515c9-108">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="515c9-109">Om du vill ta bort källfilen efter en lyckad kopiering kan du skapa en anpassad aktivitet för att ta bort filen och använda aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="515c9-109">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="515c9-110">Scenarier som stöds och typer av autentisering</span><span class="sxs-lookup"><span data-stu-id="515c9-110">Supported scenarios and authentication types</span></span>
<span data-ttu-id="515c9-111">Du kan använda den här SFTP-anslutningen för att kopiera data från **både molnet SFTP-servrar och lokala SFTP servrar**.</span><span class="sxs-lookup"><span data-stu-id="515c9-111">You can use this SFTP connector to copy data from **both cloud SFTP servers and on-premises SFTP servers**.</span></span> <span data-ttu-id="515c9-112">**Grundläggande** och **SshPublicKey** autentiseringstyper som stöds när du ansluter till SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="515c9-112">**Basic** and **SshPublicKey** authentication types are supported when connecting to the SFTP server.</span></span>

<span data-ttu-id="515c9-113">När du kopierar data från en lokal SFTP-server, måste du installera en Data Management Gateway i lokal miljö/Azure VM.</span><span class="sxs-lookup"><span data-stu-id="515c9-113">When copying data from an on-premises SFTP server, you need install a Data Management Gateway in the on-premises environment/Azure VM.</span></span> <span data-ttu-id="515c9-114">Se [Data Management Gateway](data-factory-data-management-gateway.md) information om gatewayen.</span><span class="sxs-lookup"><span data-stu-id="515c9-114">See [Data Management Gateway](data-factory-data-management-gateway.md) for details on the gateway.</span></span> <span data-ttu-id="515c9-115">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner för att konfigurera gatewayen och använder den.</span><span class="sxs-lookup"><span data-stu-id="515c9-115">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway and using it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="515c9-116">Komma igång</span><span class="sxs-lookup"><span data-stu-id="515c9-116">Getting started</span></span>
<span data-ttu-id="515c9-117">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en SFTP-datakälla med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="515c9-117">You can create a pipeline with a copy activity that moves data from an SFTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="515c9-118">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="515c9-118">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="515c9-119">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="515c9-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

- <span data-ttu-id="515c9-120">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="515c9-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="515c9-121">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="515c9-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> <span data-ttu-id="515c9-122">JSON-exempel att kopiera data från SFTP-servern till Azure Blob Storage finns [JSON-exempel: kopiera data från SFTP-server till Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="515c9-122">For JSON samples to copy data from SFTP server to Azure Blob Storage, see [JSON Example: Copy data from SFTP server to Azure blob](#json-example-copy-data-from-sftp-server-to-azure-blob) section of this article.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="515c9-123">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="515c9-123">Linked service properties</span></span>
<span data-ttu-id="515c9-124">Följande tabell innehåller en beskrivning för JSON-element som är specifika för FTP-länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="515c9-124">The following table provides description for JSON elements specific to FTP linked service.</span></span>

| <span data-ttu-id="515c9-125">Egenskap</span><span class="sxs-lookup"><span data-stu-id="515c9-125">Property</span></span> | <span data-ttu-id="515c9-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="515c9-126">Description</span></span> | <span data-ttu-id="515c9-127">Krävs</span><span class="sxs-lookup"><span data-stu-id="515c9-127">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="515c9-128">typ</span><span class="sxs-lookup"><span data-stu-id="515c9-128">type</span></span> | <span data-ttu-id="515c9-129">Egenskapen type måste anges till `Sftp`.</span><span class="sxs-lookup"><span data-stu-id="515c9-129">The type property must be set to `Sftp`.</span></span> |<span data-ttu-id="515c9-130">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-130">Yes</span></span> |
| <span data-ttu-id="515c9-131">värden</span><span class="sxs-lookup"><span data-stu-id="515c9-131">host</span></span> | <span data-ttu-id="515c9-132">Namn eller IP-adress till SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="515c9-132">Name or IP address of the SFTP server.</span></span> |<span data-ttu-id="515c9-133">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-133">Yes</span></span> |
| <span data-ttu-id="515c9-134">port</span><span class="sxs-lookup"><span data-stu-id="515c9-134">port</span></span> |<span data-ttu-id="515c9-135">Port som avlyssnas SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="515c9-135">Port on which the SFTP server is listening.</span></span> <span data-ttu-id="515c9-136">Standardvärdet är: 21</span><span class="sxs-lookup"><span data-stu-id="515c9-136">The default value is: 21</span></span> |<span data-ttu-id="515c9-137">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-137">No</span></span> |
| <span data-ttu-id="515c9-138">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="515c9-138">authenticationType</span></span> |<span data-ttu-id="515c9-139">Ange autentiseringstypen.</span><span class="sxs-lookup"><span data-stu-id="515c9-139">Specify authentication type.</span></span> <span data-ttu-id="515c9-140">Tillåtna värden: **grundläggande**, **SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="515c9-140">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="515c9-141">Referera till [använder grundläggande autentisering](#using-basic-authentication) och [med hjälp av SSH autentisering med offentlig nyckel](#using-ssh-public-key-authentication) respektive avsnitt på fler egenskaper och JSON-exempel.</span><span class="sxs-lookup"><span data-stu-id="515c9-141">Refer to [Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="515c9-142">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-142">Yes</span></span> |
| <span data-ttu-id="515c9-143">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="515c9-143">skipHostKeyValidation</span></span> | <span data-ttu-id="515c9-144">Ange om du vill hoppa över värden viktiga validering.</span><span class="sxs-lookup"><span data-stu-id="515c9-144">Specify whether to skip host key validation.</span></span> | <span data-ttu-id="515c9-145">Nej.</span><span class="sxs-lookup"><span data-stu-id="515c9-145">No.</span></span> <span data-ttu-id="515c9-146">Standardvärde: false</span><span class="sxs-lookup"><span data-stu-id="515c9-146">The default value: false</span></span> |
| <span data-ttu-id="515c9-147">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="515c9-147">hostKeyFingerprint</span></span> | <span data-ttu-id="515c9-148">Ange fingeravtryck för värdnyckeln.</span><span class="sxs-lookup"><span data-stu-id="515c9-148">Specify the finger print of the host key.</span></span> | <span data-ttu-id="515c9-149">Ja om den `skipHostKeyValidation` är inställd på false.</span><span class="sxs-lookup"><span data-stu-id="515c9-149">Yes if the `skipHostKeyValidation` is set to false.</span></span>  |
| <span data-ttu-id="515c9-150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="515c9-150">gatewayName</span></span> |<span data-ttu-id="515c9-151">Namnet på Data Management Gateway för att ansluta till en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="515c9-151">Name of the Data Management Gateway to connect to an on-premises SFTP server.</span></span> | <span data-ttu-id="515c9-152">Ja om du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="515c9-152">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="515c9-153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="515c9-153">encryptedCredential</span></span> | <span data-ttu-id="515c9-154">Krypterade autentiseringsuppgifter för åtkomst till SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="515c9-154">Encrypted credential to access the SFTP server.</span></span> <span data-ttu-id="515c9-155">Genereras automatiskt när du anger grundläggande autentisering (användarnamn och lösenord) eller SshPublicKey autentisering (användarnamn + privat sökväg eller innehåll) i guiden Kopiera eller ClickOnce popup-dialogruta.</span><span class="sxs-lookup"><span data-stu-id="515c9-155">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="515c9-156">Nej.</span><span class="sxs-lookup"><span data-stu-id="515c9-156">No.</span></span> <span data-ttu-id="515c9-157">Gäller bara när du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="515c9-157">Apply only when copying data from an on-premises SFTP server.</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="515c9-158">Med grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="515c9-158">Using basic authentication</span></span>

<span data-ttu-id="515c9-159">Om du vill använda grundläggande autentisering, `authenticationType` som `Basic`, och ange följande egenskaper förutom SFTP kopplingen generiska som introducerades i det sista avsnittet:</span><span class="sxs-lookup"><span data-stu-id="515c9-159">To use basic authentication, set `authenticationType` as `Basic`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="515c9-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="515c9-160">Property</span></span> | <span data-ttu-id="515c9-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="515c9-161">Description</span></span> | <span data-ttu-id="515c9-162">Krävs</span><span class="sxs-lookup"><span data-stu-id="515c9-162">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="515c9-163">användarnamn</span><span class="sxs-lookup"><span data-stu-id="515c9-163">username</span></span> | <span data-ttu-id="515c9-164">Användare som har åtkomst till SFTP-servern.</span><span class="sxs-lookup"><span data-stu-id="515c9-164">User who has access to the SFTP server.</span></span> |<span data-ttu-id="515c9-165">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-165">Yes</span></span> |
| <span data-ttu-id="515c9-166">lösenord</span><span class="sxs-lookup"><span data-stu-id="515c9-166">password</span></span> | <span data-ttu-id="515c9-167">Lösenord för användare (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="515c9-167">Password for the user (username).</span></span> | <span data-ttu-id="515c9-168">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-168">Yes</span></span> |

#### <a name="example-basic-authentication"></a><span data-ttu-id="515c9-169">Exempel: Grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="515c9-169">Example: Basic authentication</span></span>
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

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="515c9-170">Exempel: Grundläggande autentisering med krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="515c9-170">Example: Basic authentication with encrypted credential</span></span>

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

### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="515c9-171">Med hjälp av autentisering med SSH offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="515c9-171">Using SSH public key authentication</span></span>

<span data-ttu-id="515c9-172">Om du vill använda autentisering med SSH offentlig nyckel, `authenticationType` som `SshPublicKey`, och ange följande egenskaper förutom SFTP kopplingen generiska som introducerades i det sista avsnittet:</span><span class="sxs-lookup"><span data-stu-id="515c9-172">To use SSH public key authentication, set `authenticationType` as `SshPublicKey`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="515c9-173">Egenskap</span><span class="sxs-lookup"><span data-stu-id="515c9-173">Property</span></span> | <span data-ttu-id="515c9-174">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="515c9-174">Description</span></span> | <span data-ttu-id="515c9-175">Krävs</span><span class="sxs-lookup"><span data-stu-id="515c9-175">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="515c9-176">användarnamn</span><span class="sxs-lookup"><span data-stu-id="515c9-176">username</span></span> |<span data-ttu-id="515c9-177">Användare som har åtkomst till SFTP-server</span><span class="sxs-lookup"><span data-stu-id="515c9-177">User who has access to the SFTP server</span></span> |<span data-ttu-id="515c9-178">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-178">Yes</span></span> |
| <span data-ttu-id="515c9-179">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="515c9-179">privateKeyPath</span></span> | <span data-ttu-id="515c9-180">Ange absolut sökväg till filen för privat nyckel som gateway kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="515c9-180">Specify absolute path to the private key file that gateway can access.</span></span> | <span data-ttu-id="515c9-181">Ange antingen det `privateKeyPath` eller `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="515c9-181">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="515c9-182">Gäller bara när du kopierar data från en lokal SFTP-server.</span><span class="sxs-lookup"><span data-stu-id="515c9-182">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="515c9-183">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="515c9-183">privateKeyContent</span></span> | <span data-ttu-id="515c9-184">En serialiserad sträng av privat nyckel innehållet.</span><span class="sxs-lookup"><span data-stu-id="515c9-184">A serialized string of the private key content.</span></span> <span data-ttu-id="515c9-185">Guiden Kopiera kan läsa filen för privat nyckel och extrahera privata nyckel innehållet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="515c9-185">The Copy Wizard can read the private key file and extract the private key content automatically.</span></span> <span data-ttu-id="515c9-186">Om du använder någon annan verktyget/SDK, använder du egenskapen privateKeyPath.</span><span class="sxs-lookup"><span data-stu-id="515c9-186">If you are using any other tool/SDK, use the privateKeyPath property instead.</span></span> | <span data-ttu-id="515c9-187">Ange antingen det `privateKeyPath` eller `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="515c9-187">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="515c9-188">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="515c9-188">passPhrase</span></span> | <span data-ttu-id="515c9-189">Ange pass frasen/lösenord för att dekryptera den privata nyckeln om nyckelfilen skyddas av ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="515c9-189">Specify the pass phrase/password to decrypt the private key if the key file is protected by a pass phrase.</span></span> | <span data-ttu-id="515c9-190">Ja om filen för privata nyckeln skyddas av ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="515c9-190">Yes if the private key file is protected by a pass phrase.</span></span> |

> [!NOTE]
> <span data-ttu-id="515c9-191">SFTP-anslutningen har endast stöd för OpenSSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="515c9-191">SFTP connector only support OpenSSH key.</span></span> <span data-ttu-id="515c9-192">Kontrollera att din nyckelfilen är i rätt format.</span><span class="sxs-lookup"><span data-stu-id="515c9-192">Make sure your key file is in the proper format.</span></span> <span data-ttu-id="515c9-193">Du kan använda Putty för att konvertera från .ppk till OpenSSH-format.</span><span class="sxs-lookup"><span data-stu-id="515c9-193">You can use Putty tool to convert from .ppk to OpenSSH format.</span></span>

#### <a name="example-sshpublickey-authentication-using-private-key-filepath"></a><span data-ttu-id="515c9-194">Exempel: SshPublicKey autentisering med hjälp av privat nyckel filsökväg</span><span class="sxs-lookup"><span data-stu-id="515c9-194">Example: SshPublicKey authentication using private key filePath</span></span>

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="515c9-195">Exempel: SshPublicKey autentisering med hjälp av privat nyckel innehåll</span><span class="sxs-lookup"><span data-stu-id="515c9-195">Example: SshPublicKey authentication using private key content</span></span>

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
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="515c9-196">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="515c9-196">Dataset properties</span></span>
<span data-ttu-id="515c9-197">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="515c9-197">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="515c9-198">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="515c9-198">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="515c9-199">Den **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="515c9-199">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="515c9-200">Den innehåller information som är specifik för dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="515c9-200">It provides information that is specific to the dataset type.</span></span> <span data-ttu-id="515c9-201">TypeProperties avsnittet för en dataset av typen **filresursen** datamängden har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="515c9-201">The typeProperties section for a dataset of type **FileShare** dataset has the following properties:</span></span>

| <span data-ttu-id="515c9-202">Egenskap</span><span class="sxs-lookup"><span data-stu-id="515c9-202">Property</span></span> | <span data-ttu-id="515c9-203">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="515c9-203">Description</span></span> | <span data-ttu-id="515c9-204">Krävs</span><span class="sxs-lookup"><span data-stu-id="515c9-204">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="515c9-205">folderPath</span><span class="sxs-lookup"><span data-stu-id="515c9-205">folderPath</span></span> |<span data-ttu-id="515c9-206">Sub sökvägen till mappen.</span><span class="sxs-lookup"><span data-stu-id="515c9-206">Sub path to the folder.</span></span> <span data-ttu-id="515c9-207">Använda escape-tecknet ' \ ' för specialtecken i strängen.</span><span class="sxs-lookup"><span data-stu-id="515c9-207">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="515c9-208">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="515c9-208">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="515c9-209">Du kan kombinera den här egenskapen med **partitionBy** ha mappen sökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="515c9-209">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="515c9-210">Ja</span><span class="sxs-lookup"><span data-stu-id="515c9-210">Yes</span></span> |
| <span data-ttu-id="515c9-211">fileName</span><span class="sxs-lookup"><span data-stu-id="515c9-211">fileName</span></span> |<span data-ttu-id="515c9-212">Ange namnet på filen i den **folderPath** om du vill att referera till en viss fil i mappen.</span><span class="sxs-lookup"><span data-stu-id="515c9-212">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="515c9-213">Om du inte anger något värde för den här egenskapen tabellen pekar på alla filer i mappen.</span><span class="sxs-lookup"><span data-stu-id="515c9-213">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="515c9-214">Om filnamnet inte anges för en datamängd för utdata är namnet på den genererade filen i följande det här formatet:</span><span class="sxs-lookup"><span data-stu-id="515c9-214">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="515c9-215">Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="515c9-215">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="515c9-216">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-216">No</span></span> |
| <span data-ttu-id="515c9-217">fileFilter</span><span class="sxs-lookup"><span data-stu-id="515c9-217">fileFilter</span></span> |<span data-ttu-id="515c9-218">Ange ett filter som används för att välja en delmängd av filer i mappsökvägen i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="515c9-218">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="515c9-219">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="515c9-219">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="515c9-220">Exempel 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="515c9-220">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="515c9-221">Exempel 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="515c9-221">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="515c9-222">fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="515c9-222">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="515c9-223">Den här egenskapen stöds inte med HDFS.</span><span class="sxs-lookup"><span data-stu-id="515c9-223">This property is not supported with HDFS.</span></span> |<span data-ttu-id="515c9-224">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-224">No</span></span> |
| <span data-ttu-id="515c9-225">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="515c9-225">partitionedBy</span></span> |<span data-ttu-id="515c9-226">partitionedBy kan användas för att ange en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="515c9-226">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="515c9-227">Till exempel folderPath som innehåller parametrar för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="515c9-227">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="515c9-228">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-228">No</span></span> |
| <span data-ttu-id="515c9-229">Format</span><span class="sxs-lookup"><span data-stu-id="515c9-229">format</span></span> | <span data-ttu-id="515c9-230">Följande format stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="515c9-230">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="515c9-231">Ange den **typen** egenskap under format till ett av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="515c9-231">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="515c9-232">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="515c9-232">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="515c9-233">Om du vill **kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppa över avsnittet format i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="515c9-233">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="515c9-234">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-234">No</span></span> |
| <span data-ttu-id="515c9-235">Komprimering</span><span class="sxs-lookup"><span data-stu-id="515c9-235">compression</span></span> | <span data-ttu-id="515c9-236">Ange typ och kompression för data.</span><span class="sxs-lookup"><span data-stu-id="515c9-236">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="515c9-237">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="515c9-237">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="515c9-238">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="515c9-238">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="515c9-239">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="515c9-239">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="515c9-240">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-240">No</span></span> |
| <span data-ttu-id="515c9-241">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="515c9-241">useBinaryTransfer</span></span> |<span data-ttu-id="515c9-242">Ange om använder binära överföringsläge.</span><span class="sxs-lookup"><span data-stu-id="515c9-242">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="515c9-243">True för en binär och FALSKT ASCII.</span><span class="sxs-lookup"><span data-stu-id="515c9-243">True for binary mode and false ASCII.</span></span> <span data-ttu-id="515c9-244">Standardvärde: True.</span><span class="sxs-lookup"><span data-stu-id="515c9-244">Default value: True.</span></span> <span data-ttu-id="515c9-245">Den här egenskapen kan endast användas när associerade linked service-typen är av typen: FtpServer.</span><span class="sxs-lookup"><span data-stu-id="515c9-245">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="515c9-246">Nej</span><span class="sxs-lookup"><span data-stu-id="515c9-246">No</span></span> |

> [!NOTE]
> <span data-ttu-id="515c9-247">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="515c9-247">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="515c9-248">Med hjälp av egenskapen partionedBy</span><span class="sxs-lookup"><span data-stu-id="515c9-248">Using partionedBy property</span></span>
<span data-ttu-id="515c9-249">Du kan ange en dynamisk folderPath filnamn för tidsdata serien med partitionedBy som nämns i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="515c9-249">As mentioned in the previous section, you can specify a dynamic folderPath, filename for time series data with partitionedBy.</span></span> <span data-ttu-id="515c9-250">Du kan göra det med Data Factory-makron och systemvariabeln SliceStart, SliceEnd som indikerar den logiska tidsperioden för en viss datasektorn.</span><span class="sxs-lookup"><span data-stu-id="515c9-250">You can do so with the Data Factory macros and the system variable SliceStart, SliceEnd that indicate the logical time period for a given data slice.</span></span>

<span data-ttu-id="515c9-251">Läs om tid serien datauppsättningar schemaläggning och segment i [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och utförande](data-factory-scheduling-and-execution.md), och [skapar Pipelines](data-factory-create-pipelines.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="515c9-251">To learn about time series datasets, scheduling, and slices, See [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="515c9-252">Exempel 1:</span><span class="sxs-lookup"><span data-stu-id="515c9-252">Sample 1:</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="515c9-253">I det här exemplet {segment} ersättas med det angivna värdet för Data Factory systemvariabel SliceStart i formatet (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="515c9-253">In this example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="515c9-254">SliceStart refererar till starttid av sektorn.</span><span class="sxs-lookup"><span data-stu-id="515c9-254">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="515c9-255">FolderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="515c9-255">The folderPath is different for each slice.</span></span> <span data-ttu-id="515c9-256">Exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.</span><span class="sxs-lookup"><span data-stu-id="515c9-256">Example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="515c9-257">Exempel 2:</span><span class="sxs-lookup"><span data-stu-id="515c9-257">Sample 2:</span></span>

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
<span data-ttu-id="515c9-258">I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.</span><span class="sxs-lookup"><span data-stu-id="515c9-258">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="515c9-259">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="515c9-259">Copy activity properties</span></span>
<span data-ttu-id="515c9-260">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="515c9-260">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="515c9-261">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="515c9-261">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="515c9-262">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="515c9-262">Whereas, the properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="515c9-263">För Kopieringsaktivitet kan variera Typegenskaper beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="515c9-263">For Copy activity, the type properties vary depending on the types of sources and sinks.</span></span>

[!INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="515c9-264">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="515c9-264">Supported file and compression formats</span></span>
<span data-ttu-id="515c9-265">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="515c9-265">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-sftp-server-to-azure-blob"></a><span data-ttu-id="515c9-266">JSON-exempel: Kopiera data från SFTP-server till Azure blob</span><span class="sxs-lookup"><span data-stu-id="515c9-266">JSON Example: Copy data from SFTP server to Azure blob</span></span>
<span data-ttu-id="515c9-267">I följande exempel innehåller exempel JSON definitioner som du kan använda för att skapa en pipeline med [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="515c9-267">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="515c9-268">De visar hur du kopierar data från SFTP källa till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="515c9-268">They show how to copy data from SFTP source to Azure Blob Storage.</span></span> <span data-ttu-id="515c9-269">Dock datan kan kopieras **direkt** från alla källor till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="515c9-269">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="515c9-270">Det här exemplet innehåller JSON kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="515c9-270">This sample provides JSON snippets.</span></span> <span data-ttu-id="515c9-271">Stegvisa instruktioner för att skapa datafabriken inkluderas inte.</span><span class="sxs-lookup"><span data-stu-id="515c9-271">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="515c9-272">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="515c9-272">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="515c9-273">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="515c9-273">The sample has the following data factory entities:</span></span>

* <span data-ttu-id="515c9-274">En länkad tjänst av typen [sftp](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="515c9-274">A linked service of type [sftp](#linked-service-properties).</span></span>
* <span data-ttu-id="515c9-275">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="515c9-275">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="515c9-276">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="515c9-276">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="515c9-277">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="515c9-277">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="515c9-278">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="515c9-278">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="515c9-279">Exemplet kopierar data från en SFTP-server till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="515c9-279">The sample copies data from an SFTP server to an Azure blob every hour.</span></span> <span data-ttu-id="515c9-280">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="515c9-280">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="515c9-281">**SFTP länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="515c9-281">**SFTP linked service**</span></span>

<span data-ttu-id="515c9-282">Det här exemplet använder grundläggande autentisering med användarnamn och lösenord i klartext.</span><span class="sxs-lookup"><span data-stu-id="515c9-282">This example uses the basic authentication with user name and password in plain text.</span></span> <span data-ttu-id="515c9-283">Du kan också använda något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="515c9-283">You can also use one of the following ways:</span></span>

* <span data-ttu-id="515c9-284">Grundläggande autentisering med krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="515c9-284">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="515c9-285">Autentisering med SSH offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="515c9-285">SSH public key authentication</span></span>

<span data-ttu-id="515c9-286">Se [FTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="515c9-286">See [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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
<span data-ttu-id="515c9-287">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="515c9-287">**Azure Storage linked service**</span></span>

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
<span data-ttu-id="515c9-288">**SFTP inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="515c9-288">**SFTP input dataset**</span></span>

<span data-ttu-id="515c9-289">Den här datamängden refererar till mappen SFTP `mysharedfolder` och `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="515c9-289">This dataset refers to the SFTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="515c9-290">Pipelinen kopierar filen till målet.</span><span class="sxs-lookup"><span data-stu-id="515c9-290">The pipeline copies the file to the destination.</span></span>

<span data-ttu-id="515c9-291">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="515c9-291">Setting "external": "true" informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="515c9-292">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="515c9-292">**Azure Blob output dataset**</span></span>

<span data-ttu-id="515c9-293">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="515c9-293">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="515c9-294">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="515c9-294">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="515c9-295">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="515c9-295">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="515c9-296">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="515c9-296">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="515c9-297">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="515c9-297">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="515c9-298">I pipeline-JSON-definitionen av **källa** är inställd på **FileSystemSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="515c9-298">In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**.</span></span>

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

## <a name="performance-and-tuning"></a><span data-ttu-id="515c9-299">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="515c9-299">Performance and Tuning</span></span>
<span data-ttu-id="515c9-300">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="515c9-300">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="515c9-301">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="515c9-301">Next Steps</span></span>
<span data-ttu-id="515c9-302">Se följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="515c9-302">See the following articles:</span></span>

* <span data-ttu-id="515c9-303">[Kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) stegvisa instruktioner för att skapa en pipeline med en kopia-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="515c9-303">[Copy Activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions for creating a pipeline with a Copy Activity.</span></span>
