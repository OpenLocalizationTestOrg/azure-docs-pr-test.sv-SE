---
title: "aaaTroubleshoot Data Management Gateway utfärdar | Microsoft Docs"
description: "Innehåller tips tootroubleshoot problem relaterade tooData Management Gateway."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 85dacc8a1e8d574d6e7d5b556c995cebdc148fde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a><span data-ttu-id="20580-103">Felsöka problem med gateway för datahantering</span><span class="sxs-lookup"><span data-stu-id="20580-103">Troubleshoot issues with using Data Management Gateway</span></span>
<span data-ttu-id="20580-104">Den här artikeln innehåller information om felsökning av problem med att använda Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-104">This article provides information on troubleshooting issues with using Data Management Gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="20580-105">Se hello [Data Management Gateway](data-factory-data-management-gateway.md) artikel detaljerad information om hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-105">See hello [Data Management Gateway](data-factory-data-management-gateway.md) article for detailed information about hello gateway.</span></span> <span data-ttu-id="20580-106">Se hello [flytta data mellan lokalt och i molnet](data-factory-move-data-between-onprem-and-cloud.md) artikeln en genomgång för att flytta data från en lokal SQL Server-databasen tooMicrosoft Azure Blob storage med hjälp av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-106">See hello [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for a walkthrough of moving data from an on-premises SQL Server database tooMicrosoft Azure Blob storage by using hello gateway.</span></span>
>
>

## <a name="failed-tooinstall-or-register-gateway"></a><span data-ttu-id="20580-107">Misslyckade tooinstall eller registrera gateway</span><span class="sxs-lookup"><span data-stu-id="20580-107">Failed tooinstall or register gateway</span></span>
### <a name="1-problem"></a><span data-ttu-id="20580-108">1. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-108">1. Problem</span></span>
<span data-ttu-id="20580-109">Du ser det här felmeddelandet när du installerar och registrerar en gateway, särskilt när laddades ned installationsfilen för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-109">You see this error message when installing and registering a gateway, specifically, while downloading hello gateway installation file.</span></span>

