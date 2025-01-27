# qa_selenium_test.py

import time
import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

@pytest.fixture(scope="module")
def setup():
    # Set up the Chrome WebDriver
    driver = webdriver.Chrome()
    yield driver
    driver.quit()

def test_search_functionality(setup):
    driver = setup
    driver.get("https://www.lambdatest.com/selenium-playground/table-sort-search-demo")

    # Locate the search box
    search_box = driver.find_element(By.ID, "searchbox")
    
    # Interact with the search box
    search_box.send_keys("New York")
    search_box.send_keys(Keys.RETURN)

    # Wait for results to load
    time.sleep(2)

    # Validate the search results
    results = driver.find_elements(By.XPATH, "//tbody/tr")
    assert len(results) == 5, f"Expected 5 entries, but got {len(results)}"
    
    # Validate total entries
    total_entries = driver.find_element(By.XPATH, "//div[@id='example_info']").text
    assert "24" in total_entries, f"Expected total entries to be 24, but got {total_entries}"

if __name__ == "__main__":
    pytest.main()
