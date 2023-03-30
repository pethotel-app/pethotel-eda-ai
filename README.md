<img width="100%" height="45%" src="https://user-images.githubusercontent.com/120348555/228705384-594d0b67-9a3e-44d9-b501-6a2466fcb192.jpg">


## 당신이 쉬고 있는 동안 반려동물도 편안하게 쉬어가는 곳, 쉬다가개:heart_eyes_cat:

<div align="center">
  <h1>📌</h1>
</div>
<div align="center"> 
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/>
  <img src="https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white"/>
  <br>
  <img src="https://img.shields.io/badge/JSON-000000?style=flat-square&logo=JSON&logoColor=white"/>
  <img src="https://img.shields.io/badge/Selenium-43B02A?style=flat-square&logo=Selenium&logoColor=white"/>
  <img src="https://img.shields.io/badge/matplotlib.pyplot-40AEF0?style=flat-square&logo=&logoColor=white"/>
  <br>
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=Jupyter&logoColor=white"/>
  <img src="https://img.shields.io/badge/Anaconda-44A833?style=flat-square&logo=Anaconda&logoColor=white"/>
</div>

## 📌 Project Explanation 
* 산업 매출액 기준 2027년에는 6조를 예상할만큼 반려동물 관련 시장 규모 전망이 매우 좋고 반려동물 호텔 업체 수가 2023년 기준 5년전과 비교하면 약 3배 정도로 빠른속도로 늘어나고 있는데 반려동물 전용 숙박업소 예약 플랫폼 앱은 존재하지않기 때문에 기획하게 되었습니다.

## 📌Code block
```python
def infinite_loop(driver):
    # 최초 페이지 스크롤 설정
    # 스크롤 시키지 않았을 때의 전체 높이
    last_page_height = driver.execute_script("return document.documentElement.scrollHeight")

    while True:
        # 윈도우 창을 0에서 위에서 설정한 전체 높이로 이동
        driver.execute_script("window.scrollTo(0, document.documentElement.scrollHeight);")
        time.sleep(1.0)
        # 스크롤 다운한 만큼의 높이를 신규 높이로 설정 
        new_page_height = driver.execute_script("return document.documentElement.scrollHeight")
        # 직전 페이지 높이와 신규 페이지 높이 비교
        if new_page_height == last_page_height:
            time.sleep(1.0)
            # 신규 페이지 높이가 이전과 동일하면, while문 break
            if new_page_height == driver.execute_script("return document.documentElement.scrollHeight"):
                break
        else:
            last_page_height = new_page_height
```
```python
def naverMapCrawling(search):
    driver = webdriver.Chrome("./chromedriver.exe") #selenium 사용에 필요한 chromedriver.exe 파일 경로 지정

    driver.get(f"https://m.map.naver.com/search2/search.naver?query={search}") 
    driver.implicitly_wait(3) # 로딩이 끝날 때 까지 10초까지는 기다림

    infinite_loop(driver)

    items = driver.find_elements(By.CSS_SELECTOR, '._item ')

    for item in items:
        title = item.get_attribute('data-title')
        address = item.find_element(By.CSS_SELECTOR,'.item_address ').text.replace('주소보기\n', '')
        longtitude = float(item.get_attribute('data-longitude'))
        latitude = float(item.get_attribute('data-latitude'))
        tel = item.get_attribute('data-tel')
        id = int(item.get_attribute('data-id'))
        
        for character in string.punctuation:
            title = title.replace(character, '') # 특수기호 제거
            
        datas.append({
            "id" : id,
            "title" : title,
            "addr" : address,
            "longtitude" : longtitude,
            "latitude" : latitude,
            "tel" : tel,
            "naverUrl" : f"https://m.place.naver.com/place/{id}/home"
            })

        # url_list.append(item.find_element(By.CSS_SELECTOR, '.item_common > a').get_attribute('data-url'))
        data_id.append(id)

    print(datas)
    
    for id in data_id:
        driver.get(f"https://m.place.naver.com/place/{id}/home")
        time.sleep(1)
        try:
            description = driver.find_element(By.CSS_SELECTOR, 'span.zPfVt').text
            print(description)
        except:
            description =""
            print("내용 없음")
        
        for data in datas:
            if data["id"] ==id:
                data["description"] = description
                break
            
    
   
        driver.get(f'https://m.place.naver.com/place/{id}/photo')
        time.sleep(1)

        try:
            image = driver.find_element(By.CSS_SELECTOR, 'div.wzrbN > a > img').get_attribute('src')
            print(image)
        except:
            image = ""
            print("사진 없음")

        for data in datas:
            if data["id"] == id:
                data["description"] = description
                data["img_url"] = image
                data["menu"] = menu_list
                
                break
```
```python
# 지역별로 크롤링한 DataFrame 가져오기
df1 = pd.read_csv('강원도.csv',',')
df2 = pd.read_csv('경기도.csv',',')
df3 = pd.read_csv('경상도.csv',',')
df4 = pd.read_csv('서울시.csv',',')
df5 = pd.read_csv('세종시.csv',',')
df6 = pd.read_csv('인천시.csv',',')
df7 = pd.read_csv('전라도.csv',',')
df8 = pd.read_csv('충청도.csv',',')

# 데이터프레임 병합하기
df= pd.concat([df1, df2, df3,df4,df5,df6,df7,df8], axis=0, ignore_index=True)


# 필요없는 컬럼 삭제
df.drop(['Unnamed: 0','Unnamed: 0.1'] axis =1 , inplace=True)

# 인덱스 재설정
df.reset_index(drop=True, inplace=True)

# 호텔이랑은 관련 없는 문자열을 포함한 행 삭제

df = df[~df['col1'].str.contains('병원')
df = df[~df['col1'].str.contains('살롱')    
```


## <p align="center"> 🌈 Member</p>

### 
|왕현성|윤지수|백민우|
|:-:|:--:|:-:|
|<img src="https://user-images.githubusercontent.com/120348500/227099410-49f69b01-7b82-45a3-ab85-1d477c7ae6d1.jpg" width="100" height="100">|<img src="https://user-images.githubusercontent.com/120348555/227101223-bbfa4b86-906f-4a33-9399-da2ed5f13fbb.jpg" alt="d00hye" width="100" height="100">|<img src="https://user-images.githubusercontent.com/120348555/228713684-a3d415b9-1a34-481b-866c-7b034a2c061a.jpg" alt="DoyKim-20" width="100" height="100">|
|[hyunsungKR](https://github.com/hyunsungKR)|[Yunwltn](https://github.com/Yunwltn)|[leobaek](https://github.com/leobaek)|
