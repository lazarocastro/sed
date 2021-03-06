# Sed

COMANDOS DE 1 LINHA PARA O SED (Editor Unix de fluxo)           Abril 25, 1998

Compilado por Eric Pement <epement@jpusa.chil.il.us>            versão 4.5

Traduzido por Ricardo Sartori <sartori@lrv.ufsc.br>

A versão mais atualizada deste arquivo pode ser encontrada em <http://www.wollery.demon.co.uk>

## Alfabeto do sed

Opção | Nome | Descrição
:-----: | ---- | --------
**a**  |  append     |   Anexa um texto após o <span style="color:green;">**[BUFFER]**</span> 
**b**  |  branch     |   Pula até uma marcação
**c**  |  change     |   Troca o <span style="color:green;">**[BUFFER]**</span> por um texto
**d**  |  delete     |   Apaga o <span style="color:green;">**[BUFFER]**</span> 
**D**  |  delete     |   Apaga a primeira linha do <span style="color:green;">**[BUFFER]**</span> 
**e**  |  execute    |   Executa um comando do sistema #GNU-sed
**F**  |  filename   |   Imprime o nome do aquivo atual #GNU-sed
**g**  |  get        |   Copia texto do <span style="color:red;">**[RESERVA]**</span> pro <span style="color:green;">**[BUFFER]**</span> (sobrescreve)
**G**  |  get        |   Copia texto do <span style="color:red;">**[RESERVA]**</span> pro <span style="color:green;">**[BUFFER]**</span> (anexa)
**h**  |  hold       |   Copia texto do <span style="color:green;">**[BUFFER]**</span> pro <span style="color:red;">**[RESERVA]**</span> (sobrescreve)
**H**  |  hold       |   Copia texto do <span style="color:green;">**[BUFFER]**</span> pro <span style="color:red;">**[RESERVA]**</span> (anexa)
**i**  |  insert     |   Insere um texto antes do <span style="color:green;">**[BUFFER]**</span>
**l**  |  list       |   Imprime o <span style="color:green;">**[BUFFER]**</span> mostrando caracteres invisíveis
**n**  |  next       |   Carrega a próxima linha no <span style="color:green;">**[BUFFER]**</span> 
**N**  |  next       |   Anexa a próxima linha no <span style="color:green;">**[BUFFER]**</span> 
**p**  |  print      |   Imprime o conteúdo do <span style="color:green;">**[BUFFER]**</span> 
**P**  |  print      |   Imprime a primeira linha do <span style="color:green;">**[BUFFER]**</span> 
**q**  |  quit       |   Imprime o <span style="color:green;">**[BUFFER]**</span> e finaliza o Sed
**Q**  |  quit       |   Descarta o <span style="color:green;">**[BUFFER]**</span> e finaliza o Sed #GNU-sed
**r**  |  read       |   Mostra conteúdo do arquivo após o <span style="color:green;">**[BUFFER]**</span> 
**R**  |  read       |   Mostra uma linha do arquivo após o <span style="color:green;">**[BUFFER]**</span> #GNU-sed
**s**  |  substitute |   Substitui um trecho de texto por outro
**t**  |  tee        |   Pula na marcação, se um s/// funcionou
**T**  |  tee        |   Pula na marcação, se nenhum s/// funcionou #GNU-sed
**v**  |  version    |   Aborta se a versão do sed for incompatível #GNU-sed
**w**  |  write      |   Grava o <span style="color:green;">**[BUFFER]**</span> num arquivo
**W**  |  write      |   Grava a 1ª linha do <span style="color:green;">**[BUFFER]**</span> num arquivo #GNU-sed
**x**  |  eXchange   |   Troca os conteúdos do <span style="color:green;">**[BUFFER]**</span> e <span style="color:red;">**[RESERVA]**</span>
**y**  |  ?          |   Troca caracteres
**z**  |  zap        |   Limpa o conteúdo do <span style="color:green;">**[BUFFER]**</span> #GNU-sed

Nota:

   <span style="color:green;">**[BUFFER]**</span> - Buffer padrão do sed (pattern space)

   <span style="color:red;">**[RESERVA]**</span> - Buffer reserva do sed (hold space)

## PREENCHIMENTO DE ARQUIVOS:

Duplicar o tamanho de um arquivo

```bash
sed G
```

Triplicar o tamanho de um arquivo

