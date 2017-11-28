---
title: "aaaProtect personliga data under överföring med kryptering i Azure | Microsoft Docs"
description: "Med hjälp av kryptering i Azure tooprotect personliga data"
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
ms.openlocfilehash: 218ad3f49326e8dec299a6d92b18116da65eae71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a><span data-ttu-id="d5626-103">Azure krypteringstekniker: skydda personliga data under överföringen med kryptering</span><span class="sxs-lookup"><span data-stu-id="d5626-103">Azure encryption technologies: Protect personal data in transit with encryption</span></span>

<span data-ttu-id="d5626-104">Den här artikeln hjälper dig att förstå och använda Azure kryptering tekniker toosecure data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="d5626-104">This article will help you understand and use Azure encryption technologies toosecure data in transit.</span></span> 

<span data-ttu-id="d5626-105">Skydda hello sekretess personliga data är över nätverket hello en viktig del av en flera lager skydd på djupet säkerhetsstrategi.</span><span class="sxs-lookup"><span data-stu-id="d5626-105">Protecting hello privacy of personal data as it travels across hello network is an essential part of a multi-layered defense-in-depth security strategy.</span></span> <span data-ttu-id="d5626-106">Kryptering under överföring är utformad tooprevent en angripare fångar upp överföringar från att kunna tooview eller Använd hello-data.</span><span class="sxs-lookup"><span data-stu-id="d5626-106">Encryption in transit is designed tooprevent an attacker who intercepts transmissions from being able tooview or use hello data.</span></span>

## <a name="scenario"></a><span data-ttu-id="d5626-107">Scenario</span><span class="sxs-lookup"><span data-stu-id="d5626-107">Scenario</span></span>

<span data-ttu-id="d5626-108">Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna.</span><span class="sxs-lookup"><span data-stu-id="d5626-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="d5626-109">toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien Tyskland, Danmark och hello Storbritannien</span><span class="sxs-lookup"><span data-stu-id="d5626-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span> 

<span data-ttu-id="d5626-110">hello företag använder Microsoft Azure toostore företagsdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="d5626-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="d5626-111">Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas.</span><span class="sxs-lookup"><span data-stu-id="d5626-111">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="d5626-112">Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser.</span><span class="sxs-lookup"><span data-stu-id="d5626-112">It also includes traditional Human Resource information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="d5626-113">hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.</span><span class="sxs-lookup"><span data-stu-id="d5626-113">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="d5626-114">Personliga data av kunder har angetts i hello databasen från hello företagets fjärranslutna kontor och resa agenter finns runt hello world.</span><span class="sxs-lookup"><span data-stu-id="d5626-114">Personal data of customers is entered in hello database from hello company’s remote offices and from travel agents located around hello world.</span></span> <span data-ttu-id="d5626-115">Dokument som innehåller kundinformation överförs via hello tooAzure nätverkslagring.</span><span class="sxs-lookup"><span data-stu-id="d5626-115">Documents containing customer information are transferred across hello network tooAzure storage.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="d5626-116">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="d5626-116">Problem statement</span></span>

<span data-ttu-id="d5626-117">hello företag måste skydda hello sekretessen för kunders och anställdas personliga data när den är i överföringen tooand från Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d5626-117">hello company must protect hello privacy of customers’ and employees’ personal data while it is in transit tooand from Azure services.</span></span>

## <a name="company-goal"></a><span data-ttu-id="d5626-118">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="d5626-118">Company goal</span></span>

<span data-ttu-id="d5626-119">Hej företagets mål tooensure personliga data som är krypterade när av disken.</span><span class="sxs-lookup"><span data-stu-id="d5626-119">hello company goal tooensure that personal data is encrypted when off disk.</span></span> <span data-ttu-id="d5626-120">Om obehöriga avlyssna hello av disk personliga data, måste den vara i ett formulär som kommer att visas inte kan läsas.</span><span class="sxs-lookup"><span data-stu-id="d5626-120">If unauthorized persons intercept hello off-disk personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="d5626-121">Tillämpa kryptering ska vara enkelt eller helt transparent för användare och administratörer.</span><span class="sxs-lookup"><span data-stu-id="d5626-121">Applying encryption should be easy, or completely transparent, for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="d5626-122">Lösningar</span><span class="sxs-lookup"><span data-stu-id="d5626-122">Solutions</span></span>

