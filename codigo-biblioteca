import mysql.connector
from datetime import datetime, timedelta

conexao = mysql.connector.connect(
    host='localhost',
    user='root',
    password='1234',
    database='biblioteca',
)

cursor = conexao.cursor()

# Registro de livros
def insere_livro():
    try:
        while True:
            nome_livro = input("Insira o nome do livro a ser cadastrado:")
            genero = input("Insira o gênero do livro:")
            cursor.execute(f"SELECT * FROM sua_tabela WHERE nome = '{nome_livro}'")
            resultado = cursor.fetchall()
            if resultado:
                cursor.execute(f"UPDATE vendas SET quantidade = quantidade + 1 WHERE nome = '{nome_livro}'")
            comando = f"INSERT INTO biblioteca.livros (nome, genero) VALUES ('{nome_livro}', '{genero}')"
            cursor.execute(comando)
            conexao.commit()
            resp = input('Você deseja continuar? [S] ou [N]').upper().strip()
            if resp == 'N':
                cursor.close()
                conexao.close()
                break
            elif resp == 'S':
                print("Seguindo")
            else:
                raise Exception("Digite apenas 'S' ou 'N'")

    except Exception as e:
        print(e)

# Visualização dos livros disponíveis
def mostra_livros():
    comando = "SELECT idlivros, nome FROM livros WHERE quantidade <> 0"
    cursor.execute(comando)
    resultado = cursor.fetchall()
    print(f'Estes são os livros disponíveis: {resultado}')

# Escolha do livro
def escolhe_livro():
    nome = input("Digite o seu nome completo:").title()
    prazo_val = True
    while prazo_val:
        try:
            prazo = int(input("Informe quantos dias você deseja permanecer com livro (máximo 30)"))
            if prazo > 30:
                raise Exception ("prazo inválido")
            else:
                prazo_val = False
        except Exception as e:
            print(e)
    data_atual = datetime.now()        
    data_somada = data_atual + timedelta(days=prazo)

    validador = True
    while validador:
        try:
            cpf = input("Informe o número do seu CPF (sem pontos ou traços):")
            comando = f"INSERT INTO clientes (nome, cpf) VALUES ('{nome}', '{cpf}')"
            if len(cpf) != 11:
                raise Exception("Digite um CPF válido")
            else:
                cursor.execute(comando)
                validador = False
        except Exception as e:
            print(e)

    val = True
    while val:
        try:
            mostra_livros()
            livro = int(input("Digite o ID (número de identificação) do livro que você deseja:"))
            cursor.execute("SELECT idlivros FROM livros")
            resultados = cursor.fetchall()
            ids_livros = [resultado[0] for resultado in resultados]
            if livro not in ids_livros:
                raise Exception("Digite um ID válido")
            else:
                val = False
        except Exception as e:
            print(e)

    comando = f"UPDATE livros SET quantidade = quantidade - 1 WHERE idlivros = {livro}"
    cursor.execute(comando)
    comando = f"SELECT idclientes FROM clientes WHERE nome = '{nome}'"
    cursor.execute(comando)
    id_result = cursor.fetchone() 
    id = id_result[0]
    comando = f"INSERT INTO cliente_aluga_livro (prazo, idclientes, idlivros) VALUES ('{data_somada}', {id}, {livro})"
    cursor.execute(comando)
    print("Livro alugado com sucesso!")
    conexao.commit()
    cursor.close()
    conexao.close()

#Consultar livros alugados pelos clientes e seus respectivo prazos
def consulta_cliente(nome):
    nome = nome.title()
    comando = f"SELECT c.nome, a.prazo, l.nome FROM clientes c JOIN cliente_aluga_livro a ON c.idclientes = a.idclientes JOIN livros l ON a.idlivros = l.idlivros WHERE c.nome = '{nome}'"
    cursor.execute(comando)
    resultado = cursor.fetchall()
    print(resultado)
