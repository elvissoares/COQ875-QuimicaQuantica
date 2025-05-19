# COQ875 - Química Quântica de Moléculas e Sólidos

Esse repositório contem as notas de aula e códigos de programação e notebooks para o curso COQ875 - Química Quântica de Moléculas e Sólidos do PEQ/COPPE na UFRJ.

O curso é desenvolvido para estudantes de pós-graduação de Engenharia Química e áreas afins. 

O foco é no aprofundamento dos conceitos teóricos e métodos computacionais para a resolução de problemas envolvendo química quântica e suas aplicações em sistemas moleculares, sólidos e interfaces como em fenômenos interfaciais e catalíticos. 

## Ementa

A ementa para a edição do curso em 2025/2 está disponível [aqui](Ementa-COQ875-Quimica_Quantica_de_Moleculas_e_Solidos.pdf)

**MÉTODO DE TRABALHO:** aulas teórico-práticas em sala-de-aula e laboratório de computação com
realização de exercícios em aula e listas de exercício extra-classe. Utilização de softwares livres
como ASE, PySCF, GPAW e Quantum ESPRESSO.

**CRITÉRIOS DE AVALIAÇÃO:** avaliação baseada em listas de exercícios extra-classe e projetos de
final de curso.

## Referências 
- Vianna, D. M., Fazzio, A., Canuto, S. (2018). **Teoria Quântica de Moléculas e Sólidos:
Simulação Computacional**. Livraria da Física.
- Levine, I. N. (2013). **Quantum Chemistry**. International Edition.
- McQuarrie, D. A. (2008). **Quantum chemistry**. University Science Books.
- Teixeira-Dias, J. J. (2017). **Molecular Physical Chemistry**. Springer International Publishing AG.
- Jensen, F. (2017). **Introduction to computational chemistry**. John wiley & sons.
- Cramer, C. J. (2013). **Essentials of computational chemistry: theories and models.** John Wiley & Sons.
- Martin, R. M. (2020). **Electronic structure: basic theory and practical methods.** Cambridge university press.

## Notas de aula e códigos

- Notas de aula: pasta [``lectures``](lectures/)
- Programas e Notebooks: pasta  [``codes``](codes/)

## Programação

1. Fundamentos da Mecânica Quântica
    - Experimentos fundadores
    - Postulados da Mec. Quântica
    - Auto-valores, auto-vetores, mudança de base, comutadores
    - Operadores: posição, momento linear, momento angular e spin
    - Eq. De Schroedinger
    - Aplicações: Poço Quadrado, Tunelamento, Rotor rígido, Oscilador harmônico
    - Átomo de Hidrogênio

2. Estrutura Eletrônica de Átomos e Moléculas
    - Teorema Variacional: aplicação no Átomo de He
    - Aproximação de Born-Oppenheimer: aplicação na Molécula de H2
    - Determinante de Slater
    - Método de Hartree-Fock
    - Conjunto de Base
    - Equações de Roothan e método auto-consistente
    - Discussão de métodos pós-HF e semi-empíricos
    - Cálculos computacionais usando ASE e PySCF
    - Densidade eletrônica, momento de dipolo elétrico e polarizabilidade
    - Interação da radiação com moléculas
    - Espectro micro-ondas e a rotação molecular
    - Espectro IR e Raman e a vibração molecular
    - Calor específico de gases
    - Ressonância magnética nuclear e de spin eletrônico

3. Teoria do Funcional da Densidade
    - Modelo de Thomas-Fermi
    - Teoremas de Kohn - Hohenberg
    - Cálculo Variacional
    - Orbitais Kohn-Sham
    - Funcionais de troca e correlação
    - Discussão sobre TDDFT
    - Cálculos computacionais usando Quantum Espresso e GPAW

4. Estrutura Eletrônica de Sólidos
    - Redes Cristalinas
    - Rede Recíproca: 1ª zona de Brillouin, Índices de Miller
    - Difração de raios-X por cristais
    - Teorema de Bloch
    - Densidade de Estados
    - Estrutura de Bandas
    - Discussão de métodos avançados: tight binding, projetor de onda aumentada, pseudopotenciais
    - Propriedades óticas de sólidos
    - Vibrações da rede e fônons
    - Espectro Raman de sólidos
    - Calor específico de sólidos
    - Cálculos computacionais usando Quantum Espresso e GPAW

5. Aplicações Computacionais na Engenharia Química
    - Reação química
    - Adsorção e Catálise
    - Cálculo de Espectros
    - Dinâmica Molecular Car-Parrinello