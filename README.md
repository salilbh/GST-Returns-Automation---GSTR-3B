# GSTAutomation
Automated filing of Return GSTR-3B with the data for multiple states
Recorded Steps
________________________________________
File "Steps to depict use of program.docx" contains all the steps and information to help describe the steps in using the project

The project was created for BFSI Sector to file GST return 3B through system for accuracy and speed as the returns are filed for each state separately and Copy pasting is not allowed in GST return filing forms. 

Libraries Used:

•	Easygui – for graphical user interface
•	Selenium – For web driver automation
•	CSV – to import csv data
•	PIL – to capture captcha for input and future use in modelling

--------------------------------------------------------------------------------------------------------------------------------------
Code
--------------------------------------------------------------------------------------------------------------------------------------


from easygui import *
import easygui
import time
from selenium import webdriver
from PIL import Image
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.webdriver.support.select import Select
import csv
from collections import defaultdict
import os
from selenium.common.exceptions import NoSuchElementException
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.common.keys import Keys
columns = defaultdict(list)


a = 1

while a==1:

    mainMenu = buttonbox("GST Automation - Fill Returns Data", image=os.getcwd() + "\VBLOGO.png",choices=["Step 1 - Enter all User IDs", "Step 2 - Enter all Passwords","Step 3 - Select Financial Year", "Step 4 - Select Month", "Select CSV File & Start Process"])

    if (mainMenu == "Step 1 - Enter all User IDs"):
        UserIDs = textbox(msg="Enter User IDs of all States separated by coma without space")
        UserIDs = UserIDs.split(',')
        if UserIDs == ['']: print('UserIDs list is empty')
        else: UserID = UserIDs
        

    elif (mainMenu == "Step 2 - Enter all Passwords"):
        Password = passwordbox(msg="Enter Passwords of all States separated by coma without space")
        Password = Password.split(',')
        if Password == ['']: print('Password list is empty')
        else: Passwords = Password
        

    elif (mainMenu == "Step 3 - Select Financial Year"):
        FinYear = choicebox("Select Relevant Financial Year", "Financial Years", ["2017-18","2018-19","2019-20"])
        print(FinYear)

    elif (mainMenu == "Step 4 - Select Month"):
        Month = choicebox("Select Relevant Month", "Month", ["January","February","March","April","May","June","July","August","September","October","November","December"])
        print(Month)

    elif (mainMenu == "Select CSV File & Start Process"):
        filename = easygui.fileopenbox(msg="select csv file format", title="GSTR3B Login & Return Filing Data")
        msgbox(msg="Data Captured. Ready to Go!", title="Step 5 - Start Process", ok_button="Go")
        print(filename)
        a = 2




# -----------------------------------------------------------------------CSV File select------------------------------------------------


with open(filename) as dataset:
    reader = csv.DictReader(dataset)
    for row in reader:
        for (k, v) in row.items():
            columns[k].append(v)

