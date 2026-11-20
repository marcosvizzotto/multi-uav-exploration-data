# Mapa office: 30 episodios por condicao (10 com seed 42 + 20 com seed 43)

Referencia = mapas finais dos episodios com pose real (nominal, drop10, drop30; 90 episodios, cada um avaliado contra os demais). A uniao da referencia cobre 96.7% da grade.

Taxas: % [intervalo de confianca de 95%, Wilson]. Coberturas e parede falsa: media ± desvio padrao populacional (ddof=0). Tudo medido no mapa do instante em que o episodio parou.

| Condicao | n | Termino por cobertura (%) | Timeout (%) | Colisao (%) | Duracao (s) | Cobertura estimada (%) | Cobertura correta (%) | Parede falsa (m2) | Livre falso (m2) |
|---|---|---|---|---|---|---|---|---|---|
| Sem ruido | 30 | 70 [52; 83] | 30 [17; 48] | 0 [0; 11] | 99.6 ± 18.7 | 82.5 ± 4.7 | 82.0 ± 4.7 | 0.4 ± 0.2 | 3.0 ± 0.7 |
| LiDAR 10% perdido | 30 | 60 [42; 75] | 40 [25; 58] | 0 [0; 11] | 103.2 ± 17.0 | 81.3 ± 7.4 | 80.7 ± 7.3 | 0.4 ± 0.2 | 3.0 ± 0.8 |
| LiDAR 30% perdido | 30 | 73 [56; 86] | 27 [14; 44] | 0 [0; 11] | 102.3 ± 16.4 | 83.8 ± 2.7 | 83.2 ± 2.7 | 0.4 ± 0.2 | 3.3 ± 0.9 |
| Pose correlacionada 0,10 m / 2 graus (2a ordem, tau 2 s) | 30 | 57 [39; 73] | 43 [27; 61] | 0 [0; 11] | 102.8 ± 19.7 | 80.0 ± 7.9 | 76.8 ± 7.8 | 16.5 ± 3.1 | 4.2 ± 1.7 |

## Por seed (consistencia entre os dois blocos)

| Condicao | Seed | n | Termino por cobertura | Cobertura correta (%) | Parede falsa (m2) |
|---|---|---|---|---|---|
| Sem ruido | 42 | 10 | 8/10 | 81.4 ± 6.4 | 0.4 ± 0.2 |
| Sem ruido | 43 | 20 | 13/20 | 82.3 ± 3.5 | 0.4 ± 0.2 |
| LiDAR 10% perdido | 42 | 10 | 6/10 | 80.1 ± 9.6 | 0.4 ± 0.2 |
| LiDAR 10% perdido | 43 | 20 | 12/20 | 81.1 ± 5.8 | 0.4 ± 0.2 |
| LiDAR 30% perdido | 42 | 10 | 6/10 | 82.9 ± 2.6 | 0.4 ± 0.1 |
| LiDAR 30% perdido | 43 | 20 | 16/20 | 83.4 ± 2.7 | 0.4 ± 0.2 |
| Pose correlacionada 0,10 m / 2 graus (2a ordem, tau 2 s) | 42 | 10 | 5/10 | 74.0 ± 10.1 | 16.4 ± 3.2 |
| Pose correlacionada 0,10 m / 2 graus (2a ordem, tau 2 s) | 43 | 20 | 12/20 | 78.2 ± 6.0 | 16.5 ± 3.1 |

## Posicoes iniciais (seed 42 x seed 43)

Com posicao inicial registrada: 10 episodios de seed 42 e 80 de seed 43. Pares de posicoes iniciais distintos (arredondados a 1 m): seed 42 = 10, seed 43 = 75; episodios de seed 43 cujo par tambem aparece em seed 42: 0 de 80.
LiDAR 10%, episodio a episodio (ep i de seed 42 contra ep i de seed 43, i = 1..10): 0 com as mesmas posicoes iniciais (tolerancia 5 cm).

Por episodio: PER_EPISODE_office_30ep.csv
