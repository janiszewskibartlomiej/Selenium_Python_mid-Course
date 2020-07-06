```python
##### 1. tworzenie wirtualnego srodowiska

`python -m venv env`   trzeba dodać do .gitignore

`.\env\Scripts\activate`   Actywacja

`deactivate`  usuwanie env

##### 2. Exceptions:

    NoSuchElementException  <- nie mozna znaleśc elementu
    InvalidSelectorException    <- nie można znalesc tego selectrora
    ElementNotVisibleException  <- element jest w DOM ale nie jest widoczny
    ElementNotSelectableException   <- element ktorego nie mozna wybrac jak np 'script'
    TimeoutException    <- jezeli komenda nie wykonala sie w zalozonym czasie
    SessionNotCreatedException    <- nie mozna bylo stworzyc nowej sesji
    StaleElementReferenceException  <- dany element nie pojawia sie juz w DOM
    NoSuchAttributeException    <- nie mozna znalesc atrybutu elementu
    
##### 3. CSS i XPATH - ŁĄCZENIE WYRAŻEŃ:

    CSS : img[scr='yoda.jg'][class = 'frame']
    XPATH : //img[@src='yoda.ipg' and @class='frame']
    
##### 4. CSS i XPATH - jeden element po wskazanym elemencie:

    CSS : img.frame + img
    XPATH : //img[@class='frame']/following-sibling::img
    
##### 5. XPATH - Szukanie dowolengo textu w DOM:

    //*[text()='Master Yoda']
    
##### 6. Szukanie po rodzicach - pseudo klasy:

    //p[text()='Talent desciple']/parent::div
    
##### 7. Pomijanie elemntu > 'not':

    CSS : img:not(.frame)
    XPATH : //img[not(@class='frame')]  - pierwszy element po img z pominiecie lasy frame
    
##### 8. Commands:

    element.get_attribute('checked') <- sprawdzenie czy checkbox jest zaznaczony - zwraca 'true'
    element.is_enabled() <- sprawdza czy butto jest możliwy do klikniecia zwrac 'true' lub 'false'
    element.send_keys('jakiś tekst') <- wpisuje tekst
    element.clear() <-czysci pole
    driver.find_elementS_by.... > zwaraca liste wszystkich elementów np wszystkie buttony ratio
    driver.find_element_by_xpath("//*[text()='Male']//preceding-sibling::input")  <- znajdzi elemnt przed tekstem 'Male'  
    - ponieważ klikanie na sam tekst nic nam nie da, trzeba znalesc input przed tym tekstem
    driver.page_source <- zrodlo strony
    driver.implicitly_wait(30) <- ustawiamy ile mamy czekac maxymalnie na jakikolwiek element na stronie  
    - unikac poniewaz jest w konflikcie z explicit wait - webdriverWait
    
    WebDriverWait(self.driver, time).until(expected_conditions..... cale mnostwo waitow ustawianych indywidualnie np
    selenium.webdriver.support.expected_conditions.visibility_of_element_located(locator) - An expectation for checking 
    that an element is present on the DOM of a page and visible.
    
    driver.execute_script(‘return document.title;’)  <- mamy mozliwosc wykonywania codu JS co jest super 
    bo szybko mozna pobrac wartosci z DOM
    driver.execute_script("document.body.style.zoom = '1.7'") <- powiekszenie strony
    
    driver.get_screenshot_as_file('Screenshots/BugsNoKlubURL.png') <- wrzuciłem w blok try except i mam screen 
    
    driver.switch_to_alert().accept() <- przełączenie sie na okenku alertu o klikniecie na akcept 
    można odzucic alert komend dismiss(), mozemy wpisac tekst jak jest input send_keys()  lub 'text'
    
    for handle in driver.window_handles:  <- iteracja po wszytskich otworzonych okanch
        driver.switch_to_window(handle)   >- przełaczanei sie po wszytskich okanch 
        
    set_window_position(0,0)  < - ustawienie pozycji na stronie
    set_window_rect(x=None, y=None, width=None, height=None) <- ustawieni pozycji i wielkosci okna
    set_window_size(width, height, windowHandle='current') <- ustawienie wiekosci okna
    ^^ do tego sa metody get np get_window_position(windowHandle='current') 
    
    driver.minimize_window()  <- kliknięcie na "-" schowanie strony
    driver.fullscreen_window() <- pełny ekran tak jak np na you tube
    driver.maximize_window()  <- klikniecie w kwadracik - maksymalna wielkosc taba
    
    driver.forward()
    driver.back()
    driver.refresh()
    
    driver.delete_all_cookies()
    driver.delete_cookie('cookie_name')
    driver.add_cookie({‘name’ : ‘foo’, ‘value’ : ‘bar’, ‘path’ : ‘/’, ‘secure’:True})
    driver.get_cookies()
    driver.get_cookie('cookie_name')
    
    chrome_options = webdriver.ChromeOptions()
    chrome_options.add_argument('--headless')  <- no popup display - wszytsko laduje sie jedynie do pamieci, przegladarka nie odpala się w trybie view
    chrome_options.add_argument('--window-size=1920x1080')
    self.driver = webdriver.Chrome(options=chrome_options)
    
    element = WebDriverWait(self.driver, 10).until(EC.visibility_of_element_located((By.ID, "documentation")))
    ActionChains(self.driver).move_to_element(element).perform()  <- 'ActionChains' nasladuje ruch myszki i przesuwa sie na 'element'
    aby byl wywolany np hover. 'perform()' powoduje wywolanie wszystkich czynnosci w zadeklarowanej kolejnosci jedn po drugiej
    locator = "#documentation > ul > li.tier-2.super-navigation > p.download-buttons > a:nth-child(1)"
    py3docbutton = WebDriverWait(self.driver, 10).until(EC.visibility_of_element_located((By.CSS_SELECTOR, locator)))
    <- czekamy az element sie pojawi max 10sek
    
    assert 'No results found.' in result_elem.get_attribute('innerHTML')  <- czy tekst zawiera sie w innerHTML
    assert py3docbutton.text == 'Python 3.x Docs'  <- jest to skrutowy zapis i nie trzeba stosowac selt.assertEqual()
   

##### 9. Event listener:  <- wykonanie metody przed i po danej funkcji

    1. AbstractEventListener <- super class of event
    
    class MyListener(AbstractEventListener):
    def before_navigate_to(self, url, driver):
        print("Log: Before navigate to {}".format(url))
    def after_navigate_to(self, url, driver):
        print("Log: After navigate to {}".format(url))
        
    2. EventFirimgWebDriver owapowuje dana instancje webDriver niestatndardowym zachowanie Event sub-class
    
    driver = webdriver.Chrome()
    ef_driver = EventFiringWebDriver(driver, MyListener())
    ef_driver.get("http://python.org")
    
##### 10. Page Object Model:

    1. class BasePage:  with __init__ driver + build metod
    2. np class HomePage(BasePage):
    3. Test with setup class and next test class - those class are inherit from setup class
    
Moj pomysł na  arhitekture:

- drivers
    - *.exe
- reports
   - currentData e.g.[20200427]
       - nameRaport+curentDatetime.html e.g.[name_2020-04-28_13-37-59.html]
       - screenshots
- resourses
    - page_object
        - *.py
        -  ...
        - *.py
    - testdata.py
    - locators.py
- tests
     - suiteAllTests.py
     - test*.py
     -  ...
     - test*.py
     
        
##### 11. Wzorce projektowe:

    SOLID
    KISS
    DRY
    YAGNI
    generalnie jedna linijka kodu robi tylko jedą żecz. unikamy dublikowania kodu i powielania go, nie importujemy czego 
    do puki tego nie potrzebujey
    
##### 12. ustawiamy setup jako classa > setUpClass(cls):
  
    @classmethod
    def setUpClass(cls) -> None:
        chrome_options = webdriver.ChromeOptions()
        chrome_options.add_argument('--ignore-certificate-errors')
        cls.driver = webdriver.Chrome(Dev.CHROME_PATH, options=chrome_options)
        cls.driver.maximize_window()
        
    @classmethod
    def tearDownClass(cls):
        cls.driver.close()    <-- warto ustawić poniewaz jak sie wysypie test to okno zostanie zamkniete
    
    dzieki temu nie musimy ustawić dziczenie jako super().setUp() oraz testy bedą szybsze
    
    > niestety u mnie nie dziala to ze zwgledu na to ze przy blednym pierszym tescie drugi jest odpalany z tej samej sesji a nie nowej co powoduje błedy
    
 ##### 13. pomijanie testów dekoratorem skip:
 
     @unittest.skip('I must search solution this test case')
     
##### 14. Suite


def suite():
    suite = unittest.TestSuite()
    # Home page tests
    suite.addTest(TestHome('test_TC001_py3_doc_button'))
    suite.addTest(TestHome('test_TC002_blahblah_search'))
    suite.addTest(TestHome('test_TC004_assert_title'))
    # About page tests 
    suite.addTest(TestAbout('test_TC003_verify_upcoming_events_section_present'))
    suite.addTest(TestAbout('test_TC005_assert_url'))
    return suite


##### 15. Concurrency test - odpalanie testów równocześnie: niedziała na windows :(

https://github.com/cgoldberg/concurrencytest


pip install concurrencytest

from concurrencytest import ConcurrentTestSuite, fork_for_tests

 runner = unittest.TextTestRunner(verbosity=2)
suite = suite()
concurrent_suite = ConcurrentTestSuite(suite, fork_for_tests(2))
runner.run(concurrent_suite)
  
##### 16. Selenium Grid:

* Running tests remotely, not locally
* Running tests on different machines against different browsers in parallel

pobrac grid selenium-server https://www.selenium.dev/downloads/

odpalić server ```java -jar selenium-server-standalone-3.141.59.jar -role hub -port 5000```   - wpisujemy nr wersji.

konfiguracja dodatkowa huba poprzez: ```java -jar selenium-server-standalone-3.141.59.jar -role hub -cleanUpCycle 5000 -timeout 30```

dodanie testów do naszego serwera w nowym oknie terminala: ``` java -jar selenium-server-standalone-3.141.59.jar -role node -hub http://192.168.8.103:5000/grid/register/ -timeout 30 -maxSession 5```

