# Prudent-AI
## Santhosh K

### Task 1

```

def find_starting_index(food, people):
    total_food = sum(food)
    total_people = sum(people)
    
    if total_food < total_people:
        return -1  
    
    n = len(food)
    
    for i in range(n):
        remaining_food = 0
        for j in range(n):
            idx = (i + j) % n
            remaining_food += food[idx] - people[idx]
            
            if remaining_food < 0:
                break 
        
        if remaining_food >= 0:
            return i 
    
    return -1  

food1 = [1, 2, 3, 4, 5]
people1 = [3, 4, 5, 1, 2]
result1 = find_starting_index(food1, people1)
print(result1) 

food2 = [1, 2]
people2 = [5, 6]
result2 = find_starting_index(food2, people2)
print(result2)

```


<img width="960" alt="image" src="https://github.com/santhoshhk/Prudent-AI/assets/114757338/ac670dae-4d6e-4755-b08b-1170143240c0">


### Task 2

```
import json

def extract_info_from_json(json_data):
    # Initialize variables to store extracted information
    invoice_date = ""
    invoice_number = ""
    billing_info = ""
    shipping_info = ""
    email = ""
    total_invoice_amount = ""
    num_items = ""

    # Loop through the JSON data
    for entry in json_data:
        key = list(entry.keys())[0]
        values = entry[key]

        # Extract text based on the key
        if key == "06/10/2021":
            invoice_date = " ".join(map(str, values))
        elif key == "INVO-005":
            invoice_number = " ".join(map(str, values))
        elif key == "Billing Information":
            billing_info = " ".join(map(str, values))
        elif key == "Shipping Information":
            shipping_info = " ".join(map(str, values))
        elif key == "Email":
            email = " ".join(map(str, values))
        elif key == "Total Invoice Amount":
            total_invoice_amount = " ".join(map(str, values))
        elif key == "Number of Items in the Invoice":
            num_items = " ".join(map(str, values))

    # Return the extracted information
    return {
        "Invoice Date": invoice_date,
        "Invoice Number": invoice_number,
        "Billing Information": billing_info,
        "Shipping Information": shipping_info,
        "Email": email,
        "Total Invoice Amount": total_invoice_amount,
        "Number of Items in the Invoice": num_items
    }


json_file_path = "sample_invoice.json"

def read_json_file(file_path):
    try:
        with open(file_path, "r") as json_file:
            json_data = json.load(json_file)
            return json_data
    except FileNotFoundError:
        print(f"File not found: {file_path}")
        return None
    except json.JSONDecodeError as e:
        print(f"Error decoding JSON: {e}")
        return None

json_data = read_json_file(json_file_path)

result = extract_info_from_json(json_data)

print(result)


```

![task 2 op](https://github.com/santhoshhk/Prudent-AI/assets/114757338/dc6bd5c2-0bd7-4a22-8507-ba958203e45a)

### Task 3

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

!pip install openpyxl

book = pd.read_excel('books.xlsx')

books=book

books.head()

books.columns

books.drop([ 'isbn13','Unnamed: 12','language_code','num_pages','ratings_count','text_reviews_count','publication_date'],axis=1,inplace=True)

books.rename(columns={'authors':'author','Year-Of-Publication':'year','Publisher':'publisher','publication_date':'year','isbn':'ISBN'},inplace=True)

books.head()

books.rename(columns={'Book-Title':'title','Book-Author':'author','Year-Of-Publication':'year','Publisher':'publisher'},inplace=True)

books.head()

books.isnull().sum()

books.dropna(inplace=True)

books['average_rating'] = pd.to_numeric(books['average_rating'], errors='coerce')

book_pivot = books.pivot_table(columns='bookID', index='title', values='average_rating')

book_pivot.fillna(0,inplace=True)
book_pivot.head()

from sklearn.neighbors import NearestNeighbors
model=NearestNeighbors(algorithm='brute')

from scipy.sparse import csr_matrix
book_sparse=csr_matrix(book_pivot)

model.fit(book_sparse)

book_pivot.iloc[240].shape

distances,suggestions=model.kneighbors(book_pivot.iloc[240].values.reshape(1,-1),n_neighbors=6)

suggestions

distances

book_pivot.iloc[240].values

for i in range(len(suggestions)):
    print(book_pivot.index[suggestions[i]])

def recommend_book(book_name):
    book_id=np.where(book_pivot.index==book_name)[0][0]
    distances,suggestions=model.kneighbors(book_pivot.iloc[book_id].values.reshape(1,-1),n_neighbors=6)
    
    for i in range(len(suggestions)):
            print(book_pivot.index[suggestions[i]])

recommend_book('Exclusive')

```

![task3](https://github.com/santhoshhk/Prudent-AI/assets/114757338/766e566a-4733-4303-8208-30041afedaf1)
