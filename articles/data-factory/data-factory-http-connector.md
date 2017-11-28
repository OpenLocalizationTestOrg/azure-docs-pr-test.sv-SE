---
title: "aaaMove data från en källa för HTTP - Azure | Microsoft Docs"
description: "Läs mer om hur toomove från en lokal eller ett moln HTTP datakälla med hjälp av Azure Data Factory."
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
ms.openlocfilehash: e39b9cbff870aef4be91938cacff39a2fd12d64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-http-source-using-azure-data-factory"></a><span data-ttu-id="6c391-103">Flytta data från en HTTP-datakälla med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6c391-103">Move data from an HTTP source using Azure Data Factory</span></span>
<span data-ttu-id="6c391-104">Den här artikeln beskrivs hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en på lokalt/i molnet http-slutpunkt tooa stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="6c391-104">This article outlines how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises/cloud HTTP endpoint tooa supported sink data store.</span></span> <span data-ttu-id="6c391-105">Den här artikeln bygger på hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med kopiera aktivitet och hello lista över datakällor som stöds som källor/sänkor.</span><span class="sxs-lookup"><span data-stu-id="6c391-105">This article builds on hello [data movement activities](data-factory-data-movement-activities.md) article that presents a general overview of data movement with copy activity and hello list of data stores supported as sources/sinks.</span></span>

<span data-ttu-id="6c391-106">Data factory för närvarande stöder endast flyttning av data från en HTTP datakällan tooother datalager, men inte flytta data från andra data lagrar tooan HTTP-mål.</span><span class="sxs-lookup"><span data-stu-id="6c391-106">Data factory currently supports only moving data from an HTTP source tooother data stores, but not moving data from other data stores tooan HTTP destination.</span></span>

## <a name="supported-scenarios-and-authentication-types"></a><span data-ttu-id="6c391-107">Scenarier som stöds och typer av autentisering</span><span class="sxs-lookup"><span data-stu-id="6c391-107">Supported scenarios and authentication types</span></span>
<span data-ttu-id="6c391-108">Du kan använda den här HTTP-anslutningen tooretrieve data från **både till molnet och lokala HTTP/s-slutpunkt** med hjälp av HTTP **hämta** eller **POST** metod.</span><span class="sxs-lookup"><span data-stu-id="6c391-108">You can use this HTTP connector tooretrieve data from **both cloud and on-premises HTTP/s endpoint** by using HTTP **GET** or **POST** method.</span></span> <span data-ttu-id="6c391-109">hello följande autentiseringstyper som stöds: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, och  **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="6c391-109">hello following authentication types are supported: **Anonymous**, **Basic**, **Digest**, **Windows**, and **ClientCertificate**.</span></span> <span data-ttu-id="6c391-110">Observera hello skillnaden mellan den här kopplingen och hello [Web tabell connector](data-factory-web-table-connector.md) är: hello senare är används tooextract tabell innehåll från HTML-sidan.</span><span class="sxs-lookup"><span data-stu-id="6c391-110">Note hello difference between this connector and hello [Web table connector](data-factory-web-table-connector.md) is: hello latter is used tooextract table content from web HTML page.</span></span>

<span data-ttu-id="6c391-111">När du kopierar data från en lokal HTTP-slutpunkt, måste du installera en Data Management Gateway i hello lokala miljö/Azure VM.</span><span class="sxs-lookup"><span data-stu-id="6c391-111">When copying data from an on-premises HTTP endpoint, you need install a Data Management Gateway in hello on-premises environment/Azure VM.</span></span> <span data-ttu-id="6c391-112">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="6c391-112">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span>

## <a name="getting-started"></a><span data-ttu-id="6c391-113">Komma igång</span><span class="sxs-lookup"><span data-stu-id="6c391-113">Getting started</span></span>
<span data-ttu-id="6c391-114">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en HTTP-datakälla med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="6c391-114">You can create a pipeline with a copy activity that moves data from an HTTP source by using different tools/APIs.</span></span>

- <span data-ttu-id="6c391-115">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="6c391-115">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="6c391-116">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="6c391-116">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

