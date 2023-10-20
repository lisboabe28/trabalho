import csv 

import random 

def menu():
    print("[1] - Novo Álbum\n[2] - Acessar Álbum\n[3] - Sair do aplicativo")
    escolha = int(input("Informe o serviço desejado: "))
    return escolha

def menu_funcionalidades():
    print('[1] - Ver álbum\n[2] - Gerenciar coleção\n[3] - Abrir pacote de figurinhas\n[4] - Voltar ao menu anterior')
    escolha_funcionalidades = int(input("Informe a opção desejada: "))
    return escolha_funcionalidades

def menu_gerenciar():
    print('[1] - Colar figurinha\n[2] - Disponibilizar para troca\n[3] - Propor troca\n[4] - Revisar solicitações de troca\n[5] - Voltar ao menu anterior')
    escolha_gerenciar = int(input("Informe a opção desejada: "))
    return escolha_gerenciar

class Album:
    def __init__(self):
        self.figurinhas = [False]*15
        self.requisicoes_trocas = []

    def possui_figurinha(self, nro):
        self.figurinhas[nro] = True
        return self.figurinhas[nro]

    def colar_figurinha(self, nro):
        self.figurinhas[nro] = True

class Figurinha:
    def __init__(self,numero,nome,conteudo):
        self.numero = numero
        self.nome = nome
        self.conteudo = conteudo
        self.status = 3


class Usuário:
    def __init__(self,nome,senha):
        self.nome = nome
        self.senha = senha      
        self.album = Album()
        self.colecao = []
    
    def cadastrar(self,nome,senha):
        U = open('Usuarios.csv','a')
        writer = csv.writer(U)
        writer.writerow([nome,senha,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,False,False,False,False,False,False,False,False,False,False,False,False,False,False,False])
        U.close()
    
    def get_album(self):
        return self.album
        
    def get_nome(self):
        return self.nome

    def verificar_login(self):
        U = open('Usuarios.csv')
        reader = csv.reader(U)
        data = list(reader)
        U.close()
        data.pop(0)
        for i in range(len(data)):
            if self.nome in data[i] and self.senha in data[i]: 
                return True
        else:
            return False



escolha = menu()
logado = False

while escolha != 3:
    
    if escolha == 1:
        
        print('[CRIANDO NOVO USUÁRIO]')
        nome = input('informe um nome de usuário: ')
        v = 0
        
        while v == 0:
            Senha = input('informe uma senha: ')
            senha = input('informe a senha novamente para confirmá-la: ')
            
            if Senha == senha:
                v+=1
            
            else:
                print('senhas não correspondentes')
        
        else:

            User = Usuário(nome,Senha)
            User.cadastrar(User.nome,User.senha)
        
        
        escolha = menu()
    
    elif escolha == 2:
        print('[LOGIN]')
        
        if logado == False:
            nome_v = input('informe seu nome de usuário: ')
            senha_v = input('informe sua senha: ')
            User_v = Usuário(nome_v,senha_v)
            U = open('Usuarios.csv')
            reader = csv.reader(U)
            for l in reader:
                if nome_v in l and senha_v in l:
                        for i in range(17):
                            del l[0]
                        User_v.album.figurinhas = l        
            U.close()
            U = open('Usuarios.csv')
            reader = csv.reader(U)
        
            for l in reader:
                if nome_v in l and senha_v in l:
                    for i in range(2,17):
                        n = int(l[i])
                        User_v.colecao.append(n)
                
            U.close()
            f = open('figurinhas.csv', encoding='utf-8')
            reader = csv.reader(f)
            indices = []
            album = []
            for l in reader:
                for i in range(len(User_v.album.figurinhas)):
                    if User_v.album.figurinhas[i] !='False' and l[0] == str(i):
                        figurinhas = []
                        figurinhas.clear()
                        Figurinha.nome = l[1]
                        Figurinha.conteudo = l[2]
                        Figurinha.numero = l[0]
                        indices.append(Figurinha.numero)
                        figurinhas.append(Figurinha.nome)
                        figurinhas.append(Figurinha.conteudo)
                        album.append(figurinhas)
            f.close()
            v=User_v.verificar_login()
            logado = True
        else:   
            if v == False:
                print("[NOME DE USUÁRIO OU SENHA INCORRETA]")
                escolha = menu()
        
            else:
                print("[ÁLBUM ACESSADO]")
                escolha_func = menu_funcionalidades()
                while escolha_func !=4:
                
                    if escolha_func == 1:
                        print('[ÁLBUM]')
                        for i in range(len(indices)):
                            print(f'[{indices[i]}] - ',album[i])
                    
                        for i in range(len(User_v.album.figurinhas)):
                            if User_v.album.figurinhas[i] == "False" and User_v.colecao[i] != 0:
                                print("Você possui a figurinha [{}] na coleção e não está colada no álbum".format(i))
                        
                        escolha_func = menu_funcionalidades()
                
                    elif escolha_func == 2:
                        escolha_geren = menu_gerenciar()
                    
                        while escolha_geren !=5:
                            if escolha_geren == 1:
                                print(User_v.album.figurinhas)
                                print(User_v.colecao)
                                numero_figurinha = int(input("Informe o número da figurinha a ser colada: "))
                                if numero_figurinha >= 0 and numero_figurinha < 15:  # Certifique-se de que o número seja válido
                                    for i in range(len(User_v.colecao)):
                                        if User_v.colecao[numero_figurinha] != 0 and User_v.album.figurinhas[numero_figurinha] == "False":
                                            print("Figurinha colada com sucesso!")# A figurinha está na coleção, mas não no álbum
                                            User_v.colecao[numero_figurinha] -= 1
                                            
                                            break
                                        else:
                                            print('Usuario não possui essa figurinha.')
                                            break
                                    else:
                                        print("A figurinha já está colada no álbum.")
                                        break
                                else:
                                    print("Número de figurinha inválido.")
                                break
                            elif escolha_geren == 2:
                                pass
                            elif escolha_geren == 3:
                                pass #propor_troca(User)
                            elif escolha_geren == 4:
                                pass
                            else:
                                print("Valor inválido")
                                escolha_geren = menu_gerenciar()
                        else:
                            print("Voltando ao menu anterior")
                            break
                    
                    elif escolha_func == 3:
                     fig1 = random.randint(0,14)
                     fig2 = random.randint(0,14)
                     fig3 = random.randint(0,14)

                     print('Primeira figurinha:',fig1)
                     print('Segunda figurinha:',fig2)
                     print('Terceira figurinha:',fig3)

                     print(User_v.colecao)
                     User_v.colecao[fig1] += 1
                     User_v.album.possui_figurinha(fig1)
                     User_v.colecao[fig2] += 1
                     User_v.album.possui_figurinha(fig2)         
                     User_v.colecao[fig3] += 1
                     User_v.album.possui_figurinha(fig3)
                     print(User_v.colecao)
                     print("Figurinhas adicionadas à coleção!")
                     
                     break
                   
                    else:
                        print('[OPÇÃO INVÁLIDA, FAVOR DIGITAR OUTRO VALOR]')
                        break
                else:
                    logado = False
                    escolha = menu()
    else:
        print("[VALOR INVÁLIDO, DIGITE OUTRO NÚMERO]")
        escolha = menu()
else:
    print('[PROGRAMA ENCERRADO]')
