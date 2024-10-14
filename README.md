# WLAN Security

Las **WLANs** (Redes de Área Local Inalámbricas) permiten la conexión de dispositivos a una red sin la necesidad de cables, utilizando el estándar **IEEE 802.11**. Esta tecnología facilita el acceso a Internet y a otros servicios de red, proporcionando movilidad y flexibilidad.

Como las comunicaciones se transmiten a través de ondas de radio, cualquier atacante cercano puede intentar interceptarlas o incluso acceder a la red si no se implementan medidas de seguridad adecuadas.

### **Proceso de comunicacion 802.11**

La comunicación sigue estos pasos

**Discovery (Descubrimiento)**: Los dispositivos clientes escanean el entorno en busca de redes, o bien pasivamente esperando tramas **Beacon**, o activamente enviando solicitudes de sondeo (**Probe Request**). El punto de acceso responde con una **Probe Response**, proporcionando detalles sobre la red.

**Autenticación**: El cliente envía una **Authentication Frame** al punto de acceso, iniciando el proceso de autenticación. Dependiendo del método de autenticación, se pueden intercambiar claves (como en **WPA2-PSK**) o utilizar un servidor **RADIUS** (en **WPA2-Enterprise** o **WPA3**).

**Asociación**: Tras la autenticación exitosa, el cliente envía una solicitud de asociación (**Association Request Frame**), y el punto de acceso responde con una confirmación (**Association Response Frame**).

*Una vez que el cliente está asociado, puede comenzar a intercambiar **tramas de datos** con otros dispositivos en la red.*

### **Tipos de Tramas en 802.11**

Las tramas o frames 802.11 son estructuras de datos que contienen información importante para la transmisión de datos y el control de la red inalámbrica. Entender cómo funcionan las tramas es crucial para comprender tanto el funcionamiento de la red como las vulnerabilidades asociadas.

Las tramas 802.11 se dividen en tres categorías:

<img align="center" src="/img/1ºimagenn.PNG"  />

1. **Tramas de Gestión (Management Frames)**: Son esenciales para establecer y mantener la comunicación entre dispositivos inalámbricos y puntos de acceso. No contienen datos útiles, pero son necesarias para gestionar el estado de la conexión.
   - **Beacon Frames**: Son enviadas periódicamente por los puntos de acceso para anunciar la presencia de la red. Contienen el **SSID**, información sobre las capacidades del punto de acceso (como cifrado, velocidad), y detalles de la red.
   - **Authentication Frames**: Son utilizadas durante el proceso de autenticación entre el cliente y el punto de acceso. Si se utiliza una autenticación de código compartido (como WPA2).
   - **Association Request/Response**: Cuando un cliente se conecta a un punto de acceso, envía una trama de solicitud de asociación. El punto de acceso responde con una trama de respuesta de asociación, que contiene información sobre las capacidades aceptadas.
   - **Deauthentication/Disassociation**: Son tramas que se envían para desconectar a un cliente de la red. Estas tramas no están cifradas, lo que las hace vulnerables a ataques de desautenticación.

2. **Tramas de Control (Control Frames)**: Se utilizan para ayudar a la entrega de tramas de datos, controlando el acceso al medio inalámbrico y asegurando que las transmisiones se realicen de manera eficiente.

   - **Request to Send (RTS)/Clear to Send (CTS)**: Estas tramas se utilizan para evitar colisiones en la transmisión de datos. El dispositivo que quiere transmitir primero envía una trama **RTS**, y si la red está libre, el receptor envía una **CTS** como autorización para enviar datos.

   - **Acknowledgement (ACK)**: Después de que una trama de datos se envía correctamente, el receptor envía una trama de **ACK** para confirmar la transmisión.

3. **Tramas de Datos (Data Frames)**: Contienen los datos útiles que se transmiten entre dispositivos. Pueden ser mensajes de correo electrónico, archivos, o cualquier otro tipo de información transferida en la red.

   - **Sin cifrado**: Si una red no está utilizando cifrado (como **WEP**, **WPA2** o **WPA3**), las tramas de datos se envían en texto plano, lo que permite a los atacantes capturarlas y leer su contenido.

   - **Cifradas**: En redes seguras con WPA2 o WPA3, las tramas de datos están cifradas, lo que protege la información incluso si el atacante intercepta las tramas.

