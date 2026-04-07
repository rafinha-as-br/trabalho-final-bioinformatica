# 🧬 Trabalho Final de Bioinformática

**IFSC – Câmpus Gaspar**
Curso: ADS | Prof. Dr. Renato Simões Moreira

## 👥 Equipe

Rafael • Daniel Marques • Maria Isabel

## 🦠 Organismo

*Escherichia coli* O157:H7 (Gram-negativa)

---

## 📂 Estrutura

```
.
├── README.md
├── relatorio.pdf
├── input.fasta
├── top50.fasta
├── results_fastprotein/
└── results_epibuilder/
```

---

## ⚙️ Ambiente

* CPU: Intel Core i7-1255U
* RAM: 16 GB
* Armazenamento: SSD NVMe 512 GB
* SO: Ubuntu 24.04.4 + Docker

---

## 🧪 Pipeline

```bash
# Download proteoma
curl -s "https://rest.uniprot.org/uniprotkb/stream?format=fasta&query=proteome:UP000000558" -o input.fasta

# FastProtein
docker run --rm -v ${PWD}:/data bioinfoufsc/fastprotein:clean-latest \
fastprotein -i /data/input.fasta -o /data/results_fastprotein

# Top 50 membrana
awk '/^>/{c++} c<=50' results_fastprotein/raw/membranes.fasta > top50.fasta

# EpiBuilder (gram-negativa)
docker run -it --rm \
-v ${PWD}:/data/ \
-v /var/run/docker.sock:/var/run/docker.sock \
-v epibuilder-data:/tmp/epibuilder \
-e EPIBUILDER_VOLUME=epibuilder-data \
bioinfoufsc/epibuilder-core epibuilder \
--input_file /data/top50.fasta \
--loc gram_neg \
--output /data/results_epibuilder
```

---

## 📊 Resultados

**FastProtein**

* Proteínas processadas: **5053**
* Ignoradas: **3**
* Tempo total: **00:08:40:129s**
* Proteínas com domínio transmembranar: **1191**
* Proteínas com evidência de membrana: **1218**

**EpiBuilder**

* Proteínas analisadas: **50**
* Proteínas com epítopos preditos: **15**

---

## 📋 Epítopos (Top 5)

| ID     | Epítopo                    | Início | Fim | N-glico |
| ------ | -------------------------- | ------ | --- | ------- |
| Q8X903 | NPFDQSSQPQQQPQQQPAQQEQKDSD | 805    | 831 | Não     |
| Q7BSW5 | NWGNSTYKQTDWN              | 138    | 151 | Sim     |
| P69743 | ENQTPRSQKPD                | 312    | 323 | Sim     |
| P0AA79 | EAQHQVSTAKK                | 206    | 217 | Não     |
| P0AEE4 | FDKSNDGETPEG               | 219    | 231 | Não     |

---

## 🔬 Topologia do Epítopo #1

```text
Sequence          NPFDQSSQPQQQPQQQPAQQEQKDSD

Emini             .EEEEEEEEEEEEEEEEEEEEEEEE.

Kolaskar          .........EEEEEEEE.........

Chou Fosman       EEEEEEEEEEEEEEEEE....EEEEE

Karplus Schulz    EEEEEEEEEEEEEEEEEEEEEEEEEE

Parker            EEEEEEEEEEEEEEEEEEEEEEEEEE

All matches       .........EEEEEEEE.........

N-Glyc            ..........................

Hydropathy        --+---------------+-------
```

---

## 📚 Proteoma

* ID: **UP000000558**
* Total de proteínas: **5.056**
* [https://www.uniprot.org/proteomes/UP000000558](https://www.uniprot.org/proteomes/UP000000558)

---

## 📎 Entregáveis

* ✔️ relatorio.pdf
* ✔️ input.fasta
* ✔️ top50.fasta
* ✔️ results_fastprotein/
* ✔️ results_epibuilder/
