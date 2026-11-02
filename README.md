<div align="center">
  <img src="assets/header.svg" alt="Ubuntu Server Homelab · Diario de mi servidor casero" width="100%" />
</div>

<div align="center">
  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.demolab.com/?font=Fira+Code&weight=600&size=20&pause=1000&color=E95420&center=true&vCenter=true&width=640&lines=Docker+%2B+Ubuntu+Server;Mi+propia+nube+autoalojada;Acceso+remoto+seguro+con+Tailscale;Copias+de+seguridad+con+Kopia;Aprendiendo+cada+d%C3%ADa+rompiendo+y+arreglando" alt="Typing SVG" />
  </a>
</div>

<br>

<div align="center">
  <img src="https://img.shields.io/badge/Estado-Operativo-2ea44f?style=for-the-badge" alt="Estado" />
  <img src="https://img.shields.io/badge/SO-Ubuntu_Server-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu Server" />
  <img src="https://img.shields.io/badge/Contenedores-Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker Compose" />
  <img src="https://img.shields.io/badge/Acceso_remoto-Tailscale-4B5563?style=for-the-badge&logo=tailscale&logoColor=white" alt="Tailscale" />
</div>

<br>

<div align="center">
  <b>📖 Qué es esto</b><br>
  Un cuaderno de bitácora de mi servidor casero: qué he montado, qué se ha roto, cómo lo he arreglado y qué he aprendido por el camino.
  <br><br>
  <a href="#-arquitectura">Arquitectura</a> ·
  <a href="#-por-qué-está-montado-así">Decisiones</a> ·
  <a href="#-stack-tecnológico">Stack</a> ·
  <a href="#-bitácora">Bitácora</a> ·
  <a href="#-problemas-y-soluciones-rápidas">Problemas y soluciones</a> ·
  <a href="#-estructura-del-proyecto">Estructura</a> ·
  <a href="#-lecciones-aprendidas">Lecciones</a> ·
  <a href="#-glosario">Glosario</a>
</div>

<img src="assets/divider.svg" width="100%" alt="" />

> [!NOTE]
> **Privacidad:** en este repositorio no aparecen direcciones IP, puertos, dominios internos, claves ni identificadores de mi red. Los valores sensibles de los comandos están sustituidos por marcadores (`XXXX`).

<img src="assets/divider.svg" width="100%" alt="" />

## 🛠 Arquitectura

<table align="center" width="100%">
  <tr>
    <td width="50%" valign="top">
      <h3 align="center">🖥 Núcleo del servidor</h3>
      <ul>
        <li>🐧 <b>Sistema base:</b> Ubuntu Server (headless, sin entorno gráfico).</li>
        <li>🐳 <b>Contenedores:</b> Docker + Docker Compose, un stack por servicio.</li>
        <li>🧭 <b>Gestión visual:</b> Portainer para ver el estado y los logs de cada contenedor.</li>
        <li>🌐 <b>DNS interno:</b> AdGuard Home como filtro de anuncios y para resolver dominios locales.</li>
      </ul>
    </td>
    <td width="50%" valign="top">
      <h3 align="center">🔐 Red, proxy y seguridad</h3>
      <ul>
        <li>🔀 <b>Proxy inverso:</b> Nginx Proxy Manager para publicar cada servicio con su propio nombre.</li>
        <li>🕸 <b>Acceso remoto:</b> Tailscale (red mesh cifrada), sin abrir puertos en el router.</li>
        <li>🔑 <b>Contraseñas:</b> Vaultwarden, mi gestor autoalojado.</li>
        <li>💾 <b>Copias de seguridad:</b> Kopia con snapshots incrementales y deduplicados.</li>
      </ul>
    </td>
  </tr>
</table>

### Cómo viaja una petición

```mermaid
flowchart LR
    U([Usuario en casa]) --> DNS[AdGuard Home<br/>DNS local]
    R([Yo desde fuera]) -->|Túnel cifrado| TS[Tailscale]
    DNS --> NPM[Nginx Proxy Manager]
    TS --> NPM
    NPM --> HU[Homepage User]
    NPM --> HA[Homepage Admin]
    NPM --> APPS[Nextcloud · Vaultwarden · NocoDB · ...]
    HA --> ADM[Portainer · Kopia · NPM]
    APPS -.-> K[(Kopia<br/>Snapshots)]
    HA -.-> K
```

