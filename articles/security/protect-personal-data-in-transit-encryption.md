---
title: "Skydda personliga data under överföringen med kryptering i Azure | Microsoft Docs"
description: "Använder kryptering i Azure för att skydda personliga data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 99e40b8a09a2f151e7f83fbb58bdfc00ae4b1268
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a><span data-ttu-id="83659-103">Azure krypteringstekniker: skydda personliga data under överföringen med kryptering</span><span class="sxs-lookup"><span data-stu-id="83659-103">Azure encryption technologies: Protect personal data in transit with encryption</span></span>

<span data-ttu-id="83659-104">Den här artikeln hjälper dig att förstå och använda Azure krypteringstekniker att skydda data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="83659-104">This article will help you understand and use Azure encryption technologies to secure data in transit.</span></span> 

<span data-ttu-id="83659-105">Skydd av personliga data över nätverket är en viktig del av en flera lager skydd på djupet säkerhetsstrategi.</span><span class="sxs-lookup"><span data-stu-id="83659-105">Protecting the privacy of personal data as it travels across the network is an essential part of a multi-layered defense-in-depth security strategy.</span></span> <span data-ttu-id="83659-106">Kryptering under överföring är utformat för att hindra en angripare fångar upp överföringar från att visa eller använda data.</span><span class="sxs-lookup"><span data-stu-id="83659-106">Encryption in transit is designed to prevent an attacker who intercepts transmissions from being able to view or use the data.</span></span>

## <a name="scenario"></a><span data-ttu-id="83659-107">Scenario</span><span class="sxs-lookup"><span data-stu-id="83659-107">Scenario</span></span>

<span data-ttu-id="83659-108">Ett stort kryssning företag, sitt säte i USA utökar åtgärderna för att erbjuda färdvägar i Medelhavet, Adriatiska havet, baltiska havet samt Förenta staterna.</span><span class="sxs-lookup"><span data-stu-id="83659-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="83659-109">För att stödja dessa ansträngningar genererade den flera mindre kryssning rader i Italien, Tyskland, Danmark och Storbritannien</span><span class="sxs-lookup"><span data-stu-id="83659-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span> 

<span data-ttu-id="83659-110">Företaget använder Microsoft Azure för att lagra företagets data i molnet.</span><span class="sxs-lookup"><span data-stu-id="83659-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="83659-111">Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas.</span><span class="sxs-lookup"><span data-stu-id="83659-111">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="83659-112">Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser.</span><span class="sxs-lookup"><span data-stu-id="83659-112">It also includes traditional Human Resource information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="83659-113">Raden kryssning har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter för att spåra relationer med aktuella och tidigare kunder.</span><span class="sxs-lookup"><span data-stu-id="83659-113">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

<span data-ttu-id="83659-114">Personliga data för kunder som har angetts i databasen från företagets fjärranslutna kontor och från resa agenter finns runtom i världen.</span><span class="sxs-lookup"><span data-stu-id="83659-114">Personal data of customers is entered in the database from the company’s remote offices and from travel agents located around the world.</span></span> <span data-ttu-id="83659-115">Dokument som innehåller kundinformation överförs via nätverket till Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="83659-115">Documents containing customer information are transferred across the network to Azure storage.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="83659-116">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="83659-116">Problem statement</span></span>

<span data-ttu-id="83659-117">Företaget måste skydda kundernas och anställdas personliga data när den är i överföringen till och från Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="83659-117">The company must protect the privacy of customers’ and employees’ personal data while it is in transit to and from Azure services.</span></span>

## <a name="company-goal"></a><span data-ttu-id="83659-118">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="83659-118">Company goal</span></span>