UserID = columns['User ID']
Passwords = columns['Password']
AaTV = columns['tax on outward and reverse charge inward suplies - (a) Outward taxable supplies (other than zero rated nil rated and exempted) - taxable value']
AaIGST = columns['tax on outward and reverse charge inward suplies - (a) Outward taxable supplies (other than zero rated nil rated and exempted) - Integrated Tax']
AaCGST = columns['tax on outward and reverse charge inward suplies - (a) Outward taxable supplies (other than zero rated nil rated and exempted) - Central Tax']
AaSGST = columns['tax on outward and reverse charge inward suplies - (a) Outward taxable supplies (other than zero rated nil rated and exempted) - State/UT Tax']
AaCess = columns['tax on outward and reverse charge inward suplies - (a) Outward taxable supplies (other than zero rated nil rated and exempted) - CESS']
AbTV = columns['tax on outward and reverse charge inward suplies - (b) Outward taxable supplies (zero rated ) - taxable value']
AbIGST = columns['tax on outward and reverse charge inward suplies - (b) Outward taxable supplies (zero rated ) - Integrated Tax']
AbCess = columns['tax on outward and reverse charge inward suplies - (b) Outward taxable supplies (zero rated ) - CESS']
AcTV = columns['tax on outward and reverse charge inward suplies - (c) Other outward supplies (Nil rated exempted) - taxable value']
AdTV = columns['tax on outward and reverse charge inward suplies - (d) Inward supplies (liable to reverse charge) - taxable value']
AdIGST = columns['tax on outward and reverse charge inward suplies - (d) Inward supplies (liable to reverse charge) - Integrated Tax']
AdCGST = columns['tax on outward and reverse charge inward suplies - (d) Inward supplies (liable to reverse charge) - Central Tax']
AdSGST = columns['tax on outward and reverse charge inward suplies - (d) Inward supplies (liable to reverse charge) - State/UT Tax']
AdCess = columns['tax on outward and reverse charge inward suplies - (d) Inward supplies (liable to reverse charge) - CESS']
AeTV = columns['tax on outward and reverse charge inward suplies - (e) Non-GST outward supplies - taxable value']
B1IGST = columns['Eligible ITC - (1) Import of goods - Integrated Tax']
B1Cess = columns['Eligible ITC - (1) Import of goods - CESS']
B2IGST = columns['Eligible ITC - (2) Import of services - Integrated Tax']
B2Cess = columns['Eligible ITC - (2) Import of services - CESS']
B3IGST = columns['Eligible ITC - (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - Integrated Tax']
B3CGST = columns['Eligible ITC - (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - Central Tax']
B3SGST = columns['Eligible ITC - (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - State/UT Tax']
B3Cess = columns['Eligible ITC - (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - CESS']
B4IGST = columns['Eligible ITC - (4) Inward supplies from ISD - Integrated Tax']
B4CGST = columns['Eligible ITC - (4) Inward supplies from ISD - Central Tax']
B4SGST = columns['Eligible ITC - (4) Inward supplies from ISD - State/UT Tax']
B4Cess = columns['Eligible ITC - (4) Inward supplies from ISD - CESS']
B5IGST = columns['Eligible ITC - (5) All other ITC - Integrated Tax']
B5CGST = columns['Eligible ITC - (5) All other ITC - Central Tax']
B5SGST = columns['Eligible ITC - (5) All other ITC - State/UT Tax']
B5Cess = columns['Eligible ITC - (5) All other ITC - CESS']
Bb1IGST = columns['Eligible ITC - (1) As per Rule 42 & 43 of CGST/SGST rules - Integrated Tax']
Bb1CGST = columns['Eligible ITC - (1) As per Rule 42 & 43 of CGST/SGST rules - Central Tax']
Bb1SGST = columns['Eligible ITC - (1) As per Rule 42 & 43 of CGST/SGST rules - State/UT Tax']
Bc1Cess = columns['Eligible ITC - (1) As per Rule 42 & 43 of CGST/SGST rules - CESS']
Bc2IGST = columns['Eligible ITC - (2) Others - Integrated Tax']
Bc2CGST = columns['Eligible ITC - (2) Others - Central Tax']
Bc2SGST = columns['Eligible ITC - (2) Others - State/UT Tax']
Bc2Cess = columns['Eligible ITC - (2) Others - CESS']
Bd1IGST = columns['Eligible ITC - (1) As per section 17(5) - Integrated Tax']
Bd1CGST = columns['Eligible ITC - (1) As per section 17(5) - Central Tax']
Bd1SGST = columns['Eligible ITC - (1) As per section 17(5) - State/UT Tax']
Bd1Cess = columns['Eligible ITC - (1) As per section 17(5) - CESS']
Bd2IGST = columns['CEligible ITC - (2) Others - Integrated Tax']
Bd2CGST = columns['CEligible ITC - (2) Others - Central Tax']
Bd2SGST = columns['CEligible ITC - (2) Others - State/UT Tax']
Bd2Cess = columns['CEligible ITC - (2) Others - CESS']





# -----------------------------------------------------------------------CSV File End---------------------------------------------------


Users = len(UserID)