**Paso a paso:**

1. **Resolver el nombre.** Desde casa, AdGuard Home traduce el nombre del servicio a la dirección del servidor (y de paso filtra anuncios y rastreadores para toda la red).
2. **Entrar en la red.** Si estoy fuera, no hay ningún puerto abierto en el router: entro por el túnel cifrado de Tailscale como si estuviera en casa.
3. **Elegir el servicio.** Todas las peticiones llegan a Nginx Proxy Manager, que mira el nombre pedido y reenvía la petición al contenedor correcto.
4. **Separar usuario y admin.** Lo cotidiano cuelga de *Homepage User*; lo delicado (Portainer, Kopia, el propio proxy) solo es accesible desde *Homepage Admin*.
5. **Guardar el estado.** Kopia hace snapshots de los datos de los servicios para poder volver atrás si algo se rompe.

<img src="assets/divider.svg" width="100%" alt="" />

## 🧩 Por qué está montado así

Cada pieza responde a un problema concreto. Estas son las decisiones que hay detrás:

| Decisión | Por qué |
|---|---|
| 🚪 **Ningún puerto abierto en el router** | Tailscale crea el túnel desde dentro hacia fuera, así que el servidor no queda expuesto a internet y no hace falta pelearse con la configuración del router. |
| 🧱 **Un stack de Docker Compose por servicio** | Si un servicio se rompe o lo actualizo, el resto sigue funcionando. Cada uno tiene su `docker-compose.yml`, sus volúmenes y su red. |
| 🔀 **Proxy inverso delante de todo** | Recuerdo nombres, no combinaciones de dirección y puerto. Además, es un único sitio desde el que gestionar todos los accesos. |
| 🌐 **DNS propio con AdGuard Home** | Me permite crear reescrituras DNS para mis dominios internos y bloquear publicidad en todos los dispositivos de casa sin instalar nada en ellos. |
| 🛡 **Dos paneles separados (User / Admin)** | Principio de mínimo privilegio: quien solo usa los servicios no ve los botones que pueden tumbarlos. |
| 💾 **Copias incrementales y deduplicadas** | Kopia solo guarda lo que ha cambiado desde la última copia, así que puedo hacer snapshots a menudo sin llenar el disco. |
| 🖥 **Servidor headless** | Sin entorno gráfico hay menos consumo de RAM, menos paquetes que actualizar y menos superficie de ataque. Todo se gestiona desde la terminal y los paneles web. |

<img src="assets/divider.svg" width="100%" alt="" />

## 🧰 Stack tecnológico

<div align="center">
  <img src="https://img.shields.io/badge/Ubuntu_Server-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/Portainer-13BEF9?style=for-the-badge&logo=portainer&logoColor=white" alt="Portainer" />
  <img src="https://img.shields.io/badge/Nginx_Proxy_Manager-F15833?style=for-the-badge&logo=nginxproxymanager&logoColor=white" alt="Nginx Proxy Manager" />
  <img src="https://img.shields.io/badge/AdGuard_Home-68BC71?style=for-the-badge&logo=adguard&logoColor=white" alt="AdGuard Home" />
  <img src="https://img.shields.io/badge/Tailscale-4B5563?style=for-the-badge&logo=tailscale&logoColor=white" alt="Tailscale" />
  <img src="https://img.shields.io/badge/Nextcloud-0082C9?style=for-the-badge&logo=nextcloud&logoColor=white" alt="Nextcloud" />
  <img src="https://img.shields.io/badge/Vaultwarden-175DDC?style=for-the-badge&logo=bitwarden&logoColor=white" alt="Vaultwarden" />
  <img src="https://img.shields.io/badge/NocoDB-1890FF?style=for-the-badge&logo=nocodb&logoColor=white" alt="NocoDB" />
  <img src="https://img.shields.io/badge/Homepage-7C3AED?style=for-the-badge" alt="Homepage" />
  <img src="https://img.shields.io/badge/Kopia-0F172A?style=for-the-badge" alt="Kopia" />
  <img src="https://img.shields.io/badge/Minecraft_Server-388E3C?style=for-the-badge&logo=minecraft&logoColor=white" alt="Minecraft Server" />
