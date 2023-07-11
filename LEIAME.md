# EP4 - Simulação de Vôo

TODO:
[ ] consertar bug que faz eu conseguir ver a nave em movimento

Para facilitar a visualização dos efeitos da fonte, de luz, coloque a fonte de luz em uma posição “finita”, relativamente perto dos objetos, e desenha a fonte de luz com uma esfera branca de raio 5.

A criação da cena deve agora considerar os efeitos de iluminação de Phong.

O comportamento do programa continua basicamente o mesmo do EP3, usando a mesma interface. A única novidade é que seu programa deve permitir criar múltiplas naves e, durante a simulação, trocar a nave (câmera) usada para renderizar a cena pelas teclas ‘m’ e ‘n’. Para isso, considere uma lista circular LNAVES contendo as naves. Ao teclar ‘m’ a próxima nave dessa lista deve ser usada para renderizar a cena e, ao teclar ‘n’, use a nave anterior. As demais teclas devem continuar com o mesmo comportamento definido no enunciado do EP3.

Sugestão de roteiro
Leia os capítulos 16 e 17 das notas de aula para rever como implementar o modelo de Phong usando WebGL.

Teste a renderização dos efeitos de iluminação com apenas 1 ou 2 objetos (esferas e cubos), usando mensagens no console.log() para mostrar o que o seu programa está fazendo com a câmera parada, apenas cada objeto se movendo. Implemente e teste os botões ‘Executar/Pausar’ e ‘Passo’, acredito que vão lhe ajudar muito. Eu usei um intervalo de 1 s para cada passo da simulação.

Teste a renderização de múltiplos cubos e esferas, com a câmera parada.

Implemente e teste a navegação com apenas 1 nave (câmera), carregando apenas i-ésima nave por exemplo. Observe que a “câmera” de cada nave é independente das demais, inclusive seus parâmetros podem ser distintos.

Teste as teclas de velocidade, com mensagens apropriados ao console. Com a nave parado, modifique a variação de rotação diretamente no console e clique no botão passo para verificar se seus cálculos estão corretos.
Teste carregando agora múltiplas naves e usando as teclar ‘m’ e ‘n’ para modificar o ponto de vista usado na renderização. CUIDADO: o que acontece se a câmera usada para renderizar a cena estiver dentro de uma esfera?

Mais sugestões
O arquivo ep4-config.js contém um exemplo de arquivo de configuração com os objetos, cores e outras propriedades que usei no demo. Fique a vontade de usar esses valores para depuração mas procure utilizar outros valores no seu trabalho. Sua cena deve conter, além do cubo de referência, outros 3 cubos, 3 esferas e 3 naves.

discuta suas dúvidas com seus colegas no fórum da disciplina.

use o mouse para controlar pitch e yaw.

não deixe para a última hora!

renderize os objetos aplicando texturas.

O que você deve entregar
Entregue em um arquivo .zip contendo todos os arquivos necessários para rodar o seu programa. Procure não utilizar arquivos externos disponíveis na Internet, além dos
módulos que costumamos usar em outros exercícios. Junte tudo em uma pasta, zip e submeta o arquivo zip. Além de todos os arquivos necessário para executar o seu programa [80% da nota], você deve entregar também um arquivo de documentação com o nome leiame.txt (ou leiame.md ou similar).

Nesse arquivo leiame.xxx indique brevemente o que você fez e, caso tenha feito algo diferente (ou extra como partes opcionais) do que foi proposto, descreva. Descreva bugs que você conhecer no seu programa. Descreva também as dependências, outros arquivos e módulos usados em seu programa, caso houver.

Honestidade Acadêmica
Esse é um exercício individual, não em grupo. Isso não significa que você não pode receber ajuda de outras pessoas, inclusive de seus colegas. De uma forma geral, gostaríamos de incentivar as discussões de ideias, conceitos e alternativas de solução. Nossa maior recomendação é evitar olhar o código fonte de uma solução antes de escrever o seu programa. Em caso de dúvida, consulte nossa página.

De forma sucinta, evite as seguintes ações que caracterizam desonestidade acadêmica na realização dos trabalhos individuais desse curso:

buscar e obter uma solução (parcial ou completa, correta ou não) de exercício programa (EP) na internet ou qualquer outro meio físico ou virtual, durante o período de submissão do referido EP;
solicitar ou obter uma cópia (parcial ou completa, correta ou não) da solução de um EP durante o seu período de submissão;
permitir que um colega acesse uma cópia (parcial ou completa, correta ou não) do seu EP, durante o período de submissão;
ainda mais grave é o plágio, que se configura pela utilização de qualquer material não visto em aula ou não descrito no enunciado, que não seja de sua autoria, em parte ou ao todo, e entregar, com ou sem edição, como se fosse seu trabalho, para ser avaliado.



