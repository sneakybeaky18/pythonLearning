import vk_api
import requests
from bs4 import BeautifulSoup
import time
import json

class Anecdoti:
    def __init__(self, link):
        self.link = link

    def get_soup(self, link):
        params = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:82.0) Gecko/20100101 Firefox/82.0'
        }
        response = requests.get(link, headers=params)
        response.encoding = 'cp1251'
        soup = BeautifulSoup(response.text, 'lxml')
        return soup

    def get_page(self, url):
        try:
            link = self.get_soup(url).find('table', attrs={'class': 'pagenavibig'})
            get_link = link.find_all('td', attrs={'align': 'center'})
            for el in get_link:
                a_tag = el.find('a')
                href = a_tag.get('href')
                if (href.find('arc') != -1):
                    return href
        except AttributeError:
            link = self.get_soup(url).find_all('div', attrs={'class': 'pagenavi'})
            for el in link:
                a_tag = el.find('a')
                href = a_tag.get('href')
                if (href.find('arc') != -1):
                    print(href)
                    return href

    def get_pages(self, number_of_pages):
        list = []
        def_url = "http://anekdotov.net/"
        pages_counter = 0
        page = def_url + self.get_page('http://anekdotov.net/arc/200101.html')
        list.append(page)
        while pages_counter < number_of_pages:
            time.sleep(0.2)
            pages_counter = pages_counter + 1
            next_page = def_url + self.get_page(list[-1])
            list.append(next_page)
        return list

    def get_jokes(self, number_of_pages):
        dict = {
            'joke_text': ''
        }
        for el in self.get_pages(number_of_pages):
            for el in self.get_soup(el).find_all("div", attrs={'align': 'justify'}):
                dict['joke_text'] = el.get_text()
        return dict

    def save_to_json(self, name, data_to_save):
        with open(name + ".json", 'w', encoding="utf-8") as my_data_file:
             json.dump(data_to_save, my_data_file, ensure_ascii=False)






ac = Anecdoti('http://anekdotov.net/arc/191231.html')
# print(ac.get_soup('http://anekdotov.net/arc/191231.html'))
# print(ac.get_page('http://anekdotov.net/arc/191230.html'))
print(ac.get_jokes(2))
