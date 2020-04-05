##### 1. tworzenie wirtualnego srodowiska

`python -m venv env`   trzeba dodać do .gitignore

`.\venv\Scripts\activate`   Actywacja

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
    driver.find_element_by_xpath("//*[text()='Male']//preceding-sibling::input")  <- znajdzi
 elemnt przed tekstem 'Male'  - ponieważ klikanie na sam tekst nic nam nie da, trzeba znalesc input przed tym tekstem
    driver.page_source <- zrodlo strony
    driver.implicitly_wait(30) <- ustawiamy ile mamy czekac maxymalnie na jakikolwiek element na stronie  - unikac poniewaz jest w konflikcie z explicit wait - webdriverWait
    
    WebDriverWait(self.driver, time).until(expected_conditions..... cale mnostwo waitow ustawianych indywidualnie np selenium.webdriver.support.expected_conditions.visibility_of_element_located(locator) - An expectation for checking that an element is present on the DOM of a page and visible.
    
    driver.execute_script(‘return document.title;’)  <- mamy mozliwosc wykonywania codu JS co jest super bo szybko mozna pobrac wartosci z DOM
    driver.execute_script("document.body.style.zoom = '1.7'") <- powiekszenie strony
    
    driver.get_screenshot_as_file('Screenshots/BugsNoKlubURL.png') <- wrzuciłem w blok try except i mam screen 
    
