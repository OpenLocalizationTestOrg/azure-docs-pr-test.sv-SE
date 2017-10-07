---
title: "aaaProvision en virtuell dator för SQL Server | Microsoft Docs"
description: "Skapa och ansluta tooa SQL Server-dator i Azure med hjälp av hello Portal. Den här kursen använder hello Resource Manager-läget."
services: virtual-machines-windows
documentationcenter: na
author: rothja
editor: 
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: jroth
experimental_id: a641df96-f27d-40
ms.openlocfilehash: aaad422d6ed47f5ca00b1ef484ac270a58e24f99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-in-hello-azure-portal"></a>Etablera en virtuell dator med SQL Server i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

Den här slutpunkt till slutpunkt-kursen visar hur hello toouse Azure Portal tooprovision en virtuell dator som kör SQL Server.

galleriet för hello Azure virtuella datorer (VM) innehåller flera avbildningar som innehåller Microsoft SQL Server. Med några få klick kan du väljer du något av hello SQL VM bilder från hello-galleriet och etablera den i Azure-miljön.

I den här kursen ska du:

* [Välj en SQL-VM-avbildning från hello-galleriet](#select-a-sql-vm-image-from-the-gallery)
* [Konfigurera och skapa hello VM](#configure-the-vm)
* [Öppna hello virtuella datorn med fjärrskrivbord](#open-the-vm-with-remote-desktop)
* [Fjärransluta tooSQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-hello-gallery"></a>Välj en SQL-VM-avbildning från hello-galleriet
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ditt konto.

   > [!NOTE]
   > Om du inte har något Azure-konto besöker du sidan för [kostnadsfria utvärderingsversioner av Azure](https://azure.microsoft.com/pricing/free-trial/).

2. Klicka på hello Azure-portalen, **ny**. hello portalen öppnar hello **ny** bladet. hello SQL Server-VM resurser finns i hello **Compute** för hello Marketplace.
3. I hello **ny** bladet, klickar du på **Compute** och klicka sedan på **se alla**.
4. I hello **Filter** text skriver SQL Server och trycker på Enter hello.

   ![Blad för Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

5. Granska hello tillgängliga SQL Server-avbildningar. Varje avbildning identifierar en version av SQL Server och ett operativsystem. 
6. Välj hello avbildning för SQL Server 2016 SP1 utvecklare i Windows Server 2016.

   > [!TIP]
   > hello Developer edition används i den här självstudiekursen eftersom det är en komplett utgåva av SQL Server som är gratis för utveckling, testning. Du betalar bara för hello kostnaden för att köra hello VM.

   > [!NOTE]
   > SQL-VM-avbildningar inkludera hello licensieringskostnaderna för SQL Server i hello per minut prissättningen av hello VM som du skapar (förutom hello utvecklare och Express-versioner). SQL Server Developer är kostnadsfri när den används till utveckling och testning (inte i samband med produktion) och SQL Express är kostnadsfri för enklare arbetsbelastningar (mindre än 1 GB minne och mindre än 10 GB lagringsutrymme).
   > Det finns ett annat alternativ toobring-your-äger-licens (BYOL) och betala endast för hello VM. Dessa avbildningsnamn föregås av {BYOL}. Mer information om alternativen finns i [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prisvägledning för virtuella SQL Server Azure-datorer).

7. Under **Välj en distributionsmodell** kontrollerar du att **Resource Manager** är valt. Resource Manager är hello rekommenderas distributionsmodell för nya virtuella datorer. Klicka på **Skapa**.

    ![Skapa den virtuella SQL-datorn med Resource Manager](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-hello-vm"></a>Konfigurera hello VM
Det finns fem tillgängliga blad för att konfigurera en virtuell dator med SQL Server.

| Steg | Beskrivning |
| --- | --- |
| **Grundläggande inställningar** |[Konfigurera grundläggande inställningar](#1-configure-basic-settings) |
| **Storlek** |[Välj den virtuella datorns storlek](#2-choose-virtual-machine-size) |
| **Inställningar** |[Konfigurera valfria funktioner](#3-configure-optional-features) |
| **SQL Server-inställningar** |[Konfigurera SQL Server-inställningar](#4-configure-sql-server-settings) |
| **Sammanfattning** |[Granska hello sammanfattning](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Konfigurera grundläggande inställningar
På hello **grunderna** bladet ange hello följande information:

* Ange ett unikt **namn** för den virtuella datorn.
* Ange en **användarnamn** för hello lokala administratörskontot på hello VM. Det här kontot läggs också till toohello SQL Server **sysadmin** fasta serverrollen.
* Ange ett starkt **lösenord**.
* Om du har flera prenumerationer, kontrollera att hello prenumerationen är korrekt för hello ny virtuell dator.
* I hello **resursgruppen** Skriv ett namn för en ny resursgrupp. Du kan också toouse en befintlig resursgrupp klickar du på **använda befintliga**. En resursgrupp är en samling relaterade resurser i Azure (virtuella datorer, lagringskonton, virtuella nätverk osv.).
  
  > [!NOTE]
  > En ny resursgrupp är praktiskt om du bara testar eller lär dig om SQL Server-distributioner i Azure. När du är klar med testet tar du bort hello resurs grupp tooautomatically delete hello VM och alla resurser som associeras med resursgruppen. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md).
  > 
  > 
* Välj en **plats** för den här distributionen.
* Klicka på **OK** toosave hello inställningar.
  
    ![Grundläggande SQL-inställningar](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Välj den virtuella datorns storlek
På hello **storlek** steg, välja en storlek på virtuell dator i hello **välja en storlek** bladet. hello bladet visar rekommenderade datorstorlekar baserat på valda hello bilden.

> [!IMPORTANT]
> hello uppskattad månadskostnaden visas på hello **välja en storlek** bladet innehåller inte SQL Server-licensieringskostnaderna. Den månatliga är uppskattade kostnaden är hello kostnaden för hello VM enbart. Detta är hello totala uppskattade kostnaden för hello Express och Developer-utgåvorna av SQL Server. Andra utgåvor finns hello [sida med priser virtuella Windows-datorer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) och välj mål-utgåva av SQL Server. Se även hello [priser för SQL Server Azure Virtual Machines](virtual-machines-windows-sql-server-pricing-guidance.md).

![Storleksalternativ för en virtuell dator med SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

För produktionsarbetsbelastningar rekommenderar vi att du väljer en storlek på den virtuella datorn som har stöd för [Premium Storage](../../../storage/storage-premium-storage.md). Om du inte behöver den nivån av prestanda, använder hello **visa alla** knappen som visar alla storleksalternativ för datorer. Du kan till exempel använda en mindre datorstorlek för en utvecklings- eller testmiljö.

> [!NOTE]
> Mer information om storlekar för virtuella datorer finns i [Storlekar för virtuella datorer](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Saker att tänka på när du väljer storlek för virtuella datorer med SQL Server finns i [Prestandarelaterade metodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Välj din datorstorlek och klicka på **Välj**.

## <a name="3-configure-optional-features"></a>3. Konfigurera valfria funktioner
På hello **inställningar** bladet konfigurerar du lagring, nätverk och övervakning för hello virtuella datorn.

* Under **Storage** anger du en **disktyp**, antingen Standard eller Premium (SSD). Premium Storage rekommenderas för produktionsarbetsbelastningar.

> [!NOTE]
> Om du väljer Premium (SSD) för en datorstorlek som inte stöder Premium Storage ändras datorstorleken automatiskt.  
> 
> 

* Under **lagringskonto**, du kan acceptera hello automatiskt etablerade lagringskontonamnet. Du kan också klicka på **lagringskonto** toochoose ett befintligt konto och konfigurera hello lagringskontotypen. Som standard skapar Azure ett nytt lagringskonto med lokalt redundant lagring. Mer information om lagringsalternativ finns i [Azure Storage-replikering](../../../storage/storage-redundancy.md).
* Under **nätverk**, du kan acceptera hello fylls automatiskt värden. Du kan också klicka på varje funktion toomanually konfigurera hello **för virtuella nätverk**, **undernät**, **offentliga IP-adressen**, och **Nätverkssäkerhetsgruppen**. Behåll hello standardvärden för hello ändamål i den här kursen.
* Azure aktiverar **övervakning** som standard med hello samma lagringskonto som har utsetts för hello VM. Du kan ändra dessa inställningar här.
* Ange en tillgänglighetsuppsättning under **Tillgänglighetsuppsättning**. För hello syftet med den här självstudiekursen, kan du välja **ingen**. Om du planerar tooset in SQL AlwaysOn-Tillgänglighetsgrupper måste du konfigurera hello tillgänglighet tooavoid återskapa hello virtuella datorn.  Mer information finns i [hantera hello tillgängligheten för Virtual Machines](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

När du har konfigurerat dessa inställningar klickar du på **OK**.

## <a name="4-configure-sql-server-settings"></a>4. Konfigurera SQL Server-inställningar
På hello **SQL Server-inställningar** bladet konfigurera specifika inställningar och optimeringar för SQL Server. hello-inställningar som du kan konfigurera för SQL Server inkluderar hello följande inställningar.

| Inställning |
| --- |
| [Anslutning](#connectivity) |
| [Autentisering](#authentication) |
| [Storage-konfiguration](#storage-configuration) |
| [Automatisk uppdatering](#automated-patching) |
| [Automatisk säkerhetskopiering](#automated-backup) |
| [Azure Key Vault-integrering](#azure-key-vault-integration) |
| [R-tjänster](#r-services) |

### <a name="connectivity"></a>Anslutning
Under **SQL-anslutning**, ange hello typ av åtkomst du vill toohello SQL Server-instansen på den här virtuella datorn. Hello syftet med den här självstudiekursen, Välj **offentlig (internet)** tooallow anslutningar tooSQL Server från datorer eller tjänster på hello internet. Det här alternativet konfigurerar Azure automatiskt hello brandvägg och hello säkerhet grupp tooallow nätverkstrafik på port 1433.  

![SQL-anslutningsalternativ](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

tooconnect tooSQL Server via hello internet, även måste du aktivera SQL Server-autentisering som beskrivs i nästa avsnitt om hello.

> [!NOTE]
> Det är möjligt tooadd fler begränsningar för hello nätverket kommunikation tooyour SQL Server-VM. Du kan lägga till fler begränsningar genom att redigera hello Nätverkssäkerhetsgruppen när hello VM har skapats. Mer information finns i [Vad är en nätverkssäkerhetsgrupp?](../../../virtual-network/virtual-networks-nsg.md)
> 
> 

Om du föredrar toonot aktivera anslutningar toohello databasmotorn via Hej internet, väljer du något av följande alternativ för hello:

* **Lokala (inuti VM)** tooallow anslutningar tooSQL Server inom hello VM.
* **Privat (inom Virtual Network)** tooallow anslutningar tooSQL Server från datorer eller tjänster i hello samma virtuella nätverk.

> [!NOTE]
> hello avbildning av virtuell dator för SQL Server Express edition kan inte automatiskt hello TCP/IP-protokollet. Detta gäller även för hello anslutning för offentliga och privata alternativ. För Express-version måste du använda SQL Server Configuration Manager för[manuellt Aktivera hello TCP/IP-protokollet](#configure-sql-server-to-listen-on-the-tcp-protocol) hello VM när du har skapat.
> 
> 

I allmänhet kan förbättra säkerheten genom att välja hello mest restriktiva anslutningen som ditt scenario medger. Men alla hello alternativ kan skyddas med regler för Nätverkssäkerhetsgruppen och SQL/Windows-autentisering.

**Port** too1433 som standard. Du kan ange ett annat portnummer.
Mer information finns i [ansluta tooa virtuell dator med SQL Server (Resource Manager) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Autentisering
Om du kräver SQL Server-autentisering klickar du på **Aktivera** under **SQL-autentisering**.

![SQL Server-autentisering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Om du planerar tooaccess SQL Server över hello internet (det vill säga hello anslutning för offentliga alternativet kvar), måste du aktivera SQL-autentisering här. Allmän åtkomst toohello SQL Server kräver hello använder SQL-autentisering.
> 
> 

Om du aktiverar SQL Server-autentisering anger du ett **inloggningsnamn** och **lösenord**. Det här användarnamnet är konfigurerad som en SQL Server-autentisering inloggning och tillhör hello **sysadmin** fasta serverrollen. Mer information om autentiseringslägen finns i [Välja ett autentiseringsläge](http://msdn.microsoft.com/library/ms144284.aspx).

Om du inte aktiverar SQL Server-autentisering kan du använda hello lokala administratörskontot på hello VM tooconnect toohello SQL Server-instans.

### <a name="storage-configuration"></a>Storage-konfiguration
Klicka på **lagringskonfiguration** toospecify hello lagringsbehov.

![SQL Storage-konfiguration](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Det här alternativet är inte tillgängligt om du väljer standardlagring. Automatisk lagringsoptimering är endast tillgängligt för Premium Storage.
> 
> 

Du kan ange krav som I/O-åtgärder per sekund (IOPs), genomflöde i MB/s och totalt lagringsutrymme. Konfigurera dessa värden med hjälp av reglagen hello. hello portalen beräknar automatiskt hello antalet diskar baserat på dessa krav.

Som standard optimerar Azure hello lagringen för 5 000 IOPs, 200 MB och 1 TB lagringsutrymme. Du kan ändra dessa lagringsinställningar baserat på arbetsbelastningen. Under **Storage optimerat för**, väljer du något av följande alternativ för hello:

* **Allmänna** är standardinställningen för hello och stöder de flesta arbetsbelastningar.
* **Transaktionell** bearbetning optimerar hello lagringen för traditionella OLTP-arbetsbelastningar.
* **Datalagring** optimerar hello lagringen för analys- och rapporteringsarbetsbelastningar.

> [!NOTE]
> hello övre gräns på Hej reglage varierar beroende på storleken på din valda virtuella datorn.
> 
> 

### <a name="automated-patching"></a>Automatisk uppdatering
**Automatisk uppdatering** är aktiverat som standard. Inställningen automatisk uppdatering kan Azure tooautomatically korrigering av SQL Server och hello operativsystem. Ange en dag i veckan hello, tid och varaktighet för en underhållsperiod. Azure utför uppdateringar under den här underhållsperioden. hello underhållsperiodens schema använder hello virtuella datorns lokala tid. Om du inte vill att Azure tooautomatically korrigering av SQL Server och hello operativsystem klickar du på **inaktivera**.  

![Automatisk SQL-uppdatering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Mer information finns i [Automatisk uppdatering av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatisk säkerhetskopiering
Aktivera automatiska säkerhetskopieringar för alla databaser under **Automatisk säkerhetskopiering**. Automatisk säkerhetskopiering är inaktiverat som standard.

När du aktiverar automatisk SQL-säkerhetskopiering måste konfigurera du hello följande inställningar:

* Kvarhållningsperiod (dagar) för säkerhetskopieringar
* Storage-konto toouse för säkerhetskopieringar
* Krypteringsalternativ och lösenord för säkerhetskopieringar
* Säkerhetskopiera systemdatabaser
* Konfigurera schema för säkerhetskopiering

tooencrypt hello säkerhetskopian klickar du på **aktivera**. Ange hello **lösenord**. Azure skapar ett certifikat tooencrypt hello säkerhetskopieringar och använder hello angetts lösenord tooprotect certifikatet.

![Automatisk SQL-säkerhetskopiering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Mer information finns i [Automatisk säkerhetskopiering av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure Key Vault-integrering
toostore säkerhetshemligheter i Azure för kryptering, klickar du på **Azure key vault-integrering** och på **aktivera**.

![SQL Azure Key Vault-integrering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

hello visar följande tabell hello parametrar krävs tooconfigure Azure Key Vault-integrering.

| PARAMETER | BESKRIVNING | EXEMPEL |
| --- | --- | --- |
| **Key Vault-URL** |hello plats hello nyckelvalvet. |https://contosokeyvault.vault.azure.net/ |
| **Huvudnamn** |Azure Active Directory-tjänstens huvudnamn. Det här namnet är också hänvisade tooas hello klient-ID. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Huvudhemlighet** |Azure Active Directory-tjänstens huvudhemlighet. Den här hemligheten är också hänvisade tooas hello Klienthemlighet. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Namn på autentiseringsuppgifter** |**Namn på autentiseringsuppgifter**: AKV-integreringen skapar autentiseringsuppgifter på SQL-servern, så att hello VM toohave åtkomst toohello nyckelvalvet. Välj ett namn för autentiseringsuppgifterna. |mycred1 |

Mer information finns i [Konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines](virtual-machines-windows-ps-sql-keyvault.md).

När du har konfigurerat inställningarna för SQL Server klickar du på **OK**.

### <a name="r-services"></a>R Services
Aktivera [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx). SQL Server R Services kan du toouse avancerade analyser med SQL Server 2016. Klicka på **aktivera** på hello **SQL Server-inställningar** bladet.

![Aktivera SQL Server R Services](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)


## <a name="5-review-hello-summary"></a>5. Granska hello sammanfattning
På hello **sammanfattning** bladet granska hello sammanfattning och klicka på **OK** toocreate SQL Server, resursgruppen och resurser som har angetts för den här virtuella datorn.

Du kan övervaka hello distribution från hello azure-portalen. Hej **meddelanden** knappen hello överst i hello-skärmen visar grundläggande status för hello-distribution.

> [!NOTE]
> tooprovide dig en uppfattning om distributionen gånger, distribuerade jag en region för SQL-VM toohello östra USA med standardinställningar. Den här testdistributionen tog totalt 26 minuter toocomplete. Distributionen kan dock gå snabbare eller långsammare beroende på din region och dina valda inställningar.
> 
> 

## <a name="open-hello-vm-with-remote-desktop"></a>Öppna hello virtuella datorn med fjärrskrivbord
Använd följande steg tooconnect toohello virtuella datorn med fjärrskrivbord hello:

1. Efter hello Azure VM är inbyggd hello ikonen för hello visas VM på instrumentpanelen i Azure. Du kan också hitta den genom att bläddra bland dina befintliga virtuella datorer. Klicka på den nya virtuella SQL-datorn. Bladet **Virtuell dator** visar information om din virtuella dator.
2. Hello överst i hello **virtuella** bladet, klickar du på **Anslut**.
3. hello webbläsaren laddar ned RDP-filen för hello VM. Öppna hello RDP-filen.
    ![Remote Desktop tooSQL VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
4. hello anslutning till fjärrskrivbord meddelar dig att hello utgivaren av den här fjärranslutningen inte kan identifieras. Klicka på **Anslut** toocontinue.
5. I hello **Windows-säkerhet** dialogrutan klickar du på **använder ett annat konto**.
6. För **användarnamn** typen ** \<användarnamn >**, där <user name> är hello användarnamn som du angav när du konfigurerade hello VM. Du har tooadd ett inledande omvänt snedstreck före namnet på hello.
7. Typen hello **lösenord** som du tidigare har konfigurerat för den här virtuella datorn och klicka sedan på **OK** tooconnect.
8. Om en annan **anslutning till fjärrskrivbord** dialogrutan ber dig om tooconnect, klickar du på **Ja**.

När du har anslutit toohello SQL Server-dator kan du starta SQL Server Management Studio och Anslut med Windows-autentisering med dina autentiseringsuppgifter som lokal administratör. Om du har aktiverat SQL Server-autentisering kan du också ansluta med SQL-autentisering med hjälp av hello SQL-inloggningsnamn och lösenord som du konfigurerade under etableringen.

Åtkomst toohello datorn kan du toodirectly ändra datorn och SQL Server-inställningar baserat på dina krav. Du kan till exempel konfigurera brandväggsinställningar för hello eller ändra SQL Server-konfigurationsinställningar.

## <a name="connect-toosql-server-remotely"></a>Fjärransluta tooSQL Server
I den här självstudiekursen valde vi **offentliga** åtkomst för hello virtuell dator och **SQL Server-autentisering**. Dessa inställningar automatiskt konfigurerade hello virtuella tooallow SQL Server-anslutningar från klienter över hello internet (förutsatt att de har hello rätt SQL-inloggningsuppgifter).

> [!NOTE]
> Om du inte valde offentlig vid etablering, extra steg är nödvändiga tooaccess sedan SQL Server-instansen över hello internet. Mer information finns i [ansluta tooa virtuell dator med SQL Server](virtual-machines-windows-sql-connect.md).
> 
> 

hello följande avsnitt visar hur hello tooconnect tooyour SQL Server-instansen på den virtuella datorn från en annan dator via internet.

> [!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]
> 
> 

## <a name="next-steps"></a>Nästa steg
Annan information om hur du använder SQL Server i Azure, se [SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) och hello [vanliga frågor och svar](virtual-machines-windows-sql-server-iaas-faq.md).

Titta på en videoöversikt över SQL Server på Azure Virtual Machines [Azure VM är hello bästa plattformen för SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Utforska hello Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) för SQL Server på virtuella Azure-datorer.

