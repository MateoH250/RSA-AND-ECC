# RSA-AND-ECC
git clone https://github.com/usuario/repo.git
cd ruta/al/repositorio
git add .
git add archivo1.py archivo2.js
git commit -m "Descripción de los cambios realizados"
git push origin main
import ipywidgets as widgets
from IPython.display import display
import hashlib
import random
import cryptography.hazmat.primitives.asymmetric.rsa as rsa
import cryptography.hazmat.primitives.asymmetric.ec as ec
import cryptography.hazmat.primitives.hashes as hashes
from cryptography.hazmat.primitives.asymmetric import padding

# Lista de clientes válidos
clientes_validos = ['Cliente A', 'Cliente B', 'Cliente C', 'Cliente D', 'Cliente E']

# Generar claves RSA
private_key_rsa = rsa.generate_private_key(public_exponent=65537, key_size=2048)
public_key_rsa = private_key_rsa.public_key()

# Generar claves ECC
private_key_ecc = ec.generate_private_key(ec.SECP256R1())
public_key_ecc = private_key_ecc.public_key()

# Función para cifrar con RSA
def cifrar_rsa(mensaje):
    ciphertext = public_key_rsa.encrypt(
        mensaje.encode(),
        padding.OAEP(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            algorithm=hashes.SHA256(),
            label=None
        )
    )
    return ciphertext.hex()

# Función para descifrar con RSA
def descifrar_rsa(ciphertext):
    plaintext = private_key_rsa.decrypt(
        bytes.fromhex(ciphertext),
        padding.OAEP(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            algorithm=hashes.SHA256(),
            label=None
        )
    )
    return plaintext.decode()

# Función para cifrar con ECC
def cifrar_ecc(mensaje):
    shared_key = private_key_ecc.exchange(ec.ECDH(), public_key_ecc)
    digest = hashes.Hash(hashes.SHA256())
    digest.update(shared_key + mensaje.encode())
    return digest.finalize().hex()

# Función para encriptar el monto con SHA3-512 (no reversible)
def encriptar_monto_sha3_512(monto):
    """Encripta un monto bancario utilizando SHA3-512."""
    monto_str = str(monto).encode()  # Convertir monto a bytes
    return hashlib.sha3_512(monto_str).hexdigest()  # Hash SHA3-512

# Simulación de cifrado cuántico (aleatorio)
def cifrado_cuantico(mensaje):
    """Simula un cifrado cuántico utilizando cambios aleatorios."""
    # Convertir el mensaje en una lista de caracteres
    mensaje_lista = list(mensaje)
    # Realizar un "cambio cuántico" al mezclar aleatoriamente los caracteres
    random.shuffle(mensaje_lista)
    return ''.join(mensaje_lista)

# Función para verificar si el usuario es válido
def verificar_usuario(b):
    nombre_usuario = nombre_texto.value.strip()
    
    if nombre_usuario in clientes_validos:
        mensaje_label.value = f"Bienvenido {nombre_usuario}!"
        mostrar_opciones()
    else:
        mensaje_label.value = "Usuario no válido. Intenta con un cliente registrado."
        ocultar_opciones()

# Función para mostrar las opciones después de la verificación
def mostrar_opciones():
    opcion_mensaje.layout.display = 'inline-block'
    opcion_documento.layout.display = 'inline-block'
    opcion_transaccion.layout.display = 'inline-block'

def ocultar_opciones():
    opcion_mensaje.layout.display = 'none'
    opcion_documento.layout.display = 'none'
    opcion_transaccion.layout.display = 'none'
    texto_input.layout.display = 'none'
    destinatario_texto.layout.display = 'none'
    monto_texto.layout.display = 'none'
    opcion_rsa.layout.display = 'none'
    opcion_ecc.layout.display = 'none'
    opcion_cuantico.layout.display = 'none'
    mensaje_label.value = "Por favor ingresa tu nombre de usuario."

# Función para mostrar u ocultar las subopciones (Texto, Documento, Transacción)
def toggle_subopciones(metodo):
    if metodo == "Mensaje":
        if texto_input.layout.display == 'inline-block':  # Si ya está visible, ocultarlo
            texto_input.layout.display = 'none'
            opcion_rsa.layout.display = 'none'
            opcion_ecc.layout.display = 'none'
            opcion_cuantico.layout.display = 'none'
        else:
            texto_input.layout.display = 'inline-block'
            opcion_rsa.layout.display = 'inline-block'
            opcion_ecc.layout.display = 'inline-block'
            opcion_cuantico.layout.display = 'inline-block'
        # Ocultar las demás
        destinatario_texto.layout.display = 'none'
        monto_texto.layout.display = 'none'
        boton_subir_documento.layout.display = 'none'
    elif metodo == "Documento":
        if boton_subir_documento.layout.display == 'inline-block':  # Si ya está visible, ocultarlo
            boton_subir_documento.layout.display = 'none'
            opcion_rsa.layout.display = 'none'
            opcion_ecc.layout.display = 'none'
            opcion_cuantico.layout.display = 'none'
        else:
            boton_subir_documento.layout.display = 'inline-block'
            opcion_rsa.layout.display = 'inline-block'
            opcion_ecc.layout.display = 'inline-block'
            opcion_cuantico.layout.display = 'inline-block'
        # Ocultar las demás
        texto_input.layout.display = 'none'
        destinatario_texto.layout.display = 'none'
        monto_texto.layout.display = 'none'
    elif metodo == "Transacción":
        if destinatario_texto.layout.display == 'inline-block':  # Si ya está visible, ocultarlo
            destinatario_texto.layout.display = 'none'
            monto_texto.layout.display = 'none'
            opcion_rsa.layout.display = 'none'
            opcion_ecc.layout.display = 'none'
            opcion_cuantico.layout.display = 'none'
        else:
            destinatario_texto.layout.display = 'inline-block'
            monto_texto.layout.display = 'inline-block'
            opcion_rsa.layout.display = 'inline-block'
            opcion_ecc.layout.display = 'inline-block'
            opcion_cuantico.layout.display = 'inline-block'
        # Ocultar las demás
        texto_input.layout.display = 'none'
        boton_subir_documento.layout.display = 'none'

