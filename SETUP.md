# Guía de Configuración del Entorno (SETUP)

Este documento resume los pasos necesarios para replicar el laboratorio de **gestión de acceso seguro en Windows Server** descrito en el [Informe-Técnico.md](./Informe-Técnico.md). Se recomienda seguir esta guía en orden para obtener un entorno funcional y controlado para pruebas.

## Índice

- [Requisitos previos](#requisitos-previos)
- [1. Configuración del servidor Windows Server](#1-configuración-del-servidor-windows-server)
- [2. Instalación y configuración de Active Directory](#2-instalación-y-configuración-de-active-directory)
- [3. Configuración de red y cliente Windows 11](#3-configuración-de-red-y-cliente-windows-11)
- [4. Creación de OUs y usuarios](#4-creación-de-ous-y-usuarios)
- [5. Implementación de GPOs](#5-implementación-de-gpos)

---

## Requisitos previos

- Máquina virtual con **Windows Server 2022** o equivalente.
- Máquina virtual cliente con **Windows 11**.
- Red interna configurada (NAT o puente).
- ISO de instalación de Chrome Enterprise (.msi).
- Acceso con cuenta administrativa en ambas máquinas.

---

## 1. Configuración del servidor Windows Server

1. Cambiar el nombre del equipo (ej. `SRV-DC01`).
2. Ajustar la zona horaria.
3. Reiniciar el sistema.
4. Asignar IP estática (ej. `192.168.1.10`) y configurar DNS apuntando a sí mismo.

---

## 2. Instalación y configuración de Active Directory

1. Desde el **Administrador del servidor**, instalar:
   - Rol: **Servicios de dominio de Active Directory (AD DS)**.
2. Promover el servidor a controlador de dominio:
   - Crear nuevo bosque.
   - Nombre de dominio: `midominioholding.com`.
   - Establecer contraseña de DSRM.
3. Esperar el reinicio tras la instalación.

---

## 3. Configuración de red y cliente Windows 11

1. Asignar IP estática al cliente (ej. `192.168.1.20`) y DNS apuntando al servidor (`192.168.1.10`).
2. Unir el cliente al dominio:
   - Propiedades del sistema > Cambiar configuración > Unirse a dominio.
3. Reiniciar el cliente.

---

## 4. Creación de OUs y usuarios

1. En el servidor, abrir **Usuarios y equipos de Active Directory**.
2. Crear las siguientes Unidades Organizativas:
   - Gestión financiera
   - Dirección
3. Crear los siguientes usuarios:
   - En “Gestión financiera”: Rosa Pérez, María García, Raúl Jiménez.
   - En “Dirección”: Laura Rodríguez, Jaime Borges.

---

## 5. Implementación de GPOs

### GPO 1 – Instalación automática de Chrome

- Ruta: `Configuración de usuario > Directivas > Configuración de software`.
- Crear carpeta compartida `\\SRV-DC01\Presets` con el `.msi` dentro.
- Asignar el paquete a la OU “Gestión financiera”.

### GPO 2 – Acceso a carpeta “Documentación confidencial”

- Compartir `C:\DocumentaciónConfidencial` sólo con el grupo “Dirección”.
- GPO vinculada a “Dirección” que cree un acceso directo en el escritorio:
  - Ruta: `\\SRV-DC01\DocumentaciónConfidencial`.

### GPO 3 – Cambio de contraseña obligatorio cada 7 días

- Vincular al dominio completo.
- Ruta:  
  `Configuración del equipo > Directivas > Configuración de Windows > Configuración de seguridad > Directivas de cuenta > Directiva de contraseñas > Vigencia máxima = 7 días`.

---

## Referencias

- Detalles técnicos completos en [Informe-Técnico.md](./Informe-Técnico.md)
- Este proyecto forma parte del módulo **MF0490_3 - Seguridad Informática**.

---

**Autor:** Déborah Loisel Santana  
**Fecha de implementación:** Mayo 2025
