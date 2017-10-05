---
title: "Flytta data från en källa för HTTP - Azure | Microsoft Docs"
description: "Läs mer om hur du flyttar data från en lokal eller en källa för HTTP-av moln med Azure Data Factory."
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
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 3cc1bd293868b0bb093f617ac12e16c26780fc89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a><span data-ttu-id="6ed36-103">Flytta data från en HTTP-datakälla med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6ed36-103">Move data from an HTTP source using Azure Data Factory</span></span>
<span data-ttu-id="6ed36-104">Den här artikeln beskrivs hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en HTTP-slutpunkt på lokalt/i molnet till datakällan stöds sink.</span><span class="sxs-lookup"><span data-stu-id="6ed36-104">This article outlines how to use the Copy Activity in Azure Data Factory to move data from an on-premises/cloud HTTP endpoint to a supported sink data store.</span></span> <span data-ttu-id="6ed36-105">Den här artikeln bygger på den [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning kopieringsaktiviteten och listan över datakällor som stöds som källor/sänkor.</span><span class="sxs-lookup"><span data-stu-id="6ed36-105">This article builds on the [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and the list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="6ed36-106">Data factory stöder för närvarande endast flytta data från en HTTP-källa till andra databaser, men inte flytta data från andra data lagrar till en HTTP-mål.</span><span class="sxs-lookup"><span data-stu-id="6ed36-106">Data factory currently supports only moving data from an HTTP source to other data stores, but not moving data from other data stores to an HTTP destination.</span></span>

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="6ed36-107">Scenarier som stöds och typer av autentisering</span><span class="sxs-lookup"><span data-stu-id="6ed36-107">Supported scenarios and authentication types</span></span>
<span data-ttu-id="6ed36-108">Du kan använda den här HTTP-anslutningen för att hämta data från **både till molnet och lokala HTTP/s-slutpunkt** med hjälp av HTTP **hämta** eller **POST** metod.</span><span class="sxs-lookup"><span data-stu-id="6ed36-108">You can use this HTTP connector to retrieve data from **both cloud and on-premises HTTP/s endpoint** by using HTTP **GET** or **POST** method.</span></span> <span data-ttu-id="6ed36-109">Följande autentiseringstyper av som stöds: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, och **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-109">The following authentication types are supported: **Anonymous**, **Basic**, **Digest**, **Windows**, and **ClientCertificate**.</span></span> <span data-ttu-id="6ed36-110">Observera skillnaden mellan den här kopplingen och [Web tabell connector](data-factory-web-table-connector.md) är: används att extrahera innehållet från webbsidan HTML.</span><span class="sxs-lookup"><span data-stu-id="6ed36-110">Note the difference between this connector and the [Web table connector](data-factory-web-table-connector.md) is: the latter is used to extract table content from web HTML page.</span></span>

<span data-ttu-id="6ed36-111">När du kopierar data från en lokal HTTP-slutpunkt, måste du installera en Data Management Gateway i lokal miljö/Azure VM.</span><span class="sxs-lookup"><span data-stu-id="6ed36-111">When copying data from an on-premises HTTP endpoint, you need install a Data Management Gateway in the on-premises environment/Azure VM.</span></span> <span data-ttu-id="6ed36-112">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="6ed36-112">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="6ed36-113">Komma igång</span><span class="sxs-lookup"><span data-stu-id="6ed36-113">Getting started</span></span>
<span data-ttu-id="6ed36-114">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en HTTP-datakälla med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="6ed36-114">You can create a pipeline with a copy activity that moves data from an HTTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="6ed36-115">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-115">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="6ed36-116">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="6ed36-116">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

- <span data-ttu-id="6ed36-117">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-117">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="6ed36-118">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="6ed36-118">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> <span data-ttu-id="6ed36-119">JSON-exempel att kopiera data från http-källa till Azure Blob Storage finns [JSON-exempel](#json-examples) avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6ed36-119">For JSON samples to copy data from HTTP source to Azure Blob Storage, see [JSON examples](#json-examples) section of this articles.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="6ed36-120">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="6ed36-120">Linked service properties</span></span>
<span data-ttu-id="6ed36-121">Följande tabell innehåller beskrivning för JSON-element som är specifika för HTTP länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6ed36-121">The following table provides description for JSON elements specific to HTTP linked service.</span></span>

| <span data-ttu-id="6ed36-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6ed36-122">Property</span></span> | <span data-ttu-id="6ed36-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6ed36-123">Description</span></span> | <span data-ttu-id="6ed36-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="6ed36-124">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ed36-125">typ</span><span class="sxs-lookup"><span data-stu-id="6ed36-125">type</span></span> | <span data-ttu-id="6ed36-126">Egenskapen type måste anges till: `Http`.</span><span class="sxs-lookup"><span data-stu-id="6ed36-126">The type property must be set to: `Http`.</span></span> | <span data-ttu-id="6ed36-127">Ja</span><span class="sxs-lookup"><span data-stu-id="6ed36-127">Yes</span></span> |
| <span data-ttu-id="6ed36-128">URL: en</span><span class="sxs-lookup"><span data-stu-id="6ed36-128">url</span></span> | <span data-ttu-id="6ed36-129">Bas-URL till webbservern</span><span class="sxs-lookup"><span data-stu-id="6ed36-129">Base URL to the Web Server</span></span> | <span data-ttu-id="6ed36-130">Ja</span><span class="sxs-lookup"><span data-stu-id="6ed36-130">Yes</span></span> |
| <span data-ttu-id="6ed36-131">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="6ed36-131">authenticationType</span></span> | <span data-ttu-id="6ed36-132">Anger vilken autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="6ed36-132">Specifies the authentication type.</span></span> <span data-ttu-id="6ed36-133">Tillåtna värden är: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-133">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="6ed36-134">Avse respektive avsnitt under den här tabellen på fler egenskaper och JSON-exempel för dessa typer av autentisering.</span><span class="sxs-lookup"><span data-stu-id="6ed36-134">Refer to sections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="6ed36-135">Ja</span><span class="sxs-lookup"><span data-stu-id="6ed36-135">Yes</span></span> |
| <span data-ttu-id="6ed36-136">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="6ed36-136">enableServerCertificateValidation</span></span> | <span data-ttu-id="6ed36-137">Ange om du vill aktivera server SSL-certifikatsverifiering om datakällan är HTTPS-webbserver</span><span class="sxs-lookup"><span data-stu-id="6ed36-137">Specify whether to enable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="6ed36-138">Nej, standard är SANT</span><span class="sxs-lookup"><span data-stu-id="6ed36-138">No, default is true</span></span> |
| <span data-ttu-id="6ed36-139">gatewayName</span><span class="sxs-lookup"><span data-stu-id="6ed36-139">gatewayName</span></span> | <span data-ttu-id="6ed36-140">Namnet på Data Management Gateway för att ansluta till en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="6ed36-140">Name of the Data Management Gateway to connect to an on-premises HTTP source.</span></span> | <span data-ttu-id="6ed36-141">Ja om du kopierar data från en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="6ed36-141">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="6ed36-142">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="6ed36-142">encryptedCredential</span></span> | <span data-ttu-id="6ed36-143">Krypterade autentiseringsuppgifter till HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6ed36-143">Encrypted credential to access the HTTP endpoint.</span></span> <span data-ttu-id="6ed36-144">Genereras automatiskt när du konfigurerar autentiseringsinformation i guiden Kopiera eller ClickOnce popup-dialogruta.</span><span class="sxs-lookup"><span data-stu-id="6ed36-144">Auto-generated when you configure the authentication information in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="6ed36-145">Nej.</span><span class="sxs-lookup"><span data-stu-id="6ed36-145">No.</span></span> <span data-ttu-id="6ed36-146">Gäller bara när du kopierar data från en lokal HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="6ed36-146">Apply only when copying data from an on-premises HTTP server.</span></span> |

<span data-ttu-id="6ed36-147">Se [flytta data mellan lokala källor och molnet med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) mer information om hur du anger autentiseringsuppgifter för datakälla för lokala HTTP-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6ed36-147">See [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for details about setting credentials for on-premises HTTP connector data source.</span></span>

### <a name="using-basic-digest-or-windows-authentication"></a><span data-ttu-id="6ed36-148">Med hjälp av grundläggande, sammanfattad eller Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="6ed36-148">Using Basic, Digest, or Windows authentication</span></span>

<span data-ttu-id="6ed36-149">Ange `authenticationType` som `Basic`, `Digest`, eller `Windows`, och ange följande egenskaper utöver HTTP-anslutningen generiska de introduceras ovan:</span><span class="sxs-lookup"><span data-stu-id="6ed36-149">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="6ed36-150">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6ed36-150">Property</span></span> | <span data-ttu-id="6ed36-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6ed36-151">Description</span></span> | <span data-ttu-id="6ed36-152">Krävs</span><span class="sxs-lookup"><span data-stu-id="6ed36-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ed36-153">användarnamn</span><span class="sxs-lookup"><span data-stu-id="6ed36-153">username</span></span> | <span data-ttu-id="6ed36-154">Användarnamn för att få åtkomst till HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6ed36-154">Username to access the HTTP endpoint.</span></span> | <span data-ttu-id="6ed36-155">Ja</span><span class="sxs-lookup"><span data-stu-id="6ed36-155">Yes</span></span> |
| <span data-ttu-id="6ed36-156">lösenord</span><span class="sxs-lookup"><span data-stu-id="6ed36-156">password</span></span> | <span data-ttu-id="6ed36-157">Lösenord för användare (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="6ed36-157">Password for the user (username).</span></span> | <span data-ttu-id="6ed36-158">Ja</span><span class="sxs-lookup"><span data-stu-id="6ed36-158">Yes</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="6ed36-159">Exempel: med grundläggande, sammanfattad eller Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="6ed36-159">Example: using Basic, Digest, or Windows authentication</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a><span data-ttu-id="6ed36-160">Med hjälp av ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="6ed36-160">Using ClientCertificate authentication</span></span>

<span data-ttu-id="6ed36-161">Om du vill använda grundläggande autentisering, `authenticationType` som `ClientCertificate`, och ange följande egenskaper utöver HTTP-anslutningen generiska de introduceras ovan:</span><span class="sxs-lookup"><span data-stu-id="6ed36-161">To use basic authentication, set `authenticationType` as `ClientCertificate`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="6ed36-162">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6ed36-162">Property</span></span> | <span data-ttu-id="6ed36-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6ed36-163">Description</span></span> | <span data-ttu-id="6ed36-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="6ed36-164">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ed36-165">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="6ed36-165">embeddedCertData</span></span> | <span data-ttu-id="6ed36-166">Base64-kodade innehåll binära data i filen Personal Information Exchange (PFX).</span><span class="sxs-lookup"><span data-stu-id="6ed36-166">The Base64-encoded contents of binary data of the Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="6ed36-167">Ange antingen det `embeddedCertData` eller `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="6ed36-167">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="6ed36-168">certThumbprint</span><span class="sxs-lookup"><span data-stu-id="6ed36-168">certThumbprint</span></span> | <span data-ttu-id="6ed36-169">Tumavtryck för certifikat som har installerats på gateway-datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="6ed36-169">The thumbprint of the certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="6ed36-170">Gäller bara när du kopierar data från en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="6ed36-170">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="6ed36-171">Ange antingen det `embeddedCertData` eller `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="6ed36-171">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="6ed36-172">lösenord</span><span class="sxs-lookup"><span data-stu-id="6ed36-172">password</span></span> | <span data-ttu-id="6ed36-173">Lösenordet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="6ed36-173">Password associated with the certificate.</span></span> | <span data-ttu-id="6ed36-174">Nej</span><span class="sxs-lookup"><span data-stu-id="6ed36-174">No</span></span> |

<span data-ttu-id="6ed36-175">Om du använder `certThumbprint` för autentisering och certifikatet har installerats i det personliga arkivet i den lokala datorn, måste du bevilja läsbehörighet till gatewaytjänsten:</span><span class="sxs-lookup"><span data-stu-id="6ed36-175">If you use `certThumbprint` for authentication and the certificate is installed in the personal store of the local computer, you need to grant the read permission to the gateway service:</span></span>

1. <span data-ttu-id="6ed36-176">Starta Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="6ed36-176">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="6ed36-177">Lägg till den **certifikat** snapin-modulen som riktar sig den **lokal dator**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-177">Add the **Certificates** snap-in that targets the **Local Computer**.</span></span>
2. <span data-ttu-id="6ed36-178">Expandera **certifikat**, **personliga**, och klicka på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-178">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="6ed36-179">Högerklicka på certifikatet från det personliga arkivet och välj **alla aktiviteter**->**hantera privata nycklar...**</span><span class="sxs-lookup"><span data-stu-id="6ed36-179">Right-click the certificate from the personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="6ed36-180">På den **säkerhet** Lägg till användarkontot under vilken Data Management Gateway-värdtjänsten körs med läsbehörighet till certifikatet.</span><span class="sxs-lookup"><span data-stu-id="6ed36-180">On the **Security** tab, add the user account under which Data Management Gateway Host Service is running with the read access to the certificate.</span></span>  

#### <a name="example-using-client-certificate"></a><span data-ttu-id="6ed36-181">Exempel: använder klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="6ed36-181">Example: using client certificate</span></span>
<span data-ttu-id="6ed36-182">Det här länkade tjänsten länkar din data factory på en lokal webbserver för HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ed36-182">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="6ed36-183">Den använder ett klientcertifikat som är installerade på datorn med Data Management Gateway är installerad.</span><span class="sxs-lookup"><span data-stu-id="6ed36-183">It uses a client certificate that is installed on the machine with Data Management Gateway installed.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"

        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="6ed36-184">Exempel: använder klientcertifikat i en fil</span><span class="sxs-lookup"><span data-stu-id="6ed36-184">Example: using client certificate in a file</span></span>
<span data-ttu-id="6ed36-185">Det här länkade tjänsten länkar din data factory på en lokal webbserver för HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ed36-185">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="6ed36-186">Det använder en klient-certifikatfil på datorn med Data Management Gateway är installerad.</span><span class="sxs-lookup"><span data-stu-id="6ed36-186">It uses a client certificate file on the machine with Data Management Gateway installed.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="6ed36-187">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="6ed36-187">Dataset properties</span></span>
<span data-ttu-id="6ed36-188">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6ed36-188">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="6ed36-189">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="6ed36-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="6ed36-190">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="6ed36-190">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="6ed36-191">TypeProperties avsnittet för dataset av typen **Http** har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="6ed36-191">The typeProperties section for dataset of type **Http** has the following properties</span></span>

| <span data-ttu-id="6ed36-192">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6ed36-192">Property</span></span> | <span data-ttu-id="6ed36-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6ed36-193">Description</span></span> | <span data-ttu-id="6ed36-194">Krävs</span><span class="sxs-lookup"><span data-stu-id="6ed36-194">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6ed36-195">typ</span><span class="sxs-lookup"><span data-stu-id="6ed36-195">type</span></span> | <span data-ttu-id="6ed36-196">Ange vilken typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="6ed36-196">Specified the type of the dataset.</span></span> <span data-ttu-id="6ed36-197">måste anges till `Http`.</span><span class="sxs-lookup"><span data-stu-id="6ed36-197">must be set to `Http`.</span></span> | <span data-ttu-id="6ed36-198">Ja</span><span class="sxs-lookup"><span data-stu-id="6ed36-198">Yes</span></span> |
| <span data-ttu-id="6ed36-199">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="6ed36-199">relativeUrl</span></span> | <span data-ttu-id="6ed36-200">En relativ URL till den resurs som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="6ed36-200">A relative URL to the resource that contains the data.</span></span> <span data-ttu-id="6ed36-201">Om sökvägen inte anges används den URL som angavs i definitionen länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6ed36-201">When path is not specified, only the URL specified in the linked service definition is used.</span></span> <br><br> <span data-ttu-id="6ed36-202">Du kan använda för att skapa dynamiska URL [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md), t.ex. ”relativeUrl” ”: $$Text.Format ('/ min/rapporten? månad = {0:yyyy}-{0:MM} & fmt = csv', SliceStart)”.</span><span class="sxs-lookup"><span data-stu-id="6ed36-202">To construct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), e.g. "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)".</span></span> | <span data-ttu-id="6ed36-203">Nej</span><span class="sxs-lookup"><span data-stu-id="6ed36-203">No</span></span> |
| <span data-ttu-id="6ed36-204">requestMethod</span><span class="sxs-lookup"><span data-stu-id="6ed36-204">requestMethod</span></span> | <span data-ttu-id="6ed36-205">HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="6ed36-205">Http method.</span></span> <span data-ttu-id="6ed36-206">Tillåtna värden är **hämta** eller **efter**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-206">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="6ed36-207">Nej.</span><span class="sxs-lookup"><span data-stu-id="6ed36-207">No.</span></span> <span data-ttu-id="6ed36-208">Standardvärdet är `GET`.</span><span class="sxs-lookup"><span data-stu-id="6ed36-208">Default is `GET`.</span></span> |
| <span data-ttu-id="6ed36-209">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="6ed36-209">additionalHeaders</span></span> | <span data-ttu-id="6ed36-210">Ytterligare HTTP-begärans sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="6ed36-210">Additional HTTP request headers.</span></span> | <span data-ttu-id="6ed36-211">Nej</span><span class="sxs-lookup"><span data-stu-id="6ed36-211">No</span></span> |
| <span data-ttu-id="6ed36-212">requestBody</span><span class="sxs-lookup"><span data-stu-id="6ed36-212">requestBody</span></span> | <span data-ttu-id="6ed36-213">Brödtext för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="6ed36-213">Body for HTTP request.</span></span> | <span data-ttu-id="6ed36-214">Nej</span><span class="sxs-lookup"><span data-stu-id="6ed36-214">No</span></span> |
| <span data-ttu-id="6ed36-215">Format</span><span class="sxs-lookup"><span data-stu-id="6ed36-215">format</span></span> | <span data-ttu-id="6ed36-216">Om du vill bara **hämta data från HTTP-slutpunkt som-är** hoppa över den här formatinställningar utan parsning den.</span><span class="sxs-lookup"><span data-stu-id="6ed36-216">If you want to simply **retrieve the data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="6ed36-217">Om du vill att parsa innehållet i HTTP-svar vid kopiering, stöds följande format: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-217">If you want to parse the HTTP response content during copy, the following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="6ed36-218">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6ed36-218">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="6ed36-219">Nej</span><span class="sxs-lookup"><span data-stu-id="6ed36-219">No</span></span> |
| <span data-ttu-id="6ed36-220">Komprimering</span><span class="sxs-lookup"><span data-stu-id="6ed36-220">compression</span></span> | <span data-ttu-id="6ed36-221">Ange typ och kompression för data.</span><span class="sxs-lookup"><span data-stu-id="6ed36-221">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="6ed36-222">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-222">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="6ed36-223">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-223">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="6ed36-224">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="6ed36-224">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="6ed36-225">Nej</span><span class="sxs-lookup"><span data-stu-id="6ed36-225">No</span></span> |

### <a name="example-using-the-get-default-method"></a><span data-ttu-id="6ed36-226">Exempel: med metoden GET (standard)</span><span class="sxs-lookup"><span data-stu-id="6ed36-226">Example: using the GET (default) method</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

### <a name="example-using-the-post-method"></a><span data-ttu-id="6ed36-227">Exempel: med POST-metoden</span><span class="sxs-lookup"><span data-stu-id="6ed36-227">Example: using the POST method</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
           "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a><span data-ttu-id="6ed36-228">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="6ed36-228">Copy activity properties</span></span>
<span data-ttu-id="6ed36-229">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6ed36-229">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="6ed36-230">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6ed36-230">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="6ed36-231">Egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten å andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="6ed36-231">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="6ed36-232">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="6ed36-232">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="6ed36-233">För närvarande när datakällan i en Kopieringsaktivitet är av typen **HttpSource**, följande egenskaper stöds.</span><span class="sxs-lookup"><span data-stu-id="6ed36-233">Currently, when the source in copy activity is of type **HttpSource**, the following properties are supported.</span></span>

| <span data-ttu-id="6ed36-234">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6ed36-234">Property</span></span> | <span data-ttu-id="6ed36-235">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6ed36-235">Description</span></span> | <span data-ttu-id="6ed36-236">Krävs</span><span class="sxs-lookup"><span data-stu-id="6ed36-236">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="6ed36-237">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="6ed36-237">httpRequestTimeout</span></span> | <span data-ttu-id="6ed36-238">Tidsgräns (TimeSpan) för HTTP-begäran att få svar.</span><span class="sxs-lookup"><span data-stu-id="6ed36-238">The timeout (TimeSpan) for the HTTP request to get a response.</span></span> <span data-ttu-id="6ed36-239">Tidsgränsen är det inget svar timeout inte att läsa svarsdata.</span><span class="sxs-lookup"><span data-stu-id="6ed36-239">It is the timeout to get a response, not the timeout to read response data.</span></span> | <span data-ttu-id="6ed36-240">Nej.</span><span class="sxs-lookup"><span data-stu-id="6ed36-240">No.</span></span> <span data-ttu-id="6ed36-241">Standardvärde: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="6ed36-241">Default value: 00:01:40</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="6ed36-242">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="6ed36-242">Supported file and compression formats</span></span>
<span data-ttu-id="6ed36-243">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="6ed36-243">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="6ed36-244">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="6ed36-244">JSON examples</span></span>
<span data-ttu-id="6ed36-245">I följande exempel ger exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6ed36-245">The following example provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="6ed36-246">De visar hur du kopierar data från http-källa till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6ed36-246">They show how to copy data from HTTP source to Azure Blob Storage.</span></span> <span data-ttu-id="6ed36-247">Dock datan kan kopieras **direkt** från alla källor till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6ed36-247">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-http-source-to-azure-blob-storage"></a><span data-ttu-id="6ed36-248">Exempel: Kopiera data till Azure Blob Storage från http-källa</span><span class="sxs-lookup"><span data-stu-id="6ed36-248">Example: Copy data from HTTP source to Azure Blob Storage</span></span>
<span data-ttu-id="6ed36-249">Data Factory-lösningen för det här exemplet innehåller följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="6ed36-249">The Data Factory solution for this sample contains the following Data Factory entities:</span></span>

1. <span data-ttu-id="6ed36-250">En länkad tjänst av typen [HTTP](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6ed36-250">A linked service of type [HTTP](#linked-service-properties).</span></span>
2. <span data-ttu-id="6ed36-251">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6ed36-251">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="6ed36-252">Indata [dataset](data-factory-create-datasets.md) av typen [Http](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6ed36-252">An input [dataset](data-factory-create-datasets.md) of type [Http](#dataset-properties).</span></span>
4. <span data-ttu-id="6ed36-253">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6ed36-253">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="6ed36-254">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [HttpSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="6ed36-254">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [HttpSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="6ed36-255">Exemplet kopierar data från en HTTP-datakälla till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="6ed36-255">The sample copies data from an HTTP source to an Azure blob every hour.</span></span> <span data-ttu-id="6ed36-256">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6ed36-256">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="http-linked-service"></a><span data-ttu-id="6ed36-257">HTTP-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="6ed36-257">HTTP linked service</span></span>
<span data-ttu-id="6ed36-258">Det här exemplet använder HTTP-länkade tjänsten med anonym autentisering.</span><span class="sxs-lookup"><span data-stu-id="6ed36-258">This example uses the HTTP linked service with anonymous authentication.</span></span> <span data-ttu-id="6ed36-259">Se [HTTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="6ed36-259">See [HTTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "HttpLinkedService",
    "properties":
    {
        "type": "Http",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="6ed36-260">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="6ed36-260">Azure Storage linked service</span></span>

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

### <a name="http-input-dataset"></a><span data-ttu-id="6ed36-261">Inkommande HTTP-datamängd</span><span class="sxs-lookup"><span data-stu-id="6ed36-261">HTTP input dataset</span></span>
<span data-ttu-id="6ed36-262">Ange **externa** till **SANT** informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6ed36-262">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}

```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="6ed36-263">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6ed36-263">Azure Blob output dataset</span></span>

<span data-ttu-id="6ed36-264">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="6ed36-264">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="6ed36-265">Pipeline med kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="6ed36-265">Pipeline with Copy activity</span></span>

<span data-ttu-id="6ed36-266">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="6ed36-266">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="6ed36-267">I pipeline-JSON-definitionen av **källa** är inställd på **HttpSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="6ed36-267">In the pipeline JSON definition, the **source** type is set to **HttpSource** and **sink** type is set to **BlobSink**.</span></span>

<span data-ttu-id="6ed36-268">Se [HttpSource](#copy-activity-properties) lista över egenskaper som stöds av HttpSource.</span><span class="sxs-lookup"><span data-stu-id="6ed36-268">See [HttpSource](#copy-activity-properties) for the list of properties supported by the HttpSource.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "HttpSourceToAzureBlob",
        "description": "Copy from an HTTP source to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "HttpSourceDataInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "HttpSource"
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
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

> [!NOTE]
> <span data-ttu-id="6ed36-269">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="6ed36-269">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="6ed36-270">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="6ed36-270">Performance and Tuning</span></span>
<span data-ttu-id="6ed36-271">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="6ed36-271">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
