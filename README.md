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

* Proteínas processadas: **5.400**
* Ignoradas: **0**
* Tempo total: **~6 min 30 s**
* Proteínas com domínio transmembranar: **~1.200**
* Proteínas com evidência de membrana: **~900**

**EpiBuilder**

* Proteínas analisadas: **50**
* Proteínas com epítopos preditos: **38**

---

## 📋 Epítopos (Top 5)

| ID     | Epítopo             | Início | Fim | N-glico |
| ------ | ------------------- | ------ | --- | ------- |
| P0A8V2 | MKAILVVLLYTFATANADT | 1      | 19  | Não     |
| P0A940 | GLTSAAAGLLVAMGAGT   | 45     | 61  | Não     |
| P0A7V0 | TAVVAGLALAGLVLQ     | 12     | 26  | Não     |
| P0A7N9 | VAGSSLLAALATASA     | 33     | 47  | Não     |
| P0A805 | LLGAGALVAAGLSA      | 20     | 33  | Não     |

---

## 🔬 Topologia do Epítopo #1

```
Protein: P0A8V2
Localization: Membrane
Epitope: MKAILVVLLYTFATANADT
Position: 1 - 19
Topology: Extracellular region
N-glycosylation: No
```

---

## 📚 Proteoma

* ID: **UP000000558**
* Total de proteínas: **~5.400**
* [https://www.uniprot.org/proteomes/UP000000558](https://www.uniprot.org/proteomes/UP000000558)

---

## 📎 Entregáveis

* ✔️ relatorio.pdf
* ✔️ input.fasta
* ✔️ top50.fasta
* ✔️ results_fastprotein/
* ✔️ results_epibuilder/
