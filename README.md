## Создание виртуального сервера в Oracle Cloud Infrastructure

Регистрируемся на [oracle.com](https://www.oracle.com/cloud/)

### Перед созданием сервера необходимо сгенерировать пару ключей ssh:

```bash
ssh-keygen -t rsa
```

Вам потребуется открытый ключ из пары ключей SSH, используемой для создания данного экземпляра.

```
~/.ssh/id_rsa.pub
```

### Создать экземпляр Compute

[Здесь](https://console.eu-frankfurt-1.oraclecloud.com/compute/instances/create)

Выбираем необходимый образ, добавляем файл публичной части ключа SSH (.pub). 

Входа по паролю нет.

### Выбираем созданный Экземпляр:

Вычисления > [	Экземпляры](https://console.eu-frankfurt-1.oraclecloud.com/compute/instances) > Сведения об экземпляре 

и находим данные для доступа к экземпляру:

	Общедоступный IP-адрес: <ip-adress>
	Имя пользователя: <username>

##### Подключение через SSH:
```bash
ssh username@ip_address —i private_key.pub
```
##### Копирование файлов через SSH
С локальной машины на удаленный север:
```bash
scp file.txt ubuntu@256.257.258.259:~/path/to/dir
```


### Обновление и установка необходимых пакетов
##### Обновление
```bash
sudo apt-get update && \
sudo apt-get upgrade
```

##### Установка пакетов
```bash
sudo apt-get install \
python3 \
python3-pip \
```

---

## Настройка виртуальной среды Python

#### virtualenv

Главная задача виртуальной среды Python – создание изолированной среды для проектов Python.

Это значит, что каждый проект может иметь свои собственные зависимости, вне зависимости от того, какие зависимости у другого проекта.

Если вы используете Python 3, у вас уже должен быть модуль venv, установленный в стандартной библиотеке.

```bash
python3 -m venv env_name
```

Активация виртуальной среды:
```bash
source env_name/bin/activate
```
Деактивация:
```bash
(env_name) $ deactivate
```

#### virtualenvwrapper

Полезные функции virtualenvwrapper:

- Организация каждой виртуальной среды в одном расположении;
- Предоставляются методы, которые помогут вам легко создавать, удалять и копировать виртуальную среду, а также,
- Предоставляет одну команду для переключения между средами


У вас в распоряжении будут доступные команды оболочки, которые помогут в управлении виртуальной средой. Вот несколько из них:

```
workon
deactivate
mkvirtualenv
cdvirtualenv
rmvirtualenv
```

Установка: 
```bash
pip3 install virtualenvwrapper
```

Настройка: 
```bash
which python3
>> /usr/bin/python3

which virtualenvwrapper.sh
>> /usr/local/bin/virtualenvwrapper.sh
```

Редактируем `~/.bashrc`, добавляем в конец файла строки:
```bash
# Virtualenvwrapper
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/vwrapperhome
source /usr/local/bin/virtualenvwrapper.sh
```

Применяем настройки:
```bash
source ~/.bashrc
```

---

## Деплой

##### Клонирование репозитория на сервер
```bash
git clone https://github.com/username/repo_name.git
```

##### Секретные данные
Создать файл ```.env``` в папке с проектом и поместить в него токены Telegram и ВКонтакте:
```
TELEGRAM_TOKEN=<token>
VK_TOKEN=<token>
REDIS_HOST=<hostname>
REDIS_PORT=<port>
REDIS_DB=1
REDIS_PASSWORD=<password>
```

#####  Установка python-пакетов которые используются в проекте

```bash
pip3 install -r requirements.txt
```

##### Запуск приложения
```python
python3 main.py
```

Запуск в фоне:
```
nohup python3 bot-tg.py &
nohup python3 bot-vk.py &
```

Посмотреть работающее:
```
ps aux | grep .py
```


## Cron

`crontab -e`  - редактировать таблицу задач

`crontab -l` - показать таблицу задач

`crontab -r` - удалить таблицу задач

`crontab path/to/file.crontab` - загрузить таблицу задач из файла

Сами задачи описаны в следующем формате:

```
# строки-комментарии игнорируются
#
# задача, выполняемая ежеминутно
* * * * * /path/to/exec -a -b -c
# задача, выполняемая на 10-й минуте каждого часа
10 * * * * /path/to/exec -a -b -c
# задача, выполняемая на 10-й минуте второго часа каждого дня и использующая перенаправление стандартного потока вывода
10 2 * * * /path/to/exec -a -b -c > /tmp/cron-job-output.log
# задача, выполняемая в первую и десятую минуты каждого часа
1,10 * * * * /path/to/exec -a -b -c
# задача, выполняемая в каждую из первых десяти минут каждого часа
0-9 * * * * /path/to/exec -a -b -c

```

Первые пять полей записей: минуты [1..60], часы [0..23], дни месяца [1..31], месяцы [1..12], дни недели [0..6], где 0 — воскресенье. Последнее, шестое, поле — строка, которая будет выполнена стандартным интерпретатором команд.

В первых пяти полях значения можно перечислять через запятую или дефис

---

## ToDo

За%уярь все это в контейнер и запускай одной коммандой

