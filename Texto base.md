Para gerar valores aleatórios/randômicos em C será necessário usar a seguinte função:

    rand();

Que necessita da biblioca: 

    #include <stdio.h>

**Retorno:** valores inteiros entre e incluindo 0 e 32767. 
**Parâmetros:** nenhum.

Segue um exemplo:

    #include <stdio.h>

    int main(){
        int numeroAleatorio;
        for(int i=0; i<10; i++){
            numeroAleatorio=rand();
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }
 
O programa exibirá:

    41
    18467
    6334
    26500
    19169
    15724
    11478
    29358
    26962
    24464

Se executarmos de novo o programa exibirá:

    41
    18467
    6334
    26500
    19169
    15724
    11478
    29358
    26962
    24464

Nas duas vezes que executamos o retorno da função foi o mesmo? Sim! E se executarmos repetidas vezes esse código ele exibirá os mesmos valores. Mas então essa função não gera valores aleatórios? Por si só não, para isso precisamos incluir a seguinte função:

    srand();

Que necessita da biblioteca:

    <stdlib.h>
    
**Retorno:** nenhum. 
**Parâmetros:** valores inteiros sem sinal.

Ela irá modificar a *seed* da função rand( ), mudando a *seed* mudamos a sequência de valores aleatórios. Por isso precisa ser usada apenas uma vez antes de usar a função rand( ), mesmo que você use várias vezes rand( ). Por padrão a *seed* é igual a 1. Logo se passarmos como parâmetro o valor 1, o programa exibirá o mesmo de antes. Porém, se passarmos outro valor o resultado será diferente.

    #include <stdio.h>
    #include <stdlib.h>

    int main(){
        int numeroAleatorio;
        srand(2); 
        for(int i=0; i<10; i++){
            numeroAleatorio=rand();
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }

O programa exibirá:

    45
    29216
    24198
    17795
    29484
    19650
    14590
    26431
    10705
    18316

Passando outros valores o resultado irá ser diferente a cada novo valor. Entretando, ainda se executarmos o mesmo código mais de uma vez o resultado será igual. Como podemos resolver isso? Isso poderia ser resolvido se a cada vez que executassemos o programa a função srand( ) recebesse um parâmetro diferente. Para fazer isso iremos usar a função:

    time();

Que necessita da biblioteca:

    <time.h>

**Retorno:** segundos que se passaram desde 00:00:00 de 1 de Janeiro de 1970. 
**Parâmetros:** para nosso uso passaremos *NULL* ou 0.

    #include <stdio.h>
    #include <time.h>

    int main(){
        int tempo;
        tempo=time(NULL); //Ou tempo=time(0);
        printf("%d", tempo);
        return 0;
    }

O programa exibiu:

    1671364746

Usando um convesor de segundos para anos da internet, o resultado foi 52 anos (estou escrevendo este artigo em 2022). Agora só precisamos juntar tudo:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        int numeroAleatorio, tempo;
        tempo=time(NULL);
        srand(tempo); 
        for(int i=0; i<10; i++){
            numeroAleatorio=rand();
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }

Ou seja, estamos passando um valor diferente a cada vez que rodarmos o programa como parâmetro da função srand( ), que por sua vez irá modificar a *seed* da função rand( ). Assim a cada vez que rodarmos o programa uma sequência aleatória de valores será gerada. Por economia de código, é comum escrever:

    srand(time(NULL));

<h1>Limitando a faixa de valores aleatórios</h1>

Na maioria das vezes queremos gerar valores aleatórios dentro de uma faixa de específica, por exemplo podemos querer que o menor seja 10 ou o maior seja 100 ou ainda que sejam de 10 a 100. Vamos ver como fazer isso.

<h2>Limitando o maior valor</h2>

Primeiro vamos relembrar o operador que retorna o resto da divisão.

    29 % 10

O resultado será 9, pois 29 dividos por 10 é 2 e sobram 9. Analise o seguinte código:

    #include <stdio.h>

    int main(){
        int qualquerNumero, restoDivisao;
        scanf("%d", &qualquerNumero);
        restoDivisao=qualquerNumero%10;
        printf("%d", restoDivisao);
    }

Qual será o maior valor possível que o programa exibirá? 9, por exemplo quando *qualquerNumero* valer 49. E qual será o menor valor possível? 0, por exemplo quando *qualquerNumero* valer 50. Juntado com o que vimos:

    numeroAleatorio=rand()%100;

*numeroAleatorio* ficará limitado a uma faixa de valores de 0 a 99. Caso não queira se lembrar todo vez que o maior valor será o limiteMaximo, nesse caso 100, menos 1, pode-se fazer:

    numeroAleatorio=rand()%(limiteMaximo+1);

Nosso código ficará assim:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        int numeroAleatorio, limiteMaximo;
        limiteMaximo=5;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=rand()%(limiteMaximo+1);
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }

O programa exibiu:

    4
    0
    5
    1
    0
    1
    2
    0
    3
    2

<h2>Limitando o menor valor</h2>

Esse é mais simples, basta somar o menor valor desejado, por exemplo 20:

    numeroAleatorio=rand()+20; //numeros aleatorios de 20 a 32787

Mas fique atento, pois fazendo isso também estamos limitando o maior valor, que nesse caso será 32767 + 20. Usando isso também podemos gerar valores aleatórios negativos, basta subtrair o menor valor desejado:

    numeroAleatorio=rand()-20; //numeros aleatorios de -20 a 32747

Aqui também estaremos limitando o maior valor, que nesse caso será 32767 - 20. Veja como está o código:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        int numeroAleatorio, limiteMinimo;
        limiteMinimo=-10;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=rand()+limiteMinimo;
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }

