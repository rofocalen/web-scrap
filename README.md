import requests
from bs4 import BeautifulSoup

KEYWORDS = ['дизайн', 'фото', 'web', 'python']

URL = 'https://habr.com/ru/all/'

def main():
    response = requests.get(URL)
    response.raise_for_status()  
    
    soup = BeautifulSoup(response.text, 'html.parser')
    
    articles = soup.find_all('article')
    
    for article in articles:
        preview_text = article.text.lower()
        if any(keyword.lower() in preview_text for keyword in KEYWORDS):

            date_tag = article.find('time')
            date = date_tag['datetime'] if date_tag else 'Дата не найдена'
            
            title_tag = article.find('h2')
            title = title_tag.text.strip() if title_tag else 'Без заголовка'
            link_tag = title_tag.find('a') if title_tag else None
            link = 'https://habr.com' + link_tag['href'] if link_tag else 'Ссылка не найдена'
            
            print(f"{date} – {title} – {link}")

if __name__ == '__main__':
    main()# web-scrap