<span data-ttu-id="d5626-123">Azure tillhandahåller flera verktyg och tekniker toohelp du skydda personliga data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="d5626-123">Azure services provide multiple tools and technologies toohelp you protect personal data in transit.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="d5626-124">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d5626-124">Azure Storage</span></span>

<span data-ttu-id="d5626-125">Data som lagras i molnet hello färdas från hello-klienten, vilket kan vara fysiskt finns någonstans i hälsningsmeddelande toohello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="d5626-125">Data that is stored in hello cloud must travel from hello client, which can be physically located anywhere in hello world, toohello Azure data center.</span></span> <span data-ttu-id="d5626-126">När dessa data hämtas av användare, de överförs igen i hello motsatt riktning.</span><span class="sxs-lookup"><span data-stu-id="d5626-126">When that data is retrieved by users, it travels again, in hello opposite direction.</span></span> <span data-ttu-id="d5626-127">Data som är under överföring över hello offentliga Internet är alltid risk för avlyssning av angripare.</span><span class="sxs-lookup"><span data-stu-id="d5626-127">Data that is in transit over hello public Internet is always at risk of interception by attackers.</span></span> <span data-ttu-id="d5626-128">Det är viktigt tooprotect hello sekretess personliga data med hjälp av kryptering på transportnivå toosecure som den flyttas mellan platser.</span><span class="sxs-lookup"><span data-stu-id="d5626-128">It is important tooprotect hello privacy of personal data by using transport-level encryption toosecure it as it moves between locations.</span></span>

<span data-ttu-id="d5626-129">hello HTTPS-protokollet ger en säker och krypterad kommunikationskanal över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d5626-129">hello HTTPS protocol provides a secure, encrypted communications channel over hello Internet.</span></span> <span data-ttu-id="d5626-130">HTTPS ska använda tooaccess objekt i Azure Storage och vid anrop av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="d5626-130">HTTPS should be used tooaccess objects in Azure Storage and when calling REST APIs.</span></span> <span data-ttu-id="d5626-131">Du framtvinga användningen av hello HTTPS-protokollet när du använder [signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate åtkomst tooAzure lagringsobjekt.</span><span class="sxs-lookup"><span data-stu-id="d5626-131">You enforce use of hello HTTPS protocol when using [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate access tooAzure Storage objects.</span></span> <span data-ttu-id="d5626-132">Det finns två typer av SAS: tjänst-SAS och konto-SAS.</span><span class="sxs-lookup"><span data-stu-id="d5626-132">There are two types of SAS: Service SAS and Account SAS.</span></span>

#### <a name="how-do-i-construct-a-service-sas"></a><span data-ttu-id="d5626-133">Hur jag för att skapa en tjänst-SAS?</span><span class="sxs-lookup"><span data-stu-id="d5626-133">How do I construct a Service SAS?</span></span>

<span data-ttu-id="d5626-134">En tjänst-SAS delegater åtkomst tooa resurs i en av hello lagringstjänster (blob, kö-, tabell- eller filen service).</span><span class="sxs-lookup"><span data-stu-id="d5626-134">A Service SAS delegates access tooa resource in just one of hello storage services (blob, queue, table or file service).</span></span> <span data-ttu-id="d5626-135">tooconstruct en tjänst-SAS hello följande:</span><span class="sxs-lookup"><span data-stu-id="d5626-135">tooconstruct a Service SAS, do hello following:</span></span>

1. <span data-ttu-id="d5626-136">Ange hello signerat versionsfältet</span><span class="sxs-lookup"><span data-stu-id="d5626-136">Specify hello Signed Version Field</span></span>

