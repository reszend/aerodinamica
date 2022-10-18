# Forças Fundamentais

**Definição de força aerodinâmica: Força causada pelo fluxo de um fuido ao redor de um sólido.**

As forças fundamentais que atuam em um vôo são as forças de sustentação, arrasto, empuxo e peso, das quais as forças de sustentção e arrasto são forças aerodinâmicas (dependentes da viscosidade e pressão).

# Forças em um vôo

## Força Peso:

$$
W = mg
$$

É a força atuante sobre um corpo devido a presença de um campo gravitacional.

## Força de Empuxo

Resultante da aceleração ou do ato de expelir massa em uma direção, causando uma força de mesmo módulo porém na direção contrária à massa acelerada.

$$
T = \frac{dm}{dt}v
$$

onde **dm** é um diferencial de massa; **dt** é um diferencial de tempo e **v** é a velocidade da massa expelida.

# Força Aerodinâmica

A força aerodinâmica é a resultante de duas componentes, o arrasto e a sustentação:

## Arrasto

É a resistência contra o movimento que um fluido exerce sobre um objeto imenso nele, a força de arrasto é proporcional a velocidade para baixas velocidades, e ao quadrado da velocidade para altas velocidades e a definição sobre o que é rápido e o que é lento é dado pelo o número de Reynold.

**curiosidade:** apesar de majoritariamente a causa do arrasto ser o atrito devido a viscosidade do fluido, o arrasto turbulento independe da viscosidade.

#### A Equação Do Arrasto

$$
F_d = \frac{pv^2}{2}C_dA
$$

Onde **p** é a densidade do fluído, **v** é a velocidade relativa ao fluído, **A** é a superfície de contato normal à força e **$C_d$** é o coeficiente de arrasto, dependente do número de Reynolds e do formato do objeto, definido por:

$$
C_d = 2\frac{A_w}{A_f}\frac{Be}{{{R_e}_L}^2}
$$

$$
R_e = \frac{pvD}{\mu}
$$

$A_w$ = Wet Area.

$A_f$ = Front Area.

$B_e$ = Bejan Number.

$\mu$ = Viscosidade Dinâmica.

$$
Be = \frac{{\Delta}pL^2}{{\mu}V}
$$

$V$ = Difusão de Momento.

### Difusão de Momento (Viscosidade cinemática)

Propagação do momento entre partículas.

É um transporte de quantidade de movimento pode ocorrer em qualquer direção do fluxo do fluido. A difusão do momento pode ser atribuída à pressão externa ou à tensão de cisalhamento ou a ambos.

Quando a pressão é aplicada em um fluido incompressível, a velocidade do fluido mudará. O fluido acelera ou desacelera dependendo da direção relativa da pressão em relação à direção do fluxo. Isso ocorre porque a aplicação de pressão no fluido causou difusão de momento nessa direção.



$$
V = C*t

$$

