---
title: "aaaCreate en klassiska virtuella Azure-datorn kör MySQL | Microsoft Docs"
description: "Skapa en Azure-dator som kör Windows Server 2012 R2 och hello MySQL-databas med hjälp av hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 98fa06d2-9b92-4d05-ac16-3f8e9fd4feaa
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: cynthn
ms.openlocfilehash: 2c9564955c2bab197a8e494e939ce16c0b9bb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-created-with-hello-classic-deployment-model-running-windows-server-2016"></a>Installera MySQL på en virtuell dator som skapats med hello klassiska distributionsmodellen kör Windows Server 2016
[MySQL](https://www.mysql.com) är en populär öppen källkod, SQL-databas. De här självstudierna visar hur tooinstall och kör hello **av MySQL 5.7.18** som en MySQL-Server på en virtuell dator som kör **Windows Server 2016**. Din upplevelse kan skilja sig något för andra versioner av MySQL eller Windows Server.

Anvisningar om hur du installerar MySQL Linux avser: [hur tooinstall MySQL på Azure](../../linux/mysql-install.md).

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

## <a name="create-a-virtual-machine-running-windows-server-2016"></a>Skapa en virtuell dator som kör Windows Server 2016
Om du inte redan har en virtuell dator som kör Windows Server 2016, du kan använda detta [kursen](./tutorial.md) toocreate hello virtuell dator.

## <a name="attach-a-data-disk"></a>Anslut en datadisk
När hello virtuell dator har skapats kan du ansluta en datadisk (valfritt). Lägger till en datadisk rekommenderas för produktionsarbetsbelastningar och tooavoid utrymmet börjar ta slut på hello OS-enhet (C:), vilket innefattar hello-operativsystemet.

Se [hur tooattach data disk tooa Windows-dator](../attach-managed-disk-portal.md) och följ hello instruktioner för att bifoga en tom disk. Hello värden cache inställning för**ingen** eller **skrivskyddad**.

## <a name="log-on-toohello-virtual-machine"></a>Logga in toohello virtuell dator
Därefter ska du [inloggning toohello virtuella](./connect-logon.md) så att du kan installera MySQL.

## <a name="install-and-run-mysql-community-server-on-hello-virtual-machine"></a>Installera och köra MySQL Community Server på hello virtuell dator
Följ dessa steg tooinstall, konfigurera och köra hello Community-versionen av MySQL-Server:

> [!NOTE]
> När du hämtar artiklar som använder Internet Explorer, du kan ange hello IE **Förbättrad säkerhetskonfiguration** tooOff, och förenkla hello hämtar processen. Hello Start-menyn administrativa verktyg/Server Manager/lokal Server Klicka IE **Förbättrad säkerhetskonfiguration** och ange hello configuration tooOff).
>
>

1. När du har anslutit toohello virtuell dator med hjälp av fjärrskrivbord, klickar du på **Internet Explorer** hello startsidan.
2. Välj hello **verktyg** i hello övre högra hörnet (hello cogged hjul ikon) och klicka sedan på **Internetalternativ**. Klicka på hello **säkerhet** klickar du på hello **tillförlitliga platser** ikon, och klicka sedan på hello **platser** knappen. Lägg till http://*.mysql.com toohello lista över betrodda platser. Klicka på **Stäng**, och klicka sedan på **OK**.
3. I hello adressfältet i Internet Explorer, ange https://dev.mysql.com/downloads/mysql/.
4. Använd hello MySQL plats toolocate och hämta hello senaste versionen av hello MySQL installationsprogrammet för Windows. När du väljer hello MySQL Installer hämta hello-versionen som har hello slutföra uppsättning (till exempel hello mysql-installer-community-5.7.18.0.msi med en storlek på 352.8 MB) och spara hello installer.
5. När hello installer har hämtats, klickar du på **kör** toolaunch installationen.
6. På hello **licensavtalet** accepterar hello licensavtalet och klicka på **nästa**.
7. På hello **att välja en installationstyp** klickar du på hello installationstyp som du vill använda och klicka sedan på **nästa**. hello följande steg förutsätter hello val av hello **endast Server** typ av installation.
8. Om hello **Kontrollera krav** visas, klickar du på **kör** toolet hello installer installera alla komponenter som saknas. Följ instruktionerna som visas, till exempel hello C++ Redistributable runtime.
9. På hello **Installation** klickar du på **kör**. När installationen är klar klickar du på **nästa**.

10. På hello **produktkonfigurationen** klickar du på **nästa**.

11. På hello **typ och nätverk** anger din önskad konfiguration typ och anslutningen alternativ, inklusive hello TCP-porten om det behövs. Välj **visa avancerade alternativ**, och klicka sedan på **nästa**.
    ![](./media/mysql-2008r2/MySQL_TypeNetworking.png)

