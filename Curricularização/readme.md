# Integração

## Componentes

### ULA

A ULA, mais conhecida como unidade lógica aritmética, é um componente crucial para o bom funcionamento de qualquer processador. Isso porque é a partir dela que conseguimos realizar cálculos numéricos e fazer operações lógicas. Geralmente, a ULA apresenta as seguintes operações: adição, subtração, OR, AND e NOT.

### Registradores

Os registradores são componentes responsáveis pelo armazenamento de informações temporárias, incluindo: resultados de cálculos, endereços de memória e operandos de uma conta feita na ULA. Dentre os diversos tipos de registradores, os principais são:
  * Program counter: indica o endereço que a memória deve ler.
  * Instruction register: armazena a instução que está sendo executada.
  * Banco de registradores: conjunto de registradores responsáveis por armazenar valores que serão usados em operações.

### Memória

A memória, assim como os registradores, é um componente fundamental responsável pelo armazenamento de informações. No entanto, ao contrário dos registradores, a memória possui a capacidade de manter de forma permanente os dados e instruções, permitindo que as informações sejam preservadas ao longo do tempo, mesmo após o processamento das tarefas. Essa característica a torna essencial para o funcionamento contínuo e eficiente do sistema, proporcionando a persistência necessária para a execução de programas e a recuperação de dados durante o ciclo de operação do processador.

## Integração

A integração dos componentes requer a criação de conexões que assegurem o fluxo adequado das informações, permitindo que as tarefas sejam executadas de maneira eficiente. Com os componentes já definidos, a próxima etapa consiste em conectá-los de forma harmônica e funcional:
  * **ULA**: Responsável por realizar operações, tanto com os valores armazenados no banco de registradores quanto com os contidos na memória. Por isso, é essencial estabelecer uma conexão entre o banco de registradores e a ULA, além de uma ligação entre a memória e a ULA.
  * **UC (Unidade de Controle)**: Coordenadora das atividades de todos os outros componentes. Portanto, deve estar conectada a todos os demais componentes para garantir o correto funcionamento do sistema.
  * **Banco de Registradores**: Sua função é fornecer e receber valores tanto da ULA quanto da memória. Assim, é necessário estabelecer conexões entre o banco de registradores, a ULA e a memória, de modo que ele possa interagir com ambos.
  * **Program Counter (PC)**: Responsável por enviar o endereço atual à memória. Dessa forma, é crucial conectar o Program Counter à memória para que a sequência de instruções seja corretamente acessada.
  * **Instruction Register (IR)**: Armazena a instrução que está sendo processada e a transmite à unidade de controle. Assim, é preciso conectar o registrador de instruções tanto à memória quanto à unidade de controle para que a instrução seja corretamente gerenciada.
  * **Memória**: Recebe o endereço do Program Counter, envia a instrução ao Instruction Register, além de intercambiar dados com o banco de registradores e a ULA. Portanto, a memória deve ser conectada ao Program Counter, ao Instruction Register, ao banco de registradores e à ULA, estabelecendo assim um fluxo contínuo de informações.
