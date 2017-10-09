
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, hello following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through hello new Azure portal
-->


1. Logga in toohello [Azure-portalen](https://portal.azure.com/) på http://portal.azure.com/.
2. I hello vänstra banderoll, klickar du på **Bläddra bland alla**. Hej **Bläddra** bladet visas.
3. Bläddra och klicka på **SQL-servrar**. Hej **SQL-servrar** bladet visas.
   
    ![Hitta din Azure SQL Database-server i hello-portalen][b21-FindServerInPortal]
4. För enkelhetens skull klickar du på hello minimera kontrollen i hello tidigare **Bläddra** bladet.
5. Börja skriva hello namnet på din server hello-filtret i textrutan. Raden visas.
6. Klicka på hello rad för servern. Ett blad för servern visas.
7. Klicka på din server-bladet **inställningar**. Hej **inställningar** bladet visas.
8. Klicka på **brandväggen**. Hej **brandväggsinställningar** bladet visas.
   
    ![Klicka på Inställningar > brandväggen][b31-SettingsFirewallNavig]
9. Klicka på **Lägg till klient IP**. Ange ett namn för den nya regeln till hello första textrutan.
10. Typen i hello låg och hög IP-adressen värden för hello-intervall som du vill tooenable.
    
    * Det kan vara praktisk toohave hello lågt värde för end med **.0** och hello hög med **.255**.
    
    ![Lägg till en IP-adressintervallet tooallow][b41-AddRange]
11. Klicka på **Spara**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
