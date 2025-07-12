# Informe Técnico: Gestión de Acceso y Seguridad en Windows Server

---

## Índice

- Introducción
- Caso Práctico 1: Creación del dominio Active Directory
- Caso Práctico 2: Despliegue de software y permisos con GPO
- Preguntas Teóricas

---

## Introducción

En este informe se detalla paso a paso la resolución de los dos casos prácticos relacionados con la gestión segura del acceso en un entorno Windows Server. Se explica cómo configurar un controlador de dominio Active Directory, organizar unidades y usuarios, y aplicar directivas de grupo (GPO) para automatizar tareas de administración y reforzar la seguridad. Cada sección incluye objetivos, configuraciones a realizar y los resultados esperados. El estilo es técnico y estructurado para facilitar su comprensión y replicación.

---

## Caso Práctico 1: Creación del dominio Active Directory

En este caso se instalará y configurará un dominio de Active Directory llamado, por ejemplo, midominioholding.com, y se crearán dos Unidades Organizativas (OU) con usuarios asociados.

  ### 1. Preparar el servidor (nombre y zona horaria).
  
    - Objetivo: Asignar un nombre significativo al servidor y corregir la zona horaria antes de instalar el dominio.
    - Configuración: En el Administrador del Servidor, vaya a Configuración del sistema (Panel de Control > Sistema > Cambiar configuración > Cambiar nombre de equipo) para renombrar el equipo (por ejemplo, a SRV-DC01). Luego ajuste la zona horaria en Fecha y hora (pestaña Zona horaria). Después de estos cambios, reinicie el servidor para aplicarlos.
Resultado esperado: El servidor reinicia con el nuevo nombre y la zona horaria correcta, listo para configurar Active Directory.

  ### 2. Instalar el rol de Servicios de Dominio de Active Directory (AD DS).
  
    - Objetivo: Instalar los servicios necesarios que permitan promover el servidor como controlador de dominio.
    - Configuración: Abra el Administrador del Servidor, haga clic en Administrar > Agregar roles y características. Avance en el asistente hasta llegar a Roles de servidor. Seleccione Servicios de dominio de Active Directory (AD DS). Haga clic en Siguiente hasta llegar a la confirmación y luego en Instalar. Puede marcar la opción para reiniciar automáticamente si es necesario.
    - Resultado esperado: Se inicia la instalación del rol AD DS. Una vez completada la instalación, el asistente mostrará que se requiere reiniciar el equipo. Reinicie el servidor si no se reinició automáticamente.

[Inserte aquí captura de pantalla del asistente de “Agregar Roles y Características” mostrando la selección de AD DS]

  ### 3. Promover el servidor a controlador de dominio (nuevo bosque).

    - Objetivo: Configurar el servidor como el primer controlador de dominio del dominio midominioholding.com.
    - Configuración: Tras el reinicio, en el Administrador del Servidor aparece una notificación. Haga clic en Promover este servidor a controlador de dominio. En el asistente de Configuración de implementación, seleccione Agregar un nuevo bosque y escriba el nombre de dominio raíz (ej. midominioholding.com). Haga clic en Siguiente.
En Opciones de controlador de dominio, ingrese una contraseña de restauración de Servicios de Directorio (DSRM).
Avance a Opciones DNS (puede aceptar la configuración por defecto) y defina un nombre NetBIOS para el dominio (por lo general, la parte antes del dominio, p.ej. MIDOMINIO).
Continúe hasta la pantalla de revisión final y haga clic en Instalar. El servidor se reiniciará al completar el proceso.
    - Resultado esperado: El servidor se configura como controlador de dominio del nuevo bosque midominioholding.com. Una vez reiniciado, iniciará sesión bajo el dominio. En el Administrador del Servidor, ya aparecerá el dominio en la sección de Active Directory.

