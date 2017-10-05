---
title: "Flytta data från en FTP-server med hjälp av Azure Data Factory | Microsoft Docs"
description: "Läs mer om hur du flyttar data från en FTP-server med hjälp av Azure Data Factory."
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
ms.openlocfilehash: f8f31f3a2ee02c964737dd32145499f3dcfd0624
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a><span data-ttu-id="46bdc-103">Flytta data från en FTP-server med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="46bdc-103">Move data from an FTP server by using Azure Data Factory</span></span>
<span data-ttu-id="46bdc-104">Den här artikeln förklarar hur du använder kopieringsaktiviteten i Azure Data Factory för att flytta data från en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="46bdc-104">This article explains how to use the copy activity in Azure Data Factory to move data from an FTP server.</span></span> <span data-ttu-id="46bdc-105">Den bygger på den [Data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="46bdc-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="46bdc-106">Du kan kopiera data från en FTP-server till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="46bdc-106">You can copy data from an FTP server to any supported sink data store.</span></span> <span data-ttu-id="46bdc-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="46bdc-107">For a list of data stores supported as sinks by the copy activity, see the [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="46bdc-108">Data Factory stöder för närvarande endast flytta data från en FTP-server till andra databaser, men inte flytta data från andra data lagras på en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="46bdc-108">Data Factory currently supports only moving data from an FTP server to other data stores, but not moving data from other data stores to an FTP server.</span></span> <span data-ttu-id="46bdc-109">Den stöder både lokalt och molntjänster FTP-servrar.</span><span class="sxs-lookup"><span data-stu-id="46bdc-109">It supports both on-premises and cloud FTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="46bdc-110">Kopieringsaktiviteten tar inte bort källfilen när den har kopierats till målet.</span><span class="sxs-lookup"><span data-stu-id="46bdc-110">The copy activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="46bdc-111">Om du vill ta bort källfilen efter en lyckad kopiering, skapa en anpassad aktivitet om du vill ta bort filen och använda aktiviteten i pipeline.</span><span class="sxs-lookup"><span data-stu-id="46bdc-111">If you need to delete the source file after a successful copy, create a custom activity to delete the file, and use the activity in the pipeline.</span></span> 

## <a name="enable-connectivity"></a><span data-ttu-id="46bdc-112">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="46bdc-112">Enable connectivity</span></span>
<span data-ttu-id="46bdc-113">Om du flyttar data från en **lokalt** FTP-servern till ett moln data lagra (till exempel till Azure Blob storage), installera och använda Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="46bdc-113">If you are moving data from an **on-premises** FTP server to a cloud data store (for example, to Azure Blob storage), install and use Data Management Gateway.</span></span> <span data-ttu-id="46bdc-114">Data Management Gateway är en klientagent som är installerad på den lokala datorn och ger molntjänster att ansluta till en lokal resurs.</span><span class="sxs-lookup"><span data-stu-id="46bdc-114">The Data Management Gateway is a client agent that is installed on your on-premises machine, and it allows cloud services to connect to an on-premises resource.</span></span> <span data-ttu-id="46bdc-115">Mer information finns i [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-115">For details, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="46bdc-116">För stegvisa instruktioner om hur du konfigurerar gatewayen och använder den, se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-116">For step-by-step instructions on setting up the gateway and using it, see [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="46bdc-117">Du kan använda gatewayen för att ansluta till en FTP-server, även om servern är på en Azure-infrastruktur som en tjänst (IaaS) virtuell dator (VM).</span><span class="sxs-lookup"><span data-stu-id="46bdc-117">You use the gateway to connect to an FTP server, even if the server is on an Azure infrastructure as a service (IaaS) virtual machine (VM).</span></span>

<span data-ttu-id="46bdc-118">Det är möjligt att installera gatewayen på samma lokala dator eller IaaS VM som FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="46bdc-118">It is possible to install the gateway on the same on-premises machine or IaaS VM as the FTP server.</span></span> <span data-ttu-id="46bdc-119">Vi rekommenderar dock att du installerar en gateway på en separat dator eller IaaS-VM att undvika resurskonflikter och bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="46bdc-119">However, we recommend that you install the gateway on a separate machine or IaaS VM to avoid resource contention, and for better performance.</span></span> <span data-ttu-id="46bdc-120">När du installerar en gateway på en separat dator kan ska datorn kunna komma åt FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="46bdc-120">When you install the gateway on a separate machine, the machine should be able to access the FTP server.</span></span>

## <a name="get-started"></a><span data-ttu-id="46bdc-121">Kom igång</span><span class="sxs-lookup"><span data-stu-id="46bdc-121">Get started</span></span>
<span data-ttu-id="46bdc-122">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en FTP-datakälla med hjälp av olika verktyg eller API: er.</span><span class="sxs-lookup"><span data-stu-id="46bdc-122">You can create a pipeline with a copy activity that moves data from an FTP source by using different tools or APIs.</span></span>

<span data-ttu-id="46bdc-123">Det enklaste sättet att skapa en pipeline är att använda den **Data Factory kopiera guiden**.</span><span class="sxs-lookup"><span data-stu-id="46bdc-123">The easiest way to create a pipeline is to use the **Data Factory Copy Wizard**.</span></span> <span data-ttu-id="46bdc-124">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång.</span><span class="sxs-lookup"><span data-stu-id="46bdc-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough.</span></span>

<span data-ttu-id="46bdc-125">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="46bdc-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="46bdc-126">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="46bdc-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="46bdc-127">Om du använder verktyg eller API: er, utför följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="46bdc-127">Whether you use the tools or APIs, perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="46bdc-128">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="46bdc-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="46bdc-129">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-129">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="46bdc-130">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="46bdc-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="46bdc-131">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="46bdc-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="46bdc-132">När du använder verktyg eller API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="46bdc-132">When you use tools or APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span> <span data-ttu-id="46bdc-133">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från en FTP-datalagret finns i [JSON-exempel: kopiera data från FTP-servern till Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="46bdc-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from an FTP data store, see the [JSON example: Copy data from FTP server to Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="46bdc-134">Mer information om format för stöds och komprimering ska användas finns [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-134">For details about supported file and compression formats to use, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="46bdc-135">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter för FTP.</span><span class="sxs-lookup"><span data-stu-id="46bdc-135">The following sections provide details about JSON properties that are used to define Data Factory entities specific to FTP.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="46bdc-136">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="46bdc-136">Linked service properties</span></span>
<span data-ttu-id="46bdc-137">I följande tabell beskrivs JSON-element som är specifika för en FTP-länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="46bdc-137">The following table describes JSON elements specific to an FTP linked service.</span></span>

| <span data-ttu-id="46bdc-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46bdc-138">Property</span></span> | <span data-ttu-id="46bdc-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46bdc-139">Description</span></span> | <span data-ttu-id="46bdc-140">Krävs</span><span class="sxs-lookup"><span data-stu-id="46bdc-140">Required</span></span> | <span data-ttu-id="46bdc-141">Standard</span><span class="sxs-lookup"><span data-stu-id="46bdc-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="46bdc-142">typ</span><span class="sxs-lookup"><span data-stu-id="46bdc-142">type</span></span> |<span data-ttu-id="46bdc-143">Ange FtpServer.</span><span class="sxs-lookup"><span data-stu-id="46bdc-143">Set this to FtpServer.</span></span> |<span data-ttu-id="46bdc-144">Ja</span><span class="sxs-lookup"><span data-stu-id="46bdc-144">Yes</span></span> |&nbsp; |
| <span data-ttu-id="46bdc-145">värden</span><span class="sxs-lookup"><span data-stu-id="46bdc-145">host</span></span> |<span data-ttu-id="46bdc-146">Ange namn eller IP-adressen till FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="46bdc-146">Specify the name or IP address of the FTP server.</span></span> |<span data-ttu-id="46bdc-147">Ja</span><span class="sxs-lookup"><span data-stu-id="46bdc-147">Yes</span></span> |&nbsp; |
| <span data-ttu-id="46bdc-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="46bdc-148">authenticationType</span></span> |<span data-ttu-id="46bdc-149">Ange vilken autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="46bdc-149">Specify the authentication type.</span></span> |<span data-ttu-id="46bdc-150">Ja</span><span class="sxs-lookup"><span data-stu-id="46bdc-150">Yes</span></span> |<span data-ttu-id="46bdc-151">Grundläggande, anonyma</span><span class="sxs-lookup"><span data-stu-id="46bdc-151">Basic, Anonymous</span></span> |
| <span data-ttu-id="46bdc-152">användarnamn</span><span class="sxs-lookup"><span data-stu-id="46bdc-152">username</span></span> |<span data-ttu-id="46bdc-153">Ange den användare som har åtkomst till FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="46bdc-153">Specify the user who has access to the FTP server.</span></span> |<span data-ttu-id="46bdc-154">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-154">No</span></span> |&nbsp; |
| <span data-ttu-id="46bdc-155">lösenord</span><span class="sxs-lookup"><span data-stu-id="46bdc-155">password</span></span> |<span data-ttu-id="46bdc-156">Ange lösenord för användaren (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="46bdc-156">Specify the password for the user (username).</span></span> |<span data-ttu-id="46bdc-157">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-157">No</span></span> |&nbsp; |
| <span data-ttu-id="46bdc-158">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="46bdc-158">encryptedCredential</span></span> |<span data-ttu-id="46bdc-159">Ange krypterade autentiseringsuppgifter för åtkomst till FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="46bdc-159">Specify the encrypted credential to access the FTP server.</span></span> |<span data-ttu-id="46bdc-160">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-160">No</span></span> |&nbsp; |
| <span data-ttu-id="46bdc-161">gatewayName</span><span class="sxs-lookup"><span data-stu-id="46bdc-161">gatewayName</span></span> |<span data-ttu-id="46bdc-162">Ange namnet på gatewayen i Data Management Gateway för att ansluta till en lokal FTP-server.</span><span class="sxs-lookup"><span data-stu-id="46bdc-162">Specify the name of the gateway in Data Management Gateway to connect to an on-premises FTP server.</span></span> |<span data-ttu-id="46bdc-163">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-163">No</span></span> |&nbsp; |
| <span data-ttu-id="46bdc-164">port</span><span class="sxs-lookup"><span data-stu-id="46bdc-164">port</span></span> |<span data-ttu-id="46bdc-165">Ange den port som FTP-servern lyssnar.</span><span class="sxs-lookup"><span data-stu-id="46bdc-165">Specify the port on which the FTP server is listening.</span></span> |<span data-ttu-id="46bdc-166">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-166">No</span></span> |<span data-ttu-id="46bdc-167">21</span><span class="sxs-lookup"><span data-stu-id="46bdc-167">21</span></span> |
| <span data-ttu-id="46bdc-168">enableSsl</span><span class="sxs-lookup"><span data-stu-id="46bdc-168">enableSsl</span></span> |<span data-ttu-id="46bdc-169">Ange om du använder FTP över SSL/TLS-kanalen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-169">Specify whether to use FTP over an SSL/TLS channel.</span></span> |<span data-ttu-id="46bdc-170">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-170">No</span></span> |<span data-ttu-id="46bdc-171">SANT</span><span class="sxs-lookup"><span data-stu-id="46bdc-171">true</span></span> |
| <span data-ttu-id="46bdc-172">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="46bdc-172">enableServerCertificateValidation</span></span> |<span data-ttu-id="46bdc-173">Ange om du vill aktivera server SSL-certifikatsverifiering när du använder FTP över SSL/TLS-kanalen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-173">Specify whether to enable server SSL certificate validation when you are using FTP over SSL/TLS channel.</span></span> |<span data-ttu-id="46bdc-174">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-174">No</span></span> |<span data-ttu-id="46bdc-175">SANT</span><span class="sxs-lookup"><span data-stu-id="46bdc-175">true</span></span> |

### <a name="use-anonymous-authentication"></a><span data-ttu-id="46bdc-176">Använd anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="46bdc-176">Use Anonymous authentication</span></span>

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

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="46bdc-177">Använd användarnamn och lösenord i klartext för grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="46bdc-177">Use username and password in plain text for basic authentication</span></span>

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

### <a name="use-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="46bdc-178">Använd port, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="46bdc-178">Use port, enableSsl, enableServerCertificateValidation</span></span>

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

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="46bdc-179">Använda encryptedCredential för autentisering och gateway</span><span class="sxs-lookup"><span data-stu-id="46bdc-179">Use encryptedCredential for authentication and gateway</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="46bdc-180">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="46bdc-180">Dataset properties</span></span>
<span data-ttu-id="46bdc-181">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-181">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="46bdc-182">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-182">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="46bdc-183">Den **typeProperties** avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-183">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="46bdc-184">Den innehåller information som är specifik för dataset-typen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-184">It provides information that is specific to the dataset type.</span></span> <span data-ttu-id="46bdc-185">Den **typeProperties** avsnittet för en dataset av typen **filresursen** har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="46bdc-185">The **typeProperties** section for a dataset of type **FileShare** has the following properties:</span></span>

| <span data-ttu-id="46bdc-186">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46bdc-186">Property</span></span> | <span data-ttu-id="46bdc-187">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46bdc-187">Description</span></span> | <span data-ttu-id="46bdc-188">Krävs</span><span class="sxs-lookup"><span data-stu-id="46bdc-188">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="46bdc-189">folderPath</span><span class="sxs-lookup"><span data-stu-id="46bdc-189">folderPath</span></span> |<span data-ttu-id="46bdc-190">Underordnad sökväg till mappen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-190">Subpath to the folder.</span></span> <span data-ttu-id="46bdc-191">Använda escape-tecknet ' \ ' för specialtecken i strängen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-191">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="46bdc-192">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="46bdc-192">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="46bdc-193">Du kan kombinera den här egenskapen med **partitionBy** har mappsökvägar baserat på sektorn start-och sluttider datum.</span><span class="sxs-lookup"><span data-stu-id="46bdc-193">You can combine this property with **partitionBy** to have folder paths based on slice start and end date-times.</span></span> |<span data-ttu-id="46bdc-194">Ja</span><span class="sxs-lookup"><span data-stu-id="46bdc-194">Yes</span></span> |
| <span data-ttu-id="46bdc-195">fileName</span><span class="sxs-lookup"><span data-stu-id="46bdc-195">fileName</span></span> |<span data-ttu-id="46bdc-196">Ange namnet på filen i den **folderPath** om du vill att referera till en viss fil i mappen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-196">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="46bdc-197">Om du inte anger något värde för den här egenskapen tabellen pekar på alla filer i mappen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-197">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="46bdc-198">När **fileName** har inte angetts för en datamängd för utdata, namnet på den genererade filen är i följande format:</span><span class="sxs-lookup"><span data-stu-id="46bdc-198">When **fileName** is not specified for an output dataset, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="46bdc-199">Data. <Guid>.txt (exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="46bdc-199">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="46bdc-200">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-200">No</span></span> |
| <span data-ttu-id="46bdc-201">fileFilter</span><span class="sxs-lookup"><span data-stu-id="46bdc-201">fileFilter</span></span> |<span data-ttu-id="46bdc-202">Ange ett filter som används för att välja en delmängd av filer i den **folderPath**, i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="46bdc-202">Specify a filter to be used to select a subset of files in the **folderPath**, rather than all files.</span></span><br/><br/><span data-ttu-id="46bdc-203">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="46bdc-203">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="46bdc-204">Exempel 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="46bdc-204">Example 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="46bdc-205">Exempel 2:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="46bdc-205">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="46bdc-206">**fileFilter** gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="46bdc-206">**fileFilter** is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="46bdc-207">Den här egenskapen stöds inte med Hadoop Distributed File System (HDFS).</span><span class="sxs-lookup"><span data-stu-id="46bdc-207">This property is not supported with Hadoop Distributed File System (HDFS).</span></span> |<span data-ttu-id="46bdc-208">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-208">No</span></span> |
| <span data-ttu-id="46bdc-209">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="46bdc-209">partitionedBy</span></span> |<span data-ttu-id="46bdc-210">Används för att ange en dynamisk **folderPath** och **fileName** för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="46bdc-210">Used to specify a dynamic **folderPath** and **fileName** for time series data.</span></span> <span data-ttu-id="46bdc-211">Du kan till exempel ange en **folderPath** som har fått parametrar för varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="46bdc-211">For example, you can specify a **folderPath** that is parameterized for every hour of data.</span></span> |<span data-ttu-id="46bdc-212">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-212">No</span></span> |
| <span data-ttu-id="46bdc-213">Format</span><span class="sxs-lookup"><span data-stu-id="46bdc-213">format</span></span> | <span data-ttu-id="46bdc-214">Följande format stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="46bdc-214">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="46bdc-215">Ange den **typen** egenskap under format till ett av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="46bdc-215">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="46bdc-216">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format ](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="46bdc-216">For more information, see the [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="46bdc-217">Om du vill kopiera filer som de är mellan filbaserade butiker (binär kopia) kan du hoppa över avsnittet format i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="46bdc-217">If you want to copy files as they are between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="46bdc-218">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-218">No</span></span> |
| <span data-ttu-id="46bdc-219">Komprimering</span><span class="sxs-lookup"><span data-stu-id="46bdc-219">compression</span></span> | <span data-ttu-id="46bdc-220">Ange typ och kompression för data.</span><span class="sxs-lookup"><span data-stu-id="46bdc-220">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="46bdc-221">Typer som stöds är **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**, och stöds nivåerna **Optimal** och **snabbaste**.</span><span class="sxs-lookup"><span data-stu-id="46bdc-221">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**, and supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="46bdc-222">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="46bdc-222">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="46bdc-223">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-223">No</span></span> |
| <span data-ttu-id="46bdc-224">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="46bdc-224">useBinaryTransfer</span></span> |<span data-ttu-id="46bdc-225">Ange om du vill använda binär överföringsläget.</span><span class="sxs-lookup"><span data-stu-id="46bdc-225">Specify whether to use the binary transfer mode.</span></span> <span data-ttu-id="46bdc-226">Värdena är true för en binär (detta är standardvärdet) och false för ASCII.</span><span class="sxs-lookup"><span data-stu-id="46bdc-226">The values are true for binary mode (this is the default value), and false for ASCII.</span></span> <span data-ttu-id="46bdc-227">Den här egenskapen kan endast användas när den associera länkade tjänsttypen är av typen: FtpServer.</span><span class="sxs-lookup"><span data-stu-id="46bdc-227">This property can only be used when the associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="46bdc-228">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-228">No</span></span> |

> [!NOTE]
> <span data-ttu-id="46bdc-229">**Filnamn** och **fileFilter** kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="46bdc-229">**fileName** and **fileFilter** cannot be used simultaneously.</span></span>

### <a name="use-the-partionedby-property"></a><span data-ttu-id="46bdc-230">Använd egenskapen partionedBy</span><span class="sxs-lookup"><span data-stu-id="46bdc-230">Use the partionedBy property</span></span>
<span data-ttu-id="46bdc-231">Som nämnts ovan kan du ange en dynamisk **folderPath** och **fileName** för tid series-data med den **partitionedBy** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-231">As mentioned in the previous section, you can specify a dynamic **folderPath** and **fileName** for time series data with the **partitionedBy** property.</span></span>

<span data-ttu-id="46bdc-232">Läs om tid serien datauppsättningar schemaläggning och segment i [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och körning](data-factory-scheduling-and-execution.md), och [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-232">To learn about time series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="46bdc-233">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="46bdc-233">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="46bdc-234">I det här exemplet {segment} ersätts med värdet från Data Factory systemvariabeln SliceStart, i det format som specificerats (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="46bdc-234">In this example, {Slice} is replaced with the value of Data Factory system variable SliceStart, in the format specified (YYYYMMDDHH).</span></span> <span data-ttu-id="46bdc-235">SliceStart refererar till starttid av sektorn.</span><span class="sxs-lookup"><span data-stu-id="46bdc-235">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="46bdc-236">Sökvägen till mappen är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="46bdc-236">The folder path is different for each slice.</span></span> <span data-ttu-id="46bdc-237">(Till exempel 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.)</span><span class="sxs-lookup"><span data-stu-id="46bdc-237">(For example, wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.)</span></span>

#### <a name="sample-2"></a><span data-ttu-id="46bdc-238">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="46bdc-238">Sample 2</span></span>

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
<span data-ttu-id="46bdc-239">I det här exemplet år, månad, dag och tid för SliceStart extraheras till olika variabler som används av den **folderPath** och **fileName** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="46bdc-239">In this example, the year, month, day, and time of SliceStart are extracted into separate variables that are used by the **folderPath** and **fileName** properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="46bdc-240">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="46bdc-240">Copy activity properties</span></span>
<span data-ttu-id="46bdc-241">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter, se [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-241">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="46bdc-242">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="46bdc-242">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="46bdc-243">Egenskaper som är tillgängliga i den **typeProperties** avsnitt av aktivitet, å andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="46bdc-243">Properties available in the **typeProperties** section of the activity, on the other hand, vary with each activity type.</span></span> <span data-ttu-id="46bdc-244">För kopieringsaktiviteten varierar Typegenskaper beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="46bdc-244">For the copy activity, the type properties vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="46bdc-245">I en Kopieringsaktivitet när källan är av typen **FileSystemSource**, av följande egenskap är tillgänglig i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="46bdc-245">In copy activity, when the source is of type **FileSystemSource**, the following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="46bdc-246">Egenskap</span><span class="sxs-lookup"><span data-stu-id="46bdc-246">Property</span></span> | <span data-ttu-id="46bdc-247">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="46bdc-247">Description</span></span> | <span data-ttu-id="46bdc-248">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="46bdc-248">Allowed values</span></span> | <span data-ttu-id="46bdc-249">Krävs</span><span class="sxs-lookup"><span data-stu-id="46bdc-249">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="46bdc-250">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="46bdc-250">recursive</span></span> |<span data-ttu-id="46bdc-251">Anger om data läses rekursivt från undermapparna eller endast från den angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="46bdc-251">Indicates whether the data is read recursively from the subfolders, or only from the specified folder.</span></span> |<span data-ttu-id="46bdc-252">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="46bdc-252">True, False (default)</span></span> |<span data-ttu-id="46bdc-253">Nej</span><span class="sxs-lookup"><span data-stu-id="46bdc-253">No</span></span> |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a><span data-ttu-id="46bdc-254">JSON-exempel: kopiera data från FTP-servern till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="46bdc-254">JSON example: Copy data from FTP server to Azure Blob</span></span>
<span data-ttu-id="46bdc-255">Det här exemplet visas hur du kopierar data från en FTP-server till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="46bdc-255">This sample shows how to copy data from an FTP server to Azure Blob storage.</span></span> <span data-ttu-id="46bdc-256">Dock datan kan kopieras direkt till någon av sänkor som anges i den [datalager och format stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats), med hjälp av kopieringsaktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="46bdc-256">However, data can be copied directly to any of the sinks stated in the [supported data stores and formats](data-factory-data-movement-activities.md#supported-data-stores-and-formats), by using the copy activity in Data Factory.</span></span>  

<span data-ttu-id="46bdc-257">Följande exempel ger exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="46bdc-257">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span></span>

* <span data-ttu-id="46bdc-258">En länkad tjänst av typen [FtpServer](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="46bdc-258">A linked service of type [FtpServer](#linked-service-properties)</span></span>
* <span data-ttu-id="46bdc-259">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="46bdc-259">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="46bdc-260">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="46bdc-260">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties)</span></span>
* <span data-ttu-id="46bdc-261">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="46bdc-261">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="46bdc-262">En [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="46bdc-262">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="46bdc-263">Exemplet kopierar data från en FTP-server till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="46bdc-263">The sample copies data from an FTP server to an Azure blob every hour.</span></span> <span data-ttu-id="46bdc-264">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="46bdc-264">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="ftp-linked-service"></a><span data-ttu-id="46bdc-265">Länkad FTP-tjänsten</span><span class="sxs-lookup"><span data-stu-id="46bdc-265">FTP linked service</span></span>

<span data-ttu-id="46bdc-266">Det här exemplet använder grundläggande autentisering med användarnamn och lösenord i klartext.</span><span class="sxs-lookup"><span data-stu-id="46bdc-266">This example uses basic authentication, with the user name and password in plain text.</span></span> <span data-ttu-id="46bdc-267">Du kan också använda något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="46bdc-267">You can also use one of the following ways:</span></span>

* <span data-ttu-id="46bdc-268">Anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="46bdc-268">Anonymous authentication</span></span>
* <span data-ttu-id="46bdc-269">Grundläggande autentisering med krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="46bdc-269">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="46bdc-270">FTP över SSL/TLS (FTPS)</span><span class="sxs-lookup"><span data-stu-id="46bdc-270">FTP over SSL/TLS (FTPS)</span></span>

<span data-ttu-id="46bdc-271">Finns det [FTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="46bdc-271">See the [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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
### <a name="azure-storage-linked-service"></a><span data-ttu-id="46bdc-272">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="46bdc-272">Azure Storage linked service</span></span>

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
### <a name="ftp-input-dataset"></a><span data-ttu-id="46bdc-273">FTP-inkommande dataset</span><span class="sxs-lookup"><span data-stu-id="46bdc-273">FTP input dataset</span></span>

<span data-ttu-id="46bdc-274">Den här datamängden refererar till FTP-mappen `mysharedfolder` och `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="46bdc-274">This dataset refers to the FTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="46bdc-275">Pipelinen kopierar filen till målet.</span><span class="sxs-lookup"><span data-stu-id="46bdc-275">The pipeline copies the file to the destination.</span></span>

<span data-ttu-id="46bdc-276">Ange **externa** till **SANT** informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="46bdc-276">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory, and is not produced by an activity in the data factory.</span></span>

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="46bdc-277">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="46bdc-277">Azure Blob output dataset</span></span>

<span data-ttu-id="46bdc-278">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="46bdc-278">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="46bdc-279">Sökvägen till mappen för blobben utvärderas dynamiskt, baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="46bdc-279">The folder path for the blob is dynamically evaluated, based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="46bdc-280">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="46bdc-280">The folder path uses the year, month, day, and hours parts of the start time.</span></span>

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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a><span data-ttu-id="46bdc-281">En kopia aktivitet i en pipeline med filen system käll- och blob sink</span><span class="sxs-lookup"><span data-stu-id="46bdc-281">A copy activity in a pipeline with file system source and blob sink</span></span>

<span data-ttu-id="46bdc-282">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="46bdc-282">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="46bdc-283">I pipeline-JSON-definitionen av **källa** är inställd på **FileSystemSource**, och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="46bdc-283">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and the **sink** type is set to **BlobSink**.</span></span>

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
> <span data-ttu-id="46bdc-284">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-284">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="46bdc-285">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46bdc-285">Next steps</span></span>
<span data-ttu-id="46bdc-286">Se följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="46bdc-286">See the following articles:</span></span>

* <span data-ttu-id="46bdc-287">Mer information om viktiga faktorer som påverkan prestanda för flytt av data (kopieringsaktiviteten) i Data Factory och olika sätt att optimera den finns i [kopiera aktivitet prestanda och prestandajustering guiden](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-287">To learn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways to optimize it, see the [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="46bdc-288">Stegvisa instruktioner för att skapa en pipeline med kopiera aktiviteter finns i [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="46bdc-288">For step-by-step instructions for creating a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