12. På hello **konton och roller** anger du ett starkt lösenord för MySQL rot. Lägga till ytterligare användarkonton för MySQL efter behov och klicka sedan på **nästa**.

    ![](./media/mysql-2008r2/MySQL_AccountsRoles_Filled.png)
13. På hello **Windows Service** sidan Ange standardinställningar för ändringar toohello för att köra hello MySQL-servern som en windowstjänst efter behov och klicka sedan på **nästa**.

    ![](./media/mysql-2008r2/MySQL_WindowsService.png)
14. Hej alternativ på hello **plugin-program och tillägg** sidan är valfria. Klicka på **nästa** toocontinue.
15. På hello **avancerade alternativ** anger ändringar toologging alternativ efter behov, och klicka sedan på **nästa**.

    ![](./media/mysql-2008r2/MySQL_AdvOptions.png)
16. På hello **gäller serverkonfiguration** klickar du på **kör**. När du hello konfigurationssteg är klar klickar du på **Slutför**.
17. På hello **produktkonfigurationen** klickar du på **nästa**.
18. På hello **Installation Complete** klickar du på **kopiera loggen tooClipboard** om du vill tooexamine senare, och klicka sedan på **Slutför**.
19. Skriv från startskärmen hello **mysql**, och klicka sedan på **MySQL 5.7 kommandoradsverktyget klienten**.
20. Ange hello rotlösenordet som du angav i steg 12 och visas med en uppmaning där du kan utfärda kommandon tooconfigure MySQL. Hello information om kommandon och syntax, se [MySQL referens handböcker](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html).

    ![](./media/mysql-2008r2/MySQL_CommandPrompt.png)
21. Du kan också konfigurera server standard konfigurationsinställningar, till exempel grundläggande hello och datakataloger och enheter. Mer information finns i [6.1.2 Server Configuration standardvärden](https://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Konfigurera slutpunkter

För hello MySQL service toobe tillgängliga tooclient datorer i hello Internet, måste du konfigurerar en slutpunkt för hello TCP-port och skapa en regel för Windows-brandväggen. hello port standardvärdet på vilka hello MySQL Server tjänsten lyssnar efter MySQL-klienter är 3306. Du kan ange en annan port, så länge hello porten är konsekventa med hello-värdet som angavs på hello **typ och nätverk** (steg 11 för hello ovan).

> [!NOTE]
> Överväg att hello säkerhetsriskerna med att göra hello MySQL Server service tillgängliga tooall datorer på hello Internet för produktion. Du kan definiera hello källans IP-adresser som är tillåtna toouse hello slutpunkten med en åtkomstkontrollista (ACL). Mer information finns i [hur tooSet in slutpunkter tooa virtuella](setup-endpoints.md).
>
>

tooconfigure en slutpunkt för hello MySQL Server-tjänsten:

1. I hello Azure-portalen klickar du på **virtuella datorer (klassisk)**Klicka hello namnet på den virtuella datorn MySQL och klicka sedan på **slutpunkter**.
2. Klicka i kommandofältet hello **Lägg till**.
3. På hello **lägga till slutpunkten** , ange ett unikt namn för **namn**.
4. Välj **TCP** som hello-protokoll.
5. Ange hello portnummer, till exempel **3306**, i både **offentlig Port** och **privat Port**, och klicka sedan på **OK**.

## <a name="add-a-windows-firewall-rule-tooallow-mysql-traffic"></a>Lägg till en Windows-brandväggen regeln tooallow MySQL-trafik
tooadd en regel för Windows-brandväggen som tillåter MySQL-trafik från hello Internet, kör följande kommando på hello en _Windows PowerShell-kommandotolk_ på hello MySQL virtuella datorn.

    New-NetFirewallRule -DisplayName "MySQL57" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public

## <a name="test-your-remote-connection"></a>Testa din anslutning
tootest din körs hello för fjärranslutning toohello Azure VM på MySQL-Server-tjänsten, du måste ange hello hello molnbaserad tjänst som innehåller hello VN DNS-namn.

1. I hello Azure-portalen klickar du på **virtuella datorer (klassisk)**Klicka hello namnet på den virtuella datorn på MySQL och klicka sedan på **översikt**.
2. Observera hello hello virtuella instrumentpanel **DNS-namnet** värde. Här är ett exempel:

   ![](media/mysql-2008r2/MySQL_DNSName.png)
3. Kör hello efter kommandot toolog i som en MySQL-användare från en lokal dator som kör MySQL eller hello MySQL-klienten.

     MySQL -u <yourMysqlUsername> - p -h<yourDNSname>

   Till exempel med hjälp av hello MySQL användarnamn _dbadmin3_ och hello _testmysql.cloudapp.net_ DNS-namn för hello virtuella datorn, du kan starta MySQL med hello följande kommando:

     MySQL -u dbadmin3 -p -h testmysql.cloudapp.net

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du kör MySQL finns hello [MySQL dokumentationen](http://dev.mysql.com/doc/).
