from selenium import webdriver
from selenium.webdriver.common.keys import Keys

navegador = webdriver.Chrome('chromedriver.exe')


  #Pegar a cotação do Dólar
  
  #entrar no google
  
navegador.get('https://google.com/')

  #pesquisar "cotação dolar"
  
navegador.find_element('xpath', '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("cotação dolar")
navegador.find_element('xpath', '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys(Keys.ENTER)
  
  #pegar a cotação
  
cotacao_dolar = navegador.find_element("xpath", '//*[@id="knowledge-currency__updatable-data-column"]/div[1]/div[2]/span[1]').get_attribute('data-value')

print(cotacao_dolar)

  #Pegar a cotação do Euro
  
  #entrar no google
  
navegador.get('https://google.com/')

  #pesquisar cotação dp Euro
  
navegador.find_element('xpath', '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys("cotação euro")

navegador.find_element('xpath', '/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[2]/input').send_keys(Keys.ENTER)

  #Pegar a cotação do Euro

cotacao_euro = navegador.find_element("xpath", '//*[@id="knowledge-currency__updatable-data-column"]/div[1]/div[2]/span[1]').get_attribute('data-value')

print(cotacao_euro)

  #Pegar a cotação do Ouro
  
  #entrar no site 
  
navegador.get('https://www.melhorcambio.com/ouro-hoje')

  #pegar a cotação do Ouro

cotacao_ouro = navegador.find_element('xpath', '//*[@id="comercial"]').get_attribute('value')

cotacao_ouro = cotacao_ouro.replace(',', '.')
print(cotacao_ouro)

  #Atualizar a nossa base de preços com as novas cotações
  
  #Importar a base de dados

import pandas as pd
tabela = pd.read_excel('Produtos.xlsx')

  #Atualizar a cotação, o preço de compra e o preço de venda

  #Atualizar a cotação
  
tabela.loc[tabela['Moeda'] == 'Dólar', 'Cotação'] = float(cotacao_dolar)
tabela.loc[tabela['Moeda'] == 'Euro', 'Cotação'] = float(cotacao_euro)
tabela.loc[tabela['Moeda'] == 'Ouro', 'Cotação'] = float(cotacao_ouro)

  #Atualizar o preço de compra = Preço original * Cotação
  
tabela['Preço de Compra'] = tabela['Preço Original'] * tabela['Cotação']

  #Atualizar o preço de venda = Preço de compra * Margem
  
tabela['Preço de Venda'] = tabela['Preço de Compra'] * tabela['Margem']


display(tabela)

  #Exportar a nova base de preços atualizada
  
 tabela.to_excel("Produtos atualizados.xlsx")
