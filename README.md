from selenium import webdriver
from selenium.webdriver.common.by import By
import time
import telepot

# Crie as opções do driver
options = webdriver.ChromeOptions()
options.add_argument('--no-sandbox')  # Necessário se você estiver usando o Chrome em um ambiente restrito

# Inicialize o driver do Chrome com as opções
driver = webdriver.Chrome(options=options)

# URL da página da web que você deseja raspar
driver.get('https://casino.betfair.com/pt-br/')

# Token do seu bot do Telegram e ID do grupo
bot_token = '6640349473:AAGAwbWw5MKZeHpoKcm2BUS9rM5wl1wNnpo'  # Substitua pelo token real do seu bot
group_chat_id = -1001942114963  # Substitua pelo ID do seu grupo Telegram

# Inicialize o bot do Telegram
bot = telepot.Bot(bot_token)

while True:
    try:
        # Espera até que haja pelo menos 3 números gerados
        while True:
            elems = driver.find_elements(By.CLASS_NAME, 'number')
            if len(elems) >= 3:
                break
            time.sleep(1)  # Espere por um segundo antes de verificar novamente

        # Pegue o primeiro, segundo e terceiro números da lista de elementos
        primeiro_numero = elems[0].text
        segundo_numero = elems[1].text
        terceiro_numero = elems[2].text

        # Verifica a coluna dos números
        coluna_primeiro = None
        coluna_segundo = None
        coluna_terceiro = None
        
        colunas = {
            '1': 'COLUNA1', '2': 'COLUNA2', '3': 'COLUNA3',
            '4': 'COLUNA1', '5': 'COLUNA2', '6': 'COLUNA3',
            '7': 'COLUNA1', '8': 'COLUNA2', '9': 'COLUNA3',
            '10': 'COLUNA1', '11': 'COLUNA2', '12': 'COLUNA3',
            '13': 'COLUNA1', '14': 'COLUNA2', '15': 'COLUNA3',
            '16': 'COLUNA1', '17': 'COLUNA2', '18': 'COLUNA3',
            '19': 'COLUNA1', '20': 'COLUNA2', '21': 'COLUNA3',
            '22': 'COLUNA1', '23': 'COLUNA2', '24': 'COLUNA3',
            '25': 'COLUNA1', '26': 'COLUNA2', '27': 'COLUNA3',
            '28': 'COLUNA1', '29': 'COLUNA2', '30': 'COLUNA3',
            '31': 'COLUNA1', '32': 'COLUNA2', '33': 'COLUNA3',
            '34': 'COLUNA1', '35': 'COLUNA2', '36': 'COLUNA3',
            '0': '0',
        }
        
        coluna_primeiro = colunas.get(primeiro_numero)
        coluna_segundo = colunas.get(segundo_numero)
        coluna_terceiro = colunas.get(terceiro_numero)

        # Imprima os números raspados com suas colunas
        print(f"Terceiro número raspado: {terceiro_numero} - Coluna: {coluna_terceiro}")
        print(f"Segundo número raspado: {segundo_numero} - Coluna: {coluna_segundo}")
        print(f"Primeiro número raspado: {primeiro_numero} - Coluna: {coluna_primeiro}")
  

        # Verifica se os três números são da mesma coluna
        if coluna_primeiro == coluna_segundo == coluna_terceiro:
            # Envie um sinal para apostar nas colunas opostas
            colunas_opostas = list(set(['COLUNA1', 'COLUNA2', 'COLUNA3']) - set([coluna_primeiro]))
            roleta = "ROULETTE BR 🔴⚫️"
            mensagem_sinal = f'🎯 Sinal de Aposta 🎯\n\n💸 https://abrir.link/QeWtt 💸\n       ☝🏾 SITE DO CASINO👆🏾\n\n🖥 Roleta: {roleta}\n🔥 Entrada: {", ".join(colunas_opostas)}\n🛟 Até duas proteções - Cobrir o zero !\n🧨 Últimos números: {primeiro_numero, segundo_numero, terceiro_numero}'
            bot.sendMessage(group_chat_id, mensagem_sinal)

        # Aguarde alguns segundos antes de verificar novamente (ajuste conforme necessário)
        time.sleep(20)

    except Exception as e:
        # Lida com exceções, se ocorrerem
     print(f"Erro: {str(e)}")
     print(f"Erro: {str(e)}")
