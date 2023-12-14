# find-a-word
Certainly! To find a word in a large paragraph, you can use this method in Python

# find the word from databases in transcript column retrieve data one by one and find the word of each rows using regex method in python and therefore. you can generate the logo csv files 


    import pandas as pd
    import mysql.connector
    import datetime
    import re
    import random  
    
    # define your word into list
    word_array = ["settlement", "settle","वो अब नहीं रहे","उनकी मौत हो गई है", "वो एक्सपायर हो गए हैं"]
    all_data = []
    def connection():
        try:
            conn = mysql.connector.connect(
                host="ip address",
                user="root",
                password="password",
                database="databasename",
                charset='utf8mb4'
            )
            print("Connected :", conn)
            return conn
        except Exception as e:
            print("Error :", e)
    
    def wordFinder(word,script):
        #print(word)
        finder_word = []
        flag = False
        pattern = re.compile(r'\b{}\b'.format(re.escape(word)), re.IGNORECASE)
        matches = re.findall(pattern, script)
        #print(type(matches))
        finder_word.extend(matches)
        if matches:
            flag = True
            setword = word
            print("i got it...")
        else:
            flag = False
            print("not found..")
        return flag, len(matches)
    
    def find_all_words():
        exist_word = []
        conn = connection()
        query = pd.read_sql(f"SELECT opoid, connid, fulltranscript, calldate FROM calltrans WHERE calldate BETWEEN '2023-12-01' AND '2023-12-01'",conn)
        connid_data = query.to_dict('records')
        #alldata = query['fulltranscript'].tolist()
        #print(connid_data[3]['connid'])
        for word in word_array:
            print(word)
            wordcount = 0
            for i in range(len(connid_data)):
                flags,occur_word = wordFinder(word, connid_data[i]['fulltranscript'])
                if flags == True:
                    wordcount = wordcount + 1
                    print(f"{word}---{wordcount} ")
                    print("your this word is found :",word)
                    exist_word.append(word)
                    all_data.append({'word': word, 'word_count': wordcount, 'connid': connid_data[i]['connid'],'opoid':connid_data[i]['opoid'],'transcript': connid_data[i]['fulltranscript'],'word_occure': occur_word,'calldate':connid_data[i]['calldate']})
                else:
                    print(f"{word}---{wordcount} ")
                    print("your this word is not found :", word)
    
    
    find_all_words()
    resultof_csv = pd.DataFrame(all_data)
    filename = random.randint(1000,5000)
    resultof_csv.to_csv(f'word_finder{filename}.csv', index=False, encoding="utf-8")
    print(resultof_csv)
