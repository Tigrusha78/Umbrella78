import logging 
from telegram import Update 
from telegram.ext import Updater, MessageHandler, Filters, CallbackContext 

class TelegramBot: 
    def __init__(self, token: str, group_chat_id: int): 
        self.token = token 
        self.group_chat_id = group_chat_id 
        self.updater = Updater(token=self.token, use_context=True) 
        self.dispatcher = self.updater.dispatcher 

        logging.basicConfig( 
            format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', 
            level=logging.INFO 
        ) 

        self.dispatcher.add_handler( 
            MessageHandler(Filters.text & ~Filters.command, self.handle_message) 
        ) 

    def handle_message(self, update: Update, context: CallbackContext) -> None: 
        context.bot.send_message( 
            chat_id=self.group_chat_id, 
            text=update.message.text 
        ) 

    def run(self) -> None: 
        self.updater.start_polling() 
        self.updater.idle() 

if __name__ == '__main__': 
    TOKEN = 'YOUR_BOT_TOKEN_HERE' 
    GROUP_CHAT_ID = -123456789  # Замените на ID вашей группы 
    bot = TelegramBot(TOKEN, GROUP_CHAT_ID) 
    bot.run()
