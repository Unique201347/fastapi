![FastAPI](https://images.datacamp.com/image/upload/v1664210695/A_simple_API_architecture_design_f98bfad9ce.png)

```sh
python -m venv venv
source venv/bin/activate
```

```sh
pip install fastapi uvicorn

pip freeze > requirements.txt
pip install -r requirements.txt
```

```py
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

```sh
uvicorn main:app --reload
```

---

```py
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```

```py
from typing import Optional
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

app = FastAPI()

@app.put("/items/{item_id}")
async def create_item(item_id: int, item: Item):
    return {"item_id": item_id, **item.dict()}

```

```py
import pymongo
import json

# Connect to MongoDB
client = pymongo.MongoClient("mongodb://localhost:27017/")
db = client["courses"]
collection = db["courses"]

# Read courses from courses.json
with open("courses.json", "r") as f:
    courses = json.load(f)

# Create index for efficient retrieval
collection.create_index("name")

# add rating field to each course
for course in courses:
    course['rating'] = {'total': 0, 'count': 0}

# add rating field to each chapter
for course in courses:
    for chapter in course['chapters']:
        chapter['rating'] = {'total': 0, 'count': 0}

# Add courses to collection
for course in courses:
    collection.insert_one(course)

# Close MongoDB connection
client.close()
```

> [**Documentation**](https://fastapi.tiangolo.com/tutorial/first-steps/)