</div>

<br>

| Servicio | Para qué lo uso | Quién lo ve |
|---|---|---|
| **Nextcloud** | Mi nube personal: archivos, calendario y sincronización | Usuario |
| **Vaultwarden** | Gestor de contraseñas autoalojado | Usuario |
| **NocoDB** | Bases de datos con interfaz tipo hoja de cálculo | Usuario |
| **Minecraft Server** | Servidor para jugar con amigos | Usuario |
| **Portainer** | Control de contenedores, volúmenes y redes | Admin |
| **Nginx Proxy Manager** | Proxy inverso y gestión de hosts | Admin |
| **AdGuard Home** | DNS local y bloqueo de anuncios | Admin |
| **Kopia** | Copias de seguridad y restauración | Admin |
| **Tailscale** | Acceso remoto seguro a todo lo anterior | Admin |

<details>
<summary><b>🧪 Otras piezas con las que también trabajo en el laboratorio</b></summary>

<br>

<div align="center">
  <img src="https://img.shields.io/badge/WireGuard-881798?style=for-the-badge&logo=wireguard&logoColor=white" alt="WireGuard" />
  <img src="https://img.shields.io/badge/ELK_Stack-005571?style=for-the-badge&logo=elastic&logoColor=white" alt="ELK Stack" />
  <img src="https://img.shields.io/badge/UniFi-0559C9?style=for-the-badge&logo=ubiquiti&logoColor=white" alt="UniFi" />
</div>

<br>

| Herramienta | Qué es |
|---|---|
| **WireGuard** | Protocolo de VPN moderno y muy ligero. Es, además, el motor sobre el que funciona Tailscale por debajo. |
| **ELK Stack** | Elasticsearch, Logstash y Kibana: recoger los logs de varios servicios, guardarlos en un solo sitio y buscarlos o graficarlos desde un panel. |
| **UniFi** | Switches gestionables de Ubiquiti para la red de casa, configurados desde su controlador. |

</details>

<img src="assets/divider.svg" width="100%" alt="" />

## 📖 Bitácora

Cada fase es un episodio real del montaje del servidor: el problema, lo que lo causaba y cómo se resolvió.

```mermaid
timeline
    title Fases de la bitácora
    Fase 1 : Almacenamiento
           : Permisos y volúmenes rotos
    Fase 2 : Paneles
           : Homepage User y Admin
    Fase 3 : Proxy inverso
           : Upstream huérfano en Nginx
    Fase 4 : Acceso remoto
           : Tailscale en red restringida
    Fase 5 : Copias de seguridad
           : Limpieza y snapshot en Kopia
```

<img src="assets/fase-1.svg" alt="Fase 1 · Recuperar el almacenamiento tras reorganizar carpetas" width="100%" />

> [!CAUTION]
> **Incidencia:** tras mover y borrar carpetas que alojaban volúmenes de Docker, Nginx Proxy Manager empezó a fallar con su base de datos en modo solo lectura.
>
> **Síntoma:** errores `502 Bad Gateway` y `403 Forbidden` al abrir los servicios.

> [!WARNING]
> **Causa:** al reorganizar el almacenamiento se rompieron permisos y rutas de los volúmenes persistentes.

> [!NOTE]
> **Qué hice:**
> 1. Paré el stack de forma limpia con `docker compose down`.
> 2. Revisé y reajusté propietarios y permisos (`chown` / `chmod`) sobre los directorios persistentes.
> 3. Limpié redes huérfanas con `docker network prune` y levanté todo de nuevo con `docker compose up -d`.

> [!TIP]
> **Resultado:** servicios de vuelta y base de datos otra vez con escritura.

<details>
<summary><b>🔎 Por qué pasa esto</b></summary>

<br>

Un contenedor guarda sus datos persistentes en un **volumen** o en una carpeta del host montada dentro de él. Dentro del contenedor, el proceso se ejecuta con un usuario concreto (identificado por su UID/GID) y necesita ser propietario de esa carpeta para poder escribir.

