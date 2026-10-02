<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:E95420,100:2C001E&height=200&section=header&text=Ubuntu%20Server%20Homelab&fontSize=44&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Diario%20de%20mi%20servidor%20casero&descAlignY=58&descSize=18" alt="Ubuntu Server Homelab" width="100%" />
</div>

<div align="center">
  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=20&pause=1000&color=E95420&center=true&vCenter=true&width=640&lines=Docker+%2B+Ubuntu+Server;Mi+propia+nube+autoalojada;Acceso+remoto+seguro+con+Tailscale;Copias+de+seguridad+con+Kopia;Aprendiendo+cada+d%C3%ADa+rompiendo+y+arreglando" alt="Typing SVG" />
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
  <a href="#-stack-tecnológico">Stack</a> ·
  <a href="#-bitácora">Bitácora</a> ·
  <a href="#-problemas-y-soluciones-rápidas">Problemas y soluciones</a> ·
  <a href="#-estructura-del-proyecto">Estructura</a> ·
  <a href="#-lecciones-aprendidas">Lecciones</a>
</div>

---

> [!NOTE]
> **Privacidad:** en este repositorio no aparecen direcciones IP, puertos, dominios internos, claves ni identificadores de mi red. Los valores sensibles de los comandos están sustituidos por marcadores (`XXXX`).

---

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

---

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

---

## 📖 Bitácora

Cada fase es un episodio real del montaje del servidor: el problema, lo que lo causaba y cómo se resolvió.

### 📍 Fase 1 · Recuperar el almacenamiento tras reorganizar carpetas

> **Incidencia:** tras mover y borrar carpetas que alojaban volúmenes de Docker, Nginx Proxy Manager empezó a fallar con su base de datos en modo solo lectura.
> **Síntoma:** errores `502 Bad Gateway` y `403 Forbidden` al abrir los servicios.

- **Causa:** al reorganizar el almacenamiento se rompieron permisos y rutas de los volúmenes persistentes.
- **Qué hice:**
  1. Paré el stack de forma limpia con `docker compose down`.
  2. Revisé y reajusté propietarios y permisos (`chown` / `chmod`) sobre los directorios persistentes.
  3. Limpié redes huérfanas con `docker network prune` y levanté todo de nuevo con `docker compose up -d`.
- **Resultado:** servicios de vuelta y base de datos otra vez con escritura.

---

### 📍 Fase 2 · Dos paneles: *Homepage User* y *Homepage Admin*

