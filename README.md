## Assignment #2. Automation of the service delivery
## Prerequisites

1. Ensure Python is installed on your system. If not, you can download and install it from the [Python official website](https://www.python.org/). Additionally, choose a Python editor or Integrated Development Environment (IDE) according to your preference. Popular options include [PyCharm](https://www.jetbrains.com/pycharm/download/?section=windows) and [Visual Studio Code](https://code.visualstudio.com/download). Make sure to download the latest Python version; for this assignment, Python 3.8 or newer is required, with Python 3.11.8 being the version used.

2. Install Selenium using Python's package manager. Open your command prompt or terminal and execute the following command:
```bash
pip install selenium
```

3. Set up your Python environment using pipenv. To install pipenv, run the following command in your terminal or command line:
```bash
pip install pipenv
```

4. Configure WebDriver setup. Ensure you have the latest versions of Google Chrome and Mozilla Firefox installed. You may choose other browsers compatible with Selenium WebDriver as well. Additionally, download and install the latest WebDriver executables for these browsers: [ChromeDriver](https://chromedriver.chromium.org/downloads) for Chrome and [geckodriver](https://github.com/mozilla/geckodriver/releases) for Firefox.

5. Execute Python scripts. Run the `test_search_feature.py` file by clicking on it and selecting *Run* from the context menu. This action will display the test case process and its results.
```
import unittest
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.common.exceptions import StaleElementReferenceException
import time


class EcCouncil(unittest.TestCase):
    def setUp(self):
        # Set up the webdriver instance for Chrome browser
        self.browser_driver = webdriver.Chrome()
        # Wait for the elements implicitly during the specified time
        self.browser_driver.implicitly_wait(5)
        # Go to the search page on the Dribble application
        self.browser_driver.get("https://www.eccouncil.org/?s=")

    def test_search_mechanism(self):
        # The searched item
        search_for_course = "pen testing"

        try:
            # Wait for clickable search input
            search_input = WebDriverWait(self.browser_driver, 5).until(EC.element_to_be_clickable((By.CLASS_NAME, "the7-search-form__input")))
            # Waiting for the search input to become clickable
            search_input.send_keys(search_for_course)
            time.sleep(3)

            # Initiate the searching with the Enter key
            search_input.send_keys(Keys.RETURN)
            time.sleep(3)

            # Verify the searched item in page source
            assert search_for_course in self.browser_driver.page_source

        except StaleElementReferenceException:
            # Wait for the page to reload or element to become clickable again
            search_input = WebDriverWait(self.browser_driver, 5).until(EC.element_to_be_clickable((By.CLASS_NAME, "the7-search-form__input")))
            search_input.send_keys(search_for_course)
            time.sleep(3)

            search_input.send_keys(Keys.RETURN)
            time.sleep(3)

            assert search_for_course in self.browser_driver.page_source

    def tearDown(self):
        # Close webdriver after test
        self.browser_driver.quit()


if __name__ == "__main__":
    # Run a test case
    unittest.main()

