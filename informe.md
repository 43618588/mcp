# INFORME TÉCNICO DE ANÁLISIS DE SISTEMA ERP-CRM

**Fecha de análisis:** 27 de enero de 2026, 12:06 - 13:01 UTC  
**Analista:** Sistema de Análisis ERP-CRM  
**Servidor:** ubuntuserver2404  

---

## 1. IDENTIFICACIÓN DEL SISTEMA

### Sistema Operativo
- **Distribución:** Ubuntu 24.04.3 LTS
  > "Operating System: Ubuntu 24.04.3 LTS"
- **Kernel:** Linux 6.8.0-90-generic
  > "Kernel: Linux 6.8.0-90-generic"
- **Arquitectura:** x86-64
  > "Architecture: x86-64"
- **Virtualización:** Oracle VirtualBox
  > "Virtualization: oracle"
  > "Hardware Model: VirtualBox"
- **Machine ID:** 3d0d49febd914e9185a6d449356a4d99
  > "Machine ID: 3d0d49febd914e9185a6d449356a4d99"

### CPU
- **Modelo:** Intel Core i5-1135G7 (11ª Gen) @ 2.40GHz
  > "Model name: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz"
- **Núcleos físicos:** 4 cores, 1 thread por core
  > "CPU(s): 4"
  > "Thread(s) per core: 1"
- **Sockets:** 1
  > "Socket(s): 1"
- **Caché:** L1d: 192 KiB, L1i: 128 KiB, L2: 5 MiB, L3: 32 MiB
  > "L1d: 192 KiB (4 instances)"
  > "L3: 32 MiB (4 instances)"

### Memoria RAM
- **Memoria Total:** 10.81 GiB (11071.3 MiB)
  > "MiB Mem : 11071.3 total"
- **Memoria Libre:** 8.5 GiB (aproximadamente)
  > "8751.9 free"
- **Memoria en Uso:** 1.1 GiB
  > "1118.2 used"
- **Buffer/Cache:** 1.5 GiB
  > "1585.6 buff/cache"

### SWAP
- **Estado inicial:** Sin SWAP configurado
  > "MiB Swap: 0.0 total, 0.0 free, 0.0 used"
- **Estado final:** SWAP de 2 GB configurado durante la sesión
  > "MiB Swap: 2048.0 total, 2048.0 free, 0.0 used"
  > "NNAME      TYPE SIZE USED PRIO"
  > "/swapfile file   2G   0B   -2"

### Disco
- **Dispositivo:** /dev/sda2 (ext4)
  > "NAME   FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS"
  > "sda2 ext4   1.0         090cf59a-db56-4f38-8dca-1671a33f7b14   85.4G     8% /"
- **Capacidad Total:** 98 GB
  > "Filesystem     Type   Size  Used Avail Use% Mounted on"
  > "/dev/sda2      ext4    98G  7.6G   86G   9% /"
- **Espacio Usado:** 7.6 GB (9%)
  > "/dev/sda2      ext4    98G  7.6G   86G   9% /"
- **Espacio Disponible:** 86 GB
  > "86G   9% /"
- **Journald:** 180.1 MB
  > "Archived and active journals take up 180.1M in the file system."

### Red
- **Interfaz Principal:** enp0s3
  > "2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500"
- **Dirección IP:** 192.168.1.137/24
  > "inet 192.168.1.137/24 metric 100"
- **Gateway:** 192.168.1.1
  > "default via 192.168.1.1 dev enp0s3"
- **MAC Address:** 08:00:27:92:84:e0
  > "link/ether 08:00:27:92:84:e0"
- **MTU:** 1500
  > "mtu 1500"
- **Docker Networks:**
  - docker0: 172.17.0.1/16 (DOWN)
    > "3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1400"
  - br-278bb6a8fb3b: 172.18.0.1/16 (UP)
    > "4: br-278bb6a8fb3b: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500"

---

## 2. ANÁLISIS DE RENDIMIENTO

### a) CPU (Load Average vs Cores)

**Carga del sistema:**
> "load average: 0.06, 0.25, 0.18"

**Análisis:**
- Load average de 1 minuto: 0.06
- Load average de 5 minutos: 0.25  
- Load average de 15 minutos: 0.18
- Sistema con 4 cores disponibles
  > "CPU(s): 4"

**Interpretación:** El sistema presenta una carga extremadamente baja. Con 4 núcleos disponibles y un load average máximo de 0.25, el sistema está utilizando apenas el 6.25% de su capacidad de procesamiento. Esto indica un sistema muy holgado en términos de CPU.

**Uso de CPU por estado:**
> "%Cpu(s):  0.0 us,  0.4 sy,  0.0 ni, 99.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st"

- User space: 0.0%
- System: 0.4%
- Idle: 99.6%
- Wait I/O: 0.0%

### b) Memoria y SWAP

**Estado inicial de memoria:**
> "MiB Mem :  11071.3 total,   8751.9 free,   1118.2 used,   1585.6 buff/cache"
> "MiB Swap:      0.0 total,      0.0 free,      0.0 used.   9953.0 avail Mem"

**Análisis:**
- Memoria total: 10.81 GiB
- Memoria libre: 8.5 GiB (79%)
- Memoria en uso: 1.1 GiB (10%)
- Memoria disponible real: 9.7 GiB (89%)
- **CRÍTICO:** Sistema operando SIN SWAP inicialmente

**Acción correctiva tomada durante la sesión:**
> "sudo fallocate -l 2G /swapfile"
> "Setting up swapspace version 1, size = 2 GiB (2147479552 bytes)"
> "NAME      TYPE SIZE USED PRIO"
> "/swapfile file   2G   0B   -2"

