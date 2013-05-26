# acts_as_highlighter #

Распространяется по MIT-лицензии

- - -

#### Версия ####

**Текущая: 0.0.2**

Примечание к предыдущим версиями смотрите в конце этого *README*

- - -

#### Добавляет основанную на JavaScript подсветку синтаксиса ####

Собственно подсветка взята вот [отсюда](https://github.com/alexgorbatchev/SyntaxHighlighter) *(англ.)*. Подробней можно посмотреть [здесь](http://alexgorbatchev.com/SyntaxHighlighter) *(англ.)*. Однако стоит отметить, что плагин не поддерживает различные темы, кроме дефолтной, доступные в исходной библиотеке.

#### Установка ####

добавьте строку в `Gemfile`:

    gem 'acts_as_highlighter', git: 'git://github.com/logrusorgru/acts_as_highlighter.git'

затем выполните

    bundle install

после чего добавьте в файл `./app/assets/stylesheet/application.css[.scss|.sass]` строку:

    *= require acts_as_highlighter

и соответственно в `./app/assets/javascripts/application.js[.coffee]` строку:

    //= require acts_as_highlighter


#### Методы и параметры ####

Вы можете использовать метод `code_block+` модуля `ActsAsHighlighter`, либо метод `highlight` добавленный для класса `String`.

Примеры:

    ActsAsHighlighter.code_block( p1, [p2], [p3] )
    "some code".highlight( [p2], [p3] )

* `p1` - обязательный параметр - код для подсветки ( текст/строка )
* `p2` - не обязательный параметр - вариант синтаксиса ( см. ниже: *Варианты синтаксиса* ), если он не указан, то установлен в значение по умолчанию ( см. ниже: *Синтаксис по-умолчанию*), *строка*
* `p3` - не обязательный параметр - хеш опций ( см. ниже: *Хеш опций* ), если не установлен - то все опции по-умолчанию, *хеш*

Допускается не указывать второй параметр, но, при этом, указывать третий.

    ActsAsHighlighter( p1, p3 )
    "puts 'Hello World'".highlight( p3 )

#### Хеш опций: ####

Детали смотрите [здесь](http://alexgorbatchev.com/SyntaxHighlighter/manual/configuration) *(англ.)*

**:brush**

> Вариант синтаксиса, его можно передать и через хеш-опций и как параметр перед хешом опций. Если не установлен - то это синтаксис по-умолчанию ( см. ниже: *Синтаксис по-умолчанию* ). Примеры:
>
    "alert( 'Hello World!' )".highlight( :brush => "js" )
    "var a = 'a'".highlight( "js" )
    # выражение эквивалентно первому
    "puts 'a'".highlight( "cpp", :brush => "ruby" )
    # бессмыслица - второй параметр будет учтён, первый проигнорирован
    # подсветка синтаксиса - "ruby"

**:auto_links**

> Значение по-умолчанию: *true*. Автоматическое распознание адрессов (*url*) на странице и создание к ним ссылок. По умолчанию включено. Примеры:
>
    "puts 'my blog: http://www.some-example.blog.ru'".highlight( "ruby" )
    # ссылка будет кликабельна - выбрано значение по-умолчанию
    "url = 'https://github.com'".highlight( "ruby", :auto_links => "false" )
    # ссылки на GitHub.com не будет. 
    # Обратите внимание, что "false" - это строка, допускаетя использовать этот
    # параметр как логическое false и как строку "false". Разницы нет.

**:class_name**

> Значение по-умолчанию: *''*. Задать элементу *css*-класс для обработки его (элемента) через стили *css*. Пример:
>
    "xor eax,eax".highlight( "asm", :class_name => "assembly" )
    # элементу можно задать стили через css используюя класс assembly

**:gutter]**
> Значение по-умолчанию: *true*. Показывать номера строк. *true* - номера будут показаны, *false* - не будут покзаны. Допускается так же использовать строки *"true"* или *"false"* соответственно.

**:collapse**
> Значение по-умолчанию: *false*. Код в свёрнутом виде, разворачивается по клику, обратно не сворачивается. В свёрнутом виде видно только надпись **expand source** в рамке.

**:first_line**
> Значение по-умолчанию: *1*. Нумерация строк. Указывается с какой строки начать нумерацию. Значение - целое число, в т.ч. и в виде строки. Примеры:
>
    "nop".highlight( "asm", :first_line => 503 )
    "pusha".highlight( "asm", :first_line => "1024" )
    # допускаются оба варианта - строка с числом и чило

**:highlight**
> Значение по-умолчанию: *null*. Подсветка определённых строк. Параметр - целочисленный массив или целое число, допускается в виде строки. Примеры:
>
    "def func a,b\nputs a+b\nend".highlight( "ruby", :highlight => [1,3] )
    # строки 1 и 3 будут подсвечены
    "xor eax,eax\nmov ebx,eax\nnop".highlight( "asm", :highlight => 2 )
    # вторая строка будет подсвечена
    "a = true ?\n'a' : 'A'".highlight( "ruby", :highlight => "[1,2]" )
    # допускается использовать строки, будет подсвечены строки 1 и 2

**:html_script**
> Значение по-умолчанию: *false*. Не особо понял что делает эта опция, смотрите по ссылке, [может что прояснится](http://alexgorbatchev.com/SyntaxHighlighter/manual/configuration) *(англ.)* Предпологаю, что эта опция позволяет подсвечивать *html* в смеси с *php*, *asp* и *javascript*. Опять же предположительно - в этом случае стоит указать в качестве *:brush* соответственно *php* или например *asp*.

**:smart_tabs**
> Значение по-умолчанию: *true*. Какая-то фишка табуляции. Вообще не понял, но эта опция её меняет. Смотрите по ссылке выше, чтобы разобраться.

**:tab_size**
> Значение по-умолчанию: *4*. Размер табыляции. По умолчанию - четыре пробела.

**:toolbar**
> Значение по-умолчанию: *true*. Показывать малюсенькую ссылочку-кнопулечку в виде знака вопрос справа вверху - при нажатии на которую вылетает копирайт *Алекса Горбачёва*.

#### Варианты синтаксиса: ####

* `as3`, `actionscript3`
* `bash`, `shell`
* `cf`, `coldfusion`
* `c-sharp`, `csharp`
* `cpp`, `c`
* `css`
* `delphi`, `pas`, `pascal`
* `diff`, `patch`
* `erl`, `erlang`
* `groovy`
* `js`, `jscript`, `javascript`
* `jfx`, `javafx`
* `perl`, `pl`
* `php`
* `plain`, `text`
* `ps`, `powershell`
* `py`, `python`
* `rails`, `ror`, `ruby`
* `scala`
* `sql`
* `vb`, `vbnet`
* `xml`, `xhtml`, `xslt`, `html`

помимо этого ещё добавлены от [сюда](http://www.undermyhat.org/blog/2009/09/list-of-brushes-syntaxhighligher/) *(англ.)*:

* `ada`
* `asm`, `x86`                     *# этот и следующий - разные варианты*
* `nams8086`, `8086`, `nams`, `masm`
* `ahk`, `autohotkey`
* `bat`, `cmd`, `batch`
* `clojure`, `Clojure`, `clj`
* `latex`, `tex`
* `lua`
* `yaml`, `yml`

#### Синтаксис по-умолчанию ####

Собственно сам параметр синтаксис, можно не передавать - если установить нужное значение по-умолчанию. Первоначально оно установленно в **ruby**. Изменить его достаточно просто - при инициализации приложения установить `ActsAsHighlighter.default_syntax` в нужное значение, например так:

    ActsAsHighlighter.default_syntax = "c"

Эту строку можно поместить например в файл `highlighter_init.rb` в папке `/config/initializers/` приложения. Или изменять динамически в процессе работы приложения.

#### Применение ( примеры для *haml* ) ####

Ну например так:

    %div.code_container= code_block("def a; 'a' end")   # синтаксис ruby по-умолчанию

...или так:

    %p= my_string_variable.highlight("cpp")   # переменные строки можно подсвечивать на прямую

ну и совсем простой вариант:

    %pre{ :class => "brush: ruby;" }
      :preserve
        def a
          puts 'a'
        end

#### Применение ( примеры для *erb* ) ####

Ну например так:

    <div class="code_container">
    	<%= code_block("def a; 'a' end")  %> <!-- синтаксис ruby по-умолчанию -->
    </div>

...или так:

    <p>
        <%= my_string_variable.highlight("cpp") %>  <!-- переменные строки можно подсвечивать на прямую -->
    </p>

ну и совсем простой вариант:

    <pre class="brush: ruby;">
        def a
          puts 'a'
        end
    </pre>

- - -

#### Тестирование ####

Средствами тестовых инструментов проверок не проводилось. Однако работоспособность плагина была проверена на тестовом *Rails* приложении. Результат этого кода в *application.html.erb*:

    <%= ActsAsHighlighter.code_block("def a; puts 'a' end") %>
    <%= "alert(\"Hello world!!!\")".highlight("js") %>

можете увидеть здесь ![скриншот браузера](./test/check_acts_as_highlighter.png). Тот же самый результат, но с присутствием в дирректории приложения */config/initializers/* файла *highlighter_init.rb* следующего содержания:

    ActsAsHighlighter.default_syntax = "js"

и соответственно изменёных на

    <%= ActsAsHighlighter.code_block("def a; puts 'a' end", "ruby") %>
    <%= "alert(\"Hello world!!!\")".highlight %>

строках кода в *application.html.erb*. Вобщем эта штука работает!

- - -

#### Основания ####

Привлекательная подсветка, с вполне нормальным функционалом. Не загружает сервер, т.к. `подсвечивание` выполняется на строне клиента. При этом рекомендуется в заголовки ассетов включить кэширование на длительный период. Т.к. **.js** файл с учётом *jQuerry* и всех файлов для подсветки синтаксиса имеет размер *~205kB*, так же выдача сервером сжатых **.js.gz** файлов ещё сильнее снижает нагрузку на сервер ( размер *~75kB* )

#### Точки роста ####

Добавлене возможности менять темы, хотя бы при инициализации приложения. Добавление возможности выбора необходимых подсветок - исключая при этом файлы *.js*, которые отвечают за подсветку, не встречающуюся на данном ресурсе (сайте). При этом снижается размер результируещего .js/.js.gz файла.

#### Версия 0.0.1 ( предыдущая ) ####

Способ установки верси идентичен приведённому здесь. Основное отличие в том, что в версии 0.0.1 в качестве параметра можно передать только синтаксис языка, хеш опций для этой версии не доступен. Всё остальное аналогично.

- - -
- - -

#### Послесловие ####

Free for use ( лицензия *MIT* )

Free for fork

Free for patch