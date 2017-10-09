## <a name="os-config"></a>Lägg till IP-adresser tooa VM-operativsystem

Anslut och inloggningen tooa VM som du skapat med flera privata IP-adresser. Du måste manuellt lägga till alla hello privata IP-adresser (inklusive hello primär) som du lagt till toohello VM. Slutför hello följande steg för ditt VM-operativsystem:

### <a name="windows"></a>Windows

1. Från en kommandotolk skriver du *ipconfig /all*.  Du kan bara se hello *primära* privata IP-adressen (via DHCP).
2. Typen *ncpa.cpl* i hello kommandotolk tooopen hello **nätverksanslutningar** fönster.
3. Öppna hello egenskaper för hello rätt nätverkskort: **lokal anslutning**.
4. Dubbelklicka på Internet Protocol version 4 (IPv4).
5. Välj **Använd hello följande IP-adress** och ange hello följande värden:

    * **IP-adress**: Ange hello *primära* privat IP-adress
    * **Nätmask**: Ställ in beroende på ditt undernät. Till exempel om hello undernät är ett/24 undernät och undernät hello masken är 255.255.255.0.
    * **Standard-gateway**: hello första IP-adressen i hello undernät. Om ditt undernät 10.0.0.0/24 är hello gateway IP-adressen 10.0.0.1.
    * Klicka på **Använd hello följande DNS-serveradresser** och ange hello följande värden:
        * **Önskad DNS-server**: Ange 168.63.129.16 om du inte använder en egen DNS-server.  Om du använder DNS-servern, ange hello IP-adress för servern.
    * Klicka på hello **Avancerat** knappen och lägga till ytterligare IP-adresser. Lägg till varje hello sekundära privata IP-adresser som anges i steg 8 toohello NIC med hello samma undernät som har angetts för hello primära IP-adress.
        >[!WARNING] 
        >Om du inte följer hello stegen ovan på rätt sätt, så kan du tappa anslutningen tooyour VM. Kontrollera hello information som anges i steg 5 är korrekt innan du fortsätter.

    * Klicka på **OK** tooclose ut hello TCP/IP-inställningar och sedan **OK** igen tooclose hello inställningarna för nätverkskortet. RDP-anslutningen återupprättats.

6. Från en kommandotolk skriver du *ipconfig /all*. Alla IP-adresser som du lade till visas och DHCP är avstängt.


### <a name="validation-windows"></a>Validering (Windows)

tooensure är kan tooconnect toohello internet från din sekundära IP-konfiguration via hello offentlig IP-adress som är associerade, när du har lagt till den på rätt sätt med hjälp av stegen ovan, hello följande kommando:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>För sekundära IP-konfigurationer pinga du bara toohello Internet om hello konfiguration har en offentlig IP-adress som är kopplade till den. En offentlig IP-adress är inte obligatoriska tooping toohello Internet för primära IP-konfigurationer.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Öppna ett terminalfönster.
2. Kontrollera att du är hello rotanvändaren. Om du inte Skriv hello följande kommando:

    ```bash
    sudo -i
    ```

3. Uppdatera hello konfigurationsfilen för hello gränssnitt (förutsatt att 'eth0').

    * Behåll hello befintliga radobjekt för dhcp. hello primära IP-adress är konfigurerade som tidigare.
    * Lägga till en konfiguration för ytterligare en statisk IP-adress med hello följande kommandon:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    Du bör se en .cfg-fil.
4. Öppna hello-fil. Du bör se följande rader hello slutet av filen hello hello:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Lägg till följande rader efter hello rader som finns i den här filen hello:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. Spara filen med hello med hjälp av hello följande kommando:

    ```bash
    :wq
    ```

7. Återställ hello nätverksgränssnitt med hello följande kommando:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Kör både ifdown och ifup i hello samma rad om du använder en fjärranslutning.
    >

8. Kontrollera hello IP-adress läggs toohello nätverksgränssnitt med hello följande kommando:

    ```bash
    ip addr list eth0
    ```

    Du bör se hello IP-adress som du lade till som en del av hello-listan.

### <a name="linux-redhat-centos-and-others"></a>Linux (Redhat, CentOS och andra)

1. Öppna ett terminalfönster.
2. Kontrollera att du är hello rotanvändaren. Om du inte Skriv hello följande kommando:

    ```bash
    sudo -i
    ```

3. Ange ditt lösenord och följ instruktionerna som efterfrågas. När du är hello rotanvändaren navigera toohello nätverksmapp skript med hello följande kommando:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Lista hello relaterade ifcfg filer med hjälp av hello följande kommando:

    ```bash
    ls ifcfg-*
    ```

    Du bör se *ifcfg eth0* som en hello-filer.

5. tooadd en IP-adress, skapa en konfigurationsfil för den som visas nedan. Observera att en fil måste skapas för varje IP-konfiguration.

    ```bash
    touch ifcfg-eth0:0
    ```

6. Öppna hello *ifcfg-eth0:0* fil med hello följande kommando:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Lägga till innehåll toohello fil *eth0:0* i det här fallet med hello följande kommando. Säker på att tooupdate information vara baserat på din IP-adress.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Spara filen med hello med hello följande kommando:

    ```bash
    :wq
    ```

9. Starta om hello nätverkstjänster och kontrollera hello ändras lyckades genom att köra följande kommandon hello:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Du bör se hello IP-adress som du lade till, *eth0:0*, i hello listan som returneras.

### <a name="validation-linux"></a>Validering (Linux)

du är kan tooconnect toohello tooensure internet från din sekundära IP-konfiguration via hello offentlig IP associerat den använder hello följande kommando:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>För sekundära IP-konfigurationer pinga du bara toohello Internet om hello konfiguration har en offentlig IP-adress som är kopplade till den. En offentlig IP-adress är inte obligatoriska tooping toohello Internet för primära IP-konfigurationer.

Du kanske måste tooadd lämpliga vägar för Linux virtuella datorer, vid toovalidate utgående anslutning från ett sekundärt nätverkskort. Det finns många sätt toodo detta. Se motsvarande dokumentation för din distribution av Linux. hello följande är en metod tooaccomplish detta:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Vara säker på att tooreplace:
    - **10.0.0.5** med hello privata IP-adress som har en offentlig IP-adress kopplad tooit
    - **10.0.0.1** tooyour standard-gateway
    - **eth2** toohello namnet på nätverkskortet sekundär
