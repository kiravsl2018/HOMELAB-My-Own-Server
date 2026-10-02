<!-- Header animado con tipografía SVG -->
<div align="center">
  <img src="[https://raw.githubusercontent.com/devicons/devicon/master/icons/ubuntu/ubuntu-original.svg](https://raw.githubusercontent.com/devicons/devicon/master/icons/ubuntu/ubuntu-original.svg)" alt="Ubuntu Logo" width="75" />
  <h1>⚡ Ubuntu Server Homelab & Infrastructure Logs</h1>
  <p><b>Arquitectura de Servicios Autoalojados, Enrutamiento Dinámico y Alta Disponibilidad</b></p>
  
  <p>
    <a href="#-stack-tecnológico"><img src="[https://img.shields.io/badge/Estado-Operativo-brightgreen?style=for-the-badge&logo=shield](https://img.shields.io/badge/Estado-Operativo-brightgreen?style=for-the-badge&logo=shield)" alt="Status" /></a>
    <a href="#-arquitectura-del-sistema"><img src="[https://img.shields.io/badge/Kernel-Ubuntu_Server-E95420?style=for-the-badge&logo=ubuntu&logoColor=white](https://img.shields.io/badge/Kernel-Ubuntu_Server-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)" alt="OS" /></a>
    <a href="#-bitácora-técnica-de-despliegue"><img src="[https://img.shields.io/badge/Motor-Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white](https://img.shields.io/badge/Motor-Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)" alt="Docker" /></a>
  </p>
</div>

<br>

<div align="center">
  <a href="[https://git.io/typing-svg](https://git.io/typing-svg)">
    <img src="[https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=19&pause=1000&color=E95420&center=true&vCenter=true&width=650&lines=Base%3A+Ubuntu+Server+Headless;Proxy%3A+Nginx+Proxy+Manager+%2B+SSL;Mesh+VPN%3A+Tailscale+Cross-Network;Dashboards%3A+Split+gethomepage+(Glassmorphic);Backups%3A+Kopia+Automated+Snapshot+Vault](https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=19&pause=1000&color=E95420&center=true&vCenter=true&width=650&lines=Base%3A+Ubuntu+Server+Headless;Proxy%3A+Nginx+Proxy+Manager+%2B+SSL;Mesh+VPN%3A+Tailscale+Cross-Network;Dashboards%3A+Split+gethomepage+(Glassmorphic);Backups%3A+Kopia+Automated+Snapshot+Vault)" alt="Typing SVG" />
  </a>
</div>

<br>

<h2 id="-arquitectura-del-sistema">🛠️ Arquitectura e Infraestructura</h2>

<table align="center" width="100%">
  <tr>
    <td width="50%" valign="top">
      <h3 align="center">🖥 Core Host & Virtualización</h3>
      <ul>
        <li><b>SO Base:</b> Ubuntu Server LTS (Headless).</li>
        <li><b>Contenerización:</b> Motor Docker con aislamiento de redes virtuales por entorno.</li>
        <li><b>Gestión de Contenedores:</b> Portainer CE para monitoreo de recursos y ciclo de vida.</li>
        <li><b>Resolución DNS Interna:</b> AdGuard Home como filtro DNS y reescrituras para dominios locales.</li>
      </ul>
    </td>
    <td width="50%" valign="top">
      <h3 align="center">🔐 Seguridad, Proxy & Red</h3>
      <ul>
        <li><b>Inbound Routing:</b> Nginx Proxy Manager (NPM) gestionando SSL y hosts locales.</li>
        <li><b>Acceso Remoto Mesh:</b> Tailscale enlazando nodos externos sin exponer puertos WAN.</li>
        <li><b>Respaldo de Datos:</b> Kopia Server administrando backups incrementales deduplicados.</li>
      </ul>
    </td>
  </tr>
</table>

<br>

<h2 id="-stack-tecnológico">🧰 Stack Tecnológico Activo</h2>

<div align="center">
  <img src="[https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Nginx_Proxy_Manager-009639?style=for-the-badge&logo=nginx&logoColor=white](https://img.shields.io/badge/Nginx_Proxy_Manager-009639?style=for-the-badge&logo=nginx&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Tailscale-4B5563?style=for-the-badge&logo=tailscale&logoColor=white](https://img.shields.io/badge/Tailscale-4B5563?style=for-the-badge&logo=tailscale&logoColor=white)" />
  <img src="[https://img.shields.io/badge/AdGuard_Home-00B074?style=for-the-badge&logo=adguard&logoColor=white](https://img.shields.io/badge/AdGuard_Home-00B074?style=for-the-badge&logo=adguard&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Portainer-13BEF9?style=for-the-badge&logo=portainer&logoColor=white](https://img.shields.io/badge/Portainer-13BEF9?style=for-the-badge&logo=portainer&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Nextcloud-0082C9?style=for-the-badge&logo=nextcloud&logoColor=white](https://img.shields.io/badge/Nextcloud-0082C9?style=for-the-badge&logo=nextcloud&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Vaultwarden-175DDC?style=for-the-badge&logo=bitwarden&logoColor=white](https://img.shields.io/badge/Vaultwarden-175DDC?style=for-the-badge&logo=bitwarden&logoColor=white)" />
  <img src="[https://img.shields.io/badge/NocoDB-1890FF?style=for-the-badge&logo=nocodb&logoColor=white](https://img.shields.io/badge/NocoDB-1890FF?style=for-the-badge&logo=nocodb&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Kopia-000000?style=for-the-badge&logo=kopia&logoColor=white](https://img.shields.io/badge/Kopia-000000?style=for-the-badge&logo=kopia&logoColor=white)" />
  <img src="[https://img.shields.io/badge/Minecraft_Server-388E3C?style=for-the-badge&logo=minecraft&logoColor=white](https://img.shields.io/badge/Minecraft_Server-388E3C?style=for-the-badge&logo=minecraft&logoColor=white)" />
