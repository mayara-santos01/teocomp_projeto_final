# Documentação sobre funcionamento e complexidade da Máquina de Turing

## Funcionamento

O primeiro passo a ser executado pela máquina de Turing é a divisão das longitudes das cidades de origem e destino por 15, afim de promover as conversões de horários. Nesse sentido, um exemplo de entrada das longitudes seria 10010110e11110w, que representa o caso onde a longitude da cidade de origem é 150º leste e a longitude da cidade de origem é 30º oeste. A letra "e" (east) representa leste e "w" (west) representa oeste, representação escolhida por conta da melhor legibilidade das letras em relação a "l" e "o".

Nesse caso, foram usadas 4 fitas:

1. Contém a entrada a ser analisada;
2. Armazena a parte do número que será anlisada em uma determinada etapa e o resto;
3. Armazena o quociente;
4. Armazena o contador.

Para auxiliar no processo de divisão, uma das entradas é um conjunto de letras que funciona como contador, que seria "qqqpq". Sabendo que as longitudes sempre serão divisores de 15, temos 2 casos a considerar:

1. Se temos um número que é uma sequência de 1's seguida por uma sequência de um ou mais 0's (como é o caso dos números 15,30,60 e 120), não é necessário fazer o processo de divisão, somente é necessário identificar uma sequência de quatro 1's, marcando 1 no quociente (terceira fita) e 0 a cada 0 lido. Nesse sentido, ao ler 1 no estado q0, a máquina entra em um loop que é quebrado ao ler um 0 ou ao ler 1 na primeira fita e "p" na quarta, ou seja, se forem lidos quatro 1's em sequência.
2. Sabendo que as longitudes são dividas por 15 (1111), basta verificar se é aplicado o primeiro caso e, se não, serão lidos cinco símbolos para realizar a divisão, sabendo que qualquer número binário com 5 dígitos é maior que 15. Por isso, o contador possui cinco caracteres.

Os piores casos, em termos de complexidade, estão ligados ao segundo caso. 

## Complexidade no pior caso

Em relação à etapa da divisão das longitudes por 15, como as longitudes são números divisíveis por 15, no pior caso, são feitas sucessivas subtrações de números de 5 dígitos por 15, até o momento onde o resto é 1111 e não há mais 1's a serem processados, pelo fato de a divisão ser sempre exata.

1. Leitura dos 5 primeiros dígitos para realizar a subtração (5 passos);
2. Detecção do final da fita com o contador, indicando que já é possível fazer a subtração (1 passo);
3. Subtração do número obtido na etapa 1 (5 passos);
4. Detecção do início da segunda fita, indicando que a subtração foi concluída (1 passo);
5. Como no pior caso sempre há um *carry*, é necesário excluir o primeiro 1 do resto, assim como os 0's subsequentes (x passos);
6. Ao encontrar o segundo 1, que marca o início do resto de fato, volta-se ao início da quarta fita para realizar a contagem do número que será usado na etapa seguinte (5 passos);
7. Leitura dos números do resto ((5-x) passos);
8. Adição dos números que não foram processados ao resto e adição de 0's ao quociente, para formar um novo número pelo qual podemos fazer a divisão por 15 ((5-(5-x)) passos);
9. Reconhecimento do final da fita com o contador, indicando que forma obtidos 5 dígitos, e retirada de um 0 do quociente que foi colocado por excesso na leitura do quino dígito (3 passos);

Nesse sentido, o pior caso de divisão por 15 vai ser composto por n etapas, sendo que em n-1 etapas vão ser realizados todos os passos e na última étapa, são executados os 5 primeiros passos, e, em seguida, ocorre o seguinte:

1. São lidos 4 1's seguidos, sendo que na leitura do quarto 1 se escreve 1 no quociente (4 passos);
2. São processados 0's, caso existam, ao mesmo tempo em que se escreve 0's ao quociente (y passos);
3. Volta-se ao início da quarta fita, para reinicializar o contador (6 passos);
4. Apaga-se os 1's escritos na segunda fita, para não impactar o processamento seguinte (5 passos);
5. Escreve-se o símbolo "e" ou "w", indicado se a cidade está a leste ou oeste, e retorna-se ao início da fita para continuar o processamento (1 passo).

Complexidade total da etapa

T = (n-1)*[5+1+5+1+x+5+(5-x)+(5-(5-x))+3] + [5+1+5+1+x+4+y+6+5+1] = (n-1)*[x+25] + [x+y+28] = Definir complexidade em notação assintótica.

### Conversão do resultado final caso o número obtido seja maior que 24 (horários de madrugada):

Para a conversão do horário final temos 3 casos, sabendo que 24 = 11000:

1. Se o número tiver 4 dígitos ou menos não é necessário fazer contas;
2. Se o número tiver 5 dígitos, mas o segundo número é 0 (por exemplo 10100 = 20), também não é necessário;
3. Se tiver 5 dígitos, e o segundo for 1, analisa-se os outros números e, se só tiver 0's, o resultado deve ser 0h, ou seja, estamos analisando o caso em que o resultado é 11000;
4. Se tiver 5 dígitos e tiver mais de um 1, subtrai-se esse número por 24 (por exemplo 11010 = 26);
5. Se tiver mais de 5 dígitos, subtrai-se o número maior por 24.

Nesse sentido, pode-se fazer uma análise do número resultante da esquerda pra direita e, se o segundo símbolo for 1, entra-se em um loop para verficar se se trata do terceiro caso. Caso tenha outro 1, verifica-se o tamanho da palavra para ver se trata do caso 1, 4 ou 5. Para isso, é essencial verificar o contador. Se a palavra termina de ser processada e o contador não chega ao final, se trata do caso 1. Se a palavra termina de ser processada e o contador também, se trata do caso 4. Se o contador termina de ser processado e a palavra não chega ao final, se trata do caso 5.

