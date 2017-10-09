---
title: Skapa RemoteApp-hybridsamlingar aaaTroubleshoot | Microsoft Docs
description: "Lär dig hur tootroubleshoot RemoteApp hybrid samling fel vid skapande av"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: b32033ee-8d52-4e74-bb78-86ca873c34e2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: cc426f24bd0c349a8862d54acbafa9cf84446f4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Felsök att skapa hybridsamlingar i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

En hybridsamling är värd för och lagrar data i hello Azure-molnet men kan användare komma åt data och resurser i ditt lokala nätverk. Användarna kan öppna programmen genom att logga in med sina inloggningsuppgifter till företaget , synkroniserat eller federerat med Azure Active Directory. Du kan distribuera en hybridsamling som använder ett befintligt virtuellt Azure-nätverk eller skapa ett nytt virtuellt nätverk. Vi rekommenderar att du skapar eller använder ett undernät för virtuellt nätverk med ett CIDR-intervall som är tillräckligt stor för förväntade framtida tillväxt för Azure RemoteApp.

Samlingen ännu inte har skapats? Se [skapa en hybridsamling](remoteapp-create-hybrid-deployment.md) hello anvisningar.

Om du har problem med att skapa din samling, eller om hello samlingen inte fungerar hello sätt som du tror att det ska, se hello följande information.

## <a name="your-image-is-invalid"></a>Bilden är ogiltig
Om du ser ett meddelande som ”GoldImageInvalid” när du väntar Azure tooprovision din samling, innebär det att mallavbildningen inte uppfyller hello [definierat avbildningen krav](remoteapp-imagereqs.md). Därför gå läsa de [krav](remoteapp-imagereqs.md), åtgärda avbildningen och försök toocreate samlingen igen.

## <a name="does-your-vnet-have-network-security-groups-defined"></a>Har ditt VNET nätverkssäkerhetsgrupper definierats?
Om du har nätverkssäkerhetsgrupper som definierats i hello undernät som du använder för samlingen, se till att dessa [URL: er och portar](remoteapp-ports.md) är tillgänglig från undernätet.

Du kan lägga till ytterligare nätverk Säkerhet grupper toohello virtuella datorer distribueras av du hello undernät för bättre kontroll.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Använder du DNS-servrar? Och de som är tillgängliga från ditt VNET undernät?
> [!NOTE]
> Du har toomake att hello DNS-servrar i ditt VNET alltid är igång och alltid kan tooresolve hello virtuella datorer som finns i hello virtuella nätverk. Använd inte Google DNS för den här.
> 
> 

För hybridsamlingar använder du DNS-servrarna. Anger du dem i ditt nätverk Konfigurationsschemat eller via hello management portal när du skapar ditt virtuella nätverk. DNS-servrar som används i hello ordning som de anges i ett failover-sätt (som skillnad från tooround robin).  
Se för[namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) toomake till DNS-servrarna är konfigurerade correcly.

Kontrollera att hello DNS-servrar för samlingen är tillgängliga och från hello VNET undernät som du angav för den här samlingen.

Exempel:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definiera din DNS](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Använder du en Active Directory-domänkontrollant i samlingen?
För närvarande bara en Active Directory-domän kan associeras med Azure RemoteApp. Hej hybridsamlingen stöder enbart Azure Active Directory-konton som har synkroniserats med DirSync-verktyget från en Windows Server Active Directory-distribution mer specifikt antingen synkroniserats med alternativet för hello Lösenordssynkronisering eller synkroniseras med Active Directory Federation Services (AD FS) konfigurerats. Du behöver toocreate anpassade domäner som matchar hello UPN domänsuffixet för din lokala domän och konfigurera katalogintegrering.

Se [konfigurera Active Directory för Azure RemoteApp](remoteapp-ad.md) för mer information.

Kontrollera att hello domän informationen är giltiga och hello domänkontrollanten är nåbar från hello skapas den virtuella datorn i hello undernät som används för Azure RemoteApp. Kontrollera också att hello-tjänstkontot autentiseringsuppgifterna har behörigheter tooadd datorer toohello tillhandahållit domän och som hello Annonsnamn angivna kan matchas från hello DNS i hello virtuella nätverk.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Vilket domännamn som du angav när du skapade din samling?
hello domännamnet du har skapat eller lagts till måste vara ett internt domännamn (inte dina Azure AD domain name) och måste vara i DNS-format som kan lösas (contoso.local). Till exempel du har ett internt namn för Active Directory (contoso.local) och en Active Directory-UPN (contoso.com) – du har toouse hello interna namn när du skapar din samling.