<span data-ttu-id="83659-119">Företagets målet så att personliga data krypteras när av disken.</span><span class="sxs-lookup"><span data-stu-id="83659-119">The company goal to ensure that personal data is encrypted when off disk.</span></span> <span data-ttu-id="83659-120">Om obehöriga avlyssna av disk personliga data, måste den vara i ett formulär som kommer att visas inte kan läsas.</span><span class="sxs-lookup"><span data-stu-id="83659-120">If unauthorized persons intercept the off-disk personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="83659-121">Tillämpa kryptering ska vara enkelt eller helt transparent för användare och administratörer.</span><span class="sxs-lookup"><span data-stu-id="83659-121">Applying encryption should be easy, or completely transparent, for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="83659-122">Lösningar</span><span class="sxs-lookup"><span data-stu-id="83659-122">Solutions</span></span>

<span data-ttu-id="83659-123">Azure-tjänster ger flera verktyg och tekniker som hjälper dig att skydda personliga data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="83659-123">Azure services provide multiple tools and technologies to help you protect personal data in transit.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="83659-124">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="83659-124">Azure Storage</span></span>

<span data-ttu-id="83659-125">Data som lagras i molnet måste åka till klienten och som kan vara fysiskt finns var som helst i världen, till Azure-datacentret.</span><span class="sxs-lookup"><span data-stu-id="83659-125">Data that is stored in the cloud must travel from the client, which can be physically located anywhere in the world, to the Azure data center.</span></span> <span data-ttu-id="83659-126">När dessa data hämtas av användare, flyttar du det igen i motsatt riktning.</span><span class="sxs-lookup"><span data-stu-id="83659-126">When that data is retrieved by users, it travels again, in the opposite direction.</span></span> <span data-ttu-id="83659-127">Data som är under överföring via det offentliga Internet är alltid i fara för avlyssning av angripare.</span><span class="sxs-lookup"><span data-stu-id="83659-127">Data that is in transit over the public Internet is always at risk of interception by attackers.</span></span> <span data-ttu-id="83659-128">Det är viktigt att skydda personliga data med hjälp av kryptering på transportnivå för att skydda den som flyttas mellan platser.</span><span class="sxs-lookup"><span data-stu-id="83659-128">It is important to protect the privacy of personal data by using transport-level encryption to secure it as it moves between locations.</span></span>

<span data-ttu-id="83659-129">HTTPS-protokollet ger en säker och krypterad kommunikationskanal via Internet.</span><span class="sxs-lookup"><span data-stu-id="83659-129">The HTTPS protocol provides a secure, encrypted communications channel over the Internet.</span></span> <span data-ttu-id="83659-130">HTTPS ska användas för åtkomst till objekt i Azure Storage och vid anrop av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="83659-130">HTTPS should be used to access objects in Azure Storage and when calling REST APIs.</span></span> <span data-ttu-id="83659-131">Du kan framtvinga användning av HTTPS-protokollet när du använder [signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) för att delegera åtkomst till Azure Storage-objekt.</span><span class="sxs-lookup"><span data-stu-id="83659-131">You enforce use of the HTTPS protocol when using [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) to delegate access to Azure Storage objects.</span></span> <span data-ttu-id="83659-132">Det finns två typer av SAS: tjänst-SAS och konto-SAS.</span><span class="sxs-lookup"><span data-stu-id="83659-132">There are two types of SAS: Service SAS and Account SAS.</span></span>

#### <a name="how-do-i-construct-a-service-sas"></a><span data-ttu-id="83659-133">Hur jag för att skapa en tjänst-SAS?</span><span class="sxs-lookup"><span data-stu-id="83659-133">How do I construct a Service SAS?</span></span>

<span data-ttu-id="83659-134">En tjänst-SAS delegerar åtkomst till en resurs i en av lagringstjänsterna (blob, kö-, tabell- eller filen service).</span><span class="sxs-lookup"><span data-stu-id="83659-134">A Service SAS delegates access to a resource in just one of the storage services (blob, queue, table or file service).</span></span> <span data-ttu-id="83659-135">Om du vill skapa en tjänst-SAS gör du följande:</span><span class="sxs-lookup"><span data-stu-id="83659-135">To construct a Service SAS, do the following:</span></span>