2. <span data-ttu-id="d5626-137">Ange hello signerat resurs (Blob och endast filen Service)</span><span class="sxs-lookup"><span data-stu-id="d5626-137">Specify hello Signed Resource (Blob and File Service Only)</span></span>

3. <span data-ttu-id="d5626-138">Ange frågeparametrar tooOverride svarshuvuden (Blob-tjänsten och endast filen Service)</span><span class="sxs-lookup"><span data-stu-id="d5626-138">Specify Query Parameters tooOverride Response Headers (Blob Service and File Service Only)</span></span>

4. <span data-ttu-id="d5626-139">Ange hello tabellnamn (endast tabellen Service)</span><span class="sxs-lookup"><span data-stu-id="d5626-139">Specify hello Table Name (Table Service Only)</span></span>

5. <span data-ttu-id="d5626-140">Ange hello åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="d5626-140">Specify hello Access Policy</span></span>

6. <span data-ttu-id="d5626-141">Ange hello giltighetsintervall för signatur</span><span class="sxs-lookup"><span data-stu-id="d5626-141">Specify hello Signature Validity Interval</span></span>

8. <span data-ttu-id="d5626-142">Ange behörigheter</span><span class="sxs-lookup"><span data-stu-id="d5626-142">Specify Permissions</span></span>

9. <span data-ttu-id="d5626-143">Ange IP-adress eller IP-intervall</span><span class="sxs-lookup"><span data-stu-id="d5626-143">Specify IP Address or IP Range</span></span>

10. <span data-ttu-id="d5626-144">Ange hello HTTP-protokoll</span><span class="sxs-lookup"><span data-stu-id="d5626-144">Specify hello HTTP Protocol</span></span>

11. <span data-ttu-id="d5626-145">Ange intervall för åtkomst av tabell</span><span class="sxs-lookup"><span data-stu-id="d5626-145">Specify Table Access Ranges</span></span>

12. <span data-ttu-id="d5626-146">Ange hello signerat identifierare</span><span class="sxs-lookup"><span data-stu-id="d5626-146">Specify hello Signed Identifier</span></span>

13. <span data-ttu-id="d5626-147">Ange hello signatur</span><span class="sxs-lookup"><span data-stu-id="d5626-147">Specify hello Signature</span></span>