**Estado final de memoria:**
> "MiB Mem :  11071.3 total,   8756.3 free,   1107.6 used,   1593.0 buff/cache"
> "MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   9963.7 avail Mem"

**Parámetro vm.swappiness modificado:**
> "vm.swappiness = 60" (valor inicial)
> "vm.swappiness = 10" (valor configurado)

### c) Disco y Riesgo de Saturación

**Capacidad del sistema de archivos:**
> "/dev/sda2        98G  7.6G   86G   9% /"

**Análisis:**
- Espacio usado: 7.6 GB (9%)
- Espacio disponible: 86 GB (88%)
- **Riesgo de saturación: BAJO**

**Docker disk usage:**
> "TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE"
> "Images          2         2         3.506GB   3.506GB (100%)"
> "Containers      2         2         28.67kB   0B (0%)"
> "Local Volumes   3         3         293.1MB   0B (0%)"

**Análisis:**
- Imágenes Docker: 3.5 GB (con 100% reclaimable si se eliminan)
- Volúmenes: 293 MB
- Total Docker: ~3.8 GB del espacio usado total

---

## 3. PROCESOS CRÍTICOS DETECTADOS

### Procesos del Host

**Por consumo de CPU:**
> "USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND"
> "dhcpcd      1312  1.1  4.0 629636 458468 ?       Ssl  08:50   2:54 /usr/bin/python3 /usr/bin/odoo"
> "root        1271  1.1  0.0 1233584 11008 ?       Sl   08:50   2:44 /usr/bin/containerd-shim-runc-v2"
> "root        1265  0.5  0.0 1233840 11292 ?       Sl   08:50   1:27 /usr/bin/containerd-shim-runc-v2"
> "root         929  0.2  0.7 2573732 90168 ?       Ssl  08:50   0:40 /usr/bin/dockerd"
> "root         715  0.2  0.4 1941828 52432 ?       Ssl  08:50   0:39 /usr/bin/containerd"

**Por consumo de memoria:**
> "dhcpcd      1312  1.1  4.0 629636 458468 ?       Ssl  08:50   2:54 /usr/bin/python3 /usr/bin/odoo"
> "root         929  0.2  0.7 2573732 90168 ?       Ssl  08:50   0:40 /usr/bin/dockerd"
> "70          1559  0.0  0.7 183304 81888 ?        Ss   08:50   0:06 postgres: odoo odoo"

**Análisis:**
- **Proceso Odoo (PID 1312):** Mayor consumidor de recursos (4% RAM, 458 MB)
- **Dockerd (PID 929):** Segunda posición (0.7% CPU, 90 MB RAM)
- **PostgreSQL (PID 1559):** Base de datos con 81 MB RAM
- **Containerd-shim:** Dos instancias ejecutándose (uno por contenedor)

### Procesos Docker

**Contenedores activos:**
> "NAMES                   IMAGE                STATUS                 PORTS"
> "odoo-dev-E1UF1884       odoo:18.0            Up 3 hours (healthy)   0.0.0.0:8069->8069/tcp"
> "postgres-dev-E1UF1884   postgres:16-alpine   Up 3 hours (healthy)   0.0.0.0:5432->5432/tcp"

**Estadísticas de recursos:**
> "CONTAINER ID   NAME                    CPU %     MEM USAGE / LIMIT     MEM %"
> "a1df601f601a   odoo-dev-E1UF1884       0.02%     570.1MiB / 10.81GiB   5.15%"
> "bea5221f99d1   postgres-dev-E1UF1884   0.00%     231.4MiB / 10.81GiB   2.09%"

**Análisis:**
- **Odoo:** Consumo de 570 MB (5.15% del total)
- **PostgreSQL:** Consumo de 231 MB (2.09% del total)
- Ambos contenedores en estado "healthy"
- Total consumo Docker: ~800 MB (7.24% de RAM total)

**Puertos expuestos:**
> "tcp   LISTEN 0      4096                0.0.0.0:5432      0.0.0.0:*"
> "tcp   LISTEN 0      4096                0.0.0.0:8069      0.0.0.0:*"

- Puerto 5432: PostgreSQL (expuesto públicamente - RIESGO DE SEGURIDAD)
- Puerto 8069: Odoo (expuesto públicamente)
- Puerto 22: SSH (expuesto)

---

## 4. EVENTOS RELEVANTES DEL SISTEMA

### Eventos de Configuración

**1. Configuración de SWAP:**
> "sudo fallocate -l 2G /swapfile"
> "sudo mkswap /swapfile"
> "sudo swapon /swapfile"
> "echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab"

**Justificación:** El sistema no tenía SWAP configurado, lo que representa un riesgo crítico para la estabilidad ante picos de memoria.

**2. Ajuste de vm.swappiness:**
> "sudo sysctl -w vm.swappiness=10"
> "echo "vm.swappiness=10" | sudo tee /etc/sysctl.d/99-erp.conf"

**Justificación:** Reducir el uso de SWAP de 60 (valor por defecto) a 10 para optimizar rendimiento en sistemas con suficiente RAM.

**3. Aplicación de configuración del sistema:**
> "sudo sysctl --system | tail -n 20"
> "vm.swappiness = 10"

**Estado:** Confirmación de que la configuración se aplicó correctamente y persistirá tras reinicios.

### Eventos de Red

**Conectividad externa verificada:**
> "ping -c 4 8.8.8.8"
> "4 packets transmitted, 4 received, 0% packet loss, time 3073ms"
> "rtt min/avg/max/mdev = 28.088/29.935/34.041/2.396 ms"

