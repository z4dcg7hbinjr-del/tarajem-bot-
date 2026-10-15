from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes
from deep_translator import GoogleTranslator
from flask import Flask
import threading

TOKEN = "8849887089:AAFsRfwa_006QKtBL4ax2lo-2pGStjl2ftI"
flask_app = Flask(__name__)

@flask_app.route('/')
def home():
    return "Bot running!"

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("البوت شغال ✅ دزلي كلمة")

async def trans(update: Update, context: ContextTypes.DEFAULT_TYPE):
    txt = update.message.text
    ar = GoogleTranslator(source='auto', target='ar').translate(txt)
    en = GoogleTranslator(source='auto', target='en').translate(txt)
    await update.message.reply_text(f"AR: {ar}\nEN: {en}")

def run_bot():
    app = ApplicationBuilder().token(TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, trans))
    app.run_polling()

if __name__ == "__main__":
    threading.Thread(target=run_bot).start()
    flask_app.run(host="0.0.0.0", port=10000)
