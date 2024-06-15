## Expresiones regulares
Las expresiones regulares, también conocidas como regex, son una herramienta poderosa 
utilizada en el procesamiento de texto para buscar, validar y manipular patrones de caracteres. 
Con su sintaxis concisa y flexible, las expresiones regulares permiten realizar búsquedas complejas 
y operaciones de manipulación de texto de manera eficiente.

### **Expresión regular para correos electrónicos (email_regex):**
\b: Coincide con un límite de palabra para asegurar que la dirección de correo electrónico esté 
contenida dentro de límites de palabra y no en medio de una palabra.

[A-Za-z0-9._%+-]+: Coincide con uno o más caracteres que pueden ser letras, dígitos o algunos 
caracteres especiales comunes en las direcciones de correo electrónico, como puntos, guiones 
bajos, porcentajes y signos más o menos.

@: Coincide literalmente con el símbolo '@', que separa el nombre de usuario del dominio en una 
dirección de correo electrónico.

[A-Za-z0-9.-]+: Coincide con uno o más caracteres que pueden ser letras, dígitos o guiones y 
puntos, representando el dominio del correo electrónico.

[A-Z|a-z]{2,}: Coincide con dos o más letras para representar el dominio de nivel superior (por 
ejemplo, com, org, net).

\b: Coincide con otro límite de palabra para finalizar la dirección de correo electrónico.
Expresión regular para números de teléfono (phone_regex):

\b: Coincide con un límite de palabra para asegurar que el número de teléfono esté contenido 
dentro de límites de palabra y no en medio de una palabra.

\d{3}: Coincide con tres dígitos, representando el código de área del número de teléfono.

[-.]?: Coincide opcionalmente con un guion '-' o un punto '.', que pueden ser utilizados como 
separadores en el número de teléfono.

\d{3}: Coincide con otros tres dígitos, representando el prefijo del número de teléfono.

[-.]?: Coincide opcionalmente con otro guion '-' o punto '.', si se utiliza un separador.

\d{4}: Coincide con los últimos cuatro dígitos del número de teléfono.

\b: Coincide con otro límite de palabra para finalizar el número de teléfono

![Imagen1](https://github.com/DanielFloresBautista/Unidad2/issues/3#issue-2354537926)

![Imagen2](https://github.com/DanielFloresBautista/Unidad2/issues/2#issue-2354537631)

## Codigo complepo para la relaizacion del bot
> ```python
> import telebot
> import re
> 
> # Inicia el bot con tu token de bot
> bot = telebot.TeleBot("7157377776:AAGLLarkvSCdhf_LDmIi-A0tpZrv197SEMk")
> 
> # Compiled regular expressions
> greetings_regex = re.compile(r"hello|hi|hey|hola", re.IGNORECASE)
> order_regex = re.compile(r"estado de mi pedido número (\d+)", re.IGNORECASE)
> email_regex = re.compile(r"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b")
> phone_regex = re.compile(r"\b\d{3}[-.]?\d{3}[-.]?\d{4}\b")
> 
> # Ejemplo de función para buscar correos electrónicos en un texto
> def find_emails(text):
>     return email_regex.findall(text)
> 
> # Ejemplo de función para buscar números de teléfono en un texto
> def find_phone_numbers(text):
>     return phone_regex.findall(text)
> 
> # Handler for /start command
> @bot.message_handler(commands=['start'])
> def send_welcome(message):
>     bot.reply_to(message, 'Hola! Daniel soy tu bot de atención al cliente. ¿En qué puedo ayudarte?')
> 
> # Handler for /help command
> @bot.message_handler(commands=['help'])
> def send_help(message):
>     bot.reply_to(message, 'Puedes usarme para consultar el estado de tu pedido, solicitar asistencia general, o incluso proporcionar tu correo electrónico y número de teléfono para futuras comunicaciones.')
> 
> # General message handler
> @bot.message_handler(func=lambda m: True)
> def echo_all(message):
>     response = "No entendí tu mensaje."
>     if greetings_regex.search(message.text):
>         response = "Hola! ¿En qué puedo ayudarte hoy?"
>     elif order_regex.search(message.text):
>         match = order_regex.search(message.text)
>         if match:
>             order_number = match.group(1)
>             response = f"**Información de pedido:**\n* Número de pedido: {order_number}\n* Detalles del pedido: Tu pedido va en camino a su destino"  # Reemplaza con la lógica real de obtención de pedidos
>         else:
>             response = "No se encontró un número de pedido válido en tu mensaje."
>     else:
>         # Buscar correos electrónicos y números de teléfono
>         emails = find_emails(message.text)
>         phones = find_phone_numbers(message.text)
>         # Construir la respuesta
>         if emails:
>             response = "Enviaremos mensaje al correo: \n" + "\n".join(emails) + " para proporcionar más información"
>         elif phones:
>             response = "Enviaremos mensaje al número: \n" + "\n".join(phones) + " para proporcionar más información"
> 
>     bot.reply_to(message, response)
> 
> 
> if __name__ == "__main__":
>     bot.polling(none_stop=True)
> ```
