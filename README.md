# kbc-quiz-game.py
import random
import time
import mysql.connector
mydb = mysql.connector.connect(
    host="localhost",
    user="root",
    password="sonik@123",
    database="mydatabase",
    auth_plugin="mysql_native_password"
)
cursor =  mydb.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS kbcapp (
    did INT PRIMARY KEY,
    question VARCHAR(100) ,
    a  VARCHAR(45),
    b VARCHAR(45),
    c VARCHAR(45) ,
    d VARCHAR(45),
    ans VARCHAR(45),
    levels INT
)
""")

mydb.commit();

print("\n  WELCOME TO KAUN BANEGA CROPEPATI  \n")
Questions = [
     ["1.Who is the capital of india?",
     "a. Delhi","b. Mumbai","c. Kolkata","d. Chennai",1],
     ["2.Who was our first president?",
     "a. Dr. Rajendra Prasad", "b. Rajeev Gandhi", "c. Indira Gandhi", "d. Mahatma Gandhi",1],
     ["3.Which of these cricketers holds the record for playing the highest number of test matches.",
    "a. Stephen Flaming", "b. Alan Border", "c. Steve Waugh", "d. Sachin Tendulkar",4],
    ["4.Diu is an island of_________",
    "a.Maharashtra","b.Daman","c.Gujarat","d.Goa",3],
    ["5.Who painted the Mona Lisa?",
     "a.Vincent van Gogh", "b.Pablo Picasso", "c.Leonardo da Vinci","d. Claude Monet",3],
    ["6.What is the correct file extension for Python files?",
    "a. .pt","b. .py","c. .pyt","d. .ptn",2],
    ["7.Which keyword is used to create a function in python?",
     "a. func","b. define", "c.def","d. function",3],
    ["8.Which of the following is a valid variable name in Python?",
     "a. 2name","b. my-name","c. my_name","d. my name",3],
    ["9.Which data type is used to store True or False values?",
    "a. string","b. int","c. bool","d. float",3],
    ["10.Which connection type is ordered and changeable in Python?",
     "a. Set","b. Tuple","c. Dictionary","d. List",4],
    ]

Levels = [1000, 2000, 3000, 5000,10000, 20000, 40000, 80000,160000, 320000]
Money = 0
lifelines = {"50-50": True,"Ask the Audience": True,"Skip Question": True}
for i in range(0,len(Questions)):
    question = Questions[i]
    print(f"\nQuestion for Rs. {Levels[i]}:")
    print(question[0])
    print(question[1])
    print(question[2])
    print(question[3])
    print(question[4])
    print("\nAvailable Lifelines:", [key for key, val in lifelines.items() if val])
    use_life = input("\nDo you want to use a lifeline? (yes/no):").lower()
    if use_life == "yes":
        print("\nWhich lifeline do you want to use?")
        choice = input ("1. 50-50\n2. Ask the Audience\n3. Skip Question\nEnter choice: ")
        if choice == "1" and lifelines ["50-50"]:
            lifelines["50-50"] = False
            correct = question[-1]
            options = [1, 2, 3, 4]
            options.remove(correct)
            remove = random.sample(options, 2)
            print("\nRemaining options are:")
            for j in range(1, 5):
                if j not in remove:
                    print(question[j])
        elif choice == "2" and lifelines["Ask the Audience"]:
            lifelines["Ask the Audience"] = False
            print("\nAudience Poll Results:")
            time.sleep(1)
            correct = question[-1]
            for j in range(1, 5):
                if j == correct:
                    print(f"{question[j]}: {random.randint(60, 80)}%")
                else:
                    print(f"{question[j]}:{random.randint(5, 20)}%")
        elif choice == "3" and lifelines["Skip Questions"]:
            lifelines["Skip Question:"] = False
            print("\nYou choice to skip this question! Moving to the next one...")
            continue
        else:
            print("\nLifeline not available or invalid choice!")
            option =(int(input("\nChoose Your option (1-4): ")))
            if(option == question[-1] ):
                print(f"\n Correct Answer! You have  won Rs. {Levels[i]}")
            else:
                print("\n Wrong Answer!")
                break
                print(f"\n Game Over! You take home rs. {Money}")
                print("\n Thank You for playing KAUN BANEGA CROREPATI!!")