[Inserte aquí captura de pantalla de la ventana de promoción a controlador de dominio mostrando la configuración del nuevo bosque]

  ### 4. Configurar red del cliente y unirlo al dominio.

    - Objetivo: Integrar un equipo cliente Windows 11 al dominio creado para realizar pruebas de usuario.
    - Configuración: En el equipo cliente, configure la dirección IPv4 manualmente (por ejemplo, IP en la misma subred que el servidor) y establezca la dirección del servidor DNS apuntando al controlador de dominio. Luego, en Configuración del sistema (o en Panel de Control > Sistema > Cambiar configuración > Cambiar nombre de equipo), cambie el tipo de red a Dominio e ingrese midominioholding.com. Al solicitar credenciales, use un usuario administrador del dominio (por ejemplo, el administrador local del dominio). Reinicie el cliente después de la unión.
    - Resultado esperado: El equipo cliente se reinicia y aparecerá en Active Directory como miembro del dominio.

  ### 5. Crear Unidades Organizativas y usuarios en Active Directory.
    - Objetivo: Organizar los usuarios por departamento y asignarles credenciales de acceso.
    - Configuración: En el servidor (controlador de dominio), abra Usuarios y equipos de Active Directory. Haga clic derecho sobre el dominio midominioholding.com y seleccione Nuevo > Unidad Organizativa. Cree dos OU llamadas Gestión financiera y Dirección. A continuación, en cada OU creada, haga clic derecho y elija Nuevo > Usuario para crear las cuentas de usuario respectivas:
      - En Gestión financiera: Rosa Pérez, María García, Raúl Jiménez.
      - En Dirección: Laura Rodríguez, Jaime Borges.
Para cada usuario, ingrese los nombres y establezca una contraseña inicial.
    - Resultado esperado: Cada usuario aparece bajo la OU correspondiente en Active Directory. Estos usuarios podrán iniciar sesión en cualquier equipo unido al dominio bajo sus credenciales.

[Inserte aquí captura de pantalla de “Usuarios y Equipos de Active Directory” mostrando las OU y los usuarios creados]

  ### 6. Verificar acceso de usuario en el dominio.
    - Objetivo: Comprobar que los usuarios pueden iniciar sesión correctamente.
    - Configuración: En el equipo cliente unido al dominio, cierre sesión del administrador local y seleccione Otro usuario en la pantalla de inicio. Ingrese las credenciales de un usuario creado (por ejemplo, Rosa Pérez).
    - Resultado esperado: El sistema permite el inicio de sesión del usuario de dominio. Se mostrará el escritorio de Windows con políticas aplicadas desde el dominio (aunque inicialmente no hay políticas personalizadas hasta que se configuren en el siguiente caso práctico).

---

## Caso Práctico 2: Despliegue de software y permisos con GPO

En este caso se configurarán políticas de grupo para instalar software automáticamente y controlar el acceso a recursos compartidos según el departamento del usuario.

  ### 1. Crear carpeta compartida para instalación de software (“Presets”).
    - Objetivo: Preparar un recurso compartido que contenga el instalador de Google Chrome para su despliegue mediante GPO.
    - Configuración: En el servidor, cree una nueva carpeta, por ejemplo C:\Presets. Coloque dentro el archivo instalador de Chrome (extensión .msi, obtenido de Chrome Enterprise). Luego haga clic derecho en la carpeta y vaya a Propiedades > Compartir. Active Uso compartido avanzado, asígnele un nombre de recurso compartido (ej. \\midominioholding.local\Presets) y ajuste los permisos: agregue el grupo “Todos” o “Domain Users” con permiso de lectura. Bajo la pestaña Seguridad, asegúrese de que los usuarios del dominio tengan al menos permiso de lectura en esa carpeta.
    - Resultado esperado: La carpeta Presets está compartida en la red. Los equipos clientes podrán acceder al instalador de Chrome a través de la ruta UNC del recurso compartido.

  ### 2. Crear GPO para instalación de Chrome en “Gestión financiera”.
    - Objetivo: Automatizar la instalación de Chrome en los equipos de los usuarios de la OU “Gestión financiera”.
    - Configuración: En el servidor, abra la Consola de Administración de Directivas de Grupo (GPMC). En el dominio midominioholding.com, expanda la OU Gestión financiera, haga clic derecho y seleccione Crear un GPO en este dominio y vincularlo aquí. Nómbrelo, por ejemplo, GPO-Chrome. Edite este GPO: vaya a Configuración de usuario > Políticas > Configuración de software > Instalación de software, clic derecho y elija Nuevo > Paquete. Ingrese la ruta UNC al instalador (\\midominioholding.local\Presets\chrome_installer.msi). Marque la opción Asignado y confirme la instalación.
    - Resultado esperado: Al iniciar sesión los usuarios de “Gestión financiera”, el sistema instalará automáticamente Google Chrome desde el recurso compartido.

