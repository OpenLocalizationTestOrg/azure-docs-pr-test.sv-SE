---
title: "aaaDeploy mot Internet-belastningsutjämnaren med IPv6 - Azure-mall | Microsoft Docs"
description: "Hur stöder toodeploy IPv6 Azure belastningsutjämnare och belastningsutjämnade virtuella datorer."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: 2998e943-13fc-4ea9-a68c-875e53a08db3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2016
ms.author: kumud
ms.openlocfilehash: 68b9ba874a50161243577b64c4a6d9c76b39156c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Distribuera en Internetriktade belastningsutjämnare-lösning med IPv6 med en mall

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Mall](load-balancer-ipv6-internet-template.md)

En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP). hello belastningsutjämnare ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri tjänstinstanser i molntjänster eller virtuella datorer i en belastningen belastningsutjämnaren. Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.

## <a name="example-deployment-scenario"></a>Exempelscenario för distribution

hello följande diagram illustrerar hello nätverkslösning för belastningsutjämning som distribueras med hjälp av hello exempelmall som beskrivs i den här artikeln.

![Belastningsutjämningsscenario](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

I det här scenariot skapar du hello följande Azure-resurser:

* ett virtuellt nätverksgränssnitt för varje virtuell dator med både IPv4 och IPv6-adresser som tilldelats
* en Internetriktade belastningsutjämnare med en IPv4 och IPv6-offentlig IP-adress
* två läsa in belastningsutjämning regler toomap hello offentliga VIP toohello privata slutpunkter
* en Tillgänglighetsuppsättning som innehåller hello två virtuella datorer
* två virtuella datorer (VM)

## <a name="deploying-hello-template-using-hello-azure-portal"></a>Distribuera hello mallen med hjälp av hello Azure-portalen

Den här artikeln refererar till en mall som har publicerats i hello [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) gallery. Du kan hämta hello mallen från galleriet hello eller starta hello distribution i Azure direkt från hello-galleriet. Den här artikeln förutsätter att du har hämtat hello mallen tooyour lokal dator.

1. Öppna hello Azure-portalen och logga in med ett konto som har behörigheter toocreate virtuella datorer och nätverksresurser i en Azure-prenumeration. Även om du inte använder befintliga resurser hello kontot måste behörighet toocreate en resursgrupp och ett lagringskonto.
2. Klicka på ”+ ny” från hello-menyn och ange ”mall” i sökrutan för hello sedan. Välj ”malldistribution” hello sökresultat.

    ![lb-ipv6-portalen – steg 2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. Hej allt i bladet, klickar du på ”malldistribution”.

    ![lb-ipv6-portalen – steg 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klicka på ”Skapa”.

    ![lb-ipv6-portal-step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klicka på ”Redigera mallen”. Hello befintliga innehållet tas bort och kopiera och klistra in i hello hela innehållet i hello mallfilen (tooinclude hello start och sluta {}), klicka på ”Spara”.

    > [!NOTE]
    > Om du använder Microsoft Internet Explorer när du klistrar du in visas en dialogruta där du tooallow åtkomst toohello Urklipp. Klicka på ”Tillåt åtkomst”.

    ![lb-ipv6-portal-step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klicka på ”Redigera parametrar”. Ange hello-värden per hello vägledning i hello mallsektion parametrar i hello-bladet parametrar och sedan på ”Spara” tooclose hello parametrar bladet. Välj din prenumeration, en befintlig resursgrupp eller skapa en hello anpassad distribution-bladet. Om du skapar en resursgrupp, väljer du en plats för hello resursgruppen. Klicka sedan på **juridiska villkor**, klicka på **inköp** för hello juridiska villkor. Azure börjar distribuera hello resurser. Det tar flera minuter toodeploy alla hello-resurser.

    ![lb-ipv6-portal-step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Mer information om dessa parametrar finns hello [mallparametrar och variabler](#template-parameters-and-variables) senare i den här artikeln.

7. toosee hello resurser som skapats av hello mallen klickar du på Bläddra, bläddrar du nedåt hello listan tills du se ”resursgrupper” och sedan klicka på den.

    ![lb-ipv6-portal-step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Hello resursbladet till grupper, klicka på hello namnet på hello resursgrupp som du angav i steg 6. Du kan se en lista över alla hello-resurser som har distribuerats. Om alla gått bra ska det stå ”Succeeded” under ”senaste distribution”. Om inte, kontrollera att hello kontot du använder har behörighet toocreate hello nödvändiga resurser.

    ![lb-ipv6-portal-step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [!NOTE]
    > Om du bläddrar resursgrupperna omedelbart när du har slutfört steg 6 visas hello status för ”distribution” för ”senaste distribution” medan hello resurser distribueras.

9. Klicka på ”myIPv6PublicIP” i hello lista över resurser. Du kan se att det finns en IPv6-adress under IP-adress och att DNS-namnet är hello-värde som du angav för hello dnsNameforIPv6LbIP parameter i steg 6. Den här resursen är hello offentliga IPv6-adress och värddatornamn namn som är tillgänglig tooInternet-klienter.

    ![lb-ipv6-portal-step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Kontrollera anslutning

När hello mallen har distribuerats verifiera du anslutningsbarhet genom att slutföra hello följande uppgifter:

1. Logga in toohello Azure-portalen och ansluta tooeach av hello virtuella datorer som skapats av hello malldistribution. Om du har distribuerat en Windows Server-VM köra ipconfig/alla från en kommandotolk. Ser du att hello virtuella datorer både IPv4 och IPv6-adresser. Om du har distribuerat virtuella Linux-datorer måste tooconfigure hello Linux OS tooreceive dynamisk IPv6-adresser med hjälp av hello anvisningarna för Linux-distribution.
2. Initiera en anslutning toohello offentliga IPv6-adressen för belastningsutjämnaren hello från en klient för IPv6-Internet-anslutna. tooconfirm som hello belastningsutjämnaren belastningsutjämning mellan hello två virtuella datorer, kan du installera en webbserver som Microsoft Internet Information Services (IIS) på varje hello virtuella datorer. hello standardwebbsidan på varje server kan innehålla hello text ”Server0” eller ”Server1” toouniquely identifieras. Öppna en webbläsare på en IPv6-Internet-ansluten klient och bläddra toohello värdnamn som du angav för hello dnsNameforIPv6LbIP parameter av hello belastningen belastningsutjämnaren tooconfirm slutpunkt till slutpunkt IPv6-anslutning tooeach VM. Om du bara ser hello webbsida från en enda server kanske du måste tooclear webbläsarens cacheminne. Öppna flera privata webbläsarsessioner. Du bör se svar från varje server.
3. Initiera anslutning toohello offentlig IPv4-adress för hello belastningsutjämnare i en IPv4-Internet-ansluten klient. tooconfirm som hello belastningsutjämnare är belastningsutjämning hello två virtuella datorer, kan du testa med hjälp av IIS, enligt anvisningarna i steg 2.
4. Starta en utgående anslutning tooan IPv6 eller en IPv4-anslutna Internet-enhet från varje virtuell dator. I båda fallen kan är ses av hello målenheten hello käll-IP hello offentliga IPv4- eller IPv6-adressen för belastningsutjämnaren hello.

> [!NOTE]
> ICMP för både IPv4 och IPv6 är blockerad i hello Azure-nätverk. Därför ICMP-verktyg som pinga alltid att misslyckas. tootest anslutning, använda en TCP-alternativ, till exempel TCPing eller hello PowerShell Test-NetConnection cmdlet. Observera att hello IP-adresser som visas i diagram hello är exempel på värden som kan visas. Eftersom hello IPv6-adresser tilldelas dynamiskt hello-adresser som visas varierar och kan variera beroende på region. Dessutom är det vanligt för hello offentliga IPv6-adress på hello load balancer toostart med ett annat prefix än hello privata IPv6-adresser i hello backend-adresspool.

## <a name="template-parameters-and-variables"></a>Mallparametrar och variabler

En Azure Resource Manager-mall innehåller flera variabler och parametrar som du kan anpassa tooyour behov. Variabler används för fasta värden som du inte vill att en användare toochange. Parametrar som används för värden som du vill tooprovide en användare när du distribuerar hello mallen. hello exempel mallen har konfigurerats för hello-scenariot som beskrivs i den här artikeln. Du kan anpassa den här tooneeds i din miljö.

hello exempel mall som används i den här artikeln innehåller hello följande parametrar och variabler:

| Parametern / Variable | Anteckningar |
| --- | --- |
| adminUsername |Ange hello namnet på hello administratörskonto används toosign i toohello virtuella datorer med. |
| adminPassword |Ange hello lösenordet för administratörskontot för hello används toosign i toohello virtuella datorer med. |
| dnsNameforIPv4LbIP |Ange hello DNS-värdnamn som du vill använda tooassign som hello offentliga namn hello belastningsutjämnaren. Det här namnet matchar toohello belastningsutjämnaren offentlig IPv4-adress. hello-namnet måste vara gemener och matchar hello regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP |Ange hello DNS-värdnamn som du vill använda tooassign som hello offentliga namn hello belastningsutjämnaren. Det här namnet matchar toohello belastningsutjämnarens offentliga IPv6-adress. hello-namnet måste vara gemener och matchar hello regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. Detta kan vara hello samma namn som hello IPv4-adress. När en klient skickar returnerar en DNS-fråga för Azure både hello A och AAAA-poster när hello namnet delas. |
| vmNamePrefix |Ange hello VM prefixet. hello mallen till en siffra (0, 1, etc.) toohello namn när hello virtuella datorer skapas. |
| nicNamePrefix |Ange hello nätverksprefixet gränssnittet namn. hello mallen till en siffra (0, 1, etc.) toohello namn när hello nätverksgränssnitt skapas. |
| storageAccountName |Ange hello namnet på ett befintligt lagringskonto eller hello namnet på en ny en toobe som skapats av hello mallen. |
| availabilitySetName |Ange namnet på hello tillgänglighet ställa in toobe som används med hello virtuella datorer |
| addressPrefix |hello-adressprefix används toodefine hello adressintervall hello virtuellt nätverk |
| subnetName |hello namnet på hello undernät i skapas för hello VNet |
| subnetPrefix |hello-adressprefix används toodefine hello adressintervall hello undernät |
| vnetName |Ange hello namn för hello VNet som används av hello virtuella datorer. |
| ipv4PrivateIPAddressType |hello allokeringsmetod som används för hello privata IP-adress (statisk eller dynamisk) |
| ipv6PrivateIPAddressType |hello allokeringsmetod som används för hello privata IP-adress (dynamisk). IPv6 har endast stöd för dynamisk allokering. |
| numberOfInstances |hello antalet belastningen belastningsutjämnade instanser som distribuerats av hello mall |
| ipv4PublicIPAddressName |Ange hello DNS-namn som du vill toouse toocommunicate med hello offentliga IPv4-adress för hello belastningsutjämnare. |
| ipv4PublicIPAddressType |hello allokeringsmetod för hello offentlig IP-adress (statisk eller dynamisk) |
| Ipv6PublicIPAddressName |Ange hello DNS-namn som du vill toouse toocommunicate med hello offentliga IPv6-adressen för hello belastningsutjämnaren. |
| ipv6PublicIPAddressType |hello allokeringsmetod för hello offentlig IP-adress (dynamisk). IPv6 har endast stöd för dynamisk allokering. |
| lbName |Ange hello namn hello belastningsutjämnaren. Det här namnet visas på portalen hello eller används för att referera tooit med CLI eller PowerShell-kommando. |

hello återstående variabler i hello mallen innehåller härledda värden som tilldelas när Azure skapar hello resurser. Ändra inte dessa variabler.
