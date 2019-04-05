Code examples
==================

Java
----

.. code-block:: java

   package se.bibeacon.examples.java;
   
   import java.io.InputStream;
   import java.io.OutputStream;
   import java.net.URL;
   import java.net.URLConnection;
   
   public class SetBeacon {
   
      public static boolean setColor( String systemid, String color ) {
         try {
            URLConnection connection = new URL("https://api.cilamp.se/v1/" + systemid).openConnection();
            connection.setDoOutput(true);
            connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
            OutputStream output = connection.getOutputStream();
            output.write(("color=" + color).getBytes());
            connection.getInputStream();
         } catch( Exception e ) {
            return false;
         }
         return true;
      }
   
      // Usage: <systemid> <color>
      public static void main(String[] args) {
         setColor(args[0], args[1]);
      }
   }



JavaScript
----------

.. code-block:: javascript

   /*
   
   Usage
   =====
   
     const BIBeacon = require('bi-beacon');
   
     const beacon1 = new BIBeacon('beacon1');
     const beacon2 = new BIBeacon('beacon2');
   
     beacon1.color('#F0F0F0');
     beacon2.pulse('#0F0F0F', 1000);
   
   */
   
   const https = require('https');
   const querystring = require('querystring');
   
   class Beacon {
   
     constructor(systemid, {
       beaconHost = 'api.cilamp.se',
       apiVersion = 1,
     } = {}) {
       this._systemid = systemid;
       this._beaconHost = beaconHost;
       this._apiVersion = apiVersion;
     }
   
     color(color) {
       return this._post({ color });
     }
   
     pulse(color, period) {
       return this._post({ color, period });
     }
   
     async _post(data) {
       const path = `/v${ this._apiVersion }/${ this._systemid }/`;
       const postData = querystring.stringify(data);
   
       return new Promise((resovle, reject) => {
         let responseData = '';
   
         const request = https.request(
           {
             method: 'post',
             port: 443,
             host: this._beaconHost,
             path,
             headers: {
               'Content-type': 'application/x-www-form-urlencoded',
             },
           },
           response => {
             response.on('data', chunk => {
               responseData += chunk;
             });
   
             // The whole response has been received
             response.on('end', () => {
               try {
                 const out = JSON.parse(responseData);
   
                 if (response.statusCode === 200) {
                   resovle(out);
                 } else {
                   reject(out);
                 }
               } catch (error) {
                 reject(error);
               }
             });
           },
         );
   
         request.on('error', error => {
           reject(error);
         });
         request.write(postData);
         request.end();
       });
     }
   
   }
   
   module.exports = Beacon;
   
   {
     "name": "bi-beacon",
     "version": "1.0.0",
     "main": "index.js",
     "author": "Gustav Ahlberg <Gustav.Ahlberg@gmail.com>",
     "license": "ISC"
   }



PHP
---

.. code-block:: php

   <?php
   
   function bibeacon_set($channelid, $color, $period, $server="https://api.cilamp.se/v1/") {
      $options = array(
         'http' => array(
            'header'  => "Content-type: application/x-www-form-urlencoded\r\n",
            'method'  => 'POST',
            'content' => http_build_query(
               array("color"=>$color,
                     "period"=>$period))
         )
      );
      $context  = stream_context_create($options);
      $result   = @file_get_contents($server.$channelid, false, $context);
      if ( $result !== FALSE ) {
         if ( ($result = @json_decode($result)) !== FALSE ) {
            if ( @$result->message === "'".$channelid."' updated" ) {
               return TRUE;
            } else { echo "Invalid response: ".json_encode($result); }
         } else { echo "Server response structure error: ".error_get_last()['message']; }
      } else { echo "API Request failed: ".error_get_last()['message']; }
      return FALSE;
   }
   
   if (php_sapi_name() == "cli") {
      if ($argc != 4) {
         echo "Usage: $argv[0] <channelid> <color> <period>\n";
         exit(1);
      } else {
         exit((int)bibeacon_set($argv[1], $argv[2], $argv[3]));
      }
   }
   ?>


shell
-----

.. code-block:: shell

   #!/bin/sh
   
   # Set a BI-Beacon to blue
   curl -X POST -F "color=#0000FF" "https://api.cilamp.se/v1/simple-awesome-monitor"
   
   # Pulse purple slowly
   curl -X POST -F "color=#4400FF" -F "period=3000" "https://api.cilamp.se/v1/simple-awesome-monitor"
   