- <span data-ttu-id="6c391-117">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="6c391-117">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="6c391-118">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="6c391-118">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> <span data-ttu-id="6c391-119">JSON-exempel toocopy data från http-källa tooAzure Blob Storage, finns [JSON-exempel](#json-examples) avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6c391-119">For JSON samples toocopy data from HTTP source tooAzure Blob Storage, see [JSON examples](#json-examples) section of this articles.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="6c391-120">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="6c391-120">Linked service properties</span></span>
<span data-ttu-id="6c391-121">hello följande tabell innehåller en beskrivning för JSON-element specifika tooHTTP länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="6c391-121">hello following table provides description for JSON elements specific tooHTTP linked service.</span></span>

| <span data-ttu-id="6c391-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6c391-122">Property</span></span> | <span data-ttu-id="6c391-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c391-123">Description</span></span> | <span data-ttu-id="6c391-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="6c391-124">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c391-125">typ</span><span class="sxs-lookup"><span data-stu-id="6c391-125">type</span></span> | <span data-ttu-id="6c391-126">hello Typegenskapen måste anges till: `Http`.</span><span class="sxs-lookup"><span data-stu-id="6c391-126">hello type property must be set to: `Http`.</span></span> | <span data-ttu-id="6c391-127">Ja</span><span class="sxs-lookup"><span data-stu-id="6c391-127">Yes</span></span> |
| <span data-ttu-id="6c391-128">URL: en</span><span class="sxs-lookup"><span data-stu-id="6c391-128">url</span></span> | <span data-ttu-id="6c391-129">Bas-URL: en toohello webbserver</span><span class="sxs-lookup"><span data-stu-id="6c391-129">Base URL toohello Web Server</span></span> | <span data-ttu-id="6c391-130">Ja</span><span class="sxs-lookup"><span data-stu-id="6c391-130">Yes</span></span> |
| <span data-ttu-id="6c391-131">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="6c391-131">authenticationType</span></span> | <span data-ttu-id="6c391-132">Anger hello autentiseringstyp.</span><span class="sxs-lookup"><span data-stu-id="6c391-132">Specifies hello authentication type.</span></span> <span data-ttu-id="6c391-133">Tillåtna värden är: **anonym**, **grundläggande**, **sammanfattad**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="6c391-133">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="6c391-134">Läs respektive toosections under den här tabellen på fler egenskaper och JSON-exempel för dessa typer av autentisering.</span><span class="sxs-lookup"><span data-stu-id="6c391-134">Refer toosections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="6c391-135">Ja</span><span class="sxs-lookup"><span data-stu-id="6c391-135">Yes</span></span> |
| <span data-ttu-id="6c391-136">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="6c391-136">enableServerCertificateValidation</span></span> | <span data-ttu-id="6c391-137">Ange om tooenable server SSL-certifikatverifieringen om datakällan är HTTPS-webbserver</span><span class="sxs-lookup"><span data-stu-id="6c391-137">Specify whether tooenable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="6c391-138">Nej, standard är SANT</span><span class="sxs-lookup"><span data-stu-id="6c391-138">No, default is true</span></span> |
| <span data-ttu-id="6c391-139">gatewayName</span><span class="sxs-lookup"><span data-stu-id="6c391-139">gatewayName</span></span> | <span data-ttu-id="6c391-140">Namnet på hello Data Management Gateway tooconnect tooan lokalt http-källa.</span><span class="sxs-lookup"><span data-stu-id="6c391-140">Name of hello Data Management Gateway tooconnect tooan on-premises HTTP source.</span></span> | <span data-ttu-id="6c391-141">Ja om du kopierar data från en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="6c391-141">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="6c391-142">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="6c391-142">encryptedCredential</span></span> | <span data-ttu-id="6c391-143">Krypterade autentiseringsuppgifter tooaccess hello HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6c391-143">Encrypted credential tooaccess hello HTTP endpoint.</span></span> <span data-ttu-id="6c391-144">Genereras automatiskt när du konfigurerar hello autentiseringsinformation i Kopiera guiden eller hello ClickOnce popup-dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c391-144">Auto-generated when you configure hello authentication information in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="6c391-145">Nej.</span><span class="sxs-lookup"><span data-stu-id="6c391-145">No.</span></span> <span data-ttu-id="6c391-146">Gäller bara när du kopierar data från en lokal HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="6c391-146">Apply only when copying data from an on-premises HTTP server.</span></span> |

<span data-ttu-id="6c391-147">Se [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) mer information om hur du anger autentiseringsuppgifter för datakälla för lokala HTTP-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6c391-147">See [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for details about setting credentials for on-premises HTTP connector data source.</span></span>

### <a name="using-basic-digest-or-windows-authentication"></a><span data-ttu-id="6c391-148">Med hjälp av grundläggande, sammanfattad eller Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="6c391-148">Using Basic, Digest, or Windows authentication</span></span>

<span data-ttu-id="6c391-149">Ange `authenticationType` som `Basic`, `Digest`, eller `Windows`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:</span><span class="sxs-lookup"><span data-stu-id="6c391-149">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="6c391-150">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6c391-150">Property</span></span> | <span data-ttu-id="6c391-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c391-151">Description</span></span> | <span data-ttu-id="6c391-152">Krävs</span><span class="sxs-lookup"><span data-stu-id="6c391-152">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c391-153">användarnamn</span><span class="sxs-lookup"><span data-stu-id="6c391-153">username</span></span> | <span data-ttu-id="6c391-154">Användarnamnet tooaccess hello HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6c391-154">Username tooaccess hello HTTP endpoint.</span></span> | <span data-ttu-id="6c391-155">Ja</span><span class="sxs-lookup"><span data-stu-id="6c391-155">Yes</span></span> |
| <span data-ttu-id="6c391-156">lösenord</span><span class="sxs-lookup"><span data-stu-id="6c391-156">password</span></span> | <span data-ttu-id="6c391-157">Lösenordet för hello (användarnamn).</span><span class="sxs-lookup"><span data-stu-id="6c391-157">Password for hello user (username).</span></span> | <span data-ttu-id="6c391-158">Ja</span><span class="sxs-lookup"><span data-stu-id="6c391-158">Yes</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="6c391-159">Exempel: med grundläggande, sammanfattad eller Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="6c391-159">Example: using Basic, Digest, or Windows authentication</span></span>

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

### <a name="using-clientcertificate-authentication"></a><span data-ttu-id="6c391-160">Med hjälp av ClientCertificate autentisering</span><span class="sxs-lookup"><span data-stu-id="6c391-160">Using ClientCertificate authentication</span></span>

<span data-ttu-id="6c391-161">toouse grundläggande autentisering, ange `authenticationType` som `ClientCertificate`, och ange följande egenskaper utöver hello HTTP-anslutningen generiska de som introducerats ovan hello:</span><span class="sxs-lookup"><span data-stu-id="6c391-161">toouse basic authentication, set `authenticationType` as `ClientCertificate`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="6c391-162">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6c391-162">Property</span></span> | <span data-ttu-id="6c391-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c391-163">Description</span></span> | <span data-ttu-id="6c391-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="6c391-164">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6c391-165">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="6c391-165">embeddedCertData</span></span> | <span data-ttu-id="6c391-166">hello Base64-kodade innehåll för binära data i hello Personal Information Exchange (PFX)-fil.</span><span class="sxs-lookup"><span data-stu-id="6c391-166">hello Base64-encoded contents of binary data of hello Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="6c391-167">Ange antingen hello `embeddedCertData` eller `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="6c391-167">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="6c391-168">certThumbprint</span><span class="sxs-lookup"><span data-stu-id="6c391-168">certThumbprint</span></span> | <span data-ttu-id="6c391-169">Hej tumavtrycket för certifikatet för hello som har installerats på gateway-datorns certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="6c391-169">hello thumbprint of hello certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="6c391-170">Gäller bara när du kopierar data från en lokal http-källa.</span><span class="sxs-lookup"><span data-stu-id="6c391-170">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="6c391-171">Ange antingen hello `embeddedCertData` eller `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="6c391-171">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="6c391-172">lösenord</span><span class="sxs-lookup"><span data-stu-id="6c391-172">password</span></span> | <span data-ttu-id="6c391-173">Lösenordet som är associerat med hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="6c391-173">Password associated with hello certificate.</span></span> | <span data-ttu-id="6c391-174">Nej</span><span class="sxs-lookup"><span data-stu-id="6c391-174">No</span></span> |

<span data-ttu-id="6c391-175">Om du använder `certThumbprint` för autentisering och hello certifikat installeras i hello personliga arkivet i hello lokala datorn, behöver du toogrant hello läsbehörighet toohello gateway-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="6c391-175">If you use `certThumbprint` for authentication and hello certificate is installed in hello personal store of hello local computer, you need toogrant hello read permission toohello gateway service:</span></span>

1. <span data-ttu-id="6c391-176">Starta Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="6c391-176">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="6c391-177">Lägg till hello **certifikat** snapin-modulen som mål hello **lokal dator**.</span><span class="sxs-lookup"><span data-stu-id="6c391-177">Add hello **Certificates** snap-in that targets hello **Local Computer**.</span></span>
2. <span data-ttu-id="6c391-178">Expandera **certifikat**, **personliga**, och klicka på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="6c391-178">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="6c391-179">Högerklicka på hello certifikatet från datorarkivet hello och välj **alla aktiviteter**->**hantera privata nycklar...**</span><span class="sxs-lookup"><span data-stu-id="6c391-179">Right-click hello certificate from hello personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="6c391-180">På hello **säkerhet** lägger du till hello-användarkonto som Data Management Gateway-värdtjänsten körs under med hello läsbehörighet toohello certifikat.</span><span class="sxs-lookup"><span data-stu-id="6c391-180">On hello **Security** tab, add hello user account under which Data Management Gateway Host Service is running with hello read access toohello certificate.</span></span>  

#### <a name="example-using-client-certificate"></a><span data-ttu-id="6c391-181">Exempel: använder klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="6c391-181">Example: using client certificate</span></span>
<span data-ttu-id="6c391-182">Det här länkade tjänsten länkar din data factory tooan lokalt HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="6c391-182">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="6c391-183">Den använder ett klientcertifikat som är installerad på datorn hello med Data Management Gateway är installerad.</span><span class="sxs-lookup"><span data-stu-id="6c391-183">It uses a client certificate that is installed on hello machine with Data Management Gateway installed.</span></span>

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

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="6c391-184">Exempel: använder klientcertifikat i en fil</span><span class="sxs-lookup"><span data-stu-id="6c391-184">Example: using client certificate in a file</span></span>
<span data-ttu-id="6c391-185">Det här länkade tjänsten länkar din data factory tooan lokalt HTTP-server.</span><span class="sxs-lookup"><span data-stu-id="6c391-185">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="6c391-186">Det använder en klient-certifikatfil på hello datorn med Data Management Gateway är installerad.</span><span class="sxs-lookup"><span data-stu-id="6c391-186">It uses a client certificate file on hello machine with Data Management Gateway installed.</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="6c391-187">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="6c391-187">Dataset properties</span></span>
<span data-ttu-id="6c391-188">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6c391-188">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="6c391-189">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="6c391-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="6c391-190">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="6c391-190">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="6c391-191">Hej typeProperties avsnittet för dataset av typen **Http** har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="6c391-191">hello typeProperties section for dataset of type **Http** has hello following properties</span></span>

| <span data-ttu-id="6c391-192">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6c391-192">Property</span></span> | <span data-ttu-id="6c391-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c391-193">Description</span></span> | <span data-ttu-id="6c391-194">Krävs</span><span class="sxs-lookup"><span data-stu-id="6c391-194">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6c391-195">typ</span><span class="sxs-lookup"><span data-stu-id="6c391-195">type</span></span> | <span data-ttu-id="6c391-196">Ange hello typ av hello dataset.</span><span class="sxs-lookup"><span data-stu-id="6c391-196">Specified hello type of hello dataset.</span></span> <span data-ttu-id="6c391-197">måste anges för`Http`.</span><span class="sxs-lookup"><span data-stu-id="6c391-197">must be set too`Http`.</span></span> | <span data-ttu-id="6c391-198">Ja</span><span class="sxs-lookup"><span data-stu-id="6c391-198">Yes</span></span> |
| <span data-ttu-id="6c391-199">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="6c391-199">relativeUrl</span></span> | <span data-ttu-id="6c391-200">En relativ URL toohello resurs som innehåller hello data.</span><span class="sxs-lookup"><span data-stu-id="6c391-200">A relative URL toohello resource that contains hello data.</span></span> <span data-ttu-id="6c391-201">Om sökvägen inte anges används endast hello-URL som anges i tjänstdefinitionen hello länkad.</span><span class="sxs-lookup"><span data-stu-id="6c391-201">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> <br><br> <span data-ttu-id="6c391-202">dynamisk tooconstruct-URL som du kan använda [Data Factory-funktioner och systemvariabler](data-factory-functions-variables.md), t.ex. ”relativeUrl” ”: $$Text.Format ('/ min/rapporten? månad = {0:yyyy}-{0:MM} & fmt = csv', SliceStart)”.</span><span class="sxs-lookup"><span data-stu-id="6c391-202">tooconstruct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), e.g. "relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)".</span></span> | <span data-ttu-id="6c391-203">Nej</span><span class="sxs-lookup"><span data-stu-id="6c391-203">No</span></span> |
| <span data-ttu-id="6c391-204">requestMethod</span><span class="sxs-lookup"><span data-stu-id="6c391-204">requestMethod</span></span> | <span data-ttu-id="6c391-205">HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="6c391-205">Http method.</span></span> <span data-ttu-id="6c391-206">Tillåtna värden är **hämta** eller **efter**.</span><span class="sxs-lookup"><span data-stu-id="6c391-206">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="6c391-207">Nej.</span><span class="sxs-lookup"><span data-stu-id="6c391-207">No.</span></span> <span data-ttu-id="6c391-208">Standardvärdet är `GET`.</span><span class="sxs-lookup"><span data-stu-id="6c391-208">Default is `GET`.</span></span> |
| <span data-ttu-id="6c391-209">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="6c391-209">additionalHeaders</span></span> | <span data-ttu-id="6c391-210">Ytterligare HTTP-begärans sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="6c391-210">Additional HTTP request headers.</span></span> | <span data-ttu-id="6c391-211">Nej</span><span class="sxs-lookup"><span data-stu-id="6c391-211">No</span></span> |
| <span data-ttu-id="6c391-212">requestBody</span><span class="sxs-lookup"><span data-stu-id="6c391-212">requestBody</span></span> | <span data-ttu-id="6c391-213">Brödtext för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="6c391-213">Body for HTTP request.</span></span> | <span data-ttu-id="6c391-214">Nej</span><span class="sxs-lookup"><span data-stu-id="6c391-214">No</span></span> |
| <span data-ttu-id="6c391-215">Format</span><span class="sxs-lookup"><span data-stu-id="6c391-215">format</span></span> | <span data-ttu-id="6c391-216">Om du vill toosimply **hämta hello data från HTTP-slutpunkt som-är** hoppa över den här formatinställningar utan parsning den.</span><span class="sxs-lookup"><span data-stu-id="6c391-216">If you want toosimply **retrieve hello data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="6c391-217">Om du vill tooparse hello HTTP-svar innehåll vid kopiering, hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="6c391-217">If you want tooparse hello HTTP response content during copy, hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="6c391-218">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6c391-218">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="6c391-219">Nej</span><span class="sxs-lookup"><span data-stu-id="6c391-219">No</span></span> |
| <span data-ttu-id="6c391-220">Komprimering</span><span class="sxs-lookup"><span data-stu-id="6c391-220">compression</span></span> | <span data-ttu-id="6c391-221">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="6c391-221">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="6c391-222">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="6c391-222">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="6c391-223">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="6c391-223">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="6c391-224">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="6c391-224">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="6c391-225">Nej</span><span class="sxs-lookup"><span data-stu-id="6c391-225">No</span></span> |

