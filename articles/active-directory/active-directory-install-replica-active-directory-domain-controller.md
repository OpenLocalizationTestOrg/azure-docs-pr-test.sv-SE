---
title: "aaaInstall repliken Active Directory-domänkontrollant i Azure | Microsoft Docs"
description: "En självstudiekurs som beskriver hur tooinstall en domänkontrollant från en lokal Active Directory-skogen på en virtuell Azure-dator."
services: virtual-network
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: virtual-network
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 74876fce2ca2a29f02c2828f9b3a21227248233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Installera en replikerad Active Directory-domänkontroller i Azure-nätverk
Det här avsnittet visar hur tooinstall ytterligare domänkontrollanter (även kallat replik domänkontrollanter) för en lokal Active Directory-domän på virtuella Azure-datorer (VM) i Azure-nätverk.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.

Du kan också vara intresserad av dessa Närliggande ämnen:

* Alternativt kan du installera en ny Active Directory-skog på Azure-nätverk. De här stegen finns [installera en ny Active Directory-skog på Azure-nätverk](active-directory-new-forest-virtual-machine.md).
* Konceptuell vägledning om hur du installerar Active Directory Domain Services (AD DS) på Azure-nätverk finns i [riktlinjer för att distribuera Windows Server Active Directory på Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenario-diagram
I det här scenariot måste externa användare tooaccess program som körs på servrar som ingår i domänen. hello virtuella datorer som kör hello-programservrar och hello replik domänkontrollanter är installerade i Azure-nätverk. hello virtuellt nätverk kan vara anslutna toohello lokalt nätverk genom ett [plats-till-plats VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) anslutning, enligt följande hello diagram du kan använda [ExpressRoute](../expressroute/expressroute-locations-providers.md) för snabbare anslutningar.

hello-programservrar och hello domänkontrollanter har distribuerats i separata molntjänster toodistribute compute bearbetning och inom [tillgänglighetsuppsättningar](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för förbättrad feltolerans.
hello domänkontrollanter replikera med varandra och med lokala domänkontrollanter med hjälp av Active Directory-replikering. Inga synkroniseringsverktyg behövs.

![Diagram över pf replik Active Directory-domänkontrollant ett Azure-vnet][1]

## <a name="create-an-active-directory-site-for-hello-azure-virtual-network"></a>Skapa en Active Directory-plats för hello Azure-nätverk
Det är en bra idé toocreate en plats i Active Directory som representerar hello nätverk region motsvarande toohello virtuella nätverk. Som hjälper till att optimera autentisering, replikering och andra åtgärder för DC-plats. hello följande steg förklarar hur toocreate en plats, och mer bakgrund, se [att lägga till en ny plats](https://technet.microsoft.com/library/cc781496.aspx).

1. Öppna Active Directory-platser och tjänster: **Serverhanteraren** > **verktyg** > **Active Directory-platser och tjänster**.
2. Skapa en plats toorepresent hello-region där du skapade ett virtuellt Azure-nätverk: Klicka på **platser** > **åtgärd** > **ny plats** > typ hello namnet på hello ny plats, till exempel Azure oss Väst > Välj en platslänk > **OK**.
3. Skapa ett undernät och associera med hello ny plats: Dubbelklicka på **platser** > högerklickar du på **undernät** > **Nytt undernät** > Skriv hello IP-adressintervall hello virtuella nätverk (till exempel 10.1.0.0/16 i hello scenariot diagram) > Välj hello ny Azure-plats > **OK**.

## <a name="create-an-azure-virtual-network"></a>Skapa ett Azure-nätverk
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **ny** > **nätverkstjänster** > **virtuellt nätverk**  >  **Skapa anpassade** och Använd hello följande värden toocomplete hello guiden.

   | På den här sidan i guiden... | Ange dessa värden |
   | --- | --- |
   |  **Information om virtuellt nätverk** |<p>Namn: Ange ett namn för hello virtuella nätverk, till exempel WestUSVNet.</p><p>Region: Välj hello närmaste region.</p> |
   |  **DNS- och VPN-anslutning** |<p>DNS-servrar: Ange hello namn och IP-adressen för en eller flera lokala DNS-servrar.</p><p>Anslutningsbarhet: Välj **konfigurera en plats-till-plats-VPN**.</p><p>Lokala nätverk: Ange ett nytt lokala nätverk.</p><p>Om du använder ExpressRoute i stället för en VPN-anslutning, se [konfigurera en ExpressRoute-anslutning via en Exchange-Provider](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Plats-till-plats-anslutning** |<p>Namn: Ange ett namn för hello lokalt nätverk.</p><p>VPN-enhetens IP-adress: Ange hello offentliga IP-adressen för hello-enhet som ansluter toohello virtuellt nätverk. hello VPN-enhet kan inte finnas bakom en NAT</p><p>Adress: Ange hello-adressintervall för ditt lokala nätverk (till exempel 192.168.0.0/16 i hello scenariot diagram).</p> |
   |  **Virtuellt nätverk adressutrymmen** |<p>Adressutrymmet: Ange hello IP-adressintervall för virtuella datorer som du vill toorun i hello Azure-nätverk (till exempel 10.1.0.0/16 i hello scenariot diagram). Detta adressintervall kan inte överlappa hello adressintervall hello lokalt nätverk.</p><p>Undernät: Ange ett namn och adress för ett undernät för hello programservrar (till exempel Frontend 10.1.1.0/24) och för hello domänkontrollanter (till exempel serverdel 10.1.2.0/24).</p><p>Klicka på **Lägg till gateway-undernätet**.</p> |
2. Därefter konfigurerar du hello virtuellt gateway toocreate en säker plats-till-plats VPN-anslutning. Se [konfigurera en virtuell nätverksgateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) hello-instruktioner.
3. Skapa hello plats-till-plats VPN-anslutning mellan hello nytt virtuellt nätverk och en lokal VPN-enhet. Se [konfigurera en virtuell nätverksgateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) hello-instruktioner.

## <a name="create-azure-vms-for-hello-dc-roles"></a>Skapa virtuella datorer i Azure för hello DC-roller
Upprepa hello följande steg toocreate VMs toohost hello DC roll efter behov. Du bör distribuera minst två virtuella domänkontrollanter tooprovide feltolerans och redundans. Om hello virtuella Azure-nätverket innehåller minst två domänkontrollanter som är konfigurerad på samma sätt (som är de båda global katalog kör DNS-servern, och inget innehar FSMO-rollen, och så vidare) placera hello virtuella datorer som kör de domänkontrollanter i en tillgänglighetsuppsättning för förbättrad feltolerans.
toocreate hello virtuella datorer med hjälp av Windows PowerShell i stället för hello-Gränssnittet finns [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **ny** > **Compute** > **virtuella**  >  **Från galleriet**. Använd följande värden toocomplete hello guiden hello. Acceptera hello standardvärdet för en inställning om inte ett annat värde förslag eller krävs.

   | På den här sidan i guiden... | Ange dessa värden |
   | --- | --- |
   |  **Välj en bild** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguration av virtuell dator** |<p>Namn på virtuell dator: Skriv ett enkelt namn (till exempel AzureDC1).</p><p>Nytt användarnamn: Hello typnamn för en användare. Den här användaren ska vara medlem i hello lokala gruppen Administratörer på hello VM. Du behöver det här namnet toosign i toohello VM för hello första gången. hello inbyggt konto med namnet administratör fungerar inte.</p><p>Nytt lösenord/bekräfta: Ange ett lösenord</p> |
   |  **Konfiguration av virtuell dator** |<p>Molntjänsten: Välj <b>skapa en ny molntjänst</b> hello första virtuella datorn och välj att samma namn när du skapar flera virtuella datorer som är värd för hello DC-rollen.</p><p>Molnet tjänsten DNS-namn: Ange ett globalt unikt namn</p><p>Region/Tillhörighetsgrupp/virtuellt nätverk: Ange hello virtuella nätverksnamnet (till exempel WestUSVNet).</p><p>Storage-konto: Välj <b>använda ett automatiskt genererat lagringskonto</b> hello första virtuella datorn och välj sedan att samma lagringskonto namnet när du skapar flera virtuella datorer som är värd för hello DC-rollen.</p><p>Tillgänglighetsuppsättningen: Välj <b>skapa en tillgänglighetsuppsättning</b>.</p><p>Namn på tillgänglighetsuppsättning: Skriv ett namn för hello tillgänglighetsuppsättning när du skapar hello första virtuella datorn och välj sedan att samma namn när du skapar flera virtuella datorer.</p> |
   |  **Konfiguration av virtuell dator** |<p>Välj <b>installera hello VM-agenten</b> och andra tillägg som du behöver.</p> |
2. Ansluta en disk tooeach VM som ska köra hello DC-serverrollen. hello ytterligare disken är nödvändiga toostore hello AD-databasen, loggar och SYSVOL. Ange en storlek för hello disken (till exempel 10 GB) och lämna hello **värden Cache inställningar** ställa in också**ingen**. Hello anvisningar finns [hur tooAttach en datadisk tooa Windows-dator](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. När du loggar in första gången toohello VM, öppna **Serverhanteraren** > **fil- och lagringstjänster** toocreate en volym på disken med NTFS.
4. Reservera en statisk IP-adress för virtuella datorer som körs hello DC-rollen. tooreserve en statisk IP-adress, hämta hello Microsoft Web Platform Installer och [installera Azure PowerShell](/powershell/azure/overview) och kör hello Set-AzureStaticVNetIP cmdlet. Exempel:

    ' Get-AzureVM - ServiceName AzureDC1-Name AzureDC1 | Ange AzureStaticVNetIP - IP-adress 10.0.0.4 | Uppdatera AzureVM

Mer information om hur du anger en statisk IP-adress finns [konfigurera statiska interna IP-adress för en virtuell dator](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installera AD DS på virtuella Azure-datorer
Logga in tooa VM och kontrollera att du har anslutning över hello plats-till-plats VPN eller ExpressRoute-anslutning tooresources i ditt lokala nätverk. Installera sedan AD DS på hello Azure virtuella datorer. Du kan använda samma process som du använder tooinstall en extra Domänkontrollant i ditt lokala nätverk (UI, Windows PowerShell eller en svarsfil). När du installerar AD DS, kontrollera att du anger hello ny volym för hello platsen för hello AD-databasen, loggfilerna och SYSVOL. Om du behöver en uppdaterare på AD DS-installationen finns [installera Active Directory Domain Services (nivå 100)](https://technet.microsoft.com/library/hh472162.aspx) eller [installera en replikerad Windows Server 2012-domänkontroller i en befintlig domän (nivå 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-hello-virtual-network"></a>Konfigurera om DNS-servern för hello virtuellt nätverk
1. I hello [Azure-portalen](https://portal.azure.com), i hello **söka resurser** ange *virtuella nätverken*, klicka på **virtuella nätverk (klassiskt)** i hello sökresultat. Hello namnet på hello virtuellt nätverk, och sedan [omkonfigurera hello DNS-serveradresser för det virtuella nätverket](../virtual-network/virtual-network-manage-network.md#dns-servers) toouse hello statiska IP-adresser tilldelas toohello replik domänkontrollanter i stället för hello IP-adresser för en lokal DNS -servrar.
2. tooensure att alla hello replikeringen DC virtuella datorer på hello virtuellt nätverk har konfigurerats med toouse DNS-servrarna på hello virtuellt nätverk, klickar du på **virtuella datorer**, klickar du på hello i statuskolumnen för varje virtuell dator och klicka sedan på **starta om** . Vänta tills hello VM visar **kör** tillstånd innan du försöker toosign till den.

## <a name="create-vms-for-application-servers"></a>Skapa virtuella datorer för programservrar
1. Upprepa hello följande steg toocreate VMs toorun som programservrar. Acceptera hello standardvärdet för en inställning om inte ett annat värde förslag eller krävs.

   | På den här sidan i guiden... | Ange dessa värden |
   | --- | --- |
   |  **Välj en bild** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguration av virtuell dator** |<p>Namn på virtuell dator: Skriv ett enkelt namn (till exempel AppServer1).</p><p>Nytt användarnamn: Hello typnamn för en användare. Den här användaren ska vara medlem i hello lokala gruppen Administratörer på hello VM. Du behöver det här namnet toosign i toohello VM för hello första gången. hello inbyggt konto med namnet administratör fungerar inte.</p><p>Nytt lösenord/bekräfta: Ange ett lösenord</p> |
   |  **Konfiguration av virtuell dator** |<p>Molntjänsten: Välj **skapa en ny molntjänst** för hello första virtuella datorn och välj att samma molntjänster tjänstens namn när du skapar flera virtuella datorer som är värd för programmet hello.</p><p>Molnet tjänsten DNS-namn: Ange ett globalt unikt namn</p><p>Region/Tillhörighetsgrupp/virtuellt nätverk: Ange hello virtuella nätverksnamnet (till exempel WestUSVNet).</p><p>Lagringskonto: Välj **använda ett automatiskt genererat lagringskonto** för hello första virtuella datorn och välj sedan att samma lagringskonto namnet när du skapar flera virtuella datorer som är värd för programmet hello.</p><p>Tillgänglighetsuppsättningen: Välj **skapa en tillgänglighetsuppsättning**.</p><p>Namn på tillgänglighetsuppsättning: Skriv ett namn för hello tillgänglighetsuppsättning när du skapar hello första virtuella datorn och välj sedan att samma namn när du skapar flera virtuella datorer.</p> |
   |  **Konfiguration av virtuell dator** |<p>Välj <b>installera hello VM-agenten</b> och andra tillägg som du behöver.</p> |
2. Efter varje virtuell dator har etablerats kan du logga in och ansluta den toohello domän. I **Serverhanteraren**, klickar du på **lokal Server** > **arbetsgrupp** > **ändra...** och välj sedan **domän** och ange hello namnet på den lokala domänen. Ange autentiseringsuppgifter för en domänanvändare och starta sedan om hello VM toocomplete hello domänanslutning.

toocreate hello virtuella datorer med hjälp av Windows PowerShell i stället för hello-Gränssnittet finns [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Mer information om hur du använder Windows PowerShell finns [Kom igång med Azure-Cmdlets](/powershell/azure/overview) och [Cmdlet-referens för Azure](/powershell/azure/get-started-azureps).

## <a name="additional-resources"></a>Ytterligare resurser
* [Riktlinjer för att distribuera Windows Server Active Directory på Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Hur tooupload befintlig lokal Hyper-V domän domänkontrollanter tooAzure med hjälp av Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Installera en ny Active Directory-skog på Azure-nätverk](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IT Pro IaaS: (01) virtuella grunderna](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) att skapa virtuella nätverk och anslutning](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure cmdlet: ar](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
