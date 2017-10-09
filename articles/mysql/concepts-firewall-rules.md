---
title: "aaaAzure databas för MySQL serverbrandväggsreglerna | Microsoft Docs"
description: "Beskriver brandväggsregler för din Azure-databas för MySQL-servern."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 1f85310385da947b6c492aa6aa21c1b885c2a97d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a><span data-ttu-id="dff3d-103">Azure-databas för MySQL serverbrandväggsreglerna</span><span class="sxs-lookup"><span data-stu-id="dff3d-103">Azure Database for MySQL server firewall rules</span></span>
<span data-ttu-id="dff3d-104">Brandväggar förhindrar alla åtkomst tooyour databasserver förrän du anger vilka datorer som har behörighet.</span><span class="sxs-lookup"><span data-stu-id="dff3d-104">Firewalls prevent all access tooyour database server until you specify which computers have permission.</span></span> <span data-ttu-id="dff3d-105">hello brandväggen beviljar åtkomst toohello server baserat på hello kommer IP-adressen för varje begäran.</span><span class="sxs-lookup"><span data-stu-id="dff3d-105">hello firewall grants access toohello server based on hello originating IP address of each request.</span></span>

<span data-ttu-id="dff3d-106">tooconfigure brandväggen, skapa brandväggsregler som anger godkända IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="dff3d-106">tooconfigure your firewall, you create firewall rules that specify ranges of acceptable IP addresses.</span></span> <span data-ttu-id="dff3d-107">Du kan skapa brandväggsregler på servernivå hello.</span><span class="sxs-lookup"><span data-stu-id="dff3d-107">You can create firewall rules at hello server level.</span></span>

<span data-ttu-id="dff3d-108">**Regler i brandväggen:** reglerna aktivera klienter tooaccess Azure hela databasen för MySQL-server, det vill säga hello alla hello databaser inom samma logiska server.</span><span class="sxs-lookup"><span data-stu-id="dff3d-108">**Firewall rules:** These rules enable clients tooaccess your entire Azure Database for MySQL server, that is, all hello databases within hello same logical server.</span></span> <span data-ttu-id="dff3d-109">Brandväggsregler på servernivå kan konfigureras med hjälp av hello Azure-portalen eller med Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="dff3d-109">Server-level firewall rules can be configured by using hello Azure portal or using Azure CLI commands.</span></span> <span data-ttu-id="dff3d-110">toocreate servernivå brandväggsregler, måste du vara hello prenumeration ägare eller deltagare prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dff3d-110">toocreate server-level firewall rules, you must be hello subscription owner or a subscription contributor.</span></span>

## <a name="firewall-overview"></a><span data-ttu-id="dff3d-111">Översikt över brandväggar</span><span class="sxs-lookup"><span data-stu-id="dff3d-111">Firewall overview</span></span>
<span data-ttu-id="dff3d-112">Alla databas åtkomst tooyour Azure-databas för MySQL-servern har blockerats av hello-brandväggen som standard.</span><span class="sxs-lookup"><span data-stu-id="dff3d-112">All database access tooyour Azure Database for MySQL server is blocked by hello firewall by default.</span></span> <span data-ttu-id="dff3d-113">toobegin med servern från en annan dator, behöver du toospecify en eller fler servernivå regler tooenable åtkomst tooyour brandväggsserver.</span><span class="sxs-lookup"><span data-stu-id="dff3d-113">toobegin using your server from another computer, you need toospecify one or more server-level firewall rules tooenable access tooyour server.</span></span> <span data-ttu-id="dff3d-114">Använd hello brandväggen regler toospecify vilka IP-adressintervall från hello Internet tooallow.</span><span class="sxs-lookup"><span data-stu-id="dff3d-114">Use hello firewall rules toospecify which IP address ranges from hello Internet tooallow.</span></span> <span data-ttu-id="dff3d-115">Åtkomst toohello Azure webbplats själva påverkas inte av hello brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="dff3d-115">Access toohello Azure portal website itself is not impacted by hello firewall rules.</span></span>

<span data-ttu-id="dff3d-116">Anslutningsförsök från hello Internet och Azure måste först passera genom brandväggen hello innan de kan komma åt Azure-databasen för MySQL-databas som visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="dff3d-116">Connection attempts from hello Internet and Azure must first pass through hello firewall before they can reach your Azure Database for MySQL database, as shown in hello following diagram:</span></span>

