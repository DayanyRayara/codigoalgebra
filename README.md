

def encontrar_pivo(coluna, linha_inicio, matriz):
    max_row = linha_inicio
    for i in range(linha_inicio, len(matriz)):
        if abs(matriz[i][coluna]) > abs(matriz[max_row][coluna]):
            max_row = i
    return max_row


def normalizar_linha(linha, coluna, matriz):
    pivot = matriz[linha][coluna]
    if pivot != 0:
        for i in range(len(matriz[linha])):
            matriz[linha][i] = matriz[linha][i] / pivot


def eliminar_linhas(linha, coluna, matriz):
    for i in range(len(matriz)):
        if i != linha:
            fator = -matriz[i][coluna]
            if fator != 0:
                for j in range(len(matriz[i])):
                    matriz[i][j] = matriz[i][j] + fator * matriz[linha][j]


def transformar_matriz(matriz):
    linhas = len(matriz)
    colunas = len(matriz[0])
    coluna_pivo = 0
    
    for i in range(min(linhas, colunas)):
        if coluna_pivo >= colunas:
            break
        
        linha_pivo = encontrar_pivo(coluna_pivo, i, matriz)
        if matriz[linha_pivo][coluna_pivo] == 0:
            coluna_pivo += 1
            continue

        if linha_pivo != i:
            temp = matriz[i]
            matriz[i] = matriz[linha_pivo]
            matriz[linha_pivo] = temp

        normalizar_linha(i, coluna_pivo, matriz)
        eliminar_linhas(i, coluna_pivo, matriz)
        coluna_pivo += 1
    return matriz


def imprimir_matriz(matriz):
    for linha in matriz:
        print(' '.join(str(valor) for valor in linha))


def entrada_matriz():
    print("Digite o número de linhas da matriz:")
    num_linhas = int(input().strip())
    matriz = []

    for i in range(num_linhas):
        print(f"Digite os valores da linha {i + 1}, separados por espaço:")
        linha = list(map(int, input().strip().split()))
        matriz.append(linha)

    return matriz


if __name__ == "__main__":
    matriz_usuario = entrada_matriz()
    matriz_condensada = transformar_matriz(matriz_usuario)
    
    print("Matriz condensada:")
    imprimir_matriz(matriz_condensada)
