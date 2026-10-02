<!-- Banner Superior con Iconos e Identidad -->
<div align="center">
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/ubuntu/ubuntu-original.svg" alt="Ubuntu Logo" width="80" />
  <h1>⚡ Ubuntu Server Homelab & Infrastructure Core</h1>
  <p><b>Arquitectura de Servicios Autoalojados, Enrutamiento Dinámico y Alta Disponibilidad Local</b></p>
  
  <p>
    <a href="#-stack-tecnológico"><img src="https://img.shields.io/badge/Estado-Operativo-brightgreen?style=for-the-badge&logo=shield" alt="Status" /></a>
    <a href="#-arquitectura-del-sistema"><img src="https://img.shields.io/badge/Kernel-Ubuntu_Server-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="OS" /></a>
    <a href="#-bitácora-técnica-de-despliegue"><img src="https://img.shields.io/badge/Motor-Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" /></a>
  </p>
</div>

<br>

<!-- Subtítulo Animado Estilo Terminal -->
<div align="center">
  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=19&pause=1000&color=E95420&center=true&vCenter=true&width=650&lines=Base%3A+Ubuntu+Server+Headless;Proxy%3A+Nginx+Proxy+Manager+%2B+SSL;Mesh+VPN%3A+Tailscale+Cross-Network;Dashboards%3A+Split+gethomepage+(Glassmorphic);Backups%3A+Kopia+Automated+Snapshot+Vault" alt="Typing SVG" />
  </a>
</div>

<br>

<!-- Arquitectura General del Servidor -->
<h2 id="-arquitectura-del-sistema">🛠️ Arquitectura e Infraestructura</h2>

<table align="center" width="100%">
  <tr>
    <td width="50%" valign="top">
      <h3 align="center">🖥️ Core Host & Virtualización</h3>
      <ul>
        <li><b>SO Base:</b> Ubuntu Server LTS (Headless).</li>
        <li><b>Contenerización:</b> Motor Docker con aislación de redes virtuales por entorno.</li>
        <li><b>Gestión de Contenedores:</b> Portainer CE para monitoreo de recursos y ciclo de vida de los containers.</li>
        <li><b>Resolución DNS Interna:</b> AdGuard Home actuar como filtro DNS sumado a reescrituras para dominios locales.</li>
      </ul>
    </td>
    <td width="50%" valign="top">
      <h3 align="center">🔐 Seguridad, Proxy & Red</h3>
      <ul>
        <li><b>Inbound Routing:</b> Nginx Proxy Manager (NPM) gestionando certificados y nombres de host locales.</li>
        <li><b>Acceso Remoto Mesh:</b> Tailscale enlazando nodos externos sin exposición directa de puertos WAN en el router.</li>
        <li><b>Respaldo de Datos:</b> Kopia Server administrando backups incrementales deduplicados.</li>
      </ul>
    </td>
  </tr>
</table>

<br>

<!-- Grid Tecnológico -->
<h2 id="-stack-tecnológico">🧰 Stack Tecnológico Activo</h2>

<div align="center">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Nginx_Proxy_Manager-009639?style=for-the-badge&logo=nginx&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailscale-4B5563?style=for-the-badge&logo=tailscale&logoColor=white" />
  <img src="https://img.shields.io/badge/AdGuard_Home-00B074?style=for-the-badge&logo=adguard&logoColor=white" />
  <img src="https://img.shields.io/badge/Portainer-13BEF9?style=for-the-badge&logo=portainer&logoColor=white" />
  <img src="https://img.shields.io/badge/Nextcloud-0082C9?style=for-the-badge&logo=nextcloud&logoColor=white" />
  <img src="https://img.shields.io/badge/Vaultwarden-175DDC?style=for-the-badge&logo=bitwarden&logoColor=white" />
  <img src="https://img.shields.io/badge/NocoDB-1890FF?style=for-the-badge&logo=nocodb&logoColor=white" />
  <img src="https://img.shields.io/badge/Kopia-000000?style=for-the-badge&logo=kopia&logoColor=white" />
  <img src="https://img.shields.io/badge/Minecraft_Server-388E3C?style=for-the-badge&logo=minecraft&logoColor=white" />
