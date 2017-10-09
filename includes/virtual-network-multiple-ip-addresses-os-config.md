## <span data-ttu-id="75be8-101"><a name="os-config"></a>Lägg till IP-adresser tooa VM-operativsystem</span><span class="sxs-lookup"><span data-stu-id="75be8-101"><a name="os-config"></a>Add IP addresses tooa VM operating system</span></span>

<span data-ttu-id="75be8-102">Anslut och inloggningen tooa VM som du skapat med flera privata IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="75be8-102">Connect and login tooa VM you created with multiple private IP addresses.</span></span> <span data-ttu-id="75be8-103">Du måste manuellt lägga till alla hello privata IP-adresser (inklusive hello primär) som du lagt till toohello VM.</span><span class="sxs-lookup"><span data-stu-id="75be8-103">You must manually add all hello private IP addresses (including hello primary) that you added toohello VM.</span></span> <span data-ttu-id="75be8-104">Slutför hello följande steg för ditt VM-operativsystem:</span><span class="sxs-lookup"><span data-stu-id="75be8-104">Complete hello following steps for your VM operating system:</span></span>

### <a name="windows"></a><span data-ttu-id="75be8-105">Windows</span><span class="sxs-lookup"><span data-stu-id="75be8-105">Windows</span></span>

1. <span data-ttu-id="75be8-106">Från en kommandotolk skriver du *ipconfig /all*.</span><span class="sxs-lookup"><span data-stu-id="75be8-106">From a command prompt, type *ipconfig /all*.</span></span>  <span data-ttu-id="75be8-107">Du kan bara se hello *primära* privata IP-adressen (via DHCP).</span><span class="sxs-lookup"><span data-stu-id="75be8-107">You only see hello *Primary* private IP address (through DHCP).</span></span>
2. <span data-ttu-id="75be8-108">Typen *ncpa.cpl* i hello kommandotolk tooopen hello **nätverksanslutningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="75be8-108">Type *ncpa.cpl* in hello command prompt tooopen hello **Network connections** window.</span></span>
3. <span data-ttu-id="75be8-109">Öppna hello egenskaper för hello rätt nätverkskort: **lokal anslutning**.</span><span class="sxs-lookup"><span data-stu-id="75be8-109">Open hello properties for hello appropriate adapter: **Local Area Connection**.</span></span>
4. <span data-ttu-id="75be8-110">Dubbelklicka på Internet Protocol version 4 (IPv4).</span><span class="sxs-lookup"><span data-stu-id="75be8-110">Double-click Internet Protocol version 4 (IPv4).</span></span>
5. <span data-ttu-id="75be8-111">Välj **Använd hello följande IP-adress** och ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="75be8-111">Select **Use hello following IP address** and enter hello following values:</span></span>

    * <span data-ttu-id="75be8-112">**IP-adress**: Ange hello *primära* privat IP-adress</span><span class="sxs-lookup"><span data-stu-id="75be8-112">**IP address**: Enter hello *Primary* private IP address</span></span>
    * <span data-ttu-id="75be8-113">**Nätmask**: Ställ in beroende på ditt undernät.</span><span class="sxs-lookup"><span data-stu-id="75be8-113">**Subnet mask**: Set based on your subnet.</span></span> <span data-ttu-id="75be8-114">Till exempel om hello undernät är ett/24 undernät och undernät hello masken är 255.255.255.0.</span><span class="sxs-lookup"><span data-stu-id="75be8-114">For example, if hello subnet is a /24 subnet then hello subnet mask is 255.255.255.0.</span></span>
    * <span data-ttu-id="75be8-115">**Standard-gateway**: hello första IP-adressen i hello undernät.</span><span class="sxs-lookup"><span data-stu-id="75be8-115">**Default gateway**: hello first IP address in hello subnet.</span></span> <span data-ttu-id="75be8-116">Om ditt undernät 10.0.0.0/24 är hello gateway IP-adressen 10.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="75be8-116">If your subnet is 10.0.0.0/24, then hello gateway IP address is 10.0.0.1.</span></span>
    * <span data-ttu-id="75be8-117">Klicka på **Använd hello följande DNS-serveradresser** och ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="75be8-117">Click **Use hello following DNS server addresses** and enter hello following values:</span></span>
        * <span data-ttu-id="75be8-118">**Önskad DNS-server**: Ange 168.63.129.16 om du inte använder en egen DNS-server.</span><span class="sxs-lookup"><span data-stu-id="75be8-118">**Preferred DNS server**: If you are not using your own DNS server, enter 168.63.129.16.</span></span>  <span data-ttu-id="75be8-119">Om du använder DNS-servern, ange hello IP-adress för servern.</span><span class="sxs-lookup"><span data-stu-id="75be8-119">If you are using your own DNS server, enter hello IP address for your server.</span></span>
    * <span data-ttu-id="75be8-120">Klicka på hello **Avancerat** knappen och lägga till ytterligare IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="75be8-120">Click hello **Advanced** button and add additional IP addresses.</span></span> <span data-ttu-id="75be8-121">Lägg till varje hello sekundära privata IP-adresser som anges i steg 8 toohello NIC med hello samma undernät som har angetts för hello primära IP-adress.</span><span class="sxs-lookup"><span data-stu-id="75be8-121">Add each of hello secondary private IP addresses listed in step 8 toohello NIC with hello same subnet specified for hello primary IP address.</span></span>
        >[!WARNING] 
        ><span data-ttu-id="75be8-122">Om du inte följer hello stegen ovan på rätt sätt, så kan du tappa anslutningen tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="75be8-122">If you do not follow hello steps above correctly, you may lose connectivity tooyour VM.</span></span> <span data-ttu-id="75be8-123">Kontrollera hello information som anges i steg 5 är korrekt innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="75be8-123">Ensure hello information entered for step 5 is accurate before proceeding.</span></span>

    * <span data-ttu-id="75be8-124">Klicka på **OK** tooclose ut hello TCP/IP-inställningar och sedan **OK** igen tooclose hello inställningarna för nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="75be8-124">Click **OK** tooclose out hello TCP/IP settings and then **OK** again tooclose hello adapter settings.</span></span> <span data-ttu-id="75be8-125">RDP-anslutningen återupprättats.</span><span class="sxs-lookup"><span data-stu-id="75be8-125">Your RDP connection is re-established.</span></span>

