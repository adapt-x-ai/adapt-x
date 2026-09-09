import os
import asyncio
from aiogram import Bot, Dispatcher, types
from aiogram.filters import Command
from openai import OpenAI

BOT_TOKEN = os.getenv("BOT_TOKEN")
OPENAI_KEY = os.getenv("OPENAI_KEY")

if not BOT_TOKEN:
    raise ValueError("BOT_TOKEN не задан!")
if not OPENAI_KEY:
    raise ValueError("OPENAI_KEY не задан!")

bot = Bot(token=BOT_TOKEN)
dp = Dispatcher()
client = OpenAI(api_key=OPENAI_KEY)

SYSTEM_PROMPT = """Ты - ADAPT-X AI менеджер. Продаешь установку ИИ-менеджера. Отвечает за 30 сек 24/7, подключение за 24ч, экономит 87% на ФОТ. Цена от 19000р/мес. Цель - записать на аудит. Спрашивай: ниша, сколько заявок теряют, где пишут клиенты."""

@dp.message(Command("start"))
async def start(message: types.Message):
    await message.answer("Привет! Я ADAPT-X AI менеджер 🚀\nОтвечаю за 30 сек 24/7.\n\nРасскажи про свой бизнес — какая ниша и куда пишут клиенты?")

@dp.message()
async def handle(message: types.Message):
    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {"role":"system","content":SYSTEM_PROMPT},
                {"role":"user","content":message.text}
            ]
        )
        await message.answer(response.choices[0].message.content)
    except Exception as e:
        print(f"Error: {e}")
        await message.answer("Секунду, перезагружаюсь... Напиши еще раз 👇")

async def main():
    print("ADAPT-X Bot started...")
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