</div>

<br>

<!-- Bitácora Detallada -->
<h2 id="-bitácora-técnica-de-despliegue">📖 Bitácora Extendida de Diagnóstico, Despliegues y Mantenimiento</h2>

> ℹ️ **Nota de Seguridad:** Todos los parámetros de red, identificadores únicos, variables sensibles e IPs han sido sustituidos por constantes genéricas para mantener la privacidad del entorno.

---

### 📍 Fase 1: Recuperación de Almacenamiento y Sistema de Archivos `Read-Only`
┌────────────────────────────────────────────────────────────────────────┐
│  [INCIDENCIA]: Corrupción de permisos y estado Read-Only en SQLite     │
│  [SÍNTOMA]: NPM responde con errores 502/403 en Proxy Hosts            │
└────────────────────────────────────────────────────────────────────────┘

* **Detalle del Problema:** Durante una reorganización de volúmenes en el almacenamiento secundario, se eliminaron de manera no planificada directorios de soporte para los contenedores activos. Esto ocasionó que el motor SQLite subyacente en Nginx Proxy Manager fuera incapaz de escribir en sus tablas temporales, bloqueando la base de datos en estado de solo lectura (*read-only*).
* **Acciones de Mitigación:**
  1. Detención limpia del servicio de contenedores con `docker compose down`.
  2. Aislamiento de permisos sobre el árbol de volúmenes persistentes aplicando `chmod`/`chown` a los directorios de datos.
  3. Purga de volúmenes huérfanos e inconsistentes mediante `docker network prune` y reconstrucción controlada del esquema con `docker compose up -d`.

---

### 📍 Fase 2: Arquitectura "Split Dashboard" con `gethomepage` & Glassmorphism

Se implementó una estrategia de paneles divididos para separar el acceso de usuario habitual de los paneles críticos de administración del servidor.

              ┌────────────────────────┐
              │   Inbound Traffic      │
              └───────────┬────────────┘
                          │
                ┌─────────┴─────────┐
                │ Nginx Proxy Manager│
                └────┬────────────┬─┘
                     │            │
  ┌──────────────────┴─┐        ┌─┴──────────────────┐
  │ Homepage USER      │        │ Homepage ADMIN     │
  │ (Vista Simplificada│        │ (Métricas, Kopia,  │
  │  Apps de Diario)   │        │  Portainer, NPM)   │
  └────────────────────┘        └────────────────────┘

  * **Estructura Modular:**
  * **Homepage User:** Accesos directos a la nube personal (Nextcloud), gestor de contraseñas (Vaultwarden) y herramientas de gestión sin exponer paneles de control.
  * **Homepage Admin:** Monitoreo en tiempo real de consumo de RAM/CPU, estado de contenedores vía Docker Socket, métricas de Kopia y accesos a Portainer.
* **Personalización Estética:** Inyección de CSS personalizado (`custom.css`) para aplicar desenfoque de fondo (*backdrop-filter: blur*), bordes traslúcidos y tarjetas flotantes de estilo *Glassmorphism/Cyberpunk*.

---

### 📍 Fase 3: Depuración Interna de Upstreams y Errores de Arranque en Nginx

┌────────────────────────────────────────────────────────────────────────┐
│  nginx: [emerg] host not found in upstream "authelia" in 9.conf        │
└────────────────────────────────────────────────────────────────────────┘

