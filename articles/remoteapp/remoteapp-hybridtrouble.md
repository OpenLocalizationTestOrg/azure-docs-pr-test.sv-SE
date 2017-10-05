---
title: "Felsöka skapa RemoteApp-hybridsamlingar | Microsoft Docs"
description: "Lär dig hur du felsöker fel för RemoteApp-hybrid samling skapas"
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
ms.openlocfilehash: a486dcb3f994cd78311ee86521a6792a4d57438e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Felsök att skapa hybridsamlingar i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

En hybridsamling är värd för och lagrar data i azuremolnet, men låter användare komma åt data och resurser i ditt lokala nätverk. Användarna kan öppna programmen genom att logga in med sina inloggningsuppgifter till företaget , synkroniserat eller federerat med Azure Active Directory. Du kan distribuera en hybridsamling som använder ett befintligt virtuellt Azure-nätverk eller skapa ett nytt virtuellt nätverk. Vi rekommenderar att du skapar eller använder ett undernät för virtuellt nätverk med ett CIDR-intervall som är tillräckligt stor för förväntade framtida tillväxt för Azure RemoteApp.

Samlingen ännu inte har skapats? Se [skapa en hybridsamling](remoteapp-create-hybrid-deployment.md) anvisningar.

Om du har problem med att skapa din samling, eller om samlingen inte fungerar som du har tänkt, se följande information.

## <a name="your-image-is-invalid"></a>Bilden är ogiltig
Om du ser ett meddelande som ”GoldImageInvalid” när du väntar på att Azure för att etablera samlingen, innebär det att mallavbildningen inte uppfyller den [definierat avbildningen krav](remoteapp-imagereqs.md). Därför gå läsa de [krav](remoteapp-imagereqs.md), åtgärda avbildningen och försök att skapa samlingen igen.

## <a name="does-your-vnet-have-network-security-groups-defined"></a>Har ditt VNET nätverkssäkerhetsgrupper definierats?
Om du har nätverkssäkerhetsgrupper som definierats i undernät som du använder för samlingen, se till att dessa [URL: er och portar](remoteapp-ports.md) är tillgänglig från undernätet.

Du kan lägga till ytterligare nätverkssäkerhetsgrupper på virtuella datorer som distribueras av du i undernät för bättre kontroll.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Använder du DNS-servrar? Och de som är tillgängliga från ditt VNET undernät?
> [!NOTE]
> Du måste se till att DNS-servrar i ditt VNET alltid är igång och alltid kunna matcha de virtuella datorerna i virtuella nätverk. Använd inte Google DNS för den här.
> 
> 

För hybridsamlingar använder du DNS-servrarna. Anger du dem i ditt nätverk Konfigurationsschemat eller via hanteringsportalen när du skapar ditt virtuella nätverk. DNS-servrar som används i den ordning som de anges på ett sätt för växling vid fel (i stället för resursallokering).  
Se [namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) Kontrollera DNS-servrar är konfigurerade correcly.

Kontrollera att DNS-servrar för samlingen är tillgängliga och tillgänglig från undernätet som virtuella nätverk som du angav för den här samlingen.

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
För närvarande bara en Active Directory-domän kan associeras med Azure RemoteApp. Hybridsamlingen stöder enbart Azure Active Directory-konton som har synkroniserats med DirSync-verktyget från en Windows Server Active Directory-distribution mer specifikt antingen synkroniserats med alternativet för Lösenordssynkronisering eller synkroniseras med Active Directory Federation Services (AD FS) konfigurerats. Du måste skapa en anpassad domän som matchar UPN-suffix för domän för din lokala domän och konfigurera katalogintegrering.

Se [konfigurera Active Directory för Azure RemoteApp](remoteapp-ad.md) för mer information.

Kontrollera att domänen informationen är giltiga och domänkontrollanten kan nås från den virtuella datorn skapas i det undernät som används för Azure RemoteApp. Kontrollera också att angett autentiseringsuppgifterna för tjänstkontot har behörighet att lägga till datorer i den angivna domänen och att det angivna AD-namnet kan matchas från DNS i VNET.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Vilket domännamn som du angav när du skapade din samling?
Domännamnet du har skapat eller lagts till måste vara ett internt domännamn (inte dina Azure AD domain name) och måste vara i DNS-format som kan lösas (contoso.local). Till exempel du har ett internt namn för Active Directory (contoso.local) och en Active Directory-UPN (contoso.com) – du måste använda det interna namnet när du skapar din samling.