### **Vulnerabilidades Comunes en las WLANs y posibles soluciones**.

**SSID Cloaking (Ocultación del SSID)**:  El **SSID** (Identificador del Conjunto de Servicios) es el nombre de la red Wi-Fi. Algunos administradores intentan mejorar la seguridad ocultando el SSID para que no sea visible en las listas de redes disponibles.

Aunque oculta el nombre de la red, este método es fácilmente derrotado por atacantes experimentados, ya que las tramas de red aún contienen el SSID. Los sniffers de red como **Wireshark** pueden descubrirlo con facilidad.

*En lugar de depender solo de ocultar el SSID, es más efectivo utilizar una autenticación y cifrado fuertes (como WPA3) para proteger la red.*

------

**MAC Filtering**: (Filtrado de Direcciones MAC): Permite que solo los dispositivos con direcciones MAC específicas se conecten a la red.

Los atacantes pueden falsificar direcciones MAC (técnica conocida como **MAC Spoofing**) para imitar la de un dispositivo autorizado. Las herramientas de sniffing pueden identificar direcciones MAC permitidas en la red, facilitando la suplantación.

*Aunque el filtrado MAC puede proporcionar una capa adicional de control, no debe ser considerado un mecanismo de seguridad principal. Se recomienda su uso combinado con protocolos de seguridad avanzados como WPA3. También es crucial mantener un monitoreo constante de los dispositivos conectados para detectar posibles ataques de suplantación.*

------

**802.11 Original Authentication**: El estándar original **802.11** utiliza métodos de autenticación débiles como el **WEP** (Wired Equivalent Privacy), que se basa en claves estáticas de cifrado.

El **WEP** es extremadamente inseguro y ha sido vulnerado ampliamente. Los atacantes pueden romper este cifrado en minutos utilizando herramientas como **Aircrack-ng**, obteniendo acceso a la red.

*Es esencial migrar a estándares de cifrado más modernos como **WPA3**, que utiliza cifrado basado en estándares actuales mucho más seguros como **AES** (Advanced Encryption Standard). También se recomienda deshabilitar cualquier forma de WEP o WPA1 en los puntos de acceso.*

------

**WPA2 PSK (Pre-Shared Key)**: WPA2-PSK ha sido el estándar de seguridad predominante, utiliza una clave precompartida que, si es débil o reutilizada, puede ser fácilmente vulnerada mediante ataques de diccionario o fuerza bruta.

Si la clave compartida es predecible o débil, los atacantes pueden romperla y obtener acceso a la red. Además, ataques de desautenticación pueden forzar a los dispositivos a reconectarse, permitiendo capturar el handshake (intercambio de claves) y facilitar la obtención de la clave.

*Utilizar **WPA3** con **SAE (Simultaneous Authentication of Equals)**, que es un protocolo más seguro para la autenticación de claves. También se recomienda configurar contraseñas robustas y deshabilitar los handshakes antiguos si no son necesarios.*

------

**WPA3 Personal and Shared Key Authentication**: WPA3 introduce mejoras significativas sobre WPA2, como el uso de **SAE** en lugar de PSK, lo que hace que las claves compartidas sean más seguras y evita ataques de fuerza bruta o diccionario.

Aunque **WPA3** es mucho más seguro que sus predecesores, es posible que no esté implementado adecuadamente en todos los dispositivos, lo que puede exponer la red a ataques basados en configuraciones deficientes.

*Implementar **WPA3** con SAE para la autenticación personal y asegurarse de que todos los dispositivos de la red sean compatibles con este estándar. También es importante utilizar **802.1X** para redes empresariales, ya que proporciona un nivel adicional de seguridad a través de un servidor de autenticación.*

------

**Rogue Access Points (Puntos de Acceso Falsos)**: Un atacante puede colocar un punto de acceso no autorizado en la red, engañando a los usuarios para que se conecten a él.

*Utilizar **WIDS/WIPS** (Wireless Intrusion Detection/Prevention Systems) para detectar y bloquear puntos de acceso no autorizados. Además, mantener una política de auditoría constante en la red inalámbrica.*