```bash
sed 'G;G'
```

Desfazer a duplicação de tamanho (assume que as linhas de números iguais
estão em branco)

```bash
sed 'n;d'
```

## NUMERACÃO:

Numera cada linha de um arquivo (com alinhamento simples a esquerda). Usar
uma tabulação em vez do espaço vai preservar as margens. (veja a observação
sobre o '\t' no final desse arquivo)

```bash
sed = arquivo | sed 'N;s/\n/\t/'
```

Numera cada linha de um arquivo (números à esquerda, alinhados à direita)

```bash
sed = arquivo | sed 'N; s/^/     /; s/ *\(.\{6,\}\)\n/\1  /'
```

Numera cada linha de um arquivo, mas só imprime os números se a linha não
estiver em branco

```bash
sed '/./=' arquivo | sed '/./N; s/\n/ /'
```

Conta as linhas (emula o "wc -l")

```bash
sed -n '$='
```

## CONVERSÃO DE TEXTO E SUBSTITUIÇÃO:

EM AMBIENTE UNIX: converte o caractere de linha nova do DOS (CR/LF) para o 
formato unix

```bash
sed 's/.$//'               # assume que todas as linhas terminam com CR/LF
sed 's/^M$//'              # no bash/tcsh, pressione Ctrl-V depois Ctrl-M
sed 's/\x0D$//'            # somente no sed v1.5
```

EM AMBIENTE DOS: converte o caractere de linha nova do Unix (LF) para 
o formato DOS

```bash
sed 's/$//'                          # método 1
sed -n p                             # método 2
```

Apaga o espaço em branco inicial (espaço, tabulação) do começo
de cada linha, puxando o texto para a esquerda

```bash
sed 's/^[ \t]*//'                    # veja a nota sobre o '\t' no final
                                     # deste arquivo
```

Apaga o espaço em branco final (espaço, tabulação) do final de cada linha

```bash
sed 's/[ \t]*$//'                    # veja a nota sobre o '\t' no final
                                     # deste arquivo
```

Deleta AMBOS o espaço em branco final e inicial de cada linha

```bash
sed 's/^[ \t]*//;s/[ \t]*$//'
```

Insere 5 espaços em branco no ínicio de cada linha (faz o "offset" da pagina)

```bash
sed 's/^/     /'
```

Alinha todo a direita, numa coluna de 79 caracteres de largura

```bash
sed -e :a -e 's/^.\{1,78\}$/ &/;ta'  # definido como 78 mais 1 espaço
```

Centraliza todo o texto no meio de uma coluna de 79 caracteres de
largura. No método 1, os espaços no começo da linha são significativos,
e espaços em branco são anexados ao final de cada linha. No método 2,
os espaços no início de cada linha são descartados, logo, não é
adicionado nenhum espaço no final de cada linha.

```bash
sed  -e :a -e 's/^.\{1,77\}$/ & /;ta'                     # método 1
sed  -e :a -e 's/^.\{1,77\}$/ &/;ta' -e 's/\( *\)\1/\1/'  # método 2
```

Substituir (achar e trocar) "foo" por "bar" em cada linha

```bash
sed 's/foo/bar/'             # troca somente a 1a instância de uma linha
sed 's/foo/bar/4'            # troca somente a 4a instância de uma linha
sed 's/foo/bar/g'            # troca TODAS as instâncias de uma linha
```

Substitui "foo" por "bar" SOMENTE nas linhas que contem "baz" 

```bash
sed '/baz/s/foo/bar/g'
```

Substitui "foo" por "bar" EXCETO nas linhas que contem "baz"

```bash
sed '/baz/!s/foo/bar/g'
```

Reverter a ordem das linhas (emula o "tac")

```bash
sed '1!G;h;$!d' 
```

Reverte cada caractere em cada linha (emula o "rev")

```bash
sed '/\n/!G;s/\(.\)\(.*\n\)/&\2\1/;//D;s/.//'
```

Une pares de linhas lado a lado (como o "paste")

```bash
sed 'N;s/\n/ /'
```

## IMPRESSÃO SELETIVA DE CERTAS LINHAS:

Imprime as primeiras 10 linhas de um arquivo (emula o comportamento do "head")

```bash
sed 10q
```

Imprime a primeira linha de um arquivo (emula o "head -1")

```bash
sed q
```

