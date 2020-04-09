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
    
    WebDriverWait(self.driver, time).until(expected_conditions..... cale mnostwo waitow ustawianych indywidualnie np selenium.webdriver.support.expected_conditions.visibility_of_element_located(locator) - An expectation for checking 
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
    chrome_options.add_argument('headless')  <- no popup display - wszytsko laduje sie jedynei do pamieci
    chrome_options.add_argument('window-size=1920x1080')
    self.driver = webdriver.Chrome(options=chrome_options)
    
    element = WebDriverWait(self.driver, 10).until(EC.visibility_of_element_located((By.ID, "documentation")))
    ActionChains(self.driver).move_to_element(element).perform()  <- 'ActionChains' nasladuje ruch myszki i przesuwa sie na 'element' aby byl wywolany np hover. 'perform()' powoduje wywolanie wszystkich czynnosci w zadeklarowanej kolejnosci jedn po drugiej
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