**Análisis:**
- Conectividad estable
- Latencia promedio: 29.9 ms
- Sin pérdida de paquetes

### Eventos de Sistema de Archivos

**Uso de inodos:**
> "fs.file-max = 9223372036854775807"
> "ulimit -n"
> "1024"

**Análisis:** 
- Límite de archivos abiertos por usuario: 1024 (bajo para aplicaciones enterprise)
- Límite de sistema: prácticamente ilimitado

---

## 5. DIAGNÓSTICO TÉCNICO RAZONADO

### Fortalezas del Sistema

1. **Recursos de hardware adecuados:** 
   - El sistema cuenta con 10.81 GB de RAM, de los cuales solo se está utilizando aproximadamente el 10%, lo que evidencia que: `"MiB Mem :  11071.3 total,   8751.9 free,   1118.2 used"`. Esto proporciona un margen de crecimiento del 700-800% antes de alcanzar limitaciones de memoria.

2. **Carga de CPU muy baja:**
   - Con un load average de 0.06-0.25 en un sistema de 4 cores: `"load average: 0.06, 0.25, 0.18"`, el sistema está operando al 1.5-6.25% de capacidad. Esto indica que puede manejar cargas de trabajo 15-40 veces superiores sin degradación de rendimiento.

3. **Espacio en disco disponible:**
   - Con 86 GB libres de 98 GB totales: `"/dev/sda2        98G  7.6G   86G   9% /"`, el sistema tiene espacio para crecimiento de datos por varios años en el escenario de uso actual.

4. **Contenedores saludables:**
   - Ambos servicios críticos (Odoo y PostgreSQL) están en estado healthy: `"Up 3 hours (healthy)"`, indicando estabilidad operacional.

### Debilidades y Riesgos Identificados

1. **AUSENCIA DE SWAP (CRÍTICO - CORREGIDO):**
   - **Estado inicial:** `"MiB Swap:      0.0 total,      0.0 free,      0.0 used"`
   - **Riesgo:** Un sistema sin SWAP puede causar terminación abrupta de procesos (OOM Killer) cuando la memoria se agota, incluso temporalmente. En un entorno de producción ERP-CRM, esto significa pérdida de transacciones y posible corrupción de datos.
   - **Corrección aplicada:** Se configuró un archivo de SWAP de 2 GB y se hizo persistente.

2. **EXPOSICIÓN DE BASE DE DATOS (CRÍTICO):**
   - PostgreSQL está escuchando en todas las interfaces: `"tcp   LISTEN 0      4096                0.0.0.0:5432      0.0.0.0:*"`
   - **Riesgo:** Esto expone la base de datos a toda la red, creando un vector de ataque crítico. Las bases de datos solo deben ser accesibles desde el host local o mediante VPN/firewall.

3. **Límites de archivos abiertos insuficientes:**
   - Límite actual de 1024: `"ulimit -n" → "1024"`
   - **Riesgo:** Para un sistema ERP-CRM con múltiples usuarios concurrentes, este límite puede causar errores "too many open files", especialmente durante operaciones de reporting o importación masiva.

4. **Parámetros de kernel no optimizados:**
   - vm.swappiness inicial en 60: `"vm.swappiness = 60"`
   - **Riesgo:** Un valor alto causa uso excesivo de SWAP incluso con RAM disponible, degradando el rendimiento. El valor de 10 configurado es más apropiado.

5. **Falta de balanceo de carga / alta disponibilidad:**
   - Solo una instancia de cada servicio ejecutándose
   - **Riesgo:** No hay redundancia. Una falla en el contenedor de Odoo o PostgreSQL causa indisponibilidad total del sistema.

6. **Logs de sistema sin rotación configurada:**
   - Journald usando 180.1 MB: `"Archived and active journals take up 180.1M"`
   - **Riesgo:** Sin límites de rotación, los logs pueden crecer indefinidamente y consumir espacio crítico.

### Análisis de Rendimiento

**CPU:**
- Utilización actual: 0.4-2.3% durante picos
- Capacidad disponible: 97-99%
- **Conclusión:** CPU no es un cuello de botella ni lo será en el futuro cercano.

**Memoria:**
- Utilización: 1.1 GB de 10.81 GB (10%)
- Contenedores usando: 801 MB
- **Conclusión:** Memoria extremadamente holgada. Puede soportar 8-10 veces más carga sin problemas.

**Disco:**
- I/O wait: 0.0%: `"0.0 wa"`
- Uso de espacio: 9%
- **Conclusión:** Sin presión de I/O. El almacenamiento no es limitante.

**Red:**
- Latencia a internet: 29.9 ms
- Sin pérdida de paquetes: `"0% packet loss"`
- **Conclusión:** Conectividad estable y adecuada.

### Proyección de Capacidad

Basándome en el uso actual de recursos:
- **Usuarios soportados actualmente:** Estimado 5-10 usuarios concurrentes
- **Capacidad potencial sin mejoras:** 40-80 usuarios concurrentes (basado en RAM y CPU disponibles)
- **Limitante actual:** Configuración de aplicación y base de datos, no hardware

---

## 6. CINCO ACCIONES RECOMENDADAS

### Acción 1: Aislar PostgreSQL de la red externa

**Qué hacer:**
Modificar el archivo de configuración de Docker Compose para que PostgreSQL solo escuche en la red interna de Docker, eliminando el mapeo de puertos al host.

```yaml
# Cambiar de:
ports:
  - "0.0.0.0:5432:5432"
# A:
expose:
  - "5432"
# O específicamente a localhost:
ports:
  - "127.0.0.1:5432:5432"
```

