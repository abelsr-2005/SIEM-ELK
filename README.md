# 🛡️ Laboratorio SIEM – ELK Stack con Filebeat

## 📌 Descripción

Este repositorio documenta un laboratorio enfocado en el despliegue y comprensión de un entorno **SIEM (Security Information and Event Management)** utilizando el **ELK Stack (Elasticsearch, Logstash y Kibana)** junto con **Filebeat** para la recolección de logs.

El objetivo de esta práctica es simular una infraestructura básica de **monitorización de seguridad**, capaz de **recoger, procesar, almacenar y visualizar registros (logs)** generados por servicios como **NGINX**.

El entorno se despliega mediante **Docker Compose**, lo que permite ejecutar todos los componentes en contenedores aislados.

---

## 🧱 Arquitectura

El laboratorio se basa en la siguiente arquitectura de flujo de logs:

Cliente / Servicios → Filebeat → Logstash → Elasticsearch → Kibana

### Componentes

* **NGINX**: Genera logs de acceso y error.
* **Filebeat**: Agente ligero encargado de recopilar logs del sistema y enviarlos a Logstash.
* **Logstash**: Procesa, transforma y normaliza los eventos de log.
* **Elasticsearch**: Almacena e indexa los datos para permitir búsquedas rápidas.
* **Kibana**: Plataforma de visualización utilizada para analizar logs y crear dashboards.

---

## 🐳 Despliegue con Docker Compose

La infraestructura se despliega utilizando contenedores Docker definidos en el archivo `docker-compose.yml`.

Servicios típicos incluidos:

* elasticsearch
* logstash
* kibana
* filebeat
* nginx

### Iniciar el laboratorio

```bash
docker compose up -d
```

### Detener el laboratorio

```bash
docker compose down
```

---

## 📥 Recolección de logs con Filebeat

`Filebeat` es el encargado de leer los logs generados por el contenedor de **NGINX** y enviarlos a **Logstash** para su procesamiento.

Tareas típicas de configuración:

* Definir rutas de los logs
* Activar módulos si es necesario
* Configurar Logstash como salida

Ejemplo conceptual de configuración:

```yaml
filebeat.inputs:
  - type: log
    paths:
      - /var/log/nginx/*.log

output.logstash:
  hosts: ["logstash:5044"]
```

---

## 🔄 Procesamiento de logs con Logstash

Logstash recibe los logs enviados por Filebeat y los procesa mediante pipelines definidos en `logstash.conf`.

Tareas habituales de procesamiento:

* Parseo de logs mediante **filtros Grok**
* Estructuración de campos de log
* Envío de los eventos procesados a Elasticsearch

Ejemplo de pipeline:

```conf
input {
  beats {
    port => 5044
  }
}

filter {
  # reglas de parseo
}

output {
  elasticsearch {
    hosts => ["http://elasticsearch:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
}
```

---

## 📦 Almacenamiento de logs con Elasticsearch

**Elasticsearch** almacena los eventos procesados y proporciona capacidades avanzadas de **indexación y búsqueda**.

Características principales:

* Motor de búsqueda distribuido
* Indexación de logs en tiempo real
* Capacidad para analizar grandes volúmenes de datos

Los logs almacenados pueden ser consultados posteriormente desde **Kibana**.

---

## 📊 Visualización de logs con Kibana

**Kibana** proporciona una interfaz web para explorar y analizar los datos almacenados en Elasticsearch.

Acciones habituales realizadas en este laboratorio:

* Crear un **Index Pattern**
* Visualizar logs en la sección **Discover**
* Crear visualizaciones
* Construir dashboards para monitorización

Acceso por defecto:

```
http://localhost:5601
```

---

## 🔐 Autenticación (Opcional)

Se puede configurar **autenticación básica** para servicios como **NGINX** utilizando un archivo `htpasswd`.

Esto permite proteger ciertos recursos y generar logs de autenticación que posteriormente pueden ser analizados por el SIEM.

---

## 🧪 Pruebas del SIEM

Para generar logs y comprobar que el pipeline funciona correctamente:

1. Enviar peticiones HTTP al servidor NGINX.
2. Verificar que Filebeat recopila los logs.
3. Confirmar que Logstash recibe y procesa los eventos.
4. Comprobar que Elasticsearch indexa los datos.
5. Visualizar los logs desde Kibana.

Ejemplo de prueba:

```bash
curl http://localhost
```

---

## 🎯 Objetivos de aprendizaje

A través de este laboratorio se pretende:

* Comprender el **funcionamiento de los sistemas SIEM en ciberseguridad**.
* Desplegar una **infraestructura de logging centralizada**.
* Recoger logs utilizando **Filebeat**.
* Procesar y normalizar eventos con **Logstash**.
* Almacenar logs en **Elasticsearch**.
* Analizar eventos de seguridad mediante **Kibana**.

---

## 📚 Tecnologías utilizadas

* Docker
* Docker Compose
* Elasticsearch
* Logstash
* Kibana
* Filebeat
* NGINX

---

## 📌 Conclusión

Este laboratorio demuestra el flujo fundamental de funcionamiento de una plataforma **SIEM**, desde la generación de logs hasta su visualización. Integrando el **ELK Stack** con **Filebeat**, es posible construir un sistema centralizado de gestión de logs capaz de apoyar tareas de **monitorización de seguridad, detección de incidentes y análisis forense**.

---


<div align="center">

## 👨‍💻 Autor
**Abel Sánchez Ramos**  
🎓 Curso de Especialización en Ciberseguridad  

---

🛡️ SIEM Lab Project  
📊 Security Monitoring • Log Analysis • Threat Detection  
🚀 Built with ELK Stack

</div>