1. <span data-ttu-id="83659-136">Ange fältet signerade Version</span><span class="sxs-lookup"><span data-stu-id="83659-136">Specify the Signed Version Field</span></span>

2. <span data-ttu-id="83659-137">Ange den signera resursen (Blob och fil-tjänsten)</span><span class="sxs-lookup"><span data-stu-id="83659-137">Specify the Signed Resource (Blob and File Service Only)</span></span>

3. <span data-ttu-id="83659-138">Ange frågeparametrar om du vill åsidosätta svarshuvuden (Blob-tjänsten och fil-tjänsten)</span><span class="sxs-lookup"><span data-stu-id="83659-138">Specify Query Parameters to Override Response Headers (Blob Service and File Service Only)</span></span>

4. <span data-ttu-id="83659-139">Ange tabellnamnet på (endast Tabelltjänsten)</span><span class="sxs-lookup"><span data-stu-id="83659-139">Specify the Table Name (Table Service Only)</span></span>

5. <span data-ttu-id="83659-140">Ange åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="83659-140">Specify the Access Policy</span></span>

6. <span data-ttu-id="83659-141">Ange giltighetsintervall signatur</span><span class="sxs-lookup"><span data-stu-id="83659-141">Specify the Signature Validity Interval</span></span>

8. <span data-ttu-id="83659-142">Ange behörigheter</span><span class="sxs-lookup"><span data-stu-id="83659-142">Specify Permissions</span></span>

9. <span data-ttu-id="83659-143">Ange IP-adress eller IP-intervall</span><span class="sxs-lookup"><span data-stu-id="83659-143">Specify IP Address or IP Range</span></span>

10. <span data-ttu-id="83659-144">Ange HTTP-protokollet</span><span class="sxs-lookup"><span data-stu-id="83659-144">Specify the HTTP Protocol</span></span>

11. <span data-ttu-id="83659-145">Ange intervall för åtkomst av tabell</span><span class="sxs-lookup"><span data-stu-id="83659-145">Specify Table Access Ranges</span></span>

12. <span data-ttu-id="83659-146">Ange den signerade identifieraren</span><span class="sxs-lookup"><span data-stu-id="83659-146">Specify the Signed Identifier</span></span>

13. <span data-ttu-id="83659-147">Ange signaturen</span><span class="sxs-lookup"><span data-stu-id="83659-147">Specify the Signature</span></span>

