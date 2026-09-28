# Fraction-Class
```python
import math

class Fraction:
    def __init__(self, n, d):
        if d == 0:
            raise ValueError("Знаменатель не может быть равен нулю")
        if d < 0:
            n = -n
            d = -d
        self.n = n
        self.d = d

        self.modd()

    def modd(self):
        c = math.gcd(abs(self.n), self.d)
        self.n = self.n // c
        self.d = self.d // c

    def __str__(self):
        if self.d == 1:
            return str(self.n)

        if abs(self.n) > abs(self.d):
            abs_n = abs(self.n)
            wh_part = abs_n // self.d
            r = abs_n % self.d
            if self.n < 0:
                wh_part = -wh_part
            return f"{wh_part} {r}/{self.d}"

        return f"{self.n}/{self.d}"

    def __mul__(self, other):
        if isinstance(other, int):
            return Fraction(other, 1)
        if isinstance(other, Fraction):
            return Fraction(self.n * other.n, self.d * other.d)
        return NotImplemented

    def __truediv__(self, other):
        if isinstance(other, int):
            return Fraction(other, 1)
        if isinstance(other, Fraction):
            if other.n == 0:
                raise ZeroDivisionError("Деление на ноль невозможно")
            return Fraction(self.n * other.d, self.d * other.n)
        return NotImplemented

    def __add__(self, other):
        if isinstance(other, int):
            return Fraction(other, 1)
        if isinstance(other, Fraction):
            res_n = self.n * other.d + other.n * self.d
            res_d = self.d * other.d
            return Fraction(res_n, res_d)
        return NotImplemented

    def __sub__(self, other):
        if isinstance(other, int):
            return Fraction(other, 1)
        if isinstance(other, Fraction):
            res_n = self.n * other.d - other.n * self.d
            res_d = self.d * other.d
            return Fraction(res_n, res_d)
        return NotImplemented

    def __pow__(self, power):
        if isinstance(power, int):
            if power == 0:
                return Fraction(1, 1)
            elif power < 0:
                a = self.n
                self.n = self.d
                self.d = a
                return Fraction(self.n ** abs(power), self.d ** abs(power))
            else:
                return Fraction(self.n ** power, self.d ** power)

def inp(text):
    text = text.strip()
    if "/" not in text and isinstance(int(text), int):
        return Fraction(int(text), 1)
    n_and_d = text.split("/")
    if len(n_and_d) > 2:
        raise ValueError("Введите дробь вида a/b или целое число, пожалуйста")
    else:
        return Fraction(int(n_and_d[0]), int(n_and_d[1]))

def cons():
    while True:
        try:
            t1 = input("Введите первую дробь вида a/b или целое число: ")
            fr1 = inp(t1)

            op = input("Введите знак из набора +, -, /, *, **: ").strip()
            if op == "**":
                p = int(input("Для возведения в степень введите целое число: "))
                res = fr1 ** p
            else:
                t2 = input("Введите вторую дробь вида a/b или целое число: ")
                fr2 = inp(t2)

            if op == "+":
                res = fr1 + fr2
            elif op == "-":
                res = fr1 - fr2
            elif op == "/":
                res = fr1 / fr2
            elif op == "*":
                res = fr1 * fr2

            if op not in ["**", "+", "-", "/", "*"]:
                print("Неизвестный оператор. Пожалуйста используйте +, -, /, *, **")
                continue

            print("Ответ: ", res)

        except ValueError as e:
            print(f"Ошибка ввода: {e}")
        except ZeroDivisionError:
            print("Деление не ноль невозможно")

if __name__ == "__main__":
    cons()










