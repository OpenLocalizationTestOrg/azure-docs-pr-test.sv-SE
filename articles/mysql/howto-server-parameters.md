---
title: "Konfigurera parametrar för Server i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar parametrarna för tillgänglig server i Azure-databas för MySQL med Azure-portalen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.openlocfilehash: 718bbf359253849fbc989c563ffd6d1099f92348
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-using-the-azure-portal"></a>Konfigurera parametrar för server i Azure-databas för MySQL med Azure-portalen

Azure-databas för MySQL stöder konfiguration av vissa serverparametrar. Den här artikeln beskriver hur du konfigurerar dessa parametrar med Azure-portalen och listar parametrarna som stöds, standardvärden och intervallet med giltiga värden. Parametrar för server kan justeras. Endast de som listas här stöds.

## <a name="navigate-to-server-parameters-blade-on-azure-portal"></a>Navigera till bladet parametrar Server på Azure-portalen

Logga in på Azure-portalen och sedan på din Azure-databas för MySQL-servernamnet. Under den **inställningar** klickar du på **serverparametrar** att öppna bladet parametrar för Server för Azure-databas för MySQL.

![Azure portal parametrar serverblad](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista över konfigurerbara serverparametrar

I följande tabell visas de parametrar som stöds server. Parametrarna kan konfigureras enligt kraven för application.

> [!div class="mx-tableFixed"]
|Parameternamn|Standardvärde|intervallet|Beskrivning|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Styr hur många mikrosekunder binär logg commit väntar innan du synkroniserar den binära loggfilen till disk.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|Maximalt antal transaktioner ska vänta innan försöket avbryts aktuella fördröjningen som anges av binlog-grupp-commit-sync-fördröjning.|
|character_set_server|LATIN1|BIG5, UTF8MB4 osv.|Använd charset_name som standardteckenuppsättningen för servern.|
|div_precision_increment|4|0-30|Antalet siffror som du vill öka omfattningen av resultatet av delning.|
|group_concat_max_len|1024|4-16777216|Tillåtna resultatlängden i byte för GROUP_CONCAT().|
|innodb_adaptive_hash_index|ON|PÅ, AV|Om innodb anpassningsbar hash-index är aktiverade eller inaktiverade.|
|innodb_lock_wait_timeout|50|1-3600|Hur lång tid i sekunder en InnoDB transaktion väntar på Lås rad innan den ger upp.|
|interactive_timeout|1800|10-1800|Antalet sekunder servern väntar för aktivitet på en interaktiv anslutning innan den stängs.|
|log_bin_trust_function_creators|INAKTIVERA|PÅ, AV|Den här variabeln används när binär loggning är aktiverad. Den kontrollerar om lagrade funktionen skapare kan vara betrott inte för att skapa lagrade funktioner som gör osäkra händelser ska skrivas till binär logg.|
|log_queries_not_using_indexes|INAKTIVERA|PÅ, AV|Loggar frågor som förväntas för att hämta alla rader till långsam fråga loggen.|
|log_slow_admin_statements|INAKTIVERA|PÅ, AV|Inkludera långsam administrativa rapporter i instruktionerna som skrivs till loggfilen för långsam frågor.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Begränsar antalet sådana frågor per minut som går att skriva till loggfilen för långsam frågor.|
|long_query_time|10|1-1E + 100|Om en fråga tar längre tid än detta antal sekunder, ökar servern variabeln Slow_queries status.|
|max_allowed_packet|536870912|1024-1073741824|Den maximala storleken för ett paket eller valfri sträng genereras/mellanliggande eller valfri parameter som skickas av mysql_stmt_send_long_data() C-API-funktion.|
|min_examined_row_limit|0|0-18446744073709551615|Loggar frågor som har större än det konfigurerade antalet rader i loggfilen för långsam frågor. |
|server_id|3293747068|1000-4294967295|ID på servern som används i replikeringen att ge varje bakgrund och slave en unik identitet.|
|slave_net_timeout|60|30-3600|Antalet sekunder för mer data från master innan underordnade anses anslutningen skadad, avbryter läsningen och försöker återansluta.|
|slow_query_log|INAKTIVERA|PÅ, AV|Aktivera eller inaktivera långsam frågeloggen.|
|sql_mode|0 markerat|ALLOW_INVALID_DATES, IGNORE_SPACE osv.|Den aktuella SQL serverläge.|
|TIME_ZONE|SYSTEM|Exempel:-8: 00, +05: 30|Serverns tidszon.|
|wait_timeout|120|60-240|Antalet sekunder servern väntar för aktivitet på en icke-interaktiv anslutning innan den stängs.|

## <a name="next-steps"></a>Nästa steg
- [Anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)