</div>

<br>

<h2 id="-bitácora-técnica-de-despliegue">📖 Bitácora Extendida de Diagnóstico, Despliegues y Mantenimiento</h2>

> ℹ️ **Nota de Privacidad:** Todos los parámetros de red, identificadores de nodo, variables sensibles e IPs privadas han sido omitidos/anonimizados.

---

### 📍 Fase 1: Recuperación de Almacenamiento y Sistema de Archivos Read-Only

> **Incidencia:** Corrupción de permisos y estado Read-Only en la base de datos de Nginx Proxy Manager.  
> **Síntoma:** El proxy responde con errores `502 Bad Gateway` y `403 Forbidden` al acceder a los contenedores.

* **Detalle del Problema:** Durante la reorganización de volúmenes de almacenamiento, se eliminaron directorios vinculados a los contenedores activos. Esto ocasionó que el motor SQLite de Nginx Proxy Manager no pudiera escribir en sus tablas temporales, bloqueando la base de datos en estado de solo lectura.
* **Acciones de Mitigación:**
  1. Detención limpia de la pila con `docker compose down`.
  2. Reajuste de permisos sobre el árbol de volúmenes aplicando `chmod`/`chown` a las carpetas persistentes.
  3. Purga de volúmenes e interfaces con `docker network prune` y reconstrucción controlada con `docker compose up -d`.

---

### 📍 Fase 2: Arquitectura "Split Dashboard" con gethomepage & Glassmorphism

Se implementó una estrategia de paneles divididos para separar el acceso de usuario habitual de los paneles de administración del servidor.

```mermaid
graph TD
    A[Inbound Traffic] --> B[Nginx Proxy Manager]
    B --> C[Homepage USER - Vista Simplificada]
    B --> D[Homepage ADMIN - Métricas, Portainer, Kopia, NPM]
``` 
Estructura Modular:

Homepage User: Accesos directos a la nube personal (Nextcloud), gestor de contraseñas (Vaultwarden) y herramientas diarias sin exponer paneles de control.

Homepage Admin: Monitoreo en tiempo real del consumo de RAM/CPU, estado de contenedores vía Docker Socket, métricas de Kopia y accesos a Portainer.

Personalización Estética: Inyección de CSS personalizado (custom.css) para aplicar desenfoque de fondo (backdrop-filter: blur), bordes traslúcidos y tarjetas flotantes de estilo Glassmorphism/Cyberpunk.

📍 Fase 3: Depuración Interna de Upstreams y Errores de Arranque en Nginx
Bash
# Log de error detectado al arrancar el contenedor
nginx: [emerg] host not found in upstream "authelia" in /data/nginx/proxy_host/9.conf:60
Análisis de Fallo: Se mantuvo activo un archivo de proxy host (9.conf) que referenciaba a un contenedor de autenticación (authelia) desmontado. Al no poder resolver el nombre del contenedor extinto, Nginx cancelaba el proceso de arranque.

Resolución vía Shell del Contenedor:

Comprobación de sintaxis aislada dentro del contenedor:
sudo docker exec -it nginx-proxy-manager nginx -t

Mantenimiento renombrando el archivo dañado dentro del volumen interno:
sudo docker exec -it nginx-proxy-manager mv /data/nginx/proxy_host/9.conf /data/nginx/proxy_host/9.conf.disabled

Recarga en caliente del servicio de proxy:
sudo docker exec -it nginx-proxy-manager nginx -s reload

📍 Fase 4: Enrutamiento Mesh Remoto y Despliegue de Tailscale
Configuración de acceso remoto cifrado punto a punto utilizando la red mesh de Tailscale para conectar clientes desde redes externas o restringidas.

Superación de Restricciones HTTP 403 / OAuth:

En redes con proxies que interceptan la autenticación web, se resolvió conectando la máquina mediante una Auth Key inyectada desde la terminal:
sudo tailscale up --force-reauth --authkey=tskey-auth-XXXXXXXXXXXXXX

Resolución del Error nodekey already exists:

Se purgó la clave de nodo desactualizada mediante sudo tailscale logout seguido del reinicio del demonio tailscaled.

Aislamiento de Colisiones DNS:

Para evitar conflictos entre el DNS local y la navegación habitual del equipo cliente, se aplicó la bandera de aislamiento:
sudo tailscale up --accept-dns=false

📍 Fase 5: Estrategia de Copias de Seguridad y Deduplicación con Kopia
Objetivo: Backups automáticos sobre ~/homepage-split y volúmenes de aplicaciones mediante el servidor Kopia.

Limpieza de Referencias Huérfanas: Eliminación de políticas de snapshot obsoletas que apuntaban a rutas de carpetas eliminadas (homelab-apps-bak).

Verificación de Integridad: Ejecución y validación de snapshots incrementales para asegurar que los archivos .yaml y las reglas de Nginx estén a salvo ante cualquier fallo.

Bash
/home/administrador/
├── homepage-split/
│   ├── docker-compose.yml          # Stack principal de ambas instancias de Homepage
│   ├── config-user/                # Archivos .yaml y custom.css (Vista Usuario)
│   ├── config-admin/               # Archivos .yaml y monitoreo (Vista Admin)
│   └── data/nginx/                 # Volúmenes persistentes de Nginx Proxy Manager
│       └── proxy_host/             # Archivos de configuración .conf por dominio
├── adguard/                        # Persistencia del filtro DNS y reescrituras
└── kopia/                          # Almacenamiento local de snapshots y credenciales
  <p><b>Ubuntu Server Homelab</b> • <i>Infraestructura autoalojada y mantenida con Docker</i></p>
</div>
