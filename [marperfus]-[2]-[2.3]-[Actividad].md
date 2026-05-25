# Actividades 3 - Active Directory

## Creación de un bosque nuevo

**Objetivo:**  
El propósito de esta práctica es realizar la creación y configuración de un bosque de dominios en el sistema operativo Microsoft Windows Server 2019.

---

### Pasos para el despliegue de la máquina  
*(Nota: Se omiten ciertos detalles de configuración básica ya que fueron explicados en actividades previas).*

#### Despliegue en Microsoft Azure
1. Accedemos a la plataforma con nuestra cuenta de Gmail, vinculándola correctamente a la cuenta del instituto para poder disponer del saldo asignado. (En caso de restricciones con el correo, cabe recordar que se debe añadir el prefijo "alu" a la dirección de Gmail).
2. Tras la validación, nos desplazamos hasta la sección de "Máquinas virtuales".
3. En este apartado configuramos los parámetros de hardware de la máquina y seleccionamos la imagen ISO correspondiente a Windows Server 2019.
4. Dado que el saldo para las prácticas es limitado, resulta fundamental elegir una configuración económica. Asimismo, es crucial acordarse de apagar la máquina siempre que no se use, ya que al crearse se inicia de forma automática y comienza a consumir el crédito.
   - **Incidencia personal:** Por desconocimiento previo, aprendí esta lección de la peor manera al quedarme sin saldo en Azure, viéndome en la obligación de continuar y realizar la práctica de manera local utilizando VirtualBox.
 
*(Nota adicional sobre la instalación en la nube: Durante el proceso de creación, varios compañeros experimentamos dificultades al elegir la región del servidor debido a restricciones de disponibilidad. La solución consistió en revisar el menú desplegable y seleccionar zonas compatibles; en mi caso particular, utilicé los servidores de Francia y Australia).*

#### Despliegue en VirtualBox

1. Ejecutamos el entorno de VirtualBox, añadimos una nueva máquina e importamos la ISO de Windows Server 2019 previamente descargada.
2. Para asegurar un rendimiento óptimo y un funcionamiento fluido del sistema, asigné una configuración de hardware basada en 4 GB de memoria RAM y 2 núcleos de procesador.
3. Una vez validados todos los parámetros, finalizamos el asistente para proceder a la creación de la máquina virtual.

---

### Diagrama de flujo - Creación de un bosque nuevo

```mermaid
flowchart TD
    A[Inicio] --> B[Abrir Administrador del Servidor]
    B --> C[Acceder a Administrar > Agregar roles y características]
    C --> D[Avanzar en el asistente hasta el apartado Roles de Servidor]
    D --> E[Seleccionar Servicios de dominio de Active Directory y Servidor DNS]
    E --> F[Continuar con el asistente y proceder a la instalación]
    F --> G[Reiniciar la máquina virtual para aplicar correctamente los cambios]
    G --> H[Confirmar la instalación exitosa de Active Directory]
    H --> I[Iniciar la configuración final del dominio mediante la opción Promover este servidor a controlador de dominio]