#-----------------------------------------------------------------------LOG IN----------------------------------------------------------
UIDs = 0
while UIDs <= Users:
    driver = webdriver.Chrome(executable_path=os.getcwd() + "\chromedriver.exe")
    Wait = WebDriverWait(driver, 15)
    driver.implicitly_wait(5)

    try:
        driver.set_page_load_timeout(15)
        print("Opening website")
        driver.get("https://services.gst.gov.in/services/login")
        print("Login Page Successfully opened")

    except TimeoutException as ex:
        isrunning = 0
        print("Exception has been thrown. " + str(ex))
        driver.get("https://services.gst.gov.in/services/login")
        print("Refreshing")
        pass

    driver.maximize_window()

    # time.sleep(3)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    UserName = WebDriverWait(driver, 5).until(EC.visibility_of_element_located((By.ID, "username")));
    UserName.send_keys(UserID[UIDs])
    Password = WebDriverWait(driver, 5).until(EC.visibility_of_element_located((By.ID, "user_pass")));
    Password.send_keys(Passwords[UIDs])
    #print(UIDs)
    #print(Passwords[UIDs])
    #pwd = Passwords[UIDs]


    driver.implicitly_wait(5)
    time.sleep(1)


    CaptchaVisibility = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, '//*[@id="imgCaptcha"]')));
    CaptchaImage = driver.find_element_by_xpath('//*[@id="imgCaptcha"]')
    location = CaptchaImage.location;
    size = CaptchaImage.size;
    driver.save_screenshot("Captcha.png");
    x = location['x'];
    y = location['y'];
    width = location['x'] + size['width'];
    height = location['y'] + size['height'];
    im = Image.open('Captcha.png')
    im = im.crop((int(x), int(y), int(width), int(height)))
    im.save("Captcha.png")
    Captchavalue = easygui.enterbox(msg="Enter Captch", image="captcha.png")
    driver.find_element_by_id("captcha").send_keys(Captchavalue)
    img = Image.open("captcha.png")
    img.save(Captchavalue + ".png")
    driver.find_element_by_css_selector(".btn-primary").click()  # Submit user password captcha

    try:
        if driver.find_element_by_xpath('//*[contains(text(), "Enter valid letters shown in the image below")]').is_displayed():
            print("Element found")
            CaptchaVisibility = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, '//*[@id="imgCaptcha"]')));
            CaptchaImage = driver.find_element_by_xpath('//*[@id="imgCaptcha"]')
            location = CaptchaImage.location;
            size = CaptchaImage.size;
            driver.save_screenshot("Captcha.png");
            x = location['x'];
            y = location['y'];
            width = location['x'] + size['width'];
            height = location['y'] + size['height'];
            im = Image.open('Captcha.png')
            im = im.crop((int(x), int(y), int(width), int(height)))
            im.save("Captcha.png")
            Captchavalue = easygui.enterbox(msg="Enter Captch", image="captcha.png")
            driver.find_element_by_id("captcha").clear()
            driver.find_element_by_id("captcha").send_keys(Captchavalue)
            img = Image.open("captcha.png")
            img.save(Captchavalue + ".png")
        else:
            print("Element not found")

    except NoSuchElementException:
        pass


    try:
        if driver.find_element_by_xpath('//*[contains(text(), "Enter valid Letters shown")]').is_displayed():
            print("Element found")
            Password.clear()
            Password.send_keys(Passwords[UIDs])
            CaptchaVisibility = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, '//*[@id="imgCaptcha"]')));
            CaptchaImage = driver.find_element_by_xpath('//*[@id="imgCaptcha"]')
            location = CaptchaImage.location;
            size = CaptchaImage.size;
            driver.save_screenshot("Captcha.png");
            x = location['x'];
            y = location['y'];
            width = location['x'] + size['width'];
            height = location['y'] + size['height'];
            im = Image.open('Captcha.png')
            im = im.crop((int(x), int(y), int(width), int(height)))
            im.save("Captcha.png")
            Captchavalue = easygui.enterbox(msg="Enter Captch", image="captcha.png")
            driver.find_element_by_id("captcha").clear()
            driver.find_element_by_id("captcha").send_keys(Captchavalue)
            img = Image.open("captcha.png")
            img.save(Captchavalue + ".png")
        else:
            print("Element not found")

    except NoSuchElementException:
        pass

    driver.find_element_by_css_selector(".btn-primary").click()  # Submit user password captcha
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))


    try:
        driver.set_page_load_timeout(10)
        print("Clicking Return Dashboard")
        element = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".btn-primary.pad-l-50.pad-r-50")))
        element.click();  # Click Return Dashboard
        # driver.find_element_by_css_selector(".btn-primary.pad-l-50.pad-r-50").click()
        # time.sleep(3)

    except TimeoutException as ex:
        isrunning = 0
        print("Exception has been thrown. " + str(ex))
        print("Retrying")
        driver.back()
        time.sleep(2)
        element = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.CSS_SELECTOR, ".btn-primary.pad-l-50.pad-r-50")))
        element.click();  # Click Return Dashboard

        # driver.find_element_by_css_selector(".btn-primary.pad-l-50.pad-r-50").click()
        pass






    time.sleep(2)

    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))

    print("Selecting Financial Year")
    select = Select(driver.find_element_by_name('fin'))
    select.select_by_visible_text(FinYear)
    print("Updated Financial Year")
    driver.implicitly_wait(5)
    select2 = Select(driver.find_element_by_name('mon'))
    select2.select_by_visible_text(Month)
    print("Updated Month")
    # time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    driver.find_element_by_css_selector(".btn-primary.srchbtn").click()  # Click Search in Return DashBoard
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
# --------------------------------------------------------------------------------------------------------------------------------------
    driver.find_element_by_css_selector("body > div.content-wrapper.fade-ng-cloak > div.container > div > div:nth-child(2) > div.card.text-center > div:nth-child(3) > div:nth-child(2) > div:nth-child(1) > div > div.ct > div > div:nth-child(1) > button").click()
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    driver.find_element_by_css_selector("div.modal-footer:nth-child(3) > button:nth-child(1)").click()  # Click Ok

    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    driver.find_element_by_xpath('//*[@id="nilno"]').click()  # Answer to point a
    driver.find_element_by_xpath('//*[@id="outwardyes"]').click()  # Answer to point b
    driver.find_element_by_xpath('//*[@id="interstateno"]').click()  # Answer to point c
    driver.find_element_by_xpath('//*[@id="itcyes"]').click()  # Answer to point d
    driver.find_element_by_xpath('//*[@id="exemptno"]').click()  # Answer to point e
    driver.find_element_by_xpath('//*[@id="interestno"]').click()  # Answer to point f
    driver.find_element_by_xpath('//*[@id="latefeeno"]').click()  # Answer to point g

    driver.find_element_by_css_selector("div.mar-b:nth-child(2) > div:nth-child(1) > button:nth-child(2)").click()  # Click Next
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))

    driver.find_element_by_css_selector(".card > div:nth-child(1) > div:nth-child(1) > a:nth-child(1) > div:nth-child(1) > p:nth-child(1)").click()  # Click 3.1 tax on outward and reverse charge inward suplies
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    value1 = AaTV[UIDs]
    print(value1)


