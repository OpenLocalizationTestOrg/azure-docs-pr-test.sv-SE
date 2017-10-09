---
title: "aaaSecurity överväganden för SQL Server i Azure | Microsoft Docs"
description: "Det här avsnittet innehåller allmänna riktlinjer för att skydda SQL Server som körs i en virtuell dator i Azure."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2017
ms.author: jroth
ms.openlocfilehash: 14c3d828fa87446da67beea6d28886de254afe15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Säkerhetsöverväganden för SQL Server på Azure Virtual Machines

Det här avsnittet innehåller allmänna riktlinjer att upprätta säker åtkomst tooSQL Server-instanser i en Azure virtuell dator (VM).

Azure uppfyller flera industrins föreskrifter och standarder som gör det möjligt toobuild en lösning som är kompatibel med SQL Server som körs på en virtuell dator. Information om regelefterlevnad med Azure finns [Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-toohello-sql-vm"></a>Kontrollera åtkomst toohello SQL VM

När du skapar den virtuella datorn för SQL Server, kan du överväga hur toocarefully styra vem som har åtkomst toohello datorn och tooSQL Server. I allmänhet bör du göra hello följande:

- Begränsa åtkomst tooSQL Server tooonly hello program och klienter som behöver den.
- Följ Metodtips för hantering av användarkonton och lösenord.

hello följande avsnitt ger förslag på tänka igenom dessa punkter.

## <a name="secure-connections"></a>Säkra anslutningar

När du skapar en virtuell dator med SQL Server med en bild i galleriet hello **SQL Server-anslutning** alternativet ger du hello valet av **lokala (inuti VM)**, **privat (inom Virtual Network)** , eller **offentlig (Internet)**.

![SQL Server-anslutning](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

Välj hello mest restriktiva alternativ för ditt scenario hello bäst säkerhet. Till exempel om du kör ett program som ansluter till SQL Server på hello samma VM sedan **lokala** är hello säkrast. Om du kör ett Azure-program som kräver åtkomst toohello SQL Server, sedan **privata** skyddar kommunikation tooSQL Server endast inom hello anges [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Om du behöver **offentliga** (internest) åtkomst toohello SQL Server-VM, sedan se till att toofollow andra metodtips i det här avsnittet tooreduce din angreppsytan.

hello valda alternativ i hello portal använder inkommande säkerhetsregler på hello VMs [Nätverkssäkerhetsgruppen](../../../virtual-network/virtual-networks-nsg.md) (NSG) tooallow eller neka nätverket trafik tooyour virtuella datorn. Du kan ändra eller skapa nya regler för inkommande NSG tooallow trafik toohello SQL Server-porten (standard 1433). Du kan också ange specifika IP-adresser som är tillåten toocommunicate via den här porten.

![Regler för nätverkssäkerhetsgrupper](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Dessutom tooNSG regler toorestrict nätverkstrafik kan du också använda hello Windows-brandväggen på hello virtuella datorn.

Om du använder slutpunkter med hello klassiska distributionsmodellen, ta bort alla slutpunkter på hello virtuell dator om du inte använder dem. Anvisningar om hur du använder ACL: er med slutpunkter finns [hantera hello ACL på en slutpunkt](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint). Detta är inte nödvändigt för virtuella datorer som använder hello Resource Manager.

Slutligen bör du aktivera krypterade anslutningar för hello instans av hello SQL Server Database Engine i ditt virtuella Azure-datorn. Konfigurera SQL server-instans med ett signerat certifikat. Mer information finns i [aktivera krypterade anslutningar toohello databasmotorn](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) och [anslutningssträngar](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Använda en icke-standardport

Som standard lyssnar SQL Server på en känd port 1433. Konfigurera SQL Server-toolisten på en annan port än standardporten, till exempel 1401 för ökad säkerhet. Om du etablerar en SQL Server-galleriet avbildning i hello Azure-portalen kan du ange den här porten i hello **SQL Server-inställningar** bladet.

tooconfigure detta när du har etablerat, har du två alternativ:

- För hanteraren för virtuella datorer, kan du välja **SQL Server-konfigurationsfilen** från hello VM översikt bladet. Detta ger ett alternativ toochange hello port.

  ![TCP-port ändra i portalen](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- För klassiska virtuella datorer eller för SQL Server-datorer som inte har etablerats med hello portal konfigurera du manuellt hello port genom att ansluta via fjärranslutning toohello VM. Hello konfigurationssteg finns [konfigurera en Server tooListen på en specifik TCP-Port](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). Om du använder den här manuella metoden måste du också tooadd en Windows-brandväggen regeln tooallow inkommande trafik på den TCP-porten.

> [!IMPORTANT]
> Ange en annan port än standardporten är en bra idé om SQL Server-porten är öppen toopublic internet-anslutningar.

När SQL Server lyssnar på en icke-standardport måste du ange hello port när du ansluter. Tänk dig ett scenario där hello serverns IP-adress är 13.55.255.255 och SQL Server lyssnar på port 1401. tooconnect tooSQL Server anger du `13.55.255.255,1401` i hello anslutningssträngen.

## <a name="manage-accounts"></a>Hantera konton

Du vill inte att angripare tooeasily gissning kontonamn eller lösenord. Använd följande tips toohelp hello:

- Skapa en unik lokala administratörskontot som inte heter **administratör**.

- Använd komplexa starka lösenord för alla konton. Mer information om hur toocreate ett starkt lösenord, se [skapa ett starkt lösenord](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) artikel.

- Som standard väljer Windows-autentisering i Azure under installationen av SQL Server-dator. Därför hello **SA** inloggning är inaktiverat och ett lösenord har tilldelats av installationsprogrammet. Vi rekommenderar att hello **SA** inloggning inte ska användas eller aktiverat. Om du måste ha en SQL-inloggning kan du använda en av hello följande strategier:

  - Skapa ett SQL-konto med ett unikt namn som har **sysadmin** medlemskap. Du kan göra detta från hello-portalen genom att aktivera **SQL-autentisering** under etableringen.

    > [!TIP] 
    > Om du inte aktiverar SQL-autentisering under etableringen, måste du manuellt ändra hello autentiseringsläge för**SQL Server och Windows-autentiseringsläge**. Mer information finns i [ändra Server-autentiseringsläge](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Om du måste använda hello **SA** inloggning, aktivera hello inloggningen efter etablering och tilldela ett nytt starkt lösenord.

## <a name="follow-on-premises-best-practices"></a>Följ Metodtips för lokalt

Dessutom toohello metoder som beskrivs i det här avsnittet, rekommenderar vi att du granskar och implementera hello traditionella lokala säkerhetsåtgärder tillämpliga. Mer information finns i [säkerhetsaspekter för en SQL Server-Installation](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Nästa steg

Om du är intresserad av bästa praxis för prestanda, se [prestandarelaterade Metodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

För andra avsnitt relaterade toorunning SQL Server i virtuella Azure-datorer, se [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

