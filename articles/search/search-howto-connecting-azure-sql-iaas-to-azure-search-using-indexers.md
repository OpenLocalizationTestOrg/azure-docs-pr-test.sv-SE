---
title: "aaaSQL VM anslutning tooAzure Sök | Microsoft Docs"
description: "Aktivera krypterade anslutningar och konfigurera hello brandväggen tooallow anslutningar tooSQL Server på en virtuell Azure virtuell dator (VM) från en indexerare på Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: pablocas
editor: 
ms.assetid: 46e42e0e-c8de-4fec-b11a-ed132db7e7bc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: heidist
ms.openlocfilehash: 1f0db8a2812b0a7d012e58bb873c4b2b29fa1338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-connection-from-an-azure-search-indexer-toosql-server-on-an-azure-vm"></a>Konfigurera en anslutning från en Server med Azure Search indexeraren tooSQL på en Azure VM
Enligt beskrivningen i [ansluter Azure SQL Database tooAzure sökningen med indexerare](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), skapa indexerare mot **SQL Server på Azure Virtual Machines** (eller **SQL Azure Virtual Machines** för kort) är stöds av Azure Search, men det finns några säkerhets-relaterade krav tootake vård första. 

**Aktivitetens varaktighet:** cirka 30 minuter, under förutsättning att du redan installerat ett certifikat på hello VM.

## <a name="enable-encrypted-connections"></a>Aktivera krypterade anslutningar
Azure Search kräver en krypterad kanal för indexeraren begäranden under en offentlig Internetanslutning. Det här avsnittet innehåller hello steg toomake detta arbete.