**Por qué:**
La exposición de la base de datos a todas las interfaces de red (`"tcp   LISTEN 0      4096                0.0.0.0:5432      0.0.0.0:*"`) constituye el riesgo de seguridad más crítico identificado. PostgreSQL contiene todos los datos del sistema ERP-CRM, incluyendo información financiera, de clientes, credenciales, y datos sensibles de negocio. Exponer este servicio públicamente facilita ataques de:
- Fuerza bruta contra credenciales
- Explotación de vulnerabilidades conocidas
- Exfiltración de datos
- Ransomware/cifrado de base de datos

**Riesgo de no implementarlo:**
- **Severidad:** CRÍTICA
- **Probabilidad:** ALTA (bases de datos expuestas son escaneadas constantemente por bots)
- **Impacto:** Pérdida total de datos, incumplimiento normativo (GDPR, LOPD), responsabilidad legal, pérdida de confianza de clientes, posible quiebra del negocio.
- **Tiempo estimado hasta incidente:** 24-72 horas en promedio para sistemas expuestos en internet.

**Impacto de la implementación:**
- **Positivo:** Eliminación del 80% del riesgo de seguridad identificado. Cumplimiento con estándares de seguridad básicos.
- **Negativo:** Acceso remoto a la base de datos requerirá SSH tunneling o VPN (esto es deseable desde el punto de vista de seguridad).
- **Tiempo de implementación:** 15 minutos
- **Requisitos de downtime:** 2-3 minutos para reinicio de contenedores

### Acción 2: Aumentar límites de archivos abiertos (ulimit)

**Qué hacer:**
Configurar límites más altos para archivos abiertos tanto a nivel de sistema como para el usuario que ejecuta los servicios:

```bash
# Editar /etc/security/limits.conf
echo "* soft nofile 65535" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 65535" | sudo tee -a /etc/security/limits.conf

# Editar /etc/sysctl.conf
echo "fs.file-max = 2097152" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# Para Docker, agregar en docker-compose.yml:
ulimits:
  nofile:
    soft: 65535
    hard: 65535
```

**Por qué:**
El límite actual de 1024 archivos abiertos (`"ulimit -n" → "1024"`) es insuficiente para un sistema ERP-CRM en producción. Un sistema Odoo moderno con 20-30 usuarios concurrentes puede fácilmente requerir:
- 100-200 conexiones de base de datos
- 50-100 archivos de sesión
- 200-300 archivos de módulos cargados
- 50-100 archivos temporales
- 100-200 sockets de red

Cuando se alcanza este límite, los usuarios experimentan:
- Errores "Database connection failed"
- Timeouts en operaciones de guardado
- Fallos al generar reportes PDF
- Imposibilidad de crear nuevos documentos

**Riesgo de no implementarlo:**
- **Severidad:** ALTA
- **Probabilidad:** MEDIA (se manifestará cuando haya 15-20 usuarios concurrentes o durante operaciones batch)
- **Impacto:** Interrupciones de servicio intermitentes, pérdida de productividad, frustración de usuarios, imposibilidad de escalar el sistema.
- **Escenarios específicos:** Falla al generar reportes mensuales, errores durante cierres de mes, imposibilidad de importar grandes lotes de datos.

**Impacto de la implementación:**
- **Positivo:** Eliminación de un cuello de botella futuro, permitiendo escalar a 50-100 usuarios concurrentes sin problemas de descriptores de archivo.
- **Negativo:** Incremento mínimo en el uso de memoria kernel (aproximadamente 5-10 MB).
- **Tiempo de implementación:** 20 minutos
- **Requisitos de downtime:** Reinicio completo del sistema requerido (planificar en ventana de mantenimiento)

### Acción 3: Configurar rotación de logs y monitoreo de espacio

**Qué hacer:**
Implementar rotación automática de logs para journald y Docker, y configurar alertas de espacio en disco:

```bash
# Configurar journald
sudo mkdir -p /etc/systemd/journald.conf.d/
cat <<EOF | sudo tee /etc/systemd/journald.conf.d/size-limit.conf
[Journal]
SystemMaxUse=500M
SystemMaxFileSize=100M
MaxRetentionSec=30day
EOF
sudo systemctl restart systemd-journald

# Configurar rotación de logs de Docker
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "5"
  }
}
EOF
sudo systemctl restart docker

# Script de monitoreo de disco
cat <<'EOF' | sudo tee /usr/local/bin/disk-monitor.sh
#!/bin/bash
THRESHOLD=80
USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ $USAGE -gt $THRESHOLD ]; then
    echo "ALERT: Disk usage at ${USAGE}% on $(date)" | \
    logger -t disk-monitor -p user.warning
fi
EOF
sudo chmod +x /usr/local/bin/disk-monitor.sh

# Cron job cada hora
echo "0 * * * * /usr/local/bin/disk-monitor.sh" | sudo crontab -
```

**Por qué:**
Aunque actualmente el sistema solo usa 9% del disco (`"/dev/sda2        98G  7.6G   86G   9% /"`), y los logs ocupan 180 MB (`"Archived and active journals take up 180.1M"`), los logs crecen exponencialmente con el uso. Un sistema ERP-CRM en producción genera:
- Logs de aplicación: 50-100 MB/día
- Logs de base de datos: 20-50 MB/día
- Logs de sistema: 10-20 MB/día
- Logs de Docker: 30-60 MB/día

