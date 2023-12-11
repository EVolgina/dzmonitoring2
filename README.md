# Обязательные задания
- Вас пригласили настроить мониторинг на проект. На онбординге вам рассказали, что проект представляет из себя платформу для вычислений с выдачей текстовых отчетов, которые сохраняются на диск. Взаимодействие с платформой осуществляется по протоколу http. Также вам отметили, что вычисления загружают ЦПУ. Какой минимальный набор метрик вы выведите в мониторинг и почему?
  ## Ответ:
  - CPU utilization: Следить за использованием процессора, поскольку вычисления загружают ЦПУ.
  - HTTP response time: Оценка времени, требуемого для обработки HTTP-запросов, чтобы понимать производительность платформы
  - Disk space: Для контроля за сохраняемыми отчетами на диске и предотвращения возможных проблем с заполнением диска.
  - HTTP error rates: Отслеживание частоты возникновения ошибок HTTP может помочь выявить проблемы в обработке запросов.
- Менеджер продукта посмотрев на ваши метрики сказал, что ему непонятно что такое RAM/inodes/CPUla. Также он сказал, что хочет понимать, насколько мы выполняем свои обязанности перед клиентами и какое качество обслуживания. Что вы можете ему предложить?
  ## Ответ:
  - RAM (Random Access Memory): Объем оперативной памяти, используемой системой
  - Inodes: Количество индексных узлов в файловой системе, что важно для отслеживания использования файловых ресурсов
  - CPU Load Average (CPUla): Средняя загрузка процессора за определенный период времени
- Вашей DevOps команде в этом году не выделили финансирование на построение системы сбора логов. Разработчики в свою очередь хотят видеть все ошибки, которые выдают их приложения. Какое решение вы можете предпринять в этой ситуации, чтобы разработчики получали ошибки приложения?
  ## Ответ:
  - Использование легковесных инструментов логирования в коде (например, print() в Python)
  - Централизованное хранение внутренних логов на сервере разработки, если не удается построить полноценную систему логирования
  - Использование сервисов мониторинга ошибок (error monitoring services): Разверните инструменты, такие как Sentry, чтобы автоматически собирать и отслеживать ошибки в приложении. Эти инструменты могут предоставить подробные отчеты о каждой ошибке
- Вы, как опытный SRE, сделали мониторинг, куда вывели отображения выполнения SLA=99% по http кодам ответов. Вычисляете этот параметр по следующей формуле: summ_2xx_requests/summ_all_requests. Данный параметр не поднимается выше 70%, но при этом в вашей системе нет кодов ответа 5xx и 4xx. Где у вас ошибка?
  ## Ответ:
  - Ошибка в SLA мониторинге: Ошибка скорее всего в формуле вычисления SLA. Она должна быть следующей: summ_2xx_requests / (summ_2xx_requests + summ_5xx_requests + summ_4xx_requests).Вероятно, в формуле не учитываются 4xx запросы, что приводит к неверному расчету SLA.

- Опишите основные плюсы и минусы pull и push систем мониторинга.
- Push:
-	Плюсы: Быстрая обратная связь, возможность мгновенного реагирования.
-	Минусы: Повышенное использование ресурсов, сложность масштабирования.
-	Pull:
-	Плюсы: Эффективное использование ресурсов, легкость масштабирования.
-	Минусы: Задержка в обнаружении изменений, медленная обратная связь.

