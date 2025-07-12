# Gestión de Acceso Seguro en Windows Server

Este repositorio educativo se centra en la gestión de acceso y seguridad de recursos en entornos Windows Server. Su propósito es mostrar la configuración de un dominio de Active Directory, la creación de unidades organizativas y usuarios, así como la implementación de políticas de grupo (GPO) para controlar el acceso al sistema y desplegar software de manera automatizada. Se aborda la instalación y promoción del servidor como controlador de dominio, la configuración de permisos en carpetas compartidas y la aplicación de directivas de seguridad (contraseñas, accesos, etc.) con un enfoque práctico en ciberseguridad.

---

## Índice

- Descripción general del proyecto
- Casos prácticos y competencias técnicas
- Estructura del repositorio
- Autor y fecha

---

## Descripción general del proyecto

El proyecto aborda la gestión de acceso al sistema y a recursos en un dominio Windows Server. Se monta un dominio de Active Directory denominado, por ejemplo, midominioholding.com. Dentro de este dominio se crean Unidades Organizativas (OU) como “Gestión financiera” y “Dirección”, y se configuran usuarios asociados a cada unidad. A su vez, se aplican Políticas de Grupo (GPO) para automatizar tareas comunes: por ejemplo, instalar Google Chrome automáticamente en las máquinas cliente de un grupo de usuarios y crear accesos directos a recursos compartidos. También se incluyen directivas de seguridad para forzar el cambio de contraseña periódico y restringir accesos a carpetas confidenciales. En conjunto, estas actividades refuerzan las competencias técnicas de administración de Active Directory, gestión de usuarios y permisos, configuración de redes y recursos compartidos, y aplicación de políticas de seguridad.

---

## Casos prácticos y competencias técnicas

Este repositorio contiene dos casos prácticos principales:

Caso práctico 1: Creación de dominio y usuarios. Configuración de un controlador de dominio Windows Server (midominioholding.com), creación de Unidades Organizativas (“Gestión financiera” y “Dirección”), y creación de usuarios dentro de cada OU. Se trabajan competencias en administración de Active Directory, gestión de equipos y permisos locales y de dominio, así como configuración básica de red en el cliente para unirse al dominio.
Caso práctico 2: Despliegue de software y permisos con GPO. Instalación automática de Google Chrome en los equipos de la OU “Gestión financiera” mediante una directiva de grupo. Además, se configura un recurso compartido “Documentación confidencial” accesible solo para los usuarios de la OU “Dirección” mediante un GPO que crea accesos directos en el Escritorio. Finalmente, se implementa una política de seguridad para forzar el cambio de contraseña cada 7 días. Este caso desarrolla habilidades en creación de GPO, compartición de recursos y aplicación de políticas de seguridad en el dominio.

En el informe se detallan los pasos, objetivos y resultados esperados en cada etapa, enfatizando un tono técnico y profesional para facilitar la replicación de la práctica.

---

## Estructura del repositorio

El repositorio está organizado de la siguiente forma:

- README.md – Documento de introducción, como este archivo.
- Informe-Técnico.md – Informe detallado de la práctica, con paso a paso técnico.
- scripts/ – Carpeta (opcional) para almacenar scripts o comandos de automatización utilizados (por ejemplo, PowerShell).
- capturas/ – Carpeta destinada a capturas de pantalla ilustrativas (pendientes de añadir).
- docs/ – Documentación adicional o recursos de referencia (en caso de existir).

---

Nota: Cada archivo y carpeta está estructurado para facilitar el seguimiento de la práctica y su posterior despliegue en un entorno educativo. Las capturas de pantalla se agregan en capturas/ según el punto donde se indique “[Inserte aquí captura de pantalla...]” en el informe técnico.

---

## Autor y fecha

Autor: Deborah Loisel Santana
Fecha: 7 de mayo de 2025

