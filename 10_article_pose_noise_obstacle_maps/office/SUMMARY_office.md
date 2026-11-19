# Experimento B com obstaculos -- mapa office

Referencia = mapas finais dos episodios com pose real (nominal, drop30; 20 episodios; cada um deles e avaliado contra os demais). A uniao da referencia cobre 95.9% da grade (a grade inclui 0,5 m fora da cerca e o interior dos obstaculos, que nunca sao vistos).

Todas as coberturas sao medidas no mapa do instante em que o episodio parou (o episodio termina pela cobertura ESTIMADA, como faria um drone real).

| Condicao | n | Termino por cobertura | Timeout | Colisoes | Duracao (s) | Cobertura estimada (%) | Cobertura correta (%) | Parede falsa (m2) | Livre falso (m2) | Nao avaliavel (%) |
|---|---|---|---|---|---|---|---|---|---|---|
| Sem ruido | 10 | 8/10 | 2/10 | 0/10 | 101.4 ± 15.4 | 81.9 ± 6.4 | 81.4 ± 6.4 | 0.6 ± 0.3 | 2.8 ± 0.5 | 0.0 ± 0.0 |
| Pose 0,05 m / 1 grau | 0 | (nao rodou) |||||||
| Pose 0,10 m / 2 graus | 10 | 0/10 | 10/10 | 0/10 | 120.0 ± 0.0 | 52.1 ± 7.2 | 44.7 ± 6.4 | 38.6 ± 6.0 | 2.0 ± 0.6 | 1.2 ± 0.3 |
| Pose 0,20 m / 4 graus | 10 | 0/10 | 10/10 | 0/10 | 120.0 ± 0.0 | 45.3 ± 11.0 | 33.9 ± 9.0 | 63.0 ± 11.9 | 2.3 ± 0.9 | 1.3 ± 0.6 |
| LiDAR 30% perdido | 10 | 6/10 | 4/10 | 0/10 | 107.1 ± 13.1 | 83.4 ± 2.6 | 82.8 ± 2.6 | 0.6 ± 0.2 | 3.2 ± 0.7 | 0.0 ± 0.0 |
| So posicao 0,10 m (guinada 0) | 0 | (nao rodou) |||||||
| So guinada 2 graus (posicao 0) | 10 | 2/10 | 8/10 | 0/10 | 112.8 ± 14.5 | 77.8 ± 6.3 | 73.3 ± 6.2 | 24.8 ± 2.9 | 2.0 ± 0.8 | 0.4 ± 0.1 |
| Correlacionado 1a ordem, tau 2 s: 0,10 m / 2 graus | 0 | (nao rodou) |||||||
| Correlacionado 2a ordem, tau 2 s: 0,10 m / 2 graus | 10 | 5/10 | 5/10 | 0/10 | 106.3 ± 20.2 | 77.3 ± 10.3 | 74.0 ± 10.1 | 15.5 ± 3.3 | 4.2 ± 1.7 | 0.3 ± 0.1 |
| Correlacionado 2a ordem, tau 2 s: 0,20 m / 4 graus | 0 | (nao rodou) |||||||

Media ± desvio padrao populacional (ddof=0) entre episodios. Mapa final = ultima exportacao do mapa 2D (<= 0,5 s antes do fim).
Por episodio: PER_EPISODE_office.csv