#-----------------------------------------clear all fields----------------------------------------------------------




    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.txval']").clear()  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.iamt']").clear()  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - Integrated Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.camt']").clear()  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - Central Tax (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.samt']").clear()  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - State/UT Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.csamt']").clear()  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - CESS (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_zero.txval']").clear()  # (b) Outward taxable supplies (zero rated ) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_zero.iamt']").clear()  # (b) Outward taxable supplies (zero rated ) - Integrated Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_zero.csamt']").clear()  # (b) Outward taxable supplies (zero rated ) - CESS (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_nil_exmp.txval']").clear()  # (c) Other outward supplies (Nil rated, exempted) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.txval']").clear()  # (d) Inward supplies (liable to reverse charge) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.iamt']").clear()  # (d) Inward supplies (liable to reverse charge) - Integrated Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.camt']").clear()  # (d) Inward supplies (liable to reverse charge) - Central Tax (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.samt']").clear()  # (d) Inward supplies (liable to reverse charge) - State/UT Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.csamt']").clear()  # (d) Inward supplies (liable to reverse charge) - CESS (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_nongst.txval']").clear()  # (e) Non-GST outward supplies -  taxable value

# -----------------------------------------Fill Data in fields----------------------------------------------------------

    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.txval']").send_keys(AaTV[UIDs])  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.iamt']").send_keys(AaIGST[UIDs])  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - Integrated Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.camt']").send_keys(AaCGST[UIDs])  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - Central Tax (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.samt']").send_keys(AaSGST[UIDs])  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - State/UT Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_det.csamt']").send_keys(AaCess[UIDs])  # (a) Outward taxable supplies (other than zero rated, nil rated and exempted) - CESS (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_zero.txval']").send_keys(AbTV[UIDs])  # (b) Outward taxable supplies (zero rated ) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_zero.iamt']").send_keys(AbIGST[UIDs])  # (b) Outward taxable supplies (zero rated ) - Integrated Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_zero.csamt']").send_keys(AbCess[UIDs])  # (b) Outward taxable supplies (zero rated ) - CESS (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_nil_exmp.txval']").send_keys(AcTV[UIDs])  # (c) Other outward supplies (Nil rated, exempted) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.txval']").send_keys(AdTV[UIDs])  # (d) Inward supplies (liable to reverse charge) - taxable value
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.iamt']").send_keys(AdIGST[UIDs])  # (d) Inward supplies (liable to reverse charge) - Integrated Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.camt']").send_keys(AdCGST[UIDs])  # (d) Inward supplies (liable to reverse charge) - Central Tax (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.samt']").send_keys(AdSGST[UIDs])  # (d) Inward supplies (liable to reverse charge) - State/UT Tax
    driver.find_element_by_css_selector("[data-ng-model='sup_details.isup_rev.csamt']").send_keys(AdCess[UIDs])  # (d) Inward supplies (liable to reverse charge) - CESS (₹)
    driver.find_element_by_css_selector("[data-ng-model='sup_details.osup_nongst.txval']").send_keys(AeTV[UIDs])  # (e) Non-GST outward supplies -  taxable value

    #time.sleep(5)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))

    driver.find_element_by_css_selector("button.pull-right:nth-child(1)").click()  # Click Confirm
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    driver.find_element_by_css_selector(".card > div:nth-child(1) > div:nth-child(2) > a:nth-child(1) > div:nth-child(1) > p:nth-child(1)").click()  # click eligible ITC
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))