<span data-ttu-id="d5626-148">Mer instruktioner finns [hur du skapar en tjänst-SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span><span class="sxs-lookup"><span data-stu-id="d5626-148">For more detailed instructions, see [Constructing a Service SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span></span>

#### <a name="how-do-i-construct-an-account-sas"></a><span data-ttu-id="d5626-149">Hur jag för att skapa ett konto-SAS?</span><span class="sxs-lookup"><span data-stu-id="d5626-149">How do I construct an Account SAS?</span></span>

<span data-ttu-id="d5626-150">En konto-SAS delegerar åtkomst tooresources i en eller flera av hello lagringstjänster.</span><span class="sxs-lookup"><span data-stu-id="d5626-150">An Account SAS delegates access tooresources in one or more of hello storage services.</span></span> <span data-ttu-id="d5626-151">Du kan också delegera åtkomst tooread-, Skriv- och delete-åtgärder på blob-behållare, tabeller, köer och filresurser som inte tillåts med en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="d5626-151">You can also delegate access tooread, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="d5626-152">Skapa ett konto-SAS är liknande toothat av en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="d5626-152">Construction of an Account SAS is similar toothat of a Service SAS.</span></span> <span data-ttu-id="d5626-153">Detaljerade instruktioner finns [hur du skapar ett konto-SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span><span class="sxs-lookup"><span data-stu-id="d5626-153">For detailed instructions, see [Constructing an Account SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span></span>

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a><span data-ttu-id="d5626-154">Hur behöver jag använda HTTPS vid anrop av REST API: er?</span><span class="sxs-lookup"><span data-stu-id="d5626-154">How do I enforce HTTPS when calling REST APIs?</span></span>

<span data-ttu-id="d5626-155">tooenforce hello HTTPS används vid anrop av REST API: er tooaccess objekt i storage-konton, du kan aktivera säker överföring krävs för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d5626-155">tooenforce hello use of HTTPS when calling REST APIs tooaccess objects in storage accounts, you can enable Secure Transfer Required for hello storage account.</span></span> 

1. <span data-ttu-id="d5626-156">Markera i hello Azure-portalen, **skapa Lagringskonto**, eller ett befintligt lagringskonto, Välj **inställningar** och sedan **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="d5626-156">In hello Azure portal, select **Create Storage Account**, or for an existing storage account, select **Settings** and then **Configuration**.</span></span>

2. <span data-ttu-id="d5626-157">Under **säker överföring krävs**väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="d5626-157">Under **Secure Transfer Required**, select **Enabled**.</span></span>

![Skapa ett lagringskonto](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

<span data-ttu-id="d5626-159">Detaljerade instruktioner, inklusive hur tooenable säker överföring krävs programmässigt, se [kräver säker överföring](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span><span class="sxs-lookup"><span data-stu-id="d5626-159">For more detailed instructions, including how tooenable Secure Transfer Required programmatically, see [Require Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span></span>

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a><span data-ttu-id="d5626-160">Hur jag för att kryptera data i Azure File Storage?</span><span class="sxs-lookup"><span data-stu-id="d5626-160">How do I encrypt data in Azure File Storage?</span></span>

<span data-ttu-id="d5626-161">tooencrypt data under överföring med [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), du kan använda SMB 3.x med Windows 8, 8.1 och 10 och Windows Server 2012 R2 och Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="d5626-161">tooencrypt data in transit with [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), you can use SMB 3.x with Windows 8, 8.1, and 10 and with Windows Server 2012 R2 and Windows Server 2016.</span></span> <span data-ttu-id="d5626-162">När du använder hello Azure Files tjänsten misslyckas en anslutning utan kryptering när ”säker överföring krävs” är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="d5626-162">When you are using hello Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="d5626-163">Detta inkluderar scenarier med hjälp av SMB 2.1 och SMB 3.0 utan kryptering vissa varianter av hello Linux SMB-klienten.</span><span class="sxs-lookup"><span data-stu-id="d5626-163">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of hello Linux SMB client.</span></span>

#### <a name="azure-client-side-encryption"></a><span data-ttu-id="d5626-164">Azure klientsidan kryptering</span><span class="sxs-lookup"><span data-stu-id="d5626-164">Azure Client-Side Encryption</span></span>

<span data-ttu-id="d5626-165">Ett annat alternativ för att skydda personliga data medan de överförs mellan ett klientprogram och Azure Storage är [Client side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span><span class="sxs-lookup"><span data-stu-id="d5626-165">Another option for protecting personal data while it’s being transferred between a client application and Azure Storage is [Client-side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span></span> <span data-ttu-id="d5626-166">hello data krypteras innan de överförs till Azure Storage och när du hämtar hello data från Azure Storage hello data dekrypteras när den tas emot på hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="d5626-166">hello data is encrypted before being transferred into Azure Storage and when you retrieve hello data from Azure Storage, hello data is decrypted after it is received on hello client side.</span></span>

### <a name="azure-site-to-site-vpn"></a><span data-ttu-id="d5626-167">Azure VPN för plats-till-plats</span><span class="sxs-lookup"><span data-stu-id="d5626-167">Azure Site-to-Site VPN</span></span>

<span data-ttu-id="d5626-168">Ett effektivt sätt tooprotect personliga data som överförs mellan ett företagsnätverk eller användare och hello virtuella Azure-nätverket är toouse en [plats-till-plats](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) eller [punkt-till-plats](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuella privata nätverk (VPN).</span><span class="sxs-lookup"><span data-stu-id="d5626-168">An effective way tooprotect personal data in transit between a corporate network or user and hello Azure virtual network is toouse a [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) or [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) Virtual Private Network (VPN).</span></span> <span data-ttu-id="d5626-169">En VPN-anslutning skapar en säker krypterad tunnel via hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d5626-169">A VPN connection creates a secure encrypted tunnel across hello Internet.</span></span>

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a><span data-ttu-id="d5626-170">Hur skapar jag en plats-till-plats VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="d5626-170">How do I create a site-to-site VPN connection?</span></span>

<span data-ttu-id="d5626-171">En plats-till-plats-VPN ansluter flera användare på hello företagsnätverket tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d5626-171">A site-to-site VPN connects multiple users on hello corporate network tooAzure.</span></span> <span data-ttu-id="d5626-172">toocreate en plats-till-plats-anslutning i hello Azure-portalen hello följande:</span><span class="sxs-lookup"><span data-stu-id="d5626-172">toocreate a site-to-site connection in hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="d5626-173">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d5626-173">Create a virtual network.</span></span>

2. <span data-ttu-id="d5626-174">Ange en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="d5626-174">Specify a DNS server.</span></span>

3. <span data-ttu-id="d5626-175">Skapa hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="d5626-175">Create hello gateway subnet.</span></span>

4. <span data-ttu-id="d5626-176">Skapa hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="d5626-176">Create hello VPN gateway.</span></span> 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. <span data-ttu-id="d5626-177">Skapa hello lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="d5626-177">Create hello local network gateway.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. <span data-ttu-id="d5626-178">Konfigurera din VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="d5626-178">Configure your VPN device.</span></span>

7. <span data-ttu-id="d5626-179">Skapa hello VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d5626-179">Create hello VPN connection.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. <span data-ttu-id="d5626-180">Kontrollera hello VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d5626-180">Verify hello VPN connection.</span></span>

<span data-ttu-id="d5626-181">Detaljerade instruktioner om hur toocreate plats-till-plats-anslutning i hello Azure portal, se [skapa en plats-till-plats-anslutning i hello Azure-portalen.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="d5626-181">For more detailed instructions on how toocreate a site-to-site connection in hello Azure portal, see [Create a Site-to-Site  connection in hello Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span></span>

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a><span data-ttu-id="d5626-182">Hur skapar jag en punkt-till-plats VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="d5626-182">How do I create a point-to-site VPN connection?</span></span>

<span data-ttu-id="d5626-183">En punkt-till-plats-VPN skapar en säker anslutning från en enskild klient tooa virtuella datornätverk.</span><span class="sxs-lookup"><span data-stu-id="d5626-183">A Point-to-Site VPN creates a secure connection from an individual client computer tooa virtual network.</span></span> <span data-ttu-id="d5626-184">Detta är användbart när du vill tooconnect tooAzure från en fjärrdator som hemma eller ett hotell eller konferens Center.</span><span class="sxs-lookup"><span data-stu-id="d5626-184">This is useful when you  want tooconnect tooAzure from a remote location, such as from home or a hotel or conference center.</span></span> <span data-ttu-id="d5626-185">toocreate en punkt-till-plats-anslutning i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d5626-185">toocreate a point-to-site  connection in hello Azure portal,</span></span>

1. <span data-ttu-id="d5626-186">Skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d5626-186">Create a virtual network.</span></span>

2. <span data-ttu-id="d5626-187">Lägg till en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="d5626-187">Add a gateway subnet.</span></span>

3. <span data-ttu-id="d5626-188">Ange en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="d5626-188">Specify a DNS server.</span></span> <span data-ttu-id="d5626-189">(valfritt)</span><span class="sxs-lookup"><span data-stu-id="d5626-189">(optional)</span></span>

4. <span data-ttu-id="d5626-190">Skapa en virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="d5626-190">Create a virtual network gateway.</span></span>

5. <span data-ttu-id="d5626-191">Generera certifikat.</span><span class="sxs-lookup"><span data-stu-id="d5626-191">Generate certificates.</span></span>

6. <span data-ttu-id="d5626-192">Lägg till hello klient-adresspool.</span><span class="sxs-lookup"><span data-stu-id="d5626-192">Add hello client address pool.</span></span>

7. <span data-ttu-id="d5626-193">Överföra hello root certifikat certifikatets offentliga data.</span><span class="sxs-lookup"><span data-stu-id="d5626-193">Upload hello root certificate public certificate data.</span></span>

8. <span data-ttu-id="d5626-194">Generera och installera hello VPN-klientpaketet konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d5626-194">Generate and install hello VPN client configuration package.</span></span>

9. <span data-ttu-id="d5626-195">Installera ett exporterade klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="d5626-195">Install an exported client certificate.</span></span>

10. <span data-ttu-id="d5626-196">Ansluta tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d5626-196">Connect tooAzure.</span></span>

11. <span data-ttu-id="d5626-197">Verifiera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d5626-197">Verify your connection.</span></span>

<span data-ttu-id="d5626-198">Mer instruktioner finns [konfigurerar en punkt-till-plats-anslutning tooa VNet använder certifikatautentisering för: Azure-portalen.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="d5626-198">For more detailed instructions, see [Configure a Point-to-Site connection tooa VNet using certificate authentication: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span></span>

### <a name="ssltls"></a><span data-ttu-id="d5626-199">SSL/TLS</span><span class="sxs-lookup"><span data-stu-id="d5626-199">SSL/TLS</span></span>

<span data-ttu-id="d5626-200">Microsoft rekommenderar att du alltid använder SSL/TLS-protokollen tooexchange data mellan olika platser.</span><span class="sxs-lookup"><span data-stu-id="d5626-200">Microsoft recommends that you always use SSL/TLS protocols tooexchange data across different locations.</span></span> <span data-ttu-id="d5626-201">Organisationer som väljer toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove stora datamängder dedikerade snabba WAN-länk kan också kryptera hello data i hello på programnivå med hjälp av SSL/TLS- eller andra protokoll för extra skydd.</span><span class="sxs-lookup"><span data-stu-id="d5626-201">Organizations that choose toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove large data sets over a dedicated high-speed WAN link can also encrypt hello data at hello application-level using SSL/TLS or other protocols for added protection.</span></span>

### <a name="encryption-by-default"></a><span data-ttu-id="d5626-202">Kryptering som standard</span><span class="sxs-lookup"><span data-stu-id="d5626-202">Encryption by default</span></span>

<span data-ttu-id="d5626-203">Microsoft använder kryptering tooprotect data som överförs mellan kunder och Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="d5626-203">Microsoft uses encryption tooprotect data in transit between customers and Azure cloud services.</span></span> <span data-ttu-id="d5626-204">Om du interagerar med Azure Storage via hello Azure-portalen, alla transaktioner ska ske via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5626-204">If you are interacting with Azure Storage through hello Azure Portal, all transactions occur via HTTPS.</span></span>

<span data-ttu-id="d5626-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) är Microsoft-datacenter försöker toonegotiate med klientdatorer som ansluter till molntjänster tooMicrosoft hello-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d5626-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is hello protocol that Microsoft data centers will attempt toonegotiate with client systems that connect tooMicrosoft cloud services.</span></span> <span data-ttu-id="d5626-206">TLS innehåller stark autentisering, meddelandesekretess, och integritet (aktiveras identifiering av meddelandet manipulation avlyssning och förfalskning), samverkan, flexibla algoritmer, enkel distribution och användning.</span><span class="sxs-lookup"><span data-stu-id="d5626-206">TLS provides strong authentication, message privacy, and integrity (enables detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, ease of deployment and use.</span></span>

<span data-ttu-id="d5626-207">[Perfekt Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (Secrecy) används också så att varje anslutning mellan Microsofts molntjänster och kunders klientsystem använda unika nycklar.</span><span class="sxs-lookup"><span data-stu-id="d5626-207">[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is also employed so that each connection between customers’ client systems and Microsoft’s cloud services use unique keys.</span></span> <span data-ttu-id="d5626-208">Anslutningar tooMicrosoft molntjänster också dra nytta av RSA baserat 2 048-bitarskryptering nyckellängder.</span><span class="sxs-lookup"><span data-stu-id="d5626-208">Connections tooMicrosoft cloud services also take advantage of RSA based 2,048-bit encryption key lengths.</span></span> <span data-ttu-id="d5626-209">Hej kombination av TLS, RSA 2 048-bitars nyckellängd, och PFS gör det mycket svårare för toointercept och komma åt data som överförs mellan Microsoft-molntjänster och kunder.</span><span class="sxs-lookup"><span data-stu-id="d5626-209">hello combination of TLS, RSA 2,048-bit key lengths, and PFS makes it much  more difficult for someone toointercept and access data that is in transit between Microsoft cloud services and customers.</span></span>

<span data-ttu-id="d5626-210">Det krypteras alltid data under överföring i [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span><span class="sxs-lookup"><span data-stu-id="d5626-210">Data in transit is always encrypted in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span></span> <span data-ttu-id="d5626-211">Dessutom tooencrypting data tidigare toostoring toopersistent media hello data alltid skyddas under överföringen med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5626-211">In addition tooencrypting data prior toostoring toopersistent media, hello data is also always secured in transit by using HTTPS.</span></span> <span data-ttu-id="d5626-212">HTTPS är hello enda protokoll som stöds för hello Data Lake Store REST-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d5626-212">HTTPS is hello only protocol that is supported for hello Data Lake Store REST interfaces.</span></span>

## <a name="summary"></a><span data-ttu-id="d5626-213">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d5626-213">Summary</span></span>

<span data-ttu-id="d5626-214">hello företagets åstadkommer syftet med att skydda personliga data och hello sekretessen för sådana data genom att använda HTTPS-anslutningar tooAzure lagring, använder signaturer för delad åtkomst och att aktivera säker överföring krävs på hello storage-konton.</span><span class="sxs-lookup"><span data-stu-id="d5626-214">hello company can accomplish its goal of protecting personal data and hello privacy of such data by enforcing HTTPS connections tooAzure Storage, using Shared Access Signatures and enabling Secure Transfer Required on hello storage accounts.</span></span> <span data-ttu-id="d5626-215">De kan även skydda personliga data med hjälp av SMB 3.0-anslutningar och implementera kryptering på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="d5626-215">They can also protect personal data by using SMB 3.0 connections and implementing client-side encryption.</span></span> <span data-ttu-id="d5626-216">Plats-till-plats VPN-anslutningar från hello företagsnätverket toohello virtuella Azure-nätverket och punkt-till-plats VPN-anslutningar från enskilda användare skapar en säker tunnel som personliga data på ett säkert sätt kan överföras.</span><span class="sxs-lookup"><span data-stu-id="d5626-216">Site-to-site VPN connections from hello corporate network toohello Azure virtual network and point-to-site VPN connections from individual users will create a secure tunnel through which personal data can securely travel.</span></span> <span data-ttu-id="d5626-217">Microsofts standard kryptering praxis skydda ytterligare hello sekretess personliga data.</span><span class="sxs-lookup"><span data-stu-id="d5626-217">Microsoft’s default encryption practices will further protect hello privacy of personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5626-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5626-218">Next steps</span></span>

- [<span data-ttu-id="d5626-219">Metodtips för säkerhet för Azure Data och kryptering</span><span class="sxs-lookup"><span data-stu-id="d5626-219">Azure Data Security and Encryption Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [<span data-ttu-id="d5626-220">Planering och design för VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="d5626-220">Planning and design for VPN Gateway</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [<span data-ttu-id="d5626-221">Vanliga frågor och svar om VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="d5626-221">VPN Gateway FAQ</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [<span data-ttu-id="d5626-222">Köpa och konfigurera ett SSL-certifikat för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d5626-222">Buy and configure an SSL Certificate for your Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