6. <span data-ttu-id="75be8-126">Från en kommandotolk skriver du *ipconfig /all*.</span><span class="sxs-lookup"><span data-stu-id="75be8-126">From a command prompt, type *ipconfig /all*.</span></span> <span data-ttu-id="75be8-127">Alla IP-adresser som du lade till visas och DHCP är avstängt.</span><span class="sxs-lookup"><span data-stu-id="75be8-127">All IP addresses you added are shown and DHCP is turned off.</span></span>


### <a name="validation-windows"></a><span data-ttu-id="75be8-128">Validering (Windows)</span><span class="sxs-lookup"><span data-stu-id="75be8-128">Validation (Windows)</span></span>

<span data-ttu-id="75be8-129">tooensure är kan tooconnect toohello internet från din sekundära IP-konfiguration via hello offentlig IP-adress som är associerade, när du har lagt till den på rätt sätt med hjälp av stegen ovan, hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-129">tooensure you are able tooconnect toohello internet from your secondary IP configuration via hello public IP associated it, once you have added it correctly using steps above, use hello following command:</span></span>

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
><span data-ttu-id="75be8-130">För sekundära IP-konfigurationer pinga du bara toohello Internet om hello konfiguration har en offentlig IP-adress som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="75be8-130">For secondary IP configurations, you can only ping toohello Internet if hello configuration has a public IP address associated with it.</span></span> <span data-ttu-id="75be8-131">En offentlig IP-adress är inte obligatoriska tooping toohello Internet för primära IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="75be8-131">For primary IP configurations, a public IP address is not required tooping toohello Internet.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="75be8-132">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="75be8-132">Linux (Ubuntu)</span></span>

1. <span data-ttu-id="75be8-133">Öppna ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="75be8-133">Open a terminal window.</span></span>
2. <span data-ttu-id="75be8-134">Kontrollera att du är hello rotanvändaren.</span><span class="sxs-lookup"><span data-stu-id="75be8-134">Make sure you are hello root user.</span></span> <span data-ttu-id="75be8-135">Om du inte Skriv hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-135">If you are not, enter hello following command:</span></span>

    ```bash
    sudo -i
    ```