# -----------------------------------------clear all fields----------------------------------------------------------

    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(2) > td:nth-child(2) > input:nth-child(1)").clear()  # (1) Import of goods - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(2) > td:nth-child(5) > input:nth-child(1)").clear()  # (1) Import of goods - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(3) > td:nth-child(2) > input:nth-child(1)").clear()  # (2) Import of services - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(3) > td:nth-child(5) > input:nth-child(1)").clear()  # (2) Import of services - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(2) > input:nth-child(1)").clear()  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(3) > input:nth-child(1)").clear()  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(4) > input:nth-child(1)").clear()  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(5) > input:nth-child(1)").clear()  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(2) > input:nth-child(1)").clear()  # (4) Inward supplies from ISD - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(3) > input:nth-child(1)").clear()  # (4) Inward supplies from ISD - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(4) > input:nth-child(1)").clear()  # (4) Inward supplies from ISD - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(5) > input:nth-child(1)").clear()  # (4) Inward supplies from ISD - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(2) > input:nth-child(1)").clear()  # (5) All other ITC - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(3) > input:nth-child(1)").clear()  # (5) All other ITC - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(4) > input:nth-child(1)").clear()  # (5) All other ITC - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(5) > input:nth-child(1)").clear()  # (5) All other ITC - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(2) > input:nth-child(1)").clear()  # (1) As per Rule 42 & 43 of CGST/SGST rules - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(3) > input:nth-child(1)").clear()  # (1) As per Rule 42 & 43 of CGST/SGST rules - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(4) > input:nth-child(1)").clear()  # (1) As per Rule 42 & 43 of CGST/SGST rules - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(5) > input:nth-child(1)").clear()  # (1) As per Rule 42 & 43 of CGST/SGST rules - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(2) > input:nth-child(1)").clear()  # (2) Others - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(3) > input:nth-child(1)").clear()  # (2) Others - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(4) > input:nth-child(1)").clear()  # (2) Others - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(5) > input:nth-child(1)").clear()  # (2) Others - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(2) > input:nth-child(1)").clear()  # (1) As per section 17(5) - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(3) > input:nth-child(1)").clear()  # (1) As per section 17(5) - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(4) > input:nth-child(1)").clear()  # (1) As per section 17(5) - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(5) > input:nth-child(1)").clear()  # (1) As per section 17(5) - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(2) > input:nth-child(1)").clear()  # (2) Others - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(3) > input:nth-child(1)").clear()  # (2) Others - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(4) > input:nth-child(1)").clear()  # (2) Others - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(5) > input:nth-child(1)").clear()  # (2) Others - CESS (₹)

