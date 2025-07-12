# Gestión de Acceso Seguro en Windows Server

Este repositorio educativo se centra en la **gestión de acceso y seguridad de recursos** en entornos Windows Server. Su propósito es mostrar la configuración de un dominio de Active Directory, la creación de unidades organizativas y usuarios, así como la implementación de políticas de grupo (GPO) para controlar el acceso al sistema y desplegar software de manera automatizada. Se aborda la instalación y promoción del servidor como controlador de dominio, la configuración de permisos en carpetas compartidas y la aplicación de directivas de seguridad (contraseñas, accesos, etc.) con un enfoque práctico en ciberseguridad.

## Índice
- [Descripción general del proyecto](#descripción-general-del-proyecto)  
- [Casos prácticos y competencias técnicas](#casos-prácticos-y-competencias-técnicas)  
- [Estructura del repositorio](#estructura-del-repositorio)  
- [Autor y fecha](#autor-y-fecha)  

## Descripción general del proyecto

El proyecto aborda la **gestión de acceso al sistema y a recursos** en un dominio Windows Server. Se monta un dominio de Active Directory denominado, por ejemplo, `midominioholding.com`. Dentro de este dominio se crean **Unidades Organizativas (OU)** como “Gestión financiera” y “Dirección”, y se configuran usuarios asociados a cada unidad. A su vez, se aplican **Políticas de Grupo (GPO)** para automatizar tareas comunes: por ejemplo, instalar Google Chrome automáticamente en las máquinas cliente de un grupo de usuarios y crear accesos directos a recursos compartidos. También se incluyen directivas de seguridad para forzar el cambio de contraseña periódico y restringir accesos a carpetas confidenciales. En conjunto, estas actividades refuerzan las competencias técnicas de **administración de Active Directory, gestión de usuarios y permisos, configuración de redes y recursos compartidos, y aplicación de políticas de seguridad**.

## Casos prácticos y competencias técnicas

Este repositorio contiene dos casos prácticos principales:

- **Caso práctico 1: Creación de dominio y usuarios.** Configuración de un controlador de dominio Windows Server (`midominioholding.com`), creación de Unidades Organizativas (“Gestión financiera” y “Dirección”), y creación de usuarios dentro de cada OU.

- **Caso práctico 2: Despliegue de software y permisos con GPO.** Instalación automática de Google Chrome en los equipos de la OU “Gestión financiera” mediante una directiva de grupo. Configuración de acceso exclusivo a la carpeta “Documentación confidencial” para la OU “Dirección” mediante GPO. Finalmente, implementación de una política de seguridad para forzar el cambio de contraseña cada 7 días.

- **Preguntas Teóricas:** Evaluación de conceptos clave sobre roles, permisos, GPOs y acceso.
  
- **Capturas:** Evidencia visual paso a paso.

- **Licencia:** Detalles legales de uso y redistribución.

## Estructura del repositorio

```
📁 Gestión-de-Acceso-Seguro-en-Windows-Server
├── README.md
├── Informe-Técnico.md
├── capturas/
└── LICENSE.md

```


## Tecnologías utilizadas

- **Windows Server 2019**
- **Active Directory Domain Services (AD DS)**
- **Group Policy Management Console (GPMC)**
- **Cliente Windows 10**
- Compartición de recursos en red con permisos NTFS y de red

## Uso del repositorio

Este material está pensado como guía práctica para:

- Formación técnica en administración de sistemas Windows Server
- Laboratorios de entornos controlados
- Referencia para despliegue básico de Active Directory y GPOs

Puedes adaptarlo o ampliarlo para tus propias prácticas siempre que respetes la licencia.

## Licencia

Este repositorio se encuentra bajo la licencia:

**Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**  
https://creativecommons.org/licenses/by-nc/4.0/

> Puedes copiar, adaptar y compartir el contenido **siempre que cites a la autora (Déborah Loisel)** y no se utilice con fines comerciales.

---

## Autor y fecha

Déborah Loisel | Julio de 2025
[soydeborahloisel@gmail.com](mailto:soydeborahloisel@gmail.com)  
[github.com/DEBBKL](https://github.com/DEBBKL)