3. <span data-ttu-id="75be8-136">Uppdatera hello konfigurationsfilen för hello gränssnitt (förutsatt att 'eth0').</span><span class="sxs-lookup"><span data-stu-id="75be8-136">Update hello configuration file of hello network interface (assuming ‘eth0’).</span></span>

    * <span data-ttu-id="75be8-137">Behåll hello befintliga radobjekt för dhcp.</span><span class="sxs-lookup"><span data-stu-id="75be8-137">Keep hello existing line item for dhcp.</span></span> <span data-ttu-id="75be8-138">hello primära IP-adress är konfigurerade som tidigare.</span><span class="sxs-lookup"><span data-stu-id="75be8-138">hello primary IP address remains configured as it was previously.</span></span>
    * <span data-ttu-id="75be8-139">Lägga till en konfiguration för ytterligare en statisk IP-adress med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="75be8-139">Add a configuration for an additional static IP address with hello following commands:</span></span>

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    <span data-ttu-id="75be8-140">Du bör se en .cfg-fil.</span><span class="sxs-lookup"><span data-stu-id="75be8-140">You should see a .cfg file.</span></span>
4. <span data-ttu-id="75be8-141">Öppna hello-fil.</span><span class="sxs-lookup"><span data-stu-id="75be8-141">Open hello file.</span></span> <span data-ttu-id="75be8-142">Du bör se följande rader hello slutet av filen hello hello:</span><span class="sxs-lookup"><span data-stu-id="75be8-142">You should see hello following lines at hello end of hello file:</span></span>

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. <span data-ttu-id="75be8-143">Lägg till följande rader efter hello rader som finns i den här filen hello:</span><span class="sxs-lookup"><span data-stu-id="75be8-143">Add hello following lines after hello lines that exist in this file:</span></span>

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. <span data-ttu-id="75be8-144">Spara filen med hello med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-144">Save hello file by using hello following command:</span></span>

    ```bash
    :wq
    ```

7. <span data-ttu-id="75be8-145">Återställ hello nätverksgränssnitt med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-145">Reset hello network interface with hello following command:</span></span>

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="75be8-146">Kör både ifdown och ifup i hello samma rad om du använder en fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="75be8-146">Run both ifdown and ifup in hello same line if using a remote connection.</span></span>
    >

8. <span data-ttu-id="75be8-147">Kontrollera hello IP-adress läggs toohello nätverksgränssnitt med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-147">Verify hello IP address is added toohello network interface with hello following command:</span></span>

    ```bash
    ip addr list eth0
    ```

    <span data-ttu-id="75be8-148">Du bör se hello IP-adress som du lade till som en del av hello-listan.</span><span class="sxs-lookup"><span data-stu-id="75be8-148">You should see hello IP address you added as part of hello list.</span></span>

### <a name="linux-redhat-centos-and-others"></a><span data-ttu-id="75be8-149">Linux (Redhat, CentOS och andra)</span><span class="sxs-lookup"><span data-stu-id="75be8-149">Linux (Redhat, CentOS, and others)</span></span>

1. <span data-ttu-id="75be8-150">Öppna ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="75be8-150">Open a terminal window.</span></span>
2. <span data-ttu-id="75be8-151">Kontrollera att du är hello rotanvändaren.</span><span class="sxs-lookup"><span data-stu-id="75be8-151">Make sure you are hello root user.</span></span> <span data-ttu-id="75be8-152">Om du inte Skriv hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-152">If you are not, enter hello following command:</span></span>

    ```bash
    sudo -i
    ```

3. <span data-ttu-id="75be8-153">Ange ditt lösenord och följ instruktionerna som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="75be8-153">Enter your password and follow instructions as prompted.</span></span> <span data-ttu-id="75be8-154">När du är hello rotanvändaren navigera toohello nätverksmapp skript med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-154">Once you are hello root user, navigate toohello network scripts folder with hello following command:</span></span>

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. <span data-ttu-id="75be8-155">Lista hello relaterade ifcfg filer med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-155">List hello related ifcfg files using hello following command:</span></span>

    ```bash
    ls ifcfg-*
    ```

    <span data-ttu-id="75be8-156">Du bör se *ifcfg eth0* som en hello-filer.</span><span class="sxs-lookup"><span data-stu-id="75be8-156">You should see *ifcfg-eth0* as one of hello files.</span></span>

5. <span data-ttu-id="75be8-157">tooadd en IP-adress, skapa en konfigurationsfil för den som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="75be8-157">tooadd an IP address, create a configuration file for it as shown below.</span></span> <span data-ttu-id="75be8-158">Observera att en fil måste skapas för varje IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="75be8-158">Note that one file must be created for each IP configuration.</span></span>

    ```bash
    touch ifcfg-eth0:0
    ```