Imprime as últimas 10 linhas de um arquivo (emula o "tail") 

```bash
sed -e :a -e '$q;N;11,$D;ba'
```

Imprime somente a última linha de um arquivo (emula o "tail -1")

```bash
sed '$!d'
```

Imprime somente as linhas que se encaixam na expressão regular (emula o "grep")

```bash
sed -n '/regexp/p'           # método 1
sed '/regexp/!d'             # método 2
```

Imprime somente as linhas que NÃO se encaixam na regexp (emula o "grep -v")

```bash
sed -n '/regexp/!p'          # método 1, corresponde ao descrito acima
sed '/regexp/d'              # método 2, sintaxe mais simples
```

Imprime uma linha de contexto antes e depois da expressão regular,
com o número da linha indicando onde a expressão regular 
aparece (similar ao "grep -A1 -B1")

```bash
sed -n -e '/regexp/{=;x;1!p;g;$!N;p;D;}' -e h
```

Procura e imprime por AAA e BBB e CCC (em qualquer ordem)

```bash
sed '/AAA/!d; /BBB/!d; /CCC/!d'
```

Procura e imprime por AAA e BBB e CCC (nessa ordem)

```bash
sed '/AAA.*BBB.*CCC/!d'
```

Procura e imprime por AAA ou BBB ou CCC (emula o "egrep")

```bash
sed -e '/AAA/b' -e '/BBB/b' -e '/CCC/b' -e d
```

Imprime um parágrafo se ele possuir AAA (linhas vazias separam os parágrafos).
Com o HHsed v1.5 deve ser inserido o 'G;' apos o 'x;', nos 3 scripts abaixo

```bash
sed -e '/./{H;$!d;}' -e 'x;/AAA/!d;'
```

Imprime um parágrafo se ele possuir AAA e BBB e CCC (em qualquer ordem)

```bash
sed -e '/./{H;$!d;}' -e 'x;/AAA/!d;/BBB/!d;/CCC/!d'
```

Imprime o parágrafo inteiro se ele possuir AAA ou BBB ou CCC

```bash
sed -e '/./{H;$!d;}' -e 'x;/AAA/b' -e '/BBB/b' -e '/CCC/b' -e d
```

Imprime somente as linhas com 65 caracteres ou mais

```bash
sed -n '/^.\{65\}/p'
```

Imprime somente as linhas com menos que 65 caracteres

```bash
sed -n '/^.\{65\}/!p'        # método 1, corresponde ao descrito acima
sed '/^.\{65\}/d'            # método 2, sintaxe mais simples
```

Imprime uma parte do arquivo que vai da expressão regular até
o final do mesmo

```bash
sed -n '/regexp/,$p'
```

Imprime uma parte do arquivo baseada nos números das linhas (linhas 8-12,
inclusive)

```bash
sed -n '8,12p'               # método 1
sed '8,12!d'                 # método 2
```

Imprime a linha de número 52

```bash
sed -n '52p'                 # método 1
sed '52!d'                   # método 2
sed '52q;d'                  # método 3, eficiente com arquivos grandes
```

Imprime um pedaço de arquivo que está entre as duas
expressões regulares (inclusive)

```bash
sed -n '/Iowa/,/Montana/p'             # é case sensitive
```


DELEÇÃO SELETIVA DE CERTAS LINHAS:

Imprime todo o arquivo EXCETO a parte entre 2 expressões regulares

```bash
sed '/Iowa/,/Montana/d'
```

Deleta linhas duplicadas de um arquivo (emula o "uniq"). A primeira
linha de um conjunto de linhas duplicadas é mantida, o resto é deletada

```bash
sed '$!N; /^\(.*\)\n\1$/!P; D'
```

Deleta TODAS as linhas em branco de um arquivo (o mesmo que "grep '.' ")

```bash
sed '/^$/d'
```

Deleta todas as linhas brancas CONSECUTIVAS de um arquivo exceto a primeira;
ainda deleta todas as linhas em branco do início e fim do arquivo (emula o
"cat -s")

```bash
sed '/./,/^$/!d'          # método 1, permite 0 brancos no topo, 1 no
                          # final do arquivo
sed '/^$/N;/\n$/D'        # método 2, permite 1 branco no top, 0 no
                          # final do arquivo   
```

Deleta todas as linhas em branco do arquivo, exceto as 2 primeiras:

