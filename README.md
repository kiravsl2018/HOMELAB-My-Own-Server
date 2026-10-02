<div align="center">
  <h1>🚀 Ubuntu Server Homelab & Docker Logs</h1>
  <p><b>Diario de Virtualización, Redes y Glassmorphism</b></p>
  <p><i>Un ecosistema autoalojado sobre Ubuntu Server</i></p>
</div>

<hr>

<div align="center">
  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=20&pause=1000&color=E95420&center=true&vCenter=true&width=600&lines=Ubuntu+Server+Base;Docker+%2B+Nginx+Proxy+Manager;Split+gethomepage+(User+%2F+Admin);Mesh+Networking+con+Tailscale;Backups+Autom%C3%A1ticos+con+Kopia" alt="Typing SVG" />
  </a>
</div>

<br>

<table align="center" border="0" cellpadding="0" cellspacing="0" width="100%">
  <tr>
    <td width="35%" align="center" valign="center">
      <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/ubuntu/ubuntu-original.svg" alt="Ubuntu Logo" width="130" />
    </td>
    <td width="65%" valign="center">
      <h3 align="center">🛠️️ Arquitectura del Servidor</h3>
      <ul>
        <li>🐧 <b>Sistema Base:</b> Ubuntu Server (Docker Host headless).</li>
        <li>🌐 <b>Enrutamiento & Proxy:</b> Nginx Proxy Manager (NPM) + AdGuard Home DNS.</li>
        <li>🔐 <b>Red Mesh Remota:</b> Tailscale (acceso seguro sin exponer puertos WAN).</li>
        <li>🎨 <b>Dashboards:</b> Doble instancia de <code>gethomepage</code> (User / Admin) con interfaz Glassmorphism.</li>
        <li>💾 <b>Respaldos:</b> Kopia Server con snapshots programados.</li>
      </ul>
    </td>
  </tr>
</table>

<br>

<h3 align="center">🧰 Stack Tecnológico y Servicios</h3>

<div align="center">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Nginx_Proxy_Manager-009639?style=for-the-badge&logo=nginx&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailscale-4B5563?style=for-the-badge&logo=tailscale&logoColor=white" />
  <img src="https://img.shields.io/badge/AdGuard_Home-00B074?style=for-the-badge&logo=adguard&logoColor=white" />
  <img src="https://img.shields.io/badge/Portainer-13BEF9?style=for-the-badge&logo=portainer&logoColor=white" />
  <img src="https://img.shields.io/badge/Nextcloud-0082C9?style=for-the-badge&logo=nextcloud&logoColor=white" />
  <img src="https://img.shields.io/badge/Vaultwarden-175DDC?style=for-the-badge&logo=bitwarden&logoColor=white" />
  <img src="https://img.shields.io/badge/Minecraft_Server-388E3C?style=for-the-badge&logo=minecraft&logoColor=white" />
</div>

<br>

<h2 align="center">📖 Bitácora de Despliegue y Resolución de Incidencias</h2>

<table align="center" border="0" cellpadding="0" cellspacing="0" width="100%">
  <tr>
    <td>
      <h3>1. Arreglo de Permisos y Sistema de Archivos en Modo Lectura</h3>
      <p>Tras la eliminación accidental de ciertos directorios que alojaban volúmenes de almacenamiento, el motor de bases de datos de Nginx Proxy Manager entró en modo <code>read-only</code>, provocando caídas en los servicios web.</p>
      <ul>
        <li><b>Solución:</b> Se identificaron los volúmenes afectos, se ajustaron los permisos de sistema con <code>chmod</code>/<code>chown</code> y se reestructuró la pila de red interna de Docker reinstaurando la persistencia de datos.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <hr>
      <h3>2. Despliegue Dual de Homepage (Glassmorphism / Cyberpunk)</h3>
      <p>Se diseñó una arquitectura split para la gestión visual del servidor, separando la vista pública/usuario de la vista de administración avanzada.</p>
      <ul>
        <li><b>Homepage User:</b> Panel simplificado para servicios diarios (Nextcloud, Vaultwarden, etc.).</li>
        <li><b>Homepage Admin:</b> Control integral del sistema (Portainer, Kopia, Nginx PM, métricas de host).</li>
        <li><b>Personalización:</b> Implementación de estilos vía <code>custom.css</code> con estética de cristal (Glassmorphism) e inyección de iconos SVG.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <hr>
      <h3>3. Limpieza de Upstreams en Nginx Proxy Manager</h3>
      <p>Al recargar las reglas de Nginx se generó un bloqueo crítico en el arranque debido al archivo de configuración <code>9.conf</code>:</p>
      <pre><code>nginx: [emerg] host not found in upstream "authelia" in /data/nginx/proxy_host/9.conf:60</code></pre>
      <ul>
        <li><b>Resolución:</b> Al estar huérfana la referencia a Authelia, se accedió a la estructura interna del contenedor de NPM para deshabilitar la regla conflictiva mediante el aislamiento del archivo <code>9.conf</code>, devolviendo la estabilidad al proxy inverso.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <hr>
      <h3>4. Integración de Red Mesh con Tailscale</h3>
      <p>Configuración de acceso remoto seguro para conectar clientes situados tras redes externas restringidas.</p>
      <ul>
        <li><b>Superación de bloqueos y Firewall:</b> Se implementó la autenticación mediante <b>Auth Keys</b> de Tailscale y la re-autenticación (<code>--force-reauth</code>).</li>
        <li><b>Resolución de conflictos de ruta y DNS:</b> Uso de la bandera <code>--accept-dns=false</code> para resolver colisiones entre los servidores DNS locales y la navegación general de las redes cliente.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <hr>
      <h3>5. Políticas de Copia de Seguridad con Kopia</h3>
      <p>Estructuración de políticas de respaldos automatizados e incrementales alojados en el servidor Kopia.</p>
      <ul>
        <li>Limpieza de snapshots huérfanos que apuntaban a carpetas borradas.</li>
        <li>Validación de backups manuales y programados sobre la carpeta de configuración activa.</li>
      </ul>
    </td>
  </tr>
</table>

<br>

<h3 align="center">📂 Estructura de Directorios</h3>

<table align="center" border="0" cellpadding="0" cellspacing="0" width="90%">
  <tr>
    <td>
      <pre>
<code>homelab-root/
├── homepage-split/
│   ├── docker-compose.yml       # Orquestación de Homepage User/Admin
│   ├── config-user/             # Configuración .yaml y custom.css de usuario
│   ├── config-admin/            # Configuración .yaml de administración
│   └── data/nginx/              # Volúmenes mapeados de Nginx Proxy Manager
├── adguard/                     # Persistencia y filtros de DNS local
└── kopia/                       # Repositorio y configuraciones de snapshot</code>
      </pre>
    </td>
  </tr>
</table>

<br>

<hr>
<div align="center">
  <p><i>Servidor privado y autoalojado sobre Ubuntu Server</i></p>
</div>
