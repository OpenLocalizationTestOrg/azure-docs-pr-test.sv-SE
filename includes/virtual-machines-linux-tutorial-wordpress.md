## <a name="install-wordpress"></a>Installera WordPress

Om du vill tootry traven, installerar du en exempelapp. Exempelvis installera hello följande hello öppen källkod [WordPress](https://wordpress.org/) plattform toocreate webbplatser och bloggar. Andra arbetsbelastningar tootry inkluderar [Drupal](http://www.drupal.org) och [Moodle](https://moodle.org/). 

Den här WordPress-installationen är för konceptbeviset. Mer information och inställningar för produktion installation finns hello [WordPress dokumentationen](https://codex.wordpress.org/Main_Page). 



### <a name="install-hello-wordpress-package"></a>Installera hello WordPress-paketet

Kör följande kommando hello:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>Konfigurera WordPress

Konfigurera WordPress toouse MySQL och PHP. Kör följande kommando tooopen hello en textredigerare önskat och skapa hello fil `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Kopiera hello följande rader toohello, Ersätt ditt lösenord för *yourPassword* (lämna övriga värden oförändrade). Spara hello-filen.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

Skapa en textfil i arbetskatalogen, `wordpress.sql` tooconfigure hello WordPress-databasen: 

```bash
sudo sensible-editor wordpress.sql
```

Lägg till Hej efter kommandona, ersätter du ditt lösenord för *yourPassword* (lämna övriga värden oförändrade). Spara hello-filen.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
toowordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


Hello kör följande kommando toocreate hello databasen:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Ta bort hello filen när hello-kommandot har slutförts `wordpress.sql`.

Flytta hello WordPress installation toohello dokumentet webbservern:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Du kan nu slutföra hello WordPress-installationen och publicera på hello-plattformen. Öppna en webbläsare och gå för`http://yourPublicIPAddress/wordpress`. Ersätt hello offentliga IP-adressen för den virtuella datorn. Det bör se liknande toothis bild.

![Sidan för WordPress-installation](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)