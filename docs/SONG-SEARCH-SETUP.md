# 🎵 Guía: Configurar Búsqueda de Canciones

Esta plataforma puede buscar canciones automáticamente desde múltiples fuentes:

## Fuentes Soportadas

1. **Spotify** - Música de alta calidad
2. **YouTube** - Videos y música
3. **SoundCloud** - Música independiente
4. **Base de datos local** - Búsqueda instantánea sin API

## Configuración Rápida (Sin API - Funciona Ahora)

La plataforma incluye una base de datos local de ejemplo que funciona sin configuración.

**Para probar:**
1. Abre `index.html` en tu navegador
2. En el campo de búsqueda escribe: "Eclipse" o "Esperón"
3. ¡Listo! Te mostrará resultados

## Configuración Avanzada (Con APIs Reales)

### Paso 1: Obtener Credenciales

#### Spotify
1. Ve a https://developer.spotify.com/dashboard
2. Crea una aplicación
3. Copia tu `Client ID` y `Client Secret`

#### YouTube
1. Ve a https://console.cloud.google.com/
2. Crea un proyecto
3. Habilita YouTube Data API v3
4. Crea una clave API

#### SoundCloud
1. Ve a https://soundcloud.com/you/apps
2. Registra una aplicación
3. Obtén tu Client ID

### Paso 2: Configurar Variables de Entorno

```bash
# Copia el archivo de ejemplo
cp .env.example .env

# Edita .env con tus credenciales
nano .env
```

Contenido de `.env`:
```
YOUTUBE_API_KEY=tu_clave_aqui
SPOTIFY_CLIENT_ID=tu_id_aqui
SPOTIFY_CLIENT_SECRET=tu_secret_aqui
SOUNDCLOUD_CLIENT_ID=tu_id_aqui
```

### Paso 3: Instalar Dependencias

```bash
npm install
```

### Paso 4: Ejecutar Server

```bash
npm start
```

## Uso desde el Frontend

```javascript
// Buscar canción
const query = "Eclipse Esperón";

// El frontend enviará la solicitud al servidor
fetch(`/api/search-songs?query=${query}`)
  .then(res => res.json())
  .then(results => console.log(results))
```

## Respuesta de Búsqueda

```json
[
  {
    "title": "Eclipse",
    "artist": "Esperón",
    "duration": "3:45",
    "url": "https://open.spotify.com/track/...",
    "image": "url_imagen",
    "source": "Spotify",
    "preview": "url_preview"
  }
]
```

## Formato de Canciones Almacenadas

```javascript
{
  "title": "Nombre de la canción",
  "artist": "Artista",
  "duration": "3:45",
  "durationMs": 225000,
  "url": "https://...",
  "image": "url_imagen",
  "source": "Spotify|YouTube|SoundCloud",
  "preview": "url_preview_audio"
}
```

## Troubleshooting

### Error: "API Key inválida"
- Verifica que la clave esté correctamente copiada en `.env`
- Asegúrate de habilitar la API en la consola de desarrollador

### Error: "Límite de solicitudes excedido"
- Las APIs gratuitas tienen límites
- Spotify: 429 solicitudes/hora
- YouTube: 10,000 unidades/día

### No hay resultados
- Intenta con nombres más específicos
- Verifica que la canción exista en la plataforma

## Tips

1. **Búsqueda local es más rápida**: Usa la base de datos local para prototipo
2. **Caché de resultados**: Los resultados se guardan para búsquedas futuras
3. **Previsualizaciones**: Spotify y SoundCloud tienen preview de audio
4. **Derechos de autor**: Respeta los términos de servicio de cada plataforma

## Ejemplo Completo

```html
<!-- Frontend -->
<input id="songSearch" placeholder="Busca tu canción">
<button onclick="searchSongs()">Buscar</button>

<script>
function searchSongs() {
  const query = document.getElementById('songSearch').value;
  
  fetch(`/api/search-songs?query=${query}`)
    .then(res => res.json())
    .then(results => {
      results.forEach(song => {
        console.log(`${song.title} - ${song.artist}`);
      });
    })
    .catch(err => console.error('Error:', err));
}
</script>
```

## Próximas Mejoras

- [ ] Búsqueda en tiempo real
- [ ] Historial de canciones buscadas
- [ ] Favoritos y playlists
- [ ] Integración con Last.fm
- [ ] Caché de audio
- [ ] Descarga de canciones (si es legal)

¿Necesitas ayuda configurando una API específica? 
Consulta la documentación oficial de cada servicio.
