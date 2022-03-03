# validarCnpj
validador de cnpj simples em python

~~~python
import re
from xml.dom.minidom import Element

REGRESSIVOS = [6, 5, 4, 3, 2, 9, 8, 7, 6, 5, 4, 3, 2]


def valida_cnpj(cnpj):
    cnpj = remover_caracteres(cnpj)

    try:
        if is_sequencia(cnpj):
            return False
    except:
        return False
        
    try:
        novo_cnpj = calcula_digito(cnpj=cnpj, digito=1)
        novo_cnpj = calcula_digito(cnpj=novo_cnpj, digito=2)
    except Exception as e:
        return False

    if novo_cnpj == cnpj:
        return True
    else:
        return False


def calcula_digito(cnpj, digito):
    if digito == 1:
        regressivos = REGRESSIVOS[1:]
        novo_cnpj = cnpj[:-2]
    elif digito == 2:
        regressivos = REGRESSIVOS
        novo_cnpj = cnpj
    else:
        return None

    total = 0
    for indice, regressivo in enumerate(regressivos):
        total += int(cnpj[indice]) * regressivo

    digito = 11 - (total % 11)
    digito = digito if digito <= 9 else 0

    return f'{novo_cnpj}{digito}'


def is_sequencia(cnpj):
    sequencia = cnpj[0] * 10

    if sequencia == cnpj:
        return True
    else:
        return False


def remover_caracteres(cnpj):
    return re.sub(r'[^0-9]', '', cnpj)
    
def gera():
    primeiro_digito = random.randint(0, 9)
    segundo_digito = random.randint(0, 9)
    segundo_bloco = random.randint(100, 999)
    terceiro_bloco = random.randint(100, 999)
    quarto_bloco = '0001'

    inicio_cnpj = f'{primeiro_digito}{segundo_digito}{segundo_bloco}{terceiro_bloco}{quarto_bloco}00'

    novo_cnpj = calcula_digito(cnpj=inicio_cnpj, digito=1)
    novo_cnpj = calcula_digito(cnpj=novo_cnpj, digito=2)

    return novo_cnpj

def formata(cnpj):
    cnpj = remover_caracteres(cnpj)
    formatado = f'{cnpj[:2]}.{cnpj[2:5]}.{cnpj[5:8]}/{cnpj[8:12]}-{cnpj[12:14]}'
    return formatado
    
# chamada na main

import modulos_cnpj


cnpj1 = '19.326.484/0001-91'

if modulos_cnpj.valida_cnpj(cnpj1):
    print(f'{cnpj1} é válido!')
else:
    print(f'{cnpj1} é inválido!')  


for i in range(10):
    novo_cnpj = modulos_cnpj.gera()
    formatado = modulos_cnpj.formata(novo_cnpj)
    print(formatado)

~~~
