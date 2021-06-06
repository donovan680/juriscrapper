# juriscrapper
Juriscrapper é uma biblioteca scraper iniciada há vários anos que reúne opiniões judiciais, argumentos orais e dados PACER no sistema judiciário americano. 
Atualmente é capaz de scrap:
1- uma variedade de páginas e relatórios dentro do sistema PACER;
2- opiniões de todos os principais tribunais federais de apelação;
3- opiniões de todos os tribunais estaduais de último recurso, exceto para a Geórgia (normalmente a "Suprema Corte");
4- argumentos orais de todos os tribunais federais de apelação que os oferecem.

Juriscraper faz parte de um sistema de duas partes. 
A segunda parte é o seu código, que  se chama Juriscraper. 
Seu código é responsável por chamar um crawler , baixar e salvar seus resultados. 
Uma implementação de referência do chamador foi desenvolvida e está em uso em CourtListener.com'<https://www.courtlistener.com>'__. 

O código desse chamador pode ser encontrado aqui neste site. Há também um chamador de amostra básico incluído no Juriscraper que pode ser usado para teste ou como ponto de partida ao desenvolver o seu próprio.


Alguns dos objetivos do Design  de  Software para este projeto são:

- extensibilidade para suportar vídeo, áudio de argumento oral, etc.
- extensibilidade para apoiar geografias (EUA, Cuba, México, Califórnia)
- Identificação do tipo MIME por meio de números mágicos
- Arquitetura generalizada com repetição mínima de código
- Scraping baseado em XPath com base no analisador HTML de lxml.
- retornar todos os metadados disponíveis nos sites do tribunal (o autor da chamada pode escolher o que precisa)
- não há necessidade de um banco de dados
- limpar níveis de registro (DEBUG, INFO, WARN, CRITICAL)
- amigável quanto possível para sites de tribunal



Installation & Dependencies
===========================

First step: Install Python 3.7+.x, then:

Install the dependencies
------------------------

On Ubuntu/Debian Linux::

    sudo apt-get install libxml2-dev libxslt-dev libyaml-dev

On macOS with Homebrew <https://brew.sh>::

    brew install libyaml


Then install the code
---------------------

::

    pip install juriscraper


Você pode definir uma variável de ambiente para onde deseja armazenar seus logs (isso pode ser ignorado, e `/ var / log / juriscraper / debug.log` será usado como o
padrão se existir no sistema de arquivos) ::

    export JURISCRAPER_LOG=/path/to/your/log.txt


Finalmente, faça o seu WebDriver
--------------------------
Alguns sites são muito difíceis de rastrear sem algum tipo de sistema automatizado WebDriver. Para estes, o Juriscraper usa uma cópia instalada localmente do geckodriver ou pode ser configurado para se conectar a um webdriver remoto. Se você preferir a instalação local, você pode baixar Selenium FireFox Geckodriver ::
    # escolha o pacote compatível com o sistema operacional:
    # https://github.com/mozilla/geckodriver/releases/tag/v0.26.0
    # un-tar / zip seu download
    sudo mv geckodriver / usr / local / bin   (Linux) 

Se você preferir usar um webdriver remoto, como a "imagem" DOCKER do Selenium <https://hub.docker.com/r/selenium/standalone-firefox-debug>` __, você pode configurá-lo  com as seguintes variáveis:

'WEBDRIVER_CONN' : Use isto para definir a string de conexão para o seu remoto webdriver. Por padrão, é 'local', o que significa que irá procurar por um local de instalação do geckodriver. 
Em vez disso, você pode definir isso para algo como,
'http: // YOUR_DOCKER_IP: 4444 / wd / hub', que irá alterná-lo para usar um driver remoto e conecte-o a esse local.

'SELENIUM_VISIBLE' : Defina isso para qualquer valor para desativar o modo headless em seu driver de SELENIUM , se for compatível. Caso contrário, o padrão é headless.

Por exemplo, se você quiser assistir a execução de um navegador sem cabeça, pode fazer isso começando SELENIUM  com ::

    docker run \
        -p 4444: 4444 \
        -p 5900: 5900 \
        -v / dev / shm: / dev / shm \
         / autônomo-firefox-debug

Isso o lançará em sua máquina local com duas portas abertas:  4444 é o padrão na imagem para acessar o webdriver. 
5900 pode ser usado para conectar através de um visualizador VNC, e pode ser usado para assistir o progresso se a variável "SELENIUM_VISIBLE"  estiver definida.

Depois de ter o SELENIUM funcionando assim, você pode fazer um teste como:

    WEBDRIVER_CONN = 'http: // localhost: 4444 / wd / hub' \
    SELENIUM_VISIBLE = yes \
    python sample_caller.py -c          juriscraper.opinions.united_states.state.kan_p

O  scraper  precedencial do Kansas usa um driver da web. Se você fizer isso e observar o Selenium , deverá vê-lo em ação.