# -----------------------------------------Fill Data in fields----------------------------------------------------------

    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(2) > td:nth-child(2) > input:nth-child(1)").send_keys(B1IGST[UIDs])  # (1) Import of goods - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(2) > td:nth-child(5) > input:nth-child(1)").send_keys(B1Cess[UIDs])  # (1) Import of goods - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(3) > td:nth-child(2) > input:nth-child(1)").send_keys(B2IGST[UIDs])  # (2) Import of services - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(3) > td:nth-child(5) > input:nth-child(1)").send_keys(B2Cess[UIDs])  # (2) Import of services - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(2) > input:nth-child(1)").send_keys(B3IGST[UIDs])  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(3) > input:nth-child(1)").send_keys(B3CGST[UIDs])  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(4) > input:nth-child(1)").send_keys(B3SGST[UIDs])  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(4) > td:nth-child(5) > input:nth-child(1)").send_keys(B3Cess[UIDs])  # (3) Inward supplies liable to reverse charge (other than 1 & 2 above) - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(2) > input:nth-child(1)").send_keys(B4IGST[UIDs])  # (4) Inward supplies from ISD - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(3) > input:nth-child(1)").send_keys(B4CGST[UIDs])  # (4) Inward supplies from ISD - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(4) > input:nth-child(1)").send_keys(B4SGST[UIDs])  # (4) Inward supplies from ISD - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(5) > td:nth-child(5) > input:nth-child(1)").send_keys(B4Cess[UIDs])  # (4) Inward supplies from ISD - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(2) > input:nth-child(1)").send_keys(B5IGST[UIDs])  # (5) All other ITC - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(3) > input:nth-child(1)").send_keys(B5CGST[UIDs])  # (5) All other ITC - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(4) > input:nth-child(1)").send_keys(B5SGST[UIDs])  # (5) All other ITC - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(6) > td:nth-child(5) > input:nth-child(1)").send_keys(B5Cess[UIDs])  # (5) All other ITC - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(2) > input:nth-child(1)").send_keys(Bb1IGST[UIDs])  # (1) As per Rule 42 & 43 of CGST/SGST rules - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(3) > input:nth-child(1)").send_keys(Bb1CGST[UIDs])  # (1) As per Rule 42 & 43 of CGST/SGST rules - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(4) > input:nth-child(1)").send_keys(Bb1SGST[UIDs])  # (1) As per Rule 42 & 43 of CGST/SGST rules - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(8) > td:nth-child(5) > input:nth-child(1)").send_keys(Bc1Cess[UIDs])  # (1) As per Rule 42 & 43 of CGST/SGST rules - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(2) > input:nth-child(1)").send_keys(Bc2IGST[UIDs])  # (2) Others - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(3) > input:nth-child(1)").send_keys(Bc2CGST[UIDs])  # (2) Others - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(4) > input:nth-child(1)").send_keys(Bc2SGST[UIDs])  # (2) Others - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(9) > td:nth-child(5) > input:nth-child(1)").send_keys(Bc2Cess[UIDs])  # (2) Others - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(2) > input:nth-child(1)").send_keys(Bd1IGST[UIDs])  # (1) As per section 17(5) - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(3) > input:nth-child(1)").send_keys(Bd1CGST[UIDs])  # (1) As per section 17(5) - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(4) > input:nth-child(1)").send_keys(Bd1SGST[UIDs])  # (1) As per section 17(5) - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(12) > td:nth-child(5) > input:nth-child(1)").send_keys(Bd1Cess[UIDs])  # (1) As per section 17(5) - CESS (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(2) > input:nth-child(1)").send_keys(Bd2IGST[UIDs])  # (2) Others - Integrated Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(3) > input:nth-child(1)").send_keys(Bd2CGST[UIDs])  # (2) Others - Central Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(4) > input:nth-child(1)").send_keys(Bd2SGST[UIDs])  # (2) Others - State/UT Tax (₹)
    driver.find_element_by_css_selector(".table > tbody:nth-child(2) > tr:nth-child(13) > td:nth-child(5) > input:nth-child(1)").send_keys(Bd2Cess[UIDs])  # (2) Others - CESS (₹)
    #time.sleep(5)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    driver.find_element_by_css_selector("button.pull-right:nth-child(1)").click()  # Click Confirm
    time.sleep(2)
    WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    driver.find_element_by_css_selector("div.row:nth-child(12) > div:nth-child(1) > button:nth-child(2)").click()  # Click Save GSTR3B
    time.sleep(2)
# --------------------------------------------------------------------------------------------------------------------------------------

    # #time.sleep(5)
    # WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    # driver.find_element_by_xpath("/html/body/div[1]/ng-include[1]/header/div[2]/div/div/ul/li/div/a").click()
    # #time.sleep(5)
    # WebDriverWait(driver, 15).until(EC.invisibility_of_element((By.CSS_SELECTOR, "div[class='dimmer-holder'] [style='display: none']")))
    # driver.find_element_by_xpath("/html/body/div[1]/ng-include[1]/header/div[2]/div/div/ul/li/div/ul/li[5]/a").click()  # logout step 2


    driver.quit()
    # print("Return filed for : " + UserID)

    os.system("taskkill /f /im chromedriver.exe.exe")
    os.system("taskkill /f /im chrome.exe")

    UIDs += 1
    print(UIDs)
    continue


