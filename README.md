import streamlit as st
import zipfile
import os

# Título de la aplicación
st.title("Compresión de Archivos")

# Instrucciones
st.write("Sube tus archivos para comprimirlos en un archivo ZIP.")

# Subir archivos
uploaded_files = st.file_uploader("Elige los archivos", accept_multiple_files=True)

# Botón para comprimir los archivos
if st.button("Comprimir"):
    if uploaded_files:
        # Crear un archivo ZIP
        with zipfile.ZipFile("files.zip", 'w') as zipf:
            for uploaded_file in uploaded_files:
                # Guardar el archivo subido temporalmente
                with open(uploaded_file.name, "wb") as f:
                    f.write(uploaded_file.getbuffer())
                
                # Agregar el archivo al ZIP
                zipf.write(uploaded_file.name)

                # Eliminar el archivo temporal
                os.remove(uploaded_file.name)

        # Mostrar el éxito de la compresión
        st.success("¡Archivos comprimidos con éxito!")

        # Descargar el archivo ZIP
        with open("files.zip", "rb") as f:
            st.download_button(
                label="Descargar Archivo ZIP",
                data=f,
                file_name="files.zip",
                mime="application/zip"
            )
    else:
        st.error("Por favor, sube al menos un archivo para comprimir.")

# Opcional: Limpiar después de la descarga
if os.path.exists("compressed_files.zip"):
    os.remove("compressed_files.zip")