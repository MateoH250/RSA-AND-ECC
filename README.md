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
def generar_claves_rsa():
    private_key = rsa.generate_private_key(public_exponent=65537, key_size=2048)
    return private_key, private_key.public_key()
private_key_rsa, public_key_rsa = generar_claves_rsa()

# Generar claves ECC
def generar_claves_ecc():
    private_key = ec.generate_private_key(ec.SECP256R1())
    return private_key, private_key.public_key()
private_key_ecc, public_key_ecc = generar_claves_ecc()

# Implementación de cifrado poscuántico basado en Kyber (simulado sin librerías adicionales)
def cifrar_kyber_simulado(mensaje):
    clave_simulada = hashlib.sha3_512(mensaje.encode()).digest()
    return hashlib.sha3_512(clave_simulada + mensaje.encode()).hexdigest()

# Cifrado de monto con SHA3-512 (no reversible)
def encriptar_monto_sha3_512(monto):
    monto_str = str(monto).encode()
    return hashlib.sha3_512(monto_str).hexdigest()

# Verificar usuario
def verificar_usuario(b):
    nombre_usuario = nombre_texto.value.strip()
    if nombre_usuario in clientes_validos:
        mensaje_label.value = f"Bienvenido {nombre_usuario}!"
    else:
        mensaje_label.value = "Usuario no válido. Intenta con un cliente registrado."

# Cifrar mensaje
def cifrar_mensaje(b, metodo):
    mensaje = texto_input.value.strip()
    if not mensaje:
        mensaje_label.value = "Ingresa un mensaje válido para cifrar."
        return

    if metodo == "RSA":
        ciphertext = public_key_rsa.encrypt(
            mensaje.encode(),
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        mensaje_label.value = f"Cifrado RSA: {ciphertext.hex()}"
    elif metodo == "Kyber":
        mensaje_label.value = f"Cifrado Kyber (Simulado): {cifrar_kyber_simulado(mensaje)}"
    elif metodo == "ECC":
        shared_key = private_key_ecc.exchange(ec.ECDH(), public_key_ecc)
        hashed_key = hashlib.sha3_512(shared_key).hexdigest()
        mensaje_label.value = f"Cifrado ECC: {hashed_key}"

# Cifrar transacción
def cifrar_transaccion(b, metodo):
    destinatario = destinatario_texto.value.strip()
    monto = monto_texto.value.strip()
    if not destinatario or not monto:
        mensaje_label.value = "Por favor ingresa el destinatario y el monto."
        return

    monto_encriptado = encriptar_monto_sha3_512(monto)
    mensaje_label.value = f"Transacción Cifrada ({metodo}): Destinatario: {destinatario}, Monto: {monto_encriptado}"

# Crear widgets
nombre_texto = widgets.Text(placeholder='Ingresa tu nombre', description='Nombre:')
texto_input = widgets.Text(placeholder='Mensaje a cifrar', description='Texto:')
destinatario_texto = widgets.Text(placeholder='Ingresa destinatario', description='Destinatario:')
monto_texto = widgets.Text(placeholder='Ingresa monto', description='Monto:')

boton_verificar = widgets.Button(description="Verificar Usuario")
opcion_rsa = widgets.Button(description="Cifrar con RSA")
opcion_kyber = widgets.Button(description="Cifrar con Kyber (Simulado)")
opcion_ecc = widgets.Button(description="Cifrar con ECC")
opcion_rsa_trans = widgets.Button(description="Cifrar Transacción con RSA")
opcion_kyber_trans = widgets.Button(description="Cifrar Transacción con Kyber (Simulado)")
opcion_ecc_trans = widgets.Button(description="Cifrar Transacción con ECC")

mensaje_label = widgets.Label(value="Por favor ingresa tu nombre de usuario.")

# Enlazar eventos
boton_verificar.on_click(verificar_usuario)
opcion_rsa.on_click(lambda b: cifrar_mensaje(b, "RSA"))
opcion_kyber.on_click(lambda b: cifrar_mensaje(b, "Kyber"))
opcion_ecc.on_click(lambda b: cifrar_mensaje(b, "ECC"))
opcion_rsa_trans.on_click(lambda b: cifrar_transaccion(b, "RSA"))
opcion_kyber_trans.on_click(lambda b: cifrar_transaccion(b, "Kyber"))
opcion_ecc_trans.on_click(lambda b: cifrar_transaccion(b, "ECC"))

# Mostrar widgets
display(
    nombre_texto, boton_verificar, mensaje_label, texto_input,
    opcion_rsa, opcion_kyber, opcion_ecc,
    destinatario_texto, monto_texto,
    opcion_rsa_trans, opcion_kyber_trans, opcion_ecc_trans
)
