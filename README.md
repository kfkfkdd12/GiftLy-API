# GiftLy API

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![API Status](https://img.shields.io/badge/API-Status-Active-brightgreen.svg)](https://stars-rocket.com/api/v1/giftly/buyGift)

**GiftLy API** - это RESTful API для отправки цифровых подарков пользователям Telegram. API позволяет легко интегрировать функциональность отправки подарков в ваши приложения и боты.

**⚠️ Важно:** Для получения подарка пользователь должен написать в Telegram [@StarsRocketSupport](https://t.me/StarsRocketSupport). При успешной отправке (ответ 201) пользователь добавляется в очередь отправки подарка. Каждую минуту из очереди отправляется подарок. Спустя 60 неудачных попыток (1 час) пользователь удаляется из очереди и больше не сможет получить подарок.

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
3. **Пополните баланс** в боте для использования API
4. Используйте токен в ваших запросах к API

**💡 Примечание:** С баланса GiftLy списывается только при успешной отправке подарка.

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
                return True, "Пользователь добавлен в очередь отправки"
            else:
                error_message = response_data.get('message', 'Неизвестная ошибка')
                logging.error(f'Ошибка при отправке подарка для пользователя {user_id}: {response_data}')
                return False, error_message

# Пример использования
async def main():
    # Отправка подарка
    success, error = await send_gift(
        user_id=user_id,
        gift_id=str("5170233102089322756"), #отправляется 🧸
        text="🎉 Поздравляем с победой!"
    )
    
    if success:
        await bot.send_message(user_id, "📝 Пожалуйста, напишите @StarsRocketSupport для получения подарка")
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

- **201 Created** - Подарок успешно отправлен (пользователь добавлен в очередь отправки)
- **400 Bad Request** - Внутренняя ошибка (возвращает текст ошибки)

## 🛡️ Безопасность

- Все запросы должны содержать валидный токен
- Токен должен быть получен через официального бота @giftLyServiceBot
- Не передавайте токен в открытом виде в коде
- Используйте переменные окружения для хранения токенов
- Для использования API необходимо пополнить баланс в боте



## 🤝 Поддержка

Если у вас возникли вопросы или проблемы:

- 📧 Свяжитесь с поддержкой через [@gidddra](https://t.me/gidddra)
- 📖 Изучите документацию API



## 🔮 Будущие обновления

В следующих версиях API планируется добавить:
- 🌟 Отправка звезд через API
- 📊 Статистика отправленных подарков
- 🔔 Уведомления о статусе доставки

## ⭐ Поддержка проекта

Если этот API оказался полезным для вас, поставьте звездочку на GitHub!

---

**GiftLy API** - Делаем отправку подарков простой и удобной! 🎁 