![Exempel flödet av hur hello brandvägg fungerar](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-hello-internet"></a><span data-ttu-id="dff3d-118">Ansluta från hello Internet</span><span class="sxs-lookup"><span data-stu-id="dff3d-118">Connecting from hello Internet</span></span>
<span data-ttu-id="dff3d-119">Brandväggsregler på servernivå tillämpa tooall databaser på hello Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="dff3d-119">Server-level firewall rules apply tooall databases on hello Azure Database for MySQL server.</span></span>

<span data-ttu-id="dff3d-120">Om hello IP-adressen för hello-begäran är inom ett hello-intervall som anges i hello servernivå brandväggsregler, beviljas hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="dff3d-120">If hello IP address of hello request is within one of hello ranges specified in hello server-level firewall rules, hello connection is granted.</span></span>

<span data-ttu-id="dff3d-121">Om hello IP-adressen för begäran om hello inte ligger inom hello-intervall som anges i någon av hello databasnivå eller servernivå brandväggsregler, hello anslutningsbegäran misslyckas.</span><span class="sxs-lookup"><span data-stu-id="dff3d-121">If hello IP address of hello request is not within hello ranges specified in any of hello database-level or server-level firewall rules, hello connection request fails.</span></span>

## <a name="programmatically-managing-firewall-rules"></a><span data-ttu-id="dff3d-122">Hantera brandväggsregler via programmering</span><span class="sxs-lookup"><span data-stu-id="dff3d-122">Programmatically managing firewall rules</span></span>
<span data-ttu-id="dff3d-123">Dessutom hanterade toohello Azure-portalen brandväggen reglerna kan vara genom programmering med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="dff3d-123">In addition toohello Azure portal, firewall rules can be managed programmatically using Azure CLI.</span></span> <span data-ttu-id="dff3d-124">Se även [skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="dff3d-124">See also [Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>

## <a name="troubleshooting-hello-database-firewall"></a><span data-ttu-id="dff3d-125">Felsöka hello database-brandvägg</span><span class="sxs-lookup"><span data-stu-id="dff3d-125">Troubleshooting hello database firewall</span></span>
<span data-ttu-id="dff3d-126">Överväg följande punkter när åtkomst toohello Microsoft Azure-databas för MySQL server-tjänsten inte fungerar som förväntat hello:</span><span class="sxs-lookup"><span data-stu-id="dff3d-126">Consider hello following points when access toohello Microsoft Azure Database for MySQL server service does not behave as you expect:</span></span>

* <span data-ttu-id="dff3d-127">**Lista över tillåtna ändringar toohello inte har börjat gälla ännu:** kan det vara så mycket som en fem-minuters fördröjning för ändras toohello Azure-databas för MySQL Server brandväggen configuration tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="dff3d-127">**Changes toohello allow list have not taken effect yet:** There may be as much as a five-minute delay for changes toohello Azure Database for MySQL Server firewall configuration tootake effect.</span></span>

* <span data-ttu-id="dff3d-128">**hello inloggningen har inte behörighet eller ett felaktigt lösenord användes:** om en inloggning inte har behörighet på hello Azure-databas för MySQL-server eller hello-lösenord som används är felaktig, hello anslutning toohello Azure-databas för MySQL server nekas.</span><span class="sxs-lookup"><span data-stu-id="dff3d-128">**hello login is not authorized or an incorrect password was used:** If a login does not have permissions on hello Azure Database for MySQL server or hello password used is incorrect, hello connection toohello Azure Database for MySQL server is denied.</span></span> <span data-ttu-id="dff3d-129">Skapa en brandväggsinställningen endast ger klienterna en möjlighet tooattempt ansluter tooyour server; varje klient måste ange autentiseringsuppgifter för hello säkerhet som behövs.</span><span class="sxs-lookup"><span data-stu-id="dff3d-129">Creating a firewall setting only provides clients with an opportunity tooattempt connecting tooyour server; each client must provide hello necessary security credentials.</span></span>

* <span data-ttu-id="dff3d-130">**Dynamisk IP-adress:** om du har en Internetanslutning med dynamisk IP-adressering och du har problem med att få hello-brandväggen kan du försöka något av följande lösningar hello:</span><span class="sxs-lookup"><span data-stu-id="dff3d-130">**Dynamic IP address:** If you have an Internet connection with dynamic IP addressing and you are having trouble getting through hello firewall, you could try one of hello following solutions:</span></span>

* <span data-ttu-id="dff3d-131">Be din Internet Service Provider (ISP) för hello IP-adressintervall som tilldelats tooyour klientdatorer att åtkomst hello Azure-databas för MySQL-servern och lägger sedan till hello IP-adressintervall som en brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="dff3d-131">Ask your Internet Service Provider (ISP) for hello IP address range assigned tooyour client computers that access hello Azure Database for MySQL server, and then add hello IP address range as a firewall rule.</span></span>

* <span data-ttu-id="dff3d-132">Hämta statiska IP-adresser i stället för dina klientdatorer och lägger sedan till hello IP-adresser som brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="dff3d-132">Get static IP addressing instead for your client computers, and then add hello IP addresses as firewall rules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dff3d-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dff3d-133">Next steps</span></span>

<span data-ttu-id="dff3d-134">[Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen](./howto-manage-firewall-using-portal.md)
[skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="dff3d-134">[Create and manage Azure Database for MySQL firewall rules using hello Azure portal](./howto-manage-firewall-using-portal.md)
[Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>
