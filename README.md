# Giftly API Documentation

Добро пожаловать в документацию по Giftly API — сервису для отправки виртуальных подарков через stars-rocket.com.

## Описание API

Giftly API позволяет отправлять виртуальные подарки пользователям по их ID, передавая необходимые параметры в формате JSON на защищённый эндпоинт.  
Для работы с API требуется авторизация с помощью токена, который можно получить, зарегистрировавшись у Telegram-бота @giftLyServiceBot.

## Как отправить подарок

### HTTP запрос

POST https://stars-rocket.com/api/v1/giftly/buyGift

### Заголовки (Headers)

{
  "Content-Type": "application/json",
  "Accept": "application/json"
}

### Тело запроса (JSON)

Параметры:
- recipient (string, обязательный): ID пользователя, которому отправляется подарок  
- gift_id (string, обязательный): ID подарка  
- text (string, необязательный): сообщение к подарку (по умолчанию "Привет, я отдаю тебе выигрыш с бот!")  
- token (string, обязательный): API токен для доступа  

## Пример подарков (ID)

15 🧸 — 5170233102089322756  
15 💝 — 5170145012310081615  
25 🌹 — 5168103777563050263  
25 🎁 — 5170250947678437525  
50 🍾 — 6028601630662853006  
50 🚀 — 5170564780938756245  
50 💐 — 5170314324215857265  
50 🎂 — 5170144170496491616  
100 🏆 — 5168043875654172773  
100 💍 — 5170690322832818290  
100 💎 — 5170521118301225164  

## Пример использования на Python

```python
import aiohttp
import asyncio
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

async def send_gift(user_id: str, gift_id: str, token: str, text: str = "Привет, я отдаю тебе выигрыш с бот!") -> bool:
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
                logger.info(f"Подарок успешно отправлен пользователю {user_id}")
                return True
            else:
                error = await response.json()
                logger.error(f"Ошибка при отправке подарка пользователю {user_id}: {error.get('message', 'Неизвестная ошибка')}")
                return False

# asyncio.run(send_gift("1234567890", "5170145012310081615", "ВАШ_ТОКЕН"))