* raczej można dodać nowe instancje do grid poprez wirtualną maszę

odpalanie driver zdalnego > ```driver = webdriver.Remote(command_executor='http://192.168.8.103:5000/wd/hub', desired_capabilities={'browserName': 'chrome'})```

##### 17. headless for grid - odpalanei w tle bez open browser - jedynie nagłówki są pobierane. Są szybsze i mniej pamięci zjadają


options = webdriver.ChromeOptions()
options.set_headless()
driver = webdriver.Remote(command_executor = 'http://192.168.8.103:5000/wd/hub', 
                        desired_capabilities = options.to_capabilities())



options = webdriver.FirefoxOptions()
options.set_headless()
driver = webdriver.Remote(command_executor = 'http://192.168.8.103:5000/wd/hub', 
                        desired_capabilities = optionss.to_capabilities())

##### 18. grid + Docker section 6.30 

> bardzo ważne ponieważ mozemy uruchomic testy w srodowisku z konkrtena przegladarka i jej wersja poprzez obraz systemu 

##### 19. testy na FF i Chrome:

* proponowano duplikowanie plików z testami pod konkretną przeglądarką i zmianę jedynie drivera
* wykonano osobne suite dla ff i chrome
* nastepnie zrobiono allSuite in one > zastosowwano importy dwuch suite i dodano do Suite() dwa zestawy 

