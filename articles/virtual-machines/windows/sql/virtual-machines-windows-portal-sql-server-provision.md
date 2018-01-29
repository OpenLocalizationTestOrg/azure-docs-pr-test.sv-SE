---
title: "Så här konfigurerar du Windows SQL Server 2017 VMs i Azure portal | Microsoft Docs"
description: "Den här instruktioner beskriver alternativen för att skapa Windows SQL Server 2017 virtuella datorer i Azure-portalen."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: jroth
ms.openlocfilehash: 440c783de73652ad2d312cd92db8635dc65df9ed
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Så här skapar du en virtuell dator med Windows SQL Server i Azure-portalen

Den här guiden går igenom de olika alternativ som är tillgängliga när du skapar en virtuell dator med Windows SQL Server i Azure-portalen. Du kan följa stegen för att skapa din egen SQL Server-VM när lära dig mer om de olika alternativen. Eller så går du till avsnittet för referensen på ett visst steg i portalen.

> [!TIP]
> Om du vill komma igång snabbt med standardvärden för portalen finns i [Azure quickstart - skapa en SQL Server-VM i portalen](quickstart-sql-vm-create-portal.md).

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a id="select"></a>Välja en VM-avbildning med SQL från galleriet

1. Logga in på [Azure Portal](https://portal.azure.com) med ditt konto.

1. Klicka på **Nytt** på Azure Portal. Fönstret **Nytt** öppnas.

1. I fönstret **Nytt** klickar du på **Compute** och sedan på **Visa alla**.

   ![Nytt Compute-fönster](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

1. Skriv**SQL Server 2017** i sökfältet och tryck på RETUR.

1. Klicka sedan på **filterikonen**.

1. I filterfönstren markerar du underkategorin **Windows-baserad** och **Microsoft** som utgivare. Klicka sedan på **Klar** för att filtrera resultaten efter Microsoft-publicerade, Windows SQL Server-avbilder.

   ![Fönstret Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

1. Granska de tillgängliga SQL Server-avbildningarna. Varje avbildning identifierar en version av SQL Server och ett operativsystem.

1. Välj avbildningen med namnet **Kostnadsfri SQL Server-licens: SQL Server 2017 Developer på Windows Server 2016**.

   > [!TIP]
   > Vi använder Developer-versionen i den här självstudiekursen eftersom det är en komplett version av SQL Server som är kostnadsfri i samband med utvecklingstester. Du betalar endast för kostnaden för den VM som körs. Du kan dock själv välja vilken avbildning du vill använda i den här självstudiekursen.

   > [!TIP]
   > För avbildningar av virtuella SQL-datorer ingår licenskostnaden för SQL Server i minutpriset för den virtuella dator som du skapar (förutom för versionerna Developer och Express). SQL Server Developer är kostnadsfri när den används till utveckling och testning (inte i samband med produktion) och SQL Express är kostnadsfri för enklare arbetsbelastningar (mindre än 1 GB minne och mindre än 10 GB lagringsutrymme). Ett annat alternativ är att använda en egen licens (BYOL, bring-your-own-license) och endast betala för den virtuella datorn. Dessa avbildningsnamn föregås av {BYOL}. 
   >
   > Mer information om alternativen finns i [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prisvägledning för virtuella SQL Server Azure-datorer).

1. Under **Välj en distributionsmodell** kontrollerar du att **Resource Manager** är valt. Resource Manager är den rekommenderade distributionsmodellen för nya virtuella datorer. 

1. Klicka på **Skapa**.

    ![Skapa den virtuella SQL-datorn med Resource Manager](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a id="configure"></a> Konfigurera den virtuella datorn
Det finns fem fönster för att konfigurera en virtuell dator med SQL Server.

| Steg | Beskrivning |
| --- | --- |
| **Grundläggande inställningar** |[Konfigurera grundläggande inställningar](#1-configure-basic-settings) |
| **Storlek** |[Välj den virtuella datorns storlek](#2-choose-virtual-machine-size) |
| **Inställningar** |[Konfigurera valfria funktioner](#3-configure-optional-features) |
| **SQL Server-inställningar** |[Konfigurera SQL Server-inställningar](#4-configure-sql-server-settings) |
| **Sammanfattning** |[Granska sammanfattningen](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Konfigurera grundläggande inställningar

Ange följande i fönstret **Grundläggande inställningar**:

* Ange ett unikt **namn** för den virtuella datorn.

* Välj **SSD** som disktyp för virtuell dator för bästa prestanda.

* Ange ett **användarnamn** för det lokala administratörskontot på den virtuella datorn. Det här kontot läggs också till i den fasta SQL Server-serverrollen **sysadmin**.

* Ange ett starkt **lösenord**.

* Om du har flera prenumerationer kontrollerar du att prenumerationen är rätt för den nya virtuella datorn.

* I rutan **Resursgrupp** skriver du ett namn för en ny resursgrupp. Du kan även använda en befintlig resursgrupp genom att klicka på **Använd befintlig**. En resursgrupp är en samling relaterade resurser i Azure (virtuella datorer, lagringskonton, virtuella nätverk osv.).

  > [!NOTE]
  > En ny resursgrupp är praktiskt om du bara testar eller lär dig om SQL Server-distributioner i Azure. När du är klar med testet tar du bort resursgruppen. När du gör det tas den virtuella datorn och alla resurser som associeras med resursgruppen bort automatiskt. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md).

* Välj en **Plats** för den Azure-region som ska vara värd för den här distributionen.

* Spara inställningarna genom att klicka på **OK**.

    ![Fönstret Grundläggande SQL-inställningar](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Välj den virtuella datorns storlek

I steget **Storlek** väljer du en storlek för den virtuella datorn i fönstret **Välj en storlek**. I fönstret visas först rekommenderade datorstorlekar baserat på den avbildning som du har valt.

> [!IMPORTANT]
> Den uppskattade månadskostnaden som visas på sidan **Välj en storlek** omfattar inte SQL Server-licenskostnaden. Detta är endast kostnaden för den virtuella datorn. För SQL Server-utgåvorna Express och Developer är detta den totala uppskattade kostnaden. För andra utgåvor kan du se [sidan med priser för Windows Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) and och välja din utgåva av SQL Server. Läs också [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prisvägledning för virtuella SQL Server Azure-datorer).

![Storleksalternativ för en virtuell dator med SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Vad gäller produktionsarbetsbelastningar hittar du rekommendationer för datorstorlek och konfiguration i [Prestandametodtips för SQL Server på virtuella Azure-datorer](virtual-machines-windows-sql-performance.md). Om du behöver en datorstorlek som inte finns i listan, klickar du på **Visa alla**.

> [!NOTE]
> Mer information om storlekar för virtuella datorer finns i [Storlekar för virtuella datorer](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Välj din datorstorlek och klicka på **Välj**.

## <a name="3-configure-optional-features"></a>3. Konfigurera valfria funktioner

På sidan **Inställningar** ställer du in Azure-lagring, nätverk och övervakning för den virtuella datorn.

* Under **Lagring** väljer du **Ja** under använd **Managed Disks**.

   > [!NOTE]
   > Microsoft rekommenderar Managed Disks för SQL Server. Managed Disks hanterar lagring i bakgrunden. När virtuella datorer med Managed Disks finns i samma tillgänglighetsuppsättning, distribuerar Azure dessutom lagringsresurser för att tillhandahålla rätt redundans. Mer information finns i [Översikt över Azure Managed Disks](../../../storage/storage-managed-disks-overview.md). Mer information om hanterade diskar i en tillgänglighetsuppsättning finns i [Använda hanterade diskar för virtuella datorer i tillgänglighetsuppsättning](../manage-availability.md).

* Under **Nätverk** kan du acceptera de automatiskt ifyllda värdena. Du kan också klicka på varje funktion för att manuellt konfigurera det **virtuella nätverket**, **undernätet**, den **offentliga IP-adressen** och **nätverkssäkerhetsgruppen**. Behåll standardinställningarna i den här kursen.

* Azure aktiverar **övervakning** som standard med samma lagringskonto som används för den virtuella datorn. Du kan ändra dessa inställningar här.

* Under **Tillgänglighetsuppsättning** kan du låta standardinställningen **ingen** vara kvar under den här självstudiekursen. Om du planerar att konfigurera SQL AlwaysOn-tillgänglighetsgrupper konfigurerar du tillgängligheten för att undvika att behöva återskapa den virtuella datorn.  Mer information finns i [Hantera tillgängligheten för Virtual Machines](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

När du har konfigurerat dessa inställningar klickar du på **OK**.

## <a name="4-configure-sql-server-settings"></a>4. Konfigurera SQL Server-inställningar
På sidan **SQL Server-inställningar** anger du specifika inställningar och optimeringar för SQL Server. Du kan bland annat konfigurera följande inställningar för SQL Server.

| Inställning |
| --- |
| [Anslutning](#connectivity) |
| [Autentisering](#authentication) |
| [Storage-konfiguration](#storage-configuration) |
| [Automatisk uppdatering](#automated-patching) |
| [Automatisk säkerhetskopiering](#automated-backup) |
| [Azure Key Vault-integrering](#azure-key-vault-integration) |
| [SQL Server Machine Learning Services](#sql-server-machine-learning-services) |

### <a name="connectivity"></a>Anslutning

Under **SQL-anslutning** anger du vilken typ av åtkomst du vill ha till SQL Server-instansen på den här virtuella datorn. I den här kursen väljer du **Offentlig (Internet)** för att tillåta anslutningar till SQL Server från datorer eller tjänster på Internet. När det här alternativet är valt konfigurerar Azure automatiskt brandväggen och nätverkssäkerhetsgruppen så att trafik tillåts på port 1433.

![SQL-anslutningsalternativ](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Som standard lyssnar SQL Server på en känd port, **1433**. För ökad säkerhet kan du ändra porten så att den lyssnar på en icke-standardport, till exempel 1401, i den föregående dialogrutan. I så fall måste du ansluta via den porten från alla klientverktyg, till exempel SSMS.

Om du vill ansluta till SQL Server via Internet måste du också aktivera SQL Server-autentisering, som beskrivs i nästa avsnitt.

Om du föredrar att inte aktivera anslutningar till databasmotorn via Internet väljer du något av följande alternativ:

* **Lokalt (endast inuti VM)** om du bara vill tillåta anslutningar till SQL Server inifrån den virtuella datorn.
* **Privat (inom Virtual Network)** om du vill tillåta anslutningar till SQL Server från datorer eller tjänster i samma virtuella nätverk.

I allmänhet kan du förbättra säkerheten genom att välja den mest restriktiva anslutningen som ditt scenario medger. Men alla alternativ kan skyddas med regler för nätverkssäkerhetsgrupper och SQL/Windows-autentisering. Du kan redigera nätverkssäkerhetsgruppen när den virtuella datorn har skapats. Mer information finns i [Säkerhetsöverväganden för SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-security.md).

> [!NOTE]
> Virtuella datoravbildningar för SQL Server Express-versioner aktiverar inte TCP/IP-protokollet automatiskt. Detta gäller även för offentliga och privata anslutningsalternativ. För Express-versioner måste du använda SQL Server Configuration Manager för att [manuellt aktivera TCP/IP-protokollet](#configure-sql-server-to-listen-on-the-tcp-protocol) när du har skapat den virtuella datorn.

### <a name="authentication"></a>Autentisering

Om du kräver SQL Server-autentisering klickar du på **Aktivera** under **SQL-autentisering**.

![SQL Server-autentisering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Om du planerar att ansluta till SQL Server via Internet (dvs. alternativet för offentlig anslutning) måste du aktivera SQL-autentisering här. Offentlig åtkomst till SQL Server kräver användning av SQL-autentisering.
> 
> 

Om du aktiverar SQL Server-autentisering anger du ett **inloggningsnamn** och **lösenord**. Det här användarnamnet är konfigurerat som en inloggning med SQL Server-autentisering och som en medlem i den fasta serverrollen **sysadmin**. Mer information om autentiseringslägen finns i [Välja ett autentiseringsläge](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

Om du inte aktiverar SQL Server-autentisering kan du använda det lokala administratörskontot på den virtuella datorn för att ansluta till SQL Server-instansen.

### <a name="storage-configuration"></a>Storage-konfiguration

Klicka på **Storage-konfiguration** för att ange lagringskraven.

![SQL Storage-konfiguration](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Det här alternativet är inte tillgängligt om du manuellt har konfigurerat den virtuella datorn för användning av standardlagring. Automatisk lagringsoptimering är endast tillgängligt för Premium Storage.

> [!TIP]
> Antalet stopp och den övre gränsen för varje skjutreglage beror på den valda storleken för virtuell dator. En större och kraftfullare virtuell dator kan skalas upp mer.

Du kan ange krav som I/O-åtgärder per sekund (IOPs), genomflöde i MB/s och totalt lagringsutrymme. Konfigurera dessa värden med hjälp av reglagen. Du kan ändra dessa lagringsinställningar baserat på arbetsbelastningen. Portalen beräknar automatiskt antalet diskar som ska anslutas och konfigureras baserat på de här kraven.

Under **Storage optimerat för** väljer du något av följande alternativ:

* **Allmänt** är standardinställningen och har stöd för de flesta arbetsbelastningar.
* **Transaktionell** bearbetning optimerar lagringen för traditionella OLTP-arbetsbelastningar för databaser.
* **Datalagerhantering** optimerar lagringen för analys- och rapporteringsarbetsbelastningar.

### <a name="automated-patching"></a>Automatisk uppdatering

**Automatisk uppdatering** är aktiverat som standard. Med inställningen Automatisk uppdatering kan Azure korrigera SQL Server och operativsystemet automatiskt. Ange en dag i veckan, en tid och längden på en underhållsperiod. Azure utför uppdateringar under den här underhållsperioden. Den virtuella datorns lokala tid används för underhållsperiodens schema. Om du inte vill att Azure ska uppdatera SQL Server och operativsystemet automatiskt klickar du på **Inaktivera**.  

![Automatisk SQL-uppdatering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Mer information finns i [Automatisk uppdatering av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatisk säkerhetskopiering

Aktivera automatiska säkerhetskopieringar för alla databaser under **Automatisk säkerhetskopiering**. Automatisk säkerhetskopiering är inaktiverat som standard.

När du aktiverar automatisk SQL-säkerhetskopiering kan du konfigurera följande:

* Kvarhållningsperiod (dagar) för säkerhetskopieringar
* Lagringskonto som ska användas för säkerhetskopieringar
* Krypteringsalternativ och lösenord för säkerhetskopieringar
* Säkerhetskopiera systemdatabaser
* Konfigurera schema för säkerhetskopiering

Om du vill kryptera säkerhetskopian klickar du på **Aktivera**. Ange sedan **lösenordet**. Azure skapar ett certifikat för att kryptera säkerhetskopiorna och använder det angivna lösenordet för att skydda certifikatet.

![Automatisk SQL-säkerhetskopiering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Mer information finns i [Automatisk säkerhetskopiering av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure Key Vault-integrering

Om du vill lagra säkerhetshemligheter i Azure för kryptering klickar du på **Azure Key Vault-integrering** och klickar sedan på **Aktivera**.

![SQL Azure Key Vault-integrering](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Följande tabell innehåller de parametrar som krävs för att konfigurera Azure Key Vault-integrering.

| PARAMETER | BESKRIVNING | EXEMPEL |
| --- | --- | --- |
| **Key Vault-URL** |Platsen för nyckelvalvet. |https://contosokeyvault.vault.azure.net/ |
| **Huvudnamn** |Azure Active Directory-tjänstens huvudnamn. Det här namnet kallas också för klient-ID:t. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Huvudhemlighet** |Azure Active Directory-tjänstens huvudhemlighet. Den här hemligheten kallas även för klienthemligheten. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Namn på autentiseringsuppgifter** |**Namn på autentiseringsuppgifter**: AKV-integreringen skapar autentiseringsuppgifter på SQL-servern som gör att den virtuella datorn kan komma åt nyckelvalvet. Välj ett namn för autentiseringsuppgifterna. |mycred1 |

Mer information finns i [Konfigurera Azure Key Vault-integrering för SQL Server på Azure Virtual Machines](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services

Du kan aktivera [SQL Server Machine Learning Services](https://msdn.microsoft.com/library/mt604845.aspx). Det ger dig möjlighet att använda avancerad analys med SQL Server 2017. Klicka på **Aktivera** i fönstret **SQL Server-inställningar**.

![Aktivera SQL Server Machine Learning Services](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

När du har konfigurerat inställningarna för SQL Server klickar du på **OK**.

## <a name="5-review-the-summary"></a>5. Granska sammanfattningen

I fönstret **Sammanfattning** granskar du sammanfattningen och klickar på **Köp** för att skapa SQL Server, resursgrupp och resurser för den här virtuella datorn.

Du kan övervaka distributionen från Azure Portal. Knappen **Meddelanden** längst upp på skärmen visar grundläggande status för distributionen.

> [!NOTE]
> För att ge dig en uppfattning om distributionstiden distribuerade jag en virtuell dator med SQL till östra USA med standardinställningar. Det här testdistributionen tog cirka 12 minuter att slutföra. Distributionen kan dock gå snabbare eller långsammare beroende på din region och dina valda inställningar.

## <a id="remotedesktop"></a> Öppna den virtuella datorn med Fjärrskrivbord

Använd följande anvisningar för att ansluta till den virtuella SQL Server-datorn med Fjärrskrivbord:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

När du ansluter till den virtuella SQL Server-datorn kan du starta SQL Server Management Studio och ansluta med Windows-autentisering med hjälp av dina autentiseringsuppgifter som lokal administratör. Om du har aktiverat SQL Server-autentisering kan du också ansluta med SQL-autentisering med hjälp av SQL-inloggningsnamnet och SQL-lösenordet som du konfigurerade under etableringen.

När du har anslutit till datorn kan du direkt ändra inställningarna för datorn och SQL Server efter behov. Du kan till exempel konfigurera brandväggsinställningarna eller ändra konfigurationsinställningarna för SQL Server.

## <a id="connect"></a> Fjärransluta till SQL Server

I den här självstudiekursen valde vi **offentlig** åtkomst för den virtuella datorn och **SQL Server-autentisering**. Dessa inställningar konfigurerade automatiskt den virtuella datorn så att SQL Server-anslutningar tillåts från alla klienter över Internet (förutsatt att de har rätt SQL-inloggningsuppgifter).

> [!NOTE]
> Om du inte valde Offentlig under etableringen kan du ändra SQL-anslutningsinställningarna via portalen efter etableringen. Mer information hittar du i [Ändra SQL-anslutningsinställningarna](virtual-machines-windows-sql-connect.md#change).

Följande avsnitt visar hur du ansluter till SQL Server-instansen på den virtuella datorn från en annan dator via Internet.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nästa steg

Mer information om hur du använder SQL Server i Azure finns i [SQL Server på Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) och [Vanliga frågor och svar](virtual-machines-windows-sql-server-iaas-faq.md).