`Unable tooconnect toohello remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a><span data-ttu-id="20580-110">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-110">Cause</span></span>
<span data-ttu-id="20580-111">hello datorn som du försöker tooinstall hello gateway har misslyckats toodownload hello senaste gateway installationsfilen hello download Center på grund av nätverksproblem tooa.</span><span class="sxs-lookup"><span data-stu-id="20580-111">hello machine on which you are trying tooinstall hello gateway has failed toodownload hello latest gateway installation file from hello download center due tooa network issue.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-112">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-112">Resolution</span></span>
<span data-ttu-id="20580-113">Kontrollera din brandväggen proxy server inställningar toosee om inställningarna för hello blockerar hello nätverksanslutning från hello datorn toohello [hämtningssidan](https://download.microsoft.com/), och uppdatera hello inställningar därefter.</span><span class="sxs-lookup"><span data-stu-id="20580-113">Check your firewall proxy server settings toosee whether hello settings block hello network connection from hello computer toohello [download center](https://download.microsoft.com/), and update hello settings accordingly.</span></span>

<span data-ttu-id="20580-114">Alternativt kan du hämta hello installationsfilen för hello senaste gatewayen från hello [hämtningssidan](https://www.microsoft.com/download/details.aspx?id=39717) på andra datorer som kan komma åt hello download center.</span><span class="sxs-lookup"><span data-stu-id="20580-114">Alternatively, you can download hello installation file for hello latest gateway from hello [download center](https://www.microsoft.com/download/details.aspx?id=39717) on other machines that can access hello download center.</span></span> <span data-ttu-id="20580-115">Därefter kan du kopiera hello installer filen toohello gateway värddatorn och kör den manuellt tooinstall och uppdatera hello-gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-115">You can then copy hello installer file toohello gateway host computer and run it manually tooinstall and update hello gateway.</span></span>

### <a name="2-problem"></a><span data-ttu-id="20580-116">2. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-116">2. Problem</span></span>
<span data-ttu-id="20580-117">Det här felet visas när du försöker tooinstall en gateway genom att klicka på **installera direkt på den här datorn** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20580-117">You see this error when you're attempting tooinstall a gateway by clicking **install directly on this computer** in hello Azure portal.</span></span>

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a><span data-ttu-id="20580-118">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-118">Cause</span></span>
<span data-ttu-id="20580-119">En gateway är installerad på datorn hello.</span><span class="sxs-lookup"><span data-stu-id="20580-119">A gateway is already installed on hello machine.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-120">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-120">Resolution</span></span>
<span data-ttu-id="20580-121">Avinstallera hello befintlig gateway på hello datorn och klicka på hello **installera direkt på den här datorn** igen.</span><span class="sxs-lookup"><span data-stu-id="20580-121">Uninstall hello existing gateway on hello machine and click hello **install directly on this computer** link again.</span></span>

### <a name="3-problem"></a><span data-ttu-id="20580-122">3. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-122">3. Problem</span></span>
<span data-ttu-id="20580-123">Det här felet kan uppstå när du registrerar en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-123">You might see this error when registering a new gateway.</span></span>

`Error: hello gateway has encountered an error during registration.`

#### <a name="cause"></a><span data-ttu-id="20580-124">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-124">Cause</span></span>
<span data-ttu-id="20580-125">Du kan se det här meddelandet för en av följande orsaker hello:</span><span class="sxs-lookup"><span data-stu-id="20580-125">You might see this message for one of hello following reasons:</span></span>

* <span data-ttu-id="20580-126">hello formatet för hello gatewaynyckeln är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="20580-126">hello format of hello gateway key is invalid.</span></span>
* <span data-ttu-id="20580-127">Hej gatewaynyckeln har ogiltigförklarats.</span><span class="sxs-lookup"><span data-stu-id="20580-127">hello gateway key has been invalidated.</span></span>
* <span data-ttu-id="20580-128">Hej gatewaynyckeln har tagits återskapas från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="20580-128">hello gateway key has been regenerated from hello portal.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="20580-129">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-129">Resolution</span></span>
<span data-ttu-id="20580-130">Kontrollera om du använder hello rätt gateway-nyckeln från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="20580-130">Verify whether you are using hello right gateway key from hello portal.</span></span> <span data-ttu-id="20580-131">Om det behövs, återskapa en nyckel och använda hello viktiga tooregister hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-131">If needed, regenerate a key and use hello key tooregister hello gateway.</span></span>

### <a name="4-problem"></a><span data-ttu-id="20580-132">4. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-132">4. Problem</span></span>
<span data-ttu-id="20580-133">Du kan se hello följande felmeddelande när du registrera en gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-133">You might see hello following error message when you're registering a gateway.</span></span>

`Error: hello content or format of hello gateway key "{gatewayKey}" is invalid, please go tooazure portal toocreate one new gateway or regenerate hello gateway key.`



![Innehåll eller format för nyckel är ogiltig](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a><span data-ttu-id="20580-135">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-135">Cause</span></span>
<span data-ttu-id="20580-136">hello innehåll eller hello inkommande gatewaynyckeln format är felaktigt.</span><span class="sxs-lookup"><span data-stu-id="20580-136">hello content or format of hello input gateway key is incorrect.</span></span> <span data-ttu-id="20580-137">En av hello orsaker kan vara att endast en del av hello nyckeln kopieras hello-portalen eller du använder en ogiltig nyckel.</span><span class="sxs-lookup"><span data-stu-id="20580-137">One of hello reasons can be that you copied only a portion of hello key from hello portal or you're using an invalid key.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-138">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-138">Resolution</span></span>
<span data-ttu-id="20580-139">Skapa en gatewaynyckel i hello portal och använda hello Kopiera knappen toocopy hello hela nyckeln.</span><span class="sxs-lookup"><span data-stu-id="20580-139">Generate a gateway key in hello portal, and use hello copy button toocopy hello whole key.</span></span> <span data-ttu-id="20580-140">Klistra in den i det här fönstret tooregister hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-140">Then paste it in this window tooregister hello gateway.</span></span>

### <a name="5-problem"></a><span data-ttu-id="20580-141">5. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-141">5. Problem</span></span>
<span data-ttu-id="20580-142">Du kan se hello följande felmeddelande när du registrera en gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-142">You might see hello following error message when you're registering a gateway.</span></span>

`Error: hello gateway key is invalid or empty. Specify a valid gateway key from hello portal.`

![Gatewaynyckeln är ogiltig eller tom](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a><span data-ttu-id="20580-144">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-144">Cause</span></span>
<span data-ttu-id="20580-145">Hej gatewaynyckeln har varit återskapas eller hello gateway har tagits bort i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20580-145">hello gateway key has been regenerated or hello gateway has been deleted in hello Azure portal.</span></span> <span data-ttu-id="20580-146">Det kan också inträffa om hello Data Management Gateway-installationen inte är senaste.</span><span class="sxs-lookup"><span data-stu-id="20580-146">It can also happen if hello Data Management Gateway setup is not latest.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-147">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-147">Resolution</span></span>
<span data-ttu-id="20580-148">Kontrollera om hello Data Management Gateway-installationen är hello senaste versionen kan du hitta hello senaste versionen på hello Microsoft [hämtningssidan](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span><span class="sxs-lookup"><span data-stu-id="20580-148">Check if hello Data Management Gateway setup is hello latest version, you can find hello latest version on hello Microsoft [download center](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span></span>

<span data-ttu-id="20580-149">Om installationen är aktuella / senaste och gateway finns fortfarande på portalen, återskapa hello gateway-nyckeln i hello Azure-portalen och använder hello Kopiera knappen toocopy hello hela nyckeln och klistra in den i det här fönstret tooregister hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-149">If setup is current/ latest and gateway still exists on Portal, regenerate hello gateway key in hello Azure portal, and use hello copy button toocopy hello whole key, and then paste it in this window tooregister hello gateway.</span></span> <span data-ttu-id="20580-150">Annars återskapa hello gateway och börja om från början.</span><span class="sxs-lookup"><span data-stu-id="20580-150">Otherwise, recreate hello gateway and start over.</span></span>

### <a name="6-problem"></a><span data-ttu-id="20580-151">6. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-151">6. Problem</span></span>
<span data-ttu-id="20580-152">Du kan se hello följande felmeddelande när du registrera en gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-152">You might see hello following error message when you're registering a gateway.</span></span>

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with hello status “Gateway key is invalid”`

