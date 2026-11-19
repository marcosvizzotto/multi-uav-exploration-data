# Experimento B com obstaculos -- mapa cubes

Referencia = mapas finais dos episodios com pose real (nominal, drop30; 20 episodios; cada um deles e avaliado contra os demais). A uniao da referencia cobre 89.8% da grade (a grade inclui 0,5 m fora da cerca e o interior dos obstaculos, que nunca sao vistos).

Todas as coberturas sao medidas no mapa do instante em que o episodio parou (o episodio termina pela cobertura ESTIMADA, como faria um drone real).

| Condicao | n | Termino por cobertura | Timeout | Colisoes | Duracao (s) | Cobertura estimada (%) | Cobertura correta (%) | Parede falsa (m2) | Livre falso (m2) | Nao avaliavel (%) |
|---|---|---|---|---|---|---|---|---|---|---|
| Sem ruido | 10 | 10/10 | 0/10 | 0/10 | 72.5 ± 7.8 | 84.8 ± 0.2 | 84.3 ± 0.3 | 0.1 ± 0.1 | 2.7 ± 1.2 | 0.1 ± 0.1 |
| Pose 0,05 m / 1 grau | 10 | 0/10 | 10/10 | 0/10 | 90.0 ± 0.0 | 65.1 ± 12.5 | 59.9 ± 11.2 | 24.9 ± 6.2 | 2.8 ± 1.1 | 1.0 ± 0.2 |
| Pose 0,10 m / 2 graus | 10 | 3/10 | 7/10 | 0/10 | 88.5 ± 3.1 | 63.8 ± 21.5 | 55.2 ± 18.4 | 39.0 ± 15.1 | 2.9 ± 1.4 | 2.1 ± 0.8 |
| Pose 0,20 m / 4 graus | 10 | 3/10 | 7/10 | 0/10 | 86.2 ± 6.6 | 77.5 ± 7.3 | 59.7 ± 5.6 | 80.2 ± 8.6 | 3.8 ± 0.6 | 4.9 ± 0.4 |
| LiDAR 30% perdido | 10 | 10/10 | 0/10 | 0/10 | 71.1 ± 5.6 | 84.8 ± 0.2 | 84.3 ± 0.3 | 0.2 ± 0.1 | 2.3 ± 0.9 | 0.1 ± 0.1 |
| So posicao 0,10 m (guinada 0) | 10 | 2/10 | 8/10 | 0/10 | 88.4 ± 3.2 | 68.2 ± 17.8 | 59.7 ± 15.5 | 39.1 ± 10.0 | 2.8 ± 1.5 | 2.1 ± 0.6 |
| So guinada 2 graus (posicao 0) | 10 | 10/10 | 0/10 | 0/10 | 67.5 ± 10.3 | 84.8 ± 0.2 | 78.3 ± 0.6 | 32.9 ± 3.7 | 2.5 ± 0.9 | 1.0 ± 0.1 |
| Correlacionado 1a ordem, tau 2 s: 0,10 m / 2 graus | 0 | (nao rodou) |||||||
| Correlacionado 2a ordem, tau 2 s: 0,10 m / 2 graus | 10 | 9/10 | 1/10 | 0/10 | 73.4 ± 9.1 | 84.8 ± 0.2 | 80.4 ± 0.6 | 19.3 ± 2.2 | 4.5 ± 1.0 | 0.8 ± 0.1 |
| Correlacionado 2a ordem, tau 2 s: 0,20 m / 4 graus | 10 | 10/10 | 0/10 | 0/10 | 63.6 ± 6.9 | 84.7 ± 0.2 | 77.2 ± 0.5 | 32.5 ± 2.8 | 6.7 ± 1.3 | 1.5 ± 0.3 |

Media ± desvio padrao populacional (ddof=0) entre episodios. Mapa final = ultima exportacao do mapa 2D (<= 0,5 s antes do fim).
Por episodio: PER_EPISODE_cubes.csv
