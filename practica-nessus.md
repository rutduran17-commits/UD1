# Informe de auditoría de vulnerabilidades 

## 1. Contexto y alcance
Estudio Técnico Torrent S.L. firmará un contrato de mantenimiento informático con una nueva empresa de servicios y antes de firmar nos contrataron para saber en que estado de seguridad se encuentran sus sistemas.
Lo que haremos será encontrar los fallos de seguridad que la máquina presenta para que así el Estudio Técnico Torrent S.L. sepa lo que tiene y a base de eso firmar los servicios.

## 2. Metodología

Se usó la herramienta de Nessus que es un escáner de vulnerabilidades, específicamente Nessus Essentials Plus. Fue mediante Basic Network hacia la IP de Metasploide( supuesto servidor de Estudio Técnico Torrent S.L.)
## 3. Resumen de resultados
(Cuántas vulnerabilidades por severidad — puedes usar una tabla o una lista)
| Severidad | Cantidad |  Descripción |
|----------|------------|---------------|
Crítica | 5 | Es un riesgo extremo y compromete al sistema de forma directa |
Alta | 1 | Riesgo alto que puede comprometer la seguridad como permitir accesos no autorizados |
Media | 5 | Es un riesgo moderado como fallos de configuración o información sensible expuesta |
Combinadas | 7 | Son hallazgos múltiples encontradas en puertos/ ¡servicio. Nessus la combina si ve mucha coincidencia entre ellas.|
Baja | 4  | Son pequeños fallos o configuraciones que se pueden mejorar |
Informativas | 44 | información técnica de puertos, servicios y las versiones activas |

## 4. Vulnerabilidades clasificadas
7 CLASIFICAR TRES VULNERABILIDADES
. | Vulnerabilidad | Severidad / CVSS | Origen | Descripción |
--|----------------|------------|------|-------|
1 |  Canonical Ubuntu Linux SEoL (8.04.x)  | Crítica / 10.0 |Uso |El fallo se da porque la versión usada está obsoleta, no recibe mantenimiento y la falta de soporte hace que el proveedor no lance nuevos parches de seguridad dando como consecuencia su expocisión a riesgos graves.| 
2 | rlogin Service Detection| Alta / 7.5   | Diseño   | rlogin está activo en el puerto 513, al ser un servicio remoto antiguo no cumple con los protocolos actuales, los datos viajan en texto plano entre el cliente y el servidor, sin cifrados ni nada.  | 
3 | Unencrypted Telnet Server  | Medium / 6.5 |  Diseño   | Telnet está siendo ejecutada a través de un canal que no tiene cifrados, la información importante como contraseñas, comandos e inicios de sesión se transfieren en texto plano. ( parecido con rlogin)   | 

## 5. Análisis en profundidad
(El desarrollo del paso 8: qué es, cómo se explota, cómo se mitiga, referencia)
## rlogin Service Detection ##

* Qué es
  - El servicio de rlogin es un protocolo de redes que fue basada en Unix/Linux.
lo que hacía es que permitía a los usuarios iniciar sesión en una máquina remota a través de la red,   mediante rlogin, todo el tráfico tanto como nombres de usuarios, contraseñas son transmitidas en texto plano, su autenticación basados en red es débil.

* Cómo se podría explotar
  - Un atacante que estaría en la misma red o capaz de interceptar el tráfico puede suplantar  la IP y también al ser muy predecible con la secuencia en la que se envían los paquetes TCP es un combo con el IP Spoofing. Por último ante cualquier fallo en el se permita modifique el archivo .rhosts puede  lleva a un usuario indeseado entrar con privilegios totales en el sistema.
    
* Cómo se mitigaría
Como medida principal se recomienda desactivar el protocolo rlogin, quitarlo del sistema.
 en el archivo de configuración /etc/inetd.connf y reiniciando el servicio gestor de procesos ineted. Ya estando fuera se podría usar otras alternativas como SSH lo cual tiene los protocolos actuales como el cifrado de extremo a extremo.
* Referencia
  CVE-1999-0651

## 6. Recomendaciones
(2-3 acciones concretas que recomendarías, ordenadas por prioridad)
## 1. Actualizar el sistema operativo y desactivar las aplicaciones obsoletas ##
   - Se debe de actualizar el sistema operativo a una versión que tenga soporte activo por parte del proveedor, así nos aseguramos que se cuenten con los parcheos de vulnerabilidades.
   - También deshabilitar por completo los servicios de acceso remoto en las cuales carecen de mecanismos de seguridad, hablamos de rlogin y Telnet.
## 2. Implantación de protocolos de comunicación cifrados ##
   - Ya quitando los servicios con pocos meccanismos de seguridad que a su vez usan texto plano, se implementaría servicios seguras como SSH lo que garantizará el viaje seguro de los datos previniendo ataques de Sniffing y suplantación en la red.
## 3. Fortalecer y revisar las configuraciones ##
   - cerrar puertos y servicios innecesarios.
   -  asegurarse de que los archivos de sistema solo puedan ser leídos o modificados por quienes deben hacerlo.
   -  Eliminar contraseñas predecibles o de fábricas.
   -  Restringir el acceso a la red, usando los cortafuegos. 
## 7. Conclusión
(Una valoración breve: ¿firmarías el contrato de mantenimiento sabiendo lo que sabes ahora? ¿Con qué condiciones?)
- no la firmaría, mantener esas inseguridades es cavar tu tumba.
Las condiciones para firmar serían:
- incluir la actualización o una migración del sistema operativo a una versión que si tenga soporte activa.
- que se elimine los servicios de red sin cifrado y cambiarlas por otras que si tienen, como SSH.
-  que fortalezcan y aseguren el sistema general en la configuración de los servicios y permisos locales.
