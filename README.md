# TcpClientServerApp
------------
Проект представляет из себя пример, клиент-серверной системы, в которой сервер формирует задачу по численному интегрированию для функции**  f(x) = 1/ln(x) ** с заданием от пользователя: *нижнего предела, верхнего предела и шага интегрирования*.

TCP cервер асинхронный, он делит общую задачу на всех подключенных клиентов равномерно по количеству потоков на каждом клиенте (клиент передает серверу информацию о том, сколько потоков он желает использовать для выполнения задачи). 
Основной класс `ServerTcp`.  Данные о задаче, которую необходимо выполнить клиенту предаются в сериализованном виде.
Для хранения иформации о пользовательских сеансах используется класс `Session` .

####Численное интегрирование  f(x) = 1/ln(x)
Чтобы численно интегрировать функцию
>  f(x) = 1/ln(x);

необходимо учитывать следующие условия и ограничения:

1. интегрирование функции 1/ln(x) должно производиться на отрезке, где функция определена и непрерывна. следовательно, важно что бы \( x > 0 \), так как (ln(x)) определен только для положительных значений.

2. слишком большой или слишком маленький шаг может привести к неточным результатам.


### Зависимости / Dependencies
- C++17
- [Boost::Asio](https://www.boost.org/doc/libs/1_84_0/doc/html/boost_asio.html)
- [Boost::Serialization](https://www.boost.org/doc/libs/1_79_0/libs/serialization/doc/index.html)
- [CMake](https://cmake.org/)

### Сборка / Build
```shell
git clone --recursive git@github.com:ShtykovGregoryYMT07/-TcpClientServerApp.git
cd TcpClientServerApp/TestTask/
cmake -S . -B build
cmake --build build
```
Исполняемые файлы клиента и сервера будут созданы в каталоге сборки:

-` ./build/TestTaskTcpServer/TestTaskTcpServer`

-` ./build/TestTaskTcpClient/TestTaskTcpClient`


## Запуск / Run

Запустить `TestTaskTcpServer` в терминале:
*Прим.*
*1. запуск сервера выполняется на порту "3456";*
*2. завершение работы сервера выполняется нажатием Ctrl + C.*

```shell
./build/TestTaskTcpServer/TestTaskTcpServer  <lowerBound> <upperBound> <step> <typeIntegration>
где:

- <lowerBound> - нижняя граница интеграла.

- <upperBound> - верхняя граница интеграла.

- <step> - шаг интегрирования.

- <typeIntegration> - тип интегрирования (целое число). 1 - Монте-Карло; 0 - прямоугольников.
```
При включении сервер начинает ожидать подключения новых клиентов, пользователь после того, как нужное кол-во клиентов подключилось должен ввести в консоль команду и нажать :
```shell
/start
```
После чего сервер распределит вычисления пропроционально на каждого клиента с учетом кол-ва потоков (полученных от клиента). По завершению работы на экран и в лог выведится результат интерирования.

*Пример*
```shell
./TestTaskTcpServer 3.0 5.8956 0.00001 1
::New connect:: threads client::3
::New connect:: threads client::5
::New connect:: threads client::7

/start
...
::Calculation result::2.00003
```

Запуск `TestTaskTcpClient` в другом терминале:

```shell
./build/TestTaskTcpClient/TestTaskTcpClient <numberUseTreads>
где:

- <numberUseTreads> - кол-во потоков, которые будет использовать клиент.
```
## Логирование 
В процессе работы сервер и клиент гернерирует текстовые логи, файлы со следующим форматом имени:
log__instance_ `< PID >`.txt

Т.О. каждый запущенный клиент и сервер, будут иметь уникальные логи на момент сесиии.