<h2>Limitando o maior e o menor valor</h2>

Unindo tudo chegamos ao seguinte:

    numeroAleatorio=limiteMinimo+(rand()%(limiteMaximo+1));

Veja como está o código completo:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        int numeroAleatorio, limiteMinimo, limiteMaximo;
        limiteMinimo=2;
        limiteMaximo=6;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=limiteMinimo+(rand()%(limiteMaximo+1));
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }

Que exibiu:

    6
    3
    8
    2
    3
    3
    7
    6
    4
    4

Execute esse programa várias vezes e perceba que o programa está gerando valores aleatórios de 2 a 8. Mas nosso limite maximo não está definindo em 6? Sim, mas como dito anteriormente, mudando o limite mínimo estamos mudando também o limite máximo. Para resolver isso:

    numeroAleatorio=limiteMinimo+(rand()%(limiteMaximo+1-limiteMinimo));

Para não precisar escrever todo esse código várias vezes podemos criar uma função:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int sorteia(int limiteMinimo, int limiteMaximo);

    int main(){
        int numeroAleatorio;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=sorteia(-2, 4);
            printf("%d\n", numeroAleatorio);
        }
        return 0;
    }

    int sorteia(int limiteMinimo, int limiteMaximo){
        return limiteMinimo+(rand()%(limiteMaximo+1-limiteMinimo));
    }

O programa exibiu:

    -2
     0
     3
     1
    -1
    -2
     4
     1
     4
     3

<h1>Valores aleatórios decimais</h1>

Até agora apenas geramos valores aleatórios inteiros. Vamos ver agora como gerar valores decimais. Analise o seguinte código:

    #include <stdio.h>

    int main(){
        float quociente, dividendo, limiteMaximo;
        dividendo=0;
        limiteMaximo=10;
        for(int i=0; i<=limiteMaximo; i++){
            quociente=dividendo/limiteMaximo;
            printf("%f\n", quociente);
            dividendo++;
        }
        return 0;
    }

O programa irá exibir:

    0.000000
    0.100000
    0.200000
    0.300000
    0.400000
    0.500000
    0.600000
    0.700000
    0.800000
    0.900000
    1.000000


Ou seja, se dividirmos os valores entre 0 e um limiteMaximo, nesse caso 10, pelo próprio limiteMaximo, o que teremos será valores decimais entre e incluindo 0 e 1. Como aplicar isso ao nosso programa? Lembra da faixa de valores da função rand( )? De 0 a 32767. Portanto, basta dividirmos rand( ) por 32767 para obtermos valores entre 0 e 1.

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        float numeroAleatorio;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=rand()/32767;
            printf("%f\n", numeroAleatorio);
        }
        return 0;
    }

O programa exibirá:

    0.000000
    0.000000
    0.000000
    0.000000
    0.000000
    0.000000
    0.000000
    0.000000
    0.000000
    0.000000

Não é o que queremos. Por que isso aconteceu? Lembra dos *casts*? Quando dividimos rand( ) por 32767, estamos dividindo dois valores inteiros, logo o resultado será um inteiro. Para resolvermos isso podemos usar um *cast* do tipo *float*.

    numeroAleatorio=(float)rand()/32767;

E nosso programa exibiu o que buscamos:

    0.276833
    0.499344
    0.317545
    0.985290
    0.376263
    0.237190
    0.403790
    0.602557
    0.570299
    0.496078

Em vez de 32767 podemos usar RAND_MAX, que é uma constante definida na biblioteca stdlib.h que vale 0x7FFF, representação de 32767 em hexadecimal.

    numeroAleatorio=(float)rand()/RAND_MAX;

Por curiosidade, caso esteja usando o Visual Studio Code ou o DevC++, clique sobre <stdlib.h> segurando *Ctrl*, uma nova aba será aberta, agora aperte *Ctrl + F* para realizar uma busca e digite no campo *RAND_MAX*, você encontrá a expressão seguinte, que nada mais é do que a declação de uma constante hexadecimal.

    #define RAND_MAX 0x7FFF

<h2>Limitando o maior valor</h2>