## 1. INTRODUÇÃO

Estes arquivos contêm minha implementação de um simulador de vôo, utilizando-se de classes para criar a lógica da simulação (Engine), da starship (Starship) e dos objetos (Shape, Cube e Sphere).
A classe Engine é responsável por gerar a Starship e as Shapes e gerenciar a simulação, guardar seu estado e controlar seu tempo. Os inputs do usuário são passados ao Engine, que os encaminha 
para processamento pela Starship. O movimento da Starship considera sua posição, sua direção e sua velocidade (representada por um vetor). A aceleração da Starship é realizada sempre na direção 
para a qual ela aponta, de modo que seu movimento inercial é mantido até que a aeronave acelere em alguma direção ou seja parada por completo. A classe Shape é responsável por gerar as matrizes de
transformações dos objetos. As classes Cube e Sphere guardam os modelos iniciais dessas formas geométricas e estendem a classe Shape.

As formas são definidas a partir da linha 26 do arquivo engine.js. Cada forma é criada por meio da chamada da classe com métodos que representam as transformações que eu realizo sobre a forma. As
transformações podem ser `rotate(ângulos)`, `scale(fatores)`, `translate(ponto)` e `animate(params)`, esta última representando a rotação durante a animação e as demais são estáticas, a partir do
referencial do ponto (0, 0, 0). Ou seja, animate irá estabelecer os parâmetros da animação, que são as velocidades de rotação em cada eixo. Durante a chamada de `shape.transform` em `engine.update`,
um novo modelo será criado dentro do javascript e será passado ao GLSL para renderização.

Nota-se que todas as formas, exceto o cubo de referência, estão se movimentando. Mesmo a plataforma. Ainda, todas as cores são aleatórias, exceto as do cubo de referência.

### 1.1. Organização

O código está organizado em 3 diretórios, com a estrutura a seguir:

.
├── css
│   └── ep03.css
├── LEIAME.md
├── lib
│   ├── models
│   ├── MVnew.js
│   ├── shaders
│   │   ├── spaceship.frag
│   │   └── spaceship.vert
│   ├── utils.js
│   ├── vec3.js
│   └── webglUtils.js
└── webpage
    ├── cube.js -> define Cube, a forma de um cubo.
    ├── engine.js -> define Engine, a classe responsável por guardar as informações da simulação, por gerar
    │		     as formas, gerar o tick() e controlar o tempo da simulação.
    ├── ep3.js -> contém a função main() que cria um novo Engine.
    ├── index.html
    ├── shape.js -> define Shape, superclasse de Cube e Sphere. Contém as funções para gerar 
    │		    matrizes de transformação sobre as formas.
    ├── sphere.js -> define Sphere, a forma de uma esfera.
    └── starship.js -> define Starship, que navega pelo espaço de formas com uma câmera acoplada
		       em seu nariz. A nave tem motores na frente e atrás para gerar aceleração e
		       mudar a velocidade na direção do nariz (além, é claro, dos motores responsáveis
		       por gerar as rotações pitch, yaw e roll).


### 1.2. Execução

Para executar o programa, primeiro é preciso extrair todos os arquivos para um diretório e servir o diretório em localhost.

De dentro do diretório:

    $ python3 -m http.server

Depois, acessar a página index.html pelo browser:

    localhost:8000/webpapage/

### 1.3. Comandos

Os comandos seguem os comandos descritos no vídeo, e não no enunciado do EP. Os comandos da starship são:

- Pitch: pitch é realizado pelas teclas W e X. Essas teclas incrementam e decrementam a rotação no eixo x. W faz o nariz da starship descer e X faz o nariz subir.
- Yaw: yaw é realizado pelas teclas A e D. Essas teclas incrementam e decrementam a rotação no eixo y. A faz a starship virar para a esquerda e D para a direita.
- Roll: roll é realizado pelas teclas Z e C. Essas teclas incrementam e decrementam a rotação no eixo z. Z faz a starship girar no sentido horário e C no sentido anti-horário.
- Acelerar: Teclas L, Z e J. L acelera na direção para onde aponta a starship. J acelera na direção contrária para onde aponta a starship. Z zera a velocidade.

### 1.4. Botões

O botão Executar/Pausar executa ou pausa a animcação e o botão Passo avança a animação em aproximadamente 10 frames.

## 2. HORAS DEDICADAS

Dia 14 de Junho:
Início 12h
Fim 18h

Dia 19 de Junho
Início 16h
Fim 22h

Dia 20 de Junho
Início 14h
Fim 20h30


## 3. DIFICULDADES

A maior dificuldade envolveu a movimentação da aeronave. Não tive nenhuma outra dificuldade em particular.


## 4. BUGS

Nenhum bug conhecido.

## 5. AUTOR

Henrique de Carvalho
11819104
