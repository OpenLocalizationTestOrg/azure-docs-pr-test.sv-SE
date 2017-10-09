---
title: "aaaHow tooConfigure serverparametrar i Azure-databas för MySQL | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure tillgängliga serverparametrar i Azure-databas för att använda MySQL hello Azure-portalen."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/19/2017
ms.openlocfilehash: 8292c8eda605854a06b6a9c82185c857bd353cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-server-parameters-in-azure-database-for-mysql-using-hello-azure-portal"></a>Hur tooconfigure serverparametrar i Azure-databas för att använda MySQL hello Azure-portalen

Azure-databas för MySQL stöder konfiguration av vissa serverparametrar. Den här artikeln beskriver hur tooconfigure dessa parametrar med hello Azure-portalen och visar hello stöds parametrar, hello standardvärden och hello intervallet med giltiga värden. Parametrar för server kan justeras. Endast de som listas här hello stöds.

## <a name="navigate-tooserver-parameters-blade-on-azure-portal"></a>Navigera tooServer parametrar bladet på Azure-portalen

Logga in toohello Azure-portalen och sedan klicka på din Azure-databas för MySQL-servernamnet. Under hello **inställningar** klickar du på **serverparametrar** tooopen hello serverblad parametrar för hello Azure för MySQL-databas.

![Azure portal parametrar serverblad](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista över konfigurerbara serverparametrar

hello i den följande tabellen listas serverparametrar hello stöds för närvarande. hello-parametrar kan konfigureras enligt tooyour programkrav.

> [!div class="mx-tableFixed"]
|Parameternamn|Standardvärde|intervallet|Beskrivning|
|---|---|---|---|
|binlog_group_commit_sync_delay|1000|0, 11-1000000|Styr hello hur många mikrosekunder binär logg commit väntar innan du synkroniserar hello binär logg filen toodisk.|
|binlog_group_commit_sync_no_delay_count|0|0-1000000|hello maximalt antal transaktioner toowait för innan försöket avbryts hello aktuella fördröjning som anges av binlog-grupp-commit-sync-fördröjning.|
|character_set_server|LATIN1|BIG5, UTF8MB4 osv.|Använd charset_name som hello standardteckenuppsättningen för servern.|
|div_precision_increment|4|0-30|Antalet siffror genom vilka tooincrease hello skala hello resultatet av delning.|
|group_concat_max_len|1024|4-16777216|Tillåtna resultatlängden i byte för hello GROUP_CONCAT().|
|innodb_adaptive_hash_index|ON|PÅ, AV|Om innodb anpassningsbar hash-index är aktiverade eller inaktiverade.|
|innodb_lock_wait_timeout|50|1-3600|hello lång tid i sekunder en InnoDB transaktion väntar på Lås rad innan den ger upp.|
|interactive_timeout|1800|10-1800|Antal sekunder hello server väntar aktivitet på en interaktiv anslutning innan den stängs.|
|log_bin_trust_function_creators|INAKTIVERA|PÅ, AV|Den här variabeln används när binär loggning är aktiverad. Den styr om lagrade funktionen skapare kan vara betrodda inte toocreate lagrade funktioner som gör osäkra händelser toobe skrivs toohello binär logg.|
|log_queries_not_using_indexes|INAKTIVERA|PÅ, AV|Loggar frågor som är förväntat tooretrieve alla rader tooslow frågeloggen.|
|log_slow_admin_statements|INAKTIVERA|PÅ, AV|Inkludera långsam administrativa rapporter i skrivs toohello långsam frågeloggen hello-instruktioner.|
|log_throttle_queries_not_using_indexes|0|0-4294967295|Gränser hello antalet sådana frågor per minut som kan skrivas toohello långsam frågeloggen.|
|long_query_time|10|1-1E + 100|Om en fråga tar längre tid än detta antal sekunder, ökar hello server hello Slow_queries status variabeln.|
|max_allowed_packet|536870912|1024-1073741824|hello maximal storlek för ett paket eller valfri sträng genereras/mellanliggande eller valfri parameter som skickas av hello mysql_stmt_send_long_data() C-API-funktion.|
|min_examined_row_limit|0|0-18446744073709551615|Loggar frågor som har större än hello konfigurerade antalet rader i hello långsam frågeloggen. |
|server_id|3293747068|1000-4294967295|Hej server-ID som används i replikering toogive varje master och slave en unik identitet.|
|slave_net_timeout|60|30-3600|hello antal sekunder toowait för mer data från hello master innan hello slavserver anser hello anslutningen skadad, avbryter hello läsa och försöker tooreconnect.|
|slow_query_log|INAKTIVERA|PÅ, AV|Aktivera eller inaktivera hello långsam frågeloggen.|
|sql_mode|0 markerat|ALLOW_INVALID_DATES, IGNORE_SPACE osv.|hello aktuella SQL serverläge.|
|TIME_ZONE|SYSTEM|Exempel:-8: 00, +05: 30|hello serverns tidszon.|
|wait_timeout|120|60-240|hello antal sekunder hello server väntar aktivitet på en icke-interaktiv anslutning innan den stängs.|

## <a name="next-steps"></a>Nästa steg
- [Anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)
