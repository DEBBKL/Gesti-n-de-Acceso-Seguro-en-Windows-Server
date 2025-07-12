# Informe Técnico: Gestión de Acceso y Seguridad en Windows Server

## Índice
- [Introducción](#introducción)  
- [Caso Práctico 1: Creación del dominio Active Directory](#caso-práctico-1-creación-del-dominio-active-directory)  
- [Caso Práctico 2: Despliegue de software y permisos con GPO](#caso-práctico-2-despliegue-de-software-y-permisos-con-gpo)  
- [Preguntas Teóricas](#preguntas-teóricas)  

## Introducción

En este informe se detalla paso a paso la resolución de los dos casos prácticos relacionados con la gestión segura del acceso en un entorno Windows Server. Se explica cómo configurar un controlador de dominio Active Directory, organizar unidades y usuarios, y aplicar directivas de grupo (GPO) para automatizar tareas de administración y reforzar la seguridad. Cada sección incluye objetivos, configuraciones a realizar y los resultados esperados. El estilo es técnico y estructurado para facilitar su comprensión y replicación.

## Caso Práctico 1: Creación del dominio Active Directory

### 1. Preparar el servidor (nombre y zona horaria)

- Asignar un nombre significativo al servidor (ej. `SRV-DC01`) y corregir la zona horaria.
- Reiniciar el servidor para aplicar los cambios.

### 2. Instalar el rol de Servicios de Dominio de Active Directory (AD DS)

- Desde el **Administrador del servidor**, seleccionar "Agregar roles y características".
- Seleccionar **Servicios de dominio de Active Directory**.
- Confirmar e instalar el rol.

**[Inserte aquí captura de pantalla del asistente de instalación del rol]**

### 3. Promover el servidor a controlador de dominio

- Seleccionar "Agregar un nuevo bosque" e introducir el nombre del dominio raíz (ej. `midominioholding.com`).
- Establecer la contraseña de DSRM.
- Confirmar configuración DNS y nombre NetBIOS.
- Finalizar instalación y reiniciar.

**[Inserte aquí captura de pantalla de la promoción a controlador de dominio]**

### 4. Configurar red del cliente y unirlo al dominio

- Establecer IP manual y DNS del servidor.
- Unir el cliente al dominio `midominioholding.com`.

### 5. Crear OU y usuarios

- Desde “Usuarios y equipos de Active Directory”, crear:
  - OU "Gestión financiera" (usuarios: Rosa Pérez, María García, Raúl Jiménez).
  - OU "Dirección" (usuarios: Laura Rodríguez, Jaime Borges).

**[Inserte aquí captura de pantalla con las OU y usuarios creados]**

### 6. Verificar acceso

- Iniciar sesión desde el cliente como Rosa Pérez para validar la correcta autenticación en el dominio.

---

## Caso Práctico 2: Despliegue de software y permisos con GPO

### 1. Crear carpeta compartida “Presets” con instalador de Chrome

- Crear `C:\Presets` en el servidor y colocar el archivo `.msi` descargado desde Chrome Enterprise.
- Compartir la carpeta (ej. `\\midominioholding.local\Presets`) con permisos de lectura.

### 2. Crear GPO para instalación de Chrome en "Gestión financiera"

- En GPMC, crear y vincular GPO “Presets” a la OU “Gestión financiera”.
- Editar la GPO:
  - Ruta: `Configuración de usuario > Directivas > Configuración de software > Instalación de software`.
  - Nuevo > Paquete > ruta UNC al `.msi`.
  - Tipo de implementación: **Asignado**.

**[Inserte aquí captura de la GPO Presets con ruta al instalador .msi]**

### 3. Crear carpeta compartida “Documentación confidencial”

- Crear `C:\DocumentaciónConfidencial` en el servidor.
- Compartir solo con usuarios del grupo Dirección (lectura/escritura).
- Ajustar también los permisos NTFS desde la pestaña **Seguridad**.

### 4. Crear GPO para acceso directo a “Documentación confidencial”

- En GPMC, crear y vincular GPO “Documentación Confidencial” a la OU Dirección.
- Editar la GPO:
  - Ruta: `Configuración de usuario > Preferencias > Configuración de Windows > Accesos Directos`.
  - Crear acceso directo con destino: `\\midominioholding.local\DocumentaciónConfidencial`.
  - Ubicación: Escritorio del usuario.

**[Inserte aquí captura de acceso directo creado en GPO]**

- Marcar ambas GPOs como **exigidas (enforced)**.

### 5. Configurar política de expiración de contraseñas

- Crear GPO a nivel de dominio para forzar cambio de contraseña cada 7 días.
- Ruta: `Configuración del equipo > Directivas > Configuración de Windows > Configuración de seguridad > Directivas de cuenta > Directiva de contraseñas > Vigencia máxima`.

**[Inserte aquí captura de la directiva de expiración de contraseña]**

---

## Preguntas Teóricas

### a) ¿Cómo se modifican los permisos de un usuario en una carpeta compartida?

Mediante las propiedades de la carpeta:

- **Pestaña Compartir:** Configura permisos de red (compartición).
- **Pestaña Seguridad:** Configura permisos NTFS (nivel de sistema de archivos).
- Puede utilizarse también la línea de comandos (`icacls`) o políticas de grupo.

### b) ¿En qué se diferencia el acceso local del acceso remoto en un entorno Windows Server?

- **Acceso local:** Se realiza directamente desde el servidor físico (teclado/monitor).
- **Acceso remoto:** Se realiza mediante red, generalmente usando RDP.
- El acceso remoto requiere habilitación previa y permisos asignados.

### c) ¿Qué función tiene la consola de "Administración de directivas de grupo" (GPMC) en la gestión de permisos?

Permite:

- Crear, editar y aplicar GPOs a usuarios y equipos.
- Establecer configuraciones de seguridad, software, red, accesos, etc.
- Centralizar la administración de políticas del dominio.

### d) ¿Qué diferencia hay entre un usuario estándar y un usuario con privilegios de administrador?

- **Usuario estándar:** Permisos limitados, no puede instalar software ni modificar configuraciones del sistema.
- **Administrador:** Permisos elevados, acceso total al sistema, puede modificar políticas, usuarios, software y recursos.

---

**Fin del informe técnico.**