Al mover carpetas con otro usuario o con `sudo`, el propietario puede cambiar. El contenedor sigue arrancando, pero ya no puede escribir: la base de datos interna de Nginx Proxy Manager se abre en **solo lectura** y, como el proxy no puede trabajar, todo lo que hay detrás responde con errores `502` o `403`.

```bash
ls -ln /ruta/al/volumen      # -n muestra UID/GID numéricos: lo que de verdad ve el contenedor
docker compose logs -f       # el log del servicio suele decir exactamente qué archivo no puede escribir
```

</details>

<img src="assets/divider.svg" width="100%" alt="" />

<img src="assets/fase-2.svg" alt="Fase 2 · Dos paneles: Homepage User y Homepage Admin" width="100%" />

> [!NOTE]
> Quería separar lo que usa cualquiera de lo que solo debo tocar yo, así que monté **dos instancias de [Homepage](https://gethomepage.dev/)**.

| Panel | Contenido |
|---|---|
| 🙋 **User** | Accesos directos a servicios del día a día (Nextcloud, Vaultwarden, etc.) sin exponer nada crítico. |
| 🛡 **Admin** | Portainer, Kopia, Nginx Proxy Manager, métricas de CPU/RAM, estado de contenedores vía Docker Socket. |

> [!TIP]
> **Personalización:** estética *glassmorphism* / cyberpunk con `custom.css` (desenfoque de fondo con `backdrop-filter: blur`, bordes translúcidos y tarjetas flotantes).

> [!WARNING]
> **Ojo con el Docker Socket.** El panel de admin lee el estado de los contenedores a través de `/var/run/docker.sock`. Quien controla ese socket controla Docker, y quien controla Docker tiene, en la práctica, acceso de administrador al servidor. Por eso conviene montarlo en **solo lectura** (`/var/run/docker.sock:/var/run/docker.sock:ro`) o, mejor aún, poner delante un *socket proxy* que solo deje pasar las consultas de lectura. Y es otra razón para que ese panel sea solo de admin.

<details>
<summary><b>🔎 Curiosidad: ¿por qué no encuentro ningún archivo .html?</b></summary>

<br>

Homepage está hecha con **Next.js / React**, así que no existe una plantilla `index.html` editable en la carpeta de configuración. Lo único que se toca es:

- `services.yaml`, `widgets.yaml`, `settings.yaml`, `docker.yaml`... → qué se muestra y cómo se organiza.
- `custom.css` y `custom.js` → apariencia y scripts propios.

El HTML se genera al vuelo combinando esos `.yaml` con el código compilado dentro de la imagen de Docker. Si quiero ver el HTML final, lo puedo copiar desde las herramientas de desarrollador del navegador:

```js
copy(document.documentElement.outerHTML);
```

Es una foto fija del momento: los widgets con métricas en vivo se quedan congelados con el valor que tenían al copiar.

</details>

<img src="assets/divider.svg" width="100%" alt="" />

<img src="assets/fase-3.svg" alt="Fase 3 · Nginx Proxy Manager no arrancaba por un upstream huérfano" width="100%" />

```text
nginx: [emerg] host not found in upstream "authelia" in /data/nginx/proxy_host/9.conf:60
```

> [!WARNING]
> **Causa:** un proxy host seguía apuntando a un contenedor de autenticación (Authelia) que ya no existía. Nginx no resolvía el nombre y cancelaba todo el arranque.

> [!NOTE]
> **Solución, desde dentro del contenedor:**
>
> ```bash
> # 1. Comprobar la sintaxis de la configuración
> sudo docker exec -it nginx-proxy-manager nginx -t
>
> # 2. Desactivar el archivo roto sin borrarlo
> sudo docker exec -it nginx-proxy-manager mv /data/nginx/proxy_host/9.conf /data/nginx/proxy_host/9.conf.disabled
>
> # 3. Recargar Nginx en caliente
> sudo docker exec -it nginx-proxy-manager nginx -s reload
> ```

> [!TIP]
> Renombrar a `.disabled` en lugar de borrar permite recuperar la configuración si hace falta más adelante.

<details>
<summary><b>🔎 Qué es un «upstream» y por qué un solo archivo tumba todo Nginx</b></summary>

<br>

En un proxy inverso, el **upstream** es el servidor de destino al que se reenvían las peticiones; en Docker suele ser el nombre de otro contenedor. Nginx resuelve esos nombres **al arrancar** y, si uno no existe, no arranca a medias: aborta con `[emerg]` y se lleva por delante todos los hosts, también los que estaban bien.

Por eso, antes de recargar Nginx tras cualquier cambio, `nginx -t` es obligatorio: comprueba toda la configuración sin aplicarla y dice el archivo y la línea exacta del problema.

</details>

<img src="assets/divider.svg" width="100%" alt="" />

<img src="assets/fase-4.svg" alt="Fase 4 · Acceso remoto con Tailscale desde una red restringida" width="100%" />

> [!IMPORTANT]
> Mi objetivo era entrar a mis servicios desde otro equipo Ubuntu conectado a una red con filtros (proxy y cortafuegos). Aquí fue donde más aprendí, porque fallaron tres cosas seguidas.

<details open>
<summary><b>1️⃣ Error 403 al aceptar la invitación</b></summary>

<br>

> [!WARNING]
> **Causa probable:** la red bloqueaba el flujo de autenticación OAuth, o había otra cuenta del navegador con la sesión abierta.

> [!TIP]
> **Soluciones:**
> - Aceptar la invitación en una ventana de incógnito.
> - Hacer el login una sola vez desde la red del móvil (punto de acceso), y volver después a la red restringida.
> - Saltarse el navegador con una **Auth Key** generada desde el panel de Tailscale:
>
> ```bash
> sudo tailscale up --authkey=tskey-auth-XXXXXXXXXXXXXX
> ```

</details>

<details open>
<summary><b>2️⃣ Error <code>nodekey already exists</code></b></summary>

<br>

```text
register request: http 400: node nodekey:... already exists
```

> [!WARNING]
> **Causa:** el equipo ya se había registrado antes y conservaba una clave de nodo antigua.

> [!TIP]
> **Solución:**
>
> ```bash
> sudo tailscale logout
> sudo systemctl restart tailscaled
> sudo tailscale up --force-reauth
> ```

</details>

<details open>
<summary><b>3️⃣ Al conectar Tailscale se perdía la conexión de red</b></summary>

<br>

> [!WARNING]
> **Causa:** conflicto de DNS entre la red local y la red mesh.

> [!TIP]
> **Solución:** impedir que Tailscale sustituya el DNS del equipo.
>
> ```bash
> sudo tailscale up --accept-dns=false
> ```

> [!NOTE]
> **Efecto secundario:** con el DNS de la red restringida ya no se resuelven mis dominios internos, pero **sí funciona entrar por la IP de Tailscale del servidor**, lo que confirma que el túnel funciona.

</details>

**Comandos de diagnóstico que me han salvado:**

```bash
tailscale status      # ¿qué nodos veo y en qué estado?
tailscale netcheck    # ¿se bloquea UDP? ¿llego a los servidores DERP?
ping <ip-tailscale>   # ¿responde mi servidor a través del túnel?
```

**El orden en el que lo diagnostico ahora:**

```mermaid
flowchart TD
    A[No llego a un servicio] --> B{tailscale status<br/>¿veo el servidor conectado?}
    B -- No --> B1[Problema de login o de registro del nodo<br/>Auth Key / --force-reauth]
    B -- Sí --> C{tailscale netcheck<br/>¿UDP permitido?}
    C -- No --> C1[La red bloquea UDP:<br/>el tráfico irá por DERP, más lento pero funciona]
    C -- Sí --> D{ping a la IP de Tailscale<br/>¿responde?}
    C1 --> D
    D -- No --> D1[El túnel no llega:<br/>revisar el servicio tailscaled en ambos equipos]
    D -- Sí --> E{¿Funciona por IP<br/>pero no por nombre?}
    E -- Sí --> E1[Es DNS, no la red]
    E -- No --> F[El problema está en el proxy<br/>o en el propio servicio]
```

<details>
<summary><b>🔎 Qué son los servidores DERP</b></summary>

<br>

Tailscale intenta siempre una **conexión directa** entre los dos equipos usando WireGuard sobre UDP. En redes con cortafuegos estrictos el UDP suele estar bloqueado, así que Tailscale recurre a los **DERP**: servidores de retransmisión que reenvían el tráfico, ya cifrado de extremo a extremo, por HTTPS.

- **Ventaja:** funciona casi en cualquier red, porque el tráfico parece navegación web normal.
- **Inconveniente:** más latencia, porque los paquetes dan un rodeo.

Con `tailscale ping <nodo>` se ve si la conexión va directa o `via DERP`.

</details>

<img src="assets/divider.svg" width="100%" alt="" />

<img src="assets/fase-5.svg" alt="Fase 5 · Copias de seguridad con Kopia" width="100%" />

> [!NOTE]
> Después de mover y borrar carpetas, quise asegurarme de que las copias seguían apuntando a rutas válidas.

<table>
  <tr>
    <td width="33%" valign="top" align="center">
      <h3>🧹</h3>
      <b>Limpieza</b>
      <br><br>
      <sub>Eliminé las políticas de snapshot que apuntaban a carpetas que ya no existen.</sub>
    </td>
    <td width="33%" valign="top" align="center">
      <h3>📸</h3>
      <b>Snapshot manual</b>
      <br><br>
      <sub>Lancé una copia a mano tras terminar el mantenimiento, para dejar guardado el estado bueno (con Nginx arreglado y los dos Homepage funcionando).</sub>
    </td>
    <td width="33%" valign="top" align="center">
      <h3>✅</h3>
      <b>Verificación</b>
      <br><br>
      <sub>Comprobé que aparecía el nuevo snapshot con la fecha del día y sin errores.</sub>
    </td>
  </tr>
</table>

> [!IMPORTANT]
> Regla que me he puesto: **cada vez que reorganizo carpetas, reviso las rutas en Kopia y lanzo un snapshot manual antes de dar el trabajo por terminado.**

<details>
<summary><b>🔎 Cómo trabaja Kopia por dentro</b></summary>

<br>

- **Snapshots:** cada copia es una foto completa de una carpeta en un momento concreto, y se puede restaurar entera o archivo a archivo.
- **Incrementales y deduplicadas:** Kopia trocea los archivos en bloques y guarda cada bloque una sola vez. Si un archivo no cambia, no ocupa más espacio; si cambia un poco, solo se guarda lo nuevo.
- **Cifrado:** el repositorio de copias va cifrado con contraseña, así que se podría guardar incluso fuera de casa.
- **Políticas:** por cada ruta se define qué se guarda, qué se ignora y cuántas copias se conservan (por horas, días, semanas…).

```bash
kopia snapshot list           # copias que existen y su fecha
kopia snapshot create <ruta>  # lanzar un snapshot manual
kopia policy list             # qué rutas tienen política (aquí se ven las huérfanas)
```

> [!TIP]
> Una copia que nunca se ha probado a restaurar es solo una esperanza. De vez en cuando conviene recuperar algún archivo de un snapshot para confirmar que todo el proceso funciona.

</details>

<img src="assets/divider.svg" width="100%" alt="" />

## 🔧 Problemas y soluciones rápidas

| Síntoma | Causa | Solución |
|---|---|---|
| `502` / `403` en todos los servicios | Permisos rotos en los volúmenes tras mover carpetas | Parar el stack, reajustar `chown`/`chmod`, levantar de nuevo |
| Nginx no arranca (`host not found in upstream`) | Un proxy host apunta a un contenedor que ya no existe | Renombrar el `.conf` afectado a `.disabled` y recargar |
| Error `403` al aceptar invitación de Tailscale | Red con proxy/cortafuegos o sesión de navegador cruzada | Incógnito, red del móvil o Auth Key |
| `nodekey already exists` | Clave de nodo antigua en el equipo | `tailscale logout` + reiniciar `tailscaled` + `--force-reauth` |
| Sin internet al activar Tailscale | Conflicto de DNS | `--accept-dns=false` |
| No cargan mis dominios internos desde fuera | El DNS externo no sabe resolverlos | Entrar por IP de Tailscale, o configurar AdGuard como DNS de la red mesh |
| No encuentro el HTML de Homepage | Es una app Next.js, no hay plantilla | Editar los `.yaml` y `custom.css` |

<img src="assets/divider.svg" width="100%" alt="" />

## 📂 Estructura del proyecto

```text
~/
├── homepage-split/
│   ├── docker-compose.yml     # Orquesta Homepage User y Homepage Admin
│   ├── config-user/           # .yaml y custom.css del panel de usuario
│   ├── config-admin/          # .yaml del panel de administración
│   └── data/nginx/            # Volúmenes persistentes de Nginx Proxy Manager
│       └── proxy_host/        # Un .conf por cada host publicado
├── adguard/                   # Filtros y reescrituras DNS
└── kopia/                     # Configuración y repositorio de snapshots
```

<img src="assets/divider.svg" width="100%" alt="" />

## 🧠 Lecciones aprendidas

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>🗂</h3>
      <b>Mover o borrar carpetas en un servidor con Docker nunca es solo mover carpetas:</b> afecta a volúmenes, permisos, proxies y copias de seguridad.
    </td>
    <td width="50%" valign="top">
      <h3>🪪</h3>
      <b>Un error <code>host not found in upstream</code> suele significar que quedó una referencia a algo que ya no existe.</b>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <h3>📱</h3>
      <b>Cuando una red bloquea la autenticación, hay que probar desde otra red</b> antes de culpar a la configuración propia.
    </td>
    <td width="50%" valign="top">
      <h3>🧪</h3>
      <b>Probar por IP antes que por dominio</b> separa los problemas de red de los problemas de DNS.
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <h3>💾</h3>
      <b>Después de cada cambio importante, un snapshot manual.</b> Tarda segundos y ahorra horas.
    </td>
    <td width="50%" valign="top">
      <h3>🧾</h3>
      <b>Anotar todo.</b> Este repositorio existe porque casi todos los errores se me olvidarían a las dos semanas.
    </td>
  </tr>
</table>

<img src="assets/divider.svg" width="100%" alt="" />

## 📚 Glosario

<details>
<summary><b>Términos que aparecen en esta bitácora</b></summary>

<br>

| Término | Qué significa |
|---|---|
| **Headless** | Servidor sin entorno gráfico; se administra por terminal (SSH) y paneles web. |
| **Contenedor** | Una aplicación empaquetada con todo lo que necesita, aislada del resto del sistema. |
| **Docker Compose** | Archivo `docker-compose.yml` que describe uno o varios contenedores, sus volúmenes y redes, y los levanta con un solo comando. |
| **Volumen** | Carpeta donde un contenedor guarda los datos que deben sobrevivir aunque el contenedor se borre. |
| **Proxy inverso** | Servidor que recibe todas las peticiones y las reparte al servicio correcto según el nombre pedido. |
| **Upstream** | El servicio de destino al que el proxy reenvía una petición. |
| **DNS / reescritura DNS** | El sistema que traduce nombres a direcciones; una reescritura hace que un nombre interno apunte a mi servidor. |
| **Red mesh** | Red en la que cada equipo puede hablar directamente con los demás, sin pasar por un servidor central. |
| **DERP** | Servidores de retransmisión de Tailscale para cuando la conexión directa no es posible. |
| **Auth Key** | Clave de un solo uso para registrar un equipo en Tailscale sin pasar por el navegador. |
| **Snapshot** | Copia de seguridad de una carpeta tal y como estaba en un momento concreto. |
| **Deduplicación** | Guardar una sola vez los datos repetidos para ahorrar espacio. |
| **Docker Socket** | El canal por el que se le dan órdenes a Docker; quien tiene acceso a él controla todos los contenedores. |

</details>

<img src="assets/divider.svg" width="100%" alt="" />

## 🗺 Próximos pasos

- [ ] Hacer que mis dominios internos también se resuelvan desde fuera de casa (AdGuard Home como DNS de la red Tailscale).
- [ ] Seguir ampliando esta bitácora con cada cambio que haga en el servidor.

<img src="assets/divider.svg" width="100%" alt="" />

<div align="center">
  <sub>Servidor casero · Ubuntu Server · Docker · Hecho a base de prueba y error 🧪</sub>
</div>

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:2C001E,100:E95420&height=120&section=footer" alt="Footer" width="100%" />
</div>
