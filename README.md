# breakout_game

# Descripcion del Form1

# Clase Form1

La clase Form1 es una clase parcial que hereda de Form, lo que significa que es una parte de una definición de clase más grande que está dividida en múltiples archivos. Esta clase representa una ventana o formulario en una aplicación de Windows Forms.

**Constructor Form1()**

public Form1()
{
    InitializeComponent();
}

El constructor de la clase Form1 se llama cuando se crea una instancia de Form1. En este constructor, se llama al método InitializeComponent(), que es generado automáticamente por el Diseñador de Formularios de Windows. Este método configura todos los controles y componentes del formulario (como botones, etiquetas, etc.) y establece sus propiedades.

**Método Salir_Click**

private void Salir_Click(object sender, EventArgs e)
{
    this.Close();
}

Este método se llama cuando se hace clic en un botón u otro control que tiene el evento Click asociado a este método. El propósito de este método es cerrar el formulario actual (Form1). Aquí this.Close() llama al método Close() de la clase Form, que cierra el formulario.

* sender: El objeto que generó el evento. Generalmente, es el control que fue clicado.
* e: Argumentos del evento, que contienen información adicional sobre el evento.

**Método Btn_Jugar_Click**

private void Btn_Jugar_Click(object sender, EventArgs e)
{
    Form2 form = new Form2();
    form.Show();
    this.Hide();
}

Este método se ejecuta cuando se hace clic en un botón u otro control que tiene el evento Click asociado a este método. El propósito de este método es crear y mostrar una nueva instancia de Form2, otro formulario en la aplicación, y ocultar el formulario actual (Form1).

# Descripcion del Form2

# Clase Form2

La clase Form2 es una parte de una aplicación de Windows Forms que parece ser un juego de tipo Breakout/Arkanoid, donde el objetivo es destruir bloques con una pelota controlada por un jugador. 

**Variables**
Boleanas para control de movimiento y estado del juego


bool GoLeft;
bool GoRight;
bool IsGameOver;

* GoLeft: Indica si el jugador está moviendo el control hacia la izquierda.
* GoRight: Indica si el jugador está moviendo el control hacia la derecha.
* IsGameOver: Indica si el juego ha terminado.


**Variables de juego**

int Puntaje;
int pelotaX;
int pelotaY;
int VelocidadJugador;

* Puntaje: Almacena la puntuación del jugador.
* pelotaX: Velocidad horizontal de la pelota.
* pelotaY: Velocidad vertical de la pelota.
* VelocidadJugador: Velocidad a la que se mueve el control del jugador.
* Generador de números aleatorios y arreglo de bloques

Random rnd = new Random();
PictureBox[] bloquesArray;

* rnd: Instancia de la clase Random para generar colores aleatorios y velocidades.
* bloquesArray: Arreglo de PictureBox que representa los bloques en el juego.

**Constructor**

public Form2()
{
    InitializeComponent();
    PlaceBlocks();
    SetupGame();
}

El constructor inicializa los componentes del formulario y llama a los métodos PlaceBlocks y SetupGame para configurar los bloques y el estado inicial del juego.

**Métodos**

* SetupGame

private void SetupGame()
{
    IsGameOver = false;
    Puntaje = 0;
    pelotaX = 5;
    pelotaY = 5;
    VelocidadJugador = 12;
    TxtPuntuacion.Text = "Puntaje: " + Puntaje;
    pelota.Left = 393;
    pelota.Top = 302;
    Desplazador.Left = 315;
    TiempoJuego.Start();

    foreach (Control x in this.Controls)
    {
        if (x is PictureBox && (string)x.Tag == "blocks")
        {
            x.BackColor = Color.FromArgb(rnd.Next(256), rnd.Next(256), rnd.Next(256));
        }
    }
}
Configura el estado inicial del juego: reinicia el puntaje, posiciones de la pelota y el control del jugador, y asigna colores aleatorios a los bloques.

* GameOver

private void GameOver(String Message)
{
    IsGameOver = true;
    TiempoJuego.Stop();
    TxtPuntuacion.Text = "Puntacion: " + Puntaje + " " + Message;
}
Detiene el juego, marca el estado del juego como terminado y muestra un mensaje de fin de juego.

* PlaceBlocks

