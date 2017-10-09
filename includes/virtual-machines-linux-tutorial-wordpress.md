## <a name="install-wordpress"></a><span data-ttu-id="5f8af-101">Installera WordPress</span><span class="sxs-lookup"><span data-stu-id="5f8af-101">Install WordPress</span></span>

<span data-ttu-id="5f8af-102">Om du vill tootry traven, installerar du en exempelapp.</span><span class="sxs-lookup"><span data-stu-id="5f8af-102">If you want tootry your stack, install a sample app.</span></span> <span data-ttu-id="5f8af-103">Exempelvis installera hello följande hello öppen källkod [WordPress](https://wordpress.org/) plattform toocreate webbplatser och bloggar.</span><span class="sxs-lookup"><span data-stu-id="5f8af-103">As an example, hello following steps install hello open source [WordPress](https://wordpress.org/) platform toocreate websites and blogs.</span></span> <span data-ttu-id="5f8af-104">Andra arbetsbelastningar tootry inkluderar [Drupal](http://www.drupal.org) och [Moodle](https://moodle.org/).</span><span class="sxs-lookup"><span data-stu-id="5f8af-104">Other workloads tootry include [Drupal](http://www.drupal.org) and [Moodle](https://moodle.org/).</span></span> 

<span data-ttu-id="5f8af-105">Den här WordPress-installationen är för konceptbeviset.</span><span class="sxs-lookup"><span data-stu-id="5f8af-105">This WordPress setup is for proof of concept.</span></span> <span data-ttu-id="5f8af-106">Mer information och inställningar för produktion installation finns hello [WordPress dokumentationen](https://codex.wordpress.org/Main_Page).</span><span class="sxs-lookup"><span data-stu-id="5f8af-106">For more information and settings for production installation, see hello [WordPress documentation](https://codex.wordpress.org/Main_Page).</span></span> 



### <a name="install-hello-wordpress-package"></a><span data-ttu-id="5f8af-107">Installera hello WordPress-paketet</span><span class="sxs-lookup"><span data-stu-id="5f8af-107">Install hello WordPress package</span></span>

<span data-ttu-id="5f8af-108">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="5f8af-108">Run hello following command:</span></span>

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a><span data-ttu-id="5f8af-109">Konfigurera WordPress</span><span class="sxs-lookup"><span data-stu-id="5f8af-109">Configure WordPress</span></span>

<span data-ttu-id="5f8af-110">Konfigurera WordPress toouse MySQL och PHP.</span><span class="sxs-lookup"><span data-stu-id="5f8af-110">Configure WordPress toouse MySQL and PHP.</span></span> <span data-ttu-id="5f8af-111">Kör följande kommando tooopen hello en textredigerare önskat och skapa hello fil `/etc/wordpress/config-localhost.php`:</span><span class="sxs-lookup"><span data-stu-id="5f8af-111">Run hello following command tooopen a text editor of your choice and create hello file `/etc/wordpress/config-localhost.php`:</span></span>

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
<span data-ttu-id="5f8af-112">Kopiera hello följande rader toohello, Ersätt ditt lösenord för *yourPassword* (lämna övriga värden oförändrade).</span><span class="sxs-lookup"><span data-stu-id="5f8af-112">Copy hello following lines toohello file, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="5f8af-113">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="5f8af-113">Then save hello file.</span></span>

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

<span data-ttu-id="5f8af-114">Skapa en textfil i arbetskatalogen, `wordpress.sql` tooconfigure hello WordPress-databasen:</span><span class="sxs-lookup"><span data-stu-id="5f8af-114">In a working directory, create a text file `wordpress.sql` tooconfigure hello WordPress database:</span></span> 

```bash
sudo sensible-editor wordpress.sql
```

<span data-ttu-id="5f8af-115">Lägg till Hej efter kommandona, ersätter du ditt lösenord för *yourPassword* (lämna övriga värden oförändrade).</span><span class="sxs-lookup"><span data-stu-id="5f8af-115">Add hello following commands, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="5f8af-116">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="5f8af-116">Then save hello file.</span></span>

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
toowordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


<span data-ttu-id="5f8af-117">Hello kör följande kommando toocreate hello databasen:</span><span class="sxs-lookup"><span data-stu-id="5f8af-117">Run hello following command toocreate hello database:</span></span>

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

<span data-ttu-id="5f8af-118">Ta bort hello filen när hello-kommandot har slutförts `wordpress.sql`.</span><span class="sxs-lookup"><span data-stu-id="5f8af-118">After hello command completes, delete hello file `wordpress.sql`.</span></span>

<span data-ttu-id="5f8af-119">Flytta hello WordPress installation toohello dokumentet webbservern:</span><span class="sxs-lookup"><span data-stu-id="5f8af-119">Move hello WordPress installation toohello web server document root:</span></span>

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

<span data-ttu-id="5f8af-120">Du kan nu slutföra hello WordPress-installationen och publicera på hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="5f8af-120">Now you can complete hello WordPress setup and publish on hello platform.</span></span> <span data-ttu-id="5f8af-121">Öppna en webbläsare och gå för`http://yourPublicIPAddress/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="5f8af-121">Open a browser and go too`http://yourPublicIPAddress/wordpress`.</span></span> <span data-ttu-id="5f8af-122">Ersätt hello offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5f8af-122">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="5f8af-123">Det bör se liknande toothis bild.</span><span class="sxs-lookup"><span data-stu-id="5f8af-123">It should look similar toothis image.</span></span>

![Sidan för WordPress-installation](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)