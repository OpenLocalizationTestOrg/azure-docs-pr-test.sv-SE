---
title: "aaaSSL-anslutning för Azure-databas för MySQL | Microsoft Docs"
description: "Information för att konfigurera Azure-databas för MySQL och associerade program tooproperly använda SSL-anslutningar"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6fca7c88fc0f1fd6058d68fcff90fd409abd97a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-anslutning i Azure för MySQL-databas
Azure-databas för MySQL stöder anslutning av din databas tooclient serverprogram med Secure Sockets Layer (SSL). Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.

## <a name="default-settings"></a>Standardinställningar
Som standard bör hello database-tjänsten vara konfigurerade toorequire SSL-anslutningar när du ansluter tooMySQL.  Det rekommenderas att undvika att inaktivera hello SSL-alternativet när det är möjligt. 

Etablera en ny Azure-databas för MySQL-servern via hello Azure-portalen och CLI är verkställandet av SSL-anslutningar aktiverad som standard. 

På samma sätt kan inkludera anslutningssträngar som redan har definierats i hello ”anslutningssträngar” inställningar under din server i hello Azure-portalen hello krävs parametrar för vanliga språk tooconnect tooyour databasservern med SSL. hello SSL parametern varierar beroende på hello koppling, till exempel ”ssl = true” eller ”sslmode = kräver” eller ”sslmode = krävs” eller andra ändringar.

hur tooenable eller inaktivera SSL-anslutning när du utvecklar program, se för toolearn[hur tooconfigure SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Nästa steg
[Anslutningsbibliotek för Azure-databas för MySQL](concepts-connection-libraries.md)
