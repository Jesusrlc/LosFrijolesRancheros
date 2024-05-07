# ![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/f2bc6fa3-5598-4ed9-b6eb-29ae29574e24)

![image](https://github.com/Jesusrlc/LosFrijolesRancheros/assets/158230496/ba9a644f-0373-4875-bc94-90fc58808099)


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

            port.BaudRate = 9600;
            port.Parity = Parity.None;
            port.DataBits = 8;
            port.StopBits = StopBits.One;

        }

        private void btnConectar_Click(object sender, EventArgs e)
        {
            if (comboBox1.SelectedIndex != 0)
                port.PortName = comboBox1.Text;
            else
            {
                MessageBox.Show("Seleccionar un puerto", "AVISO", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                textBox1.Focus();
            }
        }

        private void btnCerrar_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }

        private void textBox1_Click(object sender, EventArgs e)
        {
            textBox1.Clear();
        }

        private void btnEnviar_Click(object sender, EventArgs e)
        {
            // Enviar datos a la placa PicoW
            port.Open();
            port.Write(textBox1.Text);
            port.Close();
        }

        private void btnReset_Click(object sender, EventArgs e)
        {
            textBox1.Text = "Mensajito a enviar a la picow";
        }
    }
}
```
