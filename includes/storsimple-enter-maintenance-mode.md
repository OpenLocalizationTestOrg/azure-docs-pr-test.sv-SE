<!--author=SharS last changed: 12/01/15-->

#### <a name="tooenter-maintenance-mode"></a>tooenter underhållsläge
1. I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.
2. Ange hello lösenord. hello standardlösenordet är **Password1**.
3. I hello kommandotolk, skriver du:
   
     `Enter-HcsMaintenanceMode`
4. Visas ett varningsmeddelande som talar om att underhållsläge ska störa alla i/o-begäranden och sever hello anslutning toohello klassiska Azure-portalen och du uppmanas att bekräfta. Typen **Y** tooenter underhållsläge.
   
    Både domänkontrollanter startas om. När hello omstarten är klar, visas ett annat meddelande som anger att hello-enheten är i underhållsläge.

