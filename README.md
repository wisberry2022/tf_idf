# TF_IDF Readme(2021_09_13.Ver)

문서 행렬(DTM)을 활용하여 문서 내 등장하는 어휘들마다 가중치를 부여하는 기술인 TF_IDF를 Python으로 직접 구현해보았습니다. TF_IDF 계산과 계산 결과로 나온 벡터를 활용하여 코사인 유사도를 통해 입력으로 받은 문서의 유사도를 측정할 수 있게 했습니다.

## 파일 소개
     0. all_0831_1546.db  -> 네이버 기사 본문 샘플 DB
     1. db_controller.py    -> DB 제어 위한 모듈
     2. new_DTM.py          -> DTM부터 TF_IDF 계산, 문서 유사도 계산해주는 모듈
     3. preprocess.py         -> 텍스트 데이터의 전처리를 위한 모듈

## 파일 소개2
     0. all_0831_1546.db  
          * 8월 31일자 정치, 경제, 사회, 생활, 세계, IT/과학기술 섹션의 기사 1000개를 크롤링한 데이터 베이스. 실제로는 일주일 정도 데이터를 수집하였습니다. 
            DB 생성을 위한 코드와 생성된 DB는 향후 다른 저장소에 PUSH할 예정입니다.
          * DB는 id, section, title, content의 칼럼으로 구성되어있습니다. section에는 정치, 경제, 사회와 같은 기사의 분야, title은 기사의 제목, content는 기사 본문입니다.

     1. db_controller.py    
           * DB에 저장된 데이터를 불러오는 모듈입니다. 사용할 수 있는 함수는 아래와 같습니다.
                    A. select_text
                        -> DB 이름과 섹션을 입력하면 해당 섹션의 기사 본문들을 모두 가지고 옵니다. 추가로 id와 title 옵션을 주어 원하는 id와 title의 기사를 가지고 올 수 있습니다.
                    B. select_article_by_keyword
                        -> DB 이름과 섹션, 키워드를 입력하면 사용자가 원하는 키워드가 포함된 기사 본문을 가지고 옵니다. 키워드를 입력받은 뒤 Konlpy의 Okt 형태소 분석기를 활용 
                             하여 DB에 저장된 기사 제목들을 모두 분석하여 사용자가 입력한 키워드가 포함된 기사의 본문들을 가지고 옵니다. 
                              * 올바른 값을 반환하긴 하나 향후 데이터 전처리 시 문제가 있어 해당 함수, 해당 기능과 관련한 부분은 재수정할 계획입니다.
 
     2. new_DTM.py         
         * dtm과 tf_idf 계산, 그리고 입력으로 받은 문서들의 유사도를 계산해주는 TF_IDF_FUNCTION 클래스가 있습니다. 주요 메소드는 아래와 같습니다.
                A. dtm
                    -> 입력받은 두 문서들을 대상으로 DTM을 생성합니다. 문서 개수만큼의 DTM 배열을 반환합니다.
                B. tf_idf
                    -> dtm 메소드를 통해 반환받았던 DTM 배열을 입력으로 받습니다. DTM의 각 원소들을 방문하면서 tf_idf 공식을 활용하여 각 원소(어휘)들의 가중치를 계산합니다. 
                         tf_idf 가중치로 이루어진 벡터들을 반환합니다.
                C. docu_sim
                    -> tf_idf 메소드를 활용하여 반환받았던 tf_idf 벡터를 입력으로 받아 문서 간 유사도를 측정합니다. 이 때 코사인 유사도를 활용하였습니다.

     3. preprocess.py        
         * DB에서 불러온 기사 본문 데이터들의 노이즈 데이터를 제거합니다. Cleaning_Noise 클래스에는 한글 이외의 문자들을 제거한 cleaning 메소드가 있습니다. 

## 실험 진행
   -> 현재 수집한 데이터를 기준으로 동일 섹션 내 기사들의 유사도와  서로 다른 섹션들의 기사의 유사도를 비교해볼 예정입니다. 실험 결과가 나오면 이후에 readme를 업데이 
        트 하도록 하겠습니다.
