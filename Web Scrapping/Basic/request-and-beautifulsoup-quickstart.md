# Why Web Scrapping?
리뷰를 비교하거나 제품을 인덱싱하거나 매일 웹사이트의 정보를 손쉽게 추출하는 방법. 
검색해서 원하는 사이트 스크래핑 기능도 가능!

indeed, stackoverflow 라는 구직사이트가 있음. 
직무랑 회사, 위치정보, 지원하기 등의 링크 정보가 있고 스크레퍼를 만들어서 정보를 모두 엑셀 시트에 옮겨보겠음. 

# 또 뭘 만들 수 있을까?
뉴스레터? 키워드 검색?(학교 게시판 키워드 알림 서비스?), 검색 서비스 등등..을 만들 수도 있겠다.
캐글에서 원하는 대회 주제 노트북을 찾아줄 수도 있겠고..
ref 자료들 찾아주기, 대회 정보 긁어오기, papers with code처럼..

# Requests 라는 패키지를 이용할 것임. 
- 깃헙 주소에 자세한 [튜토리얼](https://github.com/psf/requests)

```python
>>> import requests
>>> r = requests.get('https://api.github.com/user', auth=('user', 'pass'))
>>> r.status_code
200
>>> r.headers['content-type']
'application/json; charset=utf8'
>>> r.encoding
'utf-8'
>>> r.text
'{"type":"User"...'
>>> r.json()
{'disk_usage': 368627, 'private_gists': 484, ...}
```
- requests.get은 뭘까? method임. object 안에 있는 일종의 함수역할
- html 가져오려면 `r.text` 이용
- html에서 원하는 정보 추출하고 싶다면.. BeautifulSoup

# BeautifulSoup
- 자세한 [튜토리얼](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

```python
import requests
from bs4 import BeautifulSoup

indeed_result = requests.get('https://kr.indeed.com/jobs?q=%EB%8D%B0%EC%9D%B4%ED%84%B0+%EB%B6%84%EC%84%9D&l=%EA%B2%BD%EA%B8%B0%EB%8F%84+%EA%B3%A0%EC%96%91&limit=50&radius=25&start=50')

indeed_soup = BeautifulSoup(indeed_result.text, 'html.parser')
```
- html doc를 넣어주고, 'html.parser'을 이용한다는 표시
- data structure navigate 하는 방법 doc에 안내되어있음
- 그 중 `find_all` 이용해보자.
![image](https://user-images.githubusercontent.com/40880346/138112992-12edb837-9fe3-4851-a0cf-545c65216795.png)


```python
import requests
from bs4 import BeautifulSoup

indeed_result = requests.get('https://kr.indeed.com/%EC%B7%A8%EC%97%85?as_and=data+engineer&as_phr=&as_any=&as_not=&as_ttl=&as_cmp=&jt=all&st=&salary=&radius=25&l=&fromage=any&limit=50&sort=&psf=advsrch&from=advancedsearch')

indeed_soup = BeautifulSoup(indeed_result.text, 'html.parser')

# indeed 웹사이트에서 페이지 숫자를 가져오자
# 페이지 목록이 pagination-list란 ul 태그로 되어있음
pagination = indeed_soup.find('ul', {'class':'pagination-list'}) 

pages = pagination.find_all('a') # anchor

spans = []
for page in pages:
  spans.append(page.find('span'))
print(spans[:-1])
```

## 결과
![image](https://user-images.githubusercontent.com/40880346/138113115-92f4ed26-a619-46dc-adb2-4bbba999a9ba.png)
