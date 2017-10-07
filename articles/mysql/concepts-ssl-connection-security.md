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
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a><span data-ttu-id="69f4b-103">SSL-anslutning i Azure för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="69f4b-103">SSL connectivity in Azure Database for MySQL</span></span>
<span data-ttu-id="69f4b-104">Azure-databas för MySQL stöder anslutning av din databas tooclient serverprogram med Secure Sockets Layer (SSL).</span><span class="sxs-lookup"><span data-stu-id="69f4b-104">Azure Database for MySQL supports connecting your database server tooclient applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="69f4b-105">Framtvinga SSL-anslutningar mellan databasservern och ditt klientprogram hjälper dig att skydda mot attacker som ”man hello mitten” genom att kryptera hello dataströmmen mellan hello-servern och ditt program.</span><span class="sxs-lookup"><span data-stu-id="69f4b-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

## <a name="default-settings"></a><span data-ttu-id="69f4b-106">Standardinställningar</span><span class="sxs-lookup"><span data-stu-id="69f4b-106">Default settings</span></span>
<span data-ttu-id="69f4b-107">Som standard bör hello database-tjänsten vara konfigurerade toorequire SSL-anslutningar när du ansluter tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="69f4b-107">By default, hello database service should be configured toorequire SSL connections when connecting tooMySQL.</span></span>  <span data-ttu-id="69f4b-108">Det rekommenderas att undvika att inaktivera hello SSL-alternativet när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="69f4b-108">It is recommended avoid disabling hello SSL option whenever possible.</span></span> 

<span data-ttu-id="69f4b-109">Etablera en ny Azure-databas för MySQL-servern via hello Azure-portalen och CLI är verkställandet av SSL-anslutningar aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="69f4b-109">When provisioning a new Azure Database for MySQL server through hello Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="69f4b-110">På samma sätt kan inkludera anslutningssträngar som redan har definierats i hello ”anslutningssträngar” inställningar under din server i hello Azure-portalen hello krävs parametrar för vanliga språk tooconnect tooyour databasservern med SSL.</span><span class="sxs-lookup"><span data-stu-id="69f4b-110">Likewise, connection strings that are pre-defined in hello "Connection Strings" settings under your server in hello Azure portal include hello required parameters for common languages tooconnect tooyour database server using SSL.</span></span> <span data-ttu-id="69f4b-111">hello SSL parametern varierar beroende på hello koppling, till exempel ”ssl = true” eller ”sslmode = kräver” eller ”sslmode = krävs” eller andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="69f4b-111">hello SSL parameter varies based on hello connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

<span data-ttu-id="69f4b-112">hur tooenable eller inaktivera SSL-anslutning när du utvecklar program, se för toolearn[hur tooconfigure SSL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="69f4b-112">toolearn how tooenable or disable SSL connection when developing application, please refer too[How tooconfigure SSL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="69f4b-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69f4b-113">Next steps</span></span>
[<span data-ttu-id="69f4b-114">Anslutningsbibliotek för Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="69f4b-114">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)
