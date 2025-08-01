# 🎁 Giftly API Documentation

Добро пожаловать в документацию по **Giftly API** — сервису для отправки виртуальных подарков через [stars-rocket.com](https://stars-rocket.com).

---

## 📖 Описание API

Giftly API позволяет отправлять виртуальные подарки пользователям по их ID, передавая необходимые параметры в формате JSON на защищённый эндпоинт.  
Для работы с API требуется авторизация с помощью токена, который можно получить, зарегистрировавшись у Telegram-бота <code>@giftLyServiceBot</code>.

---

## 🚀 Как отправить подарок

### HTTP запрос

POST https://stars-rocket.com/api/v1/giftly/buyGift

bash
Копировать
Редактировать

### Заголовки (Headers)

```json
{
  "Content-Type": "application/json",
  "Accept": "application/json"
}
Тело запроса (JSON)
Параметр	Тип	Обязательный	Описание
recipient	string	Да	ID пользователя, которому отправляется подарок
gift_id	string	Да	ID подарка (см. таблицу ниже)
text	string	Нет	Сообщение к подарку (по умолчанию — приветствие)
token	string	Да	Ваш API токен для доступа

🎁 Доступные подарки
Стоимость (₽)	Эмодзи	ID подарка
15	🧸	5170233102089322756
15	💝	5170145012310081615
25	🌹	5168103777563050263
25	🎁	5170250947678437525
50	🍾	6028601630662853006
50	🚀	5170564780938756245
50	💐	5170314324215857265
50	🎂	5170144170496491616
100	🏆	5168043875654172773
100	💍	5170690322832818290
100	💎	5170521118301225164

🐍 Пример использования Giftly API на Python
Ниже пример асинхронной функции, которая отправляет подарок с помощью aiohttp.
Код чистый, с подробными комментариями и логированием для удобства отладки.

python
Копировать
Редактировать
import aiohttp
import asyncio
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

async def send_gift(user_id: str, gift_id: str, token: str, text: str = "Привет, я отдаю тебе выигрыш с бот!") -> bool:
    """
    Отправляет подарок пользователю через Giftly API.

    Args:
        user_id (str): ID пользователя, получателя подарка.
        gift_id (str): ID подарка.
        token (str): API токен для авторизации.
        text (str, optional): Сообщение, отправляемое вместе с подарком. По умолчанию приветствие.

    Returns:
        bool: True если подарок успешно отправлен, False при ошибке.
    """
    url = "https://stars-rocket.com/api/v1/giftly/buyGift"
    headers = {
        "Content-Type": "application/json",
        "Accept": "application/json"
    }
    payload = {
        "recipient": user_id,
        "gift_id": gift_id,
        "text": text,
        "token": token
    }

    async with aiohttp.ClientSession() as session:
        async with session.post(url, headers=headers, json=payload) as response:
            if response.status == 201:
                logger.info("Подарок успешно отправлен пользователю %s", user_id)
                return True
            else:
                error = await response.json()
                logger.error("Ошибка при отправке подарка пользователю %s: %s", user_id, error.get('message', 'Неизвестная ошибка'))
                return False

# Для теста запустите:
# asyncio.run(send_gift("1234567890", "5170145012310081615", "ВАШ_ТОКЕН"))
⚠️ Важные моменты
Получите токен у Telegram-бота <code>@giftLyServiceBot</code>. Без токена API не работает.

Код 201 в ответе означает успешную отправку подарка.

Рекомендуется использовать логирование для отладки и мониторинга.

При ошибках проверяйте корректность токена и передаваемых параметров.

📄 Лицензия
Этот проект распространяется под лицензией MIT.

Если у вас есть вопросы или предложения — создавайте issue или свяжитесь со мной напрямую!

Спасибо, что используете Giftly API! 🎉
