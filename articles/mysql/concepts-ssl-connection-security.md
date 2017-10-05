---
title: "SSL-anslutning för Azure-databas för MySQL | Microsoft Docs"
description: "Information för att konfigurera Azure-databas för MySQL och associerade program ska använda SSL-anslutningar"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a><span data-ttu-id="860a0-103">SSL-anslutning i Azure för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="860a0-103">SSL connectivity in Azure Database for MySQL</span></span>
<span data-ttu-id="860a0-104">Azure-databas för MySQL stöder anslutning databasservern till klientprogram som använder Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="860a0-104">Azure Database for MySQL supports connecting your database server to client applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="860a0-105">Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man i mitten” genom att kryptera dataströmmen mellan servern och ditt program.</span><span class="sxs-lookup"><span data-stu-id="860a0-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

## <a name="default-settings"></a><span data-ttu-id="860a0-106">Standardinställningar</span><span class="sxs-lookup"><span data-stu-id="860a0-106">Default settings</span></span>
<span data-ttu-id="860a0-107">Database-tjänsten ska konfigureras för att kräva SSL-anslutningar när du ansluter till MySQL som standard.</span><span class="sxs-lookup"><span data-stu-id="860a0-107">By default, the database service should be configured to require SSL connections when connecting to MySQL.</span></span>  <span data-ttu-id="860a0-108">Det rekommenderas att undvika att inaktivera SSL-alternativet när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="860a0-108">It is recommended avoid disabling the SSL option whenever possible.</span></span> 

<span data-ttu-id="860a0-109">Etablera en ny Azure-databas för MySQL-servern via Azure portal och CLI är verkställandet av SSL-anslutningar aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="860a0-109">When provisioning a new Azure Database for MySQL server through the Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="860a0-110">På samma sätt innehåller anslutningssträngar som har fördefinierats i inställningarna för ”anslutningssträngar” under din server i Azure portal de obligatoriska parametrarna för vanliga språk att ansluta till databasservern med SSL.</span><span class="sxs-lookup"><span data-stu-id="860a0-110">Likewise, connection strings that are pre-defined in the "Connection Strings" settings under your server in the Azure portal include the required parameters for common languages to connect to your database server using SSL.</span></span> <span data-ttu-id="860a0-111">Parametern SSL varierar beroende på anslutningen, till exempel ”ssl = true” eller ”sslmode = kräver” eller ”sslmode = krävs” eller andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="860a0-111">The SSL parameter varies based on the connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

<span data-ttu-id="860a0-112">Om du vill lära dig mer om att aktivera eller inaktivera SSL-anslutning när du utvecklar program, se [hur du konfigurerar SSL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="860a0-112">To learn how to enable or disable SSL connection when developing application, please refer to [How to configure SSL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="860a0-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="860a0-113">Next steps</span></span>
[<span data-ttu-id="860a0-114">Anslutningsbibliotek för Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="860a0-114">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)