<span data-ttu-id="83659-148">Mer instruktioner finns [hur du skapar en tjänst-SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span><span class="sxs-lookup"><span data-stu-id="83659-148">For more detailed instructions, see [Constructing a Service SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span></span>

#### <a name="how-do-i-construct-an-account-sas"></a><span data-ttu-id="83659-149">Hur jag för att skapa ett konto-SAS?</span><span class="sxs-lookup"><span data-stu-id="83659-149">How do I construct an Account SAS?</span></span>

<span data-ttu-id="83659-150">En konto-SAS delegerar åtkomst till resurser i en eller flera av lagringstjänsterna.</span><span class="sxs-lookup"><span data-stu-id="83659-150">An Account SAS delegates access to resources in one or more of the storage services.</span></span> <span data-ttu-id="83659-151">Du kan också delegera åtkomst till läs-, skriv- och borttagningsåtgärder i blobbbehållare, tabeller, köer och filresurser som inte tillåts med en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="83659-151">You can also delegate access to read, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="83659-152">Skapa ett konto-SAS liknar som en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="83659-152">Construction of an Account SAS is similar to that of a Service SAS.</span></span> <span data-ttu-id="83659-153">Detaljerade instruktioner finns [hur du skapar ett konto-SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span><span class="sxs-lookup"><span data-stu-id="83659-153">For detailed instructions, see [Constructing an Account SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span></span>

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a><span data-ttu-id="83659-154">Hur behöver jag använda HTTPS vid anrop av REST API: er?</span><span class="sxs-lookup"><span data-stu-id="83659-154">How do I enforce HTTPS when calling REST APIs?</span></span>

<span data-ttu-id="83659-155">Om du vill framtvinga användning av HTTPS vid anrop av REST API: er för åtkomst till objekt i storage-konton, kan du aktivera säker överföring krävs för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83659-155">To enforce the use of HTTPS when calling REST APIs to access objects in storage accounts, you can enable Secure Transfer Required for the storage account.</span></span> 

1. <span data-ttu-id="83659-156">Välj i Azure-portalen **skapa Lagringskonto**, eller ett befintligt lagringskonto, Välj **inställningar** och sedan **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="83659-156">In the Azure portal, select **Create Storage Account**, or for an existing storage account, select **Settings** and then **Configuration**.</span></span>

2. <span data-ttu-id="83659-157">Under **säker överföring krävs**väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="83659-157">Under **Secure Transfer Required**, select **Enabled**.</span></span>

![Skapa ett lagringskonto](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

<span data-ttu-id="83659-159">Detaljerade instruktioner, inklusive hur du ger säker överföring krävs programmässigt, se [kräver säker överföring](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span><span class="sxs-lookup"><span data-stu-id="83659-159">For more detailed instructions, including how to enable Secure Transfer Required programmatically, see [Require Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span></span>

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a><span data-ttu-id="83659-160">Hur jag för att kryptera data i Azure File Storage?</span><span class="sxs-lookup"><span data-stu-id="83659-160">How do I encrypt data in Azure File Storage?</span></span>

<span data-ttu-id="83659-161">Kryptera data under överföring med [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), du kan använda SMB 3.x med Windows 8, 8.1 och 10 och Windows Server 2012 R2 och Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="83659-161">To encrypt data in transit with [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), you can use SMB 3.x with Windows 8, 8.1, and 10 and with Windows Server 2012 R2 and Windows Server 2016.</span></span> <span data-ttu-id="83659-162">När du använder tjänsten Azure-filer, misslyckas en anslutning utan kryptering när ”säker överföring krävs” är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="83659-162">When you are using the Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="83659-163">Detta inkluderar scenarier med hjälp av SMB 2.1 och SMB 3.0 utan kryptering vissa varianter av Linux SMB-klienten.</span><span class="sxs-lookup"><span data-stu-id="83659-163">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of the Linux SMB client.</span></span>

#### <a name="azure-client-side-encryption"></a><span data-ttu-id="83659-164">Azure klientsidan kryptering</span><span class="sxs-lookup"><span data-stu-id="83659-164">Azure Client-Side Encryption</span></span>

<span data-ttu-id="83659-165">Ett annat alternativ för att skydda personliga data medan de överförs mellan ett klientprogram och Azure Storage är [Client side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span><span class="sxs-lookup"><span data-stu-id="83659-165">Another option for protecting personal data while it’s being transferred between a client application and Azure Storage is [Client-side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span></span> <span data-ttu-id="83659-166">Data krypteras innan de överförs till Azure Storage och när du hämtar data från Azure Storage data dekrypteras när den tas emot på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="83659-166">The data is encrypted before being transferred into Azure Storage and when you retrieve the data from Azure Storage, the data is decrypted after it is received on the client side.</span></span>

### <a name="azure-site-to-site-vpn"></a><span data-ttu-id="83659-167">Azure VPN för plats-till-plats</span><span class="sxs-lookup"><span data-stu-id="83659-167">Azure Site-to-Site VPN</span></span>

<span data-ttu-id="83659-168">Ett effektivt sätt att skydda personliga data som överförs mellan ett företagsnätverk eller användare och virtuella Azure-nätverket är att använda en [plats-till-plats](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) eller [punkt-till-plats](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuella privata nätverk (VPN).</span><span class="sxs-lookup"><span data-stu-id="83659-168">An effective way to protect personal data in transit between a corporate network or user and the Azure virtual network is to use a [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) or [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) Virtual Private Network (VPN).</span></span> <span data-ttu-id="83659-169">En VPN-anslutning skapar en säker krypterad tunnel via Internet.</span><span class="sxs-lookup"><span data-stu-id="83659-169">A VPN connection creates a secure encrypted tunnel across the Internet.</span></span>

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a><span data-ttu-id="83659-170">Hur skapar jag en plats-till-plats VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="83659-170">How do I create a site-to-site VPN connection?</span></span>

<span data-ttu-id="83659-171">En plats-till-plats-VPN ansluter flera användare på företagets nätverk till Azure.</span><span class="sxs-lookup"><span data-stu-id="83659-171">A site-to-site VPN connects multiple users on the corporate network to Azure.</span></span> <span data-ttu-id="83659-172">Om du vill skapa en plats-till-plats-anslutning i Azure-portalen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="83659-172">To create a site-to-site connection in the Azure portal, do the following:</span></span>

1. <span data-ttu-id="83659-173">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="83659-173">Create a virtual network.</span></span>

2. <span data-ttu-id="83659-174">Ange en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="83659-174">Specify a DNS server.</span></span>

3. <span data-ttu-id="83659-175">Skapa gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="83659-175">Create the gateway subnet.</span></span>

4. <span data-ttu-id="83659-176">Skapa VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="83659-176">Create the VPN gateway.</span></span> 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. <span data-ttu-id="83659-177">Skapa lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="83659-177">Create the local network gateway.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. <span data-ttu-id="83659-178">Konfigurera din VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="83659-178">Configure your VPN device.</span></span>

7. <span data-ttu-id="83659-179">Skapa VPN-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="83659-179">Create the VPN connection.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. <span data-ttu-id="83659-180">Kontrollera VPN-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="83659-180">Verify the VPN connection.</span></span>

<span data-ttu-id="83659-181">Mer detaljerad information om hur du skapar en plats-till-plats-anslutning i Azure portal finns i [skapa en plats-till-plats-anslutning i Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="83659-181">For more detailed instructions on how to create a site-to-site connection in the Azure portal, see [Create a Site-to-Site  connection in the Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span></span>

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a><span data-ttu-id="83659-182">Hur skapar jag en punkt-till-plats VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="83659-182">How do I create a point-to-site VPN connection?</span></span>

<span data-ttu-id="83659-183">En punkt-till-plats-VPN skapar en säker anslutning från en enskild klientdator till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="83659-183">A Point-to-Site VPN creates a secure connection from an individual client computer to a virtual network.</span></span> <span data-ttu-id="83659-184">Detta är användbart när du vill ansluta till Azure från en fjärrdator som hemma eller ett hotell eller konferens Center.</span><span class="sxs-lookup"><span data-stu-id="83659-184">This is useful when you  want to connect to Azure from a remote location, such as from home or a hotel or conference center.</span></span> <span data-ttu-id="83659-185">Att skapa en punkt-till-plats-anslutning i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="83659-185">To create a point-to-site  connection in the Azure portal,</span></span>

1. <span data-ttu-id="83659-186">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="83659-186">Create a virtual network.</span></span>

2. <span data-ttu-id="83659-187">Lägg till en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="83659-187">Add a gateway subnet.</span></span>

3. <span data-ttu-id="83659-188">Ange en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="83659-188">Specify a DNS server.</span></span> <span data-ttu-id="83659-189">(valfritt)</span><span class="sxs-lookup"><span data-stu-id="83659-189">(optional)</span></span>

4. <span data-ttu-id="83659-190">Skapa en virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="83659-190">Create a virtual network gateway.</span></span>

5. <span data-ttu-id="83659-191">Generera certifikat.</span><span class="sxs-lookup"><span data-stu-id="83659-191">Generate certificates.</span></span>

6. <span data-ttu-id="83659-192">Lägg till klient-adresspool.</span><span class="sxs-lookup"><span data-stu-id="83659-192">Add the client address pool.</span></span>

7. <span data-ttu-id="83659-193">Ladda upp roten certifikat certifikatets offentliga data.</span><span class="sxs-lookup"><span data-stu-id="83659-193">Upload the root certificate public certificate data.</span></span>

8. <span data-ttu-id="83659-194">Generera och installera VPN-klientpaketet för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="83659-194">Generate and install the VPN client configuration package.</span></span>

9. <span data-ttu-id="83659-195">Installera ett exporterade klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="83659-195">Install an exported client certificate.</span></span>

10. <span data-ttu-id="83659-196">Ansluta till Azure.</span><span class="sxs-lookup"><span data-stu-id="83659-196">Connect to Azure.</span></span>

11. <span data-ttu-id="83659-197">Verifiera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="83659-197">Verify your connection.</span></span>

<span data-ttu-id="83659-198">Mer instruktioner finns [konfigurerar en punkt-till-plats-anslutning till ett virtuellt nätverk som använder certifikatautentisering för: Azure-portalen.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="83659-198">For more detailed instructions, see [Configure a Point-to-Site connection to a VNet using certificate authentication: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span></span>

### <a name="ssltls"></a><span data-ttu-id="83659-199">SSL/TLS</span><span class="sxs-lookup"><span data-stu-id="83659-199">SSL/TLS</span></span>

<span data-ttu-id="83659-200">Microsoft rekommenderar att du alltid använder SSL/TLS-protokoll för att utbyta data mellan olika platser.</span><span class="sxs-lookup"><span data-stu-id="83659-200">Microsoft recommends that you always use SSL/TLS protocols to exchange data across different locations.</span></span> <span data-ttu-id="83659-201">Organisationer som använder [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) länk kan flytta stora datamängder via ett dedikerat snabba WAN också kryptera data på programnivå-med hjälp av SSL/TLS- eller andra protokoll för skydd.</span><span class="sxs-lookup"><span data-stu-id="83659-201">Organizations that choose to use [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) to move large data sets over a dedicated high-speed WAN link can also encrypt the data at the application-level using SSL/TLS or other protocols for added protection.</span></span>

### <a name="encryption-by-default"></a><span data-ttu-id="83659-202">Kryptering som standard</span><span class="sxs-lookup"><span data-stu-id="83659-202">Encryption by default</span></span>

<span data-ttu-id="83659-203">Microsoft använder kryptering för att skydda data under överföring mellan kunder och Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="83659-203">Microsoft uses encryption to protect data in transit between customers and Azure cloud services.</span></span> <span data-ttu-id="83659-204">Om du interagerar med Azure Storage via Azure Portal, alla transaktioner ska ske via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83659-204">If you are interacting with Azure Storage through the Azure Portal, all transactions occur via HTTPS.</span></span>

<span data-ttu-id="83659-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) är det protokoll som Microsoft-datacenter försöker att förhandla med klientdatorer som ansluter till Microsofts molntjänster.</span><span class="sxs-lookup"><span data-stu-id="83659-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is the protocol that Microsoft data centers will attempt to negotiate with client systems that connect to Microsoft cloud services.</span></span> <span data-ttu-id="83659-206">TLS innehåller stark autentisering, meddelandesekretess, och integritet (aktiveras identifiering av meddelandet manipulation avlyssning och förfalskning), samverkan, flexibla algoritmer, enkel distribution och användning.</span><span class="sxs-lookup"><span data-stu-id="83659-206">TLS provides strong authentication, message privacy, and integrity (enables detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, ease of deployment and use.</span></span>

<span data-ttu-id="83659-207">[Perfekt Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (Secrecy) används också så att varje anslutning mellan Microsofts molntjänster och kunders klientsystem använda unika nycklar.</span><span class="sxs-lookup"><span data-stu-id="83659-207">[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is also employed so that each connection between customers’ client systems and Microsoft’s cloud services use unique keys.</span></span> <span data-ttu-id="83659-208">Anslutningar till Microsoft-molntjänster också dra nytta av RSA baserat 2 048-bitarskryptering nyckellängder.</span><span class="sxs-lookup"><span data-stu-id="83659-208">Connections to Microsoft cloud services also take advantage of RSA based 2,048-bit encryption key lengths.</span></span> <span data-ttu-id="83659-209">Kombinationen av TLS, RSA 2 048-bitars nyckellängd, och PFS gör det mycket svårare för andra att snappa upp och komma åt data som överförs mellan Microsoft-molntjänster och kunder.</span><span class="sxs-lookup"><span data-stu-id="83659-209">The combination of TLS, RSA 2,048-bit key lengths, and PFS makes it much  more difficult for someone to intercept and access data that is in transit between Microsoft cloud services and customers.</span></span>

<span data-ttu-id="83659-210">Det krypteras alltid data under överföring i [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span><span class="sxs-lookup"><span data-stu-id="83659-210">Data in transit is always encrypted in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span></span> <span data-ttu-id="83659-211">Förutom att kryptera data före lagring till permanenta media skyddas alltid även data under överföring eller i rörelse med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83659-211">In addition to encrypting data prior to storing to persistent media, the data is also always secured in transit by using HTTPS.</span></span> <span data-ttu-id="83659-212">HTTPS är det enda protokoll som stöds för Data Lake Store REST-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="83659-212">HTTPS is the only protocol that is supported for the Data Lake Store REST interfaces.</span></span>

## <a name="summary"></a><span data-ttu-id="83659-213">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="83659-213">Summary</span></span>

<span data-ttu-id="83659-214">Företaget kan göra syftet med att skydda personliga data och sekretessen för sådana data genom att använda HTTPS-anslutningar till Azure Storage, använder signaturer för delad åtkomst och att aktivera säker överföring krävs på storage-konton.</span><span class="sxs-lookup"><span data-stu-id="83659-214">The company can accomplish its goal of protecting personal data and the privacy of such data by enforcing HTTPS connections to Azure Storage, using Shared Access Signatures and enabling Secure Transfer Required on the storage accounts.</span></span> <span data-ttu-id="83659-215">De kan även skydda personliga data med hjälp av SMB 3.0-anslutningar och implementera kryptering på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="83659-215">They can also protect personal data by using SMB 3.0 connections and implementing client-side encryption.</span></span> <span data-ttu-id="83659-216">Plats-till-plats VPN-anslutningar från företagsnätverket till virtuella Azure-nätverket och punkt-till-plats VPN-anslutningar från enskilda användare skapar en säker tunnel som personliga data på ett säkert sätt kan överföras.</span><span class="sxs-lookup"><span data-stu-id="83659-216">Site-to-site VPN connections from the corporate network to the Azure virtual network and point-to-site VPN connections from individual users will create a secure tunnel through which personal data can securely travel.</span></span> <span data-ttu-id="83659-217">Microsofts standard kryptering praxis att ytterligare skydda personliga data.</span><span class="sxs-lookup"><span data-stu-id="83659-217">Microsoft’s default encryption practices will further protect the privacy of personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83659-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83659-218">Next steps</span></span>

- [<span data-ttu-id="83659-219">Metodtips för säkerhet för Azure Data och kryptering</span><span class="sxs-lookup"><span data-stu-id="83659-219">Azure Data Security and Encryption Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [<span data-ttu-id="83659-220">Planering och design för VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="83659-220">Planning and design for VPN Gateway</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [<span data-ttu-id="83659-221">Vanliga frågor och svar om VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="83659-221">VPN Gateway FAQ</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [<span data-ttu-id="83659-222">Köpa och konfigurera ett SSL-certifikat för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="83659-222">Buy and configure an SSL Certificate for your Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