* **Análisis de Fallo:** Durante las tareas de reestructuración, se mantuvo activo un archivo de proxy host (`9.conf`) que referenciaba a un contenedor de autenticación (`authelia`) desmontado. Dado que el DNS interno de Docker no podía resolver el nombre de host del contenedor extinto, el binario de Nginx cancelaba el arranque (*exit code 1*).
* **Resolución vía Shell del Contenedor:**
  1. Ejecución de pruebas de sintaxis aisladas directamente sobre el contenedor:
     ```bash
     sudo docker exec -it nginx-proxy-manager nginx -t
     ```
  2. Mantenimiento in-situ renombrando el archivo dañado dentro del volumen interno:
     ```bash
     sudo docker exec -it nginx-proxy-manager mv /data/nginx/proxy_host/9.conf /data/nginx/proxy_host/9.conf.disabled
     ```
  3. Recarga en caliente del servicio de proxy sin pérdida de paquetes:
     ```bash
     sudo docker exec -it nginx-proxy-manager nginx -s reload
     ```

---

### 📍 Fase 4: Enrutamiento Mesh Remoto y Despliegue de Tailscale

Se configuró el acceso remoto cifrado punto a punto utilizando la red mesh de Tailscale para permitir el trabajo sobre el servidor desde redes restrictivas (firewalls corporativos o educativos).

* **Superación de Restricciones HTTP 403 / OAuth:**
  * En entornos con proxies que bloquean el inicio de sesión web tradicional, la autenticación mediante navegador fallaba. Se solucionó generando una **Auth Key** de nodo único e inyectándola por terminal:
    ```bash
    sudo tailscale up --force-reauth --authkey=tskey-auth-XXXXXXXXXXXXXX
    ```
* **Resolución del Error `nodekey already exists`:**
  * Se purgó la clave de nodo desactualizada almacenada en el demonio del sistema mediante `sudo tailscale logout` seguido del reinicio del servicio `tailscaled`.
* **Aislamiento de Colisiones DNS:**
  * Para evitar colisiones entre el servidor DNS de la red remota y la navegación general de los equipos clientes, se ajustaron las banderas de enrutamiento:
    ```bash
    sudo tailscale up --accept-dns=false
    ```

---

### 📍 Fase 5: Estrategia de Copias de Seguridad y Deduplicación con Kopia

Se consolidó el esquema de copias de seguridad continuas para garantizar la recuperabilidad total ante fallos de hardware.

┌────────────────────────────────────────────────────────────────────────┐
│  Snapshot Target: ~/homepage-split / Volúmenes de Aplicaciones         │
│  Frecuencia: Programada + Lanzamientos Manuales Post-Mantenimiento     │
└────────────────────────────────────────────────────────────────────────┘

* **Limpieza de Referencias Huérfanas:** Se eliminaron las políticas de snapshot que apuntaban a rutas de almacenamiento antiguas (`homelab-apps-bak`) para evitar errores en las tareas en segundo plano.
* **Verificación de Integridad:** Se validó la ejecución de backups incrementales para asegurar que los cambios realizados en las configuraciones `.yaml` y las reglas de Nginx queden respaldados.

---

<!-- Estructura de Proyecto -->
<h2 id="-estructura-de-directorios">📂 Estructura de Persistencia del Servidor</h2>

<table align="center" width="90%">
  <tr>
    <td>
      <pre><code>/home/administrador/
├── homepage-split/
│   ├── docker-compose.yml          # Stack principal de ambas instancias de Homepage
│   ├── config-user/                # Archivos .yaml y custom.css (Vista Usuario)
│   ├── config-admin/               # Archivos .yaml y monitoreo (Vista Admin)
│   └── data/nginx/                 # Volúmenes persistentes de Nginx Proxy Manager
│       └── proxy_host/             # Archivos de configuración .conf por dominio
├── adguard/                        # Persistencia del filtro DNS y reescrituras
└── kopia/                          # Almacenamiento local de snapshots y credenciales</code></pre>
    </td>
  </tr>
</table>

<br>

<hr>

<div align="center">
  <p><b>Ubuntu Server Homelab</b> • <i>Infraestructura autoalojada y mantenida con Docker</i></p>
</div>