### <a name="example-using-hello-get-default-method"></a><span data-ttu-id="6c391-226">Exempel: genom att använda metoden för hello GET (standard)</span><span class="sxs-lookup"><span data-stu-id="6c391-226">Example: using hello GET (default) method</span></span>

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

### <a name="example-using-hello-post-method"></a><span data-ttu-id="6c391-227">Exempel: använda hello POST-metoden</span><span class="sxs-lookup"><span data-stu-id="6c391-227">Example: using hello POST method</span></span>

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

## <a name="copy-activity-properties"></a><span data-ttu-id="6c391-228">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="6c391-228">Copy activity properties</span></span>
<span data-ttu-id="6c391-229">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6c391-229">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="6c391-230">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principen är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6c391-230">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="6c391-231">Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="6c391-231">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="6c391-232">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="6c391-232">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="6c391-233">För närvarande när hello-källan i en Kopieringsaktivitet är av typen **HttpSource**, hello följande egenskaper stöds.</span><span class="sxs-lookup"><span data-stu-id="6c391-233">Currently, when hello source in copy activity is of type **HttpSource**, hello following properties are supported.</span></span>

| <span data-ttu-id="6c391-234">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6c391-234">Property</span></span> | <span data-ttu-id="6c391-235">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c391-235">Description</span></span> | <span data-ttu-id="6c391-236">Krävs</span><span class="sxs-lookup"><span data-stu-id="6c391-236">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="6c391-237">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="6c391-237">httpRequestTimeout</span></span> | <span data-ttu-id="6c391-238">Hej timeout (TimeSpan) för hello HTTP-begäran tooget ett svar.</span><span class="sxs-lookup"><span data-stu-id="6c391-238">hello timeout (TimeSpan) for hello HTTP request tooget a response.</span></span> <span data-ttu-id="6c391-239">Det är hello timeout tooget ett svar inte hello timeout tooread svarsdata.</span><span class="sxs-lookup"><span data-stu-id="6c391-239">It is hello timeout tooget a response, not hello timeout tooread response data.</span></span> | <span data-ttu-id="6c391-240">Nej.</span><span class="sxs-lookup"><span data-stu-id="6c391-240">No.</span></span> <span data-ttu-id="6c391-241">Standardvärde: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="6c391-241">Default value: 00:01:40</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="6c391-242">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="6c391-242">Supported file and compression formats</span></span>
<span data-ttu-id="6c391-243">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="6c391-243">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples"></a><span data-ttu-id="6c391-244">JSON-exempel</span><span class="sxs-lookup"><span data-stu-id="6c391-244">JSON examples</span></span>
<span data-ttu-id="6c391-245">följande exempel hello ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6c391-245">hello following example provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="6c391-246">De visar hur toocopy från http-datakällan tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6c391-246">They show how toocopy data from HTTP source tooAzure Blob Storage.</span></span> <span data-ttu-id="6c391-247">Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6c391-247">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-http-source-tooazure-blob-storage"></a><span data-ttu-id="6c391-248">Exempel: Kopiera data från http-källa tooAzure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="6c391-248">Example: Copy data from HTTP source tooAzure Blob Storage</span></span>
<span data-ttu-id="6c391-249">hello Data Factory-lösning för det här exemplet innehåller hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="6c391-249">hello Data Factory solution for this sample contains hello following Data Factory entities:</span></span>

