# Gerador de QRCode através de textos

Nesta aula cosntruiremos um aplicativo capaz de gerar QR Code. Utiizaremos a biblioteca Zxing, uma biblioteca openSource que está em constante revisão pela comunidade.

<img src="https://s4.gifyu.com/images/ezgif-3-efb6bbc803.gif" alt="MarineGEO circle logo" style="height: 500px; "/>

# Importante!

Para adicionar a biblioteca, lembre-se que é necessário adicionar o repositório ao `build.graddle`

```
 implementation 'com.journeyapps:zxing-android-embedded:4.3.0@aar'
 implementation 'com.google.zxing:core:3.2.0'
 
```
# Layout:

```Kotlin
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <TextView
        android:id="@+id/titulo"
        android:textAlignment="center"
        android:textSize="25dp"
        android:layout_marginTop="20dp"
        android:layout_centerHorizontal="true"
        android:text="Transforme textos em QRCode"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

    <ImageView
        android:layout_below="@id/titulo"
        android:src="@drawable/qrvector"
        android:id="@+id/qrcode"
        android:layout_width="230dp"
        android:layout_height="230dp"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="30dp"/>

    <EditText
        android:hint="Insira o texto que deseja converter"
        android:id="@+id/texto"
        android:layout_width="325dp"
        android:layout_height="109dp"
        android:layout_below="@id/qrcode"
        android:layout_marginStart="40dp"
        android:layout_marginTop="40dp"
        android:layout_marginEnd="40dp"
        android:layout_marginBottom="40dp" />

    <Button
        android:id="@+id/btn"
        android:text="Converter"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/texto"
        android:layout_margin="40dp"
        android:layout_marginStart="40dp"
        android:layout_marginTop="40dp"
        android:layout_marginEnd="40dp"
        android:layout_marginBottom="40dp" />
</RelativeLayout>
```

# Código Kotlin MainActivity.kt

```Kotlin
package com.example.qrgenerate

import android.graphics.Bitmap
import android.graphics.Color
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button
import android.widget.EditText
import android.widget.ImageView
import android.widget.Toast
import com.google.zxing.BarcodeFormat
import com.google.zxing.WriterException
import com.google.zxing.qrcode.QRCodeWriter

class MainActivity : AppCompatActivity() {

    private lateinit var QRCode : ImageView
    private lateinit var Texto_inserido : EditText

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        QRCode = findViewById(R.id.qrcode)
        val botao : Button = findViewById(R.id.btn)
        Texto_inserido = findViewById(R.id.texto)

        botao.setOnClickListener {
            val data = Texto_inserido.text.toString().trim()

            if (data.isEmpty()){
                Toast.makeText(this,"Insira algum texto!", Toast.LENGTH_SHORT)

            }
            else{
                val writer = QRCodeWriter()
                try{
                    val bitMatrix = writer.encode(data, BarcodeFormat.QR_CODE, 512,512)
                    val width = bitMatrix.width
                    val height = bitMatrix.height
                    val bmp = Bitmap.createBitmap(width, height, Bitmap.Config.RGB_565)

                    for (x in 0 until width){
                        for (y in 0 until height){
                            bmp.setPixel(x, y, if(bitMatrix[x,y]) Color.BLACK else Color.WHITE)
                        }
                    }
                    QRCode.setImageBitmap(bmp)
                }
                catch (e: WriterException){
                    e.printStackTrace()
                }
            }
        }
    }
}
```