# Función para cifrar un mensaje con RSA, ECC o Cifrado Cuántico
def cifrar_mensaje(b, metodo):
    mensaje = texto_input.value.strip()
    if not mensaje:
        mensaje_label.value = "Ingresa un mensaje válido para cifrar."
        return
    
    if metodo == "RSA":
        mensaje_label.value = f"Mensaje Cifrado (RSA): {cifrar_rsa(mensaje)}"
    elif metodo == "ECC":
        mensaje_label.value = f"Mensaje Cifrado (ECC): {cifrar_ecc(mensaje)}"
    elif metodo == "Cuantico":
        mensaje_label.value = f"Mensaje Cifrado Cuánticamente: {cifrado_cuantico(mensaje)}"

# Función para cifrar una transacción con RSA, ECC o Cifrado Cuántico
def cifrar_transaccion(b, metodo):
    destinatario = destinatario_texto.value.strip()
    monto = monto_texto.value.strip()
    if not destinatario or not monto:
        mensaje_label.value = "Por favor ingresa el destinatario y el monto."
        return
    # Cifrado de monto con SHA3-512
    monto_encriptado = encriptar_monto_sha3_512(monto)
    
    if metodo == "RSA":
        mensaje_label.value = f"Transacción Cifrada (RSA): Destinatario: {destinatario}, Monto: {monto_encriptado}"
    elif metodo == "ECC":
        mensaje_label.value = f"Transacción Cifrada (ECC): Destinatario: {destinatario}, Monto: {monto_encriptado}"
    elif metodo == "Cuantico":
        mensaje_label.value = f"Transacción Cifrada Cuánticamente: Destinatario: {destinatario}, Monto: {cifrado_cuantico(monto_encriptado)}"

# Crear los widgets
nombre_texto = widgets.Text(
    value='',
    placeholder='Ingresa tu nombre',
    description='Nombre:',
    disabled=False
)

texto_input = widgets.Text(
    value='',
    placeholder='Mensaje a cifrar',
    description='Texto:',
    disabled=False
)

destinatario_texto = widgets.Text(
    value='',
    placeholder='Ingresa destinatario',
    description='Destinatario:',
    disabled=False
)

monto_texto = widgets.Text(
    value='',
    placeholder='Ingresa monto',
    description='Monto:',
    disabled=False
)

boton_verificar = widgets.Button(description="Verificar Usuario")

# Opciones de cifrado (Mensaje, Documento, Transacción)
opcion_mensaje = widgets.Button(description="Cifrar Mensaje", layout=widgets.Layout(display='none'))
opcion_documento = widgets.Button(description="Cifrar Documento", layout=widgets.Layout(display='none'))
opcion_transaccion = widgets.Button(description="Cifrar Transacción", layout=widgets.Layout(display='none'))

# Botones de cifrado con RSA, ECC y Cuántico
opcion_rsa = widgets.Button(description="Cifrar con RSA", layout=widgets.Layout(display='none'))
opcion_ecc = widgets.Button(description="Cifrar con ECC", layout=widgets.Layout(display='none'))
opcion_cuantico = widgets.Button(description="Cifrar Cuánticamente", layout=widgets.Layout(display='none'))

# Botones para subir documento
boton_subir_documento = widgets.Button(description="Subir Documento", layout=widgets.Layout(display='none'))

# Etiqueta para mostrar mensajes
mensaje_label = widgets.Label(value="Por favor ingresa tu nombre de usuario.")

# Enlazar eventos
boton_verificar.on_click(verificar_usuario)
opcion_mensaje.on_click(lambda b: toggle_subopciones("Mensaje"))
opcion_documento.on_click(lambda b: toggle_subopciones("Documento"))
opcion_transaccion.on_click(lambda b: toggle_subopciones("Transacción"))
opcion_rsa.on_click(lambda b: cifrar_mensaje(b, "RSA"))
opcion_ecc.on_click(lambda b: cifrar_mensaje(b, "ECC"))
opcion_cuantico.on_click(lambda b: cifrar_mensaje(b, "Cuantico"))
boton_subir_documento.on_click(lambda b: subir_documento(b, "RSA"))

# Mostrar widgets
display(nombre_texto, boton_verificar, mensaje_label, opcion_mensaje, opcion_documento, opcion_transaccion, opcion_rsa, opcion_ecc, opcion_cuantico, texto_input, destinatario_texto, monto_texto, boton_subir_documento)