Sin rotación, en 6-12 meses los logs pueden consumir 30-50 GB, y en caso de errores o depuración intensiva, pueden llenar el disco en semanas. Un disco lleno causa:
- Corrupción de base de datos (PostgreSQL no puede escribir WAL logs)
- Falla de servicios al no poder escribir logs
- Imposibilidad de hacer backups
- Pérdida de datos de sesiones activas

**Riesgo de no implementarlo:**
- **Severidad:** ALTA
- **Probabilidad:** MEDIA-ALTA (aumenta con el tiempo)
- **Impacto:** Corrupción de datos, caída del sistema, necesidad de intervención manual de emergencia, pérdida de información de auditoría.
- **Timeline:** El riesgo aumenta significativamente después de 6 meses de operación sin rotación.

**Impacto de la implementación:**
- **Positivo:** Prevención de saturación de disco, mejora en la visibilidad de problemas históricos (logs mantienen información relevante pero no infinita), cumplimiento con políticas de retención de datos.
- **Negativo:** Pérdida de logs de más de 30 días (esto es generalmente aceptable y deseable).
- **Tiempo de implementación:** 45 minutos
- **Requisitos de downtime:** Reinicio de servicios individuales (journald, docker) - 5 minutos total

### Acción 4: Implementar backups automatizados

**Qué hacer:**
Configurar un sistema de backups automáticos diarios para la base de datos PostgreSQL y archivos de configuración:

```bash
# Crear directorio de backups
sudo mkdir -p /var/backups/postgresql
sudo mkdir -p /var/backups/odoo-data

# Script de backup
cat <<'EOF' | sudo tee /usr/local/bin/backup-erp.sh
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/var/backups"
RETENTION_DAYS=7

# Backup PostgreSQL
docker exec postgres-dev-E1UF1884 pg_dump -U odoo odoo | \
gzip > ${BACKUP_DIR}/postgresql/odoo_${DATE}.sql.gz

# Backup de volúmenes de Odoo (filestore)
docker run --rm -v odoo18_odoo-data:/data -v ${BACKUP_DIR}/odoo-data:/backup \
alpine tar czf /backup/odoo-data_${DATE}.tar.gz /data

# Limpiar backups antiguos
find ${BACKUP_DIR}/postgresql -name "*.sql.gz" -mtime +${RETENTION_DAYS} -delete
find ${BACKUP_DIR}/odoo-data -name "*.tar.gz" -mtime +${RETENTION_DAYS} -delete

# Verificar backup
if [ -f "${BACKUP_DIR}/postgresql/odoo_${DATE}.sql.gz" ]; then
    logger -t backup-erp "Backup completed successfully: odoo_${DATE}.sql.gz"
else
    logger -t backup-erp -p user.error "Backup FAILED"
fi
EOF
sudo chmod +x /usr/local/bin/backup-erp.sh

# Cron job a las 2 AM diariamente
echo "0 2 * * * /usr/local/bin/backup-erp.sh" | sudo crontab -
```

**Por qué:**
Actualmente NO existe evidencia de backups configurados en el sistema. Un sistema ERP-CRM contiene datos críticos de negocio:
- Facturas y transacciones financieras
- Información de clientes (CRM)
- Órdenes de compra y venta
- Inventario
- Nóminas y datos de empleados
- Configuración de workflows empresariales

La pérdida de estos datos debido a:
- Falla de hardware (el sistema corre en VirtualBox sin redundancia)
- Corrupción de base de datos
- Ransomware
- Error humano (DELETE sin WHERE)
- Desastre físico (incendio, inundación)

...resulta en pérdida catastrófica que puede ser IRRECUPERABLE sin backups.

**Riesgo de no implementarlo:**
- **Severidad:** CRÍTICA
- **Probabilidad:** BAJA a corto plazo, pero INEVITABLE a largo plazo (tasa de falla de discos: 1-5% anual, errores humanos: probabilidad acumulada del 90% en 5 años)
- **Impacto:** 
  - Pérdida total de datos históricos
  - Imposibilidad de reconstruir operaciones
  - Incumplimiento legal (obligación de retención de facturas 4-7 años según jurisdicción)
  - Demandas por pérdida de datos de clientes
  - Posible cierre del negocio
- **Tiempo de recuperación sin backups:** NUNCA (pérdida permanente)
- **Costo sin backups:** Potencialmente todo el valor del negocio

**Impacto de la implementación:**
- **Positivo:** 
  - Capacidad de recuperación ante desastres (RPO: 24 horas)
  - Tranquilidad operacional
  - Capacidad de revertir cambios problemáticos
  - Cumplimiento normativo
  - Posibilidad de restaurar datos eliminados accidentalmente
- **Negativo:** 
  - Uso de espacio: 2-5 GB por backup (con 7 días de retención: 14-35 GB)
  - Uso de I/O durante backup (impacto mínimo a las 2 AM)
- **Tiempo de implementación:** 1 hora
- **Requisitos de downtime:** Ninguno

### Acción 5: Configurar monitoreo y alertas básicas

**Qué hacer:**
Implementar un sistema de monitoreo ligero usando scripts y cron para detectar problemas proactivamente:

