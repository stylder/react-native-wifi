# react-native-android-wifi

<p align="center">
Un módulo nativo de reacción para ver y conectarse a redes Wifi en dispositivos Android.
</p>

<p align="center">
  <a href="https://github.com/stylder">
    <img alt="Made by Santiago García Cabral" src="https://img.shields.io/badge/made-by%20Santiago%20Garc%C3%ADa%20Cabral-green">
  </a>
</p>


![example app](/docs-assets/example-app.gif)



### Instalación

### Agrégalo a tu proyecto de Android
```bash
npm install react-native-android-wifi --save
```

### Instale las dependencias nativas
Utilice el enlace react-native para instalar dependencias nativas automáticamente:
```bash
react-native link react-native-android-wifi
```

### Ejemplo de usp
```javascript
import wifi from 'react-native-android-wifi';
```

Permisos: a partir de la API de Android 25, las aplicaciones deben tener el permiso ACCESS_COARSE_LOCATION (o ACCESS_FINE_LOCATION) para obtener resultados.
```javascript
try {
      const granted = await PermissionsAndroid.request(
        PermissionsAndroid.PERMISSIONS.ACCESS_FINE_LOCATION,
        {
          'title': 'Redes wifi',
          'message': 'Necesitamos su permiso para encontrar redes wifi'
        }
      )
      if (granted === PermissionsAndroid.RESULTS.GRANTED) {
        console.log("¡Gracias! :)");
      } else {
        console.log("No podrá recuperar la lista de redes wifi disponibles");
      }
    } catch (err) {
      console.warn(err)
    }
```

Estado de la conectividad wifi:
```javascript
wifi.isEnabled((isEnabled) => {
  if (isEnabled) {
    console.log("wifi service enabled");
  } else {
    console.log("wifi service is disabled");
  }
});
```

Activar / desactivar el servicio wifi:
```javascript
wifi.setEnabled(true);
```

Iniciar sesión en el dispositivo en una red específica:
> Este método no tiene una devolución de llamada cuando la conexión se realizó correctamente, verifique [este](https://github.com/devstepbcn/react-native-android-wifi/issues/4) problema.
Se agregó soporte para el modo de seguridad wifi 'WPA2 PSK' y manejo de SSID para Lollipop y Kitkat.

```javascript
wifi.findAndConnect(ssid, password, (found) => {
  if (found) {
    console.log("wifi is in range");
  } else {
    console.log("wifi is not in range");
  }
});
```

Desconectarse de la red wifi actual
```javascript
wifi.disconnect();
```

Obtener SSID actual
```javascript
wifi.getSSID((ssid) => {
  console.log(ssid);
});
```

Obtener BSSID actual
```javascript
wifi.getBSSID((bssid) => {
  console.log(bssid);
});
```

Obtenga todas las redes wifi a su alcance
```javascript
/*
wifiStringList is a stringified JSONArray with the following fields for each scanned wifi
{
  "SSID": "El nombre de la red", 
  "BSSID": "La dirección del punto de acceso", 
  "capabilities": "Describe los esquemas de autenticación, administración de claves y cifrado admitidos por el punto de acceso",
  "frequency": "La frecuencia primaria de 20 MHz (en MHz) del canal por el que el cliente se está comunicando con el punto de acceso ",
  "level":" El nivel de señal detectado en dBm, también conocido como RSSI. (Recuerde que es un valor negativo) ", 
  "timestamp": "Marca de tiempo en microsegundos (desde el inicio) cuando se vio este resultado por última vez"
}
*/
wifi.loadWifiList((wifiStringList) => {
  var wifiArray = JSON.parse(wifiStringList);
    console.log(wifiArray);
  },
  (error) => {
    console.log(error);
  }
);
```

connectionStatus devuelve verdadero o falso dependiendo de si el dispositivo está conectado a wifi
```javascript
wifi.connectionStatus((isConnected) => {
  if (isConnected) {
      console.log("is connected");
    } else {
      console.log("is not connected");
  }
});
```

Conecta la intensidad de la señal wifi
```javascript
wifi.getCurrentSignalStrength((level) => {
  console.log(level);
});
```

Conecta la frecuencia wifi
```javascript
wifi.getFrequency((frequency) => {
  console.log(frequency);
})
```

Obtener IP actual
```javascript
wifi.getIP((ip) => {
  console.log(ip);
});
```

Obtener la dirección del servidor DHCP
```javascript
wifi.getDhcpServerAddress((ip) => {
  console.log(ip);
});
```

Eliminar / Olvidar la red Wifi del móvil por SSID, devuelve booleano Este método eliminará la red wifi según el SSID pasado de la lista de dispositivos.
``` javascript
wifi.isRemoveWifiNetwork(ssid, (isRemoved) => {
  console.log("Forgetting the wifi device - " + ssid);
});
```

Inicia el escaneo de la red wifi nativa de Android y la lista de devoluciones Actualiza de forma dura el escaneo wifi de Android, implementado `BroadcastReceiver` para garantizar que detecta automáticamente las nuevas conexiones wifi disponibles.

``` javascript
wifi.reScanAndLoadWifiList((wifiStringList) => {
  var wifiArray = JSON.parse(wifiStringList);
  console.log('Detected wifi networks - ',wifiArray);
},(error)=>{
  console.log(error);
});
```

Método para forzar el uso de wifi. Android de forma predeterminada envía todas las solicitudes a través de datos móviles si el wifi conectado no tiene conexión a Internet.
``` javascript
wifi.forceWifiUsage(true);
```

Método para obtener el estado de conexión de una red forzada (porque lleva algún tiempo configurarla).
``` javascript
wifi.connectionStatusOfBoundNetwork((isBound) => {
    if (isBound) {
        console.log('Network is bound');
    } else {
        console.log('Network isn\'t bound');
    }
});
```


Agregue una red wifi oculta y conéctese a ella.
``` javascript
wifi.connectToHiddenNetwork(ssid, password, (networkAdded) => {});
```