- Какие из ниже перечисленных систем относятся к push модели, а какие к pull? А может есть гибридные?
- Prometheus - 	Push
- TICK - 	Push
- Zabbix - Pull
- VictoriaMetrics - Гибридный 	Pull/Push
- Nagios - 	Pull
- Склонируйте себе репозиторий и запустите TICK-стэк, используя технологии docker и docker-compose.
- В виде решения на это упражнение приведите скриншот веб-интерфейса ПО chronograf (http://localhost:8888).
![1](https://github.com/EVolgina/dzmonitoring2/blob/main/hronograf.PNG) 
P.S.: если при запуске некоторые контейнеры будут падать с ошибкой - проставьте им режим Z, например ./data:/var/lib:Z
Перейдите в веб-интерфейс Chronograf (http://localhost:8888) и откройте вкладку Data explorer.
![2](https://github.com/EVolgina/dzmonitoring2/blob/main/%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B0.PNG)
внесла изменения в фвйл telegraf.conf
```
[[outputs.influxdb]]
  urls = ["http://localhost:8086"]
  database = "telegraf"
  username = "admin"
  password = "admin123"
  retention_policy = ""
  write_consistency = "any"
  timeout = "5s"
```
vagrant@vagrant:~/sandbox$ docker-compose up -d
WARNING: The TYPE variable is not set. Defaulting to a blank string.
Recreating sandbox_influxdb_1 ...
Recreating sandbox_influxdb_1 ... done
Recreating sandbox_telegraf_1  ... done
Recreating sandbox_kapacitor_1 ... done
Recreating sandbox_chronograf_1 ... done
vagrant@vagrant:~/sandbox$ ./sandbox up
Using latest, stable releases
Spinning up Docker Images...
If this is your first time starting sandbox this might take a minute...
Building influxdb
Step 1/2 : ARG INFLUXDB_TAG
Step 2/2 : FROM influxdb:$INFLUXDB_TAG
 ---> 6e977ef79a26

Successfully built 6e977ef79a26
Successfully tagged influxdb:latest
Building telegraf
Step 1/2 : ARG TELEGRAF_TAG
Step 2/2 : FROM telegraf:$TELEGRAF_TAG
 ---> 161be23be4a0

Successfully built 161be23be4a0
Successfully tagged telegraf:latest
Building kapacitor
Step 1/2 : ARG KAPACITOR_TAG
Step 2/2 : FROM kapacitor:$KAPACITOR_TAG
 ---> 79b5a5d5782d

Successfully built 79b5a5d5782d
Successfully tagged kapacitor:latest
Building chronograf
Step 1/4 : ARG CHRONOGRAF_TAG
Step 2/4 : FROM chronograf:$CHRONOGRAF_TAG
 ---> a1a8a073dfc6
Step 3/4 : ADD ./sandbox.src ./usr/share/chronograf/resources/
 ---> Using cache
 ---> a3f649c94325
Step 4/4 : ADD ./sandbox-kapa.kap ./usr/share/chronograf/resources/
 ---> Using cache
 ---> 2e451b704e1b

Successfully built 2e451b704e1b
Successfully tagged chrono_config:latest
Building documentation
Step 1/6 : FROM alpine:3.12
 ---> 24c8ece58a1a
Step 2/6 : EXPOSE 3010:3000
 ---> Using cache
 ---> 284310bb4cf4
Step 3/6 : RUN mkdir -p /documentation
 ---> Using cache
 ---> 4dda5a9d0550
Step 4/6 : COPY builds/documentation /documentation/
 ---> Using cache
 ---> e4e50da8ac18
Step 5/6 : COPY static/ /documentation/static
 ---> Using cache
 ---> b9fc045c3607
Step 6/6 : CMD ["/documentation/documentation", "-filePath", "/documentation/"]
 ---> Using cache
 ---> 79596daa0688

Successfully built 79596daa0688
Successfully tagged sandbox_documentation:latest
sandbox_documentation_1 is up-to-date
Recreating sandbox_influxdb_1 ... done
Recreating sandbox_kapacitor_1 ... done
Recreating sandbox_telegraf_1  ... done
Recreating sandbox_chronograf_1 ... done
Opening tabs in browser...
/usr/bin/xdg-open: 869: www-browser: not found
/usr/bin/xdg-open: 869: links2: not found
/usr/bin/xdg-open: 869: elinks: not found
/usr/bin/xdg-open: 869: links: not found
/usr/bin/xdg-open: 869: lynx: not found
/usr/bin/xdg-open: 869: w3m: not found
xdg-open: no method available for opening 'http://localhost:8888'
```
```
vagrant@vagrant:~/sandbox$ docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED              STATUS              PORTS                                                                                                                             NAMES
f130aa91f752   chrono_config           "/entrypoint.sh chro…"   59 seconds ago       Up 56 seconds       0.0.0.0:8888->8888/tcp, :::8888->8888/tcp                                                                                         sandbox_chronograf_1
732ff773adc5   kapacitor               "/entrypoint.sh kapa…"   About a minute ago   Up About a minute   0.0.0.0:9092->9092/tcp, :::9092->9092/tcp                                                                                         sandbox_kapacitor_1
677c663ce0f5   telegraf                "/entrypoint.sh tele…"   About a minute ago   Up About a minute   8092/udp, 8125/udp, 8094/tcp                                                                                                      sandbox_telegraf_1
6abe6f2b4ae9   influxdb                "/entrypoint.sh infl…"   About a minute ago   Up About a minute   0.0.0.0:8082->8082/tcp, :::8082->8082/tcp, 0.0.0.0:8086->8086/tcp, :::8086->8086/tcp, 0.0.0.0:8089->8089/udp, :::8089->8089/udp   sandbox_influxdb_1
96adac0c8906   sandbox_documentation   "/documentation/docu…"   8 days ago           Up 31 hours         0.0.0.0:3010->3000/tcp, :::3010->3000/tcp                                                                                         sandbox_documentation_1
```
- на web интерффейс http://localhost:8086 захожу логином и паролем, а дальше не могу состыковать с telegraf
![influx](https://github.com/EVolgina/dzmonitoring2/blob/main/influsdb2.PNG)
![inf]()
![teleg](https://github.com/EVolgina/dzmonitoring2/blob/main/telegraf1122.PNG)
-Нажмите на кнопку Add a query
-Изучите вывод интерфейса и выберите БД telegraf.autogen
-В measurments выберите cpu->host->telegraf-getting-started, а в fields выберите usage_system. Внизу появится график утилизации cpu.
-Вверху вы можете увидеть запрос, аналогичный SQL-синтаксису. Поэкспериментируйте с запросом, попробуйте изменить группировку и интервал наблюдений.
Для выполнения задания приведите скриншот с отображением метрик утилизации cpu из веб-интерфейса.

-Изучите список telegraf inputs. Добавьте в конфигурацию telegraf следующий плагин - docker:
```
[[inputs.docker]]
  endpoint = "unix:///var/run/docker.sock"
```
Дополнительно вам может потребоваться донастройка контейнера telegraf в docker-compose.yml дополнительного volume и режима privileged:
```
  telegraf:
    image: telegraf:1.4.0
    privileged: true
    volumes:
      - ./etc/telegraf.conf:/etc/telegraf/telegraf.conf:Z
      - /var/run/docker.sock:/var/run/docker.sock:Z
    links:
      - influxdb
    ports:
      - "8092:8092/udp"
      - "8094:8094"
      - "8125:8125/udp"
```
- После настройке перезапустите telegraf, обновите веб интерфейс и приведите скриншотом список measurments в веб-интерфейсе базы telegraf.autogen . Там должны появиться метрики, связанные с docker.
- Факультативно можете изучить какие метрики собирает telegraf после выполнения данного задания.