```bash
# Script de health check
cat <<'EOF' | sudo tee /usr/local/bin/health-check.sh
#!/bin/bash
LOGFILE="/var/log/health-check.log"
ALERT_EMAIL="admin@example.com"  # Configurar

echo "=== Health Check $(date) ===" >> $LOGFILE

# 1. Verificar que los contenedores están corriendo
if ! docker ps | grep -q odoo-dev-E1UF1884; then
    echo "ALERT: Odoo container is down" | tee -a $LOGFILE
fi

if ! docker ps | grep -q postgres-dev-E1UF1884; then
    echo "ALERT: PostgreSQL container is down" | tee -a $LOGFILE
fi

# 2. Verificar uso de memoria
MEM_USAGE=$(free | grep Mem | awk '{print ($3/$2) * 100.0}')
if (( $(echo "$MEM_USAGE > 85" | bc -l) )); then
    echo "ALERT: Memory usage high: ${MEM_USAGE}%" | tee -a $LOGFILE
fi

# 3. Verificar uso de disco
DISK_USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ $DISK_USAGE -gt 85 ]; then
    echo "ALERT: Disk usage high: ${DISK_USAGE}%" | tee -a $LOGFILE
fi

# 4. Verificar load average
LOAD=$(uptime | awk -F'load average:' '{print $2}' | awk '{print $1}' | sed 's/,//')
if (( $(echo "$LOAD > 3.0" | bc -l) )); then
    echo "ALERT: High load average: $LOAD" | tee -a $LOGFILE
fi

# 5. Verificar conectividad de Odoo
if ! curl -s -o /dev/null -w "%{http_code}" http://localhost:8069 | grep -q "200\|303"; then
    echo "ALERT: Odoo web interface not responding" | tee -a $LOGFILE
fi

# 6. Verificar espacio de SWAP
SWAP_USAGE=$(free | grep Swap | awk '{if ($2 > 0) print ($3/$2) * 100.0; else print 0}')
if (( $(echo "$SWAP_USAGE > 50" | bc -l) )); then
    echo "WARNING: SWAP usage high: ${SWAP_USAGE}% (might indicate memory pressure)" | \
    tee -a $LOGFILE
fi

echo "Health check completed" >> $LOGFILE
EOF
sudo chmod +x /usr/local/bin/health-check.sh

# Cron job cada 15 minutos
echo "*/15 * * * * /usr/local/bin/health-check.sh" | sudo crontab -

# Script de reporte diario
cat <<'EOF' | sudo tee /usr/local/bin/daily-report.sh
#!/bin/bash
{
echo "=== Daily System Report $(date) ==="
echo ""
echo "=== System Uptime ==="
uptime
echo ""
echo "=== Memory Usage ==="
free -h
echo ""
echo "=== Disk Usage ==="
df -h /
echo ""
echo "=== Docker Container Status ==="
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
echo ""
echo "=== Docker Resource Usage ==="
docker stats --no-stream
echo ""
echo "=== Top 5 Processes by Memory ==="
ps aux --sort=-%mem | head -6
echo ""
} | tee /var/log/daily-report-$(date +%Y%m%d).log
EOF
sudo chmod +x /usr/local/bin/daily-report.sh

# Cron job a las 9 AM diariamente
echo "0 9 * * * /usr/local/bin/daily-report.sh" | sudo crontab -
```

**Por qué:**
Actualmente el sistema no tiene monitoreo configurado. Los problemas solo se detectan cuando:
- Los usuarios reportan que el sistema "no funciona"
- La aplicación se ha caído completamente
- Los datos ya se han perdido o corrompido

Esta es una postura reactiva que resulta en:
- Tiempo de inactividad prolongado (1-4 horas mientras se diagnostica)
- Pérdida de productividad de todos los usuarios
- Imposibilidad de prevenir problemas
- Degradación gradual del servicio sin ser detectada

Con monitoreo, los problemas se detectan proactivamente:
- Memoria llegando al límite: acción antes del OOM killer
- Disco llenándose: limpieza antes de corrupción
- Servicios caídos: reinicio automático
- Degradación de rendimiento: optimización preventiva

**Riesgo de no implementarlo:**
- **Severidad:** MEDIA-ALTA
- **Probabilidad:** ALTA (los problemas ocurrirán, la pregunta es si los detectamos a tiempo)
- **Impacto:** 
  - Tiempos de inactividad no planificados de 2-8 horas
  - Pérdida de productividad masiva (todos los usuarios afectados simultáneamente)
  - Costos de oportunidad (ventas no realizadas, pedidos no procesados)
  - Estrés y carga de trabajo de emergencia para el equipo técnico
  - Desgaste de la confianza en la infraestructura
- **Costo promedio de downtime:** $5,600 por hora para una pequeña empresa (estimación)

**Impacto de la implementación:**
- **Positivo:**
  - Detección proactiva de problemas (MTTR reducido en 70-90%)
  - Visibilidad de tendencias y patrones
  - Capacidad de planificación de capacidad basada en datos
  - Auditoría de disponibilidad (SLA tracking)
  - Reducción de estrés operacional
- **Negativo:**
  - Uso mínimo de CPU (< 0.1%) cada 15 minutos
  - Espacio de logs: 10-20 MB/mes
- **Tiempo de implementación:** 1.5 horas
- **Requisitos de downtime:** Ninguno

---

## 7. ACCIONES QUE NO SE RECOMIENDAN EN PRODUCCIÓN

### Acción No Recomendada 1: Eliminar el archivo de SWAP creado

**Por qué NO hacerlo:**
El SWAP es esencial para la estabilidad del sistema. Aunque actualmente no se está utilizando (`"MiB Swap:   2048.0 total,   2048.0 free,      0.0 used"`), proporciona:

1. **Protección contra picos de memoria:** En momentos de alta concurrencia (cierre de mes, generación de reportes masivos, importaciones grandes), el uso de memoria puede aumentar temporalmente. Sin SWAP, el sistema mataría procesos aleatoriamente.

2. **Espacio para páginas inactivas:** Permite que el kernel mueva páginas de memoria raramente usadas al SWAP, liberando RAM para caché de I/O, lo que mejora el rendimiento general.

