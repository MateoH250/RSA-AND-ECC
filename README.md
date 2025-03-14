# RSA-AND-ECC
git clone https://github.com/usuario/repo.git
cd ruta/al/repositorio
git add .
git add archivo1.py archivo2.js
git commit -m "Descripción de los cambios realizados"
git push origin main

import ipywidgets as widgets
from IPython.display import display

# Función para verificar si el usuario es válido
def verificar_usuario(b):
    nombre_usuario = nombre_texto.value.strip()
    
    # Verificar si el nombre es válido
    if nombre_usuario in ['Cliente A', 'Cliente B', 'Cliente C']:
        mensaje_label.value = f"Bienvenido {nombre_usuario}!"
        mostrar_botones()
    else:
        mensaje_label.value = "Usuario no válido. Intenta con Cliente A, Cliente B o Cliente C."

# Función para mostrar los botones después de la verificación
def mostrar_botones():
    boton_transaccion.layout.display = 'inline-block'
    boton_saludar.layout.display = 'inline-block'
    boton_preguntar.layout.display = 'inline-block'
    boton_despedir.layout.display = 'inline-block'

# Función para mostrar el saludo "Hola"
def saludar(b):
    mensaje_label.value = "¡Hola!"

# Función para preguntar "¿Cómo estás?"
def preguntar_como_estas(b):
    mensaje_label.value = "¿Cómo estás?"

# Función para decir "Adiós"
def despedirse(b):
    mensaje_label.value = "¡Adiós!"

# Función para realizar la transacción
def realizar_transaccion(b):
    cliente = nombre_texto.value.strip()
    transaccion = f"{cliente} envía $500 a Cliente B"
    mensaje_label.value = transaccion

# Crear los widgets
nombre_texto = widgets.Text(
    value='',
    placeholder='Ingresa tu nombre',
    description='Nombre:',
    disabled=False
)

boton_verificar = widgets.Button(description="Verificar Usuario")

boton_saludar = widgets.Button(description="Hola", layout=widgets.Layout(display='none'))
boton_preguntar = widgets.Button(description="¿Cómo estás?", layout=widgets.Layout(display='none'))
boton_despedir = widgets.Button(description="Adiós", layout=widgets.Layout(display='none'))

boton_transaccion = widgets.Button(description="Realizar Transacción", layout=widgets.Layout(display='none'))

# Crear la etiqueta para mostrar los mensajes
mensaje_label = widgets.Label(value="Por favor ingresa tu nombre de usuario.")

# Enlazar los botones con las funciones
boton_verificar.on_click(verificar_usuario)
boton_saludar.on_click(saludar)
boton_preguntar.on_click(preguntar_como_estas)
boton_despedir.on_click(despedirse)
boton_transaccion.on_click(realizar_transaccion)

# Mostrar los widgets
display(nombre_texto, boton_verificar, mensaje_label, boton_saludar, boton_preguntar, boton_despedir, boton_transaccion)
