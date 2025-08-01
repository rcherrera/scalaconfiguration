# 📝 Resumen de Configuración PySpark + Spark Local

Este documento resume todo lo realizado para lograr ejecutar un notebook `.ipynb` con PySpark de forma local en Windows.

---

## ✅ Problema principal

El error recurrente en `getOrCreate()` (`JavaPackage is not callable`) fue causado por **incompatibilidades de versiones** entre:

- `pyspark` y `python`
- Spark local y el entorno de Jupyter
- Uso de Python 3.13 (aún no compatible con PySpark)

---

## ✅ Versiones funcionales confirmadas

| Componente | Versión funcional |
|------------|-------------------|
| Python     | 3.10 ✅            |
| Java       | 11.0.16.1 ✅       |
| PySpark    | 3.4.4 ✅           |
| Spark      | 3.4.4 ✅           |
| Scala      | 2.12.17 ✅         |
| sbt        | 1.9.7 ✅           |
| winutils   | Hadoop 3.4.0 ✅    |

---

## ⚙️ Variables de entorno necesarias

```powershell
[System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Microsoft\jdk-11.0.16.101-hotspot", "Machine")
[System.Environment]::SetEnvironmentVariable("SPARK_HOME", "C:\spark-3.4.4", "Machine")
[System.Environment]::SetEnvironmentVariable("HADOOP_HOME", "C:\hadoop", "Machine")
[System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\spark-3.4.4\bin;C:\hadoop\bin;C:\Program Files\Microsoft\jdk-11.0.16.101-hotspot\bin", "Machine")
```

---

## 🐍 Crear entorno virtual Python 3.10 con PySpark

1. Crear el entorno:

```bash
py -3.10 -m venv venv-spark
```

2. Activarlo:

```bash
.env-spark\Scriptsctivate
```

3. Instalar dependencias:

```bash
pip install pyspark==3.4.4 notebook ipykernel
```

4. Registrar kernel en Jupyter:

```bash
python -m ipykernel install --user --name pyspark310 --display-name "PySpark (Python 3.10)"
```

---

## 📁 Estructura recomendada del proyecto

```
TuProyecto/
├── venv-spark/
├── datasets/
│   └── vgsales.csv
├── notebooks/
│   └── PySpark_Proyecto_LECTURA_CSV_FINAL.ipynb
└── spark-3.4.4/
```

---

## 📘 Código base funcional en Jupyter

```python
import os

os.environ["JAVA_HOME"] = "C:\Program Files\Microsoft\jdk-11.0.16.101-hotspot"
os.environ["SPARK_HOME"] = "C:\spark-3.4.4"

from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("LecturaCSV") \
    .master("local[*]") \
    .getOrCreate()

df = spark.read.csv("datasets/vgsales.csv", header=True, inferSchema=True)
df.show(5)
df.printSchema()
```

---

✅ Con esta configuración, el entorno quedó estable y funcional para practicar Spark con Python en notebooks locales.
