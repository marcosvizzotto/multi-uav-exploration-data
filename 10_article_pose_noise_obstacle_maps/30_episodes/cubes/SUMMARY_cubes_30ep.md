# Mapa cubes: 30 episodios por condicao (10 com seed 42 + 20 com seed 43)

Referencia = mapas finais dos episodios com pose real (nominal, drop10, drop30; 90 episodios, cada um avaliado contra os demais). A uniao da referencia cobre 91.8% da grade.

Taxas: % [intervalo de confianca de 95%, Wilson]. Coberturas e parede falsa: media ± desvio padrao populacional (ddof=0). Tudo medido no mapa do instante em que o episodio parou.

| Condicao | n | Termino por cobertura (%) | Timeout (%) | Colisao (%) | Duracao (s) | Cobertura estimada (%) | Cobertura correta (%) | Parede falsa (m2) | Livre falso (m2) |
|---|---|---|---|---|---|---|---|---|---|
| Sem ruido | 30 | 100 [89; 100] | 0 [0; 11] | 0 [0; 11] | 71.8 ± 7.6 | 84.8 ± 0.1 | 84.4 ± 0.2 | 0.1 ± 0.1 | 2.7 ± 1.1 |
| LiDAR 10% perdido | 30 | 93 [79; 98] | 7 [2; 21] | 0 [0; 11] | 73.9 ± 9.0 | 84.7 ± 0.4 | 84.3 ± 0.4 | 0.1 ± 0.1 | 2.5 ± 0.7 |
| LiDAR 30% perdido | 30 | 100 [89; 100] | 0 [0; 11] | 0 [0; 11] | 73.1 ± 7.2 | 84.8 ± 0.2 | 84.5 ± 0.2 | 0.2 ± 0.1 | 2.2 ± 0.8 |
| Pose correlacionada 0,10 m / 2 graus (2a ordem, tau 2 s) | 30 | 97 [83; 99] | 3 [1; 17] | 0 [0; 11] | 71.3 ± 10.1 | 84.8 ± 0.2 | 80.5 ± 0.5 | 22.1 ± 3.0 | 4.6 ± 1.4 |

## Por seed (consistencia entre os dois blocos)

| Condicao | Seed | n | Termino por cobertura | Cobertura correta (%) | Parede falsa (m2) |
|---|---|---|---|---|---|
| Sem ruido | 42 | 10 | 10/10 | 84.4 ± 0.2 | 0.1 ± 0.1 |
| Sem ruido | 43 | 20 | 20/20 | 84.4 ± 0.2 | 0.1 ± 0.1 |
| LiDAR 10% perdido | 42 | 10 | 10/10 | 84.4 ± 0.1 | 0.2 ± 0.2 |
| LiDAR 10% perdido | 43 | 20 | 18/20 | 84.3 ± 0.4 | 0.1 ± 0.1 |
| LiDAR 30% perdido | 42 | 10 | 10/10 | 84.4 ± 0.3 | 0.2 ± 0.1 |
| LiDAR 30% perdido | 43 | 20 | 20/20 | 84.5 ± 0.1 | 0.1 ± 0.1 |
| Pose correlacionada 0,10 m / 2 graus (2a ordem, tau 2 s) | 42 | 10 | 9/10 | 80.5 ± 0.6 | 22.8 ± 2.9 |
| Pose correlacionada 0,10 m / 2 graus (2a ordem, tau 2 s) | 43 | 20 | 20/20 | 80.6 ± 0.5 | 21.7 ± 3.0 |

## Posicoes iniciais (seed 42 x seed 43)

Com posicao inicial registrada: 10 episodios de seed 42 e 80 de seed 43. Pares de posicoes iniciais distintos (arredondados a 1 m): seed 42 = 10, seed 43 = 78; episodios de seed 43 cujo par tambem aparece em seed 42: 0 de 80.
LiDAR 10%, episodio a episodio (ep i de seed 42 contra ep i de seed 43, i = 1..10): 0 com as mesmas posicoes iniciais (tolerancia 5 cm).

Por episodio: PER_EPISODE_cubes_30ep.csv
