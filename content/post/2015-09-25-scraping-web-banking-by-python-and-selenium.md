+++
date = "2015-09-25T00:01:23+09:00"
slug = "scrape-netbanking-by-python-and-selenium"
tags = ["Python", "Selenium"]
title = "PythonとSeleniumでネットバンキングをスクレイピングする"

+++

[Python東海 第28回勉強会](http://connpass.com/event/19477/)の発表の中で、Seleniumでお小遣いを稼ぐという発表がありました。
Seleniumは名前は知っているけど使ったことがなかったのでネットバンキング(三菱東京UFJ銀行)で試してみました。

<!--more-->

## Selenium？

[Selenium](http://www.seleniumhq.org/)とはブラウザの操作を自動化するツールで、WebアプリのUIのテストや今回のようにスクレイピングに使用するツールです。

JavaScriptでDOMを組み立てているサイトで活躍します

## 環境

* Mac OSX
* Python 3.5

## 準備

### Seleniumのインストール

```
$ pip install selenium
```

### Chrome driverのダウンロード

Firefoxを使う場合はDriverはSeleniumに入ってるが、Chromeを使用する場合は別途ダウンロードが必要

[ChromeDriver - WebDriver for Chrome](https://sites.google.com/a/chromium.org/chromedriver/downloads)から適当な場所にダウンロードする

## Seleniumのサンプル

### サイトのソースを取得する

Googleのトップページのソースを取得する。

実行するとGoogleトップのタイトルが表示される

```python
import os
import time
from selenium import webdriver

URL = 'http://www.google.com'
DRIVER_PATH = os.path.join(os.path.dirname(__file__), 'chromedriver')

if __name__ == '__main__':
    try:
        browser = webdriver.Chrome(DRIVER_PATH)
        browser.get(URL)
        time.sleep(3) # 画面表を待つ 秒数は適当
        print(browser.title) # タイトル
        print(browser.page_source) # ソース
    finally:
        browser.quit()
```

### Googleでの検索を自動化

「Python」で検索する

```python
import os
import time
from selenium import webdriver

URL = 'http://www.google.com'
DRIVER_PATH = os.path.join(os.path.dirname(__file__), 'chromedriver')

if __name__ == '__main__':
    try:
        browser = webdriver.Chrome(DRIVER_PATH)
        browser.get(URL)
        time.sleep(3)
        search_input = browser.find_element_by_name('q')
        search_input.send_keys('Python')
        search_input.submit()
        time.sleep(3)
        print(browser.title)
        # print(browser.page_source) # ページのソースを取得する場合
    finally:
        browser.quit()
```

### 検索した結果のタイトルを表示

取得したソースをxpathなどで解析できます。

```python
import os
import time
from selenium import webdriver

URL = 'http://www.google.com'
DRIVER_PATH = os.path.join(os.path.dirname(__file__), 'chromedriver')

SEARCH_WORD = 'python'

if __name__ == '__main__':
    try:
        browser = webdriver.Chrome(DRIVER_PATH)
        browser.get(URL)
        time.sleep(3)
        search_input = browser.find_element_by_name('q')
        search_input.send_keys(SEARCH_WORD)
        search_input.submit()
        time.sleep(3)
        print(browser.title)
        titles = browser.find_elements_by_xpath('//h3[@class="r"] ')
        for title in titles:
            print(title.text)
    finally:
        browser.quit()
```

## MUFGにログインして入出金明細を取得する

* コンソールに明細を表示するだけ
* `chromedriver`はカレントディレクトリにある想定

#### お約束

このプログラムを実行して何かあっても責任を取れませんので自己責任でお願いします。


```python
import os
import time
from selenium import webdriver

DRIVER_PATH = os.path.join(os.path.dirname(__file__), 'chromedriver')
MUFG_TOP_URL = 'https://entry11.bk.mufg.jp/ibg/dfw/APLIN/loginib/login?_TRANID=AA000_001'

INFORMATION_TITLE = 'お知らせ - 三菱東京ＵＦＪ銀行'

# MUFGのログイン情報
ID = "your mufg account id"
PASSWORD = "your mufg account password"

try:
    browser = webdriver.Chrome(DRIVER_PATH)
    browser.get(MUFG_TOP_URL)
    time.sleep(3)

    # ログイン
    browser.find_element_by_id('account_id').send_keys(ID)
    browser.find_element_by_id('ib_password').send_keys(PASSWORD)
    browser.find_element_by_xpath('//img[@alt="ログイン"]').click()
    time.sleep(5)

    # お知らせがある場合は既読にする
    # 複数個未読がある場合の処理がまだ試せてないので怪しい・・・
    while True:
        if not browser.title == INFORMATION_TITLE:
            break

        information = browser.find_element_by_xpath(
            '//table[@class="data"]/tbody/tr')
        information.find_element_by_name('hyouzi').click()
        time.sleep(5)

        browser.find_element_by_xpath('//img[@alt="トップページへ"]').click()
        time.sleep(5)

    # 入出金明細画面に移動
    browser.find_element_by_xpath('//img[@alt="入出金明細をみる"]').click()
    time.sleep(5)

    # 明細一覧を取得して表示
    banking_list = browser.find_elements_by_xpath(
        '//table[@class="data yen_nyushutsukin_001"]/tbody/tr')
    for banking in banking_list:
        print(banking.find_element_by_class_name('date').text)
        manages = banking.find_elements_by_class_name('manage')
        for manage in manages:
            print(manage.text)
        print(banking.find_element_by_class_name('transaction').text)
        print(banking.find_element_by_class_name('balance').text)
        print(banking.find_element_by_class_name('note').text)
        print('')

    # ログアウト
    browser.find_element_by_link_text('ログアウト').click()
    time.sleep(3)
finally:
    browser.quit()
```

これで入出金明細の一覧がとれるのであとは、メールやslack等のチャットに流すと入出金のレポートが完成

※別途当日分の明細だけを取得する処理が必要

### 実ソース格納予定地

[mursts/scrape-netbanking](https://github.com/mursts/scrape-netbanking)
