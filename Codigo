import yt_dlp
import os
import shutil
import glob
import time
import sys
from colorama import init, Fore, Back, Style

# Inicializar colorama
init(autoreset=True)

# Configuración de colores
COLOR_PRIMARIO = Fore.GREEN
COLOR_SECUNDARIO = Fore.LIGHTGREEN_EX
COLOR_FONDO = Back.BLACK
COLOR_ERROR = Fore.RED
COLOR_ADVERTENCIA = Fore.YELLOW

def limpiar_pantalla():
    """Limpia la pantalla de la terminal"""
    os.system('cls' if os.name == 'nt' else 'clear')

def mostrar_titulo():
    """Muestra el título estilizado del programa"""
    titulo = r"""
  ___   _   _ _____ ___  _     _____   __        _____ _     _     _____ ____  
 / _ \ | | | |_   _/ _ \| |   | ____|  \ \      / / _ \ |   | |   | ____|  _ \ 
| | | || | | | | || | | | |   |  _| ____\ \ /\ / / | | | |   | |   |  _| | | | |
| |_| || |_| | | || |_| | |___| |__|_____\ V  V /| |_| | |___| |___| |___| |_| |
 \__\_\ \___/  |_| \___/|_____|_____|     \_/\_/  \___/|_____|_____|_____|____/ 
    """
    print(COLOR_PRIMARIO + COLOR_FONDO + titulo + Style.RESET_ALL)
    print(COLOR_SECUNDARIO + " " * 20 + "YouTube Downloader - Terminal Edition" + Style.RESET_ALL)
    print(COLOR_SECUNDARIO + "=" * 70 + Style.RESET_ALL)

def animacion_carga(duracion=2, mensaje="Cargando"):
    """Muestra una animación de carga"""
    print(COLOR_PRIMARIO + mensaje, end='', flush=True)
    for _ in range(duracion * 5):
        time.sleep(0.2)
        print(COLOR_PRIMARIO + '.', end='', flush=True)
    print(Style.RESET_ALL)

def mostrar_menu():
    """Muestra el menú de opciones"""
    print("\n" + COLOR_PRIMARIO + "[1]" + COLOR_SECUNDARIO + " Descargar video")
    print(COLOR_PRIMARIO + "[2]" + COLOR_SECUNDARIO + " Descargar música (MP3)")
    print(COLOR_PRIMARIO + "[0]" + COLOR_SECUNDARIO + " Salir\n")

def mover_archivos_a_musica(patron_busqueda, carpeta_destino="~/storage/music"):
    """
    Busca archivos que coincidan con el patrón y los mueve a la carpeta de música.
    """
    try:
        carpeta_destino = os.path.expanduser(carpeta_destino)
        if not os.path.exists(carpeta_destino):
            os.makedirs(carpeta_destino)

        archivos_descargados = glob.glob(patron_busqueda)

        for archivo in archivos_descargados:
            if os.path.exists(archivo):
                shutil.move(archivo, os.path.join(carpeta_destino, os.path.basename(archivo)))
                print(COLOR_SECUNDARIO + f"✓ Archivo movido: {os.path.basename(archivo)} -> {carpeta_destino}")
            else:
                print(COLOR_ADVERTENCIA + f"⚠ Archivo no encontrado: {archivo}")
    except Exception as e:
        print(COLOR_ERROR + f"✖ Error al mover archivos: {str(e)}")

def descargar_video(urls):
    """Descarga videos de YouTube"""
    try:
        opciones_video = {
            'format': 'best',
            'outtmpl': '%(title)s.%(ext)s',
            'progress_hooks': [progreso_descarga],
        }

        with yt_dlp.YoutubeDL(opciones_video) as ydl:
            print(COLOR_PRIMARIO + "\nIniciando descarga de videos...\n")
            ydl.download(urls)
            print(COLOR_PRIMARIO + "\n✓ Descarga de videos completada!\n")

            mover_archivos_a_musica("*.mp4", "~/storage/dcim/Camera")
    except Exception as e:
        print(COLOR_ERROR + f"\n✖ Error al descargar videos: {str(e)}\n")

def descargar_musica(urls):
    """Descarga música de YouTube en formato MP3"""
    try:
        opciones_audio = {
            'format': 'bestaudio/best',
            'outtmpl': '%(title)s.%(ext)s',
            'postprocessors': [{
                'key': 'FFmpegExtractAudio',
                'preferredcodec': 'mp3',
                'preferredquality': '192',
            }],
            'ffmpeg_location': '/data/data/com.termux/files/usr/bin/ffmpeg',
            'progress_hooks': [progreso_descarga],
        }

        with yt_dlp.YoutubeDL(opciones_audio) as ydl:
            print(COLOR_PRIMARIO + "\nIniciando descarga de música...\n")
            ydl.download(urls)
            print(COLOR_PRIMARIO + "\n✓ Descarga de música completada!\n")

            mover_archivos_a_musica("*.mp3")
    except Exception as e:
        print(COLOR_ERROR + f"\n✖ Error al descargar música: {str(e)}\n")

def progreso_descarga(d):
    """Muestra el progreso de la descarga"""
    if d['status'] == 'downloading':
        porcentaje = d.get('_percent_str', 'N/A')
        velocidad = d.get('_speed_str', 'N/A')
        tiempo_restante = d.get('_eta_str', 'N/A')
        
        sys.stdout.write(COLOR_SECUNDARIO + f"\rProgreso: {porcentaje} | Velocidad: {velocidad} | Tiempo restante: {tiempo_restante}")
        sys.stdout.flush()
    elif d['status'] == 'finished':
        sys.stdout.write(COLOR_PRIMARIO + "\r✓ Descarga completada. Procesando...\n")
        sys.stdout.flush()

def ingresar_urls():
    """Permite al usuario ingresar múltiples URLs"""
    urls = []
    print(COLOR_SECUNDARIO + "\nIngresa las URLs de YouTube (máx. 10). Deja en blanco para terminar:\n")
    
    for i in range(10):
        url = input(COLOR_PRIMARIO + f"URL {i+1} > " + COLOR_SECUNDARIO)
        if url.strip() == "":
            break
        urls.append(url)
    
    return urls

def main():
    limpiar_pantalla()
    mostrar_titulo()
    animacion_carga(mensaje="Iniciando sistema")
    
    while True:
        mostrar_menu()
        opcion = input(COLOR_PRIMARIO + ">>> " + COLOR_SECUNDARIO)
        
        if opcion == "0":
            print(COLOR_PRIMARIO + "\nSaliendo del programa...")
            animacion_carga(1, "Cerrando")
            break
        elif opcion in ["1", "2"]:
            urls = ingresar_urls()
            
            if not urls:
                print(COLOR_ADVERTENCIA + "\n⚠ No se ingresaron URLs. Volviendo al menú...")
                time.sleep(1.5)
                continue
            
            if opcion == "1":
                descargar_video(urls)
            else:
                descargar_musica(urls)
            
            input(COLOR_SECUNDARIO + "\nPresiona Enter para continuar...")
            limpiar_pantalla()
            mostrar_titulo()
        else:
            print(COLOR_ERROR + "\n✖ Opción no válida. Intenta nuevamente.")
            time.sleep(1)

if __name__ == "__main__":
    main()
