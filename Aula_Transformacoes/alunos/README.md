# Aula de Transformações Geométricas: Python e OpenGL
Prof. Rodrigo Fernandes dos Santos. Computação Gráfica, Unipac Barbacena.

## Ambiente
Python 3.11 a 3.13, Pygame 2.6.1, PyOpenGL 3.1.10 e NumPy 1.26 ou 2.x.
A placa de vídeo precisa oferecer OpenGL 3.3 core.

## Windows (PowerShell)
Abra o terminal na pasta `alunos`.
```powershell
py -3 -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe demo1_matrizes.py
.venv\Scripts\python.exe demo2_transformacoes.py
.venv\Scripts\python.exe demo3_global_local.py
```

## macOS (Terminal)
Abra o terminal na pasta `alunos`.
```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python demo1_matrizes.py
.venv/bin/python demo2_transformacoes.py
.venv/bin/python demo3_global_local.py
```

No PyCharm, abra a pasta `alunos` como projeto e execute cada arquivo com **Run**.

ESC fecha a janela. P salva a captura em `capturas/resultado.png`.

## Demonstrações
| Arquivo | O que mostra | Teclas |
|---|---|---|
| `demo1_matrizes.py` | Matrizes 4x4 aplicadas a um ponto, no terminal, sem janela | — |
| `demo2_transformacoes.py` | Translação, rotação, escala e a diferença entre `T @ R` e `R @ T` | 1 a 6 |
| `demo3_global_local.py` | Transformações globais e locais, perspectiva e ortográfica | WASD ZX QE (global), IJKL UO (local), V (projeção), R (reinicia) |

## Organização
- `core/`: `Base`, `Input`, `Utils` (shaders), `Attribute`, `Uniform` (com `mat4`) e `Matrix`.
- `formas.py`: seta, quadrado e eixos em coordenadas locais.
- `objeto.py`: um VAO por objeto, desenhado com `desenhar()`.
- `shaders.py`: `gl_Position = projectionMatrix * modelMatrix * vec4(position, 1.0)`.
- `atividade_sistema_solar.py`: arquivo inicial da atividade (veja `ATIVIDADE.md`).

## Convenções
- As matrizes do NumPy estão por linhas. `Uniform` envia `mat4` com `GL_TRUE` (transpor), pois o GLSL guarda as matrizes por colunas.
- Em `A @ B @ ponto`, a matriz mais à direita é aplicada primeiro.
- Transformação **global** (eixos do mundo): `m @ modelo`. Transformação **local** (eixos do objeto): `modelo @ m`.
- Velocidades em unidades ou radianos **por segundo**, multiplicadas por `self.deltaTime`.

## Problemas comuns
- Tela preta com perspectiva: o objeto está em z = 0. A câmera olha para -z e o plano *near* é 0.1. Translade para z = -1.
- Objeto distorcido ou sumido: a matriz foi enviada sem transpor, ou o tipo da `Uniform` não é `mat4`.
- O objeto gira "em volta do mundo" em vez de girar no lugar: a ordem está trocada (`R @ T` em vez de `T @ R`).
- `ModuleNotFoundError: core`: execute a partir da pasta `alunos`.

## Referência
STEMKOSKI, Lee; PASCALE, Michael. *Developing graphics frameworks with Python and OpenGL*. Boca Raton: CRC Press, 2022. Capítulo 3 (Matrix Algebra and Transformations).



## Atividade 1 – Mini Sistema Solar

**Disciplina:** Computação Gráfica

**Atividade:** Mini Sistema Solar – Transformações Geométricas

**Integrante 1:** Carlos Eduardo Santos Almeida

**Integrante 2:** Lucas de Souza Carmo

### Descrição da implementação

A atividade consiste no desenvolvimento de um Mini Sistema Solar utilizando Python e OpenGL, aplicando conceitos de transformações geométricas.

Foram implementados os movimentos de rotação do Sol em torno do próprio centro, a órbita de um planeta ao redor do Sol com raio de 0.6 e sua rotação em torno do próprio eixo. Também foi implementada uma Lua que orbita o planeta com raio de 0.2, acompanhando seu deslocamento.

Para realizar essas transformações, foram utilizadas as funções `make_translation`, `make_rotation_z` e `make_scale`, combinadas pelo operador `@`.

O programa permite aumentar e diminuir a velocidade orbital do planeta utilizando as setas direcionais do teclado, impedindo que a velocidade assuma valores negativos.

### Controles

- **Seta ↑:** aumenta a velocidade orbital.
- **Seta ↓:** diminui a velocidade orbital até o limite mínimo de zero.
- **Tecla P:** salva uma captura de tela do resultado.

### Resposta da questão conceitual

**Por que a órbita utiliza `R @ T` e a rotação própria do planeta fica à direita da translação?**

A órbita utiliza `R @ T` porque a translação é aplicada primeiro, afastando o planeta do centro do Sol. Em seguida, a rotação movimenta essa posição ao redor da origem, formando uma trajetória orbital.

A rotação própria do planeta fica à direita da translação porque as matrizes são aplicadas da direita para a esquerda. Assim, o planeta gira em torno do próprio eixo antes de ser deslocado para sua posição orbital. Essa ordem permite combinar os dois movimentos sem alterar a distância entre o planeta e o Sol.

### Resultado da execução

O programa foi executado e os movimentos do Sol, do planeta e da Lua foram verificados. Os controles de velocidade também foram testados, confirmando o funcionamento das funcionalidades implementadas.

![Resultado do Mini Sistema Solar](capturas/resultado.png)
