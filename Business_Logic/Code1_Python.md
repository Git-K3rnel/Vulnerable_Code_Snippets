```python
from flask import Flask, request, render_template

app = Flask(__name__)


class Account:
    def __init__(self, username: str):
        self.User: str = username
        self.Currency: str = '€'
        self.Balance: float = 1000

    def Update_Balance(self, money: float):
        self.Balance = money

    def Withdraw(self, amount: float):
        if amount <= self.Balance:
            money = self.Balance - amount
            self.Update_Balance(money)
            return money
        return 0
```
