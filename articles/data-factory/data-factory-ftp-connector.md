---
title: "aaaMove data från en FTP-server med hjälp av Azure Data Factory | Microsoft Docs"
description: "Mer information om hur toomove data från en FTP-server med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jingwang
ms.openlocfilehash: c707e29532b2a8a870603948cb6150ab857bd6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a><span data-ttu-id="7cd6a-103">Flytta data från en FTP-server med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7cd6a-103">Move data from an FTP server by using Azure Data Factory</span></span>
<span data-ttu-id="7cd6a-104">Den här artikeln förklarar hur toouse hello kopieringsaktiviteten i Azure Data Factory toomove data från en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-104">This article explains how toouse hello copy activity in Azure Data Factory toomove data from an FTP server.</span></span> <span data-ttu-id="7cd6a-105">Den bygger på hello [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="7cd6a-106">Du kan kopiera data från en FTP-server tooany stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-106">You can copy data from an FTP server tooany supported sink data store.</span></span> <span data-ttu-id="7cd6a-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-107">For a list of data stores supported as sinks by hello copy activity, see hello [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="7cd6a-108">Data Factory stöder för närvarande endast flytta data från ett FTP-server tooother lagras, men inte flytta data från andra data lagras tooan FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-108">Data Factory currently supports only moving data from an FTP server tooother data stores, but not moving data from other data stores tooan FTP server.</span></span> <span data-ttu-id="7cd6a-109">Den stöder både lokalt och molntjänster FTP-servrar.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-109">It supports both on-premises and cloud FTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="7cd6a-110">Hej kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-110">hello copy activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="7cd6a-111">Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och Använd hello aktiviteten i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-111">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file, and use hello activity in hello pipeline.</span></span> 

## <a name="enable-connectivity"></a><span data-ttu-id="7cd6a-112">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="7cd6a-112">Enable connectivity</span></span>
<span data-ttu-id="7cd6a-113">Om du flyttar data från en **lokalt** datalagret för FTP-server tooa molnet (exempelvis tooAzure Blob storage kvar), installera och använda Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-113">If you are moving data from an **on-premises** FTP server tooa cloud data store (for example, tooAzure Blob storage), install and use Data Management Gateway.</span></span> <span data-ttu-id="7cd6a-114">hello Data Management Gateway är en klientagent som är installerad på den lokala datorn och det tillåter cloud services tooconnect tooan lokala resursen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-114">hello Data Management Gateway is a client agent that is installed on your on-premises machine, and it allows cloud services tooconnect tooan on-premises resource.</span></span> <span data-ttu-id="7cd6a-115">Mer information finns i [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-115">For details, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="7cd6a-116">Stegvisa instruktioner för att ställa in hello gateway och använder den finns [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-116">For step-by-step instructions on setting up hello gateway and using it, see [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="7cd6a-117">Du kan använda hello gateway tooconnect tooan FTP-server, även om hello-servern är på en Azure-infrastruktur som en tjänst (IaaS) virtuell dator (VM).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-117">You use hello gateway tooconnect tooan FTP server, even if hello server is on an Azure infrastructure as a service (IaaS) virtual machine (VM).</span></span>

<span data-ttu-id="7cd6a-118">Det är möjligt tooinstall hello gateway på hello samma lokala dator eller IaaS-VM hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-118">It is possible tooinstall hello gateway on hello same on-premises machine or IaaS VM as hello FTP server.</span></span> <span data-ttu-id="7cd6a-119">Vi rekommenderar dock att du installerar hello gateway på en separat dator eller resurskonflikter för IaaS VM tooavoid och för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-119">However, we recommend that you install hello gateway on a separate machine or IaaS VM tooavoid resource contention, and for better performance.</span></span> <span data-ttu-id="7cd6a-120">När du installerar hello gateway på en separat dator ska hello datorn kunna tooaccess hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-120">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello FTP server.</span></span>

## <a name="get-started"></a><span data-ttu-id="7cd6a-121">Kom igång</span><span class="sxs-lookup"><span data-stu-id="7cd6a-121">Get started</span></span>
<span data-ttu-id="7cd6a-122">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en FTP-datakälla med hjälp av olika verktyg eller API: er.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-122">You can create a pipeline with a copy activity that moves data from an FTP source by using different tools or APIs.</span></span>

<span data-ttu-id="7cd6a-123">hello enklaste sättet toocreate en pipeline är toouse hello **Data Factory kopiera guiden**.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-123">hello easiest way toocreate a pipeline is toouse hello **Data Factory Copy Wizard**.</span></span> <span data-ttu-id="7cd6a-124">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough.</span></span>

<span data-ttu-id="7cd6a-125">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7cd6a-126">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="7cd6a-127">Om du använder hello verktyg eller API: er, utför hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalager:</span><span class="sxs-lookup"><span data-stu-id="7cd6a-127">Whether you use hello tools or APIs, perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="7cd6a-128">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="7cd6a-129">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="7cd6a-130">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="7cd6a-131">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="7cd6a-132">När du använder verktyg eller API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-132">When you use tools or APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="7cd6a-133">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en FTP-datalagret finns hello [JSON-exempel: kopiera data från FTP-servern tooAzure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an FTP data store, see hello [JSON example: Copy data from FTP server tooAzure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="7cd6a-134">Mer information om stöds fil- och komprimering format toouse finns [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-134">For details about supported file and compression formats toouse, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="7cd6a-135">hello följande avsnitt innehåller information om JSON-egenskaper som är specifika tooFTP för används toodefine Data Factory-enheter.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-135">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooFTP.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="7cd6a-136">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="7cd6a-136">Linked service properties</span></span>
<span data-ttu-id="7cd6a-137">hello i den följande tabellen beskrivs JSON-element specifika tooan FTP-länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-137">hello following table describes JSON elements specific tooan FTP linked service.</span></span>

| <span data-ttu-id="7cd6a-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7cd6a-138">Property</span></span> | <span data-ttu-id="7cd6a-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7cd6a-139">Description</span></span> | <span data-ttu-id="7cd6a-140">Krävs</span><span class="sxs-lookup"><span data-stu-id="7cd6a-140">Required</span></span> | <span data-ttu-id="7cd6a-141">Standard</span><span class="sxs-lookup"><span data-stu-id="7cd6a-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7cd6a-142">typ</span><span class="sxs-lookup"><span data-stu-id="7cd6a-142">type</span></span> |<span data-ttu-id="7cd6a-143">Ange den här tooFtpServer.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-143">Set this tooFtpServer.</span></span> |<span data-ttu-id="7cd6a-144">Ja</span><span class="sxs-lookup"><span data-stu-id="7cd6a-144">Yes</span></span> |&nbsp; |
| <span data-ttu-id="7cd6a-145">värden</span><span class="sxs-lookup"><span data-stu-id="7cd6a-145">host</span></span> |<span data-ttu-id="7cd6a-146">Ange hello namn eller IP-adressen för hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-146">Specify hello name or IP address of hello FTP server.</span></span> |<span data-ttu-id="7cd6a-147">Ja</span><span class="sxs-lookup"><span data-stu-id="7cd6a-147">Yes</span></span> |&nbsp; |
| <span data-ttu-id="7cd6a-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="7cd6a-148">authenticationType</span></span> |<span data-ttu-id="7cd6a-149">Ange hello autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-149">Specify hello authentication type.</span></span> |<span data-ttu-id="7cd6a-150">Ja</span><span class="sxs-lookup"><span data-stu-id="7cd6a-150">Yes</span></span> |<span data-ttu-id="7cd6a-151">Grundläggande, anonyma</span><span class="sxs-lookup"><span data-stu-id="7cd6a-151">Basic, Anonymous</span></span> |
| <span data-ttu-id="7cd6a-152">användarnamn</span><span class="sxs-lookup"><span data-stu-id="7cd6a-152">username</span></span> |<span data-ttu-id="7cd6a-153">Ange hello-användare som har åtkomst till toohello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-153">Specify hello user who has access toohello FTP server.</span></span> |<span data-ttu-id="7cd6a-154">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-154">No</span></span> |&nbsp; |
| <span data-ttu-id="7cd6a-155">lösenord</span><span class="sxs-lookup"><span data-stu-id="7cd6a-155">password</span></span> |<span data-ttu-id="7cd6a-156">Ange hello användarlösenord hello (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-156">Specify hello password for hello user (username).</span></span> |<span data-ttu-id="7cd6a-157">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-157">No</span></span> |&nbsp; |
| <span data-ttu-id="7cd6a-158">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="7cd6a-158">encryptedCredential</span></span> |<span data-ttu-id="7cd6a-159">Ange hello krypterade autentiseringsuppgifter tooaccess hello FTP-server.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-159">Specify hello encrypted credential tooaccess hello FTP server.</span></span> |<span data-ttu-id="7cd6a-160">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-160">No</span></span> |&nbsp; |
| <span data-ttu-id="7cd6a-161">gatewayName</span><span class="sxs-lookup"><span data-stu-id="7cd6a-161">gatewayName</span></span> |<span data-ttu-id="7cd6a-162">Ange hello namnet på hello gateway i Data Management Gateway tooconnect tooan lokalt FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-162">Specify hello name of hello gateway in Data Management Gateway tooconnect tooan on-premises FTP server.</span></span> |<span data-ttu-id="7cd6a-163">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-163">No</span></span> |&nbsp; |
| <span data-ttu-id="7cd6a-164">port</span><span class="sxs-lookup"><span data-stu-id="7cd6a-164">port</span></span> |<span data-ttu-id="7cd6a-165">Ange hello port på vilken hello FTP-servern lyssnar.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-165">Specify hello port on which hello FTP server is listening.</span></span> |<span data-ttu-id="7cd6a-166">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-166">No</span></span> |<span data-ttu-id="7cd6a-167">21</span><span class="sxs-lookup"><span data-stu-id="7cd6a-167">21</span></span> |
| <span data-ttu-id="7cd6a-168">enableSsl</span><span class="sxs-lookup"><span data-stu-id="7cd6a-168">enableSsl</span></span> |<span data-ttu-id="7cd6a-169">Ange om toouse FTP över SSL/TLS-kanal.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-169">Specify whether toouse FTP over an SSL/TLS channel.</span></span> |<span data-ttu-id="7cd6a-170">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-170">No</span></span> |<span data-ttu-id="7cd6a-171">SANT</span><span class="sxs-lookup"><span data-stu-id="7cd6a-171">true</span></span> |
| <span data-ttu-id="7cd6a-172">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="7cd6a-172">enableServerCertificateValidation</span></span> |<span data-ttu-id="7cd6a-173">Ange om tooenable server SSL-certifikatet verifieringen när du använder FTP över SSL/TLS-kanalen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-173">Specify whether tooenable server SSL certificate validation when you are using FTP over SSL/TLS channel.</span></span> |<span data-ttu-id="7cd6a-174">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-174">No</span></span> |<span data-ttu-id="7cd6a-175">SANT</span><span class="sxs-lookup"><span data-stu-id="7cd6a-175">true</span></span> |

### <a name="use-anonymous-authentication"></a><span data-ttu-id="7cd6a-176">Använd anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="7cd6a-176">Use Anonymous authentication</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="7cd6a-177">Använd användarnamn och lösenord i klartext för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="7cd6a-177">Use username and password in plain text for basic authentication</span></span>

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="7cd6a-178">Använd port, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="7cd6a-178">Use port, enableSsl, enableServerCertificateValidation</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="7cd6a-179">Använda encryptedCredential för autentisering och gateway</span><span class="sxs-lookup"><span data-stu-id="7cd6a-179">Use encryptedCredential for authentication and gateway</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="7cd6a-180">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="7cd6a-180">Dataset properties</span></span>
<span data-ttu-id="7cd6a-181">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-181">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="7cd6a-182">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-182">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="7cd6a-183">Hej **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-183">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="7cd6a-184">Den innehåller information som är specifik toohello dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-184">It provides information that is specific toohello dataset type.</span></span> <span data-ttu-id="7cd6a-185">Hej **typeProperties** avsnittet för en dataset av typen **filresursen** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="7cd6a-185">hello **typeProperties** section for a dataset of type **FileShare** has hello following properties:</span></span>

| <span data-ttu-id="7cd6a-186">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7cd6a-186">Property</span></span> | <span data-ttu-id="7cd6a-187">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7cd6a-187">Description</span></span> | <span data-ttu-id="7cd6a-188">Krävs</span><span class="sxs-lookup"><span data-stu-id="7cd6a-188">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7cd6a-189">folderPath</span><span class="sxs-lookup"><span data-stu-id="7cd6a-189">folderPath</span></span> |<span data-ttu-id="7cd6a-190">Undersökvägen toohello mapp.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-190">Subpath toohello folder.</span></span> <span data-ttu-id="7cd6a-191">Använda escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-191">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="7cd6a-192">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-192">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="7cd6a-193">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn start- och sluttider datum.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-193">You can combine this property with **partitionBy** toohave folder paths based on slice start and end date-times.</span></span> |<span data-ttu-id="7cd6a-194">Ja</span><span class="sxs-lookup"><span data-stu-id="7cd6a-194">Yes</span></span> |
| <span data-ttu-id="7cd6a-195">fileName</span><span class="sxs-lookup"><span data-stu-id="7cd6a-195">fileName</span></span> |<span data-ttu-id="7cd6a-196">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-196">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="7cd6a-197">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-197">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="7cd6a-198">När **fileName** har inte angetts för en datamängd för utdata hello hello genereras filen heter i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7cd6a-198">When **fileName** is not specified for an output dataset, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="7cd6a-199">Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-199">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="7cd6a-200">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-200">No</span></span> |
| <span data-ttu-id="7cd6a-201">fileFilter</span><span class="sxs-lookup"><span data-stu-id="7cd6a-201">fileFilter</span></span> |<span data-ttu-id="7cd6a-202">Ange ett filter toobe används tooselect en delmängd av filer i hello **folderPath**, i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-202">Specify a filter toobe used tooselect a subset of files in hello **folderPath**, rather than all files.</span></span><br/><br/><span data-ttu-id="7cd6a-203">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-203">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="7cd6a-204">Exempel 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="7cd6a-204">Example 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="7cd6a-205">Exempel 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="7cd6a-205">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="7cd6a-206">**fileFilter** gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-206">**fileFilter** is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="7cd6a-207">Den här egenskapen stöds inte med Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-207">This property is not supported with Hadoop Distributed File System (HDFS).</span></span> |<span data-ttu-id="7cd6a-208">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-208">No</span></span> |
| <span data-ttu-id="7cd6a-209">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="7cd6a-209">partitionedBy</span></span> |<span data-ttu-id="7cd6a-210">Används toospecify en dynamisk **folderPath** och **fileName** för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-210">Used toospecify a dynamic **folderPath** and **fileName** for time series data.</span></span> <span data-ttu-id="7cd6a-211">Du kan till exempel ange en **folderPath** som har fått parametrar för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-211">For example, you can specify a **folderPath** that is parameterized for every hour of data.</span></span> |<span data-ttu-id="7cd6a-212">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-212">No</span></span> |
| <span data-ttu-id="7cd6a-213">Format</span><span class="sxs-lookup"><span data-stu-id="7cd6a-213">format</span></span> | <span data-ttu-id="7cd6a-214">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-214">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="7cd6a-215">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-215">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="7cd6a-216">Mer information finns i hello [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-216">For more information, see hello [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="7cd6a-217">Om du vill toocopy filer som de är mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-217">If you want toocopy files as they are between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="7cd6a-218">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-218">No</span></span> |
| <span data-ttu-id="7cd6a-219">Komprimering</span><span class="sxs-lookup"><span data-stu-id="7cd6a-219">compression</span></span> | <span data-ttu-id="7cd6a-220">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-220">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="7cd6a-221">Typer som stöds är **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**, och stöds nivåerna **Optimal** och **snabbaste**.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-221">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**, and supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="7cd6a-222">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-222">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="7cd6a-223">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-223">No</span></span> |
| <span data-ttu-id="7cd6a-224">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="7cd6a-224">useBinaryTransfer</span></span> |<span data-ttu-id="7cd6a-225">Ange om toouse hello binary överföringsläge.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-225">Specify whether toouse hello binary transfer mode.</span></span> <span data-ttu-id="7cd6a-226">hello-värden är true för en binär (detta är standardvärdet för hello) och false för ASCII.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-226">hello values are true for binary mode (this is hello default value), and false for ASCII.</span></span> <span data-ttu-id="7cd6a-227">Den här egenskapen kan endast användas när hello associerade linked service-typen är av typen: FtpServer.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-227">This property can only be used when hello associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="7cd6a-228">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-228">No</span></span> |

> [!NOTE]
> <span data-ttu-id="7cd6a-229">**Filnamn** och **fileFilter** kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-229">**fileName** and **fileFilter** cannot be used simultaneously.</span></span>

### <a name="use-hello-partionedby-property"></a><span data-ttu-id="7cd6a-230">Använd hello partionedBy egenskapen</span><span class="sxs-lookup"><span data-stu-id="7cd6a-230">Use hello partionedBy property</span></span>
<span data-ttu-id="7cd6a-231">Som nämnts tidigare under hello, kan du ange en dynamisk **folderPath** och **fileName** för tid series-data med hello **partitionedBy** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-231">As mentioned in hello previous section, you can specify a dynamic **folderPath** and **fileName** for time series data with hello **partitionedBy** property.</span></span>

<span data-ttu-id="7cd6a-232">toolearn om tid serien datauppsättningar schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och körning](data-factory-scheduling-and-execution.md), och [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-232">toolearn about time series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="7cd6a-233">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="7cd6a-233">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="7cd6a-234">I det här exemplet {segment} ersätts med hello värdet för variabeln SliceStart Data Factory, i hello formatera angivna (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-234">In this example, {Slice} is replaced with hello value of Data Factory system variable SliceStart, in hello format specified (YYYYMMDDHH).</span></span> <span data-ttu-id="7cd6a-235">Hej SliceStart refererar toostart tiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-235">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="7cd6a-236">hello mappsökvägen är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-236">hello folder path is different for each slice.</span></span> <span data-ttu-id="7cd6a-237">(Till exempel 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-237">(For example, wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.)</span></span>

#### <a name="sample-2"></a><span data-ttu-id="7cd6a-238">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="7cd6a-238">Sample 2</span></span>

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
<span data-ttu-id="7cd6a-239">I det här exemplet hello år, månad, dag och tidpunkt för SliceStart extraheras till olika variabler som används av hello **folderPath** och **fileName** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-239">In this example, hello year, month, day, and time of SliceStart are extracted into separate variables that are used by hello **folderPath** and **fileName** properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="7cd6a-240">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="7cd6a-240">Copy activity properties</span></span>
<span data-ttu-id="7cd6a-241">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-241">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="7cd6a-242">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-242">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="7cd6a-243">Egenskaper som är tillgängliga i hello **typeProperties** avsnitt i hello aktivitet på hello andra sidan, varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-243">Properties available in hello **typeProperties** section of hello activity, on hello other hand, vary with each activity type.</span></span> <span data-ttu-id="7cd6a-244">För hello kopieringsaktiviteten varierar hello Typegenskaper beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-244">For hello copy activity, hello type properties vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="7cd6a-245">I en Kopieringsaktivitet när hello källa är av typen **FileSystemSource**, hello efter egenskapen finns i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7cd6a-245">In copy activity, when hello source is of type **FileSystemSource**, hello following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="7cd6a-246">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7cd6a-246">Property</span></span> | <span data-ttu-id="7cd6a-247">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7cd6a-247">Description</span></span> | <span data-ttu-id="7cd6a-248">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7cd6a-248">Allowed values</span></span> | <span data-ttu-id="7cd6a-249">Krävs</span><span class="sxs-lookup"><span data-stu-id="7cd6a-249">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7cd6a-250">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="7cd6a-250">recursive</span></span> |<span data-ttu-id="7cd6a-251">Anger om hello data läses rekursivt från hello undermappar eller endast från hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-251">Indicates whether hello data is read recursively from hello subfolders, or only from hello specified folder.</span></span> |<span data-ttu-id="7cd6a-252">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-252">True, False (default)</span></span> |<span data-ttu-id="7cd6a-253">Nej</span><span class="sxs-lookup"><span data-stu-id="7cd6a-253">No</span></span> |

## <a name="json-example-copy-data-from-ftp-server-tooazure-blob"></a><span data-ttu-id="7cd6a-254">JSON-exempel: kopiera data från FTP-servern tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="7cd6a-254">JSON example: Copy data from FTP server tooAzure Blob</span></span>
<span data-ttu-id="7cd6a-255">Det här exemplet visas hur toocopy data från en FTP-server tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-255">This sample shows how toocopy data from an FTP server tooAzure Blob storage.</span></span> <span data-ttu-id="7cd6a-256">Dock datan kan kopieras direkt tooany av hello egenskaperna anges i hello [datalager och format stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats), med hjälp av hello kopieringsaktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-256">However, data can be copied directly tooany of hello sinks stated in hello [supported data stores and formats](data-factory-data-movement-activities.md#supported-data-stores-and-formats), by using hello copy activity in Data Factory.</span></span>  

<span data-ttu-id="7cd6a-257">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="7cd6a-257">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span></span>

* <span data-ttu-id="7cd6a-258">En länkad tjänst av typen [FtpServer](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-258">A linked service of type [FtpServer](#linked-service-properties)</span></span>
* <span data-ttu-id="7cd6a-259">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-259">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="7cd6a-260">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-260">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties)</span></span>
* <span data-ttu-id="7cd6a-261">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-261">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="7cd6a-262">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-262">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="7cd6a-263">hello exemplet kopierar data från en FTP-server tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-263">hello sample copies data from an FTP server tooan Azure blob every hour.</span></span> <span data-ttu-id="7cd6a-264">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-264">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="ftp-linked-service"></a><span data-ttu-id="7cd6a-265">Länkad FTP-tjänsten</span><span class="sxs-lookup"><span data-stu-id="7cd6a-265">FTP linked service</span></span>

<span data-ttu-id="7cd6a-266">Det här exemplet använder grundläggande autentisering med hello användarnamn och lösenord i klartext.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-266">This example uses basic authentication, with hello user name and password in plain text.</span></span> <span data-ttu-id="7cd6a-267">Du kan också använda något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="7cd6a-267">You can also use one of hello following ways:</span></span>

* <span data-ttu-id="7cd6a-268">Anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="7cd6a-268">Anonymous authentication</span></span>
* <span data-ttu-id="7cd6a-269">Grundläggande autentisering med krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7cd6a-269">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="7cd6a-270">FTP över SSL/TLS (FTPS)</span><span class="sxs-lookup"><span data-stu-id="7cd6a-270">FTP over SSL/TLS (FTPS)</span></span>

<span data-ttu-id="7cd6a-271">Se hello [FTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-271">See hello [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
    }
  }
}
```
### <a name="azure-storage-linked-service"></a><span data-ttu-id="7cd6a-272">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="7cd6a-272">Azure Storage linked service</span></span>

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
### <a name="ftp-input-dataset"></a><span data-ttu-id="7cd6a-273">FTP-inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="7cd6a-273">FTP input dataset</span></span>

<span data-ttu-id="7cd6a-274">Den här datamängden refererar toohello FTP mappen `mysharedfolder` och `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-274">This dataset refers toohello FTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="7cd6a-275">hello pipeline kopierar hello filen toohello mål.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-275">hello pipeline copies hello file toohello destination.</span></span>

<span data-ttu-id="7cd6a-276">Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-276">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory, and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="7cd6a-277">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="7cd6a-277">Azure Blob output dataset</span></span>

<span data-ttu-id="7cd6a-278">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-278">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="7cd6a-279">hello mappsökväg för hello blob utvärderas dynamiskt, baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-279">hello folder path for hello blob is dynamically evaluated, based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="7cd6a-280">hello mappsökväg använder hello år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-280">hello folder path uses hello year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a><span data-ttu-id="7cd6a-281">En kopia aktivitet i en pipeline med filen system käll- och blob sink</span><span class="sxs-lookup"><span data-stu-id="7cd6a-281">A copy activity in a pipeline with file system source and blob sink</span></span>

<span data-ttu-id="7cd6a-282">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-282">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="7cd6a-283">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource**, och hello **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="7cd6a-283">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and hello **sink** type is set too**BlobSink**.</span></span>

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
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
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> <span data-ttu-id="7cd6a-284">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-284">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cd6a-285">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cd6a-285">Next steps</span></span>
<span data-ttu-id="7cd6a-286">Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="7cd6a-286">See hello following articles:</span></span>

* <span data-ttu-id="7cd6a-287">toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (kopieringsaktiviteten) i Data Factory och olika sätt toooptimize, se hello [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-287">toolearn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways toooptimize it, see hello [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="7cd6a-288">Stegvisa instruktioner för att skapa en pipeline med en kopieringsaktiviteten finns hello [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7cd6a-288">For step-by-step instructions for creating a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
