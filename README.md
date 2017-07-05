# Plantilla para un Plugin de Cordova
======

Plantilla base para construir un plugin de iOS y Android.

======
# ¿Qué es un plugin de Cordova?

Cordova es un conjunto de herramientas que funcionan como puente para crear aplicaciones nativas e hibridas que se comunican a tráves de código Javascript.

Ese puente nos permite hacer cosas sencillas o tan complejas nativas que no se incorporan a los estandares de la Web.

Construir plugins de Cordova significa qu estamos escribiendo algo de JavaScript para invocar a algún código nativo (Objetive-c, Java, entre otros) que tambien deberemos de escribir y devolver el resultado a nuestro JavaScript.

En resumen, construimos un plugin de Cordova cuando queremos hacer algo nativo que aún no puede realizar el WebKit, como acceder a los datos de HealthKit, al scanner de huella, a la conexión bluetooth o a un SDK de terceros que permiten la conexión con dispositivos como impresoras y lectores.

# Construcción de nuestro propio Plugin:

Frameworks como Ionic cuentan con una libreria extensa de herramientas nativas en las cuales posiblemente encontrarás lo que buscas [IonicFramework](https://ionicframework.com/docs/native/), pero ¿cómo hacer uno propio? Bueno Cordova habilita una documentación para este fin: [Plugin Development Guide](https://cordova.apache.org/docs/en/7.x/guide/hybrid/plugins/index.html), sin embargo despues de mucho buscar y probar diferentes alternativas me guié finalmente por esta página: [How to write Cordova Plugins](https://medium.com/ionic-and-the-mobile-web/how-to-write-cordova-plugins-864e40025f2) la cúal se basa en una plantilla ya creada por el equipo de IONIC-TEAM [ionic-team/cordova-plugin-template](https://github.com/ionic-team/cordova-plugin-template) para clonar e instalar en nuestro proyecto, con el fin de entender y documentar un poco más, me decidi a en base a dicha plantilla crear una propia.

## Paso 1:

Posicionarse en el directorio de proyectos de Ionic en mi caso: `cd Documents/desarrollo/proyectos/ionic/...`

## Paso 2:

Crear un proyecto de Ionic: `ionic start nombreProyecto blank`
Seleccionar proyecto: `cd nombreProyecto/`

## Paso 3:

Instalar Plugin: `ionic cordova plugin add https://github.com/thecouk/ionic-cordova-plugin-template.git`

## Paso 4:

Modificar el archivo **home.ts** de la siguiente manera:

# Archivo home.ts:

```
import { Component } from '@angular/core';
//Agregar Platform para poder evaluar si ya se cargo y esta lista la plataforma
import { NavController, Platform } from 'ionic-angular';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  constructor(public navCtrl: NavController,public platform: Platform) {
  }

  ionViewDidLoad() {  
    //Verifica si ya se encuentra lista la plataforma  
    this.platform.ready().then(() => {
          //Realiza el llamado al plugin e invoca segun el resultado la funcion correspondiente
          (<any>window).MiPlugin.saludo('Mundo!!!', this.successCallback, this.errorCallback);
    });
  }

  //Funcion para desplegar la respuesta cuando es satisfactorio
  successCallback(message){
      alert(message);
  }

  //Funcion si hubo un error
  errorCallback(){
      alert("Hubo un error");
  }

}

```

## Paso 5:

Ejecutar a iOS: `ionic cordova run ios --prod` o para Android: `ionic cordova run android --prod`.

Posteriormente puedes ir al directorio **platforms** donde encontrarás la carpeta de cada sistema operativo y podrás abrirlo en Xcode o Android Studio y posteriormente correr la aplicación.

**LISTO** Si todo va bien verás una alerta "Hola todo el... Mundo!!!", basicamente lo que estas viendo en esa alerta es la mezcla de lo hibrido con lo nativo. De aquí en más ya puedes agregar la complejidad que desees a tu aplicación.

**NOTA:** En tu proyecto podrás observar que dentro de la carpeta de **plugins** encontrarás la carpeta **mi-plugin** que a su vez contiene los archivos de definición y las carpetas que nos interesa entender y modificar **www/** y **src/** en la primera encontraremos el código Javascript que pone en uso la libreria de Cordova y nos permite definir las funciones que pondremos a disposición para nuestra aplicación.

```
var exec = require('cordova/exec');

var PLUGIN_NAME = 'MiPlugin';

var MiPlugin = {
  saludo: function (name, successCallback, errorCallback){
        exec(successCallback, errorCallback, PLUGIN_NAME, "saludar", [name]);
  }
};

module.exports = MiPlugin;

```

En la segunda carpeta encontraremos en el caso de iOS dos archivos: **MiPlugin.h** y **MiPlugin.m** los cuales como te imaginaras son los archivos donde definiremos las funciones nativas que recibiran parametros y devolveran una respusta. En el caso de Android encontrarás una estructura de carpetas **"Es necesario respetarla"** ejemplo: **android/com/example/MiPlugin.java** donde de igual manera encontrarás las funciones para cuando ejecutes en Android.

Fuente de referencia:

https://medium.com/ionic-and-the-mobile-web/how-to-write-cordova-plugins-864e40025f2
