---
title: "aaaAzure molntjänster roller vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor om Azure-molntjänster. Svar på vanliga frågor om certifikat, webbprogram, och arbetsroller."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: b07a1990e031e60ae919a5f7c636945b89c7d3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-services-faq"></a><span data-ttu-id="7f958-104">Vanliga frågor och svar om Cloud Services</span><span class="sxs-lookup"><span data-stu-id="7f958-104">Cloud Services FAQ</span></span>
<span data-ttu-id="7f958-105">Den här artikeln besvarar några vanliga frågor om Microsoft Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="7f958-105">This article answers some frequently asked questions about Microsoft Azure Cloud Services.</span></span> <span data-ttu-id="7f958-106">Du kan också besöka hello [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) allmän Azure priser och support information.</span><span class="sxs-lookup"><span data-stu-id="7f958-106">You can also visit hello [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing and support information.</span></span> <span data-ttu-id="7f958-107">Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.</span><span class="sxs-lookup"><span data-stu-id="7f958-107">You can also consult hello [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

## <a name="certificates"></a><span data-ttu-id="7f958-108">Certifikat</span><span class="sxs-lookup"><span data-stu-id="7f958-108">Certificates</span></span>
### <a name="where-should-i-install-my-certificate"></a><span data-ttu-id="7f958-109">När bör jag installera mitt certifikat?</span><span class="sxs-lookup"><span data-stu-id="7f958-109">Where should I install my certificate?</span></span>
* <span data-ttu-id="7f958-110">**Min**</span><span class="sxs-lookup"><span data-stu-id="7f958-110">**My**</span></span>  
  <span data-ttu-id="7f958-111">Programmet certifikat med privat nyckel (\*.pfx, \*.p12).</span><span class="sxs-lookup"><span data-stu-id="7f958-111">Application Certificate with private key (\*.pfx, \*.p12).</span></span>
* <span data-ttu-id="7f958-112">**CERTIFIKATUTFÄRDARE**</span><span class="sxs-lookup"><span data-stu-id="7f958-112">**CA**</span></span>  
  <span data-ttu-id="7f958-113">Alla mellanliggande certifikat går i det här arkivet (princip och Sub CA: er).</span><span class="sxs-lookup"><span data-stu-id="7f958-113">All your intermediate certificates go in this store (Policy and Sub CAs).</span></span>
* <span data-ttu-id="7f958-114">**ROT**</span><span class="sxs-lookup"><span data-stu-id="7f958-114">**ROOT**</span></span>  
  <span data-ttu-id="7f958-115">hello rotcertifikatutfärdaren store, så att dina viktigaste rot Certifikatutfärdarens certifikat ska gå hit.</span><span class="sxs-lookup"><span data-stu-id="7f958-115">hello root CA store, so your main root CA cert should go here.</span></span>

### <a name="i-cant-remove-expired-certificate"></a><span data-ttu-id="7f958-116">Jag kan inte ta bort utgångna certifikat</span><span class="sxs-lookup"><span data-stu-id="7f958-116">I can't remove expired certificate</span></span>
<span data-ttu-id="7f958-117">Azure förhindrar att du tar bort ett certifikat medan den används.</span><span class="sxs-lookup"><span data-stu-id="7f958-117">Azure prevents you from removing a certificate while it is in use.</span></span> <span data-ttu-id="7f958-118">Du måste tooeither delete hello distribution som använder hello certifikat eller hello distribution med en annan eller förnyat certifikat.</span><span class="sxs-lookup"><span data-stu-id="7f958-118">You need tooeither delete hello deployment that uses hello certificate, or update hello deployment with a different or renewed certificate.</span></span>

### <a name="delete-an-expired-certificate"></a><span data-ttu-id="7f958-119">Ta bort ett utgånget certifikat</span><span class="sxs-lookup"><span data-stu-id="7f958-119">Delete an expired certificate</span></span>
<span data-ttu-id="7f958-120">Så länge hello certifikatet inte har, kan du använda hello [ta bort AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet tooremove ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="7f958-120">As long as hello certificate is not in use, you can use hello [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet tooremove a certificate.</span></span>

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a><span data-ttu-id="7f958-121">Jag har gått ut certifikat med namnet Windows Azure-tjänsthantering för tillägg</span><span class="sxs-lookup"><span data-stu-id="7f958-121">I have expired certificates named Windows Azure Service Management for Extensions</span></span>
<span data-ttu-id="7f958-122">Dessa certifikat skapas när ett tillägg läggs toohello molntjänst som hello Remote Desktop-tillägget.</span><span class="sxs-lookup"><span data-stu-id="7f958-122">These certificates are created whenever an extension is added toohello cloud service such as hello Remote Desktop extension.</span></span> <span data-ttu-id="7f958-123">Dessa certifikat används bara för att kryptera och dekryptera hello privata konfigureringen av hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="7f958-123">These certificates are only used for encrypting and decrypting hello private configuration of hello extension.</span></span> <span data-ttu-id="7f958-124">Det spelar ingen roll om dessa certifikat förfaller.</span><span class="sxs-lookup"><span data-stu-id="7f958-124">It does not matter if these certificates expire.</span></span> <span data-ttu-id="7f958-125">Utgångsdatum för Hej kontrolleras inte.</span><span class="sxs-lookup"><span data-stu-id="7f958-125">hello expiration date is not checked.</span></span>

### <a name="certificates-i-have-deleted-keep-reappearing"></a><span data-ttu-id="7f958-126">Certifikat som jag har tagit bort fortfarande visas</span><span class="sxs-lookup"><span data-stu-id="7f958-126">Certificates I have deleted keep reappearing</span></span>
<span data-ttu-id="7f958-127">Dessa fortfarande visas troligen på grund av ett verktyg som du använder, till exempel för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f958-127">These keep reappearing most likely because of a tool you're using, such as Visual Studio.</span></span> <span data-ttu-id="7f958-128">När du ansluter till ett verktyg som använder ett certifikat blir igen överförda tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7f958-128">Whenever you reconnect with a tool that is using a certificate, it will again be uploaded tooAzure.</span></span>

### <a name="my-certificates-keep-disappearing"></a><span data-ttu-id="7f958-129">Mina certifikat försvinner</span><span class="sxs-lookup"><span data-stu-id="7f958-129">My certificates keep disappearing</span></span>
<span data-ttu-id="7f958-130">När hello virtuell datorinstans återvinns, förloras alla lokala ändringar.</span><span class="sxs-lookup"><span data-stu-id="7f958-130">When hello virtual machine instance recycles, all local changes are lost.</span></span> <span data-ttu-id="7f958-131">Använd en [startaktivitet](cloud-services-startup-tasks.md) tooinstall certifikat toohello virtuella datorn varje gång hello roll startar.</span><span class="sxs-lookup"><span data-stu-id="7f958-131">Use a [startup task](cloud-services-startup-tasks.md) tooinstall certificates toohello virtual machine each time hello role starts.</span></span>

### <a name="i-cannot-find-my-management-certificates-in-hello-portal"></a><span data-ttu-id="7f958-132">Jag kan inte hitta min hanteringscertifikat i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="7f958-132">I cannot find my management certificates in hello portal</span></span>
<span data-ttu-id="7f958-133">[Hanteringscertifikat](../azure-api-management-certs.md) är bara tillgängliga i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7f958-133">[Management certificates](../azure-api-management-certs.md) are only available in hello Azure Classic Portal.</span></span> <span data-ttu-id="7f958-134">certifikat används inte av hello aktuella Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7f958-134">hello current Azure portal does not use management certificates.</span></span> 

### <a name="how-can-i-disable-a-management-certificate"></a><span data-ttu-id="7f958-135">Hur kan jag inaktivera ett certifikat?</span><span class="sxs-lookup"><span data-stu-id="7f958-135">How can I disable a management certificate?</span></span>
<span data-ttu-id="7f958-136">[Hanteringscertifikat](../azure-api-management-certs.md) kan inte inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="7f958-136">[Management certificates](../azure-api-management-certs.md) cannot be disabled.</span></span> <span data-ttu-id="7f958-137">Du tar bort dem via hello klassiska Azure-portalen när du inte vill att de toobe används längre.</span><span class="sxs-lookup"><span data-stu-id="7f958-137">You delete them through hello Azure Classic Portal when you do not want them toobe used anymore.</span></span>

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a><span data-ttu-id="7f958-138">Hur skapar ett SSL-certifikat för en specifik IP-adress?</span><span class="sxs-lookup"><span data-stu-id="7f958-138">How do I create an SSL certificate for a specific IP address?</span></span>
<span data-ttu-id="7f958-139">Följ anvisningarna hello i hello [skapa certifikat självstudiekursen](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="7f958-139">Follow hello directions in hello [create a certificate tutorial](cloud-services-certs-create.md).</span></span> <span data-ttu-id="7f958-140">Använd hello IP-adress som hello DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="7f958-140">Use hello IP address as hello DNS Name.</span></span>

## <a name="security"></a><span data-ttu-id="7f958-141">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="7f958-141">Security</span></span>
### <a name="disable-ssl-30"></a><span data-ttu-id="7f958-142">Inaktivera SSL 3.0</span><span class="sxs-lookup"><span data-stu-id="7f958-142">Disable SSL 3.0</span></span>
<span data-ttu-id="7f958-143">toodisable SSL 3.0 och Använd TLS-säkerhet, skapa en startåtgärd som beskrivs i det här blogginlägget: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span><span class="sxs-lookup"><span data-stu-id="7f958-143">toodisable SSL 3.0 and use TLS security, create a startup task which is documented on this blog post: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span></span>

### <a name="add-nosniff-tooyour-website"></a><span data-ttu-id="7f958-144">Lägg till **nosniff** tooyour webbplats</span><span class="sxs-lookup"><span data-stu-id="7f958-144">Add **nosniff** tooyour website</span></span>
<span data-ttu-id="7f958-145">tooprevent klienter från identifiering hello MIME-typer, lägga till en inställning i din *web.config* fil.</span><span class="sxs-lookup"><span data-stu-id="7f958-145">tooprevent clients from sniffing hello MIME types, add a setting in your *web.config* file.</span></span>

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

<span data-ttu-id="7f958-146">Du kan också lägga till detta som en inställning i IIS.</span><span class="sxs-lookup"><span data-stu-id="7f958-146">You can also add this as a setting in IIS.</span></span> <span data-ttu-id="7f958-147">Använd hello följande kommando med hello [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.</span><span class="sxs-lookup"><span data-stu-id="7f958-147">Use hello following command with hello [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a><span data-ttu-id="7f958-148">Anpassa IIS för en webbroll</span><span class="sxs-lookup"><span data-stu-id="7f958-148">Customize IIS for a web role</span></span>
<span data-ttu-id="7f958-149">Använd hello IIS startskript från hello [vanliga uppgifter för Start](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.</span><span class="sxs-lookup"><span data-stu-id="7f958-149">Use hello IIS startup script from hello [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

## <a name="scaling"></a><span data-ttu-id="7f958-150">Skalning</span><span class="sxs-lookup"><span data-stu-id="7f958-150">Scaling</span></span>
### <a name="i-cannot-scale-beyond-x-instances"></a><span data-ttu-id="7f958-151">Det går inte att skala upp X instanser</span><span class="sxs-lookup"><span data-stu-id="7f958-151">I cannot scale beyond X instances</span></span>
<span data-ttu-id="7f958-152">Azure-prenumerationen har en gräns på hello antal kärnor som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="7f958-152">Your Azure Subscription has a limit on hello number of cores you can use.</span></span> <span data-ttu-id="7f958-153">Skalning fungerar inte om du har använt alla hello kärnor som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="7f958-153">Scaling will not work if you have used all hello cores available.</span></span> <span data-ttu-id="7f958-154">Till exempel om du har en gräns på 100 kärnor, innebär det du kan ha 100 A1 storlek virtuella instanser för din molntjänst eller 50 A2 storlek instanser för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7f958-154">For example, if you have a limit of 100 cores, this means you could have 100 A1 sized virtual machine instances for your cloud service, or 50 A2 sized virtual machine instances.</span></span>

## <a name="networking"></a><span data-ttu-id="7f958-155">Nätverk</span><span class="sxs-lookup"><span data-stu-id="7f958-155">Networking</span></span>
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a><span data-ttu-id="7f958-156">Det går inte att reservera en IP-adress i en multi-VIP-molntjänst</span><span class="sxs-lookup"><span data-stu-id="7f958-156">I can't reserve an IP in a multi-VIP cloud service</span></span>
<span data-ttu-id="7f958-157">Kontrollera först att den virtuella dator hello-instansen som du försöker tooreserve hello IP för är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="7f958-157">First, make sure that hello virtual machine instance that you're trying tooreserve hello IP for is turned on.</span></span> <span data-ttu-id="7f958-158">Därefter kontrollerar du att du använder reserverade IP-adresser för besväret hello mellanlagring och produktion distributioner.</span><span class="sxs-lookup"><span data-stu-id="7f958-158">Second, make sure that you're using Reserved IPs for bother hello staging and production deployments.</span></span> <span data-ttu-id="7f958-159">**Inte** ändra hello inställningar medan hello distribution uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="7f958-159">**Do not** change hello settings while hello deployment is upgrading.</span></span>

## <a name="remote-desktop"></a><span data-ttu-id="7f958-160">Fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="7f958-160">Remote desktop</span></span>
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a><span data-ttu-id="7f958-161">Hur gör jag Fjärrskrivbord när jag har en NSG?</span><span class="sxs-lookup"><span data-stu-id="7f958-161">How do I remote desktop when I have an NSG?</span></span>
<span data-ttu-id="7f958-162">Lägg till regler toohello NSG som tillåter trafik på portarna **3389** och **20000**.</span><span class="sxs-lookup"><span data-stu-id="7f958-162">Add rules toohello NSG that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="7f958-163">Fjärrskrivbord använder port **3389**.</span><span class="sxs-lookup"><span data-stu-id="7f958-163">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="7f958-164">Molnet tjänstinstanser belastningsutjämnas, så du inte kan styra vilken instans tooconnect till direkt.</span><span class="sxs-lookup"><span data-stu-id="7f958-164">Cloud Service instances are load balanced, so you can't directly control which instance tooconnect to.</span></span>  <span data-ttu-id="7f958-165">Hej *RemoteForwarder* och *RemoteAccess* agenter hantera RDP-trafik och Tillåt hello klienten toosend en RDP-cookie och ange en enskild instans tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="7f958-165">hello *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow hello client toosend an RDP cookie and specify an individual instance tooconnect to.</span></span>  <span data-ttu-id="7f958-166">Hej *RemoteForwarder* och *RemoteAccess* agenter kräver att port **20000*** öppnas, där blockeras om du har en NSG.</span><span class="sxs-lookup"><span data-stu-id="7f958-166">hello *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>