##### 20. możemy budować suite z innych suite

import unittest
import suite_all_tests_chrome
import suite_all_tests_firefox

if __name__ == '__main__':
runner = unittest.TextTestRunner(verbosity=2)
suite = unittest.TestSuite()
suite_chrome = suite_all_tests_chrome.suite()
suite_firefox= suite_all_tests_firefox.suite()
suite.addTests(suite_chrome)
suite.addTests(suite_firefox)
runner.run(suite)

##### 21. class unittest.TestResult  to obiekt który ma bardzo dużo metod

np możemy z konsoli odpalic result lub u mnie runner.testsRun i dostaniemy liczbe testów

- result.testsRun
- result.failures
- result.errors
- result.skipped

##### 22. pytest -v nazwa_pliku.py  - pytest szuka po nazwie tests_... [trzeba pamietac o prawidłowcych nazwach]

`pip install pytest`

większe możliwości niż unittest - wizuanei nawet w konsoli wyniki sa % i kolorowe.

odpalenie za pomocą `pytest -v`   - znajdzie wszytskie pliki z początkiem test_ i je odpali

##### 23. pytest-json  do generowania raportu w formacie json

json output example with pytest 

`pip install pytest-json`
`pytest -v nazwa_pliku.py --json=report.json`    >> nazwa pliku to może  byc tylko test a nie kolecja suit

najepiej wszytskie odpalić za pomocą `pytest -v`

##### 24. pytest-html  

`pip install pytest-html`

`pytest -v result.py --html=pytest_report.html --self-contained-html`  >> nazwa pliku to może  byc tylko test a nie kolecja suit