1. <span data-ttu-id="6c391-250">En länkad tjänst av typen [HTTP](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6c391-250">A linked service of type [HTTP](#linked-service-properties).</span></span>
2. <span data-ttu-id="6c391-251">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6c391-251">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="6c391-252">Indata [dataset](data-factory-create-datasets.md) av typen [Http](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6c391-252">An input [dataset](data-factory-create-datasets.md) of type [Http](#dataset-properties).</span></span>
4. <span data-ttu-id="6c391-253">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6c391-253">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="6c391-254">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [HttpSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="6c391-254">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [HttpSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="6c391-255">hello exemplet kopierar data från en HTTP-källa tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="6c391-255">hello sample copies data from an HTTP source tooan Azure blob every hour.</span></span> <span data-ttu-id="6c391-256">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6c391-256">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="http-linked-service"></a><span data-ttu-id="6c391-257">HTTP-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="6c391-257">HTTP linked service</span></span>
<span data-ttu-id="6c391-258">Det här exemplet använder hello HTTP länkade tjänsten med anonym autentisering.</span><span class="sxs-lookup"><span data-stu-id="6c391-258">This example uses hello HTTP linked service with anonymous authentication.</span></span> <span data-ttu-id="6c391-259">Se [HTTP länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="6c391-259">See [HTTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="6c391-260">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="6c391-260">Azure Storage linked service</span></span>

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

### <a name="http-input-dataset"></a><span data-ttu-id="6c391-261">Inkommande HTTP-datamängd</span><span class="sxs-lookup"><span data-stu-id="6c391-261">HTTP input dataset</span></span>
<span data-ttu-id="6c391-262">Ange **externa** för**SANT** informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="6c391-262">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="6c391-263">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c391-263">Azure Blob output dataset</span></span>

<span data-ttu-id="6c391-264">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="6c391-264">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span>

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

### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="6c391-265">Pipeline med kopieringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="6c391-265">Pipeline with Copy activity</span></span>

<span data-ttu-id="6c391-266">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="6c391-266">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="6c391-267">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**HttpSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="6c391-267">In hello pipeline JSON definition, hello **source** type is set too**HttpSource** and **sink** type is set too**BlobSink**.</span></span>

<span data-ttu-id="6c391-268">Se [HttpSource](#copy-activity-properties) hello lista över egenskaper som stöds av hello HttpSource.</span><span class="sxs-lookup"><span data-stu-id="6c391-268">See [HttpSource](#copy-activity-properties) for hello list of properties supported by hello HttpSource.</span></span>

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
        "description": "Copy from an HTTP source tooan Azure blob",
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
> <span data-ttu-id="6c391-269">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="6c391-269">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="6c391-270">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="6c391-270">Performance and Tuning</span></span>
<span data-ttu-id="6c391-271">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="6c391-271">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
