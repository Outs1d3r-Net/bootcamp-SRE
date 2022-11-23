# Procedimentos

---

- [ ]  Instalação do Logstash
- [ ]  Instalação dos componentes(Elastic Stack)
- [ ]  Monitoramento de logs com Filebeat em uma instância com Nginx.
- [ ]  Métricas com Filebeat/Metricbeat em uma instância Nginx
- [ ]  Analizando as metricas no Kibana

[Welcome to Elastic Docs](https://www.elastic.co/guide/index.html)

## Grok Debugger

---

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled.png)

[Grok Debugger](https://grokdebug.herokuapp.com/)

## Elasticsearch

---

## Arquivo jvm.options

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%201.png)

## Testes

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%202.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%203.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%204.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%205.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%206.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%207.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%208.png)

## Kibana

---

### Arquivo /etc/kibana/kibana.yml

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%209.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2010.png)

## **Metricbeat**

---

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2011.png)

### Arquivo /etc/metricbeat/metricbeat.yml

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2012.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2013.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2014.png)

### Arquivo  /etc/metricbeat/system.yml

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2015.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2016.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2017.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2018.png)

## **Filebeat**

---

### Arquivo /etc/filebeat/filebeat.yml

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2019.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2020.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2021.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2022.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2023.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2024.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2025.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2026.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2027.png)

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2028.png)

## Para Desafio final, Deve-se configurar o  /etc/filebeat/filebeat.yml

---

**OBS:** Somente habilitar  output.logstash e desabilitar o output.elasticsearch. Depois disso, configurar o arquivo pipeline.conf.

![Untitled](Procedimentos%20496d5b67a036489a9b978e52b77a64da/Untitled%2014.png)