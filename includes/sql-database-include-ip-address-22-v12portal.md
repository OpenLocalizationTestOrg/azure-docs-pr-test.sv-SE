
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, hello following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through hello new Azure portal
-->


1. <span data-ttu-id="5088c-101">Logga in toohello [Azure-portalen](https://portal.azure.com/) på http://portal.azure.com/.</span><span class="sxs-lookup"><span data-stu-id="5088c-101">Log in toohello [Azure portal](https://portal.azure.com/) at http://portal.azure.com/.</span></span>
2. <span data-ttu-id="5088c-102">I hello vänstra banderoll, klickar du på **Bläddra bland alla**.</span><span class="sxs-lookup"><span data-stu-id="5088c-102">In hello left banner, click **BROWSE ALL**.</span></span> <span data-ttu-id="5088c-103">Hej **Bläddra** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="5088c-103">hello **Browse** blade is displayed.</span></span>
3. <span data-ttu-id="5088c-104">Bläddra och klicka på **SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="5088c-104">Scroll and click **SQL servers**.</span></span> <span data-ttu-id="5088c-105">Hej **SQL-servrar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="5088c-105">hello **SQL servers** blade is displayed.</span></span>
   
    ![Hitta din Azure SQL Database-server i hello-portalen][b21-FindServerInPortal]
4. <span data-ttu-id="5088c-107">För enkelhetens skull klickar du på hello minimera kontrollen i hello tidigare **Bläddra** bladet.</span><span class="sxs-lookup"><span data-stu-id="5088c-107">For convenience, click hello minimize control on hello earlier **Browse** blade.</span></span>
5. <span data-ttu-id="5088c-108">Börja skriva hello namnet på din server hello-filtret i textrutan.</span><span class="sxs-lookup"><span data-stu-id="5088c-108">In hello filter text box, start typing hello name of your server.</span></span> <span data-ttu-id="5088c-109">Raden visas.</span><span class="sxs-lookup"><span data-stu-id="5088c-109">Your row is displayed.</span></span>
6. <span data-ttu-id="5088c-110">Klicka på hello rad för servern.</span><span class="sxs-lookup"><span data-stu-id="5088c-110">Click hello row for your server.</span></span> <span data-ttu-id="5088c-111">Ett blad för servern visas.</span><span class="sxs-lookup"><span data-stu-id="5088c-111">A blade for your server is displayed.</span></span>
7. <span data-ttu-id="5088c-112">Klicka på din server-bladet **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="5088c-112">On your server blade, click **Settings**.</span></span> <span data-ttu-id="5088c-113">Hej **inställningar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="5088c-113">hello **Settings** blade is displayed.</span></span>
8. <span data-ttu-id="5088c-114">Klicka på **brandväggen**.</span><span class="sxs-lookup"><span data-stu-id="5088c-114">Click **Firewall**.</span></span> <span data-ttu-id="5088c-115">Hej **brandväggsinställningar** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="5088c-115">hello **Firewall Settings** blade is displayed.</span></span>
   
    ![Klicka på Inställningar > brandväggen][b31-SettingsFirewallNavig]
9. <span data-ttu-id="5088c-117">Klicka på **Lägg till klient IP**.</span><span class="sxs-lookup"><span data-stu-id="5088c-117">Click **Add Client IP**.</span></span> <span data-ttu-id="5088c-118">Ange ett namn för den nya regeln till hello första textrutan.</span><span class="sxs-lookup"><span data-stu-id="5088c-118">Type in a name for your new rule into hello first text box.</span></span>
10. <span data-ttu-id="5088c-119">Typen i hello låg och hög IP-adressen värden för hello-intervall som du vill tooenable.</span><span class="sxs-lookup"><span data-stu-id="5088c-119">Type in hello low and high IP address values for hello range you want tooenable.</span></span>
    
    * <span data-ttu-id="5088c-120">Det kan vara praktisk toohave hello lågt värde för end med **.0** och hello hög med **.255**.</span><span class="sxs-lookup"><span data-stu-id="5088c-120">It can be handy toohave hello low value end with **.0** and hello high with **.255**.</span></span>
    
    ![Lägg till en IP-adressintervallet tooallow][b41-AddRange]
11. <span data-ttu-id="5088c-122">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5088c-122">Click **Save**.</span></span>

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