3. **Prevención de OOM (Out of Memory) killer:** Sin SWAP, cuando la RAM se agota, el kernel mata procesos. Con SWAP, el sistema se degrada gradualmente (se vuelve más lento) pero permanece operativo, dando tiempo para intervención manual.

**Evidencia del problema sin SWAP:**
Inicialmente el sistema no tenía SWAP configurado (`"MiB Swap:      0.0 total"`), lo que motivó la acción correctiva de crear uno.

**Cita relevante del log:**
> "cat /proc/swaps"
> "Filename				Type		Size		Used		Priority"
> (sin entradas inicialmente, luego)
> "NAME      TYPE SIZE USED PRIO"
> "/swapfile file   2G   0B   -2"

### Acción No Recomendada 2: Aumentar vm.swappiness por encima de 10

**Por qué NO hacerlo:**
Durante la sesión se modificó vm.swappiness de 60 a 10:
> "vm.swappiness = 60" (valor inicial)
> "sudo sysctl -w vm.swappiness=10"
> "vm.swappiness = 10" (valor final)

Aumentar este valor por encima de 10 causaría que el kernel utilice SWAP agresivamente incluso cuando hay RAM disponible. Esto resulta en:

1. **Degradación de rendimiento:** El SWAP (disco) es 100-1000x más lento que la RAM. Usar SWAP cuando hay RAM disponible destruye el rendimiento.

2. **Latencia de acceso a base de datos:** PostgreSQL en particular sufre masivamente cuando sus páginas de memoria se mueven a SWAP, causando queries que normalmente toman milisegundos a tomar segundos.

3. **Desgaste de disco SSD:** El uso innecesario de SWAP acelera el desgaste de SSDs.

El valor de 10 es óptimo para sistemas con RAM suficiente (como este: 89% libre), porque:
- Solo usa SWAP en emergencias
- Mantiene el rendimiento óptimo
- Aún provee protección contra OOM

### Acción No Recomendada 3: Exponer más puertos de Docker al host sin firewall

**Por qué NO hacerlo:**
Actualmente, PostgreSQL ya está expuesto inseguramente:
> "tcp   LISTEN 0      4096                0.0.0.0:5432      0.0.0.0:*"

Exponer más servicios (Redis, servicios internos, APIs de administración) sin un firewall apropiado expande la superficie de ataque. Cada puerto expuesto públicamente es una puerta potencial para atacantes.

**Razones específicas:**
1. **Amplificación de riesgo:** Cada servicio adicional expuesto multiplica las vulnerabilidades potenciales.
2. **Violaciones de seguridad por defecto:** Los servicios Docker internos no están diseñados para exposición pública.
3. **Falta de autenticación robusta:** Muchos servicios internos asumen que están en una red confiable.

**Práctica correcta:** Usar reverse proxy (nginx, Traefik) con autenticación para cualquier acceso externo necesario.

### Acción No Recomendada 4: Ejecutar limpieza masiva de imágenes Docker sin verificación de uso

**Por qué NO hacerlo:**
El sistema muestra que hay imágenes reclaimables:
> "TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE"
> "Images          2         2         3.506GB   3.506GB (100%)"

Sin embargo, el comando:
```bash
docker image prune -a -f
```

Es peligroso en producción porque:

1. **Elimina TODAS las imágenes no usadas actualmente:** Si un contenedor se detiene temporalmente (mantenimiento, actualización), su imagen se elimina.

2. **Causa downtime en reinicios:** Al reiniciar, Docker debe descargar nuevamente todas las imágenes (puede tomar 5-30 minutos dependiendo de la conexión).

3. **Pérdida de versiones para rollback:** Si una actualización falla, no se puede volver a la versión anterior sin descargarla de nuevo.

**Práctica correcta:**
```bash
# Listar imágenes huérfanas
docker images -f "dangling=true"
# Eliminar solo imágenes sin etiqueta
docker image prune -f
```

> "Total reclaimed space: 0B"

Este resultado muestra que no hay imágenes dangling, así que una limpieza agresiva no es necesaria.

### Acción No Recomendada 5: Reducir o eliminar la memoria límite de los contenedores Docker

**Por qué NO hacerlo:**
Actualmente los contenedores tienen límites implícitos:
> "CONTAINER ID   NAME                    CPU %     MEM USAGE / LIMIT     MEM %"
> "a1df601f601a   odoo-dev-E1UF1884       0.02%     570.1MiB / 10.81GiB   5.15%"
> "bea5221f99d1   postgres-dev-E1UF1884   0.00%     231.4MiB / 10.81GiB   2.09%"

Aunque parecen no tener límites estrictos (muestran el total de RAM del sistema), en producción es tentador "dar más memoria" eliminando restricciones. Esto es contraproducente porque:

1. **Memory leak sin detección:** Sin límites, un proceso con fuga de memoria consumirá toda la RAM del host antes de ser detectado.

2. **Imposibilidad de multi-tenancy:** Si el sistema crece y necesita alojar múltiples aplicaciones, la falta de límites causa que una aplicación pueda inanir a las otras.

3. **OOM killer del host:** En lugar de matar el contenedor problemático, el kernel mata procesos aleatorios del host, incluyendo potencialmente Docker daemon.

**Práctica correcta:** Establecer límites explícitos basados en uso real + 50% de margen:
```yaml
services:
  odoo:
    mem_limit: 1g
    memswap_limit: 1.5g
  postgres:
    mem_limit: 512m
    memswap_limit: 768m
```

### Acción No Recomendada 6: Deshabilitar el health check de los contenedores