[Inserte aquí captura de pantalla de la sección de Instalación de software en la GPO “GPO-Chrome”]

  ### 3. Crear recurso compartido “Documentación confidencial” para Dirección.
    - Objetivo: Configurar una carpeta compartida accesible únicamente por el equipo de Dirección y preparar su acceso en el escritorio de esos usuarios.
    - Configuración: En el servidor, cree la carpeta C:\DocumentaciónConfidencial. En Propiedades > Compartir, habilite Uso compartido avanzado y compártala como DocumentacionConfidencial. En permisos, añada únicamente a los usuarios o grupo de la OU Dirección con permiso de lectura/escritura, eliminando otros usuarios si fuera necesario. En la pestaña Seguridad (NTFS), otorgue permisos adecuados al mismo grupo.
    - Resultado esperado: Sólo los usuarios de la OU “Dirección” pueden acceder al contenido de \\midominioholding.local\DocumentacionConfidencial en la red, mientras que otros usuarios son denegados.

  ### 4. Crear GPO para acceso a “Documentación confidencial”.
    - Objetivo: Añadir un acceso directo a la carpeta confidencial en el escritorio de los usuarios de “Dirección” mediante una GPO.
    - Configuración: En la GPMC, expanda la OU Dirección, cree y vincule un GPO llamado GPO-DocConfidencial. Edítelo: vaya a Configuración de usuario > Preferencias > Configuración de Windows > Accesos directos, clic derecho Nuevo > Acceso directo. Configure lo siguiente:
      - Acceso directo:
        - Acción: Crear.
        - Ubicación: Escritorio (user’s desktop).
        - Nombre: Documentación confidencial.
        - Destino: Ingrese la ruta de red al recurso compartido (\\midominioholding.local\DocumentacionConfidencial).
        - Marque la opción “Instalar esta aplicación durante el inicio de sesión” si se aplica (para forzar la creación del acceso).
Además, en la GPMC asegúrese de Forzar (enforced) ambas GPO creadas (GPO-Chrome y GPO-DocConfidencial) para que tengan prioridad sobre otras posibles directivas.
    - Resultado esperado: Al iniciar sesión, los usuarios de “Dirección” verán un acceso directo en su Escritorio que apunta a la carpeta compartida “Documentación confidencial”. Si un usuario de “Gestión financiera” intenta acceder a esa carpeta, recibirá un mensaje de error por falta de permisos.

[Inserte aquí captura de pantalla de la configuración del acceso directo en la GPO “GPO-DocConfidencial”]

  ### 5- Configurar directiva de expiración de contraseñas.
    - Objetivo: Aumentar la seguridad forzando a los usuarios a cambiar su contraseña cada 7 días.
    - Configuración: En la GPMC, en el nivel del dominio (o en un contenedor que abarque todos los usuarios), cree un GPO llamado GPO-PasswordPolicy. Edítelo: navegue a Configuración del equipo > Políticas > Configuración de Windows > Configuración de seguridad > Directivas de cuenta > Directiva de contraseñas > Vigencia máxima de la contraseña. Establezca el valor en 7 días. Vincule este GPO al dominio para que se aplique a todos los usuarios (o al menos a las OUs “Gestión financiera” y “Dirección”).
    - Resultado esperado: Los usuarios del dominio serán obligados a cambiar su contraseña al menos cada 7 días. En el momento del inicio de sesión después de la expiración, el sistema solicitará un nuevo password.

