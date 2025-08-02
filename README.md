# GiftLy API

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![API Status](https://img.shields.io/badge/API-Status-Active-brightgreen.svg)](https://stars-rocket.com/api/v1/giftly/buyGift)

**GiftLy API** - это RESTful API для отправки цифровых подарков пользователям Telegram. API позволяет легко интегрировать функциональность отправки подарков в ваши приложения и боты.

## 🌟 Возможности

- ✅ Отправка цифровых подарков пользователям
- ✅ Кастомизация сообщений перед подарком
- ✅ Простая интеграция с Python приложениями
- ✅ Асинхронная работа с использованием aiohttp
- ✅ Подробная обработка ошибок
- ✅ Безопасная аутентификация через токены

## 📋 Требования

- Python 3.7+
- aiohttp
- logging (встроенный модуль)

## 🚀 Установка

```bash
pip install aiohttp
```

## 🔑 Получение токена

Для использования API необходимо получить токен доступа:

1. Найдите бота [@giftLyServiceBot](https://t.me/giftLyServiceBot) в Telegram
2. Зарегистрируйтесь и получите ваш уникальный токен
3. Используйте токен в ваших запросах к API

## 📖 Использование

### Базовый пример

```python
import aiohttp
import logging
import asyncio

async def send_gift(user_id, gift_id, text: str = "Привет, я отдаю тебе выигрыш с бот!"):
    """
    Отправляет подарок пользователю через GiftLy API
    
    Args:
        user_id (str): ID пользователя Telegram
        gift_id (str): ID подарка
        text (str): Сообщение перед подарком
    
    Returns:
        tuple: (success: bool, error_message: str or None)
    """
    # Обязательные заголовки для API
    headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
    }
    
    # Данные для отправки
    data = {
        'recipient': str(user_id),
        'gift_id': str(gift_id),
        'text': str(text),
        'token': 'YOUR_API_TOKEN_HERE'  # Замените на ваш токен
    }
    
    # Отправка запроса к API
    async with aiohttp.ClientSession() as session:
        async with session.post(
            'https://stars-rocket.com/api/v1/giftly/buyGift',
            headers=headers,
            json=data
        ) as response:
            response_data = await response.json()
            
            if response.status == 201:
                logging.info(f'Подарок успешно отправлен пользователю {user_id}')
                return True, None
            else:
                error_message = response_data.get('message', 'Неизвестная ошибка')
                logging.error(f'Ошибка при отправке подарка для пользователя {user_id}: {response_data}')
                return False, error_message

# Пример использования
async def main():
    # Отправка подарка
    success, error = await send_gift(
        user_id="123456789",
        gift_id="gift_001",
        text="🎉 Поздравляем с победой!"
    )
    
    if success:
        print("✅ Подарок отправлен успешно!")
    else:
        print(f"❌ Ошибка: {error}")

# Запуск
if __name__ == "__main__":
    asyncio.run(main())
```

## 📡 API Endpoints

### POST /api/v1/giftly/buyGift

Отправляет подарок пользователю.

**Заголовки:**
```
Content-Type: application/json
Accept: application/json
```

**Тело запроса:**
```json
{
    "recipient": "string",     // ID пользователя Telegram
    "gift_id": "string",      // ID подарка
    "text": "string",         // Сообщение перед подарком
    "token": "string"         // Ваш API токен
}
```

**Ответы:**

- **201 Created** - Подарок успешно отправлен
- **400 Bad Request** - Неверные параметры запроса (возвращает текст ошибки)
- **401 Unauthorized** - Неверный токен
- **404 Not Found** - Подарок не найден
- **500 Internal Server Error** - Внутренняя ошибка сервера

## 🔧 Обработка ошибок

```python
async def handle_gift_sending(user_id: str, gift_id: str):
    """Пример обработки ошибок при отправке подарка"""
    try:
        success, error = await send_gift(user_id, gift_id)
        
        if success:
            print(f"✅ Подарок отправлен пользователю {user_id}")
        else:
            print(f"❌ Ошибка отправки: {error}")
            
    except aiohttp.ClientError as e:
        print(f"❌ Ошибка сети: {e}")
    except Exception as e:
        print(f"❌ Неожиданная ошибка: {e}")
```

## 📝 Логирование

```python
import logging

# Настройка логирования
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

# В функции send_gift уже есть логирование
```

## 🛡️ Безопасность

- Все запросы должны содержать валидный токен
- Токен должен быть получен через официального бота @giftLyServiceBot
- Не передавайте токен в открытом виде в коде
- Используйте переменные окружения для хранения токенов

### Рекомендуемый способ хранения токена

```python
import os
from dotenv import load_dotenv

load_dotenv()

# В .env файле:
# GIFTLY_API_TOKEN=your_token_here

api_token = os.getenv('GIFTLY_API_TOKEN')
```

## 🤝 Поддержка

Если у вас возникли вопросы или проблемы:

- 📧 Свяжитесь с поддержкой через [@giftLyServiceBot](https://t.me/giftLyServiceBot)
- 📖 Изучите документацию API
- 🐛 Сообщите о багах через бота

## 📄 Лицензия

Этот проект распространяется под лицензией MIT. См. файл [LICENSE](LICENSE) для получения дополнительной информации.

## ⭐ Поддержка проекта

Если этот API оказался полезным для вас, поставьте звездочку на GitHub!

---

**GiftLy API** - Делаем отправку подарков простой и удобной! 🎁 