Nosso programa está gerando valores entre e incluindo 0 e 1, para limitar o maior valor basta multiplicar pelo pelo *limiteMaximo*, assim a faixa de valores ficará entre e incluindo 0 e o *limiteMaximo*, nesse caso entre 0 e 4:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        float numeroAleatorio, limiteMaximo;
        limiteMaximo=4;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=((float)rand()/RAND_MAX)*limiteMaximo;
            printf("%f\n", numeroAleatorio); 
        }
        return 0;
    }

O programa exibiu:

    2.094790
    3.973388
    2.826991
    3.921140
    0.303964
    0.266854
    2.721274
    1.295450
    2.860439
    3.004364


<h2>Limitando o menor valor</h2>

Basta somar o menor valor desejado. Assim também conseguimos obter números negativos. Porém aqui o programa gera valores entre e incluindo 0 e 1, a faixa de valores será entre e incluindo o *limiteMinimo* e o *limiteMinimo* + 1, nesse caso entre -4 e -3.

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        float numeroAleatorio, limiteMinimo;
        limiteMinimo=-4;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=limiteMinimo+((float)rand()/RAND_MAX);
            printf("%f\n", numeroAleatorio);
        }
        return 0;
    }

O programa exibiu:

    -3.428388
    -3.231300
    -3.065889
    -3.795434
    -3.516831
    -3.292978
    -3.632160
    -3.645467
    -3.912015
    -3.496109

<h2>Limitando o maior e o menor valor</h2>

Unindo os dois coneceitos temos:

    numeroAleatorio=limiteMinimo+((float)rand()/RAND_MAX)*limiteMaximo;

O programa fica assim:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    int main(){
        float numeroAleatorio, limiteMinimo, limiteMaximo;
        limiteMinimo=2;
        limiteMaximo=5;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=limiteMinimo+((float)rand()/RAND_MAX)*limiteMaximo;
            printf("%f\n", numeroAleatorio);
        }
        return 0;
    }

O programa exibe:

    5.066957
    3.035493
    3.804254
    6.494766
    3.871243
    6.588000
    2.831629
    6.619434
    3.097293
    6.446852

O programa gerou valores maiores que 5, como acontecia para valores aleatórios inteiros, aqui também precisamos subtrair do *limiteMaximo* o *limiteMinimo*:

    numeroAleatorio=limiteMinimo+((float)rand()/RAND_MAX)*(limiteMaximo-limiteMinimo);

Para não precisarmos digitar o código várias vezes, podemos criar uma função. 

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    float sorteiaDecimais(float limiteMinimo, float limiteMaximo);

    int main(){
        float numeroAleatorio;
        srand(time(NULL)); 
        for(int i=0; i<10; i++){
            numeroAleatorio=sorteiaDecimais(-2, 5.5);
            printf("%f\n", numeroAleatorio);      
        }
        return 0;
    }

    float sorteiaDecimais(float limiteMinimo, float limiteMaximo){
        return limiteMinimo+((float)rand()/RAND_MAX)*(limiteMaximo-limiteMinimo);
    }

Observe que tanto o *limiteMinimo* quanto o *limiteMaximo* podem ser valores decimais.
<h2>Unindo tudo</h2>

Agora temos duas funções, uma que gera valores inteiros aleatórios e outra que gera valores decimais aleatórios. Porém podemos ter apenas uma, basta atribuir a função *sorteia* a um inteiro, assim a parte decimal será descartada:

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>

    float sorteia(float limiteMinimo, float limiteMaximo);

    int main(){
        float numeroAleatorioDecimal;
        int numeroAleatorioInteiro;
        srand(time(NULL)); 
        printf("\nValores inteiros aleatorios:\n");
        for(int i=0; i<10; i++){
            numeroAleatorioInteiro=sorteia(-2, 5);
            printf("%d\n", numeroAleatorioInteiro);      
        }
        printf("\nValores decimais aleatorios:\n");
        for(int i=0; i<10; i++){
            numeroAleatorioDecimal=sorteia(-2, 5);
            printf("%f\n", numeroAleatorioDecimal);  
        }
        return 0;
    }

    float sorteia(float limiteMinimo, float limiteMaximo){
        return limiteMinimo+((float)rand()/(RAND_MAX))*(limiteMaximo-limiteMinimo);
    }

O programa exibirá:

    Valores inteiros aleatorios:
    -1
     2
     4
     3
     2
     2
     3
     4
     1
     1

    Valores decimais aleatorios:
    -1.743644
     3.920103
     2.346507
     0.719932
    -0.573595
     2.968596
     4.428754
     4.077975
     1.678701
     3.819056

Porém cuidado ao usar essa função para inteiros, pois a probabilidade de *sorteia* retornar, nesse exemplo, 5 e bem menor do que retornar 4 ou outro valor, pois apenas 5.000000 será 5, já 4.991475, 4.187415, 4.099874 etc. será 4.

Lembrando que há muita discusão na internet sobre valores pseudoaleatórios, além de que o que mostrei nesse artigo é para uso casual pois não asseguro a qualidade desses números randômicos. 