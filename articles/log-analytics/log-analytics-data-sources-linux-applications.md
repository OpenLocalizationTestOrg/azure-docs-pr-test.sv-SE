---
title: aaaCollect Linux programprestanda i OMS Log Analytics | Microsoft Docs
description: "Den här artikeln innehåller information för att konfigurera hello OMS-Agent för Linux toocollect prestandaräknare för MySQL och Apache HTTP-servern."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 51105c6add5c7941a004570a76a4d94c02fc8a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a>Samla in prestandaräknare för Linux-program i logganalys 
Den här artikeln innehåller information om hur du konfigurerar hello [OMS-Agent för Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) toocollect prestandaräknare för specifika program.  hello-program som ingår i den här artikeln är:  

- [MySQL](#MySQL)
- [Apache HTTP-Server](#apache-http-server)

## <a name="mysql"></a>MySQL
Om MySQL-Server eller MariaDB Server identifieras på hello dator när hello OMS-agenten är installerad, installeras en provider för MySQL-Server för prestandaövervakning automatiskt. Den här providern ansluter toohello lokala MySQL/MariaDB server tooexpose prestandastatistik. MySQL-autentiseringsuppgifter måste konfigureras så att hello providern kan komma åt hello MySQL-servern.

### <a name="configure-mysql-credentials"></a>Konfigurera MySQL-autentiseringsuppgifter
hello MySQL OMI-providern kräver att en förkonfigurerad MySQL-användare och installerat MySQL klientbibliotek i ordning tooquery hello prestanda och hälsoinformation från hello MySQL-instans.  Dessa autentiseringsuppgifter lagras i en fil för autentisering som är lagrad på hello Linux-agenten.  Hej autentiseringsfilen anger vilka bind-adress och port hello MySQL instans lyssnar på och vilka autentiseringsuppgifter toouse toogather mått.  

Under installationen av hello OMS-Agent för Linux hello MySQL OMI genomsöks providern MySQL my.cnf configuration-filer (standardplatserna) för bind-adress och port och delvis set hello MySQL OMI autentiseringsfilen.

hello MySQL autentisering lagras på `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Filformatet för autentisering
Följande är hello format för hello MySQL OMI autentiseringsfilen

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

hello poster i hello autentiseringsfilen beskrivs i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--|:--|
| Port | Representerar hello aktuella port hello MySQL instans lyssnar på. Port 0 anger att hello egenskaper följande används för standardinstansen. |
| Bind-adress| Aktuell MySQL bind-adress. |
| användarnamn| MySQL-användare används toouse toomonitor hello MySQL server-instansen. |
| Base64-kodat lösenord| Lösenordet för hello MySQL övervakning användare kodad i Base64. |
| Automatisk uppdatering| Anger om toorescan för ändringar i hello my.cnf fil och skriva över hello MySQL OMI autentiseringsfilen när hello MySQL OMI providern uppgraderas. |

### <a name="default-instance"></a>Standardinstansen
hello MySQL OMI autentiseringsfilen kan definiera en standardinstans och port number toomake hantera flera MySQL-instanser på en Linux-värd enklare.  hello standardinstansen markeras med en instans med port 0. Alla ytterligare instanser ärver egenskaper som anges från hello standardinstansen om de ange olika värden. Till exempel om MySQL-instans som lyssnar på port '3308' läggs kommer hello standardinstans bind-adress, användarnamn och lösenord för Base64-kodade att använda tootry och övervaka hello-instans som lyssnar på 3308. Om hello-instans på 3308 är bundna tooanother adress och använder hello samma MySQL-användarnamn och lösenord par endast hello bind-adress krävs hello andra egenskaper ärvs.

hello i den följande tabellen har exempel instans inställningar 

| Beskrivning | Fil |
|:--|:--|
| Standardinstansen och instansen med port 3308. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Standardinstansen och instansen med port 3308 och annat användarnamn och lösenord. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>Programmet för MySQL OMI autentisering fil
Providern är ingår i hello installation av hello MySQL OMI ett program av MySQL OMI autentisering-fil som kan vara används tooedit hello MySQL OMI autentiseringsfilen. hello autentisering filen programmet finns på följande plats hello.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> hello autentiseringsuppgifter fil måste läsas av hello omsagent konto. Du bör köra hello mycimprovauth kommando som omsgent.

hello innehåller följande tabell information om hello syntax för att använda mycimprovauth.

| Åtgärd | Exempel | Beskrivning
|:--|:--|:--|
| automatisk uppdatering * false\|true * | mycimprovauth automatisk uppdatering false | Anger huruvida hello autentiseringsfilen uppdateras automatiskt på Starta om eller uppdatera. |
| standard *användarlösenordet för bind-adress* | mycimprovauth standard 127.0.0.1 rot pwd | Anger hello standardinstansen i hello MySQL OMI autentiseringsfilen.<br>hello lösenordsfältet ska anges i klartext - hello lösenordet i hello MySQL OMI autentiseringsfilen blir Base64-kodade. |
| ta bort * default\|portnummer * | mycimprovauth 3308 | Tar bort angivna hello-instansen som antingen standard eller av portnummer. |
| Hjälp | mycimprov hjälp | Visar en lista med kommandon toouse. |
| Skriv ut | mycimprov utskrift | Visar ett enkelt tooread MySQL OMI autentiseringsfilen. |
| Uppdatera portnummer *användarlösenordet för bind-adress* | mycimprov uppdatering 3307 127.0.0.1 rot pwd | Uppdaterar hello angivna instansen eller lägger till hello instans om det inte finns. |

hello definierar följande exempelkommandon ett standard-användarkonto för hello MySQL-server på localhost.  hello lösenordsfältet ska anges i klartext - hello lösenordet i hello MySQL OMI autentiseringsfilen kommer att Base64-kodade

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>Databasbehörigheter som krävs för MySQL-prestandaräknare
hello MySQL användare kräver åtkomst toohello följande frågor toocollect MySQL prestandainformation. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


hello MySQL-kräver också väljer åtkomst toohello följande standardtabeller.

- INFORMATION_SCHEMA
- MySQL. 

Dessa behörigheter kan tilldelas genom att köra hello följande grant-kommandon.

    GRANT SELECT ON information_schema.* too‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* too‘monuser’@’localhost’;


> [!NOTE]
> toogrant behörigheter tooa MySQL övervakning användaren hello bevilja användaren måste ha hello alternativet för BEVILJA behörighet samt hello behörighet beviljas.

### <a name="define-performance-counters"></a>Definiera prestandaräknare

När du konfigurerar hello OMS-Agent för Linux toosend data tooLog Analytics, måste du konfigurera hello prestandaräknare toocollect.  Använd hello procedur i [Windows och Linux prestanda datakällor i logganalys](log-analytics-data-sources-windows-events.md) med hello räknare i hello i den följande tabellen.

| Objektnamn | Räknarens namn |
|:--|:--|
| MySQL-databas | Ledigt diskutrymme i byte |
| MySQL-databas | Tabeller |
| MySQL-Server | Avbrutna anslutning Pct |
| MySQL-Server | Anslutningen används Pct |
| MySQL-Server | Användning av diskutrymme i byte |
| MySQL-Server | Fullständiga genomsökning Pct |
| MySQL-Server | InnoDB buffertpool träffar Pct |
| MySQL-Server | InnoDB Pool Använd Buffertprocent |
| MySQL-Server | InnoDB Pool Använd Buffertprocent |
| MySQL-Server | Viktiga träffar Pct |
| MySQL-Server | Viktiga Cache används Pct |
| MySQL-Server | Viktiga Cache skrivåtgärder Pct |
| MySQL-Server | Frågan Cache träffar Pct |
| MySQL-Server | Frågan Cache Prunes Pct |
| MySQL-Server | Frågan Cache används Pct |
| MySQL-Server | Tabell träffar Pct |
| MySQL-Server | Tabell Cache används Pct |
| MySQL-Server | Tabell Lås konkurrens Pct |

## <a name="apache-http-server"></a>Apache HTTP-Server 
Om Apache HTTP-Server har identifierats på datorn hello när hello omsagent paket installeras, installeras en prestandaövervakning provider för Apache HTTP-servern automatiskt. Den här providern förlitar sig på en Apache-modulen måste läsas in i hello Apache HTTP-Server i ordning tooaccess prestandadata. hello modul kan läsas med hello följande kommando:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

toounload hello Apache övervakningsmodulen, kör hello följande kommando:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Definiera prestandaräknare

När du konfigurerar hello OMS-Agent för Linux toosend data tooLog Analytics, måste du konfigurera hello prestandaräknare toocollect.  Använd hello procedur i [Windows och Linux prestanda datakällor i logganalys](log-analytics-data-sources-windows-events.md) med hello räknare i hello i den följande tabellen.

| Objektnamn | Räknarens namn |
|:--|:--|
| Apache HTTP-Server | Upptagen arbetare |
| Apache HTTP-Server | Inaktiv arbetare |
| Apache HTTP-Server | PCT upptagen arbetare |
| Apache HTTP-Server | Totalt antal Pct CPU |
| Apache virtuell värddator | Fel per minut - klient |
| Apache virtuell värddator | Fel per minut - Server |
| Apache virtuell värddator | KB per begäran |
| Apache virtuell värddator | Begäranden KB per sekund |
| Apache virtuell värddator | Begäranden per sekund |



## <a name="next-steps"></a>Nästa steg
* [Samla in prestandaräknare](log-analytics-data-sources-performance-counters.md) från Linux-agenter.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar. 
