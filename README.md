# 🚀 FastAPI API with Automated Testing & GitHub Actions CI/CD  

This project sets up a **FastAPI** backend with **automated testing** and **GitHub Actions integration** for CI/CD. It includes:  
✅ A simple **FastAPI server** for basic math operations  
✅ **Automated API testing** using `pytest`  
✅ **GitHub Actions CI/CD** for continuous testing  

---

## 📌 **Project Structure**  
```
FastAPI-CI-CD/
│── apiserver.py                # FastAPI server
│── testAutomation.py           # Basic test script (requests)
│── testAutomationPytest.py     # Advanced tests (pytest)
│── .github/
│   └── workflows/
│       └── test.yml           # GitHub Actions CI/CD pipeline
│── README.md                   # Project documentation
```

---

## 🔧 **Step 1: Setting Up the FastAPI Server**  

### **1️⃣ Install Dependencies**  
Run the following command in your terminal:  
```bash
pip install fastapi uvicorn
```

### **2️⃣ Create `apiserver.py`**  
Create a Python file named `apiserver.py` and add this code:  
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/add/{num1}/{num2}")
def add(num1: int, num2: int):
    return {"result": num1 + num2}

@app.get("/subtract/{num1}/{num2}")
def subtract(num1: int, num2: int):
    return {"result": num1 - num2}

@app.get("/multiply/{num1}/{num2}")
def multiply(num1: int, num2: int):  # ✅ Fixed syntax error here
    return {"result": num1 * num2}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run("apiserver:app", host="0.0.0.0", port=8000, reload=True)
```

### **3️⃣ Run the Server**  
```bash
python apiserver.py
```
Server will run at **http://localhost:8000**.  

---

## 🛠 **Step 2: Writing API Tests**  

### **1️⃣ Install Requests Library**  
```bash
pip install requests
```

### **2️⃣ Create `testAutomation.py`**  
```python
import requests

testcases = [
    {"url": "http://localhost:8000/add/2/2", "expected": 4, "description": "Test addition"},
    {"url": "http://localhost:8000/subtract/5/3", "expected": 2, "description": "Test subtraction"},
    {"url": "http://localhost:8000/multiply/2/3", "expected": 6, "description": "Test multiplication"}
]

def test():
    for case in testcases:
        response = requests.get(case["url"])
        result = response.json()["result"]
        assert result == case["expected"], f"Test failed: {case['description']}"
        print(f"✅ Test passed: {case['description']}")

test()
```

### **3️⃣ Run the Test Script**  
```bash
python testAutomation.py
```

---

## ⚡ **Step 3: Upgrading to Pytest**  

### **1️⃣ Install Pytest**  
```bash
pip install pytest
```

### **2️⃣ Create `testAutomationPytest.py`**  
```python
import pytest
import requests

testcases = [
    ("http://localhost:8000/add/2/2", 4, "Addition Test"),
    ("http://localhost:8000/subtract/5/3", 2, "Subtraction Test"),
    ("http://localhost:8000/multiply/2/3", 6, "Multiplication Test")
]

@pytest.mark.parametrize("url, expected, description", testcases)
def test_api(url, expected, description):
    response = requests.get(url)
    assert response.json()["result"] == expected, f"❌ {description} failed"
```

### **3️⃣ Run Pytest**  
```bash
pytest testAutomationPytest.py
```

---

## 🚀 **Step 4: Setting Up GitHub Actions CI/CD**  

### **1️⃣ Create `.github/workflows` Directory**  
```bash
mkdir -p .github/workflows
```

### **2️⃣ Create `test.yml` File**  
Inside `.github/workflows/`, create a file named `test.yml`:  
```yaml
name: API Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Set Up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.10"

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install fastapi uvicorn pytest requests

      - name: Start FastAPI Server
        run: |
          nohup python apiserver.py &
        env:
          PYTHONUNBUFFERED: 1

      - name: Wait for Server to Start
        run: sleep 5

      - name: Run Pytest
        run: pytest testAutomationPytest.py
```

### **3️⃣ Push Workflow to GitHub**  
```bash
git add .
git commit -m "Added GitHub Actions for API testing"
git push origin main
```

---

## 🛠 **Step 5: Troubleshooting Errors**  

| ❌ **Error** | ✅ **Solution** |
|-------------|---------------|
| `ModuleNotFoundError: No module named 'uvicorn'` | Run `pip install uvicorn` |
| `SyntaxError: unterminated string literal` | Check for missing quotes (`"`) in your Python code |
| `ConnectionError: [Errno 111] Connection refused` | Ensure the FastAPI server is running before testing |
| GitHub Actions fails with `pytest not found` | Add `pip install pytest` in workflow |
| `EADDRINUSE: address already in use` | Use `nohup` in the workflow to keep the server running |

---

## 🎯 **Step 6: Verify GitHub Actions**
1. Go to **GitHub Repository** → Click on **Actions** tab.  
2. Check if the workflow ran successfully. ✅  
3. If any test fails, check the logs and fix the issue.  

---

## 📢 **Conclusion**  

In this project, we:  
✅ Built a **FastAPI server** with basic endpoints  
✅ Wrote **automated tests** using `pytest`  
✅ Integrated **GitHub Actions** for CI/CD  
✅ Troubleshooted **common errors**  

Now, every time you push code to **GitHub**, the API will be **tested automatically**! 🚀  

---

## 🌟 **Like This Project? Give It a Star!** ⭐  
If this helped you, consider giving it a ⭐ on GitHub! 💙  