![Gatewaynyckeln är ogiltig eller tom](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a><span data-ttu-id="20580-154">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-154">Cause</span></span>
<span data-ttu-id="20580-155">Det här felet kan inträffa eftersom hello gateway har tagits bort eller hello associerade gatewaynyckeln har har återskapats.</span><span class="sxs-lookup"><span data-stu-id="20580-155">This error might happen because either hello gateway has been deleted or hello associated gateway key has been regenerated.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-156">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-156">Resolution</span></span>
<span data-ttu-id="20580-157">Om hello gateway har tagits bort, återskapa hello gateway från hello-portalen klickar du på **registrera**, kopiera hello nyckeln från hello-portalen, klistra in den och försök tooregister hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-157">If hello gateway has been deleted, re-create hello gateway from hello portal, click **Register**, copy hello key from hello portal, paste it, and try tooregister hello gateway.</span></span>

<span data-ttu-id="20580-158">Om hello gatewayen finns fortfarande, men dess nyckel har tagits återskapas, använder du hello ny nyckel tooregister hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-158">If hello gateway still exists but its key has been regenerated, use hello new key tooregister hello gateway.</span></span> <span data-ttu-id="20580-159">Om du inte har hello nyckel, återskapa hello nyckeln igen från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="20580-159">If you don’t have hello key, regenerate hello key again from hello portal.</span></span>

### <a name="7-problem"></a><span data-ttu-id="20580-160">7. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-160">7. Problem</span></span>
<span data-ttu-id="20580-161">När du registrerat en gateway kan behöva du tooenter sökväg och ett lösenord för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="20580-161">When you're registering a gateway, you might need tooenter path and password for a certificate.</span></span>

![Ange certifikat](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a><span data-ttu-id="20580-163">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-163">Cause</span></span>
<span data-ttu-id="20580-164">hello gatewayen har registrerats på andra datorer innan.</span><span class="sxs-lookup"><span data-stu-id="20580-164">hello gateway has been registered on other machines before.</span></span> <span data-ttu-id="20580-165">Under hello inledande registrering av en gateway har ett krypteringscertifikat associerats med hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-165">During hello initial registration of a gateway, an encryption certificate has been associated with hello gateway.</span></span> <span data-ttu-id="20580-166">hello certifikat kan vara själva genereras av hello gateway eller som hello användaren.</span><span class="sxs-lookup"><span data-stu-id="20580-166">hello certificate can either be self-generated by hello gateway or provided by hello user.</span></span>  <span data-ttu-id="20580-167">Det här certifikatet är används tooencrypt autentiseringsuppgifter för hello-datalagret (länkade tjänst).</span><span class="sxs-lookup"><span data-stu-id="20580-167">This certificate is used tooencrypt credentials of hello data store (linked service).</span></span>  

![Exportera certifikat](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

<span data-ttu-id="20580-169">När återställa hello gateway på en annan värddator frågar hello Registreringsguiden för det här certifikatet toodecrypt autentiseringsuppgifter tidigare krypterade med det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="20580-169">When restoring hello gateway on a different host machine, hello registration wizard asks for this certificate toodecrypt credentials previously encrypted with this certificate.</span></span>  <span data-ttu-id="20580-170">Utan det här certifikatet hello autentiseringsuppgifter inte kan dekrypteras av hello ny gateway och efterföljande kopiera aktivitet körningar som är associerade med den här nya gateway misslyckas.</span><span class="sxs-lookup"><span data-stu-id="20580-170">Without this certificate, hello credentials cannot be decrypted by hello new gateway and subsequent copy activity executions associated with this new gateway will fail.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="20580-171">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-171">Resolution</span></span>
<span data-ttu-id="20580-172">Om du har exporterat hello Autentiseringscertifikatet från hello ursprungliga gateway-datorn med hjälp av hello **exportera** hello-knappen **inställningar** fliken i Data Management Gateway Configuration Manager använder hello här certifikat.</span><span class="sxs-lookup"><span data-stu-id="20580-172">If you have exported hello credential certificate from hello original gateway machine by using hello **Export** button on hello **Settings** tab in Data Management Gateway Configuration Manager, use hello certificate here.</span></span>

<span data-ttu-id="20580-173">Du kan inte hoppa över det här steget när du återställer en gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-173">You cannot skip this stage when recovering a gateway.</span></span> <span data-ttu-id="20580-174">Om hello certifikat saknas måste toodelete hello gateway från hello-portalen och skapa en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-174">If hello certificate is missing, you need toodelete hello gateway from hello portal and re-create a new gateway.</span></span>  <span data-ttu-id="20580-175">Dessutom kan uppdatera alla länkade tjänster som är relaterade toohello gateway genom att skriva in deras autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="20580-175">In addition, update all linked services that are related toohello gateway by reentering their credentials.</span></span>

### <a name="8-problem"></a><span data-ttu-id="20580-176">8. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-176">8. Problem</span></span>
<span data-ttu-id="20580-177">Du kan se hello följande felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="20580-177">You might see hello following error message.</span></span>

`Error: hello remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a><span data-ttu-id="20580-178">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-178">Cause</span></span>
<span data-ttu-id="20580-179">Det här felet uppstår när din gateway är i en miljö som kräver en HTTP-proxy tooaccess Internet-resurser eller autentisering proxylösenord har ändrats men det inte uppdateras i din gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-179">This error happens when your gateway is in an environment that requires an HTTP proxy tooaccess Internet resources, or your proxy's authentication password is changed but it's not updated accordingly in your gateway.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-180">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-180">Resolution</span></span>
<span data-ttu-id="20580-181">Följ instruktionerna hello i hello [Proxy server-överväganden](#proxy-server-considerations) avsnitt i denna artikel och konfigurera proxyinställningar med Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="20580-181">Follow hello instructions in hello [Proxy server considerations](#proxy-server-considerations) section of this article, and configure proxy settings with Data Management Gateway Configuration Manager.</span></span>

## <a name="gateway-is-online-with-limited-functionality"></a><span data-ttu-id="20580-182">Gatewayen är online med begränsade funktioner</span><span class="sxs-lookup"><span data-stu-id="20580-182">Gateway is online with limited functionality</span></span>
### <a name="1-problem"></a><span data-ttu-id="20580-183">1. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-183">1. Problem</span></span>
<span data-ttu-id="20580-184">Du kan se hello status hello gateway som online med begränsade funktioner.</span><span class="sxs-lookup"><span data-stu-id="20580-184">You see hello status of hello gateway as online with limited functionality.</span></span>

#### <a name="cause"></a><span data-ttu-id="20580-185">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-185">Cause</span></span>
<span data-ttu-id="20580-186">Du kan se hello status hello gateway som online med begränsade funktioner för en av följande orsaker hello:</span><span class="sxs-lookup"><span data-stu-id="20580-186">You see hello status of hello gateway as online with limited functionality for one of hello following reasons:</span></span>

* <span data-ttu-id="20580-187">Gateway kan inte ansluta toocloud tjänsten via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="20580-187">Gateway cannot connect toocloud service through Azure Service Bus.</span></span>
* <span data-ttu-id="20580-188">Molntjänsten kan inte ansluta toogateway via Service Bus.</span><span class="sxs-lookup"><span data-stu-id="20580-188">Cloud service cannot connect toogateway through Service Bus.</span></span>

<span data-ttu-id="20580-189">När hello gateway är online med begränsade funktioner kanske inte kan toouse hello guiden för Data Factory kopiera toocreate data pipelines för att kopiera data tooor från lokala datalager.</span><span class="sxs-lookup"><span data-stu-id="20580-189">When hello gateway is online with limited functionality, you might not be able toouse hello Data Factory Copy Wizard toocreate data pipelines for copying data tooor from on-premises data stores.</span></span> <span data-ttu-id="20580-190">Som en tillfällig lösning kan använda du Data Factory-redigeraren i hello portal, Visual Studio eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="20580-190">As a workaround, you can use Data Factory Editor in hello portal, Visual Studio, or Azure PowerShell.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-191">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-191">Resolution</span></span>
<span data-ttu-id="20580-192">Lösning på problemet (online med begränsade funktioner) baserat på om hello gateway inte kan ansluta toohello molntjänst eller hello andra sätt.</span><span class="sxs-lookup"><span data-stu-id="20580-192">Resolution for this issue (online with limited functionality) is based on whether hello gateway cannot connect toohello cloud service or hello other way.</span></span> <span data-ttu-id="20580-193">hello följande avsnitt ger dessa lösningar.</span><span class="sxs-lookup"><span data-stu-id="20580-193">hello following sections provide these resolutions.</span></span>

### <a name="2-problem"></a><span data-ttu-id="20580-194">2. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-194">2. Problem</span></span>
<span data-ttu-id="20580-195">Hello följande fel visas.</span><span class="sxs-lookup"><span data-stu-id="20580-195">You see hello following error.</span></span>

`Error: Gateway cannot connect toocloud service through service bus`

![Gateway kan inte ansluta toocloud service](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a><span data-ttu-id="20580-197">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-197">Cause</span></span>
<span data-ttu-id="20580-198">Gateway kan inte ansluta toohello molntjänst via Service Bus.</span><span class="sxs-lookup"><span data-stu-id="20580-198">Gateway cannot connect toohello cloud service through Service Bus.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-199">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-199">Resolution</span></span>
<span data-ttu-id="20580-200">Följ dessa steg tooget hello gateway igen:</span><span class="sxs-lookup"><span data-stu-id="20580-200">Follow these steps tooget hello gateway back online:</span></span>

1. <span data-ttu-id="20580-201">Tillåt utgående regler på hello gateway-datorn och hello företagets brandvägg för IP-adress.</span><span class="sxs-lookup"><span data-stu-id="20580-201">Allow IP address outbound rules on hello gateway machine and hello corporate firewall.</span></span> <span data-ttu-id="20580-202">Du kan söka efter IP-adresser från hello Windows-händelseloggen (ID == 401): ett försök har gjorts tooaccess en socket på ett sätt som tillåts inte av åtkomstbehörigheterna XX. XX. XX. XX:9350.</span><span class="sxs-lookup"><span data-stu-id="20580-202">You can find IP addresses from hello Windows Event Log (ID == 401): An attempt was made tooaccess a socket in a way forbidden by its access permissions XX.XX.XX.XX:9350.</span></span>
* <span data-ttu-id="20580-203">Konfigurera proxyinställningar på hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-203">Configure proxy settings on hello gateway.</span></span> <span data-ttu-id="20580-204">Se hello [Proxy server-överväganden](#proxy-server-considerations) information.</span><span class="sxs-lookup"><span data-stu-id="20580-204">See hello [Proxy server considerations](#proxy-server-considerations) section for details.</span></span>
* <span data-ttu-id="20580-205">Aktivera utgående port 5671 och 9350 9354 på båda hello Windows-brandväggen på hello gateway-datorn och hello företagets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="20580-205">Enable outbound ports 5671 and 9350-9354 on both hello Windows Firewall on hello gateway machine and hello corporate firewall.</span></span> <span data-ttu-id="20580-206">Se hello [portar och brandväggen](#ports-and-firewall) information.</span><span class="sxs-lookup"><span data-stu-id="20580-206">See hello [Ports and firewall](#ports-and-firewall) section for details.</span></span> <span data-ttu-id="20580-207">Det här steget är valfritt men rekommenderas för prestanda.</span><span class="sxs-lookup"><span data-stu-id="20580-207">This step is optional, but we recommend it for performance consideration.</span></span>

### <a name="3-problem"></a><span data-ttu-id="20580-208">3. Problem</span><span class="sxs-lookup"><span data-stu-id="20580-208">3. Problem</span></span>
<span data-ttu-id="20580-209">Hello följande fel visas.</span><span class="sxs-lookup"><span data-stu-id="20580-209">You see hello following error.</span></span>

`Error: Cloud service cannot connect toogateway through service bus.`

#### <a name="cause"></a><span data-ttu-id="20580-210">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-210">Cause</span></span>
<span data-ttu-id="20580-211">Ett tillfälligt fel i nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="20580-211">A transient error in network connectivity.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-212">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-212">Resolution</span></span>
<span data-ttu-id="20580-213">Följ dessa steg tooget hello gateway igen:</span><span class="sxs-lookup"><span data-stu-id="20580-213">Follow these steps tooget hello gateway back online:</span></span>

1. <span data-ttu-id="20580-214">Vänta några minuter, hello anslutningen återställs automatiskt när hello felet är åtgärdat.</span><span class="sxs-lookup"><span data-stu-id="20580-214">Wait for a couple of minutes, hello connectivity will be automatically recovered when hello error is gone.</span></span>
* <span data-ttu-id="20580-215">Om hello felet kvarstår startar du om hello gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="20580-215">If hello error persists, restart hello gateway service.</span></span>

## <a name="failed-tooauthor-linked-service"></a><span data-ttu-id="20580-216">Misslyckade tooauthor länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="20580-216">Failed tooauthor linked service</span></span>
### <a name="problem"></a><span data-ttu-id="20580-217">Problem</span><span class="sxs-lookup"><span data-stu-id="20580-217">Problem</span></span>
<span data-ttu-id="20580-218">Det här felet kan uppstå när du försöker toouse Autentiseringshanteraren hello portal tooinput autentiseringsuppgifterna för en ny länkade tjänst eller uppdatera autentiseringsuppgifterna för en befintlig länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="20580-218">You might see this error when you try toouse Credential Manager in hello portal tooinput credentials for a new linked service, or update credentials for an existing linked service.</span></span>

`Error: hello data store '<Server>/<Database>' cannot be reached. Check connection settings for hello data source.`

<span data-ttu-id="20580-219">När du ser felet hello inställningssidan för Data Management Gateway Configuration Manager kan se ut så hello följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="20580-219">When you see this error, hello settings page of Data Management Gateway Configuration Manager might look like hello following screenshot.</span></span>

![Databasen kan inte nås](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a><span data-ttu-id="20580-221">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-221">Cause</span></span>
<span data-ttu-id="20580-222">hello SSL-certifikat kan ha varit förlorat på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="20580-222">hello SSL certificate might have been lost on hello gateway machine.</span></span> <span data-ttu-id="20580-223">hello gateway-datorn kan inte läsa in hello certifikat för närvarande som används för SSL-kryptering.</span><span class="sxs-lookup"><span data-stu-id="20580-223">hello gateway computer cannot load hello certificate currently that is used for SSL encryption.</span></span> <span data-ttu-id="20580-224">Du kan också se ett felmeddelande i hello-händelseloggen som är liknande toohello efter meddelande.</span><span class="sxs-lookup"><span data-stu-id="20580-224">You might also see an error message in hello event log that is similar toohello following message.</span></span>

 `Unable tooget hello gateway settings from cloud service. Check hello gateway key and hello network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a><span data-ttu-id="20580-225">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-225">Resolution</span></span>
<span data-ttu-id="20580-226">Följ dessa steg toosolve hello problemet:</span><span class="sxs-lookup"><span data-stu-id="20580-226">Follow these steps toosolve hello problem:</span></span>

1. <span data-ttu-id="20580-227">Starta Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="20580-227">Start Data Management Gateway Configuration Manager.</span></span>
2. <span data-ttu-id="20580-228">Växla toohello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="20580-228">Switch toohello **Settings** tab.</span></span>  
3. <span data-ttu-id="20580-229">Klicka på hello **ändra** knappen toochange hello SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="20580-229">Click hello **Change** button toochange hello SSL certificate.</span></span>

   ![Ändra certifikat för](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. <span data-ttu-id="20580-231">Välj ett nytt certifikat som hello SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="20580-231">Select a new certificate as hello SSL certificate.</span></span> <span data-ttu-id="20580-232">Du kan använda ett SSL-certifikat som genereras av du eller en organisation.</span><span class="sxs-lookup"><span data-stu-id="20580-232">You can use any SSL certificate that is generated by you or any organization.</span></span>

   ![Ange certifikat](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a><span data-ttu-id="20580-234">Kopiera aktiviteten misslyckas</span><span class="sxs-lookup"><span data-stu-id="20580-234">Copy activity fails</span></span>
### <a name="problem"></a><span data-ttu-id="20580-235">Problem</span><span class="sxs-lookup"><span data-stu-id="20580-235">Problem</span></span>
<span data-ttu-id="20580-236">Du kan se hello efter ”UserErrorFailedToConnectToSqlserver” när du har skapat en pipeline i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="20580-236">You might notice hello following "UserErrorFailedToConnectToSqlserver" failure after you set up a pipeline in hello portal.</span></span>

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect tooSQL Server`

#### <a name="cause"></a><span data-ttu-id="20580-237">Orsak</span><span class="sxs-lookup"><span data-stu-id="20580-237">Cause</span></span>
<span data-ttu-id="20580-238">Detta kan inträffa av olika anledningar och minskning varierar i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="20580-238">This can happen for different reasons, and mitigation varies accordingly.</span></span>

#### <a name="resolution"></a><span data-ttu-id="20580-239">Lösning</span><span class="sxs-lookup"><span data-stu-id="20580-239">Resolution</span></span>
<span data-ttu-id="20580-240">Tillåt utgående TCP-anslutningar via TCP-port/1433 på hello Data Management Gateway på klientsidan innan du ansluter tooan SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="20580-240">Allow outbound TCP connections over port TCP/1433 on hello Data Management Gateway client side before connecting tooan SQL database.</span></span>

<span data-ttu-id="20580-241">Kontrollera SQL Server brandväggsinställningar för Azure samt om hello måldatabasen är en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="20580-241">If hello target database is an Azure SQL database, check SQL Server firewall settings for Azure as well.</span></span>

<span data-ttu-id="20580-242">Se hello följande avsnitt tootest hello anslutning toohello lokala datalager.</span><span class="sxs-lookup"><span data-stu-id="20580-242">See hello following section tootest hello connection toohello on-premises data store.</span></span>

## <a name="data-store-connection-or-driver-related-errors"></a><span data-ttu-id="20580-243">Datalager anslutningen och drivrutinen-relaterade fel</span><span class="sxs-lookup"><span data-stu-id="20580-243">Data store connection or driver-related errors</span></span>
<span data-ttu-id="20580-244">Om du ser data lagrar anslutningen och drivrutinen-relaterade fel slutföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="20580-244">If you see data store connection or driver-related errors, complete hello following steps:</span></span>

1. <span data-ttu-id="20580-245">Starta Data Management Gateway Configuration Manager på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="20580-245">Start Data Management Gateway Configuration Manager on hello gateway machine.</span></span>
2. <span data-ttu-id="20580-246">Växla toohello **diagnostik** fliken.</span><span class="sxs-lookup"><span data-stu-id="20580-246">Switch toohello **Diagnostics** tab.</span></span>
3. <span data-ttu-id="20580-247">I **Testanslutningen**, lägga till hello gateway gruppvärden.</span><span class="sxs-lookup"><span data-stu-id="20580-247">In **Test Connection**, add hello gateway group values.</span></span>
4. <span data-ttu-id="20580-248">Klicka på **Test** toosee om du kan ansluta toohello lokal datakälla från hello gateway-datorn med hjälp av hello anslutningsinformationen och autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="20580-248">Click **Test** toosee if you can connect toohello on-premises data source from hello gateway machine by using hello connection information and credentials.</span></span> <span data-ttu-id="20580-249">Om hello Testa anslutning fortfarande misslyckas efter installation av en drivrutin, omstart hello gateway för det toopick in hello senast ändrades.</span><span class="sxs-lookup"><span data-stu-id="20580-249">If hello test connection still fails after you install a driver, restart hello gateway for it toopick up hello latest change.</span></span>

![Testa anslutning i fliken diagnostik](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a><span data-ttu-id="20580-251">Gateway-loggarna</span><span class="sxs-lookup"><span data-stu-id="20580-251">Gateway logs</span></span>
### <a name="send-gateway-logs-toomicrosoft"></a><span data-ttu-id="20580-252">Skicka tooMicrosoft för gateway-loggar</span><span class="sxs-lookup"><span data-stu-id="20580-252">Send gateway logs tooMicrosoft</span></span>
<span data-ttu-id="20580-253">När du kontaktar Microsoft Support tooget hjälp med felsökning av problem med gateway kan du bli ombedd tooshare gateway-loggarna.</span><span class="sxs-lookup"><span data-stu-id="20580-253">When you contact Microsoft Support tooget help with troubleshooting gateway issues, you might be asked tooshare your gateway logs.</span></span> <span data-ttu-id="20580-254">Du kan dela krävs gateway-loggarna med två knappen klick i Data Management Gateway Configuration Manager hello versionen av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-254">With hello release of hello gateway, you can share required gateway logs with two button clicks in Data Management Gateway Configuration Manager.</span></span>    

1. <span data-ttu-id="20580-255">Växla toohello **diagnostik** fliken i Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="20580-255">Switch toohello **Diagnostics** tab in Data Management Gateway Configuration Manager.</span></span>

    ![Fliken för data Management Gatewaydiagnostik](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. <span data-ttu-id="20580-257">Klicka på **skicka loggar** toosee hello följande dialogruta.</span><span class="sxs-lookup"><span data-stu-id="20580-257">Click **Send Logs** toosee hello following dialog box.</span></span>

    ![Data Management Gateway skicka loggar](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. <span data-ttu-id="20580-259">(Valfritt) Klicka på **visa loggar** tooreview loggas i Loggboken för hello.</span><span class="sxs-lookup"><span data-stu-id="20580-259">(Optional) Click **view logs** tooreview logs in hello event viewer.</span></span>
4. <span data-ttu-id="20580-260">(Valfritt) Klicka på **sekretess** tooreview sekretesspolicy för Microsoft webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="20580-260">(Optional) Click **privacy** tooreview Microsoft web services privacy statement.</span></span>
5. <span data-ttu-id="20580-261">När du är nöjd med vad du kommer tooupload klickar du på **skicka loggar** tooactually skicka hello loggarna från hello senaste sju dagarna tooMicrosoft för felsökning.</span><span class="sxs-lookup"><span data-stu-id="20580-261">When you are satisfied with what you are about tooupload, click **Send Logs** tooactually send hello logs from hello last seven days tooMicrosoft for troubleshooting.</span></span> <span data-ttu-id="20580-262">Du bör se hello status för hello skicka loggar åtgärd som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="20580-262">You should see hello status of hello send-logs operation as shown in hello following screenshot.</span></span>

    ![Data Management Gateway skicka loggar status](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. <span data-ttu-id="20580-264">När hello åtgärden är klar, visas en dialogruta som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="20580-264">After hello operation is complete, you see a dialog box as shown in hello following screenshot.</span></span>

    ![Data Management Gateway skicka loggar status](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. <span data-ttu-id="20580-266">Spara hello **rapport-ID** och dela den med Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="20580-266">Save hello **Report ID** and share it with Microsoft Support.</span></span> <span data-ttu-id="20580-267">hello rapport-ID är används toolocate hello gateway-loggarna som du har överfört för felsökning.</span><span class="sxs-lookup"><span data-stu-id="20580-267">hello report ID is used toolocate hello gateway logs that you uploaded for troubleshooting.</span></span>  <span data-ttu-id="20580-268">hello rapport-ID sparas även i hello Loggboken.</span><span class="sxs-lookup"><span data-stu-id="20580-268">hello report ID is also saved in hello event viewer.</span></span>  <span data-ttu-id="20580-269">Du kan hitta den genom att titta på hello händelse-ID ”25” och kontrollera hello datum och tid.</span><span class="sxs-lookup"><span data-stu-id="20580-269">You can find it by looking at hello event ID “25”, and check hello date and time.</span></span>

    ![Data Management Gateway skicka loggar rapport-ID](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a><span data-ttu-id="20580-271">Arkivera gateway-loggar på värddatorn för gateway</span><span class="sxs-lookup"><span data-stu-id="20580-271">Archive gateway logs on gateway host machine</span></span>
<span data-ttu-id="20580-272">Det finns vissa scenarier där du har problem med gateway och du kan inte dela gateway-loggarna direkt:</span><span class="sxs-lookup"><span data-stu-id="20580-272">There are some scenarios where you have gateway issues and you cannot share gateway logs directly:</span></span>

* <span data-ttu-id="20580-273">Du kan manuellt installera hello gateway och registrera hello gateway.</span><span class="sxs-lookup"><span data-stu-id="20580-273">You manually install hello gateway and register hello gateway.</span></span>
* <span data-ttu-id="20580-274">Du kan försöka tooregister hello gateway med en återskapade nyckeln i Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="20580-274">You try tooregister hello gateway with a regenerated key in Data Management Gateway Configuration Manager.</span></span>
* <span data-ttu-id="20580-275">Försök toosend loggar och går inte att ansluta hello gateway-värdtjänsten.</span><span class="sxs-lookup"><span data-stu-id="20580-275">You try toosend logs and hello gateway host service cannot be connected.</span></span>

<span data-ttu-id="20580-276">För dessa scenarier kan du spara gateway-loggarna som en zip-fil och dela den när du kontaktar Microsoft support.</span><span class="sxs-lookup"><span data-stu-id="20580-276">For these scenarios, you can save gateway logs as a zip file and share it when you contact Microsoft support.</span></span> <span data-ttu-id="20580-277">Till exempel om du får ett felmeddelande när du registrerar hello-gateway som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="20580-277">For example, if you receive an error while you register hello gateway as shown in hello following screenshot.</span></span>   

![Data Management Gateway registreringsfel](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

<span data-ttu-id="20580-279">Klicka på hello **Arkivera gateway-loggarna** länka tooarchive och spara loggar och sedan dela hello zip-filen med Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="20580-279">Click hello **Archive gateway logs** link tooarchive and save logs, and then share hello zip file with Microsoft support.</span></span>

![Data Management Gateway Arkivera loggar](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a><span data-ttu-id="20580-281">Leta reda på gateway-loggarna</span><span class="sxs-lookup"><span data-stu-id="20580-281">Locate gateway logs</span></span>
<span data-ttu-id="20580-282">Du hittar detaljerad gateway logginformation i händelseloggarna i Windows hello.</span><span class="sxs-lookup"><span data-stu-id="20580-282">You can find detailed gateway log information in hello Windows event logs.</span></span>

1. <span data-ttu-id="20580-283">Starta Windows **Loggboken**.</span><span class="sxs-lookup"><span data-stu-id="20580-283">Start Windows **Event Viewer**.</span></span>
2. <span data-ttu-id="20580-284">Leta reda på loggarna i hello **program- och tjänstloggar** > **Data Management Gateway** mapp.</span><span class="sxs-lookup"><span data-stu-id="20580-284">Locate logs in hello **Application and Services Logs** > **Data Management Gateway** folder.</span></span>

 <span data-ttu-id="20580-285">När du felsöker problem med gateway, leta efter felhändelser i Loggboken för hello.</span><span class="sxs-lookup"><span data-stu-id="20580-285">When you're troubleshooting gateway-related issues, look for error level events in hello event viewer.</span></span>

![Data Management Gateway loggas i Loggboken](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