viscometro
[Air - Dynamic and Kinematic Viscosity](https://www.engineeringtoolbox.com/air-absolute-kinematic-viscosity-d_601.html)

#### Difusão de momento devido a tensão de cesilhamento:

$$
\tau = -\mu du/dy
$$

$du/dy$ = Gradiente de velocidade em relação ao fluxo.



<img title="" src="file:///home/user/Downloads/14ilf1l.svg.png" alt="" width="180" data-align="center">







<img src="file:///home/user/Downloads/Inclinedthrow.gif" title="" alt="" data-align="center">



```python
#!/usr/bin/env/ python3

"""
Lançamento de projéteis em duas dimensões usando o método de euler
"""
import time
import numpy as np
import matplotlib.pyplot as plt


M = float(input("Entre com a massa: "))
B = float(input("Entre com o arrasto: "))
DT = float(input("Entre com o diferencial de tempo: "))
SPEED = float(input("Entre com a velocidade inicial: "))


def launchwoar(G, DT, SPEED, THET, PSI):
    """Lançamento sem a resistência do ar"""
    # Variáveis
    velocity_x = SPEED * np.cos(PSI)*np.sin(THET)
    velocity_y = SPEED * np.sin(PSI)*np.sin(THET)
    velocity_z = SPEED * np.cos(PSI)

    # Cria as arrays necessárias
    x_axis = np.array([])
    y_axis = np.array([])
    z_axis = np.array([])
    vx_axis = np.array([])
    vy_axis = np.array([])
    vz_axis = np.array([])

    # Define o valor inicial
    vx_axis = np.append(vx_axis, velocity_x)
    vy_axis = np.append(vy_axis, velocity_y)
    vz_axis = np.append(vz_axis, velocity_z)
    x_axis = np.append(x_axis, 0)
    y_axis = np.append(y_axis, 0)
    z_axis = np.append(z_axis, 0)

    rev = 0

    while z_axis[rev] >= 0:
        x_axis = np.append(x_axis, x_axis[rev] + (vx_axis[rev] * DT))
        vx_axis = np.append(vx_axis, vx_axis[rev])

        y_axis = np.append(y_axis, y_axis[rev] + (vy_axis[rev] * DT))
        vy_axis = np.append(vy_axis, vy_axis[rev])

        z_axis = np.append(z_axis, z_axis[rev] + (vz_axis[rev] * DT))
        vz_axis = np.append(vz_axis, vz_axis[rev] - (G * DT))

        rev += 1
    return x_axis, y_axis, z_axis

def launchwog(G, M, B, DT, SPEED, THET, PSI):
    """Lançamento com a resistência do ar"""
    # Variáveis
    velocity_x = SPEED * np.cos(PSI)*np.sin(THET)
    velocity_y = SPEED * np.sin(PSI)*np.sin(THET)
    velocity_z = SPEED * np.cos(PSI)

    # Cria as arrays necessárias
    x_axis = np.array([])
    y_axis = np.array([])
    z_axis = np.array([])
    vx_axis = np.array([])
    vy_axis = np.array([])
    vz_axis = np.array([])

    # Define o valor inicial
    vx_axis = np.append(vx_axis, velocity_x)
    vy_axis = np.append(vy_axis, velocity_y)
    vz_axis = np.append(vz_axis, velocity_z)
    x_axis = np.append(x_axis, 0)
    y_axis = np.append(y_axis, 0)
    z_axis = np.append(z_axis, 0)

    rev = 0

    while z_axis[rev] >= 0:
        v = np.sqrt((vx_axis)**2 + (vy_axis)**2 + (vz_axis)**2)

        x_axis = np.append(x_axis, (x_axis[rev] + (vx_axis[rev] * DT)))
        vx_axis = np.append(vx_axis, (vx_axis[rev] - (B*vx_axis[rev]*vx_axis[rev])/M * DT))

        y_axis = np.append(y_axis, (y_axis[rev] + (vy_axis[rev] * DT)))
        vy_axis = np.append(vy_axis, (vy_axis[rev] - (B*vy_axis[rev]*vy_axis[rev])/M * DT))

        z_axis = np.append(z_axis, (z_axis[rev] + (vz_axis[rev] * DT)))
        vz_axis = np.append(vz_axis, (vz_axis[rev] - (G * DT)  - (B*vz_axis[rev]*vz_axis[rev])/M * DT))

        rev += 1
    return x_axis, y_axis, z_axis


def launch(M, B, DT, SPEED, THET, PSI):
    """Lançamento com a resistência do ar e campo gravitacional variável"""
    # Variáveis
    velocity_x = SPEED * np.cos(PSI)*np.sin(THET)
    velocity_y = SPEED * np.sin(PSI)*np.sin(THET)
    velocity_z = SPEED * np.cos(PSI)
    gravity = 9.81

    # Cria as arrays necessárias
    x_axis = np.array([])
    y_axis = np.array([])
    z_axis = np.array([])
    vx_axis = np.array([])
    vy_axis = np.array([])
    vz_axis = np.array([])

    # Define o valor inicial
    vx_axis = np.append(vx_axis, velocity_x)
    vy_axis = np.append(vy_axis, velocity_y)
    vz_axis = np.append(vz_axis, velocity_z)
    x_axis = np.append(x_axis, 0)
    y_axis = np.append(y_axis, 0)
    z_axis = np.append(z_axis, 0)

    rev = 0

    while z_axis[rev] >= 0:
        v = np.sqrt((vx_axis)**2 + (vy_axis)**2 + (vz_axis)**2)

        x_axis = np.append(x_axis, (x_axis[rev] + (vx_axis[rev] * DT)))
        vx_axis = np.append(vx_axis, (vx_axis[rev] - (B*vx_axis[rev]*vx_axis[rev])/M * DT))

        y_axis = np.append(y_axis, (y_axis[rev] + (vy_axis[rev] * DT)))
        vy_axis = np.append(vy_axis, (vy_axis[rev] - (B*vy_axis[rev]*vy_axis[rev])/M * DT))

        z_axis = np.append(z_axis, (z_axis[rev] + (vz_axis[rev] * DT)))
        vz_axis = np.append(vz_axis, (vz_axis[rev] - (gravity * DT)  - (B*vz_axis[rev]*vz_axis[rev])/M * DT))

        gravity = (3.98589196e14) / (6371000 + z_axis[rev])**2

        rev += 1
    return x_axis, y_axis, z_axis

plt.ion()
fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

i = 0
varp = np.linspace(-np.pi,np.pi,100)
vart = np.linspace(0,2*np.pi,100)
while i != 100:
    a, b, c = launch(M, B, DT, SPEED, np.pi, vart[i])
    ax.plot(a, b, c)
    fig.canvas.draw()
    plt.pause(0.05)
#    ax.clear()
    i += 1
    if i == 99:
        plt.pause(10)
```



### Alternativamente:

$$
C_D = \frac{2}{pv^2A}\int_sdS(p-p_0)(n*i)+\frac{2}{pv^2A}\int_sdS(t*i)T_w
$$

$T_w$ = sheer stress module

[Modeling and Simulation in Python &#8212; Modeling and Simulation in Python](https://allendowney.github.io/ModSimPy/)

[GitHub - AllenDowney/ModSimPy: Text and supporting code for Modeling and Simulation in Python](https://github.com/AllenDowney/ModSimPy)



## Força de Sustentação:

É a componente da resultante aerodinâmica perpendicular ao vento relativo:



$$
L = C_l\frac{p}{2}SV^2
$$

$p$ = densidade do ar.

$V$ = velocidade relativa.

$S$ = area na direção da força

$C_l$ = coeficiente de sustentação = dependente do perfil, angulo de ataque, etc.



## TAT (Teoria do perfil fino)

$$
cl = cl_0 + 2\pi\alpha
$$

https://ntrs.nasa.gov/api/citations/19840018592/downloads/19840018592.pdf

# Tipos de escoamento

Os tipos de fluxo (escoamento) são definidos a partir de quatro principais fatores

1. Velocidade

2. Viscosidade

3. Temperatura

4. Densidade

os principais tipos de escoamento são, escoamento subsônico, transônico, supersônico, e hypersônico

# Camada de limite

a camada limite é a camada de fluido nas imediações de uma superfície delimitadora, fazendo-se sentir os efeitos difusivos e a dissipativos.