6. <span data-ttu-id="75be8-159">Öppna hello *ifcfg-eth0:0* fil med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-159">Open hello *ifcfg-eth0:0* file with hello following command:</span></span>

    ```bash
    vi ifcfg-eth0:0
    ```

7. <span data-ttu-id="75be8-160">Lägga till innehåll toohello fil *eth0:0* i det här fallet med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="75be8-160">Add content toohello file, *eth0:0* in this case, with hello following command.</span></span> <span data-ttu-id="75be8-161">Säker på att tooupdate information vara baserat på din IP-adress.</span><span class="sxs-lookup"><span data-stu-id="75be8-161">Be sure tooupdate information based on your IP address.</span></span>

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. <span data-ttu-id="75be8-162">Spara filen med hello med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-162">Save hello file with hello following command:</span></span>

    ```bash
    :wq
    ```

9. <span data-ttu-id="75be8-163">Starta om hello nätverkstjänster och kontrollera hello ändras lyckades genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="75be8-163">Restart hello network services and make sure hello changes are successful by running hello following commands:</span></span>

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    <span data-ttu-id="75be8-164">Du bör se hello IP-adress som du lade till, *eth0:0*, i hello listan som returneras.</span><span class="sxs-lookup"><span data-stu-id="75be8-164">You should see hello IP address you added, *eth0:0*, in hello list returned.</span></span>

### <a name="validation-linux"></a><span data-ttu-id="75be8-165">Validering (Linux)</span><span class="sxs-lookup"><span data-stu-id="75be8-165">Validation (Linux)</span></span>

<span data-ttu-id="75be8-166">du är kan tooconnect toohello tooensure internet från din sekundära IP-konfiguration via hello offentlig IP associerat den använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="75be8-166">tooensure you are able tooconnect toohello internet from your secondary IP configuration via hello public IP associated it, use hello following command:</span></span>

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
><span data-ttu-id="75be8-167">För sekundära IP-konfigurationer pinga du bara toohello Internet om hello konfiguration har en offentlig IP-adress som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="75be8-167">For secondary IP configurations, you can only ping toohello Internet if hello configuration has a public IP address associated with it.</span></span> <span data-ttu-id="75be8-168">En offentlig IP-adress är inte obligatoriska tooping toohello Internet för primära IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="75be8-168">For primary IP configurations, a public IP address is not required tooping toohello Internet.</span></span>

<span data-ttu-id="75be8-169">Du kanske måste tooadd lämpliga vägar för Linux virtuella datorer, vid toovalidate utgående anslutning från ett sekundärt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="75be8-169">For Linux VMs, when trying toovalidate outbound connectivity from a secondary NIC, you may need tooadd appropriate routes.</span></span> <span data-ttu-id="75be8-170">Det finns många sätt toodo detta.</span><span class="sxs-lookup"><span data-stu-id="75be8-170">There are many ways toodo this.</span></span> <span data-ttu-id="75be8-171">Se motsvarande dokumentation för din distribution av Linux.</span><span class="sxs-lookup"><span data-stu-id="75be8-171">Please see appropriate documentation for your Linux distribution.</span></span> <span data-ttu-id="75be8-172">hello följande är en metod tooaccomplish detta:</span><span class="sxs-lookup"><span data-stu-id="75be8-172">hello following is one method tooaccomplish this:</span></span>

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- <span data-ttu-id="75be8-173">Vara säker på att tooreplace:</span><span class="sxs-lookup"><span data-stu-id="75be8-173">Be sure tooreplace:</span></span>
    - <span data-ttu-id="75be8-174">**10.0.0.5** med hello privata IP-adress som har en offentlig IP-adress kopplad tooit</span><span class="sxs-lookup"><span data-stu-id="75be8-174">**10.0.0.5** with hello private IP address that has a public IP address associated tooit</span></span>
    - <span data-ttu-id="75be8-175">**10.0.0.1** tooyour standard-gateway</span><span class="sxs-lookup"><span data-stu-id="75be8-175">**10.0.0.1** tooyour default gateway</span></span>
    - <span data-ttu-id="75be8-176">**eth2** toohello namnet på nätverkskortet sekundär</span><span class="sxs-lookup"><span data-stu-id="75be8-176">**eth2** toohello name of your secondary NIC</span></span>