private void PlaceBlocks()
{
    bloquesArray = new PictureBox[28];
    int a = 0;
    int top = 50;
    int left = 10;

    for (int i = 0; i < bloquesArray.Length; i++)
    {
        bloquesArray[i] = new PictureBox();
        bloquesArray[i].Height = 24;
        bloquesArray[i].Width = 98;
        bloquesArray[i].Tag = "blocks";
        bloquesArray[i].BackColor = Color.White;

        if (a == 7)
        {
            top = top + 50;
            left = 10;
            a = 0;
        }

        if (a < 28)
        {
            a++;
            bloquesArray[i].Left = left;
            bloquesArray[i].Top = top;
            this.Controls.Add(bloquesArray[i]);
            left = left + 110;
        }
    }

    SetupGame();
}
Coloca los bloques en el formulario y configura el juego llamando a SetupGame.

* removerbloques

private void removerbloques()
{
    foreach (PictureBox x in bloquesArray)
    {
        this.Controls.Remove(x);
    }
}
Elimina los bloques del formulario.

* TiempoJuegoEvent

private void TiempoJuegoEvent(object sender, EventArgs e)
{
    TxtPuntuacion.Text = "Puntuacion: " + Puntaje;

    if (GoLeft == true && Desplazador.Left > 0)
    {
        Desplazador.Left -= VelocidadJugador;
    }

    if (GoRight == true && Desplazador.Left < 700)
    {
        Desplazador.Left += VelocidadJugador;
    }

    pelota.Left += pelotaX;
    pelota.Top += pelotaY;

    if (pelota.Left < 0 || pelota.Left > 780)
    {
        pelotaX = -pelotaX;
    }

    if (pelota.Top < 0)
    {
        pelotaY = -pelotaY;
    }

    if (pelota.Bounds.IntersectsWith(Desplazador.Bounds))
    {
        pelota.Top = 396;
        pelotaY = rnd.Next(5, 12) * -1;

        if (pelotaX < 0)
        {
            pelotaX = rnd.Next(5, 12) * -1;
        }
        else
        {
            pelotaX = rnd.Next(-5, 12);
        }
    }

    foreach (Control x in this.Controls)
    {
        if (x is PictureBox && (string)x.Tag == "blocks")
        {
            if (pelota.Bounds.IntersectsWith(x.Bounds))
            {
                Puntaje += 100;
                pelotaY = -pelotaY;
                this.Controls.Remove(x);
            }
        }
    }

    if (Puntaje == 2800)
    {
        GameOver(" You Win!!- - - - Press enter To play again ");
    }

    if (pelota.Top > 450)
    {
        GameOver(" You Lose - - - -  Press enter to play again");
    }
}
Controla el movimiento de la pelota, la detección de colisiones, y actualiza el estado del juego en cada tick del temporizador (TiempoJuego).

* BotonAbajo

private void BotonAbajo(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Left)
    {
        GoLeft = true;
    }
    if (e.KeyCode == Keys.Right)
    {
        GoRight = true;
    }
}
Se ejecuta cuando una tecla se presiona, y actualiza las variables de movimiento del jugador.

* BotonArriba

private void BotonArriba(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Left)
    {
        GoLeft = false;
    }
    if (e.KeyCode == Keys.Right)
    {
        GoRight = false;
    }

    if (e.KeyCode == Keys.Enter && IsGameOver == true)
    {
        removerbloques();
        PlaceBlocks();
    }
}
Se ejecuta cuando una tecla se suelta, y actualiza las variables de movimiento del jugador y reinicia el juego si ha terminado.

# Descripcion del Form2.Designer

**Variables**

* Designer Variable

private System.ComponentModel.IContainer components = null;

*components: Contenedor para los componentes que requiere el formulario.

**Métodos**

* Dispose

protected override void Dispose(bool disposing)
{
    if (disposing && (components != null))
    {
        components.Dispose();
    }
    base.Dispose(disposing);
}
Libera los recursos que está utilizando el formulario.
disposing: Si es true, se liberan tanto los recursos administrados como los no administrados.

* InitializeComponent

* InitializeComponent: Método que inicializa todos los componentes del formulario. Es generado automáticamente y no debe modificarse manualmente.
* components: Inicializa el contenedor de componentes.
* TiempoJuego: Un temporizador que controla el ciclo del juego. Su intervalo se establece en 20 milisegundos y se asocia con el evento Tick.
* pelota: Un PictureBox que representa la pelota del juego.
* Desplazador: Un PictureBox que representa el control del jugador.
* TxtPuntuacion: Un Label que muestra la puntuación actual del jugador.
Se configuran las propiedades del formulario y se asocian los eventos KeyDown y KeyUp con los métodos BotonAbajo y BotonArriba respectivamente.
Componentes del Formulario

