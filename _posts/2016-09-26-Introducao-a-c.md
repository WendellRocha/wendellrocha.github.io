---
layout: post
title: Introdução à C
date: 2016-09-26 08:45:00
---

Oi pessoal!  

Pretendo ir adicionando na página um pouco de tudo. Comecei falando [sobre o Zabbix](https://wendellrocha.github.io/articles/2016-09/instalando-o-zabbix) e hoje vou dar uma pequena introdução sobre C.
Então vamos lá! :)  

### Tipos de dados

| Tipo           | Espaço   | Escala                            |
| :------------: | :------: | :--------------------------------:|
| char           | 1 byte   | -128 a +127                       |
| int            | 2 bytes  | -32.768 a + 32.767                |
| double         | 4 bytes  | 3.4e^-308 a 1.7e^+308             |
| void           | nenhum   | nenhum                            |
| unsigned char  | 1 byte   | 0 a 255 = 2^8                     |
| unsignet int   | 2 bytes  | 0 a 65.535 = 2^16                 |
| long int       | 4 bytes  | -2.147.484.648 a + 2.147.484.647  |

 * O tipo void é utilizado quando a função não tem retorno.  
   Ex:  
   
	``` c
	#include <stdio.h>
	void imprimeNome() {
		print("Wendell");  
	}
	```
	
#### Expressões booleanas

Expressões booleanas retornam 1 se a expressão for verdadeira e 0 se a expressão for falsa.  
  
|            |                             |  
| :--------: | :-------------------------: |
| x == y     | x igual a y                 |
| x != y     | x diferente de y            |
| x < y      | x menor que y               |
| x > y      | x maior que y               |
| x <= y     | x menor (que) ou igual a y  |
| x >= y     | x maior (que) ou igual a y  |

#### Operadores lógicos

|            |            |
| :--------: | :--------: |
| !x         | NOT x      |
| x && y     | x AND y    |
| x \|\| y   | x OR y     |

#### Operadores aritméticos

|           |              |
| :-------: | :----------: |
| +         | soma         |
| -         | subtração    |
| *         | multiplicação|
| /         | divisão      |
| %         | resto        |  

 * Operadores aritmétios > Operadores relacionais > Operadores lógicos
 
#### Operadores de atribuição
 
 | Expressão  | Forma compacta |
 | :--------: | :------------: |
 | x = x + y  | x+=y           |
 | x = x - y  | x-=y           |
 | x = x * y  | x*=y           |
 | x = x / y  | x/=y           |
 | x = x % y  | x%=y           |  
 
  * Incremento e decremento  
	**Forma prefixa:** ++variavel; --variavel;  
	**Forma posfixa:** variavel++; variavel--;  
	
  * **Importante!**  
	Na forma **prefixa**, primeiro a variável é alterada e depois utiizada.  
	Na forma **posfixa**, primeiro a variável é utilizada e depois alterada.  
	
  Ex:  
	
  ``` c
  main () {
	int x = 0;
	printf("x = %d\n", x++);
	printf("x = %d\n", x);
	printf("x = %d\n", ++x);
	printf("x = %d\n", x);
  }
  ```
  
  Nesse caso, a saída será:

  ```
  x = 0
  x = 1
  x = 2
  x = 2
  ```
	
Bom pessoal, por hoje é só o básico. Já estou elaborando o próximo post e vou liberar nos próximos dias.  
Até mais! :)
