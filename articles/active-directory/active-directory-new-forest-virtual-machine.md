---
title: "aaaInstall Active Directory-skog på Azure-nätverk | Microsoft Docs"
description: "En självstudiekurs som beskriver hur toocreate en ny Active Directory-skogen på en virtuell dator (VM) på ett Azure Virtual Network."
services: active-directory, virtual-network
keywords: 'Active directory virtuell dator, installera active directory-skog, azure active directory-videor '
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
tags: 
ms.assetid: eb7170d0-266a-4caa-adce-1855589d65d1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/06/2017
ms.author: joflore
ms.openlocfilehash: 08121130777cc3c206d7b5b38974982884dca1c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Installera en ny Active Directory-skog på Azure-nätverk
Det här avsnittet visar hur toocreate en ny Windows Server Active Directory-miljön på Azure-nätverk på en virtuell dator (VM) på en [virtuella Azure-nätverket](../virtual-network/virtual-networks-overview.md). I det här fallet är hello virtuella Azure-nätverket inte anslutna tooan lokalt nätverk.

Du kan också vara intresserad av dessa Närliggande ämnen:

* Se en video som visar de här stegen [hur tooinstall en ny Active Directory-skogen på Azure-nätverk](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* Du kan alternativt [konfigurera en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) och sedan installera en ny skog eller utöka en lokal skog tooan virtuella Azure-nätverket. De här stegen finns [installera en replikerad Active Directory-domänkontroller i ett Azure Virtual Network](active-directory-install-replica-active-directory-domain-controller.md).
* Konceptuell vägledning om hur du installerar Active Directory Domain Services (AD DS) på Azure-nätverk finns i [riktlinjer för att distribuera Windows Server Active Directory på Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenario-Diagram
I det här scenariot måste externa användare tooaccess program som körs på servrar som ingår i domänen. hello virtuella datorer som kör hello programservrar installeras hello virtuella datorer som kör domänkontrollanter och installeras i sin egen Molntjänsten i Azure-nätverk. De ingår också i en tillgänglighetsuppsättning för förbättrad feltolerans.

![Active Directory-skog på en virtuell dator på Azure Virtual Network ][1] 7

## <a name="how-does-this-differ-from-on-premises"></a>Hur skiljer det lokalt?
Det finns inte mycket skillnaden mellan att installera en domänkontrollant på Azure jämfört med lokalt. hello viktigaste skillnaderna visas i hello i den följande tabellen.

| tooconfigure... | Lokal | Azure-virtuellt nätverk |
| --- | --- | --- |
| **IP-adress för hello-domänkontrollant** |Tilldela statiska IP-adress på hello egenskaper för nätverkskort |Kör hello Set-AzureStaticVNetIP cmdlet tooassign statisk IP-adress |
| **DNS-klienten matcharen** |Ange önskade och alternativa DNS-serveradress för hello egenskaper för nätverkskort för medlemmar i domänen |Ange DNS-serveradress för egenskaper för hello hello virtuella nätverk |
| **Active Directory-databaslagring** |Om du vill ändra hello standardlagringsplats från C:\ |Du behöver toochange standardlagringsplats från C:\ |

## <a name="create-an-azure-virtual-network"></a>Skapa ett Azure-nätverk
1. Logga in toohello klassiska Azure-portalen.
2. Skapa ett virtuellt nätverk. Klicka på **nätverk** > **skapa ett virtuellt nätverk**. Använd hello värden i hello följande Tabellguiden toocomplete hello.

   | På den här sidan i guiden... | Ange dessa värden |
   | --- | --- |
   |  **Information om virtuellt nätverk** |<p>Namn: Ange ett namn för det virtuella nätverket</p><p>Region: Välj hello närmaste region</p> |
   |  **DNS- och VPN** |<p>Inget DNS-server</p><p>Inte markera alternativet antingen VPN</p> |
   |  **Virtuellt nätverk adressutrymmen** |<p>Undernätnamnet: Ange ett namn för ditt undernät</p><p>Startar IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p> |

## <a name="create-vms-toorun-hello-domain-controller-and-dns-server-roles"></a>Skapa virtuella datorer toorun hello-domänkontrollant och DNS-server-roller
Upprepa hello följande steg toocreate VMs toohost hello DC roll efter behov. Du bör distribuera minst två virtuella domänkontrollanter tooprovide feltolerans och redundans. Om hello virtuella Azure-nätverket innehåller minst två domänkontrollanter som är konfigurerad på samma sätt (som är de båda global katalog kör DNS-servern, och inget innehar FSMO-rollen, och så vidare) placera hello virtuella datorer som kör de domänkontrollanter i en tillgänglighetsuppsättning för förbättrad feltolerans.

toocreate hello virtuella datorer med hjälp av Windows PowerShell i stället för hello-Gränssnittet finns [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. I klassiska hello-portalen klickar du på **ny** > **Compute** > **virtuella** > **från galleriet**. Använd följande värden toocomplete hello guiden hello. Acceptera hello standardvärdet för en inställning om inte ett annat värde förslag eller krävs.

   | På den här sidan i guiden... | Ange dessa värden |
   | --- | --- |
   |  **Välj en bild** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguration av virtuell dator** |<p>Namn på virtuell dator: Skriv ett enkelt namn (till exempel AzureDC1).</p><p>Nytt användarnamn: Hello typnamn för en användare. Den här användaren ska vara medlem i hello lokala gruppen Administratörer på hello VM. Du behöver det här namnet toosign i toohello VM för hello första gången. hello inbyggt konto med namnet administratör fungerar inte.</p><p>Nytt lösenord/bekräfta: Ange ett lösenord</p> |
   |  **Konfiguration av virtuell dator** |<p>Molntjänsten: Välj <b>skapa en ny molntjänst</b> hello första virtuella datorn och välj att samma namn när du skapar flera virtuella datorer som är värd för hello DC-rollen.</p><p>Molnet tjänsten DNS-namn: Ange ett globalt unikt namn</p><p>Region/Tillhörighetsgrupp/virtuellt nätverk: Ange hello virtuella nätverksnamnet (till exempel WestUSVNet).</p><p>Storage-konto: Välj <b>använda ett automatiskt genererat lagringskonto</b> hello första virtuella datorn och välj sedan att samma lagringskonto namnet när du skapar flera virtuella datorer som är värd för hello DC-rollen.</p><p>Tillgänglighetsuppsättningen: Välj <b>skapa en tillgänglighetsuppsättning</b>.</p><p>Namn på tillgänglighetsuppsättning: Skriv ett namn för hello tillgänglighetsuppsättning när du skapar hello första virtuella datorn och välj sedan att samma namn när du skapar flera virtuella datorer.</p> |
   |  **Konfiguration av virtuell dator** |<p>Välj <b>installera hello VM-agenten</b> och andra tillägg som du behöver.</p> |
2. Ansluta en disk tooeach VM som ska köra hello DC-serverrollen. hello ytterligare disken är nödvändiga toostore hello AD-databasen, loggar och SYSVOL. Ange en storlek för hello disken (till exempel 10 GB) och lämna hello **värden Cache inställningar** ställa in också**ingen**. Hello anvisningar finns [hur tooAttach en datadisk tooa Windows-dator](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. När du loggar in första gången toohello VM, öppna **Serverhanteraren** > **fil- och lagringstjänster** toocreate en volym på disken med NTFS.
4. Reservera en statisk IP-adress för virtuella datorer som körs hello DC-rollen. tooreserve en statisk IP-adress, hämta hello Microsoft Web Platform Installer och [installera Azure PowerShell](/powershell/azure/overview) och kör hello Set-AzureStaticVNetIP cmdlet. Exempel:

    `Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM`

Mer information om hur du anger en statisk IP-adress finns [konfigurera statiska interna IP-adress för en virtuell dator](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Installera Windows Server Active Directory
Använd hello samma rutin för[installera AD DS](https://technet.microsoft.com/library/jj574166.aspx) att du använder lokala (det vill säga du kan använda hello UI, en svarsfil eller Windows PowerShell). Du måste tooprovide administratör autentiseringsuppgifter tooinstall en ny skog. toospecify hello plats för hello Active Directory-databasen, loggar och SYSVOL, ändra hello standardlagringsplats från hello enhet toohello ytterligare data operativsystemdisk bifogade toohello VM.

När hello DC-installationen har slutförts ansluta toohello VM igen och logga in toohello DC. Kom ihåg toospecify domänautentiseringsuppgifter.

## <a name="reset-hello-dns-server-for-hello-azure-virtual-network"></a>Återställ hello DNS-servern för hello Azure-nätverk
1. Återställ inställningen för hello DNS-vidarebefordrare på hello nya DC/DNS-servern.
   1. I Serverhanteraren klickar du på **verktyg** > **DNS**.
   2. I **DNS-hanteraren**, högerklickar du på hello DNS-server hello namnet och klickar på **egenskaper**.
   3. På hello **vidarebefordrare** fliken hello IP-adressen för hello vidarebefordrare och klicka på **redigera**.  Välj hello IP-adress och klicka på **ta bort**.
   4. Klicka på **OK** tooclose hello redigeraren och **Ok** igen tooclose hello DNS-serveregenskaper.
2. Uppdatera hello DNS-Serverinställningen för hello virtuellt nätverk.
   1. Klicka på **virtuella nätverk** > Dubbelklicka på hello virtuella nätverk som du skapade > **konfigurera** > **DNS-servrar**hello namn och Skriv hello DIP på en av hello virtuella datorer som kör hello DC/DNS-serverrollen och klickar på **spara**.
   2. Välj hello VM och klicka på **starta om** tootrigger hello VM tooconfigure DNS-matchare inställningar med hello hello nya DNS-serverns IP-adress.

## <a name="create-vms-for-domain-members"></a>Skapa virtuella datorer för medlemmar i domänen
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

## <a name="see-also"></a>Se även
* [Hur tooinstall en ny Active Directory-skogen på Azure-nätverk](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* [Riktlinjer för att distribuera Windows Server Active Directory på Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Konfigurera en plats-till-plats-VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Installera en replikerad Active Directory-domänkontroller i Azure-nätverk](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IT Pro IaaS: (01) virtuella grunderna](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) att skapa virtuella nätverk och anslutning](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md)
* [Hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet-referens](/powershell/azure/get-started-azureps)
* [Ange Azure VM statisk IP-adress](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
* [Hur tooassign statiska IP-tooAzure VM](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
* [Installera en ny Active Directory-skog](https://technet.microsoft.com/library/jj574166.aspx)
* [Introduktion tooActive Directory Domain Services (AD DS)-virtualisering (nivå 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
