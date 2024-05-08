# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/f2bc6fa3-5598-4ed9-b6eb-29ae29574e24)

## La actividad 1 fue basicamente enfocarse en el Desarrollo de una Aplicación en C# para Control de Puerto Serial a nuestra picoW.
## El diseño de nuestra aplicacion 
![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/ba9a644f-0373-4875-bc94-90fc58808099)

## Este fue el codigo utilizado para el desarrollo de la aplicación.
```C#
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
