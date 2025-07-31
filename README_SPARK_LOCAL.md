# Guía de Instalación Local de Apache Spark en Windows

Esta guía detalla los pasos para configurar un entorno local con Apache Spark, Scala, Java, sbt y Visual Studio Code en Windows 10/11.

## ✅ Requisitos y versiones usadas

| Herramienta    | Versión         |
|----------------|------------------|
| Java           | 11.0.16.1        |
| Scala          | 2.12.17          |
| Spark          | 3.4.4            |
| Hadoop (winutils) | 3.4.0         |
| sbt            | 1.9.7            |
| IDE            | Visual Studio Code |

---

## 🔧 Paso 1: Instalar Java

1. Instalar JDK 11.
2. Agregar variable de entorno `JAVA_HOME`:
    ```powershell
    [System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Microsoft\jdk-11.0.16.1", "Machine")
    ```

---

## 🔣 Paso 2: Instalar Scala

1. Descargar desde [scala-lang.org](https://www.scala-lang.org/download/).
2. Usar la versión 2.12.17.
3. Agregar variables de entorno:
    ```powershell
    [System.Environment]::SetEnvironmentVariable("SCALA_HOME", "C:\Program Files (x86)\scala", "Machine")
    [System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files (x86)\scala\bin", "Machine")
    ```

---

## ⚡ Paso 3: Instalar Spark

1. Descargar Spark 3.4.4 desde [spark.apache.org](https://spark.apache.org/downloads.html).
2. Extraer en `C:\spark-3.4.4`.
3. Configurar entorno:
    ```powershell
    [System.Environment]::SetEnvironmentVariable("SPARK_HOME", "C:\spark-3.4.4", "Machine")
    [System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\spark-3.4.4\bin", "Machine")
    ```

---

## 🪓 Paso 4: Agregar `winutils.exe` (Hadoop en Windows)

1. Descargar `winutils.exe` de Hadoop 3.4.0 desde kontext.tech.
2. Colocar en `C:\hadoop\bin`.
3. Configurar entorno:
    ```powershell
    [System.Environment]::SetEnvironmentVariable("HADOOP_HOME", "C:\hadoop", "Machine")
    [System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\hadoop\bin", "Machine")
    ```

---

## 📦 Paso 5: Instalar `sbt`

1. Descargar desde [scala-sbt.org](https://www.scala-sbt.org/download.html).
2. Instalar por defecto.
3. Verificar en terminal con `sbt sbtVersion`.

---

## 🧪 Paso 6: Crear Proyecto Spark con sbt

```bash
proy01/
├── build.sbt
├── project/
│   └── build.properties
└── src/
    └── main/
        └── scala/
            └── HolaSpark.scala
```

### `build.sbt`
```scala
name := "SparkProyecto"
version := "0.1"
scalaVersion := "2.12.17"
libraryDependencies += "org.apache.spark" %% "spark-core" % "3.4.4"
libraryDependencies += "org.apache.spark" %% "spark-sql" % "3.4.4"
```

### `project/build.properties`
```properties
sbt.version=1.9.7
```

### `src/main/scala/HolaSpark.scala`
```scala
import org.apache.spark.sql.SparkSession

object HolaSpark {
  def main(args: Array[String]): Unit = {
    val spark = SparkSession.builder()
      .appName("Hola desde Spark")
      .master("local[*]")
      .getOrCreate()

    val datos = spark.range(1, 6)
    datos.show()
    spark.stop()
  }
}
```

---

## ▶️ Paso 7: Ejecutar con VS Code

1. Abrir la carpeta en VS Code.
2. Instalar extensiones: Scala (Metals), Java Extension Pack.
3. Ejecutar:
    ```bash
    sbt run
    ```

¡Y listo! Spark funcionando local en Windows con amor y paciencia. 💻🔥
