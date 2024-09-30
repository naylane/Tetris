<h1 align="center">
  <br>
  <img src="https://github.com/naylane/candi-block/blob/main/img/Candi-Block.png" alt="Candi Block" width="650"></a>
</h1>

<h4 align="center">Projeto da disciplina TEC 499 - Sistemas Digitais.</h4>


## Sumário
- [Visão Geral do Projeto](#Visão-Geral-do-Projeto)
- [Requisitos](#Requisitos)
- [Arquitetura do Kit de Desenvolvimento DE1-SoC](#Arquitetura-do-Kit-de-Desenvolvimento-DE1-SoC)
  - [Acelerômetro ADXL345](#Acelerômetro-ADXL345)
- [Desenvolvimento em C](#Desenvolvimento-em-C)
    - [Comunicação utilizando acesso direto à memória](#Comunicação-utilizando-acesso-direto-à-memória)
    - [Interface VGA e Exibição](#Interface-VGA-e-Exibição)
- [Testes](#Testes)
- [Tecnologias e Ferramentas utilizadas](#Tecnologias-e-Ferramentas-utilizadas)
- [Configurações de Ambiente e Execução](#Configurações-de-Ambiente-e-Execução)
- [Desenvolvedoras](#Desenvolvedoras)
- [Referências](#Referências)


## Visão Geral do Projeto
O nosso jogo se chama Candi Block e sua lógica foi elaborado em uma mistura de Tetris e Candy crush. O agrupamento do jogo é por cores, então as peças caem cada uma de uma cor e o agrupamento ocorre em blocos de 4 cores iguais.
<div align="center">  
  <img align="center" width=60% src="https://github.com/naylane/candi-block/blob/main/img/funcionamento-geral.gif" alt="Funcionamento Geral">
  <p><em>Lógica do Candi Block</em></p>
</div>


## Requisitos
O projeto consiste no desenvolvimento de um jogo inspirado no pioneiro Tetris utilizando o Kit de Desenvolvimento DE1-SoC, no qual o movimento do jogador é captado pelo ```acelerômetro``` presente no Kit, a interação com o jogo inclui as ações de ```iniciar, pausar, continuar e encerrar``` controladas por ```chaves (switches)``` disponíveis na placa e a exibição do jogo em um monitor realizada através da ```interface VGA```.

Sendo necessário atender aos seguintes requisitos:
- O código deve ser escrito em linguagem C;
- O sistema só poderá utilizar os componentes disponíveis na placa;
- Não é permitido o uso de bibliotecas para o acelerômetro;
- O usuário não muda a orientação das peças;
- O jogo deve pontuar e eliminar agrupamentos.

Abrangendo a compreensão e aplicação do conjunto de instruções da arquitetura, técnicas de programação, desenvolvimento de sistemas embarcados, bem como acesso direto à memória e integração do acelerômetro para captar os movimentos do jogador e controlar as peças. 


## Arquitetura do Kit de Desenvolvimento DE1-SoC
O Kit de Desenvolvimento DE1-SoC ostenta uma plataforma de design de hardware robusta com base no FPGA System-on-Chip (SoC) da Altera, que combina núcleos embarcados dual-core Cortex-A9 com lógica programável, oferecendo máxima flexibilidade de design. Dispondo do poder da reconfigurabilidade aliado a um sistema de processador de alto desempenho e baixo consumo de energia. 

O System-on-Chip (SoC) da Altera integra um sistema de processador (HPS) baseado em ARM, composto por processador, periféricos e interfaces de memória conectados de forma integrada à estrutura FPGA, utilizando interconexão de alta largura de banda. Incluindo hardwares como memória DDR3 de alta velocidade, recursos de áudio e vídeo, rede Ethernet, entre outros.

<div align="center">  
  <img align="center" width=60% src="https://github.com/naylane/candi-block/blob/main/img/kit%20de1soc.png" alt="Placa DE1-Soc">
  <p><em>Placa de Desenvolvimento DE1-SoC</em></p>
</div>

### Diagrama de Blocos da Placa DE1-SoC
Para que os usuários desfrutem de máxima flexibilidade, todas as conexões são realizadas através do dispositivo Cyclone V SoC FPGA, proporcionando liberdade ao configurar o FPGA para implementar os mais diversos projetos de sistema.

<div align="center">  
  <img align="center" width=60% src="https://github.com/naylane/candi-block/blob/main/img/diagrama.png" alt="Diagrama de Blocos DE1-Soc">
  <p><em>Diagrama de Blocos</em></p>
</div>

### Acelerômetro ADXL345
A placa DE1-SoC está equipada com um módulo sensor de acelerômetro digital, o ADXL345, também conhecido como G-sensor. Trata-se de um acelerômetro de 3 eixos, pequeno, fino, com ultra baixo consumo de energia e medição de alta resolução. Dispondo de faixas de medição selecionáveis de ±2 g, ±4 g, ±8 g ou ±16 g, proporcionando uma capacidade de detecção da aceleração crescente à medida que a faixa de medição aumenta. Os dados de saída são digitalizados em um formato de 16 bits em complemento de dois e podem ser acessados por meio de interfaces digitais SPI ou I2C.

O ADXL345 é capaz de medir tanto a aceleração estática da gravidade em aplicações de detecção de inclinação, quanto a aceleração dinâmica resultante de movimento ou choque. Sua alta resolução de 3,9 mg/LSB permite a medição de mudanças de inclinação menores que 1,0°, o que o torna ideal para aplicações em dispositivos móveis. 

Além disso, possui modos de baixo consumo que permitem um gerenciamento inteligente de energia baseado em movimento, com detecção de limiar e medição de aceleração ativa com dissipação de energia extremamente baixa, tornando-o altamente eficiente.


## Desenvolvimento em C

### Comunicação utilizando acesso direto à memória
Processo de comunicação com o acelerômetro ADXL345 na placa DE1-SoC, desde as configurações iniciais até a leitura e interpretação dos dados de aceleração, implementada em linguagem C para interagir diretamente com o hardware. 

<div align="center">  
  <img align="center" width=50% src="https://github.com/naylane/candi-block/blob/main/img/anima%C3%A7%C3%A3o%20adxl345%20(2).gif" alt="Comunicacao ADXL345">
  <p><em>Comunicação com o acelerômetro ADXL345</em></p>
</div>

### Interface VGA e Exibição
Para o desenvolvimento do código em C, tivemos que analisar uma lógica de exibição no VGA. Dessa forma, escolhemos pegar um retângulo no VGA com 180 de altura e 100 de comprimento (esses valores foram decisão de projeto, mas poderia ser com quaisquer valores) e pensar nesse retângulo como a nossa matriz que iriamos manipular no codigo.

Assim, preenchemos esse retangulo com blocos de tamanho 5x5 (também decisão de projeto) e verificamos que cabiam um total de 720 blocos. Se temos um retangulo com 180 de altura e cada bloco tem tamanho 5, logo cabem 36 blocos na altura, ou seja, 36 linhas na nossa matriz de manipulação. O mesmo para as colunas, se temos uma largura de 100 com blocos de tamanho 5 então cabem 20 blocos nas colunas desse retangulo, logo temos 20 colunas na nossa matriz de manipulação. 

Então os 720 blocos que cabem ao total na nossa mtriz se referem exatamente aos 720 elementos da nossa matriz de tamanho 36 linhas e 20 colunas.

Com essa logica podemos pensar em cada elemento da matriz sendo um bloco, tendo uma matriz de 720 elementos, e quando fazemos uma alteração dentro de um elemento dessa matriz, alteramos a sua vizualização no VGA.

<div align="center">  
  <img align="center" width=80% src="https://github.com/naylane/candi-block/blob/main/img/exibicao.gif" alt="Exibição">
  <p><em>Desenvolvimento para exibição no monitor</em></p>
</div>


## Testes
Descrição dos testes de funcionamento do sistema, bem como, análise dos resultados alcançados


## Tecnologias e Ferramentas utilizadas
- **Hardwares:**   
  - Kit de Desenvolvimento DE1-SoC
  - Monitor   
- **Linguagem de Programação:** C   
- **Ambiente de Desenvolvimento:** Visual Studio Code   
- **Compilador:** GCC   
- **Controle de Versão:** Git     
- **Ferramenta de Sistema:** Terminal Linux


## Configurações de Ambiente e Execução
Para ter acesso ao projeto, clone o repositório disponível na plataforma GitHub utilizando o seguinte comando no terminal Linux:
```bash
git clone https://github.com/naylane/candi-block.git
```
Após clonar o repositório, conecte-se à placa via SSH utilizando o seu respectivo IP. Por exemplo, se o IP for `10.0.0.120`, use o seguinte comando:
```bash
ssh aluno@10.0.0.120
```
Em seguida, transfira a pasta clonada do seu computador para o sistema de arquivos da placa:
```bash
mv candi-block/[caminho do destino]
```
Para compilar e executar o projeto desenvolvido, navegue até o diretório onde está o repositório e execute o comando:
```bash
make
```
O comando `make` gerará o arquivo de compilação e o executará. Se a operação for bem-sucedida, a tela inicial do Candi Block deverá aparecer no monitor ao qual a placa está conectada.
<br>
⚠️ **Observação:** para seguir esse passo a passo será necessário saber a senha do usuário `aluno`.


## Desenvolvedoras
<table>
  <tr>
    <td align="center"><img style="" src="https://avatars.githubusercontent.com/u/142849685?v=4" width="100px;" alt=""/><br /><sub><b> Brenda Araújo </b></sub></a><br />👨‍💻</a></td>
    <td align="center"><img style="" src="https://avatars.githubusercontent.com/u/89545660?v=4" width="100px;" alt=""/><br /><sub><b> Naylane Ribeiro </b></sub></a><br />👨‍💻</a></td>
    <td align="center"><img style="" src="https://avatars.githubusercontent.com/u/143294885?v=4" width="100px;" alt=""/><br /><sub><b> Sara Souza </b></sub></a><br />👨‍💻</a></td>    
  </tr>
</table>


## Referências
- [1] FPGAcademy. (2024) https://fpgacademy.org/
