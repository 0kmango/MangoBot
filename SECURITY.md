# Política de seguridad

## Versiones con soporte

Mango Bot se distribuye desde `main`. Solo la última versión recibe
correcciones de seguridad.

| Versión | Soporte |
| :--- | :--- |
| `main` (última, hoy `v1.7.9`) | ✅ |
| Versiones anteriores | ❌ |

## Cómo reportar una vulnerabilidad

**No abras un issue público** con los detalles: cualquiera podría aprovecharlo
antes de que exista un arreglo, y este bot maneja tokens y claves de API.

Usa **[Security → Report a vulnerability](https://github.com/0kmango/MangoBot/security/advisories/new)**
(GitHub Security Advisories): es privado y llega solo al mantenedor.

### Qué incluir

- Versión o commit afectado, sistema operativo y versión de `discord.py`.
- Pasos mínimos para reproducirlo (comando usado, configuración del `.env`
  sin valores reales, mensajes o interacciones implicadas).
- Impacto que crees que tiene: *fuga de `DISCORD_TOKEN`*, *ejecución de
  comandos reservados al dueño*, *inyección en la base de datos*,
  *SSRF en las llamadas a las APIs de IA*, *bypass de las restricciones de
  menciones masivas*, etc.
- Si lo deseas, una propuesta de arreglo.

### Qué esperar

- Confirmación de recepción en un plazo de **7 días**.
- Evaluación, parche y aviso en la sección *Security* del repositorio con
  crédito para quien reportó (salvo que prefiera el anonimato).
- El proyecto lo mantiene una persona en su tiempo libre: sin SLA de empresa,
  pero **ningún reporte se ignora**.

## Alcance

Están en alcance:

- **Manejo de secretos**: que el `DISCORD_TOKEN`, las claves de las APIs de IA
  (`OPENAI_API_KEY`, `GROQ_API_KEY`…) o los identificadores del dueño puedan
  acabar en logs, mensajes, commits o excepciones.
- **Permisos de comandos**: que un comando marcado como privado
  (`is_owner`) pueda ejecutarlo alguien que no es el dueño, o que
  `!embed` acepte contenido que escape de sus límites.
- **Robo de recursos**: comandos que permitan a terceros abusar de las cuotas
  de las APIs de IA o de la descarga de audio.
- **Validación de entrada**: URLs de YouTube/Spotify malformadas, nombres de
  playlist, parámetros de los comandos de voz.
- **Dependencias**: versiones con vulnerabilidades conocidas (hay Dependabot
  configurado, pero se agradece el aviso).

Fuera de alcance:

- Vulnerabilidades de `discord.py`, `yt-dlp`, `ffmpeg` o los servidores de
  Discord: repórtalas a su proyecto.
- **Abuso del bot por parte de quien lo autoaloja**: si tú eres el dueño y
  expones el token, no es un fallo del código.
- Problemas que requieran acceso físico al equipo donde corre el bot.

## Buenas prácticas para quien lo autoaloja

1. El token y las claves viven **solo** en `.env` (ignorado por git).
   Compruébalo con `python tests/check_secrets.py` antes de cada push.
2. No compartas capturas de la consola con el token visible.
3. Mantén `discord.py` y `yt-dlp` actualizados: cambian sus APIs a menudo.
4. Si el token se filtró, **regenéralo** en el portal de desarrolladores de
   Discord; no basta con borrar el mensaje.