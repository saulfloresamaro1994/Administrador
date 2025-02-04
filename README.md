# Administradorimport requests
import json

class MediaServerManager:
    def __init__(self, emby_url, emby_api_key, plex_url, plex_token):
        self.emby_url = emby_url
        self.emby_api_key = emby_api_key
        self.plex_url = plex_url
        self.plex_token = plex_token

    def get_emby_libraries(self):
        """Obtiene las bibliotecas de Emby."""
        url = f"{self.emby_url}/Library/SelectableMediaFolders"
        headers = {
            "X-Emby-Token": self.emby_api_key
        }
        response = requests.get(url, headers=headers)
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Error al obtener las bibliotecas de Emby: {response.status_code}")
            return None

    def get_plex_libraries(self):
        """Obtiene las bibliotecas de Plex."""
        url = f"{self.plex_url}/library/sections"
        headers = {
            "X-Plex-Token": self.plex_token
        }
        response = requests.get(url, headers=headers)
        if response.status_code == 200:
            return response.json()
        else:
            print(f"Error al obtener las bibliotecas de Plex: {response.status_code}")
            return None

    def refresh_emby_library(self, library_id):
        """Refresca una biblioteca específica en Emby."""
        url = f"{self.emby_url}/Library/Refresh"
        headers = {
            "X-Emby-Token": self.emby_api_key
        }
        data = {
            "Id": library_id
        }
        response = requests.post(url, headers=headers, json=data)
        if response.status_code == 204:
            print(f"Biblioteca {library_id} refrescada con éxito en Emby.")
        else:
            print(f"Error al refrescar la biblioteca en Emby: {response.status_code}")

    def refresh_plex_library(self, library_id):
        """Refresca una biblioteca específica en Plex."""
        url = f"{self.plex_url}/library/sections/{library_id}/refresh"
        headers = {
            "X-Plex-Token": self.plex_token
        }
        response = requests.get(url, headers=headers)
        if response.status_code == 200:
            print(f"Biblioteca {library_id} refrescada con éxito en Plex.")
        else:
            print(f"Error al refrescar la biblioteca en Plex: {response.status_code}")

# Ejemplo de uso
if __name__ == "__main__":
    # Configuración de Emby
    emby_url = "http://your_emby_server_url"
    emby_api_key = "your_emby_api_key"

    # Configuración de Plex
    plex_url = "http://your_plex_server_url"
    plex_token = "your_plex_token"

    manager = MediaServerManager(emby_url, emby_api_key, plex_url, plex_token)

    # Obtener bibliotecas
    emby_libraries = manager.get_emby_libraries()
    plex_libraries = manager.get_plex_libraries()

    if emby_libraries:
        print("Bibliotecas de Emby:", emby_libraries)
    if plex_libraries:
        print("Bibliotecas de Plex:", plex_libraries)

    # Refrescar una biblioteca específica (ejemplo con ID 1)
    manager.refresh_emby_library(1)
    manager.refresh_plex_library(1)
