# Esp32-Enviar-Correo-Gmail

```c++
#include "ESP32_MailClient.h"
const char* ssid = "tu_red_wifi";
const char* password = "tu_clave_wifi";
SMTPData datosSMTP;

void setup()
{
  Serial.begin(115200);
  Serial.println();
  Serial.print("Conectando");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED)
  {
    Serial.print(".")
    delay(200);
  }

  Serial.println();
  Serial.println("Red Wifi conectada!!!");
  Serial.println();
}
void loop()
{
  datosSMTP.setLogin("smtp.gmail.com", 465, "tu_cuenta_de_correo@gmail.com", "tu_clave_de_correo");
  datosSMTP.setSender("ESP32S", "tu_cuenta_de_correo@gmail.com");
  datosSMTP.setPriority("High");
  datosSMTP.setSubject("Probando envio de correo con ESP32");
  datosSMTP.setMessage("Hola soy el esp32s! y me estoy comunicando contigo", false);
  datosSMTP.addRecipient("direccion_de_destino@correo_cualquiera.com");
  if (!MailClient.sendMail(datosSMTP))
  {
    Serial.println("Error enviando el correo, " + MailClient.smtpErrorReason());
  }

  //Para liberar espacio
  datosSMTP.empty();
  delay(10000);
}

```
