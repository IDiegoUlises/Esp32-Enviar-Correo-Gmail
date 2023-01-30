# Esp32-Enviar-Correo-Gmail

### Libreria
<img src="https://github.com/IDiegoUlises/Esp32-Enviar-Correo-Gmail/blob/main/Images/Libreria.png" />

## Codigo de acesso
<img src="https://github.com/IDiegoUlises/Esp32-Enviar-Correo-Gmail/blob/main/Images/Contrasena-de-aplicaciones.png" />


### Codigo
```c++
#include "ESP32_MailClient.h"

//Las credenciales de la red WiFI
const char* ssid = "SSID";
const char* password = "PASSWORD";

String correoRemitente = "Remitente@gmail.com";
String correoDestinatario = "Destinatario@gmail.com";
String codigoAcceso = "yvsvsgawansjobjc";

SMTPData datosSMTP;

void setup()
{
  Serial.begin(115200);
  Serial.println();
  Serial.print("Conectando");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED)
  {
    Serial.print(".");
    delay(200);
  }

  Serial.println();
  Serial.println("Red Wifi conectada!");
  Serial.println();
}

void loop()
{

  datosSMTP.setLogin("smtp.gmail.com", 465, correoRemitente, codigoAcceso); //Establece el host del servidor SMTP y el puerto, la direccion con el codigo de acceso
  datosSMTP.setSender("ESP32", correoRemitente); //Configura el nombre del remitente y el correo del remitente
  datosSMTP.setPriority("High"); //Siempre envia con prioridad alta aunque se cambie por otro valor
  datosSMTP.setSubject("Asunto enviado desde esp32"); //Establece el asunto del correo electronico
  datosSMTP.setMessage("Hola Mundo mensaje enviado desde el Esp32", false); //El mensaje, el segundo valor "false" es para decir sin formato y "true" para insertar codigo html
  datosSMTP.addRecipient(correoDestinatario); //Elige el destinatario

  //Manda el correo
  if (!MailClient.sendMail(datosSMTP))
  {
    Serial.println("Error enviando el correo, " + MailClient.smtpErrorReason()); //Este mensaje muestra si sucede un error en el envio
  }

  //Despues de enviar el correo borra los datos del correo para liberar espacio elimando el objeto datosSMTP
  datosSMTP.empty();

  //Un retardo para no enviar demasiados correos
  delay(100000);
}

```
