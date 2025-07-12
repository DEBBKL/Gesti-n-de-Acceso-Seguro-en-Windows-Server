# Informe Técnico: Gestión de Acceso y Seguridad en Windows Server

## Índice
- [Introducción](#introducción)  
- [Caso Práctico 1: Creación del dominio Active Directory](#caso-práctico-1-creación-del-dominio-active-directory)  
- [Caso Práctico 2: Despliegue de software y permisos con GPO](#caso-práctico-2-despliegue-de-software-y-permisos-con-gpo)  
- [Preguntas Teóricas](#preguntas-teóricas)

---

## Introducción

En este informe se detalla paso a paso la resolución de los dos casos prácticos relacionados con la gestión segura del acceso en un entorno Windows Server. Se explica cómo configurar un controlador de dominio Active Directory, organizar unidades y usuarios, y aplicar directivas de grupo (GPO) para automatizar tareas de administración y reforzar la seguridad. Cada sección incluye objetivos, configuraciones a realizar y los resultados esperados. El estilo es técnico y estructurado para facilitar su comprensión y replicación.

---

## Caso Práctico 1: Creación del dominio Active Directory

### 1. Preparar el servidor (nombre y zona horaria)

Asignar nombre significativo al servidor (ej. `SRV-DC01`), ajustar la zona horaria y reiniciar.

---

### 2. Instalar el rol de Servicios de Dominio de Active Directory (AD DS)

Desde el Administrador del Servidor se inicia el asistente para agregar roles y características:

![1. Asistente para agregar roles y características, antes de comenzar](./capturas/1.PNG)

Se seleccionan las características requeridas para Servicios de dominio de Active Directory:

![2. Desea agregar las características requeridas para servicios de dominio de Active Directory](./capturas/2.PNG)

Se selecciona el rol de Servicios de dominio de Active Directory:

![3. Seleccionar roles del servidor](./capturas/3.PNG)

Se confirman las selecciones antes de la instalación:

![4. Confirmar selecciones de instalación](./capturas/4.PNG)

Se procede a hacer clic en instalar para iniciar el proceso:

![5. Hacer clic en instalar](./capturas/5.PNG)

---

### 3. Promover el servidor a controlador de dominio

### 3. Promover a controlador de dominio

Se inicia la promoción del servidor a controlador de dominio desde el Administrador del Servidor:

![6. Promover este servidor a controlador de dominio](./capturas/6.PNG)

Se configura la implementación como un nuevo bosque con el nombre de dominio `midominioholding.com`:

![7. Configuración de implementación](./capturas/7.PNG)

Se definen las opciones del controlador de dominio, incluyendo la contraseña DSRM:

![8. Opciones del controlador de dominio](./capturas/8.PNG)

Se revisan y confirman todas las opciones antes de la instalación:

![9. Revisar opciones](./capturas/9.PNG)

---

### 4. Configurar cliente y unir al dominio

Se configura la propiedad de red del servidor para asignar una IP fija y el DNS apuntando al servidor:

![10. Propiedades: Protocolo de Internet versión 4 TCP IPv4](./capturas/10.PNG)

Resultado tras la configuración de IP y DNS:

![11. Resultado tras el paso 10](./capturas/11.PNG)

Se verifica la creación de las Unidades Organizativas (OU) en Usuarios y Equipos de Active Directory, ejemplo OU “Gestión financiera”:

![12. Usuarios y equipos de Active Directory - Gestión Financiera](./capturas/12.PNG)

Se crea también la OU “Dirección”:

![13. Usuarios y equipos de Active Directory - Dirección](./capturas/13.PNG)

---

### 5. Crear usuarios y asignar permisos

Se definen permisos en carpetas compartidas para la GPO “Presets”:

![14. Permisos de Presets](./capturas/14.PNG)

Se crea el usuario "Rosa Pérez" con su correo electrónico asociado:

![15. Configuración usuario "Rosa Pérez @debbholding.com"](./capturas/15.PNG)

---

### 6. Despliegue de software y permisos con GPO

Se prepara el instalador de Google Chrome (archivo `.msi`) en la carpeta compartida “Presets”:

![16. Preset Google Chrome](./capturas/16.PNG)

La carpeta está compartida y accesible en red:

![17. Acceso a la red, la carpeta está compartida](./capturas/17.PNG)

Desde la consola de administración de directivas de grupo (GPMC) se crea y vincula la GPO “Presets” en el dominio:

![18. Administración de directivas, crear GPO en este dominio y vincularlo aquí](./capturas/18.PNG)

En el editor de administración de directivas de grupo se configura la instalación del software Google Chrome usando la ruta UNC al `.msi`:

![19. Editor de administración de directivas de grupo, configuración de software, instalación de Google Chrome](./capturas/19.PNG)

Se aplica la política a la OU “Gestión financiera” con filtrado de seguridad adecuado:

![20. Gestión Financiera, presets, filtrado de seguridad](./capturas/20.PNG)

Se crea la carpeta “Documentación Confidencial” y se comparte para usuarios del grupo Dirección:

![21. La carpeta está compartida, Documentación Confidencial](./capturas/21.PNG)

---

### 7. Configuración de permisos y accesos

Se configuran los permisos NTFS y de compartición para la carpeta “Documentación Confidencial”:

![22. Permisos de la carpeta de Documentación Confidencial](./capturas/22.PNG)

Desde la consola de administración de directivas de grupo, se crea y vincula la GPO “Documentación Confidencial” en la OU Dirección:

![23. Administración de directivas de grupo, Dirección, Documentación Confidencial](./capturas/23.PNG)

En el editor de administración de directivas de grupo se crea un acceso directo en el escritorio del usuario con destino a la carpeta compartida:

![24. Editor de administración de directivas de grupo, directiva de Documentación Confidencial, configuración de Windows, accesos directos](./capturas/24.PNG)

Resultado tras aplicar la política y comprobar el acceso directo en el escritorio:

![25. Resultado del bosque abierto con la administración de directivas de grupo](./capturas/25.PNG)

Se prueba el acceso intentando abrir la carpeta desde un grupo sin permisos, lo que es denegado correctamente:

![26. Comprobación de permisos intentando abrir el directorio desde otro grupo sin permisos](./capturas/26.PNG)

---

### 8. Política de expiración de contraseñas

Desde la consola de dominios se configura la política de expiración de contraseñas con un límite de 7 días:

![27. Dominios, contraseñas](./capturas/27.PNG)

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
