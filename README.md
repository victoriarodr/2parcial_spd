# SEGUNDO PARCIAL SPD
## Integrante
- Victoria Rodriguez Rosas  


## Proyecto: Sistema de incendios


## Descripción
Mi proyecto es un detector de incendios que posee un sensor de temperatura que moviliza un servo si se encuentra en más de 60 grados. Tambien posee un control remoto que da inicio y fin al programa y permite al usuario elegir la estacion del año segun el boton presionado. 1 = invierno, 2 = verano, 3 = primavera, 4 = otoño.

## TOTAL DE FUNCIONES: 5
## Función principal
Se inicializan las variables correspondientes, el servo, el sensor de temperatura y  el control IR para que reciba los botones presionados del control. En el caso de que se presione el boton ENCENDIDO, la bandera del sistema se pone en TRUE y el sistema de incendios se da por comenzado. En el caso de que se presione el boton STOP el sistema se pone en FALSE y el sistema de incendios se apaga.

~~~ C 
void loop()
{
  if (IrReceiver.decode())
  {
    //Serial.println(IrReceiver.decodedIRData.decodedRawData, HEX);
    if (IrReceiver.decodedIRData.decodedRawData == boton_encendido && flag_sistema == false)
    {
      flag_sistema = true;
    }
    else if (IrReceiver.decodedIRData.decodedRawData == boton_apagado)
    {
      flag_sistema = false;
    }
   IrReceiver.resume();
  }
 funcionamientoSistema();
 delay(600);
}

~~~
## Funcionamiento del sistema:
Si la bandera del sistema es FALSE, la led roja estará encendida hasta que la bandera sea TRUE. Cuando el boton de encendido es presionado, la led verde se enciende y se pone en funcionamiento el detector de temperatura y el detector de los botones del control para verificar la estacion del año deseada.

~~~ C 
void funcionamientoSistema()
{
 if(flag_sistema == false)
 {
   lcd.clear();
   digitalWrite(led_verde, LOW);
   digitalWrite(led_rojo, HIGH);
 }
 else
 {
   digitalWrite(led_rojo, LOW);
   digitalWrite(led_verde,HIGH);
   deteccionTemperatura();
   deteccionControl();
 }
  
}

~~~
## Deteccion de temperatura:
La función inicializa la variable lector_sensor para obtener la temperatura del momento. Itera sobre la misma y muestra sus valores en la placa LCD. Si el valor de temperatura es mayor a 60 grados, se da aviso de INCENDIO y se llama a la funcion del servo

~~~ C 
  void deteccionTemperatura()
{
  int lector_sensor = analogRead(sensor_temperatura);
  int temperatura = map(lector_sensor, 20, 358, -40, 125);
  if(temperatura >= 60)
  {
    lcd.print(temperatura);
    lcd.print(" Grados");
    lcd.setCursor(0, 1);
    lcd.print(" ALERTA INCENDIO");
    movimientoServo();
    delay(1000);
    lcd.clear();
  }
  else
  {
    lcd.print(temperatura);
    lcd.print(" Grados");
    delay(3000);
    lcd.clear();
  }
}

~~~
## Movimiento del Servo:
El servo se mueve si hay un incendio de 0 a 180 grados.
~~~ C 
void movimientoServo()
{
  servoMotor.write(0);
  delay(500);
  servoMotor.write(180);
}
~~~
## Deteccion del control:
Se detecta si los botones del control fueron presionados y dependiendo cuál, se muestra la estación del año seleccionada en
la placa LCD.

~~~ C 
void deteccionControl()
{
  if (IrReceiver.decodedIRData.decodedRawData == boton_verano)
  {
      IrReceiver.resume();
      lcd.setCursor(0, 1);
      lcd.print("Verano");
      delay(1000);
   	  lcd.clear();
  }
  else if (IrReceiver.decodedIRData.decodedRawData == boton_invierno)
  {
    IrReceiver.resume();
    lcd.setCursor(0, 1);
    lcd.print("Invierno");
    delay(1000);
    lcd.clear();
  }
  else if (IrReceiver.decodedIRData.decodedRawData == boton_otono)
  {
    IrReceiver.resume();
    lcd.setCursor(0, 1);
    lcd.print("Otono");
    delay(1000);
    lcd.clear();
  }
  else if (IrReceiver.decodedIRData.decodedRawData == boton_primavera)
  {
    IrReceiver.resume();
    lcd.setCursor(0, 1);
    lcd.print("Primavera");
    delay(1000);
    lcd.clear();
  }
}
~~~

## :robot: Link al proyecto
- https://www.tinkercad.com/things/dknaL9yXfZU-2parcial-spd-victoria-rodriguez-rosas-spd/editel?sharecode=KwCwi1sTJODTsnzVKzigJIkDjFArcU2iwxQZZS6wvis
