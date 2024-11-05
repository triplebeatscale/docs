> 720 × {% calc [currency=RUB] 3 × (2 × {{ sku|RUB|mdb.cluster.mysql.v3.cpu.c100|number }} + 8 × {{ sku|RUB|mdb.cluster.mysql.v3.ram|number }}) %} + 100&nbsp;×&nbsp;{{ sku|RUB|mdb.cluster.network-hdd.mysql|month|string }} = {% calc [currency=RUB] 720 × (3 × (2 × {{ sku|RUB|mdb.cluster.mysql.v3.cpu.c100|number }} + 8 × {{ sku|RUB|mdb.cluster.mysql.v3.ram|number }})) + 100 × {{ sku|RUB|mdb.cluster.network-hdd.mysql|month|number }} %}
>
> Итого: {% calc [currency=RUB] 720 × (3 × (2 × {{ sku|RUB|mdb.cluster.mysql.v3.cpu.c100|number }} + 8 × {{ sku|RUB|mdb.cluster.mysql.v3.ram|number }})) + 100 × {{ sku|RUB|mdb.cluster.network-hdd.mysql|month|number }} %} — стоимость использования кластера в течение 30 дней.

Где:
* 720 — количество часов в 30 днях.
* {% calc [currency=RUB] 3 × (2 × {{ sku|RUB|mdb.cluster.mysql.v3.cpu.c100|number }} + 8 × {{ sku|RUB|mdb.cluster.mysql.v3.ram|number }}) %} — стоимость часа работы хостов {{ MY }}.
* 100 — объем хранилища на сетевых HDD-дисках (в гигабайтах).
* {{ sku|RUB|mdb.cluster.network-hdd.mysql|month|string }} — стоимость месяца использования 1 ГБ хранилища на сетевых HDD-дисках.