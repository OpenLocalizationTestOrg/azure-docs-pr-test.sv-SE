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
# <a name="troubleshooting-specific-rdp-error-messages-tooa-windows-vm-in-azure"></a>Felsöka specifika RDP fel meddelanden tooa Windows VM i Azure
Du kan få ett visst felmeddelande när du använder Fjärrskrivbord anslutning tooa Windows virtuell dator (VM) i Azure. Det här artikeln beskriver några av hello vanligaste felmeddelandena uppstod, tillsammans med felsökning steg tooresolve dem. Om du har problem med anslutning tooyour VM med RDP men vill inte påträffar ett visst felmeddelande går hello [felsökningsguide för fjärrskrivbord](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Information om specifika felmeddelanden finns hello följande:

* [hello Fjärrsessionen kopplades från eftersom det finns inga tillgängliga servrar för fjärrskrivbordslicenser tooprovide en licens](#rdplicense).
* [Fjärrskrivbordet kan inte hitta hello ”datornamn”](#rdpname).
* [Ett autentiseringsfel har uppstått. hello lokala säkerhetskontrollen går inte att kontakta](#rdpauth).
* [Windows-Security-fel: dina autentiseringsuppgifter fungerar inte](#wincred).
* [Den här datorn kan inte ansluta toohello fjärrdatorn](#rdpconnect).

<a id="rdplicense"></a>

## <a name="hello-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-tooprovide-a-license"></a>hello Fjärrsessionen kopplades från eftersom det finns inga tillgängliga servrar för fjärrskrivbordslicenser tooprovide en licens.
Orsak: hello 120 dagar licensiering respitperiod för hello Remote Desktop-serverrollen har upphört och du behöver tooinstall licenser.

Spara en kopia av hello RDP-filen från hello-portalen som en lösning och kör kommandot på tooconnect en PowerShell-Kommandotolken. Det här steget inaktiverar licensiering för bara anslutningen:

        mstsc <File name>.RDP /admin

Om du inte verkligen behöver fler än två samtidiga Fjärrskrivbord anslutningar toohello VM, kan du använda Serverhanteraren tooremove hello Remote Desktop-serverrollen.

Mer information finns i blogginlägget hello [Azure VM misslyckas med ”ingen licens fjärrskrivbordsservrar tillgänglig”](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-hello-computer-name"></a>Fjärrskrivbordet kan inte hitta hello dator ”name”.
Orsak: hello fjärrskrivbordsklienten på datorn kan inte lösa hello namnet på hello dator i hello inställningarna för hello RDP-filen.

Möjliga lösningar:

* Om du är på intranätet kan du kontrollera att datorn har åtkomst toohello proxyserver och kan skicka tooit för HTTPS-trafik.
* Om du använder en lokalt lagrad RDP-fil, försöka använda hello något som genereras av hello-portalen. Det här steget säkerställer att hello DNS-namn för hello virtuell dator eller hello Molntjänsten och hello endpoint-port för hello VM. Här är ett exempel på en RDP-fil som genererats av hello portal:
  
        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

hello adress del av den här RDP-filen har:

* hello fullständigt kvalificerade domännamnet för hello Molntjänsten som innehåller hello VM (”tailspin-azdatatier.cloudapp.net” i det här exemplet).
* hello externa TCP-porten för hello slutpunkten för Remote Desktop-trafik (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-hello-local-security-authority-cannot-be-contacted"></a>Ett autentiseringsfel har uppstått. hello lokala säkerhetskontrollen kan inte kontaktas.
Orsak: hello mål VM kan inte hitta hello säkerhetskontrollen i hello användaren del av dina autentiseringsuppgifter.

Om användarnamnet är i form av hello *SecurityAuthority*\\*användarnamn* (exempel: corp\användare1), hello *SecurityAuthority* del är antingen hello VM datornamnet (för lokala säkerhetskontrollen för hello) eller en Active Directory-domännamn.

Möjliga lösningar:

* Om hello konto är lokala toohello VM, se till att hello VM-namnet har stavats korrekt.
* Om hello konto är på en Active Directory-domän, stavningskontroll hello hello domännamn.
* Om det är ett domänkonto för Active Directory och hello domännamnet är rättstavat, kontrollera att en domänkontrollant i domänen. Det är ett vanligt problem i virtuella Azure-nätverk som innehåller domänkontrollanter som en domänkontrollant är inte tillgänglig eftersom den inte har startats. Som en tillfällig lösning kan du använda ett lokalt administratörskonto i stället för ett domänkonto.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows-Security-fel: dina autentiseringsuppgifter fungerar inte.
Orsak: hello mål VM kan inte verifiera ditt kontonamn och lösenord.

En Windows-baserad dator kan validera hello autentiseringsuppgifterna för ett lokalt konto eller ett domänkonto.

* Använd för lokala konton hello *ComputerName*\\*användarnamn* syntax (exempel: SQL1\Admin4798).
* För domänkonton och använda hello *DomainName*\\*användarnamn* syntax (exempel: CONTOSO\peterodman).

Om du har befordrat VM tooa domänkontrollanten i en ny Active Directory-skog, hello lokala administratörskontot som du loggade in med konverteras tooan motsvarande konto med hello samma lösenord i hello ny skog och domän. hello lokalt konto raderas sedan.

Till exempel om du har loggat in med hello lokalt konto DC1\DCAdmin och sedan upphöja hello virtuell dator som en domänkontrollant i en ny skog för hello corp.contoso.com domänen, hello DC1\DCAdmin lokalt konto tas bort och ett nytt domänkonto (CORP\DCAdmin ) skapas med hello samma lösenord.

Kontrollera att kontonamnet hello är ett namn att hello virtuell dator kan verifiera som ett giltigt konto och hello lösenordet är korrekt.

Om du behöver toochange hello lösenord hello lokalt administratörskonto finns [hur tooreset ett lösenord eller hello fjärrskrivbord service för Windows-datorer](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-toohello-remote-computer"></a>Den här datorn kan inte ansluta toohello fjärrdatorn.
Orsak: hello-konto som har använt tooconnect saknar fjärrskrivbord inloggning rättigheter.

Alla Windows-datorer har ett fjärrskrivbord lokala användargruppen som innehåller hello konton och grupper som kan logga in på den via fjärranslutning. Medlemmar i gruppen lokala administratörer för hello har också åtkomst, även om dessa konton inte visas i lokala gruppen för hello Remote Desktop-användare. För domänanslutna datorer innehåller hello Domänadministratörer för domänen hello också hello lokala administratörsgruppen.

Se till att hello kontot du använder tooconnect med har Fjärrskrivbord inloggning rättigheter. Som en tillfällig lösning kan använda en domän eller en lokal administratör konto tooconnect via fjärrskrivbord. tooadd Hej önskat konto toohello fjärrskrivbord lokala användargruppen, hello Microsoft Management Console snapin-modulen (**Systemverktyg > lokala användare och grupper > grupper > användare av fjärrskrivbord**).

## <a name="next-steps"></a>Nästa steg
Om ingen av dessa fel uppstod och du har ett okänt problem med att ansluta med RDP, se hello [felsökningsguide för fjärrskrivbord](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Felsökningssteg för åtkomst till program som körs på en virtuell dator, finns i [Felsök åtkomst tooan program som körs på en Azure VM](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Om du har problem med hjälp av SSH (Secure Shell) tooconnect tooa Linux VM i Azure, se [felsökning av SSH-anslutningar tooa Linux VM i Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