**Por qué NO hacerlo:**
Actualmente los contenedores tienen health checks configurados:
> "odoo-dev-E1UF1884       odoo:18.0            Up 3 hours (healthy)"
> "postgres-dev-E1UF1884   postgres:16-alpine   Up 3 hours (healthy)"

Es tentador eliminar health checks porque:
- Consumen recursos mínimos
- Agregan "complejidad" al docker-compose.yml
- Parecen innecesarios cuando "todo funciona"

Sin embargo, sin health checks:

1. **Servicios zombi:** Un contenedor puede estar "Up" pero la aplicación dentro estar colgada o no respondiendo.

2. **Cascada de fallos sin detección:** Si PostgreSQL se cuelga pero el contenedor sigue corriendo, Odoo acumula errores silenciosamente.

3. **Imposibilidad de auto-recuperación:** Con health checks, se puede configurar restart policies que reinician contenedores unhealthy.

**Práctica correcta:** Mantener y mejorar los health checks existentes:
```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U odoo"]
  interval: 30s
  timeout: 5s
  retries: 3
  start_period: 60s
```

### Acción No Recomendada 7: Realizar actualizaciones de sistema operativo sin planificación en horario laboral

**Por qué NO hacerlo:**
Un sistema de producción ERP-CRM requiere alta disponibilidad durante horas laborales. Las actualizaciones de sistema operativo pueden:

1. **Requerir reinicios:** Especialmente actualizaciones de kernel:
   > "Kernel: Linux 6.8.0-90-generic"

2. **Romper compatibilidad:** Actualizaciones mayores de librerías pueden causar fallos en aplicaciones.

3. **Causar downtime inesperado:** Incluso actualizaciones "menores" pueden requerir reinicio de servicios.

**Práctica correcta:**
1. Realizar actualizaciones en ventanas de mantenimiento (fines de semana, noches)
2. Configurar unattended-upgrades para parches de seguridad automáticos (solo security updates)
3. Probar actualizaciones mayores en entorno de staging primero
4. Mantener snapshots/backups antes de actualizar

**Sistema de actualizaciones actual:**
> "/usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown"

Esto indica que hay un sistema de actualizaciones desatendidas configurado, lo cual es correcto, pero debe estar limitado a actualizaciones de seguridad.

---

## CONCLUSIONES Y PRÓXIMOS PASOS

### Resumen Ejecutivo

El sistema analizado (`"Static hostname: ubuntuserver2404"`) es una implementación funcional de un ERP-CRM basado en Odoo 18.0 con PostgreSQL 16, ejecutándose en contenedores Docker sobre Ubuntu 24.04 LTS. El análisis revela un sistema con **recursos de hardware excedentes** (79% de RAM libre, 99% de CPU idle, 88% de disco disponible) pero con **deficiencias críticas de configuración y seguridad**.

### Estado Actual (Semáforo)

🔴 **CRÍTICO (Requiere acción inmediata):**
- Base de datos PostgreSQL expuesta públicamente
- Ausencia de sistema de backups

🟡 **ADVERTENCIA (Requiere atención en 1-2 semanas):**
- Límites de archivos abiertos insuficientes
- Falta de monitoreo y alertas
- Ausencia de rotación automática de logs

🟢 **SATISFACTORIO:**
- Recursos de hardware (CPU, RAM, disco)
- Configuración de SWAP (corregida durante la sesión)
- Estabilidad de contenedores
- Conectividad de red

### Priorización de Acciones

**INMEDIATO (0-48 horas):**
1. Aislar PostgreSQL de red externa
2. Configurar backups automatizados

**CORTO PLAZO (1-2 semanas):**
3. Aumentar límites de archivos abiertos
4. Implementar rotación de logs
5. Configurar monitoreo básico

**MEDIANO PLAZO (1-3 meses):**
- Implementar firewall (ufw/iptables)
- Configurar SSL/TLS para Odoo
- Establecer entorno de staging
- Documentar procedimientos de recuperación

### Capacidad y Escalabilidad

**Capacidad actual estimada:**
- Usuarios concurrentes soportados: 40-80 (basado en recursos disponibles)
- Headroom de crecimiento: 700-800% antes de necesitar expansión de hardware

**Limitantes identificados:**
- No es el hardware (extremadamente holgado)
- Configuración de aplicación y base de datos
- Prácticas de seguridad y operación

### Métricas de Éxito para Validación

Después de implementar las recomendaciones, medir:

1. **Seguridad:** 0 puertos de base de datos expuestos públicamente
2. **Confiabilidad:** Backups exitosos diarios verificables
3. **Observabilidad:** Alertas funcionando (simular condiciones)
4. **Rendimiento:** Mantener load average < 1.0 bajo carga normal
5. **Disponibilidad:** Uptime > 99.5% (máximo 3.6 horas downtime/mes)

---

**FIRMA DEL INFORME**

Informe generado por: Sistema de Análisis ERP-CRM  
Basado en datos de: log.txt (sesión del 2026-01-27, 12:04-13:01 UTC)  
Duración del análisis: Aproximadamente 57 minutos de recopilación de datos del sistema  

---

**NOTAS FINALES**

Este informe se basa exclusivamente en los datos observados en el archivo log.txt. Algunas recomendaciones pueden requerir ajustes basados en:
- Requisitos específicos del negocio
- Políticas de seguridad de la organización
- Compliance y regulaciones aplicables
- Presupuesto y recursos disponibles

Se recomienda una revisión trimestral de este análisis para validar que las mejoras se mantienen y ajustar según el crecimiento del sistema.