1. Kontrollera hello egenskaper för hello tooverify hello certifikatämnesnamn är hello fullständigt kvalificerade domännamnet (FQDN) för hello Azure VM. Du kan använda ett verktyg som CertUtils eller hello certifikat snapin-modulen tooview hello egenskaper. Du kan hämta hello FQDN från hello VM service bladets Essentials i avsnittet hello **offentlig IP-adress/DNS-namnetikett** i hello [Azure-portalen](https://portal.azure.com/).
   
   * För virtuella datorer som skapats med hjälp av hello senare **Resource Manager** mallen hello FQDN formateras som `<your-VM-name>.<region>.cloudapp.azure.com`. 
   * För äldre virtuella datorer skapas som en **klassiska** VM hello FQDN formateras som `<your-cloud-service-name.cloudapp.net>`. 
2. Konfigurera SQL Server toouse hello certifikat med hjälp av hello Registereditorn (regedit). 
   
    Även om SQL Server Configuration Manager används ofta för den här uppgiften, kan du använda den för det här scenariot. Det kommer inte att hitta hello importera certifikatet eftersom hello FQDN för hello VM på Azure inte matchar hello FQDN utifrån hello VM (den visar hello domän hello lokal dator eller hello nätverket domän toowhich den är ansluten). När namnen inte matchar, kan du använda regedit toospecify hello certifikat.
   
   * Bläddra i regedit toothis registernyckeln: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     Hej `[MSSQL13.MSSQLSERVER]` del varierar beroende på version och instansnamn. 
   * Värdet för hello hello **certifikat** viktiga toohello **tumavtrycket** för hello SSL-certifikat du importerade toohello VM.
     
     Det finns flera sätt tooget hello tumavtrycket, vissa bättre än andra. Om du kopierar den från hello **certifikat** snapin-modulen i MMC, hämtar du förmodligen upp ett osynligt inledande tecken [som beskrivs i den här supportartikeln](https://support.microsoft.com/kb/2023869/), vilket resulterar i ett fel när du försöker en anslutning. Det finns flera lösningar för att korrigera problemet. hello enklaste är toobackspace över och sedan skriva hello första tecknet i hello tumavtrycket tooremove hello inledande tecken i hello nyckelvärdesfält i regedit. Du kan också använda ett annat verktyg toocopy hello tumavtryck.
3. Bevilja behörigheter toohello-tjänstkontot. 
   
    Kontrollera att hello SQL Server-tjänstkontot har beviljats behörighet för hello privata nyckeln för hello SSL-certifikat. Om du missar det här steget startar inte SQL Server. Du kan använda hello **certifikat** snapin-modulen eller **CertUtils** för den här uppgiften.
4. Starta om hello SQL Server-tjänsten.

## <a name="configure-sql-server-connectivity-in-hello-vm"></a>Konfigurera SQL Server-anslutningen i hello VM
När du har skapat hello krypterad anslutning krävs för Azure Search finns ytterligare konfiguration steg inre tooSQL Server på virtuella Azure-datorer. Om du inte redan har gjort är hello nästa steg toofinish konfigurationen med hjälp av någon av följande artiklar:

* För en **Resource Manager** VM, se [ansluta tooa virtuell dator med SQL Server på Azure med hjälp av hanteraren för filserverresurser](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* För en **klassiska** VM, se [ansluta tooa virtuell dator med SQL Server på Azure klassiska](../virtual-machines/windows/classic/sql-connect.md).

I synnerhet granska hello i varje artikel för avsnittet ”hello ansluter via internet”.

## <a name="configure-hello-network-security-group-nsg"></a>Konfigurera hello Nätverkssäkerhetsgrupp (NSG)
Det är inte ovanligt tooconfigure hello NSG och motsvarande Azure endpoint eller åtkomstkontrollista (ACL) toomake din Azure VM tillgänglig tooother parter. Risken är att du har gjort detta innan tooallow egna program logik tooconnect tooyour SQL Azure VM. Det är inte annorlunda för ett Azure Search anslutning tooyour SQL Azure VM. 

hello länkarna nedan ger instruktioner för NSG-konfiguration för distribueringar av Virtuella datorer. Följ dessa instruktioner tooACL en slutpunkt för Azure SEarch baserat på dess IP-adress.

> [!NOTE]
> Bakgrund, se [vad är en Nätverkssäkerhetsgrupp?](../virtual-network/virtual-networks-nsg.md)
> 
> 

* För en **Resource Manager** VM, se [hur toocreate NSG: er för ARM-distributioner](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 
* För en **klassiska** VM, se [hur toocreate NSG: er för klassiska distributioner](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-adresser kan utgöra några utmaningar som enkelt kan lösa om du är medveten om hello problem och möjliga lösningar. hello återstående avsnitten ger rekommendationer för att hantera problem relaterade tooIP adresser i hello ACL.

#### <a name="restrict-access-toohello-search-service-ip-address"></a>Begränsa åtkomst toohello Sök service IP-adress
Vi rekommenderar starkt att du begränsar hello åtkomst toohello IP-adressen för din söktjänst i hello ACL i stället för att göra din SQL Azure Virtual Machines bred öppna tooany anslutningsbegäranden. Du kan enkelt ta reda hello IP adress genom att skriva hello FQDN (till exempel `<your-search-service-name>.search.windows.net`) för din söktjänst.

#### <a name="managing-ip-address-fluctuations"></a>Hantera variationer för IP-adress
Om din söktjänst har endast en sökenheten (det vill säga en replik och en partition), ändras hello IP-adress under rutinunderhåll tjänsten har startats om upphäva en befintlig ACL med IP-adressen för din söktjänst.

Enkelriktade tooavoid hello efterföljande anslutningsfel är toouse mer än en replik och en partition i Azure Search. Då ökar hello kostnad, men den också löser problemet för hello IP-adress. I Azure Search ändras inte IP-adresser när du har mer än en sökning-enhet.

En andra sättet är tooallow hello anslutning toofail och sedan konfigurera om hello ACL: er i hello NSG. I genomsnitt räkna du med IP-adresser toochange varje några veckor. Den här metoden kan vara användbart för kunder som har kontrollerat indexering på ett ovanligt sätt.

Ett tredje sätt lönsam (men inte särskilt säkert) är toospecify hello IP-adressintervall hello Azure-region där din söktjänst etableras. hello lista med IP-adressintervall som offentliga IP-adresser tilldelas tooAzure resurser har publicerats på [Azure Datacenter-IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-hello-azure-search-portal-ip-addresses"></a>Inkludera hello Azure Search portal IP-adresser
Om du använder hello Azure portal toocreate en indexerare måste Azure Search portal logik också åtkomst tooyour SQL Azure VM under skapandeprocessen. Azure search portal IP-adresser kan hittas genom att skriva `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Nästa steg
Med konfigurationen utanför hello sätt kan ange du nu en SQL Server på Azure VM som hello datakälla för en indexerare för Azure Search. Se [ansluter Azure SQL Database tooAzure sökningen med indexerare](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) för mer information.

