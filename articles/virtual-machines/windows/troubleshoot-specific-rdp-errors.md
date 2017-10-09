---
title: "aaaSpecific RDP-felmeddelanden för virtuella datorer i Azure | Microsoft Docs"
description: "Förstå felmeddelanden som kan visas när du försöker använda Fjärrskrivbord anslutning tooa Windows virtuell dator i Azure"
keywords: "Remote desktop-fel, fel anslutning till fjärrskrivbord kan inte ansluta tooVM, felsökning: fjärrskrivbord"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 8e1551be23e696bd60adbd76c3e1ea86d9dd11aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-specific-rdp-error-messages-tooa-windows-vm-in-azure"></a><span data-ttu-id="4decb-104">Felsöka specifika RDP fel meddelanden tooa Windows VM i Azure</span><span class="sxs-lookup"><span data-stu-id="4decb-104">Troubleshooting specific RDP error messages tooa Windows VM in Azure</span></span>
<span data-ttu-id="4decb-105">Du kan få ett visst felmeddelande när du använder Fjärrskrivbord anslutning tooa Windows virtuell dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="4decb-105">You may receive a specific error message when using Remote Desktop connection tooa Windows virtual machine (VM) in Azure.</span></span> <span data-ttu-id="4decb-106">Det här artikeln beskriver några av hello vanligaste felmeddelandena uppstod, tillsammans med felsökning steg tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="4decb-106">This article details some of hello more common error messages encountered, along with troubleshooting steps tooresolve them.</span></span> <span data-ttu-id="4decb-107">Om du har problem med anslutning tooyour VM med RDP men vill inte påträffar ett visst felmeddelande går hello [felsökningsguide för fjärrskrivbord](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4decb-107">If you are having issues connecting tooyour VM using RDP but do not encounter a specific error message, see hello [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="4decb-108">Information om specifika felmeddelanden finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="4decb-108">For information on specific error messages, see hello following:</span></span>

* <span data-ttu-id="4decb-109">[hello Fjärrsessionen kopplades från eftersom det finns inga tillgängliga servrar för fjärrskrivbordslicenser tooprovide en licens](#rdplicense).</span><span class="sxs-lookup"><span data-stu-id="4decb-109">[hello remote session was disconnected because there are no Remote Desktop License Servers available tooprovide a license](#rdplicense).</span></span>
* <span data-ttu-id="4decb-110">[Fjärrskrivbordet kan inte hitta hello ”datornamn”](#rdpname).</span><span class="sxs-lookup"><span data-stu-id="4decb-110">[Remote Desktop can't find hello computer "name"](#rdpname).</span></span>
* <span data-ttu-id="4decb-111">[Ett autentiseringsfel har uppstått. hello lokala säkerhetskontrollen går inte att kontakta](#rdpauth).</span><span class="sxs-lookup"><span data-stu-id="4decb-111">[An authentication error has occurred. hello Local Security Authority cannot be contacted](#rdpauth).</span></span>
* <span data-ttu-id="4decb-112">[Windows-Security-fel: dina autentiseringsuppgifter fungerar inte](#wincred).</span><span class="sxs-lookup"><span data-stu-id="4decb-112">[Windows Security error: Your credentials did not work](#wincred).</span></span>
* <span data-ttu-id="4decb-113">[Den här datorn kan inte ansluta toohello fjärrdatorn](#rdpconnect).</span><span class="sxs-lookup"><span data-stu-id="4decb-113">[This computer can't connect toohello remote computer](#rdpconnect).</span></span>

<a id="rdplicense"></a>

## <a name="hello-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-tooprovide-a-license"></a><span data-ttu-id="4decb-114">hello Fjärrsessionen kopplades från eftersom det finns inga tillgängliga servrar för fjärrskrivbordslicenser tooprovide en licens.</span><span class="sxs-lookup"><span data-stu-id="4decb-114">hello remote session was disconnected because there are no Remote Desktop License Servers available tooprovide a license.</span></span>
<span data-ttu-id="4decb-115">Orsak: hello 120 dagar licensiering respitperiod för hello Remote Desktop-serverrollen har upphört och du behöver tooinstall licenser.</span><span class="sxs-lookup"><span data-stu-id="4decb-115">Cause: hello 120-day licensing grace period for hello Remote Desktop Server role has expired and you need tooinstall licenses.</span></span>

<span data-ttu-id="4decb-116">Spara en kopia av hello RDP-filen från hello-portalen som en lösning och kör kommandot på tooconnect en PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="4decb-116">As a workaround, save a local copy of hello RDP file from hello portal and run this command at a PowerShell command prompt tooconnect.</span></span> <span data-ttu-id="4decb-117">Det här steget inaktiverar licensiering för bara anslutningen:</span><span class="sxs-lookup"><span data-stu-id="4decb-117">This step disables licensing for just that connection:</span></span>

        mstsc <File name>.RDP /admin

<span data-ttu-id="4decb-118">Om du inte verkligen behöver fler än två samtidiga Fjärrskrivbord anslutningar toohello VM, kan du använda Serverhanteraren tooremove hello Remote Desktop-serverrollen.</span><span class="sxs-lookup"><span data-stu-id="4decb-118">If you don't actually need more than two simultaneous Remote Desktop connections toohello VM, you can use Server Manager tooremove hello Remote Desktop Server role.</span></span>

<span data-ttu-id="4decb-119">Mer information finns i blogginlägget hello [Azure VM misslyckas med ”ingen licens fjärrskrivbordsservrar tillgänglig”](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span><span class="sxs-lookup"><span data-stu-id="4decb-119">For more information, see hello blog post [Azure VM fails with "No Remote Desktop License Servers available"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).</span></span>

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-hello-computer-name"></a><span data-ttu-id="4decb-120">Fjärrskrivbordet kan inte hitta hello dator ”name”.</span><span class="sxs-lookup"><span data-stu-id="4decb-120">Remote Desktop can't find hello computer "name".</span></span>
<span data-ttu-id="4decb-121">Orsak: hello fjärrskrivbordsklienten på datorn kan inte lösa hello namnet på hello dator i hello inställningarna för hello RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="4decb-121">Cause: hello Remote Desktop client on your computer can't resolve hello name of hello computer in hello settings of hello RDP file.</span></span>

<span data-ttu-id="4decb-122">Möjliga lösningar:</span><span class="sxs-lookup"><span data-stu-id="4decb-122">Possible solutions:</span></span>

* <span data-ttu-id="4decb-123">Om du är på intranätet kan du kontrollera att datorn har åtkomst toohello proxyserver och kan skicka tooit för HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="4decb-123">If you're on an organization's intranet, make sure that your computer has access toohello proxy server and can send HTTPS traffic tooit.</span></span>
* <span data-ttu-id="4decb-124">Om du använder en lokalt lagrad RDP-fil, försöka använda hello något som genereras av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4decb-124">If you're using a locally stored RDP file, try using hello one that's generated by hello portal.</span></span> <span data-ttu-id="4decb-125">Det här steget säkerställer att hello DNS-namn för hello virtuell dator eller hello Molntjänsten och hello endpoint-port för hello VM.</span><span class="sxs-lookup"><span data-stu-id="4decb-125">This step ensures that you have hello correct DNS name for hello virtual machine, or hello cloud service and hello endpoint port of hello VM.</span></span> <span data-ttu-id="4decb-126">Här är ett exempel på en RDP-fil som genererats av hello portal:</span><span class="sxs-lookup"><span data-stu-id="4decb-126">Here is a sample RDP file generated by hello portal:</span></span>
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

<span data-ttu-id="4decb-127">hello adress del av den här RDP-filen har:</span><span class="sxs-lookup"><span data-stu-id="4decb-127">hello address portion of this RDP file has:</span></span>

* <span data-ttu-id="4decb-128">hello fullständigt kvalificerade domännamnet för hello Molntjänsten som innehåller hello VM (”tailspin-azdatatier.cloudapp.net” i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="4decb-128">hello fully qualified domain name of hello cloud service that contains hello VM ("tailspin-azdatatier.cloudapp.net" in this example).</span></span>
* <span data-ttu-id="4decb-129">hello externa TCP-porten för hello slutpunkten för Remote Desktop-trafik (55919).</span><span class="sxs-lookup"><span data-stu-id="4decb-129">hello external TCP port of hello endpoint for Remote Desktop traffic (55919).</span></span>

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-hello-local-security-authority-cannot-be-contacted"></a><span data-ttu-id="4decb-130">Ett autentiseringsfel har uppstått.</span><span class="sxs-lookup"><span data-stu-id="4decb-130">An authentication error has occurred.</span></span> <span data-ttu-id="4decb-131">hello lokala säkerhetskontrollen kan inte kontaktas.</span><span class="sxs-lookup"><span data-stu-id="4decb-131">hello Local Security Authority cannot be contacted.</span></span>
<span data-ttu-id="4decb-132">Orsak: hello mål VM kan inte hitta hello säkerhetskontrollen i hello användaren del av dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4decb-132">Cause: hello target VM can't locate hello security authority in hello user name portion of your credentials.</span></span>

<span data-ttu-id="4decb-133">Om användarnamnet är i form av hello *SecurityAuthority*\\*användarnamn* (exempel: corp\användare1), hello *SecurityAuthority* del är antingen hello VM datornamnet (för lokala säkerhetskontrollen för hello) eller en Active Directory-domännamn.</span><span class="sxs-lookup"><span data-stu-id="4decb-133">When your user name is in hello form *SecurityAuthority*\\*UserName* (example: CORP\User1), hello *SecurityAuthority* portion is either hello VM's computer name (for hello local security authority) or an Active Directory domain name.</span></span>

<span data-ttu-id="4decb-134">Möjliga lösningar:</span><span class="sxs-lookup"><span data-stu-id="4decb-134">Possible solutions:</span></span>

* <span data-ttu-id="4decb-135">Om hello konto är lokala toohello VM, se till att hello VM-namnet har stavats korrekt.</span><span class="sxs-lookup"><span data-stu-id="4decb-135">If hello account is local toohello VM, make sure that hello VM name is spelled correctly.</span></span>
* <span data-ttu-id="4decb-136">Om hello konto är på en Active Directory-domän, stavningskontroll hello hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="4decb-136">If hello account is on an Active Directory domain, check hello spelling of hello domain name.</span></span>
* <span data-ttu-id="4decb-137">Om det är ett domänkonto för Active Directory och hello domännamnet är rättstavat, kontrollera att en domänkontrollant i domänen.</span><span class="sxs-lookup"><span data-stu-id="4decb-137">If it is an Active Directory domain account and hello domain name is spelled correctly, verify that a domain controller is available in that domain.</span></span> <span data-ttu-id="4decb-138">Det är ett vanligt problem i virtuella Azure-nätverk som innehåller domänkontrollanter som en domänkontrollant är inte tillgänglig eftersom den inte har startats.</span><span class="sxs-lookup"><span data-stu-id="4decb-138">It's a common issue in Azure virtual networks that contain domain controllers that a domain controller is unavailable because it hasn't been started.</span></span> <span data-ttu-id="4decb-139">Som en tillfällig lösning kan du använda ett lokalt administratörskonto i stället för ett domänkonto.</span><span class="sxs-lookup"><span data-stu-id="4decb-139">As a workaround, you can use a local administrator account instead of a domain account.</span></span>

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a><span data-ttu-id="4decb-140">Windows-Security-fel: dina autentiseringsuppgifter fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="4decb-140">Windows Security error: Your credentials did not work.</span></span>
<span data-ttu-id="4decb-141">Orsak: hello mål VM kan inte verifiera ditt kontonamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="4decb-141">Cause: hello target VM can't validate your account name and password.</span></span>

<span data-ttu-id="4decb-142">En Windows-baserad dator kan validera hello autentiseringsuppgifterna för ett lokalt konto eller ett domänkonto.</span><span class="sxs-lookup"><span data-stu-id="4decb-142">A Windows-based computer can validate hello credentials of either a local account or a domain account.</span></span>

* <span data-ttu-id="4decb-143">Använd för lokala konton hello *ComputerName*\\*användarnamn* syntax (exempel: SQL1\Admin4798).</span><span class="sxs-lookup"><span data-stu-id="4decb-143">For local accounts, use hello *ComputerName*\\*UserName* syntax (example: SQL1\Admin4798).</span></span>
* <span data-ttu-id="4decb-144">För domänkonton och använda hello *DomainName*\\*användarnamn* syntax (exempel: CONTOSO\peterodman).</span><span class="sxs-lookup"><span data-stu-id="4decb-144">For domain accounts, use hello *DomainName*\\*UserName* syntax (example: CONTOSO\peterodman).</span></span>

<span data-ttu-id="4decb-145">Om du har befordrat VM tooa domänkontrollanten i en ny Active Directory-skog, hello lokala administratörskontot som du loggade in med konverteras tooan motsvarande konto med hello samma lösenord i hello ny skog och domän.</span><span class="sxs-lookup"><span data-stu-id="4decb-145">If you have promoted your VM tooa domain controller in a new Active Directory forest, hello local administrator account that you signed in with is converted tooan equivalent account with hello same password in hello new forest and domain.</span></span> <span data-ttu-id="4decb-146">hello lokalt konto raderas sedan.</span><span class="sxs-lookup"><span data-stu-id="4decb-146">hello local account is then deleted.</span></span>

<span data-ttu-id="4decb-147">Till exempel om du har loggat in med hello lokalt konto DC1\DCAdmin och sedan upphöja hello virtuell dator som en domänkontrollant i en ny skog för hello corp.contoso.com domänen, hello DC1\DCAdmin lokalt konto tas bort och ett nytt domänkonto (CORP\DCAdmin ) skapas med hello samma lösenord.</span><span class="sxs-lookup"><span data-stu-id="4decb-147">For example, if you signed in with hello local account DC1\DCAdmin, and then promoted hello virtual machine as a domain controller in a new forest for hello corp.contoso.com domain, hello DC1\DCAdmin local account gets deleted and a new domain account (CORP\DCAdmin) is created with hello same password.</span></span>

<span data-ttu-id="4decb-148">Kontrollera att kontonamnet hello är ett namn att hello virtuell dator kan verifiera som ett giltigt konto och hello lösenordet är korrekt.</span><span class="sxs-lookup"><span data-stu-id="4decb-148">Make sure that hello account name is a name that hello virtual machine can verify as a valid account, and that hello password is correct.</span></span>

<span data-ttu-id="4decb-149">Om du behöver toochange hello lösenord hello lokalt administratörskonto finns [hur tooreset ett lösenord eller hello fjärrskrivbord service för Windows-datorer](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4decb-149">If you need toochange hello password of hello local administrator account, see [How tooreset a password or hello Remote Desktop service for Windows virtual machines](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-toohello-remote-computer"></a><span data-ttu-id="4decb-150">Den här datorn kan inte ansluta toohello fjärrdatorn.</span><span class="sxs-lookup"><span data-stu-id="4decb-150">This computer can't connect toohello remote computer.</span></span>
<span data-ttu-id="4decb-151">Orsak: hello-konto som har använt tooconnect saknar fjärrskrivbord inloggning rättigheter.</span><span class="sxs-lookup"><span data-stu-id="4decb-151">Cause: hello account that's used tooconnect does not have Remote Desktop sign-in rights.</span></span>

<span data-ttu-id="4decb-152">Alla Windows-datorer har ett fjärrskrivbord lokala användargruppen som innehåller hello konton och grupper som kan logga in på den via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="4decb-152">Every Windows computer has a Remote Desktop users local group, which contains hello accounts and groups that can sign into it remotely.</span></span> <span data-ttu-id="4decb-153">Medlemmar i gruppen lokala administratörer för hello har också åtkomst, även om dessa konton inte visas i lokala gruppen för hello Remote Desktop-användare.</span><span class="sxs-lookup"><span data-stu-id="4decb-153">Members of hello local administrators group also have access, even though those accounts are not listed in hello Remote Desktop users local group.</span></span> <span data-ttu-id="4decb-154">För domänanslutna datorer innehåller hello Domänadministratörer för domänen hello också hello lokala administratörsgruppen.</span><span class="sxs-lookup"><span data-stu-id="4decb-154">For domain-joined machines, hello local administrators group also contains hello domain administrators for hello domain.</span></span>

<span data-ttu-id="4decb-155">Se till att hello kontot du använder tooconnect med har Fjärrskrivbord inloggning rättigheter.</span><span class="sxs-lookup"><span data-stu-id="4decb-155">Make sure that hello account you're using tooconnect with has Remote Desktop sign-in rights.</span></span> <span data-ttu-id="4decb-156">Som en tillfällig lösning kan använda en domän eller en lokal administratör konto tooconnect via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="4decb-156">As a workaround, use a domain or local administrator account tooconnect over Remote Desktop.</span></span> <span data-ttu-id="4decb-157">tooadd Hej önskat konto toohello fjärrskrivbord lokala användargruppen, hello Microsoft Management Console snapin-modulen (**Systemverktyg > lokala användare och grupper > grupper > användare av fjärrskrivbord**).</span><span class="sxs-lookup"><span data-stu-id="4decb-157">tooadd hello desired account toohello Remote Desktop users local group, use hello Microsoft Management Console snap-in (**System Tools > Local Users and Groups > Groups > Remote Desktop Users**).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4decb-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4decb-158">Next steps</span></span>
<span data-ttu-id="4decb-159">Om ingen av dessa fel uppstod och du har ett okänt problem med att ansluta med RDP, se hello [felsökningsguide för fjärrskrivbord](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4decb-159">If none of these errors occurred and you have an unknown issue with connecting using RDP, see hello [troubleshooting guide for Remote Desktop](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="4decb-160">Felsökningssteg för åtkomst till program som körs på en virtuell dator, finns i [Felsök åtkomst tooan program som körs på en Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4decb-160">For troubleshooting steps in accessing applications running on a VM, see [Troubleshoot access tooan application running on an Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="4decb-161">Om du har problem med hjälp av SSH (Secure Shell) tooconnect tooa Linux VM i Azure, se [felsökning av SSH-anslutningar tooa Linux VM i Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4decb-161">If you are having issues using Secure Shell (SSH) tooconnect tooa Linux VM in Azure, see [Troubleshoot SSH connections tooa Linux VM in Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

