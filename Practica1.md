# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/f2bc6fa3-5598-4ed9-b6eb-29ae29574e24)

## Introducción
El objetivo de este proyecto es desarrollar una aplicación en C# con una interfaz gráfica que permita interactuar con un dispositivo PicoW a través del puerto serial.

## Descripción de Interfaz Gráfica
* **ComboBox:** Selección de puerto serial
* **Botón 'Conectar':** Establecer conexión
* **Botón 'Desconectar':** Cerrar conexión
* **Botón 'Reset':** Limpiar TextBox
* **TextBox 'TxtBoxMensaje':** Ingreso y despliegue de mensajes

## Diseño de nuestra aplicacion 
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/ba9a644f-0373-4875-bc94-90fc58808099)

## Código utilizado para el desarrollo de la aplicación.
```C#
/*Problema:
Control de Puerto Serial a nuestra picoW desde interfaz visual en C#
Instituto Técnologico de Tijuana
Ing en Sistemas Computacionales

  Autor: 
- Almeida Valles Jose de Jesus @Nickname JoseAlmeida21
- Lopez Contreras Jesús Rafael @Nickname Jesusrlc
- Nuño Reyes Gerardo @Nickname GeraNuno

Fecha: 20/05/2024
Repositorio: https://github.com/Jesusrlc/LosFrijolesRancheros
*/

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

using System.IO.Ports;

namespace Practica1_RaspberryPicoW
{
    public partial class frmPico : Form
    {
        SerialPort port = new SerialPort();

        public frmPico()
        {
            InitializeComponent();

            for (int i = 0; i < 16; i++)
            {
                comboBox1.Items.Add("COM" + i);
            }

        }

        private void btnConectar_Click(object sender, EventArgs e)
        {
            try
            {
                port.PortName = comboBox1.SelectedItem.ToString();
                port.BaudRate = 115200;
                port.DtrEnable = true;

                try
                {
                    port.Open();
                    MessageBox.Show("Raspberry Pico W Conectada");
                }
                catch (Exception ex)
                {
                    MessageBox.Show("No se ha conectado\n\n" + ex);
                }

            }
            catch (Exception a)
            {
                MessageBox.Show("Por favor elige un puerto\n\n" + a);
            }
        }

        private void btnCerrar_Click(object sender, EventArgs e)
        {
            port.Close();
            MessageBox.Show("El puerto ha sido desconectado");
            comboBox1.ResetText();
        }

        private void textBox1_Click(object sender, EventArgs e)
        {
            textBox1.Clear();
        }

        private void btnEnviar_Click(object sender, EventArgs e)
        {
            try
            {
                port.WriteLine(textBox1.Text);
                MessageBox.Show("Texto: \n " + textBox1.Text + "\n\nSe envio correctamente", "AVISO", MessageBoxButtons.OK, MessageBoxIcon.Information);
            }
            catch (Exception ex)
            {
                MessageBox.Show("No se envió" + ex);
            }
        }

        private void btnReset_Click(object sender, EventArgs e)
        {
            textBox1.Clear();
        }

        private void btnSalir_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }
    }
}


```
## Las siguientes imagenes despliegan el resultado de la practica 1
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/7ddd75b3-51d1-4b0f-9d45-82445daf3bca)
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/1a65d976-af67-48d8-94e0-e31d0adee1ac)
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/f73f4173-86d1-44ef-8eef-dfe27aeda7bc)
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/3aed3921-efc5-4e87-a969-04f649e5fdf7)