[Inserte aquí captura de pantalla de la directiva de expiración de contraseña en GPO-PasswordPolicy]

    - Resultado Final: Con estos pasos, se ha logrado un dominio de Active Directory funcional con dos grupos de usuarios, instalación automática de software para un grupo y control de accesos a recursos críticos para otro. La directiva de contraseñas mejora la seguridad al obligar cambios periódicos.

---

## Preguntas Teóricas
- a) ¿Cómo se modifican los permisos de un usuario en una carpeta compartida?
Para cambiar los permisos de un usuario en una carpeta compartida, hay que ajustar tanto los permisos de compartición como los permisos NTFS. Por ejemplo, haga clic derecho sobre la carpeta en el servidor o equipo donde reside, seleccione Propiedades > Compartir y luego Uso compartido avanzado > Permisos. Allí puede agregar o seleccionar al usuario y asignarle permisos (lectura/escritura). Además, en la pestaña Seguridad (NTFS) de Propiedades de la carpeta, seleccione Editar, agregue el usuario y configure sus permisos a nivel de sistema de archivos. También es posible gestionar permisos mediante el comando icacls o usando el Centro de Redes y recursos compartidos en entornos más antiguos. En resumen, se modifican en las propiedades de la carpeta, estableciendo los permisos apropiados en Compartir y en Seguridad para el usuario deseado.

- b) ¿En qué se diferencia el acceso local del acceso remoto en un entorno Windows Server?
El acceso local ocurre cuando un usuario inicia sesión directamente en el servidor con teclado y monitor conectados físicamente. En este caso, tiene acceso completo a la consola del sistema, incluidas las unidades locales y dispositivos conectados. Por el contrario, el acceso remoto se realiza a través de la red, típicamente mediante Remote Desktop Protocol (Escritorio Remoto) u otras herramientas de administración remota. El acceso remoto requiere que el servidor tenga habilitado el servicio de Escritorio Remoto y que el usuario tenga permisos de sesión remota. Desde remoto, el usuario administra el servidor a distancia como si estuviera delante de él, aunque depende de la conectividad y puede tener restricciones adicionales (por ejemplo, no puede usar dispositivos que no estén redirigidos). En resumen, el acceso local es directamente en la máquina y el remoto es por red, cada uno con sus requisitos de configuración y permisos.

- c) ¿Qué función tiene la consola de "Administración de directivas de grupo" (GPMC) en la gestión de permisos?
La Consola de Administración de Directivas de Grupo (GPMC) es la herramienta central para crear, editar y gestionar las Políticas de Grupo (GPO) en un dominio de Active Directory. Su función principal es permitir a los administradores definir configuraciones de seguridad, restricciones y automatizaciones que se aplican a usuarios y equipos en el dominio. Por ejemplo, desde GPMC se pueden asignar permisos de acceso a recursos compartidos creando directivas que instalan accesos directos, restringen el uso de programas o configuran contraseñas. También se usa para desplegar software (mediante políticas de software) y aplicar configuraciones de seguridad del sistema operativo. En resumen, la GPMC sirve para centralizar la administración de políticas de configuración y seguridad, incluyendo la gestión de permisos, garantizando que un conjunto consistente de reglas se aplique a todos los objetos designados del dominio.

- d) ¿Qué diferencia hay entre un usuario estándar y un usuario con privilegios de administrador?
Un usuario estándar (o usuario normal) es aquel que tiene permisos limitados en el sistema. Puede realizar tareas básicas como ejecutar aplicaciones y cambiar configuraciones personales, pero no puede modificar configuraciones del sistema, instalar software que afecte a todos los usuarios ni acceder a recursos restringidos del sistema. Un usuario administrador, por su parte, tiene privilegios elevados que le permiten realizar cualquier operación en la máquina o dominio: instalar o desinstalar programas, cambiar configuraciones de seguridad, agregar o eliminar usuarios, gestionar políticas del sistema, etc. En un dominio Windows, un administrador de dominio puede realizar cambios en todo el dominio, mientras que un usuario estándar sólo tiene control sobre su perfil y algunos recursos asignados. La principal diferencia es, por tanto, el nivel de permisos: el administrador tiene control total, mientras que el usuario estándar está restringido a actividades no críticas para la seguridad del sistema.