* Timer (TiempoJuego)

private System.Windows.Forms.Timer TiempoJuego;

Temporizador que controla la lógica del juego a intervalos regulares. Su evento Tick está asociado al método TiempoJuegoEvent.

* PictureBox (pelota)

private System.Windows.Forms.PictureBox pelota;
Representa la pelota del juego.
Propiedades iniciales: color rojo, tamaño y ubicación establecidos.

* PictureBox (Desplazador)

private System.Windows.Forms.PictureBox Desplazador;
Representa el control del jugador (paddle).
Propiedades iniciales: color púrpura, tamaño y ubicación establecidos.

*Label (TxtPuntuacion)

private System.Windows.Forms.Label TxtPuntuacion;
Muestra la puntuación del jugador.
Propiedades iniciales: texto "Puntuacion: 0", color de texto blanco, fuente en negrita.

**Eventos**

* TiempoJuegoEvent

Método asociado con el evento Tick del Timer. Controla la actualización de la lógica del juego en cada tick del temporizador.
BotonAbajo

Método asociado con el evento KeyDown del formulario. Detecta cuando se presiona una tecla para mover el control del jugador.
BotonArriba

Método asociado con el evento KeyUp del formulario. Detecta cuando se suelta una tecla para detener el movimiento del control del jugador y reiniciar el juego si ha terminado.

# # Descripcion del Program

La clase Program es la clase principal que inicia la aplicación de Windows Forms. Es una clase estática y contiene el punto de entrada principal (Main) del programa.


# Descripcion del Form1.Designer

La clase Form1 es un formulario de Windows Forms que sirve como pantalla principal de la aplicación. Esta clase incluye componentes visuales como botones, imágenes y un panel de degradado. 

**Variables y componentes**

partial class Form1
{
    /// <summary>
    /// Variable del diseñador necesaria.
    /// </summary>
    private System.ComponentModel.IContainer components = null;

    private System.Windows.Forms.PictureBox pictureBox1;
    private System.Windows.Forms.Button Salir;
    private System.Windows.Forms.Button Btn_Jugar;
    private Degradado degradado1;
    private System.Windows.Forms.Timer gameTimer;
}

* components: Es una variable de tipo System.ComponentModel.IContainer que almacena todos los componentes del formulario. Es utilizada para gestionar la limpieza de los recursos utilizados por los componentes.

* pictureBox1: Un componente PictureBox que se utiliza para mostrar una imagen en el formulario.

* Salir: Un botón (Button) que, al ser presionado, cierra la aplicación.

* Btn_Jugar: Un botón (Button) que, al ser presionado, inicia el juego abriendo un nuevo formulario (Form2).

* degradado1: Un control personalizado (Degradado) que se utiliza como fondo del formulario con un efecto de degradado.

* gameTimer: Un temporizador (Timer) que puede ser utilizado para manejar eventos de tiempo dentro del juego.

* Método Dispose

protected override void Dispose(bool disposing)
{
    if (disposing && (components != null))
    {
        components.Dispose();
    }
    base.Dispose(disposing);
}
Este método se encarga de liberar los recursos que están siendo utilizados por los componentes del formulario. Si disposing es true, se liberan tanto los recursos administrados como los no administrados; de lo contrario, solo se liberan los recursos no administrados.

* Método InitializeComponent
Aquí se configuran las propiedades de los controles y se añaden al formulario.

pictureBox1: Configuración del componente PictureBox. Se establece su imagen, posición, tamaño y modo de visualización.

Salir: Configuración del botón Salir. Se establecen sus propiedades de estilo, posición, tamaño y evento de clic.

Btn_Jugar: Configuración del botón Btn_Jugar. Similar al botón Salir, se establecen sus propiedades y evento de clic.

degradado1: Configuración del control personalizado Degradado que se utiliza como fondo del formulario.

gameTimer: Configuración del temporizador Timer, aunque no se establece ninguna propiedad específica en este fragmento.

* Eventos
Salir_Click: Método manejador del evento de clic del botón Salir. Cierra la aplicación cuando el botón es presionado.

Btn_Jugar_Click: Método manejador del evento de clic del botón Btn_Jugar. Abre un nuevo formulario (Form2) y oculta el formulario actual (Form1).