```bash
sed '/^$/N;/\n$/N;//D'
```

Deleta todas as linhas em branco iniciais, no início do arquivo

```bash
sed '/./,$!d'
```

Deleta todas as linhas em branco finais, no final do arquivo

```bash
sed -e :a -e '/^\n*$/N;/\n$/ba'
```

Deleta a última linha de cada parágrafo

```bash
sed -n '/^$/{p;h;};/./{x;/./p;}'
```

# APLICAÇÕES ESPECIAIS:

Remove overstrikes nroff (caracter, backspace) das man pages. O comando
'echo' pode precisar da opção -e se você usar Unix System V ou uma shell bash

```bash
sed "s/.`echo \\\b`//g"    # as aspas duplas são necessárias em ambiente Unix
sed 's/.^H//g'             # no bash/tcsh, pressione Ctrl-V e depois Ctrl-H
sed 's/.\x08//g'           # expressão hexadecimal para o sed v1.5
```

Mostra as mensagens de cabeçalho de um Usenet/e-mail

```bash
sed '/^$/q'                # deleta tudo após a primeira linha em branco
```

Mostra o corpo da mensagem de um Usenet/e-mail

```bash
sed '1,/^$/d'              # deleta tudo "up to" da primeira linha em branco
```

Mostra o cabeçalho Subject, mas remove a porção inicial "Subject :"

```bash
sed '/^Subject: */!d; s///;q'
```

Pega o cabeçalho de endereço de resposta 

```bash
sed '/^Reply-To:/q; /^From:/h; /./d;g;q'
```

Verifica o endereço de maneira correta. Pega o endereço de e-mail
através da 1a linha do cabeçalho de endereço de retorno (veja
o script acima)

```bash
sed 's/ *(.*)//; s/>.*//; s/.*[:<] *//'
```

Adiciona um sinal de maior com um espaço a cada linha (citação de uma
mensagem)

```bash
sed 's/^/> /'
```

Deleta o sinal de maior e o espaço de cada linha (remove a
citação de uma mensagem)

```bash
sed 's/^> //'
```

Remove a maioria das tags HTML (acomoda tags de múltiplas linhas)

```bash
sed -e :a -e 's/<[^>]*>//g;/zipup.bat
dir /b *.txt | sed "s/^\(.*\)\.TXT/pkzip -mo \1 \1.TXT/" >>zipup.bat
```

USO TÍPICO: O sed pega um ou mais comandos de edição e aplica todos eles,
em sequência, a cada linha de entrada. Após todos os comandos terem sido
aplicados a primeira linha de entrada, ela é jogada para a saída e a 
segunda linha de entrada começa a ser processada, e o ciclo recomeça.
O exemplo acima assume que a entrada venha do dispositivo padrão  (por exemplo,
o console, onde  normalmente a entrada é via pipe). Um ou mais nomes de 
arquivo podem ser passados na linha de comando se a entrada não vier da
entrada padrão. A saída é mandada para a saída padrão (a tela). Assim:

```bash
cat arquivo | sed '10q'        # usa a entrada via pipe
sed '10q' arquivo              # tem o mesmo efeito, mas evita o uso do "cat"
sed '10q' arquivo > novo-arquivo    # redireciona a saída para o disco
```