Quería separar lo que usa cualquiera de lo que solo debo tocar yo, así que monté **dos instancias de [Homepage](https://gethomepage.dev/)**.

| Panel | Contenido |
|---|---|
| 🙋 **User** | Accesos directos a servicios del día a día (Nextcloud, Vaultwarden, etc.) sin exponer nada crítico. |
| 🛡 **Admin** | Portainer, Kopia, Nginx Proxy Manager, métricas de CPU/RAM, estado de contenedores vía Docker Socket. |

**Personalización:** estética *glassmorphism* / cyberpunk con `custom.css` (desenfoque de fondo con `backdrop-filter: blur`, bordes translúcidos y tarjetas flotantes).

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

---

### 📍 Fase 3 · Nginx Proxy Manager no arrancaba por un upstream huérfano

```text
nginx: [emerg] host not found in upstream "authelia" in /data/nginx/proxy_host/9.conf:60
```

- **Causa:** un proxy host seguía apuntando a un contenedor de autenticación (Authelia) que ya no existía. Nginx no resolvía el nombre y cancelaba todo el arranque.
- **Solución, desde dentro del contenedor:**

```bash
# 1. Comprobar la sintaxis de la configuración
sudo docker exec -it nginx-proxy-manager nginx -t

# 2. Desactivar el archivo roto sin borrarlo
sudo docker exec -it nginx-proxy-manager mv /data/nginx/proxy_host/9.conf /data/nginx/proxy_host/9.conf.disabled

# 3. Recargar Nginx en caliente
sudo docker exec -it nginx-proxy-manager nginx -s reload
```

> [!TIP]
> Renombrar a `.disabled` en lugar de borrar permite recuperar la configuración si hace falta más adelante.

---

### 📍 Fase 4 · Acceso remoto con Tailscale desde una red restringida

Mi objetivo era entrar a mis servicios desde otro equipo Ubuntu conectado a una red con filtros (proxy y cortafuegos). Aquí fue donde más aprendí, porque fallaron tres cosas seguidas.

<details open>
<summary><b>1️⃣ Error 403 al aceptar la invitación</b></summary>

<br>

- **Causa probable:** la red bloqueaba el flujo de autenticación OAuth, o había otra cuenta del navegador con la sesión abierta.
- **Soluciones:**
  - Aceptar la invitación en una ventana de incógnito.
  - Hacer el login una sola vez desde la red del móvil (punto de acceso), y volver después a la red restringida.
  - Saltarse el navegador con una **Auth Key** generada desde el panel de Tailscale:

```bash
sudo tailscale up --authkey=tskey-auth-XXXXXXXXXXXXXX
```

</details>

<details open>
<summary><b>2️⃣ Error <code>nodekey already exists</code></b></summary>

<br>

```text
register request: http 400: node nodekey:... already exists
```

- **Causa:** el equipo ya se había registrado antes y conservaba una clave de nodo antigua.
- **Solución:**

```bash
sudo tailscale logout
sudo systemctl restart tailscaled
sudo tailscale up --force-reauth
```

</details>

<details open>
<summary><b>3️⃣ Al conectar Tailscale se perdía la conexión de red</b></summary>

<br>

- **Causa:** conflicto de DNS entre la red local y la red mesh.
- **Solución:** impedir que Tailscale sustituya el DNS del equipo.

```bash
sudo tailscale up --accept-dns=false
```

- **Efecto secundario:** con el DNS de la red restringida ya no se resuelven mis dominios internos, pero **sí funciona entrar por la IP de Tailscale del servidor**, lo que confirma que el túnel funciona.

</details>

**Comandos de diagnóstico que me han salvado:**

```bash
tailscale status      # ¿qué nodos veo y en qué estado?
tailscale netcheck    # ¿se bloquea UDP? ¿llego a los servidores DERP?
ping <ip-tailscale>   # ¿responde mi servidor a través del túnel?
```

---

### 📍 Fase 5 · Copias de seguridad con Kopia

Después de mover y borrar carpetas, quise asegurarme de que las copias seguían apuntando a rutas válidas.

- **Limpieza:** eliminé las políticas de snapshot que apuntaban a carpetas que ya no existen.
- **Snapshot manual:** lancé una copia a mano tras terminar el mantenimiento, para dejar guardado el estado bueno (con Nginx arreglado y los dos Homepage funcionando).
- **Verificación:** comprobé que aparecía el nuevo snapshot con la fecha del día y sin errores.

> [!IMPORTANT]
> Regla que me he puesto: **cada vez que reorganizo carpetas, reviso las rutas en Kopia y lanzo un snapshot manual antes de dar el trabajo por terminado.**

---

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

---

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

---

## 🧠 Lecciones aprendidas

- 🗂 **Mover o borrar carpetas en un servidor con Docker nunca es solo mover carpetas:** afecta a volúmenes, permisos, proxies y copias de seguridad.
- 🪪 **Un error `host not found in upstream` suele significar que quedó una referencia a algo que ya no existe.**
- 📱 **Cuando una red bloquea la autenticación, hay que probar desde otra red** antes de culpar a la configuración propia.
- 🧪 **Probar por IP antes que por dominio** separa los problemas de red de los problemas de DNS.
- 💾 **Después de cada cambio importante, un snapshot manual.** Tarda segundos y ahorra horas.
- 🧾 **Anotar todo.** Este repositorio existe porque casi todos los errores se me olvidarían a las dos semanas.

---

## 🗺 Próximos pasos

- [ ] Hacer que mis dominios internos también se resuelvan desde fuera de casa (AdGuard Home como DNS de la red Tailscale).
- [ ] Seguir ampliando esta bitácora con cada cambio que haga en el servidor.

---

<div align="center">
  <sub>Servidor casero · Ubuntu Server · Docker · Hecho a base de prueba y error 🧪</sub>
</div>

<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:2C001E,100:E95420&height=120&section=footer" alt="Footer" width="100%" />
</div>
