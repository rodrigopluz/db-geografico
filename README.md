# Descrição projeto-final - DB-GEOGRAFICO

O trabalho apresentado será mostrado a opção 4 do objetivo do projeto final da disciplina BD Geográfico, e para isso foi escolhido 2 municipios de diferentes estados, um do RS e outro de SC. Na tabela dos municipios está sendo retornado somente os 2 municipios escolhidos, e na tabela de ruas, está somente com dados de ruas desse mesmos 2 municipios.

O objetivo era criar tabelas referentes:

- **Estádios de Futebol**
- **Ginásios de Espostes**
- **Academias**.

Com isso o Diagrama ER de banco de dados (projeto_final) abaixo.

<img src="/home/dev-rodrigo/Documentos/DB-GEOGRAFICO/projeto-final/diagrama-er.png" width="1024" height="768" />

E as vistas que foram criadas a partir do resultado das consultas, tanto para as _não-espaciais_ como para as _espaciais_ estão, nomeadas pelas siglas - **NE.Numero** (_NãoEspaciais.Numero_) e **E.Numero** (_Espaciais.Numero_).

As consultas criadas estão listadas a baixo:

Consultas não-espaciais:

```zsh
$ SELECT a.*,m.nm_mun,m.sigla,m.area_km2
  FROM academias as a
  JOIN municipios as m ON a.cd_mun = m.cd_mun WHERE m.sigla IN ('SC');
```

```zsh
$ SELECT g.*,m.nm_mun,m.sigla,m.area_km2
  FROM ginasios_esportes as g
  JOIN municipios as m ON g.cd_mun = m.cd_mun WHERE m.sigla IN ('SC');
```

```zsh
$ SELECT e.*,m.nm_mun,m.sigla,m.area_km2
  FROM estadio_futebol as e
  JOIN municipios as m ON e.cd_mun = m.cd_mun WHERE m.sigla IN ('RS');
```

```zsh
$ SELECT e.*,m.nm_mun,m.sigla,m.area_km2
  FROM estadio_futebol as e
  JOIN municipios as m ON e.cd_mun = m.cd_mun WHERE m.sigla IN ('SC');
```

```zsh
$ SELECT a.*,m.nm_mun,m.sigla,m.area_km2
  FROM academias as a
  JOIN municipios as m ON a.cd_mun = m.cd_mun WHERE m.sigla IN ('RS');
```

Consultas espaciais:

```zsh
$ SELECT a.*, m.nm_mun, m.sigla
  FROM academias as a, municipios as m
  WHERE st_distance(a.geom,m.geom) <= 0.0001 AND m.sigla IN ('SC');
```

```zsh
$ SELECT g.*, m.nm_mun, m.sigla
  FROM ginasios_esportes as g, municipios as m
  WHERE st_distance(g.geom,m.geom) <= 0.0001 AND m.sigla IN ('RS');
```

```zsh
$ SELECT e.*, m.nm_mun, m.sigla
  FROM estadio_futebol as e, municipios as m
  WHERE st_distance(e.geom,m.geom) <= 0.0001 AND m.sigla IN ('RS','SC');
```

```zsh
$ SELECT e.*, m.nm_mun, m.sigla
  FROM estadio_futebol as e, municipios as m
  WHERE st_intersects(e.geom,m.geom);
```

```zsh
$ SELECT a.*, m.nm_mun, m.sigla
  FROM academias as a, municipios as m
  WHERE st_intersects(a.geom,m.geom) AND m.nm_mun = 'Itapema';
```