Para instruções de sintaxe adicionais, incluindo a maneira de aplicar
comandos de edição a partir de um arquivo no disco, ao invés da linha
de comando, consulte "sed & awk, 2nd Edition," por Dale Dougherty e
Arnold Robbins (O'Reilly, 1997; http://www.ora.com), "UNIX Text 
Processing," por Dale Dougherty e Tim O'Reilly (Hayden Books, 1987)
ou os tutoriais do Mike Arst distribuídos como U-SEDIT2.ZIP (em vários
sites). Para explorar totalmente o poder do sed, deve-se entender
as "expressões regulares". Para isso, veja "Mastering Regular Expressions"
de Jeffrey Friedl (O'Reilly, 1997). As páginas de manual ("man pages")
dos sistemas Unix podem ser úteis (tente "man sed", "man regexp", ou
a subseção sobre expressões regulares no "man ed"), mas as páginas
de manual são notadamente mais difíceis de se compreender.
Elas não são escritas para ensinar o uso do sed ou das 
expressões regulares para usuários iniciantes, mas como texto de
referência para aqueles que já tem certa experiência com as duas ferramentas.

SINTAXE DAS ASPAS: Os exemplos acima utilizam as aspas simples ('...')
ao invés das aspas duplas ("...") para agrupar comandos de edição,
visto que o sed é comumente utilizado em plataformas Unix. As
aspas simples impedem que a shell Unix interprete o cifrão ($)
e a crase (`...`), os quais seriam expandidos pela shell se estivessem
dentro das aspas duplas. Usuários da shell "csh" e derivadas
ainda precisarão utilizar a barra invertida (\) antes do sinal de 
exclamação (po exemplo \!) para conseguir rodar os exemplos acima, mesmo
usando as aspas simples. Versões do sed escritas para o DOS
invariavelmente necessitam das aspas duplas ("...") ao invés das
aspas simples para agrupar os comandos de edição.

USO DO '\t' NOS SCRIPTS SED: Para maior clareza na documentação, nós
utilizamos a expressão '\t' para indicar o caractere de tabulação 
(0x09) nos scripts sed. Porém, a maioriaa das versões do sed não reconhece
a abreviação '\t', logo, quando for executar estes scripts via linha
de comando, você deve apertar a tecla TAB. A abreviação '\t' só
é reconhecida como um metacaractere nas expressões regulares no
awk, perl e em algumas implementações do sed.

VERSÕES DO SED: As versões do sed diferem entre si, logo, algumas 
variações na sintaxe são esperadas. Em particular, a maioria
não suporta o uso de rótulos (:nome) ou instruções ramificadas 
(b,t) na edição dos comandos, exceto no final dos mesmos. Nós utilizamos
uma sintaxe que seria portável para a maioria dos usuarios do sed,
mesmo sabendo que a popular versão GNU do sed permite uma sintaxe
mais sucinta. Quando o leitor vê um comando comprido como esse:

```bash
sed -e '/AAA/b' -e '/BBB/b' -e '/CCC/b' -e d
```

É importante que ele saiba que o GNU sed permite uma redução, como:

```bash
sed '/AAA/b;/BBB/b;/CCC/b;d'
```

Lembre-se ainda que, apesar de muitas versões do sed aceitarem comandos
como "/one/ s/RE1/RE2/", algumas não permitem o uso de "/one/! s/RE1/RE2/",
a qual contém um espaço antes do 's'. Omita o espaço quando for digitar
o comando.

OTIMIZANDO PARA MAIOR VELOCIDADE: Se a velocidade de execução precisa
aumentar (em virtude de grandes arquivos de entrada ou de processadores
e discos rígidos lentos), a substituição será executada mais rapidamente
se a expressão de "procura" é especificada antes da instrução
"s/.../.../". Assim:

```bash
sed 's/foo/bar/g' arquivo         # comando de substituição padrão
sed '/foo/ s/foo/bar/g' arquivo   # executa de forma mais rápida
sed '/foo/ s//bar/g' arquivo      # sintaxe mais sucinta
```

Na seleção ou remoção de linhas nas quais você somente precisa
ver uma primeira parte de um arquivo, o comando "quit" (q) no script
irá reduzir drasticamente o tempo de processamento para arquivos
grandes. Assim:

```bash
sed -n '45,50p' arquivo           # imprime as linhas nos. 45-50
sed -n '51q;45,50p' arquivo       # mesma coisa, mas faz muito mais
                                  # rapidamente
```

---
Se você deseja contribuir com mais scripts ou se achou algum erro neste
documento, por favor mande um email para o compilador. Indique a
versão do sed que voce usou, o sistema operacional para o qual ele
foi compilado e a natureza do problema. Vários scripts mostrados
neste arquivo foram escritos por:

Al Aab <af137@freenet.toronto.on.ca>   # moderador da lista "seders"
Yiorgos Adamopoulos <adamo@softlab.ece.ntua.gr>
Dale Dougherty <dale@songline.com>     # autor do "sed & awk"
Carlos Duarte <cdua@algos.inesc.pt>    # autor do "do it with sed"
Eric Pement <epement@jpusa.chi.il.us>  # autor deste documento
S.G.Ravenhall <S.G.Ravenhall@open.ac.uk> # um grande script de-html
Greg Ubben <gsu@romulus.ncsc.mil>      # muitas contribuições & muita ajuda
