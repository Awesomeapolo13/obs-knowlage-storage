
# Высокоуровневая архитектура

**Ключи:**
- Обратные:
	- [Elasticsearch](elk-search);
- Прямые:

**Хештеги:** 
- #Programming/Databases/Elasticsearch
- #ElasticStack/Elasticsearch

## Описание

Кластер elasticsearch может иметь один или более узлов (*node*). Узел - это физический экземпляр, на котором размещен elasticsearch. Узел может иметь один или более elasticsearch шардов. Elasticsearch шарды - базовые индексы Lucene, которые содержат множество неизменяемых мини-индексов, называемых сегментами. Сегменты индекса сожержит все структуры данных, такие как хранимые поля (stored fields), обратные индексы (inverted index), документы-значения (doc-values) и пр.

![[Pasted image 20240326143641.png]]

![[Pasted image 20240326143712.png]]

![[Pasted image 20240326143728.png